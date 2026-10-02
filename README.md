# unp-github

Org-wide GitHub defaults for Unnatural Products.

## New repositories

The org's base permission is **No permission**: members see only public repos and repos a team grants them. New repos get no team automatically, so when you create one, grant the engineering team access:

```bash
gh api -X PUT orgs/UnnaturalProducts/teams/unp-coredevs/repos/UnnaturalProducts/REPO -f permission=push
```

Or in the repo: Settings → Collaborators and teams → Add teams → `UNP-CoreDevs`, role Write.

To catch up every private repo at once (safe to rerun):

```bash
gh api orgs/UnnaturalProducts/repos --paginate --jq '.[] | select(.visibility=="private") | .name' |
  while read r; do
    gh api -X PUT "orgs/UnnaturalProducts/teams/unp-coredevs/repos/UnnaturalProducts/$r" -f permission=push --silent || echo "FAIL $r"
  done
```

`UNP-Vibers` gets access only to the specific repos it needs. CI that reaches another repo uses a deploy key or a GitHub App, never a personal token.
