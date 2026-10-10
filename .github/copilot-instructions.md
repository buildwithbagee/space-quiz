# Copilot instructions

## Project shape

The quiz is a self-contained static page in `index.html`: its question data, markup, CSS, and browser-side JavaScript all live in that file. It has no package manager, framework, server, or external runtime dependencies; open the HTML file directly in a browser.

The script owns quiz state (`current`, `score`, and `answered`) and renders each question into the existing page. Answer selection updates score, progress, feedback, and button states; advancing renders the next question or replaces the quiz view with results. Restart resets the state and returns focus to the first question.

## Commands and checks

There is no configured build, lint, or automated test runner. Node.js can syntax-check the inline JavaScript without creating files:

```sh
node --check <(sed -n '/<script>/,/<\/script>/p' index.html | sed '1d;$d')
```

For a focused browser check, open `index.html`, verify one correct answer and one incorrect answer update feedback and score as expected, then finish the quiz and use **Play again** to verify results and reset behavior. A complete pass checks all ten answers and the progress indicator.

## Conventions

- Keep the application self-contained in `index.html`; do not add a build system or external libraries for ordinary quiz changes.
- Add or edit quiz content in the `questions` array. Each item uses `category`, `question`, `choices`, and a zero-based `answer` index into `choices`.
- Keep display text inserted as text when rendering user-facing question and choice content. Reuse the existing rendering and state-update flow so answer locking, scoring, progress, and feedback stay consistent.
- Theme colors are CSS custom properties in `:root` with dark-mode overrides under `prefers-color-scheme: dark`. Use these tokens for new UI colors so both themes remain coherent.
- Preserve native button interaction, heading structure, live feedback/progress semantics, visible focus styles, and the existing reduced-motion support when changing the UI or interaction flow.
