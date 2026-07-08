# AI-Native Enterprise Mini MBA Lab — Facilitator & Author Guide

This guide covers everything you need to set up, run, and maintain the course platform.

---

## 1. How the system is built (overview)

Your platform uses three pieces working together:

| Piece | Role | Where |
|-------|------|-------|
| **GitHub Pages** | Hosts the website files participants visit | github.com |
| **Firebase Realtime Database** | Stores live data (slide position, timers, scores) shared across everyone | console.firebase.google.com |
| **Your two files** | The platform itself + your content | On your computer |

The two files you work with:
- `course-platform.html` — the platform engine. You almost never edit this.
- `course-config.json` — all your content (slides, activities, rewards). This is the only file you edit.

**Key principle:** GitHub Pages serves the files; Firebase syncs the live data between facilitator and participants. Both must be set up (they already are).

---

## 2. One-time setup (already completed)

These were done during initial setup and do not need repeating:

1. Firebase project created (`ai-mba-lab`)
2. Realtime Database created (region: asia-southeast1)
3. Firebase config embedded into `course-platform.html`
4. Database security rules published (see Section 8)
5. Files hosted on GitHub Pages

If you ever need to rebuild on a new Firebase project, you would replace the `firebaseConfig` block near the top of the `<script>` section in `course-platform.html`.

---

## 3. The files: how to host them

The two files MUST live together in the same place.

**To deploy or update:**
1. Go to your repository on GitHub
2. Click **Add file → Upload files**
3. Upload files **individually** (not inside a folder) — drag `index.html` and `course-config.json` directly
4. Click **Commit changes** — your live site updates within 1–2 minutes

> Note: files must be at the **root level** of the repository. Do not upload them inside a subfolder or GitHub Pages won't find them.

**When you change content later:** edit `course-config.json` on your computer, then upload it to GitHub replacing the existing file. Changes go live within 1–2 minutes automatically. Your GitHub Pages URL never changes.

---

## 4. Editing your content: course-config.json

Open the file in any text editor (Notepad, TextEdit, VS Code). It has four top-level sections: `course`, `slides`, `activities`, `rewards`. The filename must always be exactly `course-config.json` — no spaces, no other name.

### 4a. Course settings
```json
"course": {
  "title": "AI-Native Enterprise Mini MBA Lab",
  "subtitle": "Building the AI-native organisation",
  "facilitator_password": "CHANGE-THIS",
  "cohort_id": "cohort-01"
}
```
- **title** — shown in the sidebar and browser tab
- **facilitator_password** — change from the default before any real session
- **cohort_id** — change this for each new group of participants (see Section 6)

---

### 4b. Slide types — complete reference

Every slide must have a unique `id` (e.g. `s1`, `s2`) and a `type`. Below is the full reference for every supported slide type.

---

#### TYPE: title
Opening slide or day-start slide. Centred layout with large heading.
```json
{
  "id": "s1",
  "type": "title",
  "title": "AI-Native Enterprise Mini MBA",
  "subtitle": "Business Foundations & Leadership in the AI Era",
  "body": "Welcome text shown below the subtitle.",
  "facilitator_notes": "Optional — only you see this."
}
```
- `title` — large heading (required)
- `subtitle` — smaller line below heading (optional)
- `body` — paragraph below subtitle (optional)

---

#### TYPE: text
Plain content slide for explanations, context, or narrative.
```json
{
  "id": "s2",
  "type": "text",
  "title": "Why This Programme Exists",
  "body": "Your content here. Can include HTML formatting tags.",
  "facilitator_notes": "Optional — only you see this."
}
```

---

#### TYPE: keypoints
Bullet-point slide. Each point gets a teal dot.
```json
{
  "id": "s3",
  "type": "keypoints",
  "title": "Three hallmarks of AI-native orgs",
  "points": [
    "Data infrastructure is a strategic asset",
    "AI is embedded in decision loops",
    "Employees are trained to work with AI"
  ],
  "facilitator_notes": "Optional — only you see this."
}
```
- `points` — array of strings, one per bullet (required)

---

#### TYPE: quote
Pull-quote slide. Displays a styled quotation with attribution.
```json
{
  "id": "s4",
  "type": "quote",
  "title": "Executive insight",
  "quote": "The question is no longer whether to use AI, but how fast you can rewire your operating model.",
  "attribution": "McKinsey Global Institute",
  "facilitator_notes": "Optional — only you see this."
}
```

---

#### TYPE: image
Displays an image with optional title and caption. Image auto-fits the screen — no scrolling.
```json
{
  "id": "s5",
  "type": "image",
  "title": "The AI maturity spectrum",
  "body": "Optional caption text below the image.",
  "image_url": "images/maturity-diagram.png",
  "facilitator_notes": "Optional — only you see this."
}
```
- `image_url` — use just the filename if the image is uploaded to GitHub alongside your files (e.g. `"images/diagram.png"`). Use a full URL if the image is hosted externally.
- Upload images to the `images/` folder in your GitHub repository.
- GitHub is case-sensitive: `Diagram.PNG` and `diagram.png` are different.
- Do NOT use SharePoint, OneDrive or Dropbox links — they point to viewer pages, not the raw image.
- Title and body are both optional — you can leave them empty (`""`) if you want a full-screen image with no text.

---

#### TYPE: video
Embeds a video player on the slide.
```json
{
  "id": "s6",
  "type": "video",
  "title": "What makes a company AI-native?",
  "description": "8 min · Module 1, Lesson 2",
  "video_url": "https://www.youtube.com/embed/VIDEO_ID",
  "facilitator_notes": "Optional — only you see this."
}
```
- `video_url` — MUST be the YouTube **embed** URL format, not the regular watch URL.
  - ✅ Correct: `https://www.youtube.com/embed/dQw4w9WgXcQ`
  - ❌ Wrong: `https://www.youtube.com/watch?v=dQw4w9WgXcQ`
- To get the embed URL: on YouTube, click Share → Embed → copy the `src` value from the iframe code.
- Set YouTube videos to **Unlisted** if they are private/internal.
- Vimeo embed URLs also work: `https://player.vimeo.com/video/VIDEO_ID`

---

#### TYPE: chart
Interactive bar chart where participants rate their organisation across multiple dimensions using sliders (0–10).
```json
{
  "id": "s7",
  "type": "chart",
  "title": "AI adoption maturity — your organisation",
  "description": "Rate your organisation in each area (0–10)",
  "axes": ["Strategy", "Data", "People", "Process", "Culture"],
  "facilitator_notes": "Optional — only you see this."
}
```
- `axes` — array of dimension labels (2–8 recommended)

---

#### TYPE: table
Fillable table where participants enter text in cells. Good for canvases, frameworks, and planning tools.
```json
{
  "id": "s8",
  "type": "table",
  "title": "AI opportunity canvas",
  "description": "Fill in the table for your department",
  "columns": ["Use case", "Business value", "Effort level", "Owner / next step"],
  "rows": [
    "Customer service automation",
    "Forecasting & planning",
    "Content generation",
    "Compliance monitoring"
  ],
  "facilitator_notes": "Optional — only you see this."
}
```
- `columns` — column headers (first column is read-only label, rest are fillable)
- `rows` — row labels shown in the first column

---

#### TYPE: static_table
Read-only informational table for presenting data, frameworks, comparisons, or reference material. Participants cannot edit it. Styled like an executive consulting presentation — purple header row, subtle alternating row colours, bold first column. Automatically sizes columns to content.
```json
{
  "id": "s39",
  "type": "static_table",
  "title": "Value Depends on Perspective",
  "description": "Businesses create value for multiple stakeholders, and each stakeholder defines value differently.",
  "columns": ["Stakeholder", "Examples of What They Value"],
  "rows": [
    ["Customers", "Better outcomes, convenience, quality, trust, lower cost, great experiences"],
    ["Shareholders", "Revenue growth, profitability, brand recognition, customer loyalty"],
    ["Employees", "Meaningful work, growth and development, recognition, wellbeing, fair compensation"],
    ["Partners", "Successful collaboration, reliability, shared growth, trust, mutual success"],
    ["Society", "Social impact, sustainability, ethical behaviour, responsible business practices"],
    ["Regulators", "Compliance, transparency, security, reduced risk, consumer protection"]
  ],
  "footer": "There is no single definition of value. Value depends on whose perspective you consider.",
  "facilitator_notes": "Optional — only you see this."
}
```
- `columns` — array of column header strings (required)
- `rows` — array of arrays; each inner array is one row, with one value per column (required)
- `description` — subtitle shown below the title (optional)
- `footer` — italicised note shown below the table in a purple accent box (optional). Use for key takeaways or caveats.
- `facilitator_notes` — private notes only the facilitator sees (optional)

**Key differences from `table` (interactive):**

| | `table` | `static_table` |
|---|---|---|
| Participant editing | ✅ Yes | ❌ No |
| Row format | Array of strings (labels only) | Array of arrays (full row data) |
| Use case | Workshops, canvases | Reference data, comparisons, frameworks |
| Footer field | ❌ No | ✅ Yes |

**Best practices:**
- Keep the first column short (1–2 words) — it renders bold and doesn't wrap
- Use `description` to frame the table before participants read it
- Use `footer` for the single most important takeaway from the table
- Ideal for: stakeholder maps, comparison matrices, process steps, role definitions, framework summaries
- Works best with 2–4 columns; beyond 4 columns may require horizontal scrolling on small screens

---

#### TYPE: activity
Links a slide to an activity defined in the `activities` array. The slide shows a waiting state until the facilitator launches it from the admin panel.
```json
{
  "id": "s9",
  "type": "activity",
  "activity_id": "a1"
}
```
- `activity_id` — must exactly match an `id` in the `activities` array

---

### 4b-i. Facilitator notes (optional, on any slide)

Add a `facilitator_notes` field to any slide. Only the facilitator sees it — participants never do. It appears in a yellow box below the slide on the facilitator's screen.

Use for: timing reminders, discussion questions, things to emphasise, expected answers, stories to tell.

```json
"facilitator_notes": "Spend ~5 min here. Ask: can anyone name a company that feels AI-native vs one that just bolted AI on?"
```

---

### 4b-ii. Formatting text (bold, colours, sizes, links)

Any text field (`body`, `subtitle`, `quote`, `description`, `facilitator_notes`) accepts HTML formatting tags. Use the in-app editor toolbar or add tags by hand.

| Effect | Tag to use | Example |
|--------|-----------|---------|
| Bold | `<b>…</b>` | `<b>important</b>` |
| Italic | `<i>…</i>` | `<i>emphasis</i>` |
| Underline | `<u>…</u>` | `<u>note</u>` |
| Line break | `<br>` | `line one<br>line two` |
| Paragraph space | `<br><br>` | `para one<br><br>para two` |
| Larger text (22px) | `<span style='font-size:22px'>…</span>` | |
| Largest text (28px) | `<span style='font-size:28px'>…</span>` | |
| Purple text | `<span style='color:#534AB7'>…</span>` | |
| Amber text | `<span style='color:#BA7517'>…</span>` | |
| Green text | `<span style='color:#1D9E75'>…</span>` | |
| Red text | `<span style='color:#A32D2D'>…</span>` | |
| Clickable link | `<a href='https://example.com' target='_blank'>link text</a>` | Opens in new tab |

**Brand colours:** purple `#534AB7` · amber `#BA7517` · green `#1D9E75` · red `#A32D2D`

> CRITICAL RULE: inside JSON strings, always use **single quotes** inside HTML tags (`style='color:red'`, `href='https://...'`). Never double quotes — double quotes break the JSON.

**Webpage link example:**
```json
"body": "Read the full report: <a href='https://www.mckinsey.com/report' target='_blank'>McKinsey AI Report</a>"
```

---

### 4c. Activity types — complete reference

Activities are defined in the `activities` array and linked to slides via `activity_id`. Every activity needs: `id`, `type`, `title`, `timer_seconds`, `coins`.

The facilitator launches each activity manually from the Admin panel. The timer starts immediately on all participant screens. When it expires, the activity locks — no late submissions.

---

#### ACTIVITY TYPE: multiple_choice
Single-question quiz. Awards full coins for correct answer, half coins for wrong answer.
```json
{
  "id": "a1",
  "type": "multiple_choice",
  "title": "Knowledge check",
  "timer_seconds": 60,
  "coins": 15,
  "question": "Which best describes an AI-native organisation?",
  "options": [
    "Uses AI only in IT",
    "AI embedded across all decision-making",
    "Has adopted cloud computing",
    "Sells AI products"
  ],
  "correct": 1
}
```
- `options` — array of answer strings (2–6 options)
- `correct` — the index of the correct answer, **0-based** (0 = first option, 1 = second, etc.)

---

#### ACTIVITY TYPE: fill_blank
Sentence completion. Awards coins proportionally based on how many blanks are correct.
```json
{
  "id": "a2",
  "type": "fill_blank",
  "title": "Fill in the blanks",
  "timer_seconds": 90,
  "coins": 25,
  "items": [
    { "text": "AI-native companies treat data as a strategic ___", "answer": "asset" },
    { "text": "The second stage of AI maturity is being ___ driven", "answer": "data" },
    { "text": "Employees focus on ___ and creativity", "answer": "judgment" }
  ]
}
```
- `items` — array of sentence objects, each with `text` and `answer`
- Use exactly `___` (three underscores) as the blank placeholder in the `text` field
- `answer` — the exact word expected (case-insensitive matching)

---

#### ACTIVITY TYPE: drag_drop
Participants drag items into the correct category zones. Awards coins proportionally.
```json
{
  "id": "a3",
  "type": "drag_drop",
  "title": "Sort into AI adoption stages",
  "timer_seconds": 120,
  "coins": 30,
  "zones": ["Exploring", "Scaling", "Transforming"],
  "items": [
    { "id": "d1", "text": "Pilot chatbot", "zone": "Exploring" },
    { "id": "d2", "text": "AI-led pricing engine", "zone": "Transforming" },
    { "id": "d3", "text": "Automated invoicing", "zone": "Scaling" },
    { "id": "d4", "text": "PoC recommender", "zone": "Exploring" },
    { "id": "d5", "text": "Enterprise AI governance", "zone": "Transforming" },
    { "id": "d6", "text": "Expanding to 5 BUs", "zone": "Scaling" }
  ]
}
```
- `zones` — array of category names (2–5 zones recommended)
- `items` — each item needs a unique `id`, a `text` label, and a `zone` that **exactly matches** one of the zone names (case-sensitive)

---

#### ACTIVITY TYPE: poll
Live opinion poll. All participants vote and see live percentage bars updating in real time. No right/wrong answer. Awards no coins. Results persist for the entire cohort.
```json
{
  "id": "p1",
  "type": "poll",
  "title": "Quick pulse check",
  "timer_seconds": 60,
  "coins": 0,
  "question": "Is your organisation ready to become AI-native?",
  "options": [
    "Yes, we are on our way",
    "Not yet, early stages",
    "No, we have not started"
  ]
}
```
- `coins` — must always be `0` for polls
- `options` — 2–6 answer options (works for Yes/No, Agree/Disagree, or multiple choice)
- Results stay visible after the timer expires — they are not cleared until a new cohort starts

---

### 4d. Rewards
```json
"rewards": [
  { "name": "Amazon gift card", "cost": 300, "description": "$30 AUD", "icon": "gift" },
  { "name": "Cafe voucher", "cost": 200, "description": "$20 at a café", "icon": "coffee" }
]
```
- `cost` — coins required to redeem
- `icon` options: `gift` `coffee` `book` `ticket` `award`
- Only the top 3 participants on the leaderboard can redeem rewards, and only after the facilitator clicks "End cohort & unlock rewards"

---

## 5. Running a session (daily workflow)

### Facilitator
1. Open the GitHub Pages URL
2. Click **"Facilitator? Switch to admin login"**
3. Enter your password → you land on the Admin panel
4. Use **Prev / Next** to move slides — every participant's screen follows you live
5. At an activity slide, click **Start** in the Activity launcher — a countdown begins on all screens
6. When the timer ends, the activity locks automatically
7. At day's end, just close the browser — your slide position is saved in Firebase

### Participants
1. Open the same URL
2. Enter their name → click Join
3. Their screen automatically shows whatever slide you are on
4. They cannot skip ahead or go back — they follow the facilitator
5. They complete activities within the timer to earn coins

### The pacing model
- You control the pace live. Some days you may cover 5 slides, some days 15 — it's up to the room.
- The next day, everyone resumes from exactly where you stopped.

### Editing slides inside the app (no JSON needed)
As facilitator, you can format and edit slide text visually without touching the JSON file:

1. On any content slide (title, text, image, keypoints, video, quote), click the **"Edit this slide"** button at the bottom-left of the screen
2. An editor opens with the slide's fields on the left and a **live preview** on the right
3. Select text and use the toolbar to apply **bold**, *italic*, underline, paragraph breaks, bigger text, or colours (purple, amber, green, red)
4. You can also type formatting tags directly, e.g. `<b>important</b>`
5. Two save options:
   - **Save for everyone (live)** — pushes the change to Firebase instantly; every participant sees the updated slide right away. Persists for the rest of the cohort.
   - **Export updated JSON** — downloads a fresh `course-config.json` with your edits baked in. Re-upload this to GitHub to make the change permanent across all future cohorts.

**Which save to use:** "Save for everyone" is perfect for quick live tweaks during a session. "Export updated JSON" is for making edits permanent — do this when you want the change to survive into new cohorts (since changing the `cohort_id` clears live overrides).

> Tip: when entering colours or sizes by hand in the JSON file, use **single quotes** inside the tag (`<span style='color:#534AB7'>`), never double quotes — double quotes break the JSON.

---

## 6. Cohorts: managing multiple groups (IMPORTANT)

The `cohort_id` in `course-config.json` is what separates one group from another. All data — slide position, scores, activity states — is stored under this ID in Firebase.

### The rules
- **Use ONE cohort_id for an entire group, for all their sessions (all 10 days).**
- **Keep it the same the whole way through** so coins accumulate across days.
- **Only change it when starting a brand-new group of people.**
- **Never change it mid-course** — doing so hides the current group's scores and resets everything.

### Example schedule
| Group | cohort_id | Duration |
|-------|-----------|----------|
| First group | `cohort-01` | All 10 days |
| Second group | `cohort-02` | All 10 days |
| Third group | `cohort-03` | All 10 days |

To start a new group: change `cohort_id`, save, re-upload `course-config.json` to GitHub. The new group begins with a clean slate automatically.

---

## 7. Coins, leaderboard & rewards rules

- **Coins** are earned by completing activities within the timer. Each activity can only be earned once.
- **Activities lock permanently** after their timer expires or after they've been started and time runs out. There is no re-earning — this protects scores already earned. (This is intentional; do not try to "reset" past activities.)
- **A participant appears on the leaderboard / Live participants list only after earning their first coins.** A logged-in participant with 0 coins will show "No participants yet" — this is normal.
- **Leaderboard** shows the top 3 on a podium, plus the rest listed below.
- **Coins accumulate across all session days** within a cohort — they never reset mid-course.
- **Rewards stay locked** until you click **"End cohort & unlock rewards"** on the final day.
- **Only the top 3** can redeem rewards. Positions 4+ see a locked rewards store.
- **Redemption** deducts coins and shows a confirmation. You (the facilitator) then issue the actual voucher/gift card manually outside the platform.

---

## 8. Security

The database uses scoped-open rules: anyone with the URL can read/write within the app's structure, but score values are validated (must be a number between 0 and 100,000) to prevent tampering, and the structure is locked to the cohorts path.

Current published rules (Firebase → Realtime Database → Rules):
```json
{
  "rules": {
    "cohorts": {
      "$cohortId": {
        ".read": true,
        ".write": true,
        "leaderboard": {
          "$player": {
            "coins": { ".validate": "newData.isNumber() && newData.val() >= 0 && newData.val() <= 100000" }
          }
        }
      }
    }
  }
}
```

This is appropriate for a private internal training course with no sensitive personal or financial data. The database URL is not published anywhere public. If you ever needed stronger security (e.g. external/public participants), the next step would be Firebase Anonymous Authentication with per-user write rules.

---

## 9. Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| Participant screen stuck on slide 1 | They're a participant; only the facilitator advances slides | Log in as facilitator in another window and click Next |
| Slides don't sync between windows | Firebase not connected, or different cohort_id | Check both are on the same URL and the same cohort_id |
| "No participants yet" despite someone logged in | They haven't earned coins yet | Have them complete an activity; leaderboard tracks scorers only |
| Leaderboard empty | No scores written yet, or wrong cohort_id | Complete an activity; verify cohort_id matches in config |
| Activities all show "Locked" on a fresh group | Leftover activity states from earlier testing in that cohort | In Firebase → Data, delete the `activities` node under that cohort, OR use a fresh cohort_id |
| Activity timer shows 0s when participant opens slide | They joined after the timer already expired | Start the activity again only if appropriate; otherwise it stays locked (by design) |
| "Connecting…" hangs forever | Firebase config or rules problem | Verify rules are published and the firebaseConfig in the HTML is correct |
| Images don't load | Broken URL, placeholder, or image file not uploaded to GitHub | Upload image files to the `images/` folder in your GitHub repository — see Section 4b-i |
| GitHub not showing new content | File uploaded inside a folder, or old cached version | Upload files directly to root (not inside a subfolder); hard refresh browser (Ctrl+Shift+R) |

### Clearing test data
To wipe everything and start completely fresh: Firebase Console → Realtime Database → Data → hover over the top-level `cohorts` node → click the red X → confirm.

---

## 10. Pre-launch checklist (first real session)

- [ ] `cohort_id` set to your first real group (e.g. `cohort-01`)
- [ ] `facilitator_password` changed from the default
- [ ] Real slides and activities entered in `course-config.json`
- [ ] Real image URLs (not placeholders) — see Section 4b-i — image files uploaded to `images/` folder in GitHub; YouTube EMBED URLs used for videos
- [ ] Reward names, costs, descriptions finalised
- [ ] `index.html` and `course-config.json` uploaded to GitHub repository root
- [ ] Firebase test data cleared (delete `cohorts` node)
- [ ] One full dry run completed solo (play both facilitator and participant)
- [ ] Participants told to use Chrome for best reliability
- [ ] GitHub Pages URL ready to share (Settings → Pages to find it)

---

## 11. Quick reference

**Facilitator login:** open URL → "Switch to admin login" → enter password
**Advance slides:** Admin panel → Prev / Next (syncs to everyone live)
**Run an activity:** Admin panel → Activity launcher → Start
**End the course:** Admin panel → End cohort & unlock rewards
**New group:** change `cohort_id` in config → re-upload folder
**Update content:** edit `course-config.json` → re-upload folder
**Wipe data:** Firebase → Data → delete `cohorts` node
