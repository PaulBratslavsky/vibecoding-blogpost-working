# Create PR Branch

Create a new branch, push it to origin, and prepare for a pull request.

Usage: `/project:create-pr-branch <branch-name> [base-branch]`

## Steps:

1. **Create and switch to new branch**
   - Create branch from current branch (or specified base branch)
   - Switch to the new branch

2. **Push branch to remote**
   - Push the new branch to origin with upstream tracking
   - Set up remote tracking for future pushes

3. **Prepare for PR creation**
   - Show branch status
   - Provide GitHub CLI command to create PR when ready

## Implementation:

```bash
# Get the branch name from arguments
BRANCH_NAME="$ARGUMENTS"
BASE_BRANCH="${2:-$(git branch --show-current)}"

# Validate branch name
if [ -z "$BRANCH_NAME" ]; then
  echo "Error: Branch name is required"
  echo "Usage: /project:create-pr-branch <branch-name> [base-branch]"
  exit 1
fi

# Create and switch to new branch
echo "Creating branch '$BRANCH_NAME' from '$BASE_BRANCH'..."
git checkout -b "$BRANCH_NAME"

# Push branch to remote with upstream tracking
echo "Pushing branch to origin..."
git push -u origin "$BRANCH_NAME"

# Show status
echo "✅ Branch '$BRANCH_NAME' created and pushed to origin"
echo ""
echo "Next steps:"
echo "1. Make your changes and commit them"
echo "2. Create a PR with: gh pr create --title \"Your PR Title\" --body \"PR description\""
echo "3. Or use: /project:create-pr \"PR Title\" \"PR description\""
```

This command will:
- Create a new branch from the current branch (or specified base)
- Push it to origin with upstream tracking
- Provide next steps for creating the actual PR