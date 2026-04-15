# Validation Strategy

## Runtime Correctness (Automated CI)

HEEx fixture files in `fixtures/liveview/` are validated on every PR and push to `main` via the [Validate LiveView HEEx Fixtures](.github/workflows/validate-liveview.yml) GitHub Actions workflow.

The workflow:

1. Checks out this repo and [zaalipro/liveview-skill-validator](https://github.com/zaalipro/liveview-skill-validator)
2. Copies fixtures into the validator's test directory
3. Runs `mix compile --warnings-as-errors` and `mix test` inside the validator
4. Validates each fixture compiles and renders without exceptions

**What CI validates:**

- HEEx syntax correctness (balanced tags, proper `<%= %>` expressions)
- Phoenix LiveView HTMLEngine compilation
- Runtime rendering with default and themed assigns (dark/light mode)
- No unhandled exceptions during render

**Adding new fixtures:** Create a `.html.heex` file under `fixtures/liveview/<category>/`. The CI step discovers files recursively by category directory. Each fixture should be a standalone HEEx snippet using Nothing Design tokens as Tailwind classes — no external component dependencies.

## QA Boundary (Manual Review)

QA focuses on areas the automated validator cannot assess:

- **Output quality**: Does generated code follow Nothing Design philosophy (Section 2 of SKILL.md)?
- **Prompt behavior**: Does the skill prompt produce correct output for edge cases?
- **Visual fidelity**: Would the rendered output match Nothing Design intent?
- **Token correctness**: Are the right color, spacing, and typography tokens used?
- **Component patterns**: Are slot compositions, JS interactivity, and LiveComponent boundaries used appropriately?
- **Cross-platform consistency**: Do LiveView outputs match the visual intent of React/SwiftUI equivalents?

QA should prompt Claude with the skill (`nothing-design`) and inspect the generated HEEx against the reference fixtures for pattern consistency.
