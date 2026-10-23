# button

2026-10-22 — golden pair via CLI — migrated the pristine radix-vega wrapper to base-vega.

## Changed

- `src/components/ui/button.tsx:1`: replaced Radix Slot/native button with the real Base UI Button primitive and its props; retained Vega variants and sizes.
- `components.json:3`: switched the whole-project registry style to base-vega.
- `.migration/button.md`: recorded migration and verification.
- Registry classification: stock radix-vega matched after formatting. `grep -n "radix-ui\|@radix-ui" src/components/ui/button.tsx` is clean; no IconPlaceholder remains.
- Per-component build passed. Type checking reported only the pre-existing tooltip consumer `render` error, to be resolved by the tooltip migration.
- Consumer sweep: no button `asChild` usage, custom event callbacks, or form-submit consumers need conversion.

## Left alone

- `src/components/ui/code-block.tsx`: custom non-Radix composition; existing Button props remain compatible.
- `src/routes/index.tsx`: existing button usage is compatible; tooltip consumer handled separately.
- No cmdk, vaul, sonner, input-otp, calendar, or chart wrappers are installed.

## Behavior changes

- Composition now uses `render`, not `asChild`; non-button render targets require appropriate `nativeButton` configuration.
- Base UI supplies button semantics, including a default non-submit button type.
- The stock Base registry omits the old `data-variant` and `data-size` attributes. No project selectors depend on them.

## Verify by hand

- Tab to wrap-lines, line-numbers, and copy controls; confirm visible focus.
- Activate each with Enter and Space; check toggles update once and copy still works.
- Confirm icon sizes, hover appearance, and disabled states remain correct.
