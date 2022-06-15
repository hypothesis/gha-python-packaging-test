GitHub Actions Python Packaging Test Package
============================================

`gha-python-packaging-test` is a test and demonstration package for our
[GitHub Actions](https://docs.github.com/en/actions)-based Python package
release process.
It is actually [published to PyPI](https://pypi.org/project/gha-python-packaging-test/)
and you can actually install it:

```terminal
pipx install gha-python-packaging-test
gha_python_packaging_test --version
```

Here's how it works:

1. The [`pyproject.toml`](pyproject.toml) file includes [setuptools_scm](https://pypi.org/project/setuptools-scm/).
   This means that the version number comes from the git tags.
   There's no version number anywhere in the source tree
   (e.g. not in `pyproject.toml` nor in `setup.cfg`, nor in the Python code itself).
   This means that you don't need to make a git commit to update the version
   number before doing a release.
   All you need to do is add a new git tag, and GitHub does that for you when
   you create a GitHub release.

2. When you want to release a new version of a project you just
   create a new GitHub release using either GitHub's web UI
   or [GitHub CLI](https://cli.github.com/).
   See [Managing releases in a repository](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)
   in GitHub's docs.

3. This repo contains a [`pypi.yml`](.github/workflows/pypi.yml) workflow that runs when
   a new GitHub release is published and calls a
   [shared `pypi.yml` workflow in our `workflows` repo](https://github.com/hypothesis/workflows/blob/main/.github/workflows/pypi.yml)
   (see [Reusing workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) in the GitHub Actions docs).
   The shared workflow builds the Python package and uploads it to PyPI.
   Projects will be owned by the `eng.hypothes.is` user on PyPI.

4. In order for this to work the repo needs to have access to an
   organization-level GitHub secret named `PYPI_TOKEN`. If you're setting up a
   new repo get someone with admin access to our GitHub organization to add your
   new repo to the secret's scopes.
   See [Adding secrets for an organization](https://docs.github.com/en/codespaces/managing-codespaces-for-your-organization/managing-encrypted-secrets-for-your-repository-and-organization-for-codespaces#adding-secrets-for-an-organization) in GitHub's docs.

For more info see [How to Publish a Python Package from GitHub Actions](https://www.seanh.cc/2022/05/21/publishing-python-packages-from-github-actions/).
