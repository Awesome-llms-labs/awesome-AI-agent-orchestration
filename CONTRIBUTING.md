# Contributing

Thanks for helping keep this list honest. Entries and corrections are welcome via pull request.

## What belongs here

Agent **orchestration** — the coordination layer between agents, tools, humans, and infrastructure:

- Multi-agent frameworks and team-organization primitives
- Agent communication protocols and payment/trust standards
- Agent memory and durable-state systems
- Managed orchestration platforms, durable execution, and deployment backends
- Observability and evaluation tooling for multi-agent systems
- Benchmarks and research papers on multi-agent systems

Things that belong in sibling lists instead: single-model capabilities → [awesome-flagship-llms](https://github.com/awesome-llms-labs/awesome-flagship-llms); cost/speed/free tiers → [awesome-flash-llms](https://github.com/awesome-llms-labs/awesome-flash-llms) / [awesome-fast-llms](https://github.com/awesome-llms-labs/awesome-fast-llms) / [awesome-free-llms](https://github.com/awesome-llms-labs/awesome-free-llms); sandboxes → [awesome-ai-sandboxes](https://github.com/awesome-llms-labs/awesome-ai-sandboxes); decision-making evals → [awesome-decisions-llms](https://github.com/awesome-llms-labs/awesome-decisions-llms).

## Verification rules (the important part)

This list's credibility rests on its ✅/⚠️ stamps. Do not mark an entry verified unless you personally confirmed the claim on the **vendor's official page, the project's repo, or the arXiv abstract page** on the date you stamp.

- **Verified** (`orchestration_verified: true`): include a working official `source_url` and the `verified_date` (ISO `YYYY-MM-DD`).
- **Unverified** (`orchestration_verified: false`): leave `source_url` and `verified_date` empty. It is always better to flag something unverified than to invent a fact.
- Never copy claims from press releases or aggregator listicles without checking the official source — and say so in the `notes` field when a claim is provisional.

## JSON schema

Every entry in [`data/orchestration.json`](data/orchestration.json) must be an object with exactly these fields:

| Field | Type | Required | Notes |
|---|---|---|---|
| `name` | string | yes | Canonical project name |
| `vendor` | string | yes | Organization / steward |
| `url` | string | yes | Official HTTPS URL (repo or docs) |
| `description` | string | yes | One-to-three sentences, no marketing fluff |
| `category` | string | yes | One of: `framework`, `protocol`, `memory`, `platform`, `observability`, `benchmark`, `paper` |
| `features` | array of strings | yes | 3–8 keywords describing what it does |
| `orchestration_verified` | boolean | yes | See verification rules above |
| `source_url` | string | yes | Official page where you verified the claim (empty string if unverified) |
| `verified_date` | string | yes | `YYYY-MM-DD` (empty string if unverified) |
| `status` | string | yes | One of: `active`, `maintenance`, `archived`, `commercial`, `paper` |
| `notes` | string | yes | Verification provenance, renames, license caveats, naming collisions |

### Status meanings

- `active` — maintained and recommended for new builds.
- `maintenance` — community-maintained, no new features (e.g. AutoGen).
- `archived` — discontinued; listed for lineage with a `docs/status-changes.md` entry (e.g. Flowise, ACP).
- `commercial` — managed service with no OSS core.
- `paper` — research artifact; adoption may be academic-only.

### Status changes

When a project's status changes (archive, merge, rename, license change):

1. Update the JSON record (`status`, `url` if moved, `notes`, `verified_date`).
2. Add a line to `docs/status-changes.md`, newest first.
3. If a README section references it (e.g. Discontinued & superseded), update that too.

## Local validation

Before opening a PR, run the same checks CI runs:

```bash
# 1. JSON parses and satisfies the schema
python3 - <<'EOF'
import json, re
d = json.load(open('data/orchestration.json'))
cats = {'framework','protocol','memory','platform','observability','benchmark','paper'}
stats = {'active','maintenance','archived','commercial','paper'}
req = ['name','vendor','url','description','category','features','orchestration_verified','source_url','verified_date','status','notes']
for i, e in enumerate(d):
    assert isinstance(e, dict), i
    assert set(e) == set(req), (i, set(e) ^ set(req))
    assert e['name'] and e['vendor'] and e['description'], i
    assert e['url'].startswith('https://'), (i, e['url'])
    assert e['category'] in cats and e['status'] in stats, i
    assert isinstance(e['features'], list) and len(e['features']) >= 3, i
    assert isinstance(e['orchestration_verified'], bool), i
    if e['orchestration_verified']:
        assert e['source_url'].startswith('https://'), (i, e['name'])
        assert re.fullmatch(r'\d{4}-\d{2}-\d{2}', e['verified_date']), (i, e['name'])
    if e['status'] in ('archived','maintenance'):
        assert e['notes'], (i, e['name'])
names = [e['name'] for e in d]
assert len(names) == len(set(names)), 'duplicate names'
print(f'OK: {len(d)} entries, {sum(e["orchestration_verified"] for e in d)} verified')
EOF
```

CI also runs a lychee link check over every markdown file — if your link 403s to bots, add the domain (regex-bare, e.g. `example\.com`) to `.lychee.toml` rather than removing a good source.

## Pull request checklist

- [ ] New/changed entry validates against the schema above
- [ ] Verification stamp is honest (official source, correct date) or explicitly unverified
- [ ] README updated if the entry belongs in a section (and counts in the intro stay accurate)
- [ ] Status-change entries added to `docs/status-changes.md` where relevant
- [ ] No sibling-repo URLs left pointing at personal accounts (use `github.com/Awesome-llms-labs/...`)
