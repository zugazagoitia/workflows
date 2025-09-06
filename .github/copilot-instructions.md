# e-CODEX Reusable Workflows Repository

This repository contains reusable GitHub Actions workflows and configuration files for the e-CODEX project. It is NOT a traditional application but a collection of workflow templates, configuration files, and custom actions for CI/CD pipelines.

**ALWAYS** reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Working Effectively

### Quick Setup and Validation
- Install Node.js dependencies for validation tools:
  - `npm install prettier eslint @commitlint/config-conventional commitlint` -- takes 15 seconds. NEVER CANCEL.
- Validate all files before making changes:
  - Check YAML syntax: `yamllint .github/workflows/*.yaml .github/workflows/*.yml`
  - Check JavaScript formatting: `npx prettier --check *.js`
  - Validate commit messages: `npx commitlint --last --verbose`
  - Format JavaScript files: `npx prettier --write *.js` (if needed)

### Core Repository Structure
- `.github/workflows/` - Reusable workflow YAML files organized by theme:
  - `sast/` - Static Analysis Security Testing workflows
  - `ci-cd/` - Continuous Integration and Deployment workflows  
  - `security/` - Security-focused workflows
  - `github/` - GitHub-specific workflows
  - `testing/` - Testing workflows
  - `validate-repository.yaml` - Repository validation workflow
- `.github/actions/install-graphviz/` - Custom action for installing Graphviz across platforms
- `checkstyle.xml` - Java code style configuration (Google-based)
- `.eslintrc.js`, `.prettierrc.js`, `commitlint.config.js` - JavaScript/commit formatting configs
- `README.md` - Comprehensive documentation of all workflows and usage examples

### Essential Validation Commands
Run these commands EVERY time before making changes:

```bash
# Install tools (run once per session)
npm install prettier eslint @commitlint/config-conventional commitlint

# Validate YAML files - takes 5 seconds. NEVER CANCEL.
yamllint .github/workflows/ .github/actions/

# Check JavaScript formatting - takes 3 seconds. NEVER CANCEL.
npx prettier --check *.js

# Validate commit message format - takes 2 seconds. NEVER CANCEL.
npx commitlint --last --verbose

# Fix JavaScript formatting if issues found
npx prettier --write *.js
```

### Working with Workflow Files
- **DO NOT** modify workflow files without YAML validation first
- **ALWAYS** check workflow syntax: `yamllint .github/workflows/[folder/filename]`
- **NEVER** commit workflow files with YAML syntax errors
- Test workflow changes by creating a test repository that uses the workflows
- **NEW**: The repository includes `validate-repository.yaml` that automatically validates:
  - YAML syntax across all workflows
  - Conventional commit messages
  - JavaScript formatting
  - External action version pinning to commit SHA
  - Security scanning with CodeQL

### Timing Expectations
- **NEVER CANCEL** any commands - all validation is fast:
  - npm install: ~15 seconds
  - YAML validation: ~5 seconds
  - Prettier checks: ~3 seconds
  - Commitlint validation: ~2 seconds
- Set timeout to 60+ seconds for npm install, 30+ seconds for other commands

## Validation Scenarios

### After Making Any Changes
1. **ALWAYS** run full validation suite:
   ```bash
   # Install validation tools first
   npm install prettier eslint @commitlint/config-conventional commitlint
   
   # Validate syntax and formatting
   yamllint .github/workflows/ .github/actions/
   npx prettier --check *.js
   npx commitlint --last --verbose
   ```

2. **Test workflow integration** (if workflow files changed):
   - Create a test repository or use an existing e-CODEX project
   - Reference the modified workflow from a test GitHub Actions file
   - Verify the workflow runs without errors in GitHub Actions tab

3. **Validate configuration files** (if config files changed):
   - Checkstyle.xml: Verify syntax with `yamllint` (it's XML but similar validation)
   - JavaScript configs: Run `npx prettier --check *.js` after changes
   - Test actual functionality: Create a Java file and test checkstyle rules

4. **Manual test scenarios**:
   - Format JavaScript files: `npx prettier --write *.js`
   - Check specific workflow: `yamllint .github/workflows/[specific-file].yaml`
   - Validate specific commit: `echo "feat: test commit" | npx commitlint`

### Commit Message Validation
- **ALWAYS** use conventional commit format: `type(scope): description`
- Examples: `feat: add maven snapshot workflow`, `fix(checkstyle): update Java 21 compatibility`
- Test with: `npx commitlint --last --verbose`

## Key Workflows in Repository

### CI/CD Workflows (.github/workflows/)
- `maven-ci.yaml` - Maven CI pipeline with multi-OS testing, SBOM generation
- `maven-publish-snapshot.yaml` - Publish Maven snapshots to repository
- `maven-publish-release.yaml` - Publish Maven releases
- `maven-tag-release-version.yaml` - Tag release versions
- `maven-validate-release-version.yaml` - Validate release versions

### SAST (Security) Workflows
- `sonar-java.yaml` - SonarCloud analysis for Java projects
- `qodana.yaml` - Qodana code quality analysis
- `codeql-java.yaml` - GitHub CodeQL security analysis
- `java-linting.yaml` - Checkstyle-based Java code linting

### Utility Workflows
- `commitlint.yaml` - Conventional commit message validation
- `dependency-review.yml` - Dependency security review
- `docker-build-push.yaml` - Docker image build and push

### Custom Actions (.github/actions/)
- `install-graphviz/` - Cross-platform Graphviz installation (Ubuntu, Windows, macOS)

## Configuration Files

### checkstyle.xml
- Google-based Java code style configuration with e-CODEX customizations
- Used by java-linting.yaml workflow
- IDE setup URL: `https://raw.githubusercontent.com/e-CODEX/workflows/main/checkstyle.xml`

### JavaScript Configuration Files
- `.eslintrc.js` - ESLint configuration (legacy format, needs migration to flat config)
- `.prettierrc.js` - Prettier formatting rules
- `commitlint.config.js` - Conventional commit validation rules

## Common Tasks

### Adding a New Workflow
1. Create new YAML file in `.github/workflows/`
2. Follow existing workflow patterns for inputs/outputs
3. Validate with: `yamllint .github/workflows/[new-workflow].yaml`
4. Test in a sample repository before committing
5. Update README.md with usage documentation

### Modifying Configuration Files
1. Edit the configuration file
2. Validate JavaScript files: `npx prettier --check [file].js`
3. For checkstyle.xml: validate against a sample Java file
4. Test changes in dependent repositories

### Repository Maintenance
1. **ALWAYS** validate before committing:
   ```bash
   yamllint .github/workflows/*.yaml .github/workflows/*.yml
   npx prettier --check *.js
   ```
2. **NEVER** commit dependency files (node_modules/, package*.json)
3. Use `.gitignore` to exclude temporary files
4. Maintain conventional commit message format

## Critical Warnings

- **NEVER CANCEL** validation commands - they complete in under 30 seconds
- **ALWAYS** validate YAML syntax before committing workflow changes
- **DO NOT** test workflows in production repositories - use test repos
- **NEVER** commit node_modules or package*.json files
- **ALWAYS** use conventional commit format to pass CI checks
- **DO NOT** modify workflows without understanding their dependencies and usage patterns

## Tools Available
- Node.js v20+ and npm for JavaScript validation
- yamllint for YAML syntax validation
- GitHub CLI (gh) for repository operations
- Java and Maven (available but not used for building this repo)
- System package managers (apt, brew, choco) available in GitHub Actions

## Common Command Outputs

### Repository Root Structure
```
.
├── .github/
│   ├── actions/install-graphviz/
│   ├── dependabot.yml
│   └── workflows/ (13 workflow files)
├── .eslintrc.js
├── .gitignore
├── .prettierrc.js
├── checkstyle.xml
├── commitlint.config.js
└── README.md
```

### Successful Validation Output
```bash
$ yamllint .github/workflows/commitlint.yaml
# No output = success

$ npx prettier --check .prettierrc.js
Checking formatting...
All matched files use Prettier code style!

$ npx commitlint --last --verbose
⧗   input: feat: add new workflow validation
✔   found 0 problems, 0 warnings
```