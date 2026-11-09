# Changelog

All notable changes to `qalainau/filament-warp-megamenu` are documented here.

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
