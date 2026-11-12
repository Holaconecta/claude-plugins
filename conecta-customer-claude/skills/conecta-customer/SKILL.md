---
name: conecta-customer
description: Use the customer's authorized Conecta account for contacts, tasks, appointments, messages, document-request status, billing records, supported workflows, and organization AWS websites.
---

# Work in Conecta

Use only the connected customer MCP tools. The human user connects through supported OAuth/login using an existing Conecta account; the plugin inherits that account’s active organization and permissions. Do not create or use a separate AI-agent account. Resolve the authenticated context when needed; never infer or override the organization. Respect the tool's scopes, page permissions, schema, pagination, and returned errors.

Resolve contacts by exact email or phone when supplied. Ask the user to choose between ambiguous matches before a write. Use returned IDs and explicitly requested values. Verify successful changes with the relevant read tool. Do not fabricate success, silently substitute recipients/channels, or retry uncertain outbound actions.

Private mailbox access requires ownership or explicit conversation sharing, including for owners and admins. Treat private and quarantined conversations and message detail as restricted; do not use another tool to bypass that access check. A contact search result does not grant permission to open a private conversation. Document tools provide operational status; do not request or disclose government identifiers, payment-card details, document contents, storage URLs, or secrets.

Before sending or scheduling a communication, show the exact final content, recipients, channel, subject, and links or attachments, then obtain explicit approval. For messages, use prepare/get/approve/send with the persisted action; never change the approved recipient or body at send time. Confirm destructive and financial actions only after the user reviews their exact effect.

Blast tools can list blast records and retrieve a selected blast’s status and delivery counts in the authenticated organization. They do not expose recipient-level snapshots. Create a blast only as an unscheduled email or SMS draft with confirm=true, the required message content (and email subject when applicable), and an audience the user requested: filter_type=everyone or filter_type=tags with the corresponding tag_id. Set the blast name and message content from the user’s request. For blast detail reads, pass the channel returned with the selected blast along with its ID. Verify the saved draft with its read tool; never claim that a blast was sent, queued, or scheduled.

For automation questions, use bounded list and detail reads to explain the current definition. In this submission scope, automation creation, definition updates, activation, resumption, and activation/resumption review tools are not available. Existing pause and delete operations remain subject to their own safeguards; never imply a read or review changed lifecycle state.

## Help Desk text workflow (Steps 0–2)

Use only the supported customer MCP tools for this workflow:

1. **Find the relevant guide.** Call `conecta_search_help` with the user's question. Include `locale` only when known and `currentPageKey` only when the user is working from a known Conecta page. Treat returned Guide content as the source for product instructions; do not invent steps when the search returns no useful match.
2. **Check organization setup.** Call `conecta_get_organization_setup` with no arguments. Use only the safe organization profile and onboarding fields returned, including `settingsAvailable`, to explain what is configured or still needed. Do not expose organization secrets or infer missing settings.
3. **Rename the organization only when requested.** First read the current name from `conecta_get_organization_setup`. Ask for explicit approval of the exact new name before writing. Call `conecta_update_organization_settings` with `orgName` set to the requested name, `expectedOrgName` set to the exact name just read, and `confirm=true`. This is a compare-and-set for the organization name only. Report success only from the tool's independent fresh readback; if the current name changed, the write fails, or the readback is unavailable, report that outcome without claiming the rename succeeded.

Never use raw SQL or another organization's identifiers to perform these steps. Help search and setup reads require `conecta:helpdesk:read`; the rename requires `conecta:settings:write`. A confirmation flag does not replace asking the user to approve the exact name change.

Explain raw SQL, platform administration, cross-organization access, and restricted identity/document-content requests as outside this plugin. Do not use internal/admin tools to fulfil them.

## AWS website management

Use the existing Conecta connection. Call `conecta_list_websites` and select the correct returned `siteId`; organizations can have multiple websites. Never supply or request AWS credentials, bucket names, distribution IDs, MCP tokens, or another organization's identifiers.

Read the relevant live files with `conecta_read_website_files`. Keep each file's `etag` and `versionId`. For a missing path, use the returned null versions. Pass these as `expectedEtag` and `expectedVersionId` when preparing edits with `conecta_prepare_website_changes`. Preparation validates and returns proposed files; it does not save a draft, create a hosted preview, or publish.

Show the customer the exact proposed edits, selected website, and change summary. Only after explicit approval, call `conecta_publish_website_changes` with those files and `confirm=true`. If files changed since the read, read them again and obtain approval for the revised changes. Never replace the original expected versions just to force a publication through.

Use `conecta_get_website_deployment_state` to review a release before calling `conecta_rollback_website_release` with the exact `releaseId` and `confirm=true` after customer approval. Rollback undoes that release and rejects files changed by later edits. An invalidation marked InProgress is still propagating; do not call the change publicly verified yet. If a write times out or its response is unclear, inspect history rather than repeating it. If `sourceSyncStatus` is `needs_reconciliation` or `not_linked`, explain that source history needs support attention before further blog/source publication. Never claim a failed or uncertain operation succeeded.
