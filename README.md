# Changelog Generator Action

Cyral automated change log generator tool published in GitHub Marketplace.

## Usage
The tool requires docker to login to our private container registry, thus make
sure your login credentials have permissions to access it.

The downloaded image will then fetch all the git history for the given
repository, perform the comparison from the informed tag and the previous one and
finally create a `CHANGELOG.md` file and associate it with the git release.

```yaml
steps:
- uses: actions/checkout@v2
  with:
    fetch-depth: 0 # Downloads the whole commit history

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
