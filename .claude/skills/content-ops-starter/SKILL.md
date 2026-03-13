# content-ops-starter Development Patterns

> Auto-generated skill from repository analysis

## Overview

This skill covers development patterns for the content-ops-starter repository, a TypeScript-based Next.js application focused on content operations. The codebase emphasizes automated dependency management through Renovate bot, with frequent updates to maintain security and compatibility. The project follows modern TypeScript/React development practices with a strong emphasis on automated maintenance workflows.

## Coding Conventions

### File Naming
- Use **kebab-case** for all file names
- Examples: `content-manager.ts`, `api-client.tsx`, `user-profile.component.ts`

### Import Style
```typescript
// Mixed import patterns - use what's most appropriate:
import React from 'react'
import { useState, useEffect } from 'react'
import * as utils from '../utils'
import type { UserProfile } from '../types'
```

### Export Style
- Prefer **named exports** over default exports
```typescript
// ✅ Preferred
export const ContentManager = () => { ... }
export const useContentHook = () => { ... }
export type ContentConfig = { ... }

// ❌ Avoid default exports
export default ContentManager
```

### Commit Messages
- **Freeform style** with average 45 character length
- No strict prefixes required
- Focus on clarity and conciseness
- Examples: "Update deps", "Fix auth flow", "Add content validation"

## Workflows

### Dependency Update (Renovate)
**Trigger:** When Renovate bot detects available package updates
**Command:** `/update-deps`

1. Renovate detects outdated dependency in package.json
2. Creates automated PR with version bump details
3. Updates package-lock.json with resolved dependency tree
4. For major dependencies, also updates package.json
5. Review PR for breaking changes before merging
6. Merge once CI passes and changes are verified

```json
// Typical package.json change
{
  "dependencies": {
-   "next": "13.0.0",
+   "next": "13.1.0"
  }
}
```

### Security Update Workflow
**Trigger:** When security vulnerabilities are detected in dependencies
**Command:** `/security-update`

1. Security scanner identifies vulnerability in dependency
2. Renovate creates high-priority PR with [SECURITY] tag in title
3. Updates both package.json and package-lock.json simultaneously
4. **Immediate review and deployment recommended**
5. Verify security patch doesn't break functionality
6. Deploy to production as soon as possible

### Lock-Only Dependency Update
**Trigger:** When minor/patch versions of dependencies are available
**Command:** `/update-lock`

1. Renovate detects compatible version update (patch/minor)
2. Updates only package-lock.json (no package.json changes)
3. No breaking changes expected due to semver compatibility
4. Safe to auto-merge if CI passes
5. Monitor for any runtime issues post-deployment

### Next.js Framework Update
**Trigger:** When new Next.js versions are released
**Command:** `/update-nextjs`

1. Next.js version bump detected by Renovate
2. Updates package.json with new Next.js version
3. Updates package-lock.json with all related dependencies
4. May include security patches and performance improvements
5. **Test thoroughly** - framework updates can have breaking changes
6. Check Next.js migration guides for version-specific changes

```bash
# Verify Next.js update locally
npm run build
npm run start
# Test key application routes
```

### React Ecosystem Update
**Trigger:** When React ecosystem packages have coordinated releases
**Command:** `/update-react`

1. React monorepo release detected (React, ReactDOM, types)
2. Updates React core packages in lockfile
3. Updates corresponding TypeScript definitions (@types/react)
4. Ensures version compatibility across entire React ecosystem
5. Test components for any behavioral changes
6. Pay attention to TypeScript errors that may surface

## Testing Patterns

### Test File Structure
- Test files follow pattern: `*.test.*`
- Place tests adjacent to source files or in dedicated test directories

```typescript
// Example test structure
src/
  components/
    content-manager.tsx
    content-manager.test.tsx
  utils/
    api-client.ts
    api-client.test.ts
```

### Testing Framework
- Framework not specified in analysis - likely Jest with React Testing Library
- Focus on component behavior and user interactions
- Test dependency updates by running full test suite

## Commands

| Command | Purpose |
|---------|---------|
| `/update-deps` | Handle general dependency updates from Renovate |
| `/security-update` | Process critical security dependency updates |
| `/update-lock` | Manage lock-file-only dependency updates |
| `/update-nextjs` | Handle Next.js framework version updates |
| `/update-react` | Process React ecosystem package updates |

## Best Practices

1. **Monitor Renovate PRs** - Review ~20-30 dependency updates monthly
2. **Prioritize Security** - Handle [SECURITY] tagged PRs immediately  
3. **Test Framework Updates** - Next.js and React updates need thorough testing
4. **Maintain Compatibility** - Ensure TypeScript definitions stay in sync
5. **Automate Where Safe** - Lock-only updates can often be auto-merged
6. **Document Breaking Changes** - Update team on major version bumps