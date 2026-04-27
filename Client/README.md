# Firengii — Nuxt.js Crash Course Project

A fire-extinguisher product catalogue and rental application built as a hands-on Nuxt.js crash course. Users can browse products, view detailed product pages, and add items to their personal rental list.

---

## Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Pages & Routing](#pages--routing)
- [Layouts](#layouts)
- [Components](#components)
- [Vuex Store](#vuex-store)
- [Plugins](#plugins)
- [Testing](#testing)

---

## Overview

**Firengii** is a client-side Nuxt.js 2 application that demonstrates:

- File-system based routing (static and dynamic routes)
- Reusable Vue components
- Vuex state management (state, mutations, getters)
- Third-party plugin registration
- Nuxt layouts for page-level layout switching
- The Nuxt `fetch` hook for async data fetching
- Jest-based unit testing

---

## Tech Stack

| Dependency | Version | Purpose |
|---|---|---|
| [Nuxt.js](https://nuxtjs.org) | ^2.14 | SSR/SSG framework built on Vue |
| [Bootstrap](https://getbootstrap.com) | ^4.5 | Utility CSS |
| [bootstrap-vue](https://bootstrap-vue.org) | ^2.21 | Bootstrap components for Vue (modals, buttons) |
| [v-calendar](https://vcalendar.io) | ^2.2 | Date-picker / calendar component |
| [vue-js-modal](https://github.com/euvl/vue-js-modal) | ^2.0.0-rc | Programmatic modal support |
| [vue-star-rating](https://github.com/craigh411/vue-star-rating) | ^1.7 | Star-rating display component |
| [core-js](https://github.com/zloirock/core-js) | ^3.8 | Polyfills |

**Dev dependencies:** Jest, @vue/test-utils, babel-jest, vue-jest

---

## Project Structure

```
Client/
├── assets/
│   ├── data.js           # Static card-section data (largeCardSections, smallCardSections)
│   ├── images/           # Product images (fe1.jpg – fe24.jpg, etc.)
│   └── svg/              # SVG assets (fire-extinguisher.svg)
├── components/
│   ├── Hero.vue           # Landing-page hero banner
│   ├── Nav.vue            # Top navigation bar
│   ├── LargeCard.vue      # Single large product card
│   ├── LargeCardDisplay.vue  # Section wrapper for LargeCard items
│   ├── SmallCard.vue      # Single small product card (links to product detail)
│   ├── SmallCardDisplay.vue  # Section wrapper for SmallCard items
│   ├── MyItem.vue         # Rented item row used on My Items page
│   ├── RentModal.vue      # Bootstrap-Vue modal with date-picker for ordering
│   ├── Reviews.vue        # Fetches & displays customer reviews from Random User API
│   ├── ReviewCard.vue     # Single reviewer card (avatar + username)
│   └── PageNotFound.vue   # 404 / product-not-found fallback
├── layouts/
│   ├── default.vue        # Layout with Nav component (used by most pages)
│   ├── no-nav.vue         # Layout without Nav; shows a "Go Back" link
│   └── error.vue          # Nuxt error layout
├── middleware/            # (reserved for future route middleware)
├── pages/
│   ├── index.vue          # Home page — Hero + card sections
│   ├── my-items.vue       # My Items page — rented products list
│   └── products/
│       ├── index.vue      # Products listing page
│       └── _id.vue        # Dynamic product detail page (/products/:id)
├── plugins/
│   ├── vue-calendar.js    # Registers v-calendar (client-side only)
│   ├── vue-js-modal.js    # Registers vue-js-modal (SSR-compatible)
│   └── vue-star-rating.js # Registers vue-star-rating
├── static/                # Static assets served at root (favicon, etc.)
├── store/
│   └── index.js           # Vuex store — state, mutations, getters
├── test/                  # Jest unit tests
├── nuxt.config.js         # Nuxt configuration
└── package.json
```

---

## Getting Started

### Prerequisites

- Node.js ≥ 14
- npm ≥ 6

### Installation

```bash
# Install dependencies
npm install
```

### Development

```bash
# Start dev server with hot-reload at http://localhost:3000
npm run dev
```

### Production

```bash
# Build optimised bundle
npm run build

# Launch production server
npm run start
```

### Static Generation

```bash
# Generate a fully static site
npm run generate
```

---

## Pages & Routing

Nuxt automatically generates routes from the `pages/` directory.

| URL | File | Layout | Description |
|---|---|---|---|
| `/` | `pages/index.vue` | `default` | Home page with Hero banner, large and small card sections |
| `/products` | `pages/products/index.vue` | `default` | Full product listing |
| `/products/:id` | `pages/products/_id.vue` | `default` | Product detail page; shows 404 component if the ID is not found |
| `/my-items` | `pages/my-items.vue` | `no-nav` | List of the user's currently rented items from Vuex state |

---

## Layouts

### `default.vue`

Wraps every page with the `<Nav>` component followed by `<Nuxt />` (page outlet). Also defines global font and box-sizing styles.

### `no-nav.vue`

Used by the **My Items** page. Replaces the navbar with a simple "Go Back" link that navigates to `/`.

### `error.vue`

Nuxt's built-in error layout — displayed when a navigation-level error occurs.

---

## Components

### `Hero.vue`

Static hero banner for the home page. Displays a heading, a short description, and a "Start Looking" call-to-action button alongside an SVG illustration.

### `Nav.vue`

Dark Bootstrap navbar with the brand name **Firengii** and links to `/products` and `/my-items`. Uses `<NuxtLink>` for client-side navigation.

### `LargeCard.vue`

Renders a single product card with an image, title, and snippet text.

**Props:**
| Prop | Type | Description |
|---|---|---|
| `card` | Object | Product object with `id`, `title`, `snippet`, `image` |

### `LargeCardDisplay.vue`

Section wrapper that renders a heading, a snippet, and a row of `LargeCard` components.

**Props:**
| Prop | Type | Description |
|---|---|---|
| `cardsSection` | Object | `{ title, snippet, cards[] }` |

### `SmallCard.vue`

A compact image-only card that links to `/products/:id`.

**Props:**
| Prop | Type | Description |
|---|---|---|
| `card` | Object | Product object with `id` and `image` |

### `SmallCardDisplay.vue`

Wraps a grid of `SmallCard` components under a section heading.

**Props:**
| Prop | Type | Description |
|---|---|---|
| `cardsSection` | Object | `{ title, cards[] }` |

### `MyItem.vue`

Displays a single rented item as a horizontal card (image + title + description).

**Props:**
| Prop | Type | Description |
|---|---|---|
| `item` | Object | Rental object with `title`, `description`, `image` |

### `RentModal.vue`

Contains the **Rent** button and the Bootstrap-Vue modal that opens on click. The modal includes a short description paragraph and a `<vc-date-picker>` range calendar. Clicking **Order** calls the `addItem` Vuex mutation and closes the modal.

**Props:**
| Prop | Type | Description |
|---|---|---|
| `product` | Object | Product object; `product.id` is passed to `addItem` |

**Methods:** `showModal`, `hideModal`, `toggleModal`

**Vuex:** maps `addItem` mutation.

### `Reviews.vue`

Fetches 5 random user profiles from [randomuser.me](https://randomuser.me/api/?results=5) using the Nuxt `fetch` hook and renders a `<ReviewCard>` for each result.

### `ReviewCard.vue`

Displays a reviewer's avatar and username.

**Props:**
| Prop | Type | Description |
|---|---|---|
| `review` | Object | Random User API result object |

### `PageNotFound.vue`

Shown on the product detail page when the requested `id` does not match any product in the store.

---

## Vuex Store

`store/index.js` uses the Vuex module factory pattern required by Nuxt.

### State

| Key | Type | Description |
|---|---|---|
| `products` | `Product[]` | 24 fire-extinguisher products (id, title, snippet, description, image) |
| `myRentals` | `Product[]` | Products added by the user via the Rent modal (pre-seeded with one item) |

### Mutations

| Name | Payload | Description |
|---|---|---|
| `addItem` | `id` (Number) | Finds the product with the given `id` and pushes it into `myRentals` |

### Getters

| Name | Signature | Description |
|---|---|---|
| `getProductById` | `(state) => (id) => Product` | Returns a single product by `id`; used by the product detail page |

---

## Plugins

| File | Mode | Purpose |
|---|---|---|
| `plugins/vue-calendar.js` | `client` | Registers the `v-calendar` library with component prefix `vc` (e.g. `<vc-date-picker>`) |
| `plugins/vue-js-modal.js` | `server` | Registers `vue-js-modal` with dialog, dynamic, and modals-container support |
| `plugins/vue-star-rating.js` | — | Registers the `vue-star-rating` component |

---

## Testing

Unit tests are written with **Jest** and **@vue/test-utils**.

```bash
# Run all tests
npm test
```

Test files live in the `test/` directory. Babel is configured via `.babelrc` for Jest to transpile Vue SFC files using `vue-jest` and `babel-jest`.

---

## Further Reading

- [Nuxt.js Documentation](https://nuxtjs.org)
- [Bootstrap-Vue Documentation](https://bootstrap-vue.org)
- [v-calendar Documentation](https://vcalendar.io)
- [Vuex Documentation](https://vuex.vuejs.org)
