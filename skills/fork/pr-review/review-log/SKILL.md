---
name: review-log
description: Create and grow an HTML review log for a PR, starting with the overview page, then one section per review stop, then the final comments. Use when a guided review needs its overview page, a stop recorded, or its review journey finished.
---

Keep one HTML file per PR review, open beside the terminal in a preview pane. Path: `<repo root>/.scratch/reviews/pr-<n>.html`. If `.scratch/` is not ignored, add it to `.git/info/exclude`. Every time you create or change the log, print its full absolute path, starting with `/`, on its own line.

Start from [template.html](template.html). Edit in place with exact replacements at the marker comments, so earlier sections stay untouched. Escape `<`, `>` and `&` in code excerpts. Put `data-copy` on the parts the reviewer may paste back into the terminal or GitHub, as the template comments show; the page adds the copy buttons and the light/dark toggle itself.

## Page intent

The page is a **map** the reviewer glances at while sparring in the terminal, then the record of their review journey.

- The overview fits about one screen: why, tree, stops, flags. Detail lives in stop sections.
- The why is **scannable**: a one-sentence lead, then one-line items. Acceptance criteria read as plain-word bullets, each with its story id as a trailing tag. Copy the intent snippet's wording; it is already in Simplified Technical English.
- The tree comes first under the why, one small tree per stop, so the reviewer sees which files make up which concern before reading code.
- Accent marks only the why, `!` flags and `blocking` comments.
- One diagram per stop at most, and only where it carries what the excerpt cannot.
- The reviewer's explain-back goes in word for word.

## Operations

- **Create.** Copy the template. Fill the header (repo, PR number and title, mode, depth, date, and the log's absolute path in the eyebrow's `data-copy`), the why from the intent snippet, one Shape chunk per map-snippet chunk with `pr-lenses` flags merged in as `!` after the file they point at, the skip lines under the chunks, the stop list, the worth-a-look flags, and the not-checked list.
- **Append a stop.** Insert one `<section class="stop">` before `<!-- stops:end -->`: stop number and title, the excerpt shown, the test coverage (covered, gaps, weak, run status), the reviewer's explain-back word for word, candidate comments, and a diagram only when the guide asked for one. For a diagram, call the Skill tool with `diagram-design`, then copy only its `<svg>` element into the section.
- **Finish.** Fill the comments and open questions from `review-comments` at `<!-- finish -->`, and list the unvisited stops.

Done when the file renders with the new section and the full path has been printed.
