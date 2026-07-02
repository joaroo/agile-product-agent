# Example: Guest Checkout — offline lifecycle fixture

A worked end-to-end run of `/lifecycle` in local fallback mode, from a one-page BRD to dev-ready stories. Use it two ways:

1. **See it work** — read `brd.md` (the input) and `expected-workspace/` (what a completed run produces).
2. **Regression-check the plugin** — after editing any skill or standard, re-run the flow offline and compare the result against `expected-workspace/`.

## Run it

```bash
mkdir /tmp/lifecycle-check && cd /tmp/lifecycle-check   # fresh dir, no .mcp.json → local fallback mode
claude
```

Then in the session:

```
/lifecycle <path-to-plugin>/examples/guest-checkout/brd.md
```

Accept the defaults when prompted (project key `PROD`, space `TEAM`), review each stage gate, and answer `proceed` at all five gates.

## Check the result

Model output varies run to run, so don't expect a byte-identical diff. Compare **structure**, not wording:

| Check | Against |
|-------|---------|
| Same file set exists under `workspace/` | `expected-workspace/` tree |
| Issue frontmatter has all fields (`key`, `type`, `summary`, `status`, `priority`, `epic`, `story_points`, `labels`, `created`, `updated`) | `expected-workspace/jira/PROD/issues/*.md` |
| Page frontmatter has `title`, `space`, `parent`, `status` | `expected-workspace/confluence/TEAM/*.md` |
| Every stage artifact opens with a populated **Carried Context** block | `standards/lifecycle.md` |
| Stories carry BDD ACs (happy + sad + edge) and a DoR verdict | `expected-workspace/jira/PROD/issues/PROD-2.md` |
| Traceability spine is consistent: `REQ-1`/`REQ-2` appear in the Input Brief, the Lifecycle Index table, and the stories' Context sections | all files |
| `.meta/counters.json` matches the number of issues created | `expected-workspace/.meta/counters.json` |

```bash
diff -rq workspace/ <path-to-plugin>/examples/guest-checkout/expected-workspace/   # file-set check
```

If a structural check fails, the skill or standard you just edited probably broke that contract.
