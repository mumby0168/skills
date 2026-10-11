# Toolbox

Reuse the repo's tools. These are defaults for gaps, not a list to install.

| Concern | .NET | TypeScript |
| --- | --- | --- |
| Formatting | [dotnet format](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-format) with `.editorconfig` | [Prettier](https://prettier.io/docs/comparison) |
| Code quality | [Built-in .NET analyzers](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/overview) | [ESLint with typescript-eslint](https://typescript-eslint.io/docs/) |
| Custom structural patterns | [Semgrep](https://docs.semgrep.dev/supported-languages), when existing rules cannot express the check | Same |

- Keep compiler and type checks as the baseline. Formatters handle layout; analyzers handle code rules.
- Choose the cheapest reliable check. Use compiler-aware analysis when types or symbol identity matter. Verify the installed Semgrep edition and language support against the actual examples.
- For architecture boundaries, start with project references, import restrictions, and existing architecture tests. Add a dedicated library only for a demonstrated gap.
- Add only the rule this gripe needs. Test counterexamples, measure runtime, and scope it to the affected service, package, or domain. Follow the skill's approval step for new tooling.
