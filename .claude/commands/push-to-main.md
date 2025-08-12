# Push to Main with Documentation

Document changes and push directly to main branch (use with caution).

Usage: `/project:push-to-main [commit-message]`

## Steps:

1. **Check current status**
   - Ensure we're on main branch
   - Show current changes to be committed

2. **Generate commit message**
   - Use provided message or auto-generate based on changes
   - Follow conventional commit format

3. **Commit and push**
   - Add all changes
   - Commit with descriptive message
   - Push to main branch

## Implementation:

```bash
# Get commit message argument
COMMIT_MSG="$*"

# Ensure we're on main branch
CURRENT_BRANCH=$(git branch --show-current)
if [ "$CURRENT_BRANCH" != "main" ] && [ "$CURRENT_BRANCH" != "master" ]; then
  echo "⚠️  Warning: Not on main branch (currently on: $CURRENT_BRANCH)"
  echo "Switching to main branch..."
  git checkout main || git checkout master
fi

# Show current status
echo "📋 Current changes:"
git status --porcelain

# Check if there are changes to commit
if [ -z "$(git status --porcelain)" ]; then
  echo "✅ No changes to commit"
  exit 0
fi

# Auto-generate commit message if not provided
if [ -z "$COMMIT_MSG" ]; then
  # Count different types of changes
  ADDED=$(git status --porcelain | grep -c "^A" || echo 0)
  MODIFIED=$(git status --porcelain | grep -c "^M" || echo 0)
  DELETED=$(git status --porcelain | grep -c "^D" || echo 0)
  UNTRACKED=$(git status --porcelain | grep -c "^??" || echo 0)
  
  # Generate message based on changes
  if [ "$ADDED" -gt 0 ] && [ "$MODIFIED" -eq 0 ] && [ "$DELETED" -eq 0 ]; then
    COMMIT_MSG="docs: add new content and documentation"
  elif [ "$MODIFIED" -gt 0 ] && [ "$ADDED" -eq 0 ] && [ "$DELETED" -eq 0 ]; then
    COMMIT_MSG="docs: update existing documentation"
  elif [ "$DELETED" -gt 0 ]; then
    COMMIT_MSG="docs: remove outdated content"
  else
    COMMIT_MSG="docs: update blog content and documentation"
  fi
fi

# Show what will be committed
echo ""
echo "📝 Will commit with message: '$COMMIT_MSG'"
echo ""
echo "📦 Files to be committed:"
git diff --name-only --cached 2>/dev/null || git ls-files --others --exclude-standard

# Add all changes
echo ""
echo "📁 Adding all changes..."
git add .

# Commit changes
echo "💾 Committing changes..."
git commit -m "$COMMIT_MSG

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>"

# Push to main
echo "🚀 Pushing to main..."
git push origin main || git push origin master

echo ""
echo "✅ Successfully pushed changes to main branch!"
echo "📊 Repository status:"
git log --oneline -3
```

**⚠️ Warning**: This command pushes directly to main. Use only for:
- Documentation repositories
- Personal projects
- Non-collaborative workflows

For team projects, use `/project:create-pr-branch` instead.