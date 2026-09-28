# Entrance Dose

**Your Entrance. Your Future. Your Dose of Preparation.**

Entrance Dose is an entrance-exam preparation web app for students in Nepal. It covers CEE, IOE, Nursing, Paramedical, B.Pharm, BPH and engineering entrance exams. It combines a public marketing website, a student learning dashboard and an admin panel in one responsive app.

> **Status:** front-end prototype. Everything runs in the browser from a single HTML file with sample data. There is no backend yet (see [Limitations](#limitations)).

---

## Contents

- [Quick start](#quick-start)
- [Features](#features)
- [Pages and routes](#pages-and-routes)
- [Tech stack](#tech-stack)
- [Project structure](#project-structure)
- [Customising content](#customising-content)
- [Using the admin panel](#using-the-admin-panel)
- [Design system](#design-system)
- [Browser support](#browser-support)
- [Limitations](#limitations)
- [Road to production](#road-to-production)

---

## Quick start

No build step and no install.

1. Save the app as `index.html`.
2. Open it in a modern browser (see [Browser support](#browser-support)).

To serve it locally, for example to test on your phone over Wi-Fi:

```bash
# Python 3
python -m http.server 8080

# or Node
npx serve .
```

Then visit `http://localhost:8080`, or your computer's local IP address from your phone.

You can jump straight to any page with a hash URL, for example `index.html#catalog` or `index.html#admin`.

---

## Features

### Public website
- **Hero slider:** a rounded card slider with swipe, auto-advance every 6 seconds, dash indicators and hover arrows. It has three slides by default:
  - a "CEE Zone" promo;
  - the "Past Question Series — Final Push to CEE" banner;
  - the "STETH 7.0 — CEE Online Live Class" banner.
- **Separate layouts for desktop (2048×768) and mobile (750×660)** for every banner, so text stays readable on phones.
- **Stats section:** four pastel cards whose numbers count up when they scroll into view.
- **Courses section:** a 3-column grid on desktop. On mobile it becomes a swipeable stacked card deck with a counter, dots and previous/next buttons.
- **Course page:** a full curriculum shown as Subject → Chapter → Sub-chapter, with video counts and durations, free preview lessons, a notes tab, an overview tab and a price card that stays in view.
- **Courses, About and Contact pages**, each with its own route.
- **Contact form** that opens WhatsApp with the message already typed.
- **Tap-to-call and WhatsApp links** on banners, the contact page and the footer.

### Student app
- Home dashboard with a promo card that changes with the selected exam, exam shortcuts, continue learning, explore courses, recommendations and progress.
- Learn page with subject tabs, chapter progress and locks.
- Video lesson player with speed, quality, seek and fullscreen controls, plus tabs and an Up Next list.
- Practice mode supporting single-choice, multiple-correct, numerical and assertion–reason questions.
- Test series and a full exam simulator with a question palette, timer, mark for review and a submit confirmation.
- Test analysis: score ring, percentile, subject-wise and topic-wise results.
- Performance analytics with charts.
- AI study planner with a weekly timeline. Each day can be marked complete, skipped or rescheduled.
- Doubt solver that accepts text, photo or voice, with a step-by-step solution and follow-up questions.
- Notes, bookmarks, downloads, achievements, profile, pricing and faculty pages.
- Global search (press `/`), notifications and an exam switcher.

### Admin panel
- Dashboard with key numbers, charts and quick actions.
- **Hero Banners:** upload desktop and mobile images, link a banner to a course, reorder and delete.
- **Courses:** upload a poster, set the title, exam, price, original price, rating, "New" badge, description and features.
- **Question Bank:** filter, add questions and bulk-import from CSV or Excel. A template can be downloaded.
- Management tables for students, subjects, chapters, videos, mock tests, test series, faculty, doubts, subscriptions, payments, notifications and settings.

---

## Pages and routes

Routes are hash-based (`#route`).

| Route | Page | Area |
|---|---|---|
| `#landing` | Home: slider, stats, courses, CTA, footer | Public |
| `#catalog` | All courses with search, filter and sort | Public |
| `#coursepage` | Course detail and course content | Public |
| `#about` | About us | Public |
| `#contact` | Contact, form and FAQ | Public |
| `#auth` | Log in, sign up and 3-step setup | Public |
| `#home` | Student dashboard | App |
| `#learn`, `#course`, `#video` | Learning | App |
| `#practice`, `#tests`, `#exam`, `#result` | Practice and tests | App |
| `#performance`, `#plan`, `#doubts` | Analytics, planner, doubts | App |
| `#notes`, `#bookmarks`, `#downloads` | Library | App |
| `#achievements`, `#profile`, `#pricing`, `#faculty` | Account | App |
| `#admin` | Admin panel | Admin |

---

## Tech stack

| Part | Choice |
|---|---|
| Markup, styles, logic | Plain HTML, CSS and JavaScript in one file, with no framework |
| Rendering | Template-string functions and a single `render()` driven by a state object `S` |
| Graphics | Inline SVG for the logo, banners, posters, illustrations and charts |
| Fonts | Google Fonts: Inter (UI), Montserrat (banners), Anton (posters), Caveat (handwriting) |
| Excel import | SheetJS (`xlsx` 0.18.5) from cdnjs |

---

## Project structure

Everything is in `index.html`, split into sections by `/* ==== */` comments.

```
index.html
├── <head>
│   ├── Google Fonts
│   └── <style>
│       ├── Tokens (:root colours, fonts, shadows)
│       ├── Components (buttons, chips, cards, tabs, inputs)
│       ├── App shell (sidebar, top bar, bottom nav)
│       ├── Page styles (home, learn, video, practice, tests, result, plan…)
│       ├── Public site (hero slider, stats, course cards, course deck, CTA, footer)
│       └── Course page, public pages, nav and logo
└── <script>
    ├── Icons (I) and helpers: ic(), dic(), solid(), logo(), logoMark()
    ├── Data: EXAMS, SUBC, CH, QS, TESTS, FAC, NOTES, COURSES, BANNERS, CUR
    ├── State: S
    ├── Illustrations: board(), doctorG(), poster(), heroArt(), charts
    ├── App pages: home(), learn(), video(), practice(), tests(), examUI()…
    ├── Public pages: landing(), catalogPage(), aboutPage(), contactPage(), coursePage()
    ├── Banners: bannerD()/bannerM(), pqsD()/pqsM(), promoHTML()
    ├── Admin: adminShell(), courseAdmin(), bannerAdmin(), qbank()
    └── Router: go(), render()
```

> Some functions are declared more than once as features were added. In JavaScript the **last declaration wins**, so when editing a function, search for its **last** definition in the file.

---

## Customising content

| What | Where |
|---|---|
| Brand colours | `:root` CSS variables (`--blue`, `--navy`, `--purple`…) |
| Logo | `logoMark()` and `logo()` |
| Phone and WhatsApp numbers | Search for `9863423123`, `9708080189`, `9865821825` and `9702020389` |
| Courses | the `COURSES` array, or Admin → Courses |
| Course curriculum | `CUR`: subjects → chapters → sub-chapters |
| Hero slides | the `BANNERS` array, or Admin → Hero Banners |
| Promo slide text | `promoHTML()` |
| Stats numbers | the `ST` arrays in `landing()` and `aboutPage()` |
| Exams and subjects | `EXAMS` |
| Faculty | `FAC` |
| FAQ | the `Q` array in `contactPage()` |

### Course object

```js
{
  id: 1,
  t: 'NEXUS 7.0',            // title
  ex: 'CEE',                 // exam
  p: 7999, old: 10000,       // price and original price (NPR)
  r: 4.2, n: 20,             // rating and number of reviews (n = 0 shows "New")
  isNew: true,               // "NEW COURSE" badge
  desc: '…', feat: ['…'],    // description and features
  img: 'data:image/…',       // optional uploaded poster (replaces the built-in poster)
  poster: 'nexus'            // built-in poster: nexus | inhouse | pyq | ioe | nursing
}
```

### Banner object

```js
// Uploaded image banner
{ id, img: '<desktop 2048×768>', mimg: '<mobile image, optional>', alt: '…', course: 1 }

// Built-in designs
{ id, kind: 'promo' }                       // CEE Zone slide
{ id, kind: 'pqs', price, off, phone, … }   // Past Question Series
{ id, theme: 'blue', badge, ribbon, title, sub, feat, free, date, phone, books, person, course } // STETH style
```

---

## Using the admin panel

Open **Admin** from the footer, the student sidebar or `#admin`.

- **Hero Banners** → *Upload banner*: choose a desktop image (2048×768 px) and optionally a mobile image. Without a mobile image, the desktop image is shown in full on phones with bars around it. Pick the course it opens, then publish.
- **Courses** → *Upload course*: add a 16:9 poster (JPG or PNG, up to 5 MB), then fill in the details and features (one per line). The course appears straight away on the home page, the Courses page and the student dashboard.
- **Question Bank** → *Template* downloads the CSV format. *Import CSV/Excel* adds questions in bulk. Expected columns:
  `question, subject, chapter, topic, exam, year, difficulty, type, option_a, option_b, option_c, option_d, answer, explanation, tags`

---

## Design system

| Token | Value |
|---|---|
| Electric Blue (primary) | `#1769FF` |
| Deep Navy | `#07152F` |
| Dark Navy | `#0B1F3A` |
| Purple / Violet | `#673DE6` / `#7C4DFF` |
| Success / Warning / Danger | `#18C878` / `#FFB800` / `#FF4D5E` |
| Background / Card | `#F5F7FB` / `#FFFFFF` |
| Text / Secondary | `#101828` / `#667085` |

- Mobile-first breakpoints: `640`, `768`, `1024` and `1440` px.
- Mobile uses a bottom navigation bar. Desktop uses a navy sidebar.
- Accessibility work so far:
  - visible keyboard focus;
  - `aria` labels on icon buttons;
  - respects the reduced-motion setting;
  - inputs use a 16 px font on mobile so phones don't zoom in when you tap them.

---

## Browser support

| Browser | Version |
|---|---|
| Chrome / Edge | 105+ |
| Safari (macOS and iOS) | 16+ |
| Firefox | 110+ |

Container query units (`cqw`) are used in the hero promo slide and need the versions above.

---

## Limitations

This is a prototype. Before going live:

- **No backend.** Logins, enrolments, uploads, notes and test results live in memory and are **lost on refresh**.
- **Placeholder content.** Faculty names, ratings, student counts and prices are samples. The stats ("15 Million+" and so on) must be replaced with real figures.
- **QR code** on the STETH banner is decorative and won't scan. Replace it with your real payment QR or upload the real banner.
- **Offer text** ("1 Day Left", "Starting From Tomorrow", "Limited Seats Left", "36% Discount") is fixed text. Update or remove it when the offer ends.
- **AI features** (study planner, doubt solver) return sample answers and are not connected to a model.
- **Video player** is a visual demo and plays no real video.
- Pinch zoom is turned off on purpose, at the owner's request. This makes the site less accessible for users who need to zoom.

---

## Road to production

1. **Split the file.** Move to a component framework (React / Next.js or similar) and separate styles, data and pages.
2. **Backend and database.** Store users, courses, curriculum, banners, questions, test attempts and progress.
3. **Authentication.** Phone OTP and Google sign-in, with roles for student and admin.
4. **Media.** Upload images and videos to storage or a CDN, and stream video.
5. **Payments.** Connect a payment gateway and generate real payment QR codes.
6. **AI.** Connect the study planner and doubt solver to a language model API.
7. **Notifications.** SMS, push and WhatsApp reminders for classes and tests.
8. **Analytics and SEO.** Page metadata, sitemap and usage tracking.

---

© 2026 Entrance Dose. Made in Nepal for entrance aspirants.
