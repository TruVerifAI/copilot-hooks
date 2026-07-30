# Changelog — TruVerifAI gates for GitHub Copilot

## 0.18.0 (first release)
- Write + commit gates for Copilot CLI / VS Code / Cloud Agent via
  `.github/hooks` config, generated from the shared cross-platform gate core.
- Total-containment lifecycle on the CLI surface (fail-closed host): any gate
  error emits an explicit allow — never blocks your edit.
- VS Code agent hooks are Preview upstream; a format change fails open.
