# CLAUDE.md — SoapBros platform (godhand, godo, behelit, falconia, guts)

Scope: the five platform repos in this folder. The `sudoku*` folders are a
different project (the Nonet game) with their own instructions — ignore this
file when working there.

Each repo has its own `CLAUDE.md` and a contract skill. This file is the
cross-repo picture and the working method. Where this file and a repo's
contract disagree, the contract wins; where a contract and the tree disagree,
the tree wins.

## What this is

A multi-cloud infrastructure platform written as code, plus the images and
automation around it. Nothing here has ever been planned or applied against a
real cloud account. Everything is proven offline only (format, validate,
render, policy tests, security scans). Say "validated offline", never "works"
or "deployed".

| Repo | What it is | Depends on |
|---|---|---|
| `behelit` | CI toolchain image (`behelit-ci-toolchain`: terraform, tofu, terragrunt, terramate, tfsec, checkov, conftest, four cloud CLIs) and a Packer template for a self-hosted runner VM | nothing |
| `godo` | Internal images built on behelit: `factory` (toolchain + Claude Code + gh) and `femto-portal` (the Backstage developer portal) | behelit digest |
| `godhand` | The infrastructure code: one Terraform module set for Azure, STACKIT, GCP and AWS, run by four orchestrators. Also `femto/`, the Backstage catalog data and scaffolder templates | behelit digest, godo factory digest |
| `falconia` | Azure Databricks lakehouse built on godhand's Azure modules at a pinned commit | godhand commit SHA, behelit digest |
| `guts` | The GitOps layer. What runs on every cluster: add-ons, shared manifests and apps, delivered by Argo CD or Flux from the same files | nothing yet |

Dependencies flow one way: behelit → godo → godhand → falconia. A change
upstream reaches downstream only when the downstream pin is bumped.

Known defects are in `godhand/docs/fixes.md`, one task per pull request.
Planned expansion (new clouds, on-premises, the open-source stack) is in
`godhand/docs/roadmap.md`. Read both before proposing improvements. Both are on godhand `main`.

Brand rules, tokens, logos and a paste-into-any-LLM prompt block are in
`SoapBros/.github`, `brand/BRANDING.md` (public). Follow them for anything
visual or any wording a customer sees.

The portal's look is pluggable. Its default is always the Soap Bros 8-bit
skin with the real duck-and-bubbles logo; never draw a logo or a stand-in.
Changing the look, or matching it to a client, follows
`godo/.claude/skills/portal-skin/SKILL.md`.

godhand also has skeletons for Huawei Cloud and Sakura Cloud (three modules
each). The owner's order of work is Huawei and Sakura first, then
on-premises.

**Names.** Every internal tool has a Berserk codename (godhand, void, slan,
conrad, ubik, femto, behelit, godo, falconia, guts). They are codenames
only. A client's copy uses the client's own names and look, so keep names in
one place per repo where you can, and never put a codename in anything a
client sees unless they chose it. A new internal tool gets a Berserk name;
ask the owner to confirm it.

In `guts`, add-ons and apps are data; never hand-write an Argo CD or Flux
object for one, and never edit `flux/generated/`. Its rules are in
`guts/CLAUDE.md`; its gates are `scripts/gates.sh`. Never run `kubectl
apply`, `helm install`, `argocd` or `flux` against a cluster.

`femto` is not a repo. It is `godhand/femto/` (catalog data) plus
`godo/images/femto-portal/` (the Backstage app that serves it).

### godhand's four orchestrators

The same modules are wired four ways. `void` is the reference; the others
copy it.

| Folder | Tool | State of it |
|---|---|---|
| `void/` | Terragrunt | every cloud, dev + staging + prod, AWS landing zone |
| `slan/` | Terramate | dev only, partial |
| `conrad/` | plain Terraform | one or a few proof units per cloud |
| `ubik/` | OpenTofu | same shape as conrad |

Run `./godhand list` for the real coverage. Do not quote unit or module
counts from docs or memory; several docs are stale.

### The factory

Each repo has slash commands in `.claude/skills/` that do the common jobs
(new module, port units, bump toolchain, new data product). The same commands
run headless in GitHub Actions (`claude-factory.yml`) from a labelled issue
and end in a pull request. The factory writes code and runs gates. A human
reviews, merges and applies.

## Folder map

```
behelit/
  docker/ci-toolchain/Dockerfile     every tool version is an ARG here
  packer/runner/                     runner VM; install.sh versions must match the Dockerfile
  .github/workflows/                 build-and-push (publishes digest, dispatches to godhand + godo), packer, claude-factory

godo/
  images/factory/Dockerfile          ARG TOOLCHAIN_IMAGE is owned by rebase.yml
  images/femto-portal/               full Backstage app source, multi-stage Dockerfile
  .github/workflows/                 build (per-image, digest in run summary), rebase

godhand/
  modules/<cloud>/<name>/            pure HCL, six files each, no provider or backend blocks
  void/<cloud>/root.hcl              backend + provider + mandatory tags
  void/<cloud>/<env>/env.hcl         everything that differs per environment
  void/<cloud>/<env>/<unit>/terragrunt.hcl   byte-identical across dev/staging/prod
  void/aws/{management,network,log-archive}/ landing-zone tiers, not environments
  slan/<cloud>/_generate.tm.hcl      generates each stack's _generated_*.tf
  conrad/ ubik/                      seven hand-written files per unit
  femto/                             catalog entities + one scaffolder template per cloud
  policy/opa/                        mandatory-tags rule + tests
  scripts/factory/gates.sh           the definition of done
  scripts/ci/                        validate.sh, render-void.sh
  scripts/godhand/                   CLI internals; guide.sh holds TOOLCHAIN_IMAGE
  scripts/bootstrap-*-state.sh       create real cloud resources — human only
  docs/limitations.md                what is not proven; keep it honest
  .claude/                           contract skill, factory skills, agents, guard + fmt hooks

falconia/
  common/common-locals.hcl           godhand source + pinned commit SHA
  live/root.hcl                      backend, azurerm provider, tags
  live/<env>/env.hcl                 per-environment values
  live/<env>/<unit>/terragrunt.hcl   byte-identical across environments (gated)
  live/<env>/**/{data-products,grants,endpoints,indexes,models}/*.yaml   requests as data
  modules/<name>/ + tests/*.tftest.hcl
  backstage/templates/               request templates that open PRs adding YAML
  scripts/gates.sh                   the definition of done
  .github/workflows/deploy.yml       manual plan/apply of one unit — human only
```

## Before you touch anything

1. Work from inside the repo you are changing. A session started in
   `~/soapbros` does not load the repo's `.claude/settings.json`, hooks or
   skills, so the guard hook and formatter are not running. If you are in the
   parent folder, read the contract file yourself and format by hand.
2. Read the contract first:
   - godhand: `.claude/skills/godhand-contract/SKILL.md`
   - falconia: `.claude/skills/falconia-contract/SKILL.md`
   - godo and behelit: the repo `CLAUDE.md`
3. Open the nearest existing sibling of what you are about to write (same
   cloud, same tool, same kind of resource) and copy its shape.
4. For a job a factory command covers, use the command's skill file as the
   procedure even if you are doing it by hand.

## Commands

Run from the repo root. All are offline and safe.

```sh
# godhand
./godhand list                           # coverage: tool × cloud × env
scripts/factory/gates.sh [cloud…]        # every CI gate; 0 pass, 1 fail, 3 skipped (not a pass)
scripts/factory/gates.sh --in-image      # same, inside the pinned toolchain image (needs docker)
scripts/ci/validate.sh <cloud>           # terraform/tofu validate for modules, conrad, ubik, slan
scripts/ci/render-void.sh <cloud>        # terragrunt render of every void unit, mock outputs
scripts/factory/parity.sh --summary      # units void has that the other tools lack
cd slan && terramate generate            # after any slan change
./godhand docs                           # after adding or removing units; commit the result
conftest verify --policy policy/opa      # OPA unit tests

# falconia
scripts/gates.sh [--in-image]            # same exit codes
scripts/render.sh [env…]
python3 scripts/check-data.py            # validates the YAML request files

# behelit
docker build docker/ci-toolchain         # the final RUN prints every tool version
(cd packer/runner && packer init . && packer fmt -check . && packer validate -var image_version=x -var gcp_project_id=placeholder -var azure_subscription_id=00000000-0000-0000-0000-000000000000 .)

# godo
docker build -t godo-factory:local images/factory
docker build -t femto-portal:local images/femto-portal
# yarn for femto-portal runs inside node:24-trixie-slim if the host Node is not 22 or 24
```

There is no "run" for the infrastructure: no credentials are meant to be
used from here and nothing is deployed. "Build" means the docker builds
above. "Test" means the gates.

## Coding rules

Terraform modules (godhand and falconia):
- Exactly the contract's file set. Every variable has `type` and
  `description`; every output has `description`.
- No provider blocks, no backend, no tool-specific syntax in a module. It must
  run unchanged under Terraform and OpenTofu.
- The toolchain runs Terraform 1.9.8 and modules declare `>= 1.7`. Do not use
  language features newer than that.
- Never guess a provider attribute. Check it with
  `terraform init -backend=false` then `terraform providers schema -json`,
  and delete `.terraform/` and the lock file afterwards.
- Copy the cloud's existing provider constraint. Do not widen or bump it in a
  module change.
- Tag variable is `tags` on Azure and AWS, `labels` on GCP and STACKIT. Pass
  it to every taggable resource. Mandatory keys: `org`, `managed_by`, `owner`.
- Security findings: fix them. If a finding is a false positive, suppress it
  inline with the reason (`#tfsec:ignore:<rule>` plus a comment,
  `#checkov:skip=<id>:<reason>`). Do not add to `modules/.checkov.baseline`
  to make a gate pass.

Units:
- void and falconia unit files are byte-identical across environments. Write
  once, copy to every env, put differences in each `env.hcl`.
- Never set the tag variable in a void unit; `root.hcl` injects it.
- Every `dependency` block has realistic `mock_outputs` and
  `mock_outputs_allowed_terraform_commands = ["validate", "plan", "init"]`.
  falconia also needs the `skip_outputs = get_env("FALCONIA_RENDER_MOCKS", …)` line.
- AWS void units outside `management/` include both `root` and `account`.
- Never hand-edit `slan/**/_generated_*.tf`. Run `terramate generate`.
- conrad and ubik units differ in exactly three things: the state key, the
  backend comment's tool line, and `managed_by`. Write conrad, copy to ubik.
- If an upstream unit is missing for that tool, stop and report it. Do not
  invent a mock or wire around it.

Secrets and identifiers:
- No secrets, account IDs, subscription IDs or project IDs in the repos.
  void reads them with `get_env("TG_GODHAND_<NAME>")` and the name gets a
  description in `env_var_description()` in `scripts/godhand/lib.sh`.
  conrad, ubik and slan use a variable with no default, left out of tfvars.
- falconia YAML holds a secret's key name, never its value.
- Images: `${ENV}` substitution only in configs.

Pinning:
- Images by `@sha256:` digest, never a tag. godhand by full commit SHA in
  falconia, never a branch.
- A toolchain digest lives in several files per repo. Change all of them
  together: godhand's workflow jobs and `scripts/godhand/guide.sh`; falconia's
  `ci.yml`, `deploy.yml` and `scripts/gates.sh`. Use `/adopt-toolchain`.
- Never edit `ARG TOOLCHAIN_IMAGE` in godo by hand; `rebase.yml` owns it.
- In behelit, a version changes in the Dockerfile and `packer/runner/install.sh`
  together, and is checked against the real release asset name first.

Never commit: `.terraform/`, `.terraform.lock.hcl`, `.terragrunt-cache/`,
`.godhand/`, `.factory/`, `tfplan*`, state files.

Git: conventional commits (`feat(modules): …`, `feat(slan): …`, `ci: …`,
`docs: …`). No AI attribution in commits, PRs or docs. Work on a branch and
open a PR; commit or push to `main` only when the user says so in that
message.

## Design rules

- **One module contract, cloud-native inside.** Inputs follow the same shape
  across clouds (`name`, region, tag variable, network references). Internals
  stay idiomatic to the cloud. Do not force uniformity.
- **Secure by default.** Private endpoints, no public access, encryption with
  a customer-managed key where the service has one, deletion protection on
  data stores. A less secure setting is an explicit input with a reason.
- **Environments differ only by data.** One `env.hcl` per environment. Prod
  is sized up there, never in a unit.
- **Requests are data, not code.** In falconia a data product, grant,
  endpoint, index or model is a YAML file. Units and modules never change for
  a request.
- **void is the reference.** New behaviour lands in void first; the other
  tools mirror its dependency graph.
- **Network ranges are allocated.** Each environment has its own /16s (table
  in godhand's README; falconia uses 10.100–10.165). Take the next free
  block; never reuse one.
- **The factory never touches a cloud.** It writes files and runs offline
  gates. Humans apply.
- **Honest validation.** Every claim names the check that proved it. Anything
  unproven goes in `docs/limitations.md`.
- **Generated docs are generated.** `docs/guides/` and the README matrix come
  from `./godhand docs`. Do not edit them by hand.

## Mistakes a weaker model makes here, and how to avoid each

| Mistake | Do this instead |
|---|---|
| Saying a change "works" or "is deployed" | Say "validated offline" and name the gates that ran. Nothing has been applied, ever. |
| Treating gates exit code 3 as success | 3 means a tool was missing and a gate was skipped. Re-run with `--in-image`. If docker is unavailable, report those gates as unverified. |
| Running `plan`, `apply`, `import`, a bootstrap script, or `aws`/`az`/`gcloud`/`databricks`/`stackit` | Never. This machine may hold real cloud credentials. The guard hook does not catch every form (`bash -c '…'`, `;`, `&&`, cloud CLIs in godhand) and does not run at all from the parent folder. The rule holds whether or not a hook stops you. |
| Editing a unit in one environment | Edit dev, copy the file to every env, diff them. Per-env values go in `env.hcl`, added to all env files. |
| Writing resource arguments from memory | Pull the provider schema. Provider versions float within `~>` and no lock files are committed, so memory is often wrong. |
| Using `tags` on GCP/STACKIT or `labels` on Azure/AWS | Check the table in the godhand contract; copy a sibling module. |
| Adding a module and stopping at the module | Also: `catalog-info.yaml`, `femto/app-config.yaml`, the cloud's `femto/templates/<cloud>/template.yaml` enum, void units in every env, `./godhand docs`, and the counts the contract names. |
| Forgetting `terramate generate` or `./godhand docs` | Both are gates. Run them, keep the regenerated files, run gates again. |
| Making a gate pass by weakening it | Do not edit `gates.sh`, the checkov baseline, the OPA policy or a workflow to turn a red gate green. Fix the code or report the failure. |
| Changing a tool version in godhand or falconia | Tool versions live in behelit. Change them there; downstream repos adopt the new digest. |
| Updating a digest or SHA in one file | Grep for the old value across the repo and change every hit. |
| Expecting falconia to see a godhand change | It will not until `godhand_ref` is bumped with `/adopt-godhand`. |
| Hard-coding a data product, grant or endpoint in a falconia unit | Add a YAML file and run `python3 scripts/check-data.py`. |
| Quoting counts or coverage from a README | Count from the tree (`./godhand list`, `ls modules/<cloud>`). Fix the stale doc if it is in scope. |
| Assuming platform pieces exist | These do not exist yet: any real deployment, an apply pipeline in godhand, cloud OIDC for CI, Argo CD or Flux, an observability stack, a deployed portal, secret scanning in CI. Check `docs/limitations.md` before describing what the platform does. |
| Putting a secret where state or logs can see it | No secret in tfvars, YAML, outputs without `sensitive = true`, or echo statements. Prefer the cloud's managed secret (as `rds-postgresql` does). |
| Fixing unrelated things in the same change | One purpose per PR. List other problems you saw in the report. |
| Looping on a failing gate | Three fix rounds, then stop and report what is still red with the error text. |
| Committing build debris | `git status --porcelain` before finishing; remove `.terraform/`, lock files, caches, plans. |

## Before saying a task is done

- [ ] I worked inside the right repo and read its contract.
- [ ] The change matches the nearest existing sibling's shape and file set.
- [ ] Unit files are identical across every environment (`cmp` or `diff` them); per-env values are in every `env.hcl`.
- [ ] Generated files are regenerated: `terramate generate`, `./godhand docs`.
- [ ] For a new module: catalog entry, femto config, template enum, units, docs counts.
- [ ] Every digest or SHA I changed is changed everywhere it appears.
- [ ] The gates script for the repo exits 0. I quote its summary line verbatim. If it exited 3, I re-ran in the image or I say which gates are unverified.
- [ ] No gate, baseline, policy or workflow was weakened to get there.
- [ ] `git status --porcelain` shows only intended files; no lock files, caches, plans, state or `.godhand/`.
- [ ] No secrets or account identifiers in the diff.
- [ ] I ran no plan, apply, import, bootstrap script or cloud CLI.
- [ ] Anything I could not prove is stated, and added to `docs/limitations.md` if it is a lasting gap.
- [ ] My report says: files changed, gates summary, what was not verified, the apply order a human would follow if relevant, and a conventional commit message with no AI attribution.
- [ ] I did not commit or push unless asked.
