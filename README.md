# GitRepositoryAutomation
The purpose of this project is to provide basic CI/CD automation for git based projects.

## Intended usage and setup

### Linux setup (Tested on Ubuntu 16.04)
Add an /etc/crontab entry to automatically run the check_update_branch script every minute:

`* * * * * root cd <git repository directory> && ./check_update_branch <name of branch to be checked and updated>`

<b>Note:</b> To allow crontab to successfully execute the script, add your github username and password (or ssh keys) to your repository, 
so that `git pull` executes without prompting for username and password.

### Example usage
You have a development release ecosystem consisting of 3 stages:

1. Stage 1: Development environments that commit changes on their own branches and merge them to a single staging branch
2. Stage 2: Staging/testing environments where manual and automatic testing is conducted.
3. Stage 3: Production environments where tested and stable code ends up once approved by stage 2.

Each stage has its own server environment (local repository).
Consider a single developer ecosystem with git branches being as follows:

1. Stage 1: `dev`
2. Stage 2: `staging`
3. Stage 3: `live`

When commits from `dev` get merged to `staging` via a PR, the execution of `./check_update_branch staging` on 
**staging** environment, through the crontab we have setup, will automatically pull remote changes to our local repository, 
allowing us to proceed directly with testing these changes, without having to manually handle server deployment.

When we have tested the changes on staging environment and we merge them through a PR to our `live` git branch,
a similar setup on our live environment will pull our changes for us.