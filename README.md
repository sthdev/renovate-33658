# minimal-reproduction-template

First, read the [Renovate minimal reproduction instructions](https://github.com/renovatebot/renovate/blob/main/docs/development/minimal-reproductions.md).

Then replace the current `h1` with the Renovate Issue/Discussion number.

## Current behavior

If a PR created by Renovate for an update of a __Copier__ template dependency is extended using the _git fix-up_ method (see e.g. https://mikulskibartosz.name/git-fixup-explained) and Renovate runs again, it does not detect that the PR contains manual additions.
Instead, Renovate rebases the PR and discards all manual additions.

The file [templated-file.md](./templated-file.md) is generated from the Copier template at https://github.com/sthdev/renovate-33658-copier-template.
In PR https://github.com/sthdev/renovate-33658/pull/4, the dependency to the template is updated from 1.0.1 to 1.1.0.
Version 1.1.0 changes the templated file.
For some arbitrary reason, this makes manual changes to this file necessary (e.g. because repository-specific data has to be inserted there), which are appended to RenovateBot's commit using the fix-up method.

The problem seems to only happen in case of Copier template dependencies.
The [Dockerfile](./Dockerfile) contains a dependency to the Ubuntu base image.
The PR https://github.com/sthdev/renovate-33658/pull/3 also contains manual additions but these are preserved.

---

### git fix-up tl;dr:

- checkout the branch created by renovate
- make manual changes
- commit them
- run `git rebase -i HEAD~2`
- a text editor opens, replace `pick` with `fixup` in the line that corresponds to the manual commit
- run `git push --force`

---

## Expected behavior

RenovateBot should detect that the PR to update the Copier template has manual additions and should not modify it.

## Link to the Renovate issue or Discussion

https://github.com/renovatebot/renovate/discussions/33658
