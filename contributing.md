---
title: Contributing
toc: false
---

## Get involved {#get-involved}

### Run a workshop 

Contact the [**BioCommons Training Team**](https://www.biocommons.org.au/event-support) to deliver a BioShell-powered training event at your institution.

### Share your work 

If you have published research or training materials using BioShell, [**let us know**](mailto:comms@biocommons.org.au) so we can feature it here.

### Contribute to BioShell

Visit the [**BioShell GitHub repository**](https://github.com/AustralianBioCommons/BioShell) to report issues or contribute to development.

### Contribute to BioShell User Guides

This site is built with [Jekyll](https://jekyllrb.com/) using the [ELIXIR Toolkit theme](https://github.com/ELIXIR-Belgium/elixir-toolkit-theme). To fix a typo, update a page, or add a new guide:

1. Fork or clone the [**BioShell-User-Guide repository**](https://github.com/Sydney-Informatics-Hub/BioShell-User-Guide)
2. Edit or add pages under `pages/`, following the layout in `pages/example_page.md`
3. [Render the site locally](#rendering-locally) to preview your changes
4. Open a pull request with your changes

See the [ELIXIR Toolkit theme markdown cheat sheet](https://elixir-belgium.github.io/elixir-toolkit-theme/markdown_cheat_sheet) for formatting help (message boxes, images, etc).

#### Rendering locally {#rendering-locally}

You'll need Ruby and [Bundler](https://bundler.io/) installed. This site uses the `github-pages` gem, which needs Ruby 2.7–3.2 — a newer Ruby (3.4+) will fail with "cannot load such file" errors for gems like `csv` and `webrick` that were removed from Ruby's default library.

**macOS (Homebrew):**

```bash
brew install ruby@3.2
export PATH="/opt/homebrew/opt/ruby@3.2/bin:$PATH"
```

{% include callout.html type="tip" content="Add the `export PATH` line to your `~/.zshrc` or `~/.bash_profile` so it persists across terminal sessions." %}

**Install dependencies and Bundler:**

```bash
gem install bundler
bundle install
```

**Start the local server:**

```bash
bundle exec jekyll serve
```

Then open the printed URL (e.g. `http://127.0.0.1:4000/BioShell-User-Guide/`) in your browser. Jekyll watches for file changes and rebuilds automatically — refresh the page to see edits.

{% include callout.html type="note" content="If bundle install fails with a dependency resolution error, delete Gemfile.lock and try again." %}
