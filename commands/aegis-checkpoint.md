# /aegis-checkpoint

Use this workflow after completing meaningful project work.

## Goal

Create a safe local recovery commit without leaking secrets or staging unrelated work.

## Steps

1. Check whether `.git/` exists.
2. If no git repo exists, report that checkpoint was skipped and do not create a repo.
3. Run `git status`.
4. Review changed files and identify which belong to the completed task.
5. Review relevant diffs.
6. Check for likely secrets and private data in changed/staged files.
7. Stage only intentional files.
8. Run `git diff --cached --stat`.
9. Run `git diff --cached` and inspect before committing.
10. Commit locally with a concise conventional commit message.
11. Do not push unless explicitly approved.

## Secret Keywords

Check for obvious sensitive terms:

```text
api_key
apikey
password
token
secret
smtp
imap
credential
private
.env
database_url
connection_string
```

Also check for project-specific private names, domains, emails, customer data, and lead data if known.

## Commit Message Examples

```text
docs: add aegis trail lite rules
chore: add agent checkpoint policy
fix: protect local credentials from git
```
