# Role Extensibility in openedx-authz — slides

Slidev deck covering the two in-flight role-extensibility projects in
`openedx-authz` and a proposal for **Custom Roles** (issue
[#465](https://github.com/openedx/openedx-authz/issues/465)).

Format follows [rodmgwgu/rbac2026](https://github.com/rodmgwgu/rbac2026):
click-to-reveal icon rows, `two-cols` / `two-cols-header` layouts, accent
colors `#00BBF9` (positive) and `#9D0054` (problem/decision).

## Run

```bash
npm install
npm run dev      # opens the deck at http://localhost:3030
```

## Export

```bash
npm run export   # PDF
npm run build    # static SPA in dist/
```

## Slides

1. Two in-flight projects side by side (#408 vs #465)
2. Custom Roles — product requirements
3. Development plan — main epics
4. Support more than one scope type per role — tasks
5. Permission dependency mapping — tasks
6. Custom roles implementation — tasks
7. Decisions that may change the estimate
