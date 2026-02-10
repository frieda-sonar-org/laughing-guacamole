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
- `--color-accent-primary`: #4b9fd8 (Blue - primary actions)
- `--color-accent-secondary`: #4b9fd8 (Blue - icons, links)
- `--color-link`: #4b9fd8

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
  - "Review changes" button with dropdown modal
  - Review options: Comment, Request changes, Approve
- ✅ **Files View** (default and only view):
  - Left sidebar: Groups panel showing file groups
  - Right content: File change cards with:
    - File metadata and statistics
    - Code diff viewer with syntax highlighting (YAML)
    - Inline comment capability (UI only)
    - Expandable/collapsible sections

**Design Notes**:
- Context tab has been removed to focus solely on file review
- All card components use transparent backgrounds with borders
- Scrollbar always visible to prevent layout shift

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
- `showGroupFiles`: Toggle file list visibility
- `showFileChanges`: Toggle file changes section
- `showReviewModal`: Control review dropdown visibility
- `reviewType`: Selected review type (comment/request changes/approve)
- `reviewComment`: Review comment text

**Sections**:
1. **Top Navigation & Sidebar** (same as homepage)

2. **Page Header**
   - Breadcrumb with PR number
   - Title with version hashtag
   - "Review changes" button

3. **Review Modal**
   - Three review types with radio selection
   - Comment textarea
   - Submit button

4. **Files View**
   - Groups sidebar (left):
     - Shows file groupings
     - Group names and file counts
   - File changes (right):
     - File metadata cards
     - Code diff tables with:
       - Line numbers
       - Add/remove indicators
       - Syntax-highlighted code
       - Inline comment buttons (on hover)

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

### Responsive Design
- Desktop: Full sidebar and all columns visible
- Tablet (< 1024px): Condensed navigation, smaller sidebar
- Mobile (< 768px): Collapsible sidebar, simplified layout

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
- ✅ Collapsible file sections
- ✅ Review modal with multiple review types
- ✅ Client-side navigation

### Planned Features
- [ ] Additional pages (Summary, Issues, Dashboard, etc.)
- [ ] Interactive search and filtering
- [ ] Real data integration (SonarQube API)
- [ ] User authentication
- [ ] Functional inline comment submission
- [ ] File and group review workflow (mark as reviewed)
- [ ] Coverage indicators with visual metrics
- [ ] Real-time collaboration features

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

**Last Updated**: February 10, 2026
**Created by**: Claude Code Session
**Status**: Active POC Development
