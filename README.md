# Terraform Modules for Compliance

Lab 2.4 (GCP): a Terraform module with a hardcoded compliance floor. Consumers
supply naming and business config; the module enforces encryption, access
control, versioning, retention, and required labeling regardless of what a
consumer passes in.

## Layout

- `terraform/modules/compliant-gcs-bucket/` — the module. See its own
  [README](terraform/modules/compliant-gcs-bucket/README.md) for the full
  input/output reference and the control-to-evidence mapping.
- `terraform/consumers/dev/` — dev consumer (30-day retention).
- `terraform/consumers/prod/` — prod consumer (365-day retention).
- `terraform/consumers/negative-test/dev/` — negative test: `environment =
  "prod"` with `retention_days = 30`, which fails Terraform's variable
  validation at `plan` time, before any resource is created.
- `evidence/lab-2-4/` — captured evidence from an applied `dev` run:
  `plan.json` (`terraform show -json tfplan`) and
  `compliance_attestation.json` (`terraform output -json
  compliance_attestation`), the latter cross-checked against the actual
  applied `terraform.tfstate`.

## Controls enforced

SC-12, SC-13, SC-28, AU-11, CM-6 — none exposed as consumer-settable
variables. Full detail in the module README.

Local Terraform state, plan binaries, and `.terraform/` provider caches are
intentionally excluded from this repo (see `.gitignore`); they aren't
portable and can carry resource-specific data that doesn't belong in
version control.
