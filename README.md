# Hola Conecta plugins for Claude

This is the Claude plugin for Hola Conecta. It lets Claude work with your Hola Conecta account: contacts, tasks, appointments, messages, billing records, workflows, blast drafts, and your AWS websites.

You need an existing Hola Conecta account. You sign in with it, and Claude gets the same access your account already has. It never gets a separate account.

## Install in Claude Code

```
/plugin marketplace add Holaconecta/claude-plugins
/plugin install conecta-customer@conecta
```

Then run `/mcp`, pick `conecta-customer`, and sign in with your Hola Conecta account.

## Use it in claude.ai or Claude Desktop

You don't need the plugin for this. Add Hola Conecta as a custom connector:

1. Go to Customize > Connectors.
2. Click + Add, then Add custom connector.
3. Name it Hola Conecta and paste this URL: `https://proxy.holaconecta.com/mcp/conecta`
4. Click Continue, then sign in with your Hola Conecta account.

Free Claude accounts can add one custom connector. On Claude Team and Enterprise plans, an owner adds it first, then each person connects with their own Hola Conecta login.

## What it can do

- Find contacts, tasks, appointments, workflows, and the conversations you have permission to view.
- Check document-request, invoice, and payment status.
- Create or update supported records when your account permissions allow.
- Prepare email and SMS blast drafts. It cannot send, queue, or schedule blasts.
- Read your website files, prepare edits, publish changes you confirm, and roll back a release you confirm.

Messages and other actions that need approval still ask you first.

## More information

- Setup and data details: https://holaconecta.com/customer-claude-connection
- Spanish: https://holaconecta.com/es/conecta-en-claude
- Privacy policy: https://holaconecta.com/privacy-policy
- Support: https://holaconecta.com/contact_us

## License

MIT. See [LICENSE](LICENSE).
