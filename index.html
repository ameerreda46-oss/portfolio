#!/usr/bin/env bash
set -Eeuo pipefail

# Upload this portfolio repository to a GitHub repository without changing origin.
# Usage: bash upload-to-github.sh

fail() {
  printf 'Error: %s\n' "$1" >&2
  exit 1
}

repo_root="$(cd -- "$(dirname -- "${BASH_SOURCE[0]}")" && pwd)"
cd "$repo_root"

if ! git rev-parse --is-inside-work-tree >/dev/null 2>&1; then
  printf 'This folder does not have Git history yet. Initializing a local repository on main.\n'
  git init --initial-branch=main
fi

read -r -p 'Paste your GitHub repository URL (HTTPS or SSH): ' github_url
github_url="${github_url%/}"

if [[ ! "$github_url" =~ ^https://github\.com/[A-Za-z0-9_.-]+/[A-Za-z0-9_.-]+(\.git)?$ && ! "$github_url" =~ ^git@github\.com:[A-Za-z0-9_.-]+/[A-Za-z0-9_.-]+(\.git)?$ ]]; then
  fail 'Use a GitHub clone URL such as https://github.com/USERNAME/REPOSITORY.git or git@github.com:USERNAME/REPOSITORY.git.'
fi

# Preserve origin and refuse to silently retarget a different existing GitHub remote.
if git remote get-url github >/dev/null 2>&1; then
  configured_url="$(git remote get-url github)"
  [[ "$configured_url" == "$github_url" ]] || fail "A different 'github' remote already exists: $configured_url. Change it deliberately with git remote set-url github YOUR_REPOSITORY_URL, then rerun."
else
  git remote add github "$github_url"
fi

# Refuse to upload obvious environment files or private keys already tracked by Git.
sensitive_tracked="$(git ls-files | grep -E '(^|/)(\.env($|\.)|id_(rsa|ed25519)$|[^/]+\.(pem|p12|key)$)' | grep -vE '(^|/)\.env\.example$' || true)"
[[ -z "$sensitive_tracked" ]] || {
  printf 'Stopped: these sensitive-looking files are already tracked by Git:\n%s\n' "$sensitive_tracked" >&2
  printf 'Remove them from Git and rotate any exposed credentials before continuing.\n' >&2
  exit 1
}

if [[ -n "$(git status --porcelain)" ]]; then
  printf '\nThere are uncommitted or untracked changes:\n'
  git status --short
  read -r -p 'Stage and commit these changes before uploading? [y/N] ' confirm_commit
  [[ "$confirm_commit" =~ ^[Yy]$ ]] || fail 'No changes were committed or uploaded.'

  git add --all

  sensitive_staged="$(git diff --cached --name-only --diff-filter=ACMRT | grep -E '(^|/)(\.env($|\.)|id_(rsa|ed25519)$|[^/]+\.(pem|p12|key)$)' | grep -vE '(^|/)\.env\.example$' || true)"
  [[ -z "$sensitive_staged" ]] || {
    printf 'Stopped: these sensitive-looking files are staged and will not be uploaded:\n%s\n' "$sensitive_staged" >&2
    printf 'Review the staged files, unstage/remove secrets, then rerun the script.\n' >&2
    exit 1
  }

  if ! git diff --cached --quiet; then
    read -r -p 'Commit message [Update portfolio]: ' commit_message
    commit_message="${commit_message:-Update portfolio}"
    git commit -m "$commit_message"
  fi
fi

if ! git rev-parse --verify HEAD >/dev/null 2>&1; then
  fail 'There is no commit to upload. Add and commit the portfolio files, then rerun.'
fi

commit="$(git rev-parse --short HEAD)"
printf '\nReady to push commit %s to GitHub branch main.\nRemote: %s\n' "$commit" "$github_url"
printf 'The existing origin remote will not be changed.\n'
read -r -p 'Continue? [y/N] ' confirm_push
[[ "$confirm_push" =~ ^[Yy]$ ]] || fail 'Upload cancelled; nothing was pushed.'

git push --set-upstream github HEAD:main
printf '\nUpload complete. GitHub repository: %s\n' "$github_url"
