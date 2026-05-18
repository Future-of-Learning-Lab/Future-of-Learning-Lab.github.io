# Future of Learning Lab Website

Source repository for **[learning.cis.cornell.edu](https://learning.cis.cornell.edu)** -- the lab website for the Future of Learning Lab at Cornell University.

This is a static site hosted on **GitHub Pages**. There is no build step. When changes are pushed (or merged) to the `main` branch, the site updates automatically within a few minutes.

---

## Table of Contents

- [Repository Structure](#repository-structure)
- [Data File Formats](#data-file-formats)
- [How-To Guides](#how-to-guides)
  - [Add a New Member](#add-a-new-member)
  - [Move a Member to Alumni](#move-a-member-to-alumni)
  - [Remove a Member](#remove-a-member)
  - [Add / Edit / Remove a Publication](#add--edit--remove-a-publication)
  - [Add / Edit / Remove a Course](#add--edit--remove-a-course)
  - [Update a Member's Photo](#update-a-members-photo)
- [Approach A: GitHub Web UI (Beginner)](#approach-a-github-web-ui-beginner)
- [Approach B: Git CLI (Advanced)](#approach-b-git-cli-advanced)
- [Local Development](#local-development)
- [Tips and Troubleshooting](#tips-and-troubleshooting)

---

## Repository Structure

```
├── data/                    # JSON data files (the main files you'll edit)
│   ├── members.json         # Current lab members
│   ├── alumni.json          # Former lab members
│   ├── publications.json    # Publications list
│   ├── courses.json         # Courses taught
│   ├── projects.json        # Research projects
│   └── news.json            # News items on the homepage
│
├── img/                     # All images
│   ├── people/              # Member and alumni photos
│   ├── courses/             # Course images
│   ├── projects/            # Research project images
│   ├── brand/               # Lab and Cornell logos
│   └── ...
│
├── js/                      # JavaScript loaders (render JSON data on each page)
│   ├── members-loader.js
│   ├── alumni-loader.js
│   ├── publications-loader.js
│   ├── courses-loader.js
│   └── ...
│
├── components/              # Reusable HTML partials (navbar, footer, cards)
├── css/                     # Stylesheets
│
├── index.html               # Homepage
├── current-members.html     # Current members page
├── alumni.html              # Alumni page
├── publications.html        # Publications page
├── courses.html             # Courses page
├── research.html            # Research page
├── contact.html             # Contact page
│
├── CNAME                    # Custom domain config (do not edit)
├── serve.py                 # Local dev server (Python)
└── package.json             # Local dev server (Node)
```

**For most tasks you only need to edit files inside `data/` and add images to `img/`.** You should never need to touch the HTML, JS, or CSS files unless you are changing the site's layout or functionality.

---

## Data File Formats

Each data file is a JSON array (`[ ... ]`) of objects. Below are the exact fields for each type with copy-paste templates.

### Member / Alumni (`data/members.json` and `data/alumni.json`)

Both files use the same format. Each member is an object with these fields:

| Field      | Type   | Description                                                    |
|------------|--------|----------------------------------------------------------------|
| `name`     | string | Full name                                                      |
| `pronouns` | string | e.g. `"she/her"`. Use `""` if not provided.                   |
| `title`    | string | Role or affiliation (e.g. `"PhD Student in Information Science"`) |
| `image`    | string | Path to photo, e.g. `"img/people/jane-doe.jpg"`. Use `""` for no photo (a placeholder will be shown). |
| `email`    | string | Email address. Use `""` to omit.                               |
| `website`  | string | Personal website URL. Use `""` to omit.                        |
| `linkedin` | string | LinkedIn profile URL. Use `""` to omit.                        |

**Template -- copy and paste this into the JSON array:**

```json
{
  "name": "Jane Doe",
  "pronouns": "she/her",
  "title": "PhD Student in Information Science",
  "image": "img/people/jane-doe.jpg",
  "email": "",
  "website": "https://janedoe.com",
  "linkedin": ""
}
```

> Members appear on the site in the same order they are listed in the JSON file.

### Publication (`data/publications.json`)

| Field     | Type    | Description                                                        |
|-----------|---------|--------------------------------------------------------------------|
| `year`    | number  | Publication year (used to group and sort by year, descending)      |
| `title`   | string  | Paper title                                                        |
| `authors` | string  | Full author citation string, e.g. `"Smith, J., & Doe, A. (2025)."` |
| `details` | string  | Venue or journal, e.g. `"Proceedings of CHI 2025."`               |
| `link`    | string  | URL to the paper. Use `""` if not available.                       |
| `award`   | boolean | `true` to show an award badge, `false` otherwise                   |

**Template:**

```json
{
  "year": 2026,
  "title": "Your Paper Title Here",
  "authors": "Last, F., & Last, F. (2026).",
  "details": "Proceedings of the Conference Name (ABBREV 2026).",
  "link": "https://arxiv.org/abs/XXXX.XXXXX",
  "award": false
}
```

> Publications are automatically grouped by year and sorted with the most recent year first.

### Course (`data/courses.json`)

| Field         | Type   | Description                                                      |
|---------------|--------|------------------------------------------------------------------|
| `code`        | string | Course catalog code, e.g. `"INFO 4100/5101"`                    |
| `title`       | string | Course title                                                     |
| `professor`   | string | Instructor name(s)                                               |
| `description` | string | Course description text                                          |
| `image`       | string | Path to course image, e.g. `"img/courses/info-4100.jpg"`        |
| `links`       | array  | Array of link objects: `[{ "label": "Course Page", "link": "https://..." }]` |
| `tags`        | array  | Array of topic tag strings, e.g. `["AI", "Education"]`          |

**Template:**

```json
{
  "code": "INFO XXXX",
  "title": "Course Title",
  "professor": "Prof. Name",
  "description": "Course description here.",
  "image": "img/courses/course-image.jpg",
  "links": [
    { "label": "Course Page", "link": "https://classes.cornell.edu/..." }
  ],
  "tags": ["Tag1", "Tag2"]
}
```

---

## How-To Guides

Each task below is explained at a high level. See **Approach A** (GitHub Web UI) or **Approach B** (Git CLI) further down for the step-by-step mechanics of making and saving your edits.

### Add a New Member

1. **Upload a photo** to `img/people/`. Name the file in lowercase with hyphens (e.g. `jane-doe.jpg`).
2. **Open `data/members.json`** and add a new entry at the position where you want the person to appear. Copy the template from the [Member format section](#member--alumni-datamembersjson-and-dataalumnijson) and fill in the fields.
3. **Save / commit** your changes.

### Move a Member to Alumni

1. **Open `data/members.json`** and find the person's entry.
2. **Cut** (copy and then delete) their entire JSON object (from `{` to `}`), including the comma before or after it.
3. **Open `data/alumni.json`** and **paste** the entry into the array. Optionally update the `title` field to reflect their current role (e.g. `"Software Engineer, Google"`). You can clear the `pronouns` field to `""` if desired.
4. Make sure both files still have valid JSON (no trailing commas, no missing commas).
5. **Save / commit** your changes.

### Remove a Member

1. **Open `data/members.json`** (or `data/alumni.json`).
2. **Delete** the person's JSON object. Make sure there are no leftover commas that break the JSON.
3. Optionally delete their photo from `img/people/`.
4. **Save / commit** your changes.

### Add / Edit / Remove a Publication

- **Add:** Open `data/publications.json` and insert a new object using the [Publication template](#publication-datapublicationsjson). Place it anywhere in the array -- publications are sorted by `year` automatically.
- **Edit:** Find the publication by title and modify the relevant fields.
- **Remove:** Delete the entire object from `{` to `}` and fix any trailing commas.

### Add / Edit / Remove a Course

- **Add:** Open `data/courses.json` and insert a new object using the [Course template](#course-datacoursesjson). Upload a course image to `img/courses/`.
- **Edit:** Find the course by `code` and update the relevant fields.
- **Remove:** Delete the entire object and its associated image.

### Update a Member's Photo

- **Option 1 (same filename):** Replace the existing file in `img/people/` with the new photo using the exact same filename. No JSON change needed.
- **Option 2 (new filename):** Upload the new photo with a new name, then update the `image` field in `data/members.json` or `data/alumni.json` to point to the new file.

---

## Approach A: GitHub Web UI (Beginner)

No software installation required -- just a browser and a GitHub account with access to the repository.

### Editing a JSON File

1. Go to the repository on GitHub: [github.com/Future-of-Learning-Lab/future-of-Learning-Lab.github.io](https://github.com/Future-of-Learning-Lab/future-of-Learning-Lab.github.io)
2. Navigate to the file you want to edit (e.g. click `data` > `members.json`).
3. Click the **pencil icon** (Edit this file) in the top-right corner of the file view.
4. Make your changes in the editor. You can use the template from this README and paste it in.
5. Scroll down to **"Commit changes"**.
   - Write a short commit message describing what you changed (e.g. "Add Jane Doe to members").
   - Select **"Commit directly to the `main` branch"** for simple changes, or **"Create a new branch for this commit and start a pull request"** if you want someone to review first.
6. Click **"Commit changes"** (or **"Propose changes"**).
7. The site will update automatically within a few minutes.

### Uploading an Image

1. Go to the repository on GitHub.
2. Navigate to the target folder (e.g. `img/people/`).
3. Click **"Add file"** > **"Upload files"**.
4. Drag and drop your image or click "choose your files" to select it.
5. **Make sure the filename matches** what you referenced (or will reference) in the JSON. Use lowercase with hyphens and no spaces (e.g. `jane-doe.jpg`).
6. Write a commit message and commit to `main`.

### Deleting a File

1. Navigate to the file on GitHub.
2. Click the **trash can icon** (or click the `...` menu > "Delete file").
3. Commit the deletion.

### Editing Multiple Files in One Go

If you need to edit more than one file (e.g. upload a photo **and** edit a JSON file), you can either:
- Make two separate commits (one for the image upload, one for the JSON edit), or
- Use the GitHub **web editor**: press `.` (period key) on the repo page to open the full VS Code-style editor in your browser, make all changes, then commit them together.

---

## Approach B: Git CLI (Advanced)

This approach uses the command line and gives you full control, local preview, and the ability to batch multiple changes into a single commit.

### One-Time Setup

```bash
# Clone the repository
git clone https://github.com/Future-of-Learning-Lab/future-of-Learning-Lab.github.io.git
cd future-of-Learning-Lab.github.io
```

### Making Changes

```bash
# 1. Pull the latest version before making changes
git pull

# 2. Make your edits:
#    - Edit JSON files in data/ with any text editor
#    - Add images to img/people/, img/courses/, etc.

# 3. (Optional) Preview locally before pushing -- see "Local Development" below
python3 serve.py
# Open http://localhost:8000 in your browser

# 4. Stage your changes
git add data/members.json img/people/jane-doe.jpg    # add specific files
# or
git add .                                              # add everything

# 5. Commit with a descriptive message
git commit -m "Add Jane Doe to current members"

# 6. Push to GitHub
git push
```

The site will update automatically within a few minutes of pushing.

### Useful Commands

| Command                    | What it does                              |
|----------------------------|-------------------------------------------|
| `git status`               | See which files you've changed            |
| `git diff`                 | See exactly what changed                  |
| `git pull`                 | Download the latest changes from GitHub   |
| `git log --oneline -10`    | See the last 10 commits                   |
| `git checkout -- <file>`   | Undo uncommitted changes to a file        |

---

## Local Development

To preview the site locally, you need to run a local web server (the site uses `fetch()` which does not work with the `file://` protocol).

### Option 1: Python (Recommended)

```bash
python3 serve.py
```

Then open http://localhost:8000 in your browser.

### Option 2: Node.js

```bash
npm run serve
```

Or if you don't have npm:

```bash
npx serve . -p 8000
```

### Option 3: Python Built-in Server

```bash
python3 -m http.server 8000
```

### Option 4: VS Code Live Server

If you're using VS Code, install the "Live Server" extension and click "Go Live" in the status bar.

---

## Tips and Troubleshooting

**JSON must be valid.** A single misplaced comma or missing bracket will break the entire page that loads that file. Common mistakes:
- Trailing comma after the last item in an array: `..., }]` -- remove the comma before `}`
- Missing comma between objects: `} {` -- add a comma: `}, {`
- Unescaped quotes inside strings: use `\"` for literal quotes within a JSON string value

**Validate your JSON.** Before committing, paste your JSON into [jsonlint.com](https://jsonlint.com) to check for errors. Most code editors (VS Code, Cursor) also highlight JSON syntax errors.

**Image naming conventions:**
- Use lowercase filenames with hyphens: `jane-doe.jpg` (not `Jane Doe.jpg`)
- Avoid spaces in filenames -- they can cause issues with URLs
- Keep file sizes reasonable (under 500 KB) for fast page loads

**Order matters for members.** Members and alumni appear on the site in the exact order listed in their JSON file. Place new entries where you want them to show up.

**Changes go live on push.** There is no build step or approval gate (unless you use a pull request). Anything pushed to `main` is live on the website within a few minutes.

**When in doubt, use a pull request.** If you are unsure about a change, create a branch and open a pull request instead of committing directly to `main`. This lets someone else review before it goes live.
