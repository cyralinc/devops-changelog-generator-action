# Changelog Generator Action

Cyral automated change log generator tool published in GitHub Marketplace.

## Usage
Such action depends on a docker login to Google Cloud Registry.
So, make sure your login credentials have permissions to access `gcr.io/cyral-dev`.
Also, the target repository must fetch all history for tags.
A `CHANGELOG.md` file will be generated after execute the generator.

```yaml
steps:
- uses: actions/checkout@v2
  with:
    fetch-depth: 0

- uses: docker/login-action@v1
  with:
    registry: gcr.io
    username: _json_key
    password: ${{ secrets.GCR_JSON_TOKEN }}
  
- uses: cyralinc/devops-changelog-generator-action@v1
  name: Generate changelog
  with:
    # The target tag which the changelog will be generated.
    # REQUIRED
    tag: ${{ github.ref_name }}

    # Repository owner and name in the format <owner>/<name>
    # REQUIRED
    repository: ${{ github.repository }}

    # Jira ticket prefixes, separated by comma
    # REQUIRED
    jira-prefixes: FOO,BAR

    # Github's Personal Access Token with access to clone all dependency
    # repositories. It is required only if the target repository has
    # dependencies to be listed in the changelog.
    # NOT REQUIRED
    credentials: ${{secrets.PERSONAL_ACCESS_TOKEN_TO_CLONE_DEPENDENCY_REPOS }}

    # The JSON configuration for the dependency repositories.
    # It is required only if the target repository has dependencies to be
    # listed in the changelog.
    # Ensure the JSON uses double quotes insted of single quotes.
    # NOT REQUIRED
    dependencies-config: ${{secrets.JSON_REPOSITORIES_CONFIG}}
```
