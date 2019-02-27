# Jira Transition
Transition Jira issue

For examples on how to use this, check out the [gajira-demo](https://github.com/atlassian/gajira-demo) repository
> ##### Only supports Jira Cloud. Does not support Jira Server (hosted)

## Usage

> ##### Note: this action requires [Jira Login Action](https://github.com/marketplace/actions/jira-login)

![Issue Transition](../assets/example.gif?raw=true)

Example transition action:

    action "Jira Transition" {
        uses = "atlassian/gajira-transition@master"
        needs = ["Jira Login"]
        args = "deployed to production --issue=GA-181"
    }

You can omit `--issue` parameter if preceding action is [`Create`](https://github.com/marketplace/actions/jira-create) or [`Find Issue Key`](https://github.com/marketplace/actions/jira-find) and just specify a transition name in action args. Here is full example workflow:

    workflow "Transition issue" {
        on = "push"
        resolves = ["Jira Login"]
    }

    action "Jira Login" {
        uses = "atlassian/gajira-login@v1.0.0"
        secrets = ["JIRA_API_TOKEN", "JIRA_USER_EMAIL", "JIRA_BASE_URL"]
    }

    action "Jira Find Issue Key" {
        uses = "atlassian/gajira-find-issue-key@v1.0.0"
        needs = ["Jira Login"]
        args = "--from=branch"
    }
    
    action "Jira Transition" {
        uses = "atlassian/gajira-transition@v1.0.0"
        needs = ["Jira Find Issue Key"]
        args = "deployed to production"
    }

----
## Action Spec:

### Environment variables
- None

### Arguments
- `<transition name>` - Case insensetive name of transition to apply. Example: `Cancel` or `Accept`
- `--issue=<KEY-NUMBER>` - issue key to perform a transition on
- `--id=<transition id>` - transition id to apply to an issue

### Reads fields from config file at $HOME/jira/config.yml
- `issue`
- `transitionId`

### Writes fields to config file at $HOME/jira/config.yml
- None