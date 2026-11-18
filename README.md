# RUMAI plugin

The RUMAI plugin for ChatGPT and Claude. RUMAI is reproductive intelligence for livestock: it predicts estrus, alerts when a cycle deviates and recommends when to act. With this plugin, an assistant reads the farms a person belongs to in RUMAI, read-only.

This repository holds no server code and no farm data. It holds the listing, the connection to RUMAI's remote MCP server (`https://api.rumai.farm/mcp`, OAuth), one skill and the icons.

## Layout

```
rumai/
  plugin.json                       listing, review test cases, publication
  mcp.json                          remote server, OAuth (dynamic client registration)
  skills/rumai-farm-read/SKILL.md   how to read a farm with RUMAI's tools
  assets/logo.png, assets/icon.png  icons
REVIEW.md                           why each tool is read-only, closed-world and non-destructive
```

## Connect without the plugin

Any MCP client can add `https://api.rumai.farm/mcp` as a custom connector. Guide: https://rumai.farm/connect

## Links

- Website: https://rumai.farm
- Privacy: https://rumai.farm/privacy
- Terms: https://rumai.farm/terms
- Support: https://rumai.farm/support
