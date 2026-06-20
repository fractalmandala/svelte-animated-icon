---
title: About
id: 30
group: others
description: "About this project, Github and other links."
---

`svelte-animated-icon` is a Svelte 5 animated icon library - tree-shakeable, multi-library, and powered by the native Web Animations API.

## Project

- **Source:** the `/Users/amrit/fractals/packages/svelte-animated-icon` package in this monorepo.
- **Built with:** SvelteKit 2, Svelte 5, the `@sveltejs/package` packaging tool, and `mdsvex` for these docs.
- **Animation engine:** the browser's Web Animations API. No animation library dependency.
- **Icon sources:** [Phosphor Icons](https://phosphoricons.com/) (MIT) and [Remix Icons](https://remixicon.com/).

## How this site is structured

The package and the docs site share a single SvelteKit project:

- `src/lib/` is the **package source** - `AnimatedIcon`, the templates, and the generated icon files.
- `src/routes/` is the **docs site** you're reading. The home route is an interactive explorer; everything under `/docs` is an `.md` file compiled by mdsvex.
- `scripts/generate.js` reads source SVGs and writes the per-icon `.svelte` files.
- `static/svg/` holds the raw Phosphor and Remix source SVGs.

The full architecture is documented at `src/lib/data/site-config.md`.

## Credits

- **Phosphor Icons** - MIT. Maintained by the Phosphor Icons team.
- **Remix Icons** - Free for commercial and personal use, with the "icons aren't the main value" redistribution clause.
- **Svelte & SvelteKit** - MIT.
- **Vite** - MIT.

## Related projects in this monorepo

This package is part of the Fractals monorepo. Adjacent surfaces:

- **Fractal Creation Engine** - the broader design system project this library belongs to.
- **Fractaldharma** - [fractaldharma.in](https://www.fractaldharma.in)
- **Fractalwork** - [fractalwork.in](https://www.fractalwork.in)

## License

The package's own source code is released under the license declared in `package.json`. The Phosphor icons are MIT; the Remix icons follow Remix Icon's terms. See [License](/docs/05-license) for the full breakdown.

## Contributing

The most useful contributions right now:

1. **Try the library** and report friction. The home route is the fastest way to browse.
2. **Run the codegen** with new icon sources if you maintain a set that fits the model in [Adding an Icon Library](/docs/25-adding-library).
3. **Improve a doc** - every page in this site accepts PRs against the corresponding `.md` file in `src/routes/docs/`.

Open an issue or PR on the project's repository to start a conversation.