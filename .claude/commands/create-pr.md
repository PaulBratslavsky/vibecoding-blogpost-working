# Create Pull Request

Create a pull request from the current branch with proper formatting.

Usage: `/project:create-pr <title> [description]`

## Steps:

1. **Check current branch status**
   - Ensure we're not on main/master
   - Check if there are commits to PR
   - Verify branch is pushed to remote

2. **Create pull request**
   - Use GitHub CLI to create PR
   - Include proper title and description
   - Add common PR template elements

3. **Provide PR URL**
   - Return the created PR URL for easy access

## Implementation:

```bash
# Get arguments
TITLE="$1"
DESCRIPTION="$2"

# Validate inputs
if [ -z "$TITLE" ]; then
  echo "Error: PR title is required"
  echo "Usage: /project:create-pr \"PR Title\" \"[Optional description]\""
  exit 1
fi

# Check current branch
CURRENT_BRANCH=$(git branch --show-current)
if [ "$CURRENT_BRANCH" = "main" ] || [ "$CURRENT_BRANCH" = "master" ]; then
  echo "Error: Cannot create PR from main/master branch"
  echo "Use /project:create-pr-branch first to create a feature branch"
  exit 1
fi

# Check if branch has commits
COMMITS=$(git rev-list --count HEAD ^main 2>/dev/null || git rev-list --count HEAD ^master 2>/dev/null || echo "0")
if [ "$COMMITS" = "0" ]; then
  echo "Error: No commits found on current branch"
  echo "Make some changes and commit them first"
  exit 1
fi

# Ensure branch is pushed
echo "Ensuring branch is pushed to remote..."
git push -u origin "$CURRENT_BRANCH"

# Create default description if not provided
if [ -z "$DESCRIPTION" ]; then
  DESCRIPTION="## Summary
<!-- Brief description of changes -->

## Changes
- <!-- List key changes -->

## Testing
- [ ] Manual testing completed
- [ ] No breaking changes

🤖 Generated with [Claude Code](https://claude.ai/code)"
fi

# Create the PR
echo "Creating pull request..."
gh pr create --title "$TITLE" --body "$DESCRIPTION"

echo ""
echo "✅ Pull request created successfully!"
echo "View your PR: gh pr view --web"
```

This command will:
- Validate you're on a feature branch with commits
- Push any unpushed changes
- Create a properly formatted PR with GitHub CLI
- Provide the PR URL for immediate access