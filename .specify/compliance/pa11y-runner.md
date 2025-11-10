# Pa11y integration & runner

This file describes how to run the pa11y accessibility checks locally and how the CI workflow (`.github/workflows/pa11y.yml`) runs them.

Quick overview
- The workflow uses `pa11y-ci` and the repository-level config `.pa11yci`.
- Update `.pa11yci` with the URLs you want assessed (deployed preview URLs are recommended for PR runs).
- The workflow attempts to build some known example subprojects (`react/next`, `react/react-router`) so Storybook or static sites are available when possible. Adjust the paths in the workflow if your project structure differs.

Run locally

1. Install pa11y-ci globally (or use npx):

```bash
npm install -g pa11y-ci@6
# or
npx pa11y-ci --config .pa11yci
```

2. If your target URLs are local (Storybook, dev server), start those servers first. Example Storybook for the next template:

```bash
cd react/next
npm ci
npm run storybook
# then in another shell run pa11y
pa11y-ci --config ../../.pa11yci
```

CI behavior
- Runs on push to `main` and on pull requests.
- Pa11y issues will surface in the workflow logs. The example workflow currently does not fail the job on pa11y issues (it runs with `|| true`) to avoid blocking on initial runs; you can remove the `|| true` to make failures block merges once baselined.
- Reports: pa11y does not emit json/html by default here; modify the workflow to redirect output or use the `--reporter` flag if you want artifacts. The workflow includes an artifact upload step that looks for `pa11y-report.json`/`pa11y-report.html` if you change pa11y invocation to produce them.

Notes & tips
- Prefer pointing `.pa11yci` at deployed preview URLs (Netlify/Pages/Vercel) created for each PR. This gives realistic pages and assets.
- Start with a permissive CI setting (warnings do not fail) while you iterate. Tighten to fail on critical/high issues later.
