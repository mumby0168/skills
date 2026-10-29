---
name: review-log
description: Create and grow an HTML review log for a PR, starting with the overview page, then one section per review stop, then the final comments. Use when a guided review needs its overview page, a stop recorded, or its review journey finished.
---

Keep one HTML file per PR review, open beside the terminal in a preview pane. Path: `<repo root>/.scratch/reviews/pr-<n>.html`. If `.scratch/` is not ignored, add it to `.git/info/exclude`. Always give the reviewer the full absolute path.

Start from [template.html](template.html). Edit in place with exact replacements at the marker comments, so earlier sections stay untouched. Escape `<`, `>` and `&` in code excerpts.

## Page intent

The page is a **map** the reviewer glances at while sparring in the terminal, then the record of their review journey.

- The overview fits about one screen: why, tree, stops, flags. Detail lives in stop sections.
- The tree comes first under the why; the reviewer reads shape before code.
- Accent marks only the why, `!` flags and `blocking` comments.
- One diagram per stop at most, and only where it carries what the excerpt cannot.
- The reviewer's explain-back goes in word for word.

## Operations

- **Create.** Copy the template. Fill the header (repo, PR number and title, mode, depth, date), the why from the intent snippet, the tree from the map snippet with `pr-lenses` flags merged in as `!` after the stop number, the stop list, the worth-a-look flags, and the not-checked list.
- **Append a stop.** Insert one `<section class="stop">` before `<!-- stops:end -->`: stop number and title, the excerpt shown, the reviewer's explain-back word for word, candidate comments, and a diagram only when the guide asked for one. For a diagram, call the Skill tool with `diagram-design`, then copy only its `<svg>` element into the section.
- **Finish.** Fill the comments and open questions from `review-comments` at `<!-- finish -->`, and list the unvisited stops.

Done when the file renders with the new section and the path has been given.
