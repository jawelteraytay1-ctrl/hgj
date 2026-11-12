# 📚 BookFinder – Online Book Discovery System

BookFinder is a single-page web app built with **React** and **Vite**. You can use it to **discover, search, explore and save books**. All book information comes live from the free public **[Open Library API](https://openlibrary.org/developers/api)**. The app contains no hard-coded sample data.

---

## Table of contents

1. [Project overview](#project-overview)
2. [Main features](#main-features)
3. [Technologies used](#technologies-used)
4. [API information](#api-information)
5. [Installation & setup](#installation--setup)
6. [How to run the application](#how-to-run-the-application)
7. [Project folder structure](#project-folder-structure)
8. [Application architecture](#application-architecture)
9. [GitHub collaboration workflow](#github-collaboration-workflow)
10. [Team member contributions](#team-member-contributions)

---

## Project overview

BookFinder works like a modern online bookstore or digital library:

- **Discover / Home (`/`)**: search Open Library by title, author or keyword. Results appear in a responsive grid of book cards that you can filter, sort and page through.
- **Book Details (`/book/:workId`)**: shows the full record for one book: large cover, title, authors, first publication year, number of editions, description, subjects, places, time periods and a link to the original Open Library page.
- **Favorites (`/favorites`)**: lists every book you saved. Favorites are stored in the browser's `localStorage`, so they are still there after you refresh or reopen the browser.

The app runs entirely in the browser. It has **no backend, no database and no PHP/Laravel**.

## Main features

| Feature | Details |
| --- | --- |
| 🔎 **Search** | Search by **all fields (keyword)**, **title** or **author**. Popular-search chips give one-click searches. The search is stored in the URL, so results can be bookmarked and shared, and the browser Back button works. |
| 🗂️ **Filtering** | Filter by **first publication year** (2020–present, 2010s, 2000s, 1990s, 1980s, 1950–1979, 1900–1949, before 1900). |
| ↕️ **Sorting** | **Most relevant**, **Title (A–Z)**, **Newest first**, **Oldest first**. |
| 📄 **Pagination** | **12 books per page**, **Previous / Next** buttons and a **"Page X of Y"** indicator. The buttons are disabled at the first and last page, and a page number in the URL that is out of range is moved back to the last valid page. |
| ❤️ **Favorites** | Add or remove favorites from any card or from the details page. They are saved in `localStorage` (key `bookfinder:favorites`), loaded when the app starts, synced between open tabs, and counted in a badge in the navigation bar. |
| ⏳ **Loading states** | An animated "book shelf" spinner appears while data is being fetched. |
| ⚠️ **Error handling** | Clear messages for being offline or having network problems, Open Library server errors, invalid responses, invalid book IDs and books that don't exist, each with a **Retry** button where retrying makes sense. |
| 🫙 **Empty states** | Shows "No books found. Try another search." when a search has no results, and offers to clear the year filter. The Favorites page has its own empty state. |
| 🖼️ **Missing data** | When a cover image is missing or broken, a styled placeholder is shown instead. Missing authors, years, descriptions and subjects show friendly fallback text. |
| 📱 **Responsive UI** | Layouts for desktop, tablet and mobile in a green / cream / white library theme, with hover effects and transitions. Respects the reduced-motion setting. |
| ♿ **Accessibility** | Semantic HTML, labelled form controls, `aria-pressed` on favorite buttons, a skip link, visible focus outlines and live regions for loading and paging updates. |

## Technologies used

- **React 18**: functional components and hooks (`useState`, `useEffect`, `useContext`, `useCallback`, `useMemo`, `useRef`, `useId`)
- **React Router DOM 6**: client-side routing (`BrowserRouter`, `Routes`, `useParams`, `useSearchParams`, `useNavigate`)
- **JavaScript (ES6+)**: modules, async/await, destructuring, optional chaining
- **Fetch API** with `async/await` and `AbortController` (cancels requests that are no longer needed)
- **HTML5 & CSS3**: CSS custom properties, Grid, Flexbox, media queries, animations
- **Browser `localStorage`**: stores favorites so they persist
- **Vite 5**: development server and production build
- **Git & GitHub**: feature-branch workflow with Pull Requests

## API information

| | |
| --- | --- |
| Provider | [Open Library](https://openlibrary.org) (Internet Archive) |
| Documentation | https://openlibrary.org/developers/api · [Search API](https://openlibrary.org/dev/docs/api/search) · [Covers API](https://openlibrary.org/dev/docs/api/covers) |
| Search | `https://openlibrary.org/search.json?q=harry+potter&page=1&limit=12` |
| Work details | `https://openlibrary.org/works/{WORK_ID}.json` (e.g. `OL27448W`) |
| Authors (fallback) | `https://openlibrary.org/authors/{AUTHOR_ID}.json` |
| Covers | `https://covers.openlibrary.org/b/id/{COVER_ID}-L.jpg` (sizes `S`, `M`, `L`) |

**How BookFinder uses the API** (see `src/services/openLibrary.js`):

- Search by title uses `title=`, by author uses `author=`, and by keyword uses `q=`.
- The year filter adds a range clause to `q`, for example `first_publish_year:[2000 TO 2009]`.
- Sorting uses `sort=title | new | old`. If no `sort` is sent, results come back by relevance.
- Pagination uses `page=` and `limit=12`. Filtering, sorting and paging all happen on the server, so they apply to the **whole result set** and stay consistent from page to page.
- Only the needed fields are requested (`fields=key,title,author_name,...`) to keep responses small.

### 🔑 No API key or environment variables needed

The Open Library endpoints used here are free and public. They **do not need an API key, an account or any authentication**, so the project has **no `.env` file and no environment variables**. Clone it, install it and run it.

## Installation & setup

### Prerequisites

- [Node.js](https://nodejs.org/) **18 or newer** (includes npm)
- [Git](https://git-scm.com/)
- An internet connection (the app loads its data live from Open Library)

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/bookfinder-react.git

# 2. Move into the project folder
cd bookfinder-react

# 3. Install dependencies
npm install
```

That's it. No configuration files need to be created.

## How to run the application

```bash
# Start the development server (opens http://localhost:5173)
npm run dev

# Create an optimized production build in /dist
npm run build

# Preview the production build locally
npm run preview
```

> **Deploying:** the app uses client-side routing (`BrowserRouter`). If you host the `dist/` folder on a static host, configure it to serve `index.html` for unknown paths. On Netlify, add a `_redirects` file containing `/* /index.html 200`; on Vercel, routing is handled automatically. Without this, refreshing on `/favorites` returns a 404.

## Project folder structure

```
bookfinder-react/
├── .github/
│   └── pull_request_template.md   # PR checklist for the team
├── public/
│   └── favicon.svg
├── src/
│   ├── assets/
│   │   └── logo.svg
│   ├── components/
│   │   ├── BookCard.jsx           # one book: cover, title, author, year, actions
│   │   ├── BookCover.jsx          # cover image with graceful placeholder
│   │   ├── BookGrid.jsx           # responsive grid of BookCards
│   │   ├── ErrorMessage.jsx       # error / empty-state panel with Retry
│   │   ├── FavoriteButton.jsx     # add/remove favorite toggle
│   │   ├── FilterSort.jsx         # publication-year filter + sort selector
│   │   ├── Header.jsx             # logo + navigation + favorites badge
│   │   ├── Icons.jsx              # inline SVG icons
│   │   ├── Loading.jsx            # loading animation
│   │   ├── Pagination.jsx         # Previous / Next + "Page X of Y"
│   │   └── SearchBar.jsx          # search input + search type + button
│   ├── context/
│   │   └── FavoritesContext.jsx   # global favorites state + localStorage
│   ├── hooks/
│   │   ├── useBookDetails.js      # fetch one work (with retry)
│   │   ├── useBookSearch.js       # fetch search results (with retry)
│   │   └── useDocumentTitle.js    # browser tab titles
│   ├── pages/
│   │   ├── BookDetails.jsx        # /book/:workId
│   │   ├── Favorites.jsx          # /favorites
│   │   ├── Home.jsx               # /
│   │   └── NotFound.jsx           # any unknown route
│   ├── services/
│   │   └── openLibrary.js         # all Open Library API calls + error handling
│   ├── App.jsx                    # layout + routes
│   ├── main.jsx                   # entry point (Router + FavoritesProvider)
│   └── styles.css                 # global styles and responsive rules
├── .gitignore
├── index.html
├── package.json
├── README.md
└── vite.config.js
```

## Application architecture

```
main.jsx
 └─ <BrowserRouter>
     └─ <FavoritesProvider>          ← favorites state, persisted to localStorage
         └─ <App>
             ├─ <Header>             ← reads favorites count from context
             └─ <Routes>
                 ├─ "/"              → <Home>        → useBookSearch()  → services/openLibrary.searchBooks()
                 ├─ "/book/:workId"  → <BookDetails> → useBookDetails() → services/openLibrary.getBookDetails()
                 ├─ "/favorites"     → <Favorites>   → useFavorites()
                 └─ "*"              → <NotFound>
```

- **The search state lives in the URL** (`?q=…&type=…&sort=…&year=…&page=…`). Home reads it with `useSearchParams`, so refreshing the page, sharing a link or pressing the browser Back button all bring back the exact same results.
- **Data fetching** is handled by custom hooks. Each hook cancels its previous request with `AbortController` when the inputs change, and exposes a `retry()` function used by the Retry buttons.
- **All API code** lives in `services/openLibrary.js`. That file turns raw API responses into a consistent book object (`{ workId, title, authors, year, coverId }`) and converts every failure into a typed `ApiError` (`offline`, `network`, `not-found`, `invalid-id`, `invalid-data`, `server`). The UI uses that type to choose the message to show.

## GitHub collaboration workflow

This project is developed by a **two-member team**. Nobody commits directly to `main`: every change goes through a feature branch and a Pull Request.

### 1. One-time repository setup (repository owner)

```bash
git init -b main
git add .gitignore README.md
git commit -m "chore: initial project setup"
git remote add origin https://github.com/<your-username>/bookfinder-react.git
git push -u origin main
```

Then, on GitHub:

1. Make the repository **Public**.
2. **Settings → Collaborators → Add people**: invite your teammate.
3. **Settings → Branches → Add branch protection rule** for `main`: turn on *Require a pull request before merging* and *Require approvals (1)*. This stops anyone from pushing directly to `main`.

### 2. Branch naming

| Branch | Purpose |
| --- | --- |
| `feature/api-fetch` | Open Library service, search/details hooks, loading and error components |
| `feature/search-bar` | SearchBar component and search types |
| `feature/book-details` | Book Details page and routing |
| `feature/favorites` | FavoritesContext, localStorage and the Favorites page |
| `feature/pagination` | Pagination, filtering and sorting |
| `style/responsive-ui` | Global styles and the responsive layout |
| `docs/readme` | Documentation |

### 3. Daily workflow

```bash
# Always start from an up-to-date main
git checkout main
git pull origin main

# Create a branch for your task
git checkout -b feature/search-bar

# Work, then commit in small, descriptive steps
git add src/components/SearchBar.jsx
git commit -m "feat: add book search functionality"

# Push the branch and open a Pull Request on GitHub
git push -u origin feature/search-bar
```

On GitHub: **Compare & pull request**, then fill in the PR template. Your **teammate reviews and approves** it, and you merge it with **"Squash and merge"** or **"Create a merge commit"**. After that, delete the branch.

Keeping a long-running branch up to date:

```bash
git checkout feature/search-bar
git pull origin main          # or: git rebase origin/main
# resolve any conflicts, then
git push
```

### 4. Commit message convention ([Conventional Commits](https://www.conventionalcommits.org/))

| Prefix | Use for | Example |
| --- | --- | --- |
| `feat:` | a new feature | `feat: integrate Open Library API` |
| `fix:` | a bug fix | `fix: resolve API loading errors` |
| `style:` | styling / layout only | `style: improve responsive layout` |
| `refactor:` | code changes that are not new features or fixes | `refactor: extract BookCover component` |
| `docs:` | documentation | `docs: update README` |
| `chore:` | tooling, configuration, dependencies | `chore: configure Vite` |

### 5. Suggested order of Pull Requests

1. `chore: initial project setup` (Vite, React, Router, folder structure)
2. `feature/api-fetch`: `feat: integrate Open Library API`
3. `feature/search-bar`: `feat: add book search functionality`
4. `feature/book-details`: `feat: add book details page`
5. `feature/favorites`: `feat: implement favorites using localStorage`
6. `feature/pagination`: `feat: add pagination, filtering and sorting`
7. `style/responsive-ui`: `style: improve responsive layout`
8. `docs/readme`: `docs: update README`

## Team member contributions

> ✏️ Fill in your names, GitHub usernames and the PRs each of you authored. The PR links on GitHub are the evidence of who did what.

| Member | GitHub | Responsibilities | Branches / PRs |
| --- | --- | --- | --- |
| **Member 1** – _Name_ | `@username1` | Open Library API service, search & details hooks, Home page, search bar, pagination, filtering and sorting | `feature/api-fetch`, `feature/search-bar`, `feature/pagination` |
| **Member 2** – _Name_ | `@username2` | Book Details page, FavoritesContext & localStorage, Favorites page, responsive styling, README | `feature/book-details`, `feature/favorites`, `style/responsive-ui`, `docs/readme` |

Both members review each other's Pull Requests before merging.

---

**Data source:** Book data and cover images are provided by [Open Library](https://openlibrary.org), a project of the Internet Archive.
