### `.releaserc` Template for Terraform Project

```json
{
  "branches": ["main"],
  "tagFormat": "${version}",
  "plugins": [
    [
      "@semantic-release/commit-analyzer",
      {
        "preset": "conventionalcommits",
        "releaseRules": [
          { "type": "feat", "release": "minor" },
          { "type": "fix", "release": "patch" },
          { "type": "chore", "scope": "deps", "release": "patch" },
          { "type": "docs", "release": false },
          { "type": "style", "release": false },
          { "type": "refactor", "release": "patch" },
          { "type": "perf", "release": "patch" },
          { "type": "test", "release": false },
          { "type": "ci", "release": false }
        ]
      }
    ],
    [
      "@semantic-release/release-notes-generator",
      {
        "preset": "conventionalcommits",
        "presetConfig": {
          "types": [
            { "type": "feat", "section": "Features" },
            { "type": "fix", "section": "Bug Fixes" },
            { "type": "chore", "section": "Chores" },
            { "type": "docs", "section": "Documentation Updates" },
            { "type": "refactor", "section": "Refactor" },
            { "type": "perf", "section": "Performance Improvements" },
            { "type": "test", "section": "Tests" },
            { "type": "ci", "section": "CI/CD Updates" }
          ]
        }
      }
    ],
    [
      "@semantic-release/changelog",
      {
        "changelogFile": "CHANGELOG.md"
      }
    ],
    "@semantic-release/github",
    [
      "@semantic-release/git",
      {
        "assets": ["CHANGELOG.md"],
        "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
      }
    ]
  ]
}
```

### Explanation of the Configuration

1. **Branches**: The `main` branch is the default branch for release. You can modify this if your release branch is different (e.g., `master`, `develop`).

2. **Tag Format**: Tags will be created in the format `${version}` (e.g., `1.0.0`).

3. **Plugins**:
   - **`@semantic-release/commit-analyzer`**: Analyzes commit messages and determines the type of release based on custom rules.
   - **`@semantic-release/release-notes-generator`**: Generates release notes based on conventional commit messages.
   - **`@semantic-release/changelog`**: Updates the `CHANGELOG.md` file with the new release notes.
   - **`@semantic-release/github`**: Creates GitHub releases, including release notes and tags.
   - **`@semantic-release/git`**: Commits updated `CHANGELOG.md` and pushes it back to the repository.

4. **Release Rules**:
   - `feat`: Triggers a **minor** release (e.g., `1.1.0`).
   - `fix`: Triggers a **patch** release (e.g., `1.0.1`).
   - `chore` with scope `deps`: Triggers a **patch** release for dependency updates.
   - `docs`, `style`, `test`, and `ci`: Do not trigger a release.

5. **Changelog File**: The `CHANGELOG.md` file will be updated with each release.

6. **Git Commit for Release**: The updated files (e.g., `CHANGELOG.md`) are committed with a standardized message.

### Prerequisites
1. Ensure you have a `CONTRIBUTING.md` file describing the commit message format (`Conventional Commits`).
2. Install Semantic Release and the required plugins:
   ```bash
   npm install --save-dev semantic-release @semantic-release/commit-analyzer @semantic-release/release-notes-generator @semantic-release/changelog @semantic-release/github @semantic-release/git
   ```
3. Set up repository permissions for GitHub tokens to allow releases and changelog updates.

### CI/CD Integration
Integrate Semantic Release in your CI/CD pipeline by adding a step in your workflow (e.g., GitHub Actions, GitLab CI, or Azure DevOps) to run:
```bash
npx semantic-release
```
