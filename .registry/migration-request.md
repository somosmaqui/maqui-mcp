# Draft: migrate Maqui Analytics to its verified domain namespace

Not submitted. Requires authorization before sending to Registry maintainers.

Current name: `io.github.luis-fuentes/maqui`
Requested name: `com.somosmaqui/maqui`
Remote URL: `https://app.somosmaqui.com/api/mcp`
Repository: https://github.com/somosmaqui/maqui-mcp
Website: https://somosmaqui.com/asistentes-ia

We control the current GitHub publisher account and the somosmaqui.com domain. DNS authentication for somosmaqui.com succeeds using the public MCPv1 TXT proof at the domain apex. No private keys or tokens are included here.

Publishing the proposed domain manifest returns HTTP 400:

> remote URL https://app.somosmaqui.com/api/mcp is already used by server io.github.luis-fuentes/maqui

Could you advise or perform a migration that preserves the endpoint and existing downstream directory references? Please do not remove the current record before confirming a safe transition. We are not requesting a second duplicate listing or a new endpoint.
