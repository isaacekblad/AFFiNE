# Ode Platform Foundation Guide

Ode is envisioned as a knowledge studio that borrows the collaborative canvas, document, and planning primitives from AFFiNE while introducing its own brand story and curated workflows. This document captures how to treat the existing AFFiNE mono-repository as the technical foundation for Ode.

## Guiding Principles

- **Reuse core primitives**: Retain the AFFiNE block-based editor, canvas rendering, and synchronization layers (`blocksuite` workspace, CRDT data model, and syncing services). These components already power a reliable local-first experience.
- **Progressive rebranding**: Layer Ode-specific branding, assets, and messaging without forking the runtime logic. Keep shared components (UI kit, editor modules) in place and theme them via the existing design token system.
- **Composable feature set**: Ship Ode features as opt-in packages or feature flags inside the `packages/frontend` workspaces, so upstream AFFiNE updates remain consumable.

## Suggested Roadmap

1. **Branding layer**
   - Duplicate the `packages/frontend/apps/web` entry point into a new `ode` application package while re-exporting AFFiNE core modules.
   - Configure Tailwind and the shared theme provider with Ode color palettes, typography, and iconography.
   - Replace marketing copy and default templates with Ode narratives.
2. **Workflow curation**
   - Create Ode-specific workspace templates inside `packages/frontend/core/src/components/affine/template-gallery`.
   - Introduce guided onboarding steps (e.g., Ode “first canvas” tour) by extending the onboarding flows under `packages/frontend/core/src/components/affine/onboarding`.
3. **Service integration**
   - Wire Ode’s authentication and billing APIs by adapting the AFFiNE backend gateway modules in `packages/backend`.
   - Keep the `blocksuite` synchronization contracts untouched to ensure cross-project compatibility.
4. **Release pipeline**
   - Add CI jobs that build both the AFFiNE reference apps and the Ode branded targets using the scripts under `tools/`.
   - Publish Ode artifacts with distinct app IDs (mobile) and bundle identifiers (desktop) while sharing code owners and linting rules.

## Configuration Touchpoints

| Area           | Files / Modules                                                | Notes                                                                            |
| -------------- | -------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Branding       | `packages/frontend/theme`, `packages/frontend/core/src/styles` | Update theme tokens, image assets, and font declarations.                        |
| Product naming | `package.json`, `docs/`, marketing copy                        | Use feature flags to toggle references between AFFiNE and Ode during transition. |
| Desktop builds | `packages/frontend/apps/electron`, `blocksuite/apps/desktop`   | Introduce new bundle identifiers and icons; reuse updater infrastructure.        |
| Mobile builds  | `packages/frontend/apps/mobile`, `packages/frontend/apps/ios`  | Share module configuration while customizing splash screens and asset catalogs.  |

## Collaboration Model

- Track Ode-specific work as feature branches that merge into this repository, keeping parity with AFFiNE’s upstream changes.
- Contribute improvements back to the shared modules first; isolate Ode-only logic behind configuration boundaries to avoid long-term divergence.
- Document any deviations inside this `docs/ode` folder so future contributors can audit the differences quickly.

By following these guidelines, Ode can iterate quickly on top of AFFiNE’s mature foundation while maintaining compatibility with upstream enhancements.
