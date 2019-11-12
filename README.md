# CI: Add build information to Pull Request
[![Build Status](https://travis-ci.org/levonet/ansible-ci-github-pr-description.svg?branch=master)](https://travis-ci.org/levonet/ansible-ci-github-pr-description)

Add build information to Pull Request description.

## Role Variables

- `ci_github_api` (default: https://api.github.com)
- `ci_github_username` (required): Github username.
- `ci_github_password` (required): [Github access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).
- `ci_github_owner` (required): Github owner or organisation name.
- `ci_github_repo` (required): Github project repository name.
- `ci_github_pr_number` (required): Github Pull Request number.
- `ci_github_branch` (optional): Github branch name.
- `ci_github_jira_task_url` (optional): Github repository url.
- `ci_github_jira_task_filter` (optional): Regexp filter for Jira task from github branch.  
  For example: `(TODO|BUGS)-\d+`.
- `ci_github_message_body` (required): Some text messages with CI information.

## Example Playbook

```yaml
- hosts: 127.0.0.1
  connection: local
  gather_facts: no
  vars:
    ci_github_username: ci-bot
    ci_github_password: secret
    ci_github_branch: "{{ github_branch }}"
    ci_github_pr_number: "{{ github_pr_number }}"
    ci_github_owner: myorg
    ci_github_repo: myapp
    ci_github_jira_task_filter: (MYAPPAPI|MYAPPDB|BUGS)-\d+
    ci_github_jira_task_url: https://myorg.atlassian.net/browse/
    ci_github_message_body: |
      * App: [pr-{{ ci_github_pr_number }}.myapp.myorg.com|http://pr-{{ ci_github_pr_number }}.myapp.myorg.com]
      * Logs: [myapp-PR-{{ ci_github_pr_number }}|http://grafana.myorg.com/d/XxXxXx/logs?var-host=sandbox1&var-app=myapp-PR-{{ ci_github_pr_number }}]
      * Jenkins: [PR-{{ ci_github_pr_number }}|http://jenkins.myorg.com/job/myapp/view/change-requests/job/PR-{{ ci_github_pr_number }}/]
  roles:
    - role: levonet.ci_github_pr_description
```

And run in Jenkins:

```bash
ansible-playbook myplaybook.yml -e github_branch="${CHANGE_BRANCH}" -e github_pr_number="${CHANGE_ID}"
```

As a result, you will receive a description in Pull Request:

> Old description information if exists
> * Jira: [TODO-40](#)
> * App: [pr-115.myapp.myorg.com](#)
> * Logs: [myapp-PR-115](#)
> * Jenkins: [PR-115](#)

## License

[MIT](https://opensource.org/licenses/MIT)

## Author Information

This role was created by [Pavlo Bashynskyi](https://github.com/levonet)
