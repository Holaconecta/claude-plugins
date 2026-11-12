# Hola Conecta customer plugin

Connects Claude to your Hola Conecta account through the server at `https://proxy.holaconecta.com/mcp/conecta`. You sign in with your existing Hola Conecta account, and Claude gets the access that account already has.

Install and setup steps are in the [repository README](../README.md) and at https://holaconecta.com/customer-claude-connection.

The skill in `skills/conecta-customer/` tells Claude how to use the tools safely: confirm before sending or publishing, respect your permissions, and never invent results.

MIT licensed.
