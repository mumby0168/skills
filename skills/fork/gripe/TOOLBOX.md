# Toolbox

Reuse the repo's tools. These are defaults for gaps, not a list to install.

| Concern | .NET | TypeScript |
| --- | --- | --- |
| Formatting | [dotnet format](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-format) with `.editorconfig` | [Prettier](https://prettier.io/docs/comparison) |
| Code quality | [Built-in .NET analyzers](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/overview) first; [Roslynator](https://josefpihrt.github.io/docs/roslynator/) when a specific diagnostic fills a gap, enabled selectively before writing a custom rule | [ESLint with typescript-eslint](https://typescript-eslint.io/docs/) |
| Architecture boundaries | [ArchUnitNET](https://github.com/TNG/ArchUnitNET) for compiled dependency tests when project references cannot enforce the boundary; scope to selected assemblies or namespaces | [dependency-cruiser](https://github.com/sverweij/dependency-cruiser) for dependency boundaries and cycles when existing import restrictions are insufficient; scope rules to service or domain paths |
| Unused code and dependencies | Existing compiler and analyzer diagnostics | [Knip](https://knip.dev/explanations/why-use-knip) for unused files, exports, and dependencies; verify entry points and framework configuration, including consumers in related workspaces |
| Custom structural patterns | [Semgrep](https://docs.semgrep.dev/supported-languages), when existing rules cannot express the check | Same |

- For structural matches or rewrites that Semgrep cannot handle reliably, test [ast-grep](https://ast-grep.github.io/guide/introduction) as a fallback for C# and TypeScript. Rules are language-specific; syntax matches do not establish type identity. Keep one enforcement tool per concern.
- Keep compiler and type checks as the baseline. Formatters handle layout; analyzers handle code rules.
- Choose the cheapest reliable check. Use compiler-aware analysis when types or symbol identity matter. Verify the installed Semgrep edition and language support against the actual examples.
- For architecture boundaries, start with project references, import restrictions, and existing architecture tests. Add a dedicated library only for a demonstrated gap.
- Add only the rule this gripe needs. Test counterexamples, measure runtime, and scope it to the affected service, package, or domain. Follow the skill's approval step for new tooling.
