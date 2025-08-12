# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a blog content repository containing documentation about building applications with V0 and Claude Code. The project focuses on "vibe coding" - using AI tools for rapid prototyping and development.

## Repository Structure

- `blog-draft.md` - Main blog post about building with V0 and Claude Code
- `full-prompt.txt` - Detailed prompt example for V0 frontend generation
- `mock.json` - Sample Strapi API response data for testing
- `other-prompt.txt` - Additional prompt examples
- `images/` - Screenshots and visual documentation

## Key Concepts

### Content Architecture
- **Strapi Integration**: Uses Strapi as a headless CMS with block-based content modeling
- **Block Components**: Dynamic content rendered via `__component` discriminators (e.g., `section.hero`, `section.features`)
- **API Structure**: Strapi API responses with nested blocks and metadata

### Development Workflow
1. **V0 Prototyping**: Use Vercel V0 for rapid UI generation with structured prompts
2. **Data-Driven Development**: Start with actual API responses to guide TypeScript types
3. **Claude Code Integration**: Move from prototype to local development environment

## Best Practices Referenced

### V0 Prompt Engineering
- Define clear role and tech stack
- Provide concrete data structures
- List specific requirements and constraints
- Break complex builds into focused steps
- Include real API responses for accurate type inference

### Claude Code Workflow
- Use CLAUDE.md for project documentation
- Implement Explore → Plan → Code → Commit pattern
- Leverage slash commands for reusable workflows
- Maintain context hygiene with `/clear` between tasks

## Architecture Patterns

### Block-Based Content System
```typescript
interface Block {
  __component: string;
  id: number;
  // Component-specific properties
}
```

### Component Mapping
- Use component maps or switch statements for block rendering
- Each block type maps to a dedicated React component
- Maintain type safety with discriminated unions

## Common Commands

Since this is a documentation repository without a build system, common operations include:

- `ls` - List repository contents
- `cat <file>` - Read file contents
- `grep -r <term> .` - Search across files

## Custom Slash Commands

### Branch and PR Management

- `/project:create-pr-branch <branch-name> [base-branch]` - Create new branch and push to origin
- `/project:create-pr <title> [description]` - Create pull request from current branch
- `/project:push-to-main [commit-message]` - Document changes and push directly to main

Example workflows:
```bash
# Feature branch workflow (recommended for teams)
/project:create-pr-branch feature/new-content
# Make changes and commit
/project:create-pr "Add new blog content" "Added section about best practices"

# Direct to main workflow (personal/docs repos only)
/project:push-to-main "Update documentation with new examples"
```

## Development Notes

- Focus on clean TypeScript structure and type safety
- Prioritize functional logic over visual design
- Use Next.js App Router patterns
- Implement proper error handling and loading states
- Always validate API responses before using in production