# Contributing to PushUp

Thank you for your interest in contributing to PushUp! This document provides comprehensive guidelines for contributing to the project. We value all contributions, from bug reports to feature implementations.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Naming Conventions](#naming-conventions)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)
- [Issue Reporting](#issue-reporting)
- [Feature Requests](#feature-requests)
- [API Integration Details](#api-integration-details)
- [Architecture Guidelines](#architecture-guidelines)

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment for all contributors regardless of:
- Experience level
- Gender identity and expression
- Sexual orientation
- Disability
- Personal appearance
- Body size
- Race
- Ethnicity
- Age
- Religion
- Nationality

### Expected Behavior

- Use welcoming and inclusive language
- Be respectful of differing viewpoints and experiences
- Gracefully accept constructive criticism
- Focus on what is best for the community
- Show empathy towards other community members
- Provide helpful and constructive feedback

### Unacceptable Behavior

- Trolling, insulting or derogatory comments
- Personal or political attacks
- Public or private harassment
- Publishing others' private information without permission
- Other conduct which could reasonably be considered inappropriate

## Getting Started

### Prerequisites

Before contributing, ensure you have the following installed:

1. Node.js (version 16 or higher)
```bash
node --version
```

2. npm or yarn package manager
```bash
npm --version
```

3. Git for version control
```bash
git --version
```

4. A code editor (VS Code recommended)

5. Chrome or Chromium-based browser for testing

### Fork and Clone

1. Fork the repository on GitHub by clicking the "Fork" button

2. Clone your forked repository:
```bash
git clone https://github.com/YOUR_USERNAME/PushUp.git
cd PushUp
```

3. Add the upstream remote to stay updated:
```bash
git remote add upstream https://github.com/revanthm1902/PushUp.git
```

4. Verify remotes:
```bash
git remote -v
```

Expected output:
```
origin    https://github.com/YOUR_USERNAME/PushUp.git (fetch)
origin    https://github.com/YOUR_USERNAME/PushUp.git (push)
upstream  https://github.com/revanthm1902/PushUp.git (fetch)
upstream  https://github.com/revanthm1902/PushUp.git (push)
```

### Environment Setup

1. Install backend dependencies:
```bash
cd backend
npm install
```

2. Install frontend dependencies:
```bash
cd ../frontend
npm install
```

3. Create backend environment file:
```bash
cd ../backend
cp .env.example .env
```

4. Configure your `.env` file with required credentials:
```env
GITHUB_CLIENT_ID=your_github_oauth_app_client_id
GITHUB_CLIENT_SECRET=your_github_oauth_app_secret
REDIRECT_URI=http://localhost:3000/auth/github/callback
FRONTEND_REDIRECT=http://localhost:5173/
EXTENSION_ID=your_extension_id_after_loading
GITHUB_OAUTH_SCOPE=repo,user
```

### Running Development Servers

1. Start backend server:
```bash
cd backend
npm run dev
```

Backend runs on: `http://localhost:3000`

2. In a new terminal, start frontend development server:
```bash
cd frontend
npm run dev
```

Frontend dev server runs on: `http://localhost:5173`

3. Load the extension in Chrome:
   - Navigate to `chrome://extensions/`
   - Enable "Developer mode"
   - Click "Load unpacked"
   - Select the `frontend/dist` folder (after building with `npm run build`)

## Development Workflow

### Branch Strategy

We follow a feature branch workflow:

1. Always work on a new branch for your changes
2. Branch from `main` for new features
3. Use descriptive branch names

**Branch Naming Convention**:
```
feature/add-hackerrank-support
bugfix/fix-leetcode-detection
docs/update-api-documentation
refactor/improve-github-service
test/add-streak-calculation-tests
```

### Creating a Feature Branch

1. Ensure your local `main` is up to date:
```bash
git checkout main
git pull upstream main
```

2. Create and switch to a new branch:
```bash
git checkout -b feature/your-feature-name
```

3. Make your changes and commit regularly:
```bash
git add .
git commit -m "feat: add initial support for HackerRank detection"
```

4. Push your branch to your fork:
```bash
git push origin feature/your-feature-name
```

### Keeping Your Branch Updated

Regularly sync your branch with upstream:

```bash
git fetch upstream
git rebase upstream/main
```

If conflicts occur, resolve them and continue:
```bash
# Fix conflicts in your editor
git add .
git rebase --continue
```

## Pull Request Process

### Before Submitting

1. Ensure all tests pass (if applicable)
2. Update documentation for any changed functionality
3. Follow the coding standards outlined below
4. Verify your changes work in the browser extension
5. Check that your code doesn't introduce console errors

### Creating a Pull Request

1. Push your feature branch to your fork:
```bash
git push origin feature/your-feature-name
```

2. Go to the original repository on GitHub

3. Click "New Pull Request"

4. Select your fork and branch

5. Fill out the pull request template:

```markdown
## Description
Brief description of what this PR does.

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Related Issue
Fixes #(issue number)

## Changes Made
- Added X functionality to Y component
- Fixed Z bug in A module
- Updated documentation for B

## Testing Done
- [ ] Tested on LeetCode platform
- [ ] Tested on Codeforces platform
- [ ] Tested on CodeChef platform
- [ ] Verified GitHub integration works
- [ ] Checked extension popup displays correctly

## Screenshots (if applicable)
Add screenshots showing the changes.

## Checklist
- [ ] My code follows the project's coding standards
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have updated the documentation accordingly
- [ ] My changes generate no new warnings or errors
- [ ] I have tested my changes thoroughly
```

### Pull Request Title Format

Use conventional commit format in PR titles:

```
feat: add support for HackerRank platform
fix: resolve LeetCode code extraction issue
docs: update API integration documentation
refactor: improve GitHub service error handling
test: add unit tests for streak calculation
```

### Review Process

1. At least one maintainer will review your PR
2. Address any feedback or requested changes
3. Once approved, a maintainer will merge your PR
4. Your contribution will be credited in the release notes

### Making Changes After Feedback

1. Make the requested changes in your local branch
2. Commit the changes:
```bash
git add .
git commit -m "fix: address review feedback"
```

3. Push to your fork:
```bash
git push origin feature/your-feature-name
```

The PR will automatically update with your new commits.

## Coding Standards

### TypeScript Guidelines

**Type Safety**:
- Always use explicit types for function parameters and return values
- Avoid using `any` type unless absolutely necessary
- Use `unknown` instead of `any` when type is uncertain
- Prefer `interface` over `type` for object definitions

**Good Example**:
```typescript
interface SubmissionData {
  platform: string;
  problemId: string;
  code: string;
  language: string;
}

function processSubmission(data: SubmissionData): Promise<boolean> {
  // Implementation
  return Promise.resolve(true);
}
```

**Bad Example**:
```typescript
function processSubmission(data: any) {
  // Avoid this - use proper typing
  return true;
}
```

### React Component Guidelines

**Functional Components**:
Always use functional components with hooks, not class components.

```typescript
// Good
import { useState, useEffect } from 'react';

export default function SubmissionCalendar() {
  const [data, setData] = useState<DailySubmissionData[]>([]);
  
  useEffect(() => {
    // Load data
  }, []);
  
  return <div>Calendar</div>;
}

// Avoid class components
class SubmissionCalendar extends React.Component {
  // Don't use this pattern
}
```

**Props Definition**:
```typescript
interface CalendarProps {
  dailySubmissions: DailySubmissionData[];
  onDateClick?: (date: string) => void;
}

export default function Calendar({ dailySubmissions, onDateClick }: CalendarProps) {
  // Component implementation
}
```

### Error Handling

**Custom Error Classes**:
```typescript
export class GitHubError extends Error {
  readonly code: string;
  
  constructor(message: string, code: string) {
    super(message);
    this.name = 'GitHubError';
    this.code = code;
  }
}
```

**Error Handling Pattern**:
```typescript
async function saveSolution(data: SubmissionData): Promise<void> {
  try {
    await githubService.saveSolution(
      data.platform,
      data.problemId,
      data.code,
      data.language
    );
    console.log('[PushUp] Solution saved successfully');
  } catch (error) {
    console.error('[PushUp] Failed to save solution:', error);
    
    if (error instanceof GitHubError) {
      // Handle GitHub-specific errors
      throw new Error(`GitHub Error: ${error.message} (${error.code})`);
    }
    
    // Re-throw unknown errors
    throw error;
  }
}
```

### Async/Await Best Practices

**Preferred Pattern**:
```typescript
async function updateStreak(): Promise<StreakData> {
  const data = await chrome.storage.local.get(['streak', 'lastPushDate']);
  // Process data
  return data;
}
```

**Avoid Mixing Promise Chains**:
```typescript
// Bad
function updateStreak() {
  return chrome.storage.local.get(['streak']).then(data => {
    // Don't mix .then() with async/await
  });
}
```

### Console Logging

Use consistent logging prefixes:

```typescript
console.log('[PushUp] Normal operation message');
console.error('[PushUp] Error message with details');
console.warn('[PushUp] Warning about potential issue');
console.debug('[PushUp DEBUG] Detailed debugging information');
```

## Naming Conventions

### Files and Directories

**Component Files**: PascalCase
```
SubmissionCalendar.tsx
GitHubService.ts
NotificationCenter.tsx
```

**Utility Files**: kebab-case
```
storage-helper.ts
oauth-utils.ts
github-integration.ts
```

**Directories**: kebab-case
```
src/components/
src/utils/
src/content-scripts/
```

### Variables and Functions

**Variables**: camelCase
```typescript
const dailySubmissions = [];
const currentStreak = 0;
let isAuthenticated = false;
```

**Functions**: camelCase (descriptive verb + noun)
```typescript
function updateStreak() {}
function fetchUserProfile() {}
function calculateDifficulty() {}
```

**Constants**: UPPER_SNAKE_CASE
```typescript
const MAX_STREAK_DAYS = 365;
const GITHUB_API_BASE_URL = 'https://api.github.com';
const DEFAULT_REPO_NAME = 'coding-solutions';
```

### React Components

**Component Names**: PascalCase
```typescript
function SubmissionCalendar() {}
function AchievementBadge() {}
function SettingsPanel() {}
```

**Custom Hooks**: camelCase starting with "use"
```typescript
function useGitHubAuth() {}
function useSubmissionData() {}
function useStreakCalculation() {}
```

### Interfaces and Types

**Interfaces**: PascalCase with descriptive names
```typescript
interface SubmissionData {
  platform: string;
  problemId: string;
}

interface GitHubConfig {
  token: string;
  owner: string;
  repo: string;
}

interface DailySubmissionData {
  date: string;
  submissions: number;
}
```

**Type Aliases**: PascalCase
```typescript
type Platform = 'leetcode' | 'codeforces' | 'codechef';
type Difficulty = 'easy' | 'medium' | 'hard';
type AchievementStatus = 'locked' | 'unlocked' | 'in-progress';
```

### CSS Classes

Follow Tailwind conventions or use BEM methodology:

**Tailwind** (preferred):
```tsx
<div className="flex items-center justify-between p-4 bg-gray-800 rounded-lg">
```

**BEM** (for custom CSS):
```css
.submission-calendar {}
.submission-calendar__day {}
.submission-calendar__day--active {}
```

## Commit Message Guidelines

We follow the Conventional Commits specification for consistent commit messages.

### Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, missing semicolons, etc.)
- `refactor`: Code refactoring without changing functionality
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `chore`: Build process or auxiliary tool changes
- `ci`: CI configuration changes

### Scopes

Scope indicates which part of the codebase is affected:

- `content`: Content script changes
- `background`: Background service worker
- `popup`: Extension popup interface
- `github`: GitHub integration
- `calendar`: Calendar component
- `streak`: Streak calculation logic
- `oauth`: OAuth authentication
- `storage`: Storage utilities
- `api`: API integrations

### Examples

**Feature Addition**:
```
feat(content): add support for HackerRank platform

- Implement HackerRank submission detector
- Add platform-specific code extraction
- Update content script to include HackerRank URLs

Closes #45
```

**Bug Fix**:
```
fix(github): resolve file encoding issue in code commits

The btoa function was not properly handling special characters.
Switched to using base64 encoding with UTF-8 support.

Fixes #78
```

**Documentation**:
```
docs(api): update GitHub API integration documentation

- Add examples for error handling
- Document new GitHubError class
- Include rate limiting information
```

**Refactoring**:
```
refactor(streak): improve streak calculation logic

Simplified the date comparison logic and extracted
reusable functions for better maintainability.
```

**Performance**:
```
perf(calendar): optimize rendering for large datasets

- Implement virtualization for 365-day view
- Reduce re-renders with React.memo
- Cache computed values with useMemo
```

### Commit Message Rules

1. Use the imperative mood ("add feature" not "added feature")
2. Capitalize the first letter of the subject
3. Do not end the subject line with a period
4. Limit the subject line to 50 characters
5. Separate subject from body with a blank line
6. Wrap the body at 72 characters
7. Use the body to explain what and why, not how

## Testing Guidelines

### Manual Testing Checklist

Before submitting a PR, test the following:

**Extension Loading**:
- [ ] Extension loads without errors in `chrome://extensions/`
- [ ] No console errors on extension startup
- [ ] Popup opens correctly when clicking extension icon

**Platform Detection**:
- [ ] LeetCode submissions are detected correctly
- [ ] Codeforces submissions are detected correctly
- [ ] CodeChef submissions are detected correctly
- [ ] Code extraction works for different languages

**GitHub Integration**:
- [ ] Repository creation works properly
- [ ] Solutions are committed with correct file paths
- [ ] Commit messages are properly formatted
- [ ] Folder structure is created correctly

**UI Functionality**:
- [ ] Calendar displays correctly with proper colors
- [ ] Statistics update in real-time
- [ ] Streak counter increments properly
- [ ] Settings page saves preferences correctly

**Edge Cases**:
- [ ] Handle network failures gracefully
- [ ] Deal with invalid GitHub tokens
- [ ] Handle missing permissions
- [ ] Properly handle rate limiting

### Testing Platform Detection

Use the debug utilities in the browser console:

```javascript
// On LeetCode/Codeforces/CodeChef page
window.PushUpDebug.testExtraction();
window.PushUpDebug.checkSuccessIndicators();
window.PushUpDebug.debugDOM();
window.PushUpDebug.findTitleElements();
```

### Testing GitHub Integration

1. Create a test repository
2. Configure extension with test repo
3. Submit a solution on a platform
4. Verify the commit appears in GitHub
5. Check file content and commit message
6. Delete test repository after testing

## Documentation

### Code Comments

**Function Documentation**:
```typescript
/**
 * Updates the user's coding streak based on submission date.
 * Checks if the submission maintains the consecutive day streak.
 * 
 * @returns Object containing updated streak and any new achievements
 * @throws {Error} If storage access fails
 */
async function updateStreak(): Promise<{ streak: number; newAchievements: Achievement[] }> {
  // Implementation
}
```

**Complex Logic Comments**:
```typescript
// Extract problem ID from URL
// LeetCode URLs format: /problems/{problem-slug}/
const urlMatch = window.location.pathname.match(/\/problems\/([^/]+)/);
if (!urlMatch) {
  // Fallback to title extraction if URL parsing fails
  return extractFromTitle();
}
```

**TODO Comments**:
```typescript
// TODO: Add support for CodeChef contest problems
// TODO(username): Implement caching for repeated API calls
// FIXME: Handle special characters in problem titles
```

### README Updates

When adding new features, update:

1. Feature list in README.md
2. API documentation if adding new interfaces
3. Configuration section if adding new settings
4. Usage examples if changing user workflow

### JSDoc for Public APIs

```typescript
/**
 * Service for managing GitHub repository operations.
 * Handles authentication, file creation, and repository management.
 * 
 * @example
 * const github = new GitHubService({
 *   token: 'ghp_xxx',
 *   owner: 'username',
 *   repo: 'solutions'
 * });
 * 
 * await github.saveSolution('leetcode', 'two-sum', code, 'javascript');
 */
export class GitHubService {
  // Implementation
}
```

## Issue Reporting

### Before Creating an Issue

1. Search existing issues to avoid duplicates
2. Check if the issue exists in the latest version
3. Verify it's not covered in documentation
4. Gather relevant information (browser version, OS, error messages)

### Bug Report Template

```markdown
## Bug Description
A clear and concise description of the bug.

## Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. Submit solution on '...'
4. See error

## Expected Behavior
What you expected to happen.

## Actual Behavior
What actually happened.

## Environment
- Browser: Chrome 120.0.6099.109
- OS: Windows 11
- Extension Version: 1.0.0
- Platform: LeetCode

## Console Errors
```
Paste any relevant console errors here
```

## Screenshots
Add screenshots if applicable.

## Additional Context
Any other relevant information.
```

### Labels

Use appropriate labels when creating issues:

- `bug`: Something isn't working
- `enhancement`: New feature or request
- `documentation`: Documentation improvements
- `good first issue`: Good for newcomers
- `help wanted`: Extra attention needed
- `question`: Further information requested
- `platform:leetcode`: LeetCode-specific issue
- `platform:codeforces`: Codeforces-specific issue
- `platform:codechef`: CodeChef-specific issue

## Feature Requests

### Feature Request Template

```markdown
## Feature Description
Clear description of the proposed feature.

## Problem It Solves
What problem does this feature address?

## Proposed Solution
How would you implement this feature?

## Alternatives Considered
What alternative solutions have you thought about?

## Additional Context
- Mockups or diagrams
- Example use cases
- Related features in other tools

## Checklist
- [ ] I have searched for similar feature requests
- [ ] This feature aligns with the project's goals
- [ ] I am willing to contribute to implementing this feature
```

### Feature Discussion

1. Create a feature request issue
2. Discuss the feature with maintainers
3. Wait for approval before starting implementation
4. Create a PR referencing the feature request issue

## API Integration Details

### Adding New Platform Support

To add support for a new coding platform:

1. **Content Script Detection**:

Create a new platform detector class in `content.ts`:

```typescript
class HackerRankDetector extends SubmissionDetector {
  constructor() {
    super();
    this.platform = 'hackerrank';
  }
  
  protected isSuccessfulSubmission(): boolean {
    // Implement platform-specific success detection
    return document.querySelector('.success-indicator') !== null;
  }
  
  protected extractProblemId(): string {
    // Extract problem identifier
    const match = window.location.pathname.match(/\/challenges\/([^/]+)/);
    return match ? match[1] : 'unknown';
  }
  
  protected extractCode(): string {
    // Extract code from editor
    const codeEditor = document.querySelector('.CodeMirror');
    return codeEditor?.textContent || '';
  }
  
  protected extractLanguage(): string {
    // Detect programming language
    const langSelect = document.querySelector('select[name="language"]');
    return langSelect?.value || 'unknown';
  }
}
```

2. **Update Manifest Permissions**:

Add platform URLs to `manifest.json`:

```json
{
  "content_scripts": [
    {
      "matches": [
        "*://*.hackerrank.com/*"
      ],
      "js": ["assets/content.js"],
      "run_at": "document_idle"
    }
  ]
}
```

3. **GitHub Service Integration**:

Add platform folder handling in `github.ts`:

```typescript
private async initializeFolders() {
  const folders = [
    { name: 'leetcode', description: 'LeetCode Solutions' },
    { name: 'codeforces', description: 'Codeforces Solutions' },
    { name: 'codechef', description: 'CodeChef Solutions' },
    { name: 'hackerrank', description: 'HackerRank Solutions' }  // New
  ];
  // Rest of implementation
}
```

4. **Update File Path Logic**:

```typescript
private getFilePath(platform: string, problemId: string, language: string): string {
  const ext = this.getFileExtension(language);
  const sanitizedProblemId = this.sanitizeFileName(problemId);
  
  switch (platform.toLowerCase()) {
    case 'leetcode':
      return `leetcode/${sanitizedProblemId}.${ext}`;
    case 'codeforces':
      return `codeforces/${sanitizedProblemId}.${ext}`;
    case 'codechef':
      return `codechef/${sanitizedProblemId}.${ext}`;
    case 'hackerrank':  // New
      return `hackerrank/${sanitizedProblemId}.${ext}`;
    default:
      throw new GitHubError(`Unsupported platform: ${platform}`, 'INVALID_PLATFORM');
  }
}
```

5. **Test Thoroughly**:
   - Test submission detection on actual problems
   - Verify code extraction accuracy
   - Check GitHub repository organization
   - Validate commit messages

### GitHub API Best Practices

**Rate Limiting**:
```typescript
async function handleRateLimit(error: GitHubError) {
  if (error.code === 'RATE_LIMITED') {
    const resetTime = error.response?.headers['x-ratelimit-reset'];
    if (resetTime) {
      const waitTime = parseInt(resetTime) * 1000 - Date.now();
      console.warn(`[PushUp] Rate limited. Waiting ${waitTime}ms`);
      await new Promise(resolve => setTimeout(resolve, waitTime));
    }
  }
}
```

**Retry Logic**:
```typescript
async function retryOperation<T>(
  operation: () => Promise<T>,
  maxRetries: number = 3
): Promise<T> {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      return await operation();
    } catch (error) {
      if (attempt === maxRetries) throw error;
      
      const delay = Math.pow(2, attempt) * 1000;  // Exponential backoff
      console.log(`[PushUp] Retry attempt ${attempt} after ${delay}ms`);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
  throw new Error('Max retries exceeded');
}
```

**Token Validation**:
```typescript
async function validateToken(token: string): Promise<boolean> {
  try {
    const octokit = new Octokit({ auth: token });
    await octokit.users.getAuthenticated();
    return true;
  } catch (error) {
    console.error('[PushUp] Token validation failed:', error);
    return false;
  }
}
```

### Chrome Extension API Usage

**Storage Operations**:
```typescript
// Set data
await chrome.storage.local.set({
  streak: 10,
  lastPushDate: '2025-01-15'
});

// Get data
const data = await chrome.storage.local.get(['streak', 'lastPushDate']);
console.log('Current streak:', data.streak);

// Remove data
await chrome.storage.local.remove('streak');

// Clear all data
await chrome.storage.local.clear();
```

**Message Passing**:
```typescript
// From content script to background
chrome.runtime.sendMessage({
  type: 'NEW_SUBMISSION',
  data: submissionData
}, (response) => {
  if (response.success) {
    console.log('Submission processed');
  }
});

// In background script
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  if (message.type === 'NEW_SUBMISSION') {
    handleSubmission(message.data)
      .then(() => sendResponse({ success: true }))
      .catch((error) => sendResponse({ success: false, error: error.message }));
    return true;  // Will respond asynchronously
  }
});
```

**Notifications**:
```typescript
chrome.notifications.create({
  type: 'basic',
  iconUrl: '/logo.png',
  title: 'Solution Saved',
  message: 'Your LeetCode solution has been pushed to GitHub'
});
```

**Alarms**:
```typescript
// Create alarm
chrome.alarms.create('dailyCheck', {
  periodInMinutes: 60 * 24  // 24 hours
});

// Listen for alarm
chrome.alarms.onAlarm.addListener((alarm) => {
  if (alarm.name === 'dailyCheck') {
    performDailyCheck();
  }
});
```

## Architecture Guidelines

### Component Structure

Follow this structure for new components:

```typescript
import { useState, useEffect } from 'react';

// Types
interface ComponentProps {
  data: DataType;
  onEvent?: (param: string) => void;
}

// Component
export default function ComponentName({ data, onEvent }: ComponentProps) {
  // State
  const [localState, setLocalState] = useState<StateType>(initialValue);
  
  // Effects
  useEffect(() => {
    // Setup logic
    return () => {
      // Cleanup logic
    };
  }, [dependencies]);
  
  // Handlers
  const handleClick = () => {
    // Handler logic
  };
  
  // Render
  return (
    <div>
      {/* Component JSX */}
    </div>
  );
}
```

### Service Layer Pattern

Organize business logic in service classes:

```typescript
// services/SubmissionService.ts
export class SubmissionService {
  private githubService: GitHubService;
  
  constructor(githubService: GitHubService) {
    this.githubService = githubService;
  }
  
  async processSubmission(data: SubmissionData): Promise<void> {
    // Validate data
    this.validateSubmission(data);
    
    // Save to GitHub
    await this.githubService.saveSolution(
      data.platform,
      data.problemId,
      data.code,
      data.language
    );
    
    // Update statistics
    await this.updateStats(data);
  }
  
  private validateSubmission(data: SubmissionData): void {
    if (!data.code || data.code.trim().length === 0) {
      throw new Error('Code cannot be empty');
    }
    // More validation
  }
  
  private async updateStats(data: SubmissionData): Promise<void> {
    // Update implementation
  }
}
```

### State Management

Use React hooks and Chrome storage:

```typescript
// Custom hook for managing GitHub configuration
function useGitHubConfig() {
  const [config, setConfig] = useState<GitHubConfig | null>(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    chrome.storage.local.get(['githubToken', 'githubOwner', 'githubRepo'], (data) => {
      if (data.githubToken && data.githubOwner && data.githubRepo) {
        setConfig({
          token: data.githubToken,
          owner: data.githubOwner,
          repo: data.githubRepo
        });
      }
      setLoading(false);
    });
  }, []);
  
  const updateConfig = (newConfig: GitHubConfig) => {
    chrome.storage.local.set({
      githubToken: newConfig.token,
      githubOwner: newConfig.owner,
      githubRepo: newConfig.repo
    });
    setConfig(newConfig);
  };
  
  return { config, loading, updateConfig };
}
```

## Questions and Support

If you have questions about contributing:

1. Check the existing documentation
2. Search closed issues for similar questions
3. Ask in GitHub Discussions
4. Create an issue with the `question` label

## Recognition

Contributors will be recognized in:
- Repository contributors page
- Release notes for their contributions
- Special mentions for significant features

## License

By contributing to PushUp, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to PushUp! Your efforts help make coding practice tracking better for everyone.
