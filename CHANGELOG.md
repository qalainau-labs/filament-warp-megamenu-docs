# Changelog

All notable changes to `qalainau/filament-warp-megamenu` are documented here.

## [1.3.2] - 2026-11-12

### Security

- Now requires Filament 5.11 or later (was 5.0). Older versions do not have Filament's `#[Unexposed]` attribute, which the plugin uses to keep internal methods of the menu and the menu editor (such as importing a menu file) from being called directly from the browser. The tests now also run on the lowest supported versions before each release.

### Fixed

- A page whose navigation label is empty was not added to *Recently opened* (an error was reported in the log each time it was opened). It is now listed under a name made from its class.
- **No more “Nothing found” while records are still being searched:** typing `#1042` showed *Nothing found for “#1042”* for a moment, then the order once the answer arrived — people pressing Enter right away thought the number did not work. While the search waits for you to stop typing and for the answer, it now shows *Searching records…* instead, and pressing Enter during that moment opens the first result once the answer arrives (before, Enter did nothing, or opened the first link under *Nothing found*).
- **The kind of record under a number starts with a capital letter:** in a panel that keeps model labels in lower case (`$hasTitleCaseModelLabel = false`), the line under a record opened by its number read *order*; it now reads *Order*.
- **Translations:** German — the editor's button that goes back to previewing the menu as yourself now reads *Zurück zu meiner Ansicht* (was *Als ich ansehen*). Brazilian Portuguese — the search is called *pesquisa* everywhere (it was *busca* in some places).

## [1.3.1] - 2026-11-11

### Changed

- **The original text under each field while typing another language:** with a language chosen in *Names in*, every field you can type per language now shows its original text under it (*Original: Paper cups*). Before, the original showed only as the placeholder, so it disappeared as soon as the field held a translation — in the panel menu and in menus you make.
- **No tour bubbles on the menu editor:** with `->onboarding()`, the three bubbles for people opening the menu for the first time no longer start on the *Edit the menu* page, where they covered the heading and the explanations. They still start on every other page.

### Fixed

- **Line breaks in Japanese and Chinese:** panels, the launcher and the menus on public pages now use `line-break: strict`, so a line no longer starts with ー, a small kana or a closing punctuation mark (a card read 「メニュ／ーも」).

## [1.3.0] - 2026-11-11

### Added

- **Menus you make in several languages:** with `->editorLocales([...])`, the *Names in* list now also shows when you edit a menu you made. Choose a language and type its texts — the menu's name, the names and descriptions of groups and links, column headings, tags, the card, the link at the top right and the buttons under the panel; the original text shows as the placeholder and the preview and group tabs switch to that language. `<x-warp-megamenu::menu>`, `button` and `footer` and the button in the panel bar show the texts of `app()->getLocale()`, and the original text where none was typed, so one menu can serve a site in several languages. The texts are saved with the menu and travel with *Export / import*; removing a link removes its texts too. Before, a menu you made had its texts in one language only.

### Fixed

- **Links to a part of a page in made menus:** a link with `#` (`/#reviews`, `/pricing#teams`) was marked as the current page (`aria-current="page"`, the accent colour on the link and its group) whenever the page around it was open, so on a home page with several such links every one of them and their groups looked current. A link with `#` is now never the current page; links without one work as before.

## [1.2.0] - 2026-11-10

### Security

- **Icon colour of links in made menus:** the icon colour saved with a link of a menu made in the editor is now used only when it is a colour name (`success`, `info`, …). Before, a value sent by changing the editor's request by hand was written into the `style` attribute of the link, so someone who may edit menus could change how the bar looks for everyone.

### Fixed

- **Groups named in Japanese, Chinese, Korean and other scripts:** `Str::slug()` turned such names into an empty string, so every group got the key `group:` (the bucket for pages without a group): the groups were merged into one panel, with the last group's name and description. Names that slug to nothing now get a key made from the name itself (`group:u-…`). Keys of names written in Latin letters do not change, so menus saved in the editor keep working.

### Changed

- **Tenant index on the layout history:** a new migration (`update_warp_megamenu_layout_versions_add_tenant_index`) indexes `warp_megamenu_layout_versions` by `tenant_type`, `tenant_id`, `panel`, `audience` and `publish_at`. The history is read per tenant, so in an app with many tenants the history and the scheduled changes no longer scan the other tenants' rows. Run `php artisan migrate` (it runs by itself when the migrations are not published; published apps publish again with `--tag=filament-warp-megamenu-migrations`).

## [1.1.1] - 2026-11-09

### Changed

- **“More below” on long panels:** a panel taller than the screen (often the launcher) now shows a small *More below* label and a light shadow at its bottom edge while there is more to scroll to, so the pages below the edge are not missed where scrollbars stay hidden (macOS with a trackpad, touch screens). Clicking the label scrolls the panel down without moving the focus out of the search box; the label goes away at the end of the list. The label is translated into the 10 languages of the plugin.

## [1.1.0] - 2026-11-09

### Added

- **Open a panel from an action:** `->openMegaMenu('group:sales')` on any Filament action — a table row action, a form action, a page header action — opens that group's panel as if the group were clicked in the bar (from *More*, beside the sidebar group, or in the drawer on phones). Without a group it opens the launcher. It takes a group key, a group name, a navigation group enum or `'menu:…'`.
- **Open from your own button:** the browser event `warp-megamenu-open` (`$dispatch('warp-megamenu-open', { group: 'group:sales' })`) does the same from any Blade view, widget or render hook.
- **The launcher at the right end of the bar:** `->launcherPosition('end')` puts the *Menu* button at the right end of the bar, before the search box and the user menu; its panel opens toward the left.

## [1.0.0] - 2026-11-08

### Added

- **Mega panels from your navigation:** every navigation group opens as a wide panel, with no configuration. `MegaGroup` (description, columns, width, a card, *View all*, *+ New* buttons, bento and flow layouts, plain dropdowns), `MegaSection` (headings, dividers, spans, items in columns) and `MegaItem` (descriptions, coloured icons, tags, keywords, coloured *new* dots, locked items, help links, child items, external links and Filament actions). `HasMegaMenuItem` to write an item next to its resource.
- **Numbers:** `->stat()` read only when a panel opens, Filament badges, `->poll()` and `->zeroBadges()`.
- **The launcher:** search over pages and records (ignoring case, accents, width and kana), `RecordLookup` to open a record by its number, favorites, recently opened pages and records, *Recommended* and *To do*.
- **Opening:** click or hover with a delay, switching between groups, a *More* button for groups that do not fit, keyboard navigation, a `g m` shortcut, `g 1`–`g 9` (or `MegaGroup::shortcut()`) to open a group's panel, shown on the group button, animations that respect reduced motion, right-to-left pages.
- **Sidebar flyout**: the sidebar lists only the groups, each opening its panel (also from the collapsed sidebar); `groupsOnly: false` keeps Filament's list of pages. A **full-screen drawer** on phones.
- **Changing the menu:** each person chooses their groups; with `->editable()`, allowed people edit the menu on a live preview of the real panel (names, descriptions — also under a group name —, icons with an icon picker, tags and their colours, the “new” dot with its last day and colour, the panel of the *Menu* button (button name, the parts of its left column, pages recommended to everyone, its card), the *View all* link and the card with its picture — each clicked in the panel and edited on its own —, order, columns, groups — and new groups added from templates (columns, columns and a card, tiles, steps with arrows) —, pages dragged in from a list of the panel's pages, hidden pages, links, buttons under a panel added, renamed, hidden and moved, and a format — colours, font, size, bold, italic, underline — for every part), saved per panel and tenant, and undo it all.
- **Menus you make:** any number of menus made in the editor (groups, columns, links to web addresses or panel pages — dragged in from a list of the panel's pages —, cards with an uploaded picture, buttons under each panel, the same formats), shown as a button in the panel bar or placed on public pages of the app with `<x-warp-megamenu::menu name="…" />` and `<x-warp-megamenu::button menu="…" />` (no Filament, Alpine or Livewire needed on those pages).
- **Menus per role** (`->audience()`), **preview as someone else**, a **history** of every saved menu with going back, **publishing later** at a date and time, **export and import** of all menus, and **names in other languages** (`->editorLocales()`) in the editor.
- **Usage** in the editor (people who opened each page in 30 days, pages nobody opened), **automatic recommendations** (`->autoRecommended()`), other names and **shortcuts** for pages (`->shortcut('g o')`), and **colour rules** for numbers.
- **Search** finds the create pages as *Actions* and shows **recent searches**; each person can put the groups in **their own order** and add **their own links**; a **tour** for new people (`->onboarding()`); a **small chart** in a panel (`MegaChart`).
- **Items made from data** in menus you make (`->menuSources()`), **schema.org** navigation for search engines, and a **footer** component (`<x-warp-megamenu::footer>`).
- **All pages:** a printable page with every page and its description (`->sitemap()`).
- Favorites, recent pages, the edited menu and choices are kept per user, panel and tenant (`tenantKeyUsing()`, `php artisan warp:assign-tenant`).
- Render hooks, publishable views, and ten languages.
