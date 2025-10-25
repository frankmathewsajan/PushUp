# PushUp - Automatic Coding Solution Tracker

A browser extension that automatically tracks your coding submissions across multiple platforms (LeetCode, Codeforces, CodeChef), pushes solutions to GitHub, and visualizes your progress with a GitHub-style contribution calendar.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Development](#development)
- [Technologies](#technologies)
- [Contributing](#contributing)
- [License](#license)

## Overview

PushUp is designed to help competitive programmers and coding enthusiasts maintain a professional portfolio of their solutions while tracking their progress visually. The extension automatically detects successful submissions on supported platforms, organizes code in a structured GitHub repository, and provides insights through an intuitive dashboard.

### Supported Platforms

- LeetCode
- Codeforces
- CodeChef

## Features

### Automatic Solution Tracking
- Real-time detection of successful submissions on supported platforms
- Automatic extraction of problem metadata (title, difficulty, language)
- Intelligent code extraction with fallback mechanisms
- Platform-specific detection algorithms for accuracy

### GitHub Integration
- Automatic repository creation with predefined folder structure
- Organized solution storage by platform
- Professional commit messages with context
- Automatic README generation for repository documentation

### Progress Visualization
- GitHub-style 365-day contribution calendar
- Color-coded intensity based on daily submission count
- Interactive tooltips showing submission details
- Real-time calendar updates on new submissions

### Statistics Dashboard
- Total problems solved counter
- Difficulty breakdown (Easy/Medium/Hard)
- Submission distribution pie chart
- Daily and cumulative statistics tracking

### Streak Tracking
- Consecutive day coding streak counter
- Achievement system for milestones
- Notifications for streak maintenance
- Longest streak records

### User Preferences
- Configurable notifications
- GitHub account integration
- Repository management
- Custom settings per user preference

## Architecture

### System Components

```
┌─────────────────┐
│  Content Scripts│
│  (Injected)     │
└────────┬────────┘
         │
         │ Detects Submissions
         │
         ▼
┌─────────────────┐
│  Background     │
│  Service Worker │
└────────┬────────┘
         │
         │ Processes Data
         │
         ├──────────┐
         │          │
         ▼          ▼
┌─────────────┐  ┌──────────────┐
│  GitHub     │  │  Chrome      │
│  API        │  │  Storage     │
└─────────────┘  └──────────────┘
         │          │
         │          │
         ▼          ▼
┌─────────────────────┐
│   Popup Dashboard   │
│   (React UI)        │
└─────────────────────┘
```

### Content Scripts
Injected into supported coding platforms to monitor DOM changes and detect successful submissions.

**File**: `frontend/src/content/content.ts`

**Responsibilities**:
- Monitor page for submission success indicators
- Extract problem data (ID, title, difficulty)
- Extract solution code from editor
- Detect programming language used
- Send submission data to background script

**Platform-Specific Detection**:
- LeetCode: Monitors for success icons, "Accepted" text, result panels
- Codeforces: Watches for "Accepted" verdict displays
- CodeChef: Detects "Success" status in result sections

### Background Service Worker
Manages core extension functionality and coordinates between components.

**File**: `frontend/src/background.ts`

**Responsibilities**:
- Process incoming submission data
- Manage GitHub service instance
- Update daily submission tracking
- Calculate and update streak information
- Trigger notifications for achievements
- Handle alarm-based periodic checks
- Maintain extension state

**Key Functions**:
- `handleNewSubmission()`: Process and save new submissions
- `updateDailySubmissions()`: Track daily submission counts
- `handleDailyCheck()`: Check streak status daily
- `handleRepoCheck()`: Periodic repository validation

### GitHub Service
Handles all interactions with GitHub API for repository management.

**File**: `frontend/src/utils/github.ts`

**Responsibilities**:
- Create and manage GitHub repositories
- Initialize folder structure for platforms
- Save solutions as commits
- Generate professional README files
- Handle API authentication and errors

**Key Methods**:
- `createRepositoryWithStructure()`: Create repo with predefined structure
- `saveSolution()`: Commit solution to appropriate folder
- `initializeFolders()`: Set up platform-specific directories
- `handleGitHubError()`: Unified error handling

### Popup Interface
React-based user interface for the extension popup.

**File**: `frontend/src/popup/App.tsx`

**Components**:
- Home dashboard with statistics
- Submission calendar component
- Settings configuration panel
- Achievements display
- Notifications center

### Backend OAuth Server
Express server handling GitHub OAuth authentication flow.

**File**: `backend/server.js`

**Endpoints**:
- `GET /auth/github/login`: Initiate OAuth flow
- `GET /auth/github/callback`: Handle OAuth callback
- `GET /me`: Verify token and fetch user profile
- `GET /`: Health check endpoint

## Installation

### Prerequisites

- Node.js (version 16 or higher)
- npm or yarn package manager
- Git for version control
- Chrome or Chromium-based browser
- GitHub account with personal access token

### Backend Setup

1. Clone the repository:
```bash
git clone https://github.com/revanthm1902/PushUp.git
cd PushUp
```

2. Install backend dependencies:
```bash
cd backend
npm install
```

3. Create environment configuration:
```bash
cp .env.example .env
```

4. Configure environment variables in `.env`:
```env
GITHUB_CLIENT_ID=your_github_client_id
GITHUB_CLIENT_SECRET=your_github_client_secret
REDIRECT_URI=http://localhost:3000/auth/github/callback
FRONTEND_REDIRECT=http://localhost:5173/
EXTENSION_ID=your_extension_id
GITHUB_OAUTH_SCOPE=repo,user
```

5. Start the backend server:
```bash
npm start
```

The server will run on `http://localhost:3000`

### Frontend Setup

1. Navigate to frontend directory:
```bash
cd ../frontend
```

2. Install frontend dependencies:
```bash
npm install
```

3. Build the extension:
```bash
npm run build
```

This creates a `dist` folder with the compiled extension.

### Loading Extension in Chrome

1. Open Chrome and navigate to `chrome://extensions/`

2. Enable "Developer mode" (toggle in top-right corner)

3. Click "Load unpacked" button

4. Select the `frontend/dist` folder from the build output

5. The PushUp extension should now appear in your extensions list

6. Pin the extension to your toolbar for easy access

## Configuration

### GitHub OAuth Setup

1. Go to GitHub Settings > Developer settings > OAuth Apps

2. Click "New OAuth App"

3. Fill in application details:
   - Application name: `PushUp Extension`
   - Homepage URL: `http://localhost:3000`
   - Authorization callback URL: `http://localhost:3000/auth/github/callback`

4. Click "Register application"

5. Copy the Client ID and Client Secret to your `.env` file

### GitHub Personal Access Token (Alternative)

For simpler setup without OAuth server:

1. Go to GitHub Settings > Developer settings > Personal access tokens > Tokens (classic)

2. Click "Generate new token" > "Generate new token (classic)"

3. Configure token:
   - Note: `PushUp Extension Access`
   - Expiration: Choose appropriate duration
   - Scopes: Select `repo` (full repository access)

4. Click "Generate token"

5. Copy the token and save it securely

6. Use this token in the extension settings instead of OAuth flow

### Extension Configuration

1. Click the PushUp extension icon in Chrome toolbar

2. Navigate to Settings (gear icon)

3. Click "Login with GitHub"

4. Complete authentication (OAuth or Personal Token)

5. Configure repository settings:
   - Enter repository name (e.g., `coding-solutions`)
   - Click "Set Up Repository"

6. Enable/disable preferences:
   - Notifications: Toggle submission notifications
   - Repo Checking: Enable periodic repository validation

## Usage

### Daily Workflow

1. Navigate to a supported platform (LeetCode, Codeforces, or CodeChef)

2. Solve a coding problem as usual

3. Submit your solution

4. On successful submission:
   - Extension automatically detects the submission
   - Extracts your code and problem metadata
   - Commits the solution to your GitHub repository
   - Updates your streak and statistics
   - Shows a success notification

5. Click the extension icon to view:
   - Updated contribution calendar
   - Current streak count
   - Difficulty breakdown
   - Total problems solved

### Viewing Your Progress

**Contribution Calendar**:
- Displays 365 days of submission activity
- Color intensity indicates daily submission count
- Hover over dates for detailed breakdown
- Gray squares indicate no submissions

**Statistics Panel**:
- Pie chart showing difficulty distribution
- Individual counters for Easy/Medium/Hard problems
- Total solved problems counter
- Current streak with fire icon

**GitHub Repository**:
- Solutions organized by platform in separate folders
- Each solution file named after the problem
- Professional README files for documentation
- Commit history showing your coding journey

### Manual Operations

**Manual Streak Increment** (for testing):
```javascript
// Available in popup for debugging purposes
chrome.storage.local.set({
  streak: currentStreak + 1,
  lastUpdated: new Date().toISOString()
});
```

**Force Repository Sync**:
Navigate to Settings > Click "Set Up Repository" to reinitialize folder structure.

**Clear Extension Data**:
```javascript
chrome.storage.local.clear();
```

## Project Structure

```
PushUp/
├── backend/
│   ├── .env                    # Environment configuration
│   ├── .gitignore              # Git ignore rules
│   ├── package.json            # Backend dependencies
│   └── server.js               # Express OAuth server
│
├── frontend/
│   ├── public/
│   │   └── manifest.json       # Chrome extension manifest
│   │
│   ├── src/
│   │   ├── achievements/
│   │   │   └── achievements.tsx    # Achievement display component
│   │   │
│   │   ├── components/
│   │   │   └── SubmissionCalendar.tsx  # GitHub-style calendar
│   │   │
│   │   ├── content/
│   │   │   └── content.ts      # Content script for platforms
│   │   │
│   │   ├── notifications/
│   │   │   └── notifications.tsx   # Notification center
│   │   │
│   │   ├── popup/
│   │   │   ├── App.css         # Popup styles
│   │   │   ├── App.tsx         # Main popup component
│   │   │   ├── index.css       # Global styles
│   │   │   └── main.tsx        # Popup entry point
│   │   │
│   │   ├── settings/
│   │   │   └── settings.tsx    # Settings configuration
│   │   │
│   │   ├── utils/
│   │   │   ├── github.ts       # GitHub API service
│   │   │   ├── oauth.ts        # OAuth helper functions
│   │   │   ├── storage-helper.ts   # Storage utilities
│   │   │   ├── storage.ts      # Storage interface
│   │   │   └── streak.ts       # Streak calculation logic
│   │   │
│   │   ├── background.ts       # Service worker
│   │   └── vite-env.d.ts       # TypeScript declarations
│   │
│   ├── .gitignore              # Frontend Git ignore
│   ├── eslint.config.js        # ESLint configuration
│   ├── index.html              # Extension popup HTML
│   ├── package.json            # Frontend dependencies
│   ├── postcss.config.cjs      # PostCSS configuration
│   ├── README.md               # Frontend documentation
│   ├── tailwind.config.js      # Tailwind CSS config
│   ├── tsconfig.app.json       # App TypeScript config
│   ├── tsconfig.json           # Base TypeScript config
│   ├── tsconfig.node.json      # Node TypeScript config
│   └── vite.config.ts          # Vite build configuration
│
├── LICENSE                     # Project license
├── README.md                   # This file
├── CONTRIBUTING.md             # Contribution guidelines
└── WORKFLOW.md                 # Detailed workflow documentation
```

## API Documentation

### Chrome Storage Schema

**GitHub Configuration**:
```typescript
interface GitHubConfig {
  github_token: string;        // GitHub access token
  githubOwner: string;         // Repository owner username
  githubRepo: string;          // Repository name
  repoUrl?: string;            // Full repository URL
  setupDate?: string;          // ISO timestamp of setup
}
```

**Submission Tracking**:
```typescript
interface DailySubmissionData {
  date: string;                // ISO date (YYYY-MM-DD)
  submissions: number;         // Total submissions on date
  problems: {
    easy: number;              // Easy problems solved
    medium: number;            // Medium problems solved
    hard: number;              // Hard problems solved
  };
}

interface SubmissionStats {
  totalSolved: number;         // Total problems solved
  easy: number;                // Total easy problems
  medium: number;              // Total medium problems
  hard: number;                // Total hard problems
  lastSubmissionDate: string;  // Last submission date
  dailySubmissions: DailySubmissionData[];  // 365 days max
}
```

**Streak Data**:
```typescript
interface StreakData {
  streak: number;              // Current consecutive days
  lastPushDate: string;        // Last push date (ISO)
  longestStreak: number;       // Longest streak achieved
}
```

**User Preferences**:
```typescript
interface UserPreferences {
  notifications: boolean;      // Enable/disable notifications
  repoCheck: boolean;          // Enable periodic repo checks
  githubUser: string;          // GitHub username
}
```

### Message Passing Protocol

**Content Script to Background**:
```typescript
// New submission detected
chrome.runtime.sendMessage({
  type: 'NEW_SUBMISSION',
  data: {
    platform: 'leetcode' | 'codeforces' | 'codechef',
    problemId: string,
    code: string,
    language: string,
    difficulty?: 'easy' | 'medium' | 'hard',
    title?: string
  }
});
```

**GitHub Configuration Update**:
```typescript
chrome.runtime.sendMessage({
  type: 'GITHUB_CONFIG_UPDATED',
  data: {
    token: string,
    owner: string,
    repo: string
  }
});
```

**Get Submission Statistics**:
```typescript
const response = await chrome.runtime.sendMessage({
  type: 'GET_SUBMISSION_STATS'
});

// Response format:
{
  success: true,
  data: {
    dailySubmissions: DailySubmissionData[],
    totalSolved: number,
    easy: number,
    medium: number,
    hard: number
  }
}
```

### GitHub API Integration

**Service Initialization**:
```typescript
import { GitHubService } from './utils/github';

const github = new GitHubService({
  token: 'ghp_xxxxxxxxxxxxx',
  owner: 'username',
  repo: 'repository-name'
});
```

**Create Repository**:
```typescript
const result = await github.createRepositoryWithStructure(
  'my-solutions',
  'My coding solutions managed by PushUp'
);

// Returns:
{
  success: true,
  repoUrl: 'https://github.com/username/my-solutions',
  owner: 'username',
  repo: 'my-solutions'
}
```

**Save Solution**:
```typescript
await github.saveSolution(
  'leetcode',              // platform
  'two-sum',               // problemId
  'function twoSum() {}',  // code
  'javascript'             // language
);
```

**Error Handling**:
```typescript
import { GitHubError } from './utils/github';

try {
  await github.saveSolution(...);
} catch (error) {
  if (error instanceof GitHubError) {
    console.error(`GitHub Error: ${error.message}`);
    console.error(`Error Code: ${error.code}`);
    // Codes: AUTH_REQUIRED, UNAUTHORIZED, FORBIDDEN, NOT_FOUND, etc.
  }
}
```

### Streak Calculation Algorithm

**Update Streak**:
```typescript
import { updateStreak } from './utils/streak';

const result = await updateStreak();
// Returns: { streak: number, newAchievements: Achievement[] }
```

**Get Streak Statistics**:
```typescript
import { getStreakStats } from './utils/streak';

const stats = await getStreakStats();
// Returns: { streak: number, lastPushDate: string, longestStreak: number }
```

**Achievement System**:
```typescript
interface Achievement {
  id: string;                  // Unique achievement ID
  title: string;               // Display title
  message: string;             // Achievement message
  emoji: string;               // Display emoji
  condition: (streak: number) => boolean;  // Unlock condition
}

// Predefined achievements:
// - streak-7: 7-day streak
// - streak-30: 30-day streak
// - streak-50: 50-day streak
// - streak-100: 100-day streak
```

## Development

### Development Setup

1. Fork the repository on GitHub

2. Clone your fork:
```bash
git clone https://github.com/YOUR_USERNAME/PushUp.git
cd PushUp
```

3. Add upstream remote:
```bash
git remote add upstream https://github.com/revanthm1902/PushUp.git
```

4. Install dependencies:
```bash
cd backend && npm install
cd ../frontend && npm install
```

5. Start development servers:

**Backend**:
```bash
cd backend
npm run dev
```

**Frontend**:
```bash
cd frontend
npm run dev
```

### Development Commands

**Backend**:
```bash
npm start          # Start production server
npm run dev        # Start development server with hot reload
```

**Frontend**:
```bash
npm run dev        # Start Vite dev server
npm run build      # Build extension for production
npm run lint       # Run ESLint checks
npm run preview    # Preview production build
```

### Code Style Guidelines

**TypeScript**:
- Use strict type checking
- Prefer interfaces over types for object shapes
- Use explicit return types for functions
- Avoid `any` type unless absolutely necessary

**Naming Conventions**:
- Components: PascalCase (`SubmissionCalendar`)
- Functions: camelCase (`updateStreak`)
- Constants: UPPER_SNAKE_CASE (`GITHUB_API_URL`)
- Files: kebab-case for utilities, PascalCase for components
- Interfaces: PascalCase with descriptive names

**React Components**:
- Use functional components with hooks
- Keep components focused and single-purpose
- Extract reusable logic into custom hooks
- Use TypeScript for prop validation

**File Organization**:
- Group related functionality in directories
- Keep utility functions in `utils/` folder
- Separate components by feature
- Co-locate styles with components when using CSS modules

### Testing Extensions Locally

1. Build the extension:
```bash
cd frontend
npm run build
```

2. Load unpacked extension in Chrome:
   - Navigate to `chrome://extensions/`
   - Enable Developer mode
   - Click "Load unpacked"
   - Select `frontend/dist` folder

3. Test on coding platforms:
   - Visit LeetCode/Codeforces/CodeChef
   - Solve and submit a problem
   - Check console for extension logs
   - Verify GitHub repository update

4. Debug content script:
```javascript
// Available in browser console on coding platforms
window.PushUpDebug.testExtraction();
window.PushUpDebug.checkSuccessIndicators();
window.PushUpDebug.debugDOM();
```

### Building for Production

1. Update version in `manifest.json`:
```json
{
  "version": "1.1.0"
}
```

2. Build optimized bundle:
```bash
cd frontend
npm run build
```

3. Test the production build:
```bash
npm run preview
```

4. Create distribution package:
```bash
cd dist
zip -r ../pushup-extension-v1.1.0.zip .
```

## Technologies

### Frontend Technologies

**Core Framework**:
- React 19.1.1 - UI component library
- TypeScript 5.8.3 - Type-safe JavaScript
- Vite 7.1.3 - Build tool and dev server

**UI Libraries**:
- Tailwind CSS 3.4.17 - Utility-first CSS framework
- Lucide React 0.542.0 - Icon library
- Recharts 3.1.2 - Chart and graph library

**Chrome APIs**:
- chrome.storage - Persistent data storage
- chrome.runtime - Extension messaging
- chrome.notifications - System notifications
- chrome.alarms - Scheduled tasks

**GitHub Integration**:
- @octokit/rest 22.0.0 - GitHub REST API client

**Routing**:
- react-router-dom 7.8.2 - Client-side routing

### Backend Technologies

**Runtime**:
- Node.js - JavaScript runtime
- Express 4.21.2 - Web application framework

**Authentication**:
- Passport 0.7.0 - Authentication middleware
- passport-github2 0.1.12 - GitHub OAuth strategy

**Utilities**:
- axios 1.6.7 - HTTP client
- cors 2.8.5 - CORS middleware
- dotenv 16.3.1 - Environment configuration
- express-session 1.18.2 - Session management

### Development Tools

**Code Quality**:
- ESLint 9.33.0 - JavaScript linter
- TypeScript ESLint 8.39.1 - TypeScript-specific linting

**Build Tools**:
- Vite - Fast build tool and dev server
- PostCSS 8.5.6 - CSS transformation
- Autoprefixer 10.4.21 - CSS vendor prefixes

**Development**:
- ts-node-dev 2.0.0 - TypeScript execution with hot reload
- copyfiles 2.4.1 - File copying utility

## Contributing

We welcome contributions from the community! Please read our [CONTRIBUTING.md](CONTRIBUTING.md) file for detailed guidelines on:

- Code of Conduct
- Setting up development environment
- Submitting pull requests
- Coding standards and conventions
- Reporting bugs and requesting features

Quick contribution steps:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Write/update tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built with inspiration from GitHub's contribution calendar
- Icon designs from Lucide React
- OAuth integration powered by Passport.js
- Community contributions and feedback

## Support

For questions, issues, or feature requests:

- GitHub Issues: https://github.com/revanthm1902/PushUp/issues
- Discussions: https://github.com/revanthm1902/PushUp/discussions

## Roadmap

Planned features and improvements:

- Support for additional coding platforms (HackerRank, AtCoder)
- Advanced analytics and insights dashboard
- Contest tracking and performance metrics
- Multi-language solution support per problem
- Social features and friend comparisons
- Export statistics as images
- Dark/light theme customization
- Code review and solution comparison tools

---

Built with dedication for the coding community. Happy coding!
