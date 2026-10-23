# tooltip

2026-10-22 — merge — migrated radix-vega to base-vega, preserving the local omission of the client directive.

## Changed

- `src/components/ui/tooltip.tsx:1`: uses Base UI Tooltip; merged the registry golden pair and resolved the import/directive conflict while preserving the original non-RSC setup.
- `src/components/ui/tooltip.tsx:4`: provider uses `delay`, preserving the zero-delay default.
- `src/components/ui/tooltip.tsx:16`: forwards side/alignment offsets to Positioner, renders Popup and the Base registry's side-aware arrow, and uses Base UI's transform-origin variable.
- `src/routes/index.tsx:56`: formatted the checkpointed `render={<Button />}` composition. Its behavior and wrap toggle callback were retained; migration fixes the baseline Radix prop type error.
- `.migration/tooltip.md`: records migration and verification.
- `grep -n "radix-ui\|@radix-ui" src/components/ui/tooltip.tsx` is clean; no IconPlaceholder remains.
- Type check passed after wrapper migration and again after consumer formatting. Full `vp check` and component-batch build passed.
- App-wide consumer sweep found no remaining asChild, Radix imports/CSS variables, old delay props, or incompatible callbacks.

## Left alone

- `src/routes/__root.tsx`: existing TooltipProvider usage needs no prop conversion.
- `src/components/ui/code-block.tsx`: non-Radix custom composition remains unchanged.
- No cmdk, vaul, sonner, input-otp, calendar, or chart wrappers are installed.

## Behavior changes

- Tooltip content now defaults to top placement, as in the Base Vega registry (Radix default was bottom).
- Tooltip trigger composition uses `render` rather than `asChild`. Provider delay stays zero; hover/focus timing and collision/arrow positioning now follow Base UI.

## Verify by hand

- Hover the wrap-lines control: check tooltip text, top placement, and arrow alignment.
- Tab onto the control: verify focus and accessible tooltip; press Escape to dismiss.
- Activate wrap-lines with Space and a mouse click: ensure it toggles exactly once.
- Move the pointer away and repeat near a viewport edge; verify dismissal and collision positioning.
