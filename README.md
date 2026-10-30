# re-Terminal (Helltop variant)

This is the re-Terminal variant maintained for [Helltop.net](https://helltop.net). It is forked from
[mtwb47/re-terminal](https://github.com/mtwb47/re-terminal), which is itself a fork of
[Mirus's re-Terminal](https://github.com/mirus-ua/hugo-theme-re-terminal).

The retro terminal design and the upstream re-Terminal feature set are unchanged. This variant adds:

- **FediComment for Mastodon and Wafrn** — comment threads and counters understand both platforms.
- **Edge-render friendly comments** — the comments partial emits `data-fedicomment-*` attributes and
  keeps the client-side loader as a fallback, so a Worker can render the thread into the page.
- **ListenBrainz dashboard** — `listenbrainz` and `recent` shortcodes for listening stats and a
  compact now-playing card, fed from `data/listenbrainz/`.
- **Smart typography** — a `typography.html` partial that turns `...` into an ellipsis and straight
  quotes into curly quotes for display text from external sources.
- **Wafrn icon** in the fediverse callout.
- **Lightbox and fingerprinted assets** — click content images to enlarge; CSS and JS are
  fingerprinted in production.

See [FEDICOMMENT.md](FEDICOMMENT.md) for the comment setup. The companion Worker that renders
comments and the ListenBrainz dashboard at the edge lives in the
[Helltop site repo](https://github.com/anoraktrend/Helltop), not in this theme.

![re-Terminal](images/screenshot.png)

### DEMO - https://helltop.net

### ⚠️ This variant needs at least Hugo **Extended** v0.168.0.

The theme uses `.Site.Language.Locale`, which older Hugo versions do not provide. Upstream
re-Terminal releases may still work with older Hugo versions.

---

## Table of Contents

- [Features](#features)
- [What's different in this fork](#whats-different-in-this-fork)
- [CSS Variables](#css-variables)
- [Built-in shortcodes](#built-in-shortcodes)
- [Code highlighting](#code-highlighting)
- [How to start](#how-to-start)
- [How to run your site](#how-to-run-your-site)
- [How to configure](#how-to-configure)
- [Post archetype](#post-archetype)
- [Content Types](#content-types)
- [Add-ons](#add-ons)
- [How to edit the theme](#how-to-edit)
- [Found a bug?](#bug)
- [New cool idea or feature](#feature)
- [Using this theme?](#using-this-theme)
- [License](#license)

## Features

- **15 theme families** with both dark and light variants (30 total themes)
- **Theme switcher** with random theme option for dynamic theme selection
- **Multiple post types** including standard posts, links, images, and videos with galleries
- [**Fira Code**](https://github.com/tonsky/FiraCode) as default monospaced font. It's gorgeous!
- **Really nice duotone**, custom syntax highlighting based on [**PrismJS**](https://prismjs.com)
- **Fully responsive** design with mobile menu support
- **Fully based on Hugo ecosystem** (Pipes and Modules)
- **Native CSS variables** for super easy color customization
- **Archive page** with chronological post listing
- **Image galleries** with lightbox support
- **YouTube shortcode** for embedded videos
- **FediComment** — Mastodon and Wafrn commenting with platform-aware counters (no backend required; optional edge rendering)
- **ListenBrainz shortcodes** for listening stats and a now-playing card
- **Smart typography** partial for externally sourced display text
- **CRT shortcode** for opt-in scanline/green-phosphor display blocks
- **Customizable scrollbar** and selection styling
- **Logo animations** and customization
- **Light/dark theme toggle**
- **Footer menu** support
- **Dropdown menus** and submenus
- **Sticky menu** with configurable position (top, bottom, or none)

## What's different in this fork

This variant is based on mtwb47/re-terminal and keeps the upstream feature set. The Helltop-specific
changes are:

### Comments

- `fediverse.platform` frontmatter accepts `mastodon` (default) or `wafrn`.
- Wafrn threads are fetched from `/api/forum/:id`, and rewoots are filtered out.
- Wafrn avatars are loaded through the instance's v2 avatar cache endpoint.
- `comments.html` emits `data-fedicomment-*` attributes used by both the client-side fallback and
  edge rendering.
- `comments_counter.html` caches counts per platform and skips the API request when the count was
  already rendered into the page.

### Music

- The `listenbrainz` shortcode renders top artists, recordings, releases, and listening activity
  from `data/listenbrainz/`.
- The `recent` shortcode renders the compact now-playing card from
  `data/listenbrainz/recent.json` (it previously read Last.fm data).
- Display text is passed through `layouts/partials/typography.html` for smart punctuation.

### Presentation and assets

- Wafrn icon in the fediverse callout.
- Content images open in a lightbox.
- CSS and JS are fingerprinted in production.
- Social/Open Graph images fall back from the page cover to the site cover to the favicon.
- `assets/css/scanlines.css` provides the CRT/scanline styles used by the `crt` shortcode.

## CSS Variables

You can find all of them in the browser's page inspector, but here is the list with default values anyway:

```css
  /* the colors are from dark pallets; light themes have different defaults */
  :root {
    --accent: #23B0FF; /* 1 of 5 basic colors */
    --background: color-mix(in srgb, var(--accent) 2%, #1D1E28 98%); /* background color; inherit shades of the accent */
    --accent-contrast-color: black; /* mainly uses for text on the accent backgrounds but not limited */
    --color: white; /* text color, also some other text use the variable in color mixing */
    --border-color: rgba(255, 255, 255, .1); /* border color */
    --article-link-color: var(inherit); /* for you, who want to colorize your article links */
    --menu-color: black; /* a variable for menus color */

    /* code syntax */
    /* take a look at themes/re-terminal/assets/css/syntax.scss to understand in detail which color stands for */
    --syntax-func-color: color-mix(in srgb, var(--accent) 70%, #999 30%);
    --syntax-var-color: color-mix(in srgb, var(--accent) 90%, transparent);
    --syntax-value-color: color-mix(in srgb, var(--accent), white);

    /* breakpoints */
    /* unfortunately, native CSS variables don't support media queries, so use SCSS vars instead */
    $phone: 684px;
    $tablet: 900px;
  }
```

### Theme Configuration

The theme switcher is built into the header and allows dynamic theme selection. Available themes:

**Theme Families** (all with dark and light variants):
- gruvbox, onedark, nightowl, palenight, everforest
- catppuccin, high-contrast, oxocarbon, dracula, nord
- rosepine, tokyonight, solarized, monokai, ayu

**Random Theme Option**: Select "🎲 random" to automatically randomize the theme on each page load while respecting your light/dark preference.

**Series Navigation Configuration**:

```toml
[params]
  # Set to false to have series navigation collapsed by default
  seriesOpenByDefault = true

  # Limit number of series items shown (0 = show all, default)
  # Example: seriesMaxItems = 6 shows only the 6 most recent posts
  seriesMaxItems = 0
```

## Built-in shortcodes

### `image`
Display an image with optional positioning and styling.

**Required props:** `src`
**Optional props:** `alt`, `position` (left | center | right), `style`

```go
{{< image src="/img/hello.png" alt="Hello Friend" position="center" style="border-radius: 8px;" >}}
```

### `figure`
Same as `image`, plus caption support.

**Required props:** `src`
**Optional props:** `alt`, `position`, `style`, `caption`, `captionPosition` (left | center | right), `captionStyle`

```go
{{< figure src="/img/hello.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Hello Friend!" captionPosition="right" captionStyle="color: red;" >}}
```

### `code`
Display collapsible code blocks with syntax highlighting.

**Required props:** `language`
**Optional props:** `title`, `id`, `expand` (default "△"), `collapse` (default "▽"), `isCollapsed`

```go
{{< code language="css" title="Really cool snippet" id="1" expand="Show" collapse="Hide" isCollapsed="true" >}}
pre {
  background: #1a1a1d;
  padding: 20px;
  border-radius: 8px;
}
{{< /code >}}
```

### `collapse`
Create collapsible sections.

**Optional props:** `title`, `open`, `class`

```go
{{< collapse title="Click to expand" >}}
This content is hidden by default.
{{< /collapse >}}
```

### `crt`
Display content in a CRT/scanline panel with green phosphor text. The styles live in
`assets/css/scanlines.css` and are loaded by the theme.

```go
{{< crt >}}
HELLTOP
{{< /crt >}}
```

### `youtube`
Embed YouTube videos with responsive sizing.

**Required:** Video ID or full YouTube URL

```go
{{< youtube "dQw4w9WgXcQ" >}}
```

Or with full URL:
```go
{{< youtube "https://www.youtube.com/watch?v=dQw4w9WgXcQ" >}}
```

### `archives`
Display a chronological archive of all posts, links, and images grouped by year.

```go
{{< archives >}}
```

### `wordcount`
Display blog statistics including total posts and word count.

```go
{{< wordcount >}}
```

### `listenbrainz`
Display the ListenBrainz dashboard: top artists, recordings, releases, and listening activity.
Reads the JSON snapshots in `data/listenbrainz/`. Fetching those snapshots is left to the site (for
example a CI script or a Worker cron job).

```go
{{< listenbrainz >}}
```

### `recent`
Display the most recent ListenBrainz listen as a compact now-playing card. Reads
`data/listenbrainz/recent.json`.

```go
{{< recent >}}
```

### `lastfm`
Display Last.fm listening statistics including recent tracks, top tracks, and top artists.

**Note:** This is the upstream shortcode. It requires Last.fm data saved to `data/lastfm/` and is
not used by the Helltop variant.

```go
{{< lastfm >}}
```

## Code highlighting

A custom syntax highlighting based on PrismJS. All you need to do is to wrap you code like this:

````
```html
  // your code here
```
````

**Supported languages**: actionscript, apacheconf, applescript, bash, c, clike, cmake, coffeescript, cpp, csharp, csp, css, css-extras, diff, django, docker, elixir, elm, erlang, flow, fsharp, git, go, graphql, haml, handlebars, haskell, http, java, javascript, json, jsx, kotlin, latex, less, llvm, makefile, markdown, markup, markup-templating, nasm, objectivec, ocaml, perl, php, php-extras, powershell, processing, pug, python, r, reason, ruby, rust, sass, scala, scheme, scss, sql, stylus, swift, textile, toml, tsx, twig, typescript, vim, visual-basic, wasm, yaml.

## How to start

You can download the theme manually from
[https://github.com/anoraktrend/re-terminal](https://github.com/anoraktrend/re-terminal) and paste it
into `themes/re-terminal` in your Hugo root directory.

You can also choose **one of the 3 possibilities** to install the theme:

1. as Hugo Module
2. as a standalone local directory
3. as a git submodule

⚠️ This variant needs at least Hugo **Extended** v0.168.0.

### Install theme as Hugo Module

```bash
# If this is the first time you're using Hugo Modules
# in your project. You have to initiate your own module before
# you fetch the theme module.
#
# hugo mod init [your website/module name]
hugo mod get github.com/anoraktrend/re-terminal/v2
```

and in your config file add:

```toml
[module]
  # this is needed when you fetch the theme as a submodule to your repo.
  # replacements = "github.com/anoraktrend/re-terminal/v2 -> themes/re-terminal"
[[module.imports]]
  path = 'github.com/anoraktrend/re-terminal/v2'
```

This fork has no tagged releases, so Hugo Modules resolves a pseudo-version from `main`. Use the
submodule or vendored options below if you want to pin a specific commit in your site repo.

Keep in mind that the theme by default won't show up in the `themes` directory. This means that you are using the theme as it was on the repository at the moment you fetched it. Your local `go.sum` file keeps all the references. Read more about Hugo Modules in the [official documentation](https://gohugo.io/hugo-modules/).

### Install theme locally

```bash
git clone https://github.com/anoraktrend/re-terminal.git themes/re-terminal
```

This will clone the repository directly to the `themes/re-terminal` directory. The Helltop site
uses this layout, with the theme vendored into the site repo.

### Install theme as a submodule

```bash
git submodule add -f https://github.com/anoraktrend/re-terminal.git themes/re-terminal
```

This will install the repository as a submodule in the `themes/re-terminal` directory.

⚠️ If you encounter any issues with:

```bash
Error: module "re-terminal" not found; either add it as a Hugo Module or store it in "[...your custom path]/themes".: module does not exist
```

then please try to remove `theme = "re-terminal"` from your config file.

## How to run your site

```bash
hugo server -t re-terminal
```

and go to `localhost:1313` in your browser. From now on all the changes you make will go live, so you don't need to refresh your browser every single time.

## How to configure

The theme doesn't require any advanced configuration. Just copy:

```toml
baseurl = "/"
locale = "en-us"
# Add it only if you keep the theme in the `themes` directory.
# Remove it if you use the theme as a remote Hugo Module.
theme = "re-terminal"
pagination.pagerSize = 5

[params]
  # dir name of your main content (default is `content/posts`).
  # the list of set content will show up on your index page (baseurl).
  contentTypeName = "posts"

  # ["orange", "blue", "red", "green", "pink", "paper"]
  themeColor = "orange"

  # if you set this to 0, only submenu trigger will be visible
  showMenuItems = 2

  # show selector to switch language
  showLanguageSelector = false

  # set theme to full screen width
  fullWidthTheme = false

  # center theme with default width
  centerTheme = false

  # if your resource directory contains an image called `cover.(jpg|png|webp)`,
  # then the file will be used as a cover automatically.
  # With this option you don't have to put the `cover` param in a front-matter.
  autoCover = true

  # set post to show the last updated
  # If you use git, you can set `enableGitInfo` to `true` and then post will automatically get the last updated
  showLastUpdated = false

  # set a custom favicon (default is a `themeColor` square)
  # favicon = "favicon.ico"

  # Provide a string as a prefix for the last update date. By default, it looks like this: 2020-xx-xx [Updated: 2020-xx-xx] :: Author
  # updatedDatePrefix = "Updated"

  # set all headings to their default size (depending on browser settings)
  # oneHeadingSize = true # default

  # whether to show a page's estimated reading time
  # readingTime = false # default

  # whether to show a table of contents
  # can be overridden in a page's front-matter
  # Toc = false # default

  # set title for the table of contents
  # can be overridden in a page's front-matter
  # TocTitle = "Table of Contents" # default

  # sticky menu position on desktop (min-width: 769px)
  # options: "top" (menu above logo, sticky), "bottom" (menu below logo, sticky), "none" (no sticky behavior)
  # default: "bottom"
  # stickyMenuPosition = "bottom"

  # you can set a banner on the top of the page with a call to action
  # defaults: dismissible = false; URL is optional
  # [params.banner]
  # dismissible = false
  # text = "Check it out on GitHub"
  # url = "https://github.com/anoraktrend/re-terminal"

[params.twitter]
  # set Twitter handles for Twitter cards
  # see https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started#card-and-content-attribution
  # do not include @
  creator = ""
  site = ""

[languages]
  [languages.en.params]
    languageName = "English"
    title = "re-Terminal"
    subtitle = "A simple, retro theme for Hugo"
    owner = ""
    keywords = ""
    copyright = ""
    menuMore = "Show more"
    readMore = "Continue Reading"  # Text for the read-more link on post excerpts
    readOtherPosts = "Read other posts"
    newerPosts = "Newer posts"
    olderPosts = "Older posts"
    missingContentMessage = "Page not found..."
    missingBackButtonLabel = "Back to home page"
    minuteReadingTime = "min read"
    words = "words"

    [languages.en.params.logo]
      logoText = "re-Terminal"
      logoHomeLink = "/"

    [languages.en.menu]
      # Submenus is available since v2.1.0
      # [[languages.en.menu.main]]
      #   identifier = "submenuParent"
      #   name = "Submenu"
      # [[languages.en.menu.main]]
      #   parent = "submenuParent"
      #   identifier = "anItem"
      #   name = "AnItem"
      [[languages.en.menu.main]]
        identifier = "about"
        name = "About"
        url = "/about"
      [[languages.en.menu.main]]
        identifier = "showcase"
        name = "Showcase"
        url = "/showcase"

# Only needed when installing the theme as a Hugo Module. Skip this when
# the theme lives in themes/re-terminal and `theme = "re-terminal"` is set.
# [module]
#   # In case you would like to make changes to the theme and keep it locally in your repository,
#   # uncomment the line below (and correct the local path if necessary).
#   # replacements = "github.com/anoraktrend/re-terminal/v2 -> themes/re-terminal"
# [[module.imports]]
#   path = 'github.com/anoraktrend/re-terminal/v2'
```

to `config.toml` file in your Hugo root directory and change params fields. In case you need, here's [a YAML version](https://gist.github.com/panr/9eeea6f595c257febdadc11763e3a6d1).

**NOTE:** Please keep in mind that `main menu` supports submenu nesting (since v2.1.0).

## Post archetype

See the default `post` file params supported by the theme — https://github.com/anoraktrend/re-terminal/blob/main/archetypes/posts.md

## Content Types

re-Terminal supports multiple content types with different front matter configurations:

### Standard Posts

Create with: `hugo new posts/my-post.md`

```toml
+++
title = "My Post Title"
date = "2025-10-19"
author = ""
authorTwitter = "" #do not include @
cover = ""
coverCaption = ""
tags = ["tag1", "tag2"]
keywords = ["keyword1", "keyword2"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++
```

### Link Posts

Create with: `hugo new links/my-link.md`

Link posts appear with special styling and a 🔗 icon in archives.

```toml
+++
title = "Interesting Article"
date = "2025-10-19"
author = ""
authorTwitter = "" #do not include @
cover = ""
coverCaption = ""
tags = ["links", "web"]
keywords = [""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
type = "links"
link_url = "https://example.com/article" # The external URL for the link post
+++
```

### Image Posts

Create with: `hugo new images/my-image.md`

Image posts support galleries with lightbox and appear with a 📷 icon in archives.

```toml
+++
title = "My Photo Gallery"
date = "2025-10-19"
author = ""
type = "images"
image = ""
imageAlt = ""
imageCaption = ""
tags = ["photography", "gallery"]
keywords = [""]
description = ""
showFullContent = true
readingTime = false
hideComments = false
color = ""
+++
```

## Add-ons

- **Comments** — for adding comments to your blog posts please take a look at `layouts/partials/comments.html` https://github.com/anoraktrend/re-terminal/blob/main/layouts/partials/comments.html and [FEDICOMMENT.md](FEDICOMMENT.md).
- **Comments counter** — for adding comments counter to your blog posts please take a look at `layouts/partials/comments_counter.html` https://github.com/anoraktrend/re-terminal/blob/main/layouts/partials/comments_counter.html.
- **Extended Head** — please take a look at `layouts/partials/extended_head.html` https://github.com/anoraktrend/re-terminal/blob/main/layouts/partials/extended_head.html
- **Extended Footer** — please take a look at `layouts/partials/extended_footer.html` https://github.com/anoraktrend/re-terminal/blob/main/layouts/partials/extended_footer.html

### Low-level add-ons

- **Post base** — Renders bare minimum of posts' content. For motivation and details take a look at https://github.com/anoraktrend/re-terminal/blob/main/layouts/partials/single_basic.html

## How to edit the theme `<a id="how-to-edit" />`

If you are using as a remote Hugo Module (you don't have the theme files in the `theme/re-terminal`) and you have to override only some of the styles, you can do this easily by adding `static/style.css` in your root directory and point things you want to change.

Thanks to the native CSS variables migration, you can easily customize colors without touching the theme files. Just override the CSS variables in your `static/style.css`:

```css
:root {
  --accent: #FF6B35; /* Change accent color */
  --background: #1a1a1a; /* Change background */
  --color: #f0f0f0; /* Change text color */
}
```

If you have the theme files in the theme directory, then you can directly edit anything in the theme, you just have to go to `themes/re-terminal` and modify the files. No compilation step needed.

## Found a bug? `<a id="bug" />`

If you spot any bugs in this variant, please use the fork's [issue tracker](https://github.com/anoraktrend/re-terminal/issues) or open a [pull request](https://github.com/anoraktrend/re-terminal/pulls). Issues in the base theme that also affect upstream should go to [mtwb47/re-terminal](https://github.com/mtwb47/re-terminal/issues) or [mirus-ua/hugo-theme-re-terminal](https://github.com/mirus-ua/hugo-theme-re-terminal/issues).

## New cool idea or feature? `<a id="feature" />`

This variant is maintained for Helltop. Open an issue or pull request on the
[fork](https://github.com/anoraktrend/re-terminal) if you want to change it here. If the change is
also useful to the base theme, send it upstream to [mtwb47/re-terminal](https://github.com/mtwb47/re-terminal)
or [mirus-ua/hugo-theme-re-terminal](https://github.com/mirus-ua/hugo-theme-re-terminal).

## Using this theme?

If you use this variant, feel free to add your site to [USERS.md](USERS.md).

## Changelog

For a detailed list of all changes, improvements, and new features, please see [CHANGELOG.md](CHANGELOG.md).

## License

Copyright © 2019-2022 Radosław Kozieł ([@panr](https://twitter.com/panr))

Copyright © 2024 Mirus

Fork modifications © 2026 Lucy Ada Randall

The theme is released under the MIT License. See [LICENSE.md](LICENSE.md) for the full text and
upstream attribution.
