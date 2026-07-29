## About

Please see the [How-to Guide for the template](https://australianbiocommons.github.io/how-to-guide-template/) for more information on how to get started.  

This template is based on the [ELIXIR Toolkit Theme example](https://github.com/ELIXIR-Belgium/elixir-toolkit-theme-example).

## Contribute to BioShell User Guides

This site is built with [Jekyll](https://jekyllrb.com/) using the [ELIXIR Toolkit theme](https://github.com/ELIXIR-Belgium/elixir-toolkit-theme). To fix a typo, update a page, or add a new guide:

1. Fork or clone the [**BioShell-User-Guide repository**](https://github.com/Sydney-Informatics-Hub/BioShell-User-Guide)
2. Edit or add pages under `pages/`, following the layout in `pages/example_page.md`
3. [Render the site locally](#rendering-locally) to preview your changes
4. Open a pull request with your changes

See the [ELIXIR Toolkit theme markdown cheat sheet](https://elixir-belgium.github.io/elixir-toolkit-theme/markdown_cheat_sheet) for formatting help (message boxes, images, etc).

#### Rendering locally {#rendering-locally}

You'll need Ruby and [Bundler](https://bundler.io/) installed. This site uses the `github-pages` gem, which needs Ruby 2.7–3.2.

**macOS (Homebrew):**

```bash
brew install ruby@3.2
```

`ruby@3.2` is keg-only, so it won't override your system Ruby automatically. Rather than relying on `PATH` (which tools like conda can silently reorder between terminals), call it explicitly:

```bash
/opt/homebrew/opt/ruby@3.2/bin/gem install bundler
/opt/homebrew/opt/ruby@3.2/bin/bundle install
```

{% include callout.html type="tip" content="Sanity check before installing: run <code>/opt/homebrew/opt/ruby@3.2/bin/ruby -v</code> and confirm it prints 3.2.x. If a plain <code>ruby -v</code> already prints 3.2.x, you can drop the full paths and just use <code>gem</code>/<code>bundle</code> directly." %}

**Start the local server, with live-reload so the browser updates automatically on save:**

```bash
/opt/homebrew/opt/ruby@3.2/bin/bundle exec jekyll serve --livereload
```

Then open the printed URL (e.g. `http://127.0.0.1:4000/BioShell-User-Guide/`) in your browser. Every time you save a page, Jekyll rebuilds it and the open browser tab refreshes on its own — no need to restart the server or reload manually.

{% include callout.html type="note" content="If bundle install fails with a dependency resolution error (e.g. a bundler version mismatch in Gemfile.lock), delete Gemfile.lock and run bundle install again." %}

## Acknowledgements for the guides

This work is supported by the [Australian BioCommons](https://www.biocommons.org.au/) via funding from [Bioplatforms Australia](https://bioplatforms.com/), the Australian Research Data Commons (https://doi.org/10.47486/PL105) and the Queensland Government RICF programme. Bioplatforms Australia and the Australian Research Data Commons are funded by the National Collaborative Research Infrastructure Strategy (NCRIS).

This repository makes use of the ELIXIR toolkit theme: [![theme badge](https://img.shields.io/badge/ELIXIR%20toolkit%20theme-jekyll-blue?color=0d6efd)](https://github.com/ELIXIR-Belgium/elixir-toolkit-theme)