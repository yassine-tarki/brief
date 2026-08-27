# Brief

A one-page resume / personal site template where the **same single page** is both the web
version and the printed PDF.

There is no separate "download my resume" file to keep in sync. You edit `index.html`,
you open it in a browser, and `Cmd+P` / `Ctrl+P` gives you a clean, properly paginated
document with the site chrome stripped out and everything set in black on white.

## What's in it

```
brief/
├── index.html    the resume itself — this is the file you edit
├── style.css     screen styles, responsive rules, and the print stylesheet
├── README.md
├── LICENSE
└── .gitignore
```

Two files. No build step, no `npm install`, no dependencies, no network requests —
`index.html` and `style.css` are the whole thing.

## Running it

Open the file directly:

```sh
open index.html          # macOS
xdg-open index.html      # Linux
start index.html         # Windows
```

Double-clicking it in a file manager works just as well. `file://` is fine — nothing
in the page needs a server.

If you would rather serve it (useful when you want a shareable localhost URL):

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Exporting the PDF

1. Open `index.html` in Chrome, Edge, Safari or Firefox.
2. Press `Cmd+P` (macOS) or `Ctrl+P` (Windows/Linux), or click **Save as PDF** in the
   toolbar at the top of the page.
3. Choose **Save as PDF** as the destination.
4. In the print dialog:
   - **Margins:** Default. The stylesheet sets its own `@page` margins (14 mm), so
     letting the browser use the default is what you want. Choosing "None" will make
     the text run to the edge of the sheet.
   - **Scale:** 100% / Default. Do not use "Fit to page".
   - **Headers and footers:** off, unless you want the browser's date and URL stamped
     on the page.
   - **Background graphics:** not needed. The print styles are black on white by design.
5. Paper size is deliberately left to you — the stylesheet does not force `A4` or
   `Letter`, and the margins are chosen to sit comfortably on both.

Chrome and Edge give the most faithful result; Safari is very close. Firefox honours the
page-break rules but sometimes spaces the margins slightly differently.

## Customising it

### The content

Everything is in `index.html`, in plain semantic HTML, in the order it appears on the
page: header, summary, experience, projects, skills, education. The sample content
belongs to a fictional person, Alex Rivera — replace it with your own.

- **Name, title, contacts** live in `<header class="masthead">`. The email is a real
  `mailto:` link and the phone a real `tel:` link; keep them that way so they stay
  clickable on the web version.
- **A job** is one `<article class="entry">` containing an `.entry-head` (role,
  employer, dates, location) and a `<ul class="bullets">`. Copy one and edit it to add
  another.
- **Projects and education** use `<article class="entry entry-compact">`, which is the
  same block with a prose paragraph instead of bullets and tighter spacing.
- **Skills** are `<div class="skill-row">` pairs inside a `<dl>` — a `<dt>` category and
  a `<dd>` list.
- **Reordering sections** is a matter of moving the `<section>` blocks. Nothing depends
  on their order.
- **Removing a section** — delete the whole `<section>`. Nothing else references it.

Also update the `<title>`, the `description` and `author` meta tags at the top of the
file, and the `lang` attribute on `<html>` if you are not writing in English.

### The look

The palette, fonts and page width are CSS custom properties at the top of `style.css`,
under `:root`. Changing `--accent` restyles every link and employer name. `--measure`
controls how wide the page is on screen. `--font-serif` is used only for your name.

Dark mode is a second block, `:root[data-theme="dark"]`, with the same set of variables.
The toggle in the toolbar sets that attribute and remembers the choice in
`localStorage`; the page follows your operating system's preference on first visit.
None of this affects the PDF — the print stylesheet overrides both palettes back to
black on white.

### The print stylesheet

Section 11 of `style.css`. The rules worth knowing about if you are adjusting it:

- `@page { margin: 14mm 14mm 16mm; }` — the sheet margins. No `size` is declared, on
  purpose, so the paper chosen in the print dialog is respected.
- `.entry { break-inside: avoid; }` — a job or project is never split across two pages.
- `.section-heading`, `.entry-head` and every heading use `break-after: avoid-page`, so
  a heading is never left stranded at the bottom of a page without its content.
- `p, li, dd { orphans: 3; widows: 3; }` — no single dangling line at a page boundary.
- `.no-print` hides an element on paper. Add the class to anything you want on the web
  version only.
- Links print as plain black text with no underline and no expanded URL, because the
  addresses are already written out in the header.
- Body text is `10.5pt` with the sections stepping down from there. If your content runs
  a few lines past a page boundary, nudging this to `10pt` usually pulls it back.

## Fitting on one page

If you want a strict one-pager and are slightly over:

- Trim bullets to three or four per role — the older roles rarely need more.
- Shorten the summary; two or three sentences is plenty.
- Drop a project, or the `.colophon` footer.
- Reduce the print `font-size` on `html, body` from `10.5pt` to `10pt`, and
  `.section { margin-top }` from `13pt` to `10pt`.

Conversely, the layout is happy to run to two or three pages — the page-break rules are
written for a multi-page document, not just a single sheet.

## Applicant tracking systems

The markup is plain semantic HTML: one `<h1>` for your name, `<h2>` per section,
`<h3>` per role, real `<section>` and `<article>` elements, `<time>` on dates, and an
`<address>` block with `mailto:`/`tel:` links. There are no images of text, no icon
fonts standing in for words, no multi-column layout, and no text baked into CSS
`content` beyond decorative separators. PDFs exported from the browser keep this as
selectable, copyable text in reading order.

That is the reason the markup is shaped this way, not a guarantee about any particular
vendor's parser — every ATS behaves a little differently, and the only way to know is to
export the PDF and copy the text out of it to see what comes through.

## Accessibility

Headings nest properly, the toolbar buttons carry `aria-pressed` and `aria-label`,
focus is visible on every interactive element, and the whole page is usable from the
keyboard. Colour contrast in both themes clears WCAG AA for body text.
`prefers-reduced-motion` is honoured.

## Browser support

Any current version of Chrome, Edge, Safari or Firefox. The layout uses CSS grid,
flexbox, custom properties and `color-mix()`; `color-mix()` is only used for two
decorative surfaces and degrades to a transparent value in browsers without it.

## License

MIT — see [LICENSE](LICENSE). Use it for your own resume, change whatever you like,
no attribution needed.
