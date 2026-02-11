# Code Review POC - Technical Documentation

## Overview
This is a prototype application demonstrating an enhanced code review experience for contextualizing reviewers. The interface is inspired by SonarQube Cloud's design patterns while focusing on reducing reviewer cognitive load through better presentation of quality metrics and code changes.

## Tech Stack
- **Framework**: Next.js 16.1.1 (React 19.2.3)
- **Styling**: Custom CSS with design system + Tailwind CSS v4
- **TypeScript**: v5

## Project Structure

```
code-review-fab/
├── app/
│   ├── components/
│   │   └── PullRequestsPage.tsx    # Main Pull Requests page component
│   ├── pr/
│   │   └── [id]/
│   │       └── page.tsx            # PR detail page (client component)
│   ├── design-system.css           # Color palette and design tokens
│   ├── echoes-tokens-base.css      # Echoes design system base tokens
│   ├── echoes-tokens-dark.css      # Echoes design system dark theme
│   ├── styles.css                  # Component-specific styles
│   ├── globals.css                 # Global styles and imports
│   ├── layout.tsx                  # Root layout
│   ├── page.tsx                    # Home page (renders PullRequestsPage)
│   └── favicon.ico                 # Favicon
├── public/
│   ├── Sonar Qube Cloud.svg        # SonarQube Cloud logo
│   └── *.svg                       # Various UI icons
├── next.config.ts                  # Next.js configuration
├── package.json
└── README.md
```

## Design System

### Custom Design System (SonarQube Cloud-inspired)

The design system uses a **custom color palette** inspired by SonarQube Cloud, defined in `app/design-system.css` with CSS custom properties. The application is **dark mode by default**.

#### Primary Colors
- `--color-bg-primary`: #1E212E (Main content background)
- `--color-bg-secondary`: #3c4248
- `--color-sidebar-bg`: #2B2F3F (Sidebar and top nav background)
- `--color-content-bg`: #1E212E
- `--color-card-bg`: #393f45 (Card/Panel background)

#### Text Colors
- `--color-text-primary`: #ffffff
- `--color-text-secondary`: #e6e6e6
- `--color-text-tertiary`: #b4b8bd
- `--color-text-muted`: #8a8f95

#### Accent Colors
- `--color-accent-primary`: #9FA9ED (Purple-Blue - primary actions)
- `--color-accent-primary-hover`: #BDC6FF (Purple-Blue hover state)
- `--color-accent-secondary`: #9FA9ED (Purple-Blue - icons, links)
- `--color-link`: #9FA9ED

#### Status Colors
- `--color-success`: #4caf50 (Green - Passed status)
- `--color-warning`: #ff9800 (Orange - Warning badges)
- `--color-error`: #f44336 (Red - Failed status)

#### Badge Colors
- `--color-badge-new`: #4b9fd8 (Blue - "New" badge)
- `--color-badge-beta`: #4b9fd8 (Blue - "Beta" badge)
- `--color-badge-private`: #8a8f95

#### Border Colors
- `--color-border-primary`: #4d5463
- `--color-border-secondary`: #3a3f45
- `--color-border-subtle`: #2e3238

#### Button Colors
- `--color-btn-primary-bg`: #9FA9ED (Primary button background)
- `--color-btn-primary-hover`: #BDC6FF (Primary button hover state)
- `--color-btn-primary-text`: #2A2F40 (Primary button text color)
- `--color-btn-primary-active`: #8892E8 (Primary button active state)

### Usage
All colors are accessible via CSS custom properties:

```css
.my-element {
  background-color: var(--color-bg-primary);
  color: var(--color-text-primary);
  border-color: var(--color-border-primary);
}
```

## Pages

### Pull Requests Page (Homepage)
**Route**: `/` (localhost:3000)

**Features**:
- ✅ Top navigation bar with SonarQube Cloud logo and navigation links
- ✅ Left sidebar with project selector and navigation menu
- ✅ Pull Requests link is active in sidebar
- ✅ Page header with breadcrumb and title
- ✅ Search bar and filters
- ✅ Pull requests list (dummy items) - clickable, links to detail pages
- ✅ Status indicators with visual checkmarks/icons
- ✅ Floating action button (bottom right)

**Status**: Fully functional for navigation

### PR Detail Page
**Route**: `/pr/[id]` (e.g., `/pr/35`)

**Features**:
- ✅ Same top navigation and sidebar as homepage
- ✅ Page header with:
  - Breadcrumb navigation with PR number
  - Title with version
  - Action buttons (left to right):
    - "Review changes" button with dropdown modal (primary button)
    - "View on GitHub" button with GitHub icon (secondary button, tooltip: "This project is bound to GitHub")
    - "Star" icon button (secondary button, tooltip: "Add this project to favorites")
  - Review options: Comment, Request changes, Approve
  - Sticky positioning when scrolling
- ✅ **Files View** (default and only view):
  - Left sidebar: Groups panel showing file groups (sticky positioning)
    - File groups collapsed by default (click chevron to expand file list)
    - Click group name to anchor/scroll to that group's files
  - Right content: File change cards with:
    - File metadata and statistics
    - Code diff viewer with syntax highlighting (YAML)
    - Inline comment capability (UI only)
    - Expandable/collapsible sections
    - Mark as reviewed functionality
- ✅ **Author's Note Panel** (bottom-right FAB):
  - Opens by default when loading PR page
  - Tab navigation: "Context" and "Conversations"
  - Context tab:
    - PR summary and key changes
    - Progress tracking (files reviewed, groups completed, estimated time)
    - Visual progress bar
  - Conversations tab:
    - GitHub-style comment thread
    - Dummy conversation between team members
    - Avatar placeholders, timestamps, formatted content
  - "Ask AI about this PR" search input (bottom of panel)
  - Collapsible panel (click collapse button or FAB to toggle)

**Design Notes**:
- Context tab has been removed to focus solely on file review
- All card components use transparent backgrounds with borders
- Scrollbar always visible to prevent layout shift
- Sticky positioning: Page header and file groups sidebar remain visible during scroll
- Primary button color scheme: #9FA9ED background, #2A2F40 text, #BDC6FF hover

## Running the Project

### Development Server
```bash
npm run dev
```
The app will be available at: http://localhost:3000

### Build for Production
```bash
npm run build
npm start
```

### Linting
```bash
npm run lint
```

### Cleanup
```bash
# Remove build cache to reduce project size
rm -rf .next out
```

## GitHub Pages Deployment

### Live Site
**URL**: https://frieda-sonar-org.github.io/laughing-guacamole/
**Repository**: frieda-sonar-org/laughing-guacamole

### Deployment Configuration
The project is configured for static export to GitHub Pages:
- **basePath**: `/laughing-guacamole` (matches GitHub repository name)
- **assetPrefix**: `/laughing-guacamole`
- **output**: `export` (static site generation)

### GitHub Actions Workflow
Location: `.github/workflows/deploy.yml`

**IMPORTANT**: The workflow has been configured to handle corporate npm registry issues:

```yaml
- name: Setup Node
  uses: actions/setup-node@v4
  with:
    node-version: '20'
    # NOTE: npm cache is intentionally DISABLED to prevent caching JFrog auth

- name: Install dependencies
  run: |
    rm -f package-lock.json .npmrc
    npm install --registry=https://registry.npmjs.org/
```

### Troubleshooting: npm Authentication Errors

**Problem**: GitHub Actions fails with `npm error code E401 - Incorrect or missing password`

**Root Cause**:
- Local development uses corporate JFrog Artifactory registry (configured in `~/.npmrc`)
- When `package-lock.json` is generated locally, it contains JFrog registry URLs for all packages
- `npm ci` uses exact URLs from `package-lock.json`, causing authentication failures in GitHub Actions
- npm cache in GitHub Actions can persist old authentication state

**Solution Applied**:
1. **Remove npm cache**: The `cache: 'npm'` option is removed from `setup-node` step to prevent caching authentication state
2. **Delete lockfile before install**: `package-lock.json` and `.npmrc` are deleted in CI before install
3. **Force public registry**: Use `--registry=https://registry.npmjs.org/` flag to bypass JFrog URLs

**DO NOT**:
- ❌ Add `cache: 'npm'` back to the workflow
- ❌ Use `npm ci` (it will use exact URLs from package-lock.json)
- ❌ Try to fix package-lock.json manually (it has 400+ package URLs)

**Alternative Solutions** (not implemented):
- Regenerate `package-lock.json` from a clean environment without JFrog registry
- Use a separate `.npmrc` for CI that overrides all registry settings
- Set up GitHub Actions secrets with JFrog credentials (not recommended)

### Manual Deployment
To manually trigger deployment:
```bash
git push origin main
```

The GitHub Actions workflow will automatically:
1. Install dependencies from public npm registry
2. Build Next.js static export
3. Deploy to GitHub Pages

## Component Details

### PullRequestsPage Component
Location: `app/components/PullRequestsPage.tsx`

**Sections**:
1. **Top Navigation**
   - Logo (SonarQube cloud SVG)
   - Main navigation (My Projects, My Issues, My Portfolios, Explore)
   - Right actions (search, notifications, user menu icons)

2. **Sidebar**
   - Project selector dropdown
   - Navigation sections:
     - Overview & Dashboards
     - Analysis (Summary, Issues, Security Hotspots, etc.)
     - Information (Pull Requests, Branches)
   - Badges for "New" and "Beta" features
   - Count indicators

3. **Main Content**
   - Breadcrumb trail
   - Page title
   - Search and filter controls
   - Pull requests list with:
     - PR number and title
     - Status (Passed/Failed)
     - Icons
     - Timestamps
     - Commit hashes

4. **Floating Action Button**
   - Fixed position (bottom right)
   - Gradient background
   - Checkmark icon

### PR Detail Page Component
Location: `app/pr/[id]/page.tsx`

**Client Component** (`'use client'`)

**State Management**:
- `showGroupFiles`, `showGroupFiles2`, `showGroupFiles3`, `showGroupFiles4`: Toggle file list visibility for each group (default: false/collapsed)
- `showFileChanges`, `showFileChanges2`, `showFileChanges3`: Toggle file changes section visibility
- `showReviewModal`: Control review dropdown visibility
- `reviewType`: Selected review type (comment/request changes/approve)
- `reviewComment`: Review comment text
- `showReviewerNote`: Toggle Author's Note panel visibility (default: true/open)
- `authorNoteTab`: Active tab in Author's Note panel ('context' or 'conversation')
- `aiQuestion`: User input for AI search query
- `fileExpanded1-6`: Individual file collapse states
- `fileChecked1-6`: Individual file review status
- `groupReviewed1-4`: Group review status (all files in group marked as reviewed)

**Sections**:
1. **Top Navigation & Sidebar** (same as homepage)

2. **Page Header** (sticky positioning, top: 0)
   - Breadcrumb with PR number
   - Title with version hashtag
   - Action buttons:
     - "Review changes" button (primary button with dropdown)
     - "View on GitHub" button (secondary with GitHub icon)
     - "Star" button (icon-only secondary)

3. **Review Modal**
   - Three review types with radio selection (styled with primary button colors)
   - Comment textarea (placeholder: "Add a comment")
   - Submit button (primary button style)

4. **Files View**
   - Groups sidebar (left, sticky positioning at top: 152px):
     - Shows file groupings
     - Group names and file counts (clickable for navigation)
     - Chevron buttons to toggle file list visibility
     - Scrollable with max-height constraint
   - File changes (right):
     - File metadata cards
     - Code diff tables with:
       - Line numbers
       - Add/remove indicators
       - Syntax-highlighted code
       - Inline comment buttons (on hover)
     - "Mark as reviewed" button per file (primary button style)

5. **Author's Note Panel** (FAB - Floating Action Button)
   - FAB button (bottom-right):
     - Gradient purple-blue background
     - Icon and text label: "Author's Note"
     - Toggles panel visibility
   - Panel (appears above FAB when open):
     - Header with title "Author's Note" and collapse button
     - Tab navigation (`.author-note-tabs`):
       - "Context" tab (default)
       - "Conversations" tab
     - Tab content:
       - **Context tab**:
         - PR summary text (description and key changes)
         - PR Progress section with statistics and progress bar
         - Files reviewed, groups completed, estimated time left
       - **Conversations tab**:
         - Conversation thread (`.conversation-thread`)
         - GitHub-style comments with avatars, authors, timestamps
         - Support for formatted content (paragraphs, lists, inline code)
     - AI Search section (bottom):
       - "Ask AI about this PR" title
       - Input field with search button
       - Placeholder: "e.g., 'What security concerns should I look for?' or 'Explain the authentication flow'"

## Styling Architecture

### CSS Organization
The CSS is organized in layers:

1. **design-system.css**: Core design tokens (colors, spacing)
2. **echoes-tokens-base.css**: Echoes design system base tokens
3. **echoes-tokens-dark.css**: Echoes design system dark theme
4. **styles.css**: Component-specific styles
5. **globals.css**: Global imports and configuration

### Dark Mode
The application is **dark mode by default** with no light mode option:
- Root variables set to dark colors
- Body background and text color enforced
- All components styled using dark theme colors

### Sticky Positioning
- **Page Header**: Uses `position: sticky` with `top: 0` relative to `.main-content` scroll container
  - Stays at the top when scrolling through file changes
  - `z-index: 100` ensures it stays above other content
- **Files Groups Sidebar**: Uses `position: sticky` with `top: 152px` (page header height + padding)
  - Remains visible while scrolling file changes
  - Has `max-height: calc(100vh - 224px)` with `overflow-y: auto` for long file lists

### Responsive Design
- Desktop: Full sidebar and all columns visible
- Tablet (< 1024px): Condensed navigation, smaller sidebar
- Mobile (< 768px): Collapsible sidebar, simplified layout

### Key CSS Classes - Author's Note Panel
- `.fab-container`: Container for FAB button and panel
- `.fab-button`: Floating action button with gradient background
- `.reviewer-note-panel`: Main panel container (fixed positioning)
- `.reviewer-note-header`: Panel header with title and collapse button
- `.reviewer-note-content`: Panel content area with padding and flex layout
- `.author-note-tabs`: Tab navigation container with border-bottom
- `.author-note-tab`: Individual tab button (`.active` class for selected tab)
- `.reviewer-note-section`: Content section with gap between items
- `.reviewer-note-section-title`: Section heading (14px, semibold)
- `.reviewer-note-text`: Body text (14px, secondary color)
- `.pr-progress-stats`: Grid layout for progress statistics
- `.progress-bar`: Visual progress indicator with gradient fill
- `.conversation-thread`: Container for conversation comments
- `.conversation-comment`: Individual comment card with border and padding
- `.comment-header`: Comment header with avatar and metadata
- `.comment-avatar-text`: Avatar placeholder with initials
- `.comment-meta`: Author name and timestamp container
- `.comment-body`: Comment content with formatted text support
- `.ai-search-container`: AI search input and button container
- `.ai-search-input`: Search input field
- `.ai-search-button`: Search submit button (primary button style)

## Navigation Structure
```
/ (Pull Requests List)
  └── /pr/:id (Pull Request Detail - Files View)
```

## Key Design Decisions

### Why Files-Only View?
We removed the Context tab (Description + Discussion) to focus solely on **code review**:
- **Reduces context switching**: Everything needed for review is in one place
- **Minimizes cognitive load**: No need to toggle between tabs
- **Emphasizes code**: The files and quality metrics are the primary focus
- **Streamlines workflow**: Faster path from viewing to reviewing

### Color Scheme
- Background colors: #2B2F3F (sidebar/nav), #1E212E (content area)
- Maintains SonarQube Cloud's exact color palette

### Layout
- Scrollbar always visible (`overflow-y: scroll`) to prevent layout shift
- Transparent card backgrounds with stroke/borders only (no fill)

## Future Development

### Completed Features
- ✅ Click-through functionality for PR items
- ✅ PR detail page with Files view
- ✅ Code diff viewer with syntax highlighting
- ✅ Collapsible file sections and groups
- ✅ Review modal with multiple review types
- ✅ Client-side navigation
- ✅ File and group review workflow (mark as reviewed)
- ✅ Author's Note panel with Context and Conversations tabs
- ✅ Progress tracking (files reviewed, groups completed, time estimation)
- ✅ Conversation thread (dummy data)
- ✅ Group anchor navigation (click group name to scroll to section)

### Planned Features
- [ ] Additional pages (Summary, Issues, Dashboard, etc.)
- [ ] Interactive search and filtering
- [ ] Real data integration (SonarQube API)
- [ ] User authentication
- [ ] Functional inline comment submission
- [ ] Coverage indicators with visual metrics
- [ ] Real-time collaboration features
- [ ] AI-powered PR analysis (Ask AI functionality)
- [ ] Real conversation data integration

## Notes
- All content is currently dummy data
- Navigation between pages is functional (PR list → PR detail → PR list)
- PR detail page shows Files view by default (no tab navigation)
- Collapsible sections functional for file groups
- **Context tab removed**: Focus is now entirely on file-based code review

## Resources
- Design reference: SonarQube Cloud interface
- Framework: [Next.js Documentation](https://nextjs.org/docs)
- Styling: [Tailwind CSS v4](https://tailwindcss.com/docs)

---

## Development History

### Recent Changes

- **February 11, 2026**:
  - **UI Enhancement - Page Header Actions**:
    - Added "View on GitHub" button with GitHub icon and tooltip ("This project is bound to GitHub")
    - Added "Star" icon button with tooltip ("Add this project to favorites")
    - Reordered action buttons: "Review changes" (leftmost), "View on GitHub", "Star"
    - Changed review modal placeholder text from "Write your review" to "Add a comment"

  - **Design System Update - Primary Button Color Scheme**:
    - Updated primary button colors from blue (#4b9fd8) to purple-blue (#9FA9ED)
    - Button text color: #2A2F40 (dark, high contrast on light purple background)
    - Hover state: #BDC6FF (lighter purple-blue)
    - Applied consistently across all primary buttons:
      - "Review changes" button
      - "Submit review" button
      - "Mark as reviewed" button
      - "Upgrade" button in sidebar
    - Updated radio button selected state to match primary button colors (#9FA9ED background, #2A2F40 dot)

  - **Sticky Positioning Implementation**:
    - Made page header sticky (`position: sticky`, `top: 0`, `z-index: 100`)
    - Made file groups sidebar sticky (`position: sticky`, `top: 152px`)
    - Fixed gap before page header by removing negative margin and adding conditional padding removal
    - Sticky positioning is relative to `.main-content` scroll container
    - File groups sidebar has `max-height: calc(100vh - 224px)` with scrollable overflow

  - **Button Style Standardization**:
    - Audited all primary buttons across the application
    - Ensured all primary buttons use design system CSS variables:
      - `var(--color-btn-primary-bg)` for background
      - `var(--color-btn-primary-text)` for text color
      - `var(--color-btn-primary-hover)` for hover state
    - Secondary buttons (View on GitHub, Star) use transparent background with border

  - **Component Styling**:
    - Added `.btn-view-github` styles (secondary button with GitHub icon)
    - Added `.btn-star` styles (icon-only secondary button)
    - Updated `.btn-review-changes` to use design system variables
    - Updated `.btn-review-submit` to use design system variables
    - Updated `.mark-reviewed-button` to use design system variables
    - Updated `.review-option-radio.selected` to use primary button colors

  - **Author's Note Panel Enhancement**:
    - Renamed "Reviewer's Note" to "Author's Note" throughout the application
    - Added tab navigation with two tabs: "Context" and "Conversations"
    - Reorganized content structure:
      - "Ask AI about this PR" section moved to bottom (below tab content)
      - "Summary" section title removed (content now part of Context tab)
      - Changed "Estimated Time Left" to "Est. Time Left" to shorten the label
      - File groups default to collapsed state (file lists hidden by default)
    - Implemented "Conversations" tab with dummy conversation thread:
      - GitHub-style comment layout with avatar placeholders
      - 4 conversation comments between team members (jdoe, asmith, mkumar)
      - Realistic PR discussion: initial description, questions, responses, feedback
      - Support for paragraphs, lists, inline code, and emojis
      - Comment metadata: author names, timestamps (relative time)
    - Added conversation thread styling (`.conversation-thread`, `.conversation-comment`)
    - Reused existing comment styles (`.comment-header`, `.comment-avatar`, `.comment-meta`, `.comment-body`)
    - Author's Note panel opens by default when loading PR detail page

- **February 10, 2026**:
  - **UI Simplification**: Removed Context tab from PR detail page
    - Eliminated tab navigation (Context and Files tabs)
    - Files view now displays by default with no tab switching required
    - Removed Description and Discussion sections to streamline the review workflow
    - Removed unused state variables: `showDescription`, `showDiscussion`, `activeTab`
    - Removed unused `comments` array data
    - Simplified PR detail page to focus solely on file review functionality
    - Updated documentation to reflect single-view architecture
  - **File Size Reduction**: Reduced PR detail page from 1020 lines to 807 lines
  - **Project Cleanup**: Reduced project size from 891MB to 421MB by removing build cache

- **January 12, 2026**:
  - Reverted from Echoes design system integration back to custom design system
  - Updated dark mode enforcement in globals.css and styles.css
  - Fixed CSS import order (custom CSS before Tailwind)
  - Documented all design tokens and color values

- **Previous work**:
  - Implemented PR detail pages with initial tab structure
  - Added inline comment styling
  - Built code diff viewer with syntax highlighting

---

**Last Updated**: February 11, 2026
**Created by**: Claude Code Session
**Status**: Active POC Development - Ready for Real Implementation
