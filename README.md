# template-repo

Use this template to create new repositories in our organization. **After** creating the new repo, follow the steps below:

* [Create a personal access token in your GitHub account](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) and enable all `repo` scope permissions;
* Assign the access token value and the repository name to the following script and run it:

```
ACCESS_TOKEN=your_access_token
REPO=your_repo_name

echo "Defining repo configuration settings..."
curl -X PATCH \
     -H "Authorization: token $ACCESS_TOKEN" \
     -H "Accept: application/vnd.github.v3+json" \
     -d '{"has_wiki":false,"has_projects":false,"has_issues":false,"allow_squash_merge":true,"allow_merge_commit":false,"allow_rebase_merge":false,"delete_branch_on_merge":true,"auto_merge":false}' \
     https://api.github.com/repos/cyralinc/${REPO}

echo "Defining protection rules for 'main' branch..."
curl -X PUT \
     -H "Authorization: token $ACCESS_TOKEN" \
     -H "Accept: application/vnd.github.luke-cage-preview+json" \
     -d '{"required_status_checks":null,"enforce_admins":true,"required_pull_request_reviews":{"required_approving_review_count":1},"restrictions":null}' \
     https://api.github.com/repos/cyralinc/${REPO}/branches/main/protection
```

* Update this file and define a `README.md` that suits your new repository.
