# Upgrade Plan: 14.7.0 -> 15.6.7
This document serves as a guide to create a strong plan to upgrade a self-managed GitLab instance

## Phase 1:

```
14.7.0 to 14.9.5
```
## Pre-Upgrade Checks and Tasks
```
1.Review the version-specific upgrade notes for 14.9.0
2.Run the post-upgrade checks in this doc
3.Ensure that gitlab-ctl reconfigure exits cleanly
4.Perform a backup gitlab-backup create
5.Copy contents of current gitlab.rb and /etc/gitlab/gitlab-secrets.json in safe place 
6.Verify the contents of the backup file
7. Database Migreation gitlab-rake db:migrate:status
```

## Upgrade Plan
```
Turn on  maintenance mode: https://docs.gitlab.com/ee/administration/maintenance_mode/

::Gitlab::CurrentSettings.update!(maintenance_mode: true)
::Gitlab::CurrentSettings.update!(maintenance_mode_message: "GitLab is being upgraded.")

Install the newer version of GitLab: apt-get install  gitlab-ee=14.9.5-ee.0
Observe that the command above exits successfully or address any errors that come up

```
## Verification and Post-Upgrade Checks and Tasks
The root user can log in
Turn Maintenance Mode off

Other users can log in
  Ensure users can log in with local passwords
  Ensure users can log in via LDAP
Projects can be cloned
Commits can be pushed

The output of gitlab-rake gitlab:check looks good
The output of gitlab-rake gitlab:env:info looks good
Hand off to developers for further testing
Deploy new GitLab Runners, test, decommission old Runners

::Gitlab::CurrentSettings.update!(maintenance_mode: false)

## Rollback Plan
```
The rollback plan involves:
    restoring the gitlab.rb and gitlab-secrets.json, other useful contents in /etc and GitLab
    application backup that was taken and transferred
    verifying the functionality by performing the Pre-Upgrade Checks and Tasks
    updating the DNS record for gitlab.brie.dev to point to the new instance once the new GitLab server is up and running.
```
## Phase 2:
```
14.9.5 to 14.10.5
```
## Phase 3:
```
10.10.5 to 15.0.5
```

## Phase 4:
```
15.0.5 to 15.4.6
```

## Phase 5:
```
15.4.6 to 15.6.7
```




