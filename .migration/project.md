# Project migration

2026-10-22 — whole-project Radix to Base UI migration completed on `migrate/radix-to-base`.

## Dependency and registry changes

- `components.json`: radix-vega → base-vega. Future shadcn additions use Base UI while preserving the Vega preset.
- `@base-ui/react` was already installed at ^1.9.0, so no redundant dependency installation was needed.
- `package.json` and `pnpm-lock.yaml`: removed radix-ui only after button and tooltip were migrated, using `vp remove` with pnpm 12.15.0; followed by `vp install`.
- Separate commits cover button and tooltip; reports are `.migration/button.md` and `.migration/tooltip.md`.

## App-code sweep

- Reviewed all three UI wrappers and their consumers, including index route, root provider, and code-block button composition.
- Preserved the user's wrap-lines tooltip change in a baseline checkpoint. That checkpoint bypassed the hook because it used Base UI's render API with the still-Radix wrapper; component migration commits passed the hook normally.
- Fixed that pre-existing render type error by migrating TooltipTrigger. Only formatting was needed in the consumer.
- No remaining Radix imports, CSS variables, asChild usages, old provider delay props, or incompatible callback props in src.
- code-block is a custom non-Radix wrapper and was intentionally unchanged. No cmdk, vaul, sonner, input-otp, calendar, or chart wrappers were present.
- Mandatory leftover scan found no radix-ui, @radix-ui, or IconPlaceholder occurrences in src, package.json, or pnpm-lock.yaml.
- Behavior differences and concrete manual QA steps are listed in each component report. Browser QA has not been performed.

## Verification

- Baseline build: passed before migration.
- Baseline check: formatting issue and TooltipTrigger render prop type error in the user's index-route change.
- Per-component builds: passed. Button-stage type check retained only the known tooltip error; tooltip-stage type check passed.
- Final `vp check`: passed formatting, lint, and type checks.
- Final `vp run build`: passed after dependency removal.
- Final `vp test`: no test files; exits 1 with the same pre-existing React `ReferenceError: module is not defined` and server shutdown timeout seen before migration. This is not a passing test suite.
- `vp env doctor`: overall all checks passed; PATH warnings say node/npm/npx resolve directly to the managed Node 24.22.1 binaries rather than vp shims. pnpm 12.15.0 resolves correctly through its shim. No environment changes made.
- Build/test regenerated routeTree.gen.ts with formatting-only changes; restored that generated file to avoid unrelated drift.

0 wrappers remain on Radix.
