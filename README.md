# Food Recall Check

Look beyond a brand name. Food Recall Check opens the FDA enforcement record so you can compare the package size, lot code, reason, and distribution with the item in front of you.

[Connect with Claude, ChatGPT, Muse Code, or Grok](PROVIDERS.md)

## Try it

- Show recent Class I FDA food recalls, then open one for its lot codes and distribution.
- I have a packaged cheese product. Help me check its label against FDA recall records.

## Install in Claude

In Claude Code, run the following commands.

```text
/plugin marketplace add OpZero-sh/plugins
/plugin install food-recall-check@opzero
```

In Claude chat or Cowork, open the installed plugin’s **Connectors** tab and connect the listed service. The hosted services use public data and require no sign-in. Then ask a question or use the `check-a-product` skill. In Cowork or Claude Code, the `lot-match` agent can handle a deeper comparison.

## Sources and coverage

The source covers FDA-regulated foods. Federal enforcement reports are posted weekly and can lag a public notice. USDA-regulated meat, poultry, and egg products have a separate recall source.

[FDA public recall notices](https://www.fda.gov/safety/recalls-market-withdrawals-safety-alerts) · [USDA FSIS recalls](https://www.fsis.usda.gov/recalls)

- [food-recall-check](https://food-recall-check.mcp.opzero.sh/) · [Privacy](https://food-recall-check.mcp.opzero.sh/?page=privacy) · [Support](https://food-recall-check.mcp.opzero.sh/?page=support)

## In use

Examples from the connected services in Claude.

![The FDA enforcement record for a food recall](media/fda-recall.webp)
