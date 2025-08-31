# Income Tax Calculator FY 2025-26

## 1. Project Overview

This is a comprehensive income tax calculator for the Indian Financial Year 2025-26. It is built as a **standalone, single-file HTML application** that runs entirely in the browser without needing a server or a complex development environment.

### Key Features:
-   **Dual Regime Comparison:** Calculates tax liability under both the Old and New Tax Regimes, with a clear suggestion for the most beneficial option.
-   **Detailed Calculations:** Includes modules for HRA, LTA, House Property income/loss, and various deductions under Chapter VI-A.
-   **Tax-Saving Planner:** Provides actionable insights into potential tax savings by maximizing deductions under the Old Regime.
-   **Data Persistence:** Automatically saves your input data to the browser's `localStorage`, so your information is preserved between sessions.
-   **Visual Insights:** A dynamic chart visualizes the breakdown of your income into tax, deductions, and post-tax income for easy comparison.
-   **Complete Portability:** As a single HTML file, it can be easily shared, archived, or run from anywhere, even offline.

---

## 2. Architecture: The Standalone HTML Approach

This project uses a special architecture to ensure maximum portability and ease of use. Unlike typical React projects that require a build step, this application is self-contained within a single `index.html` file.

### How it Works:
1.  **CDN Dependencies:** Instead of using `npm` to install packages, the application loads all its dependencies (React, ReactDOM, Chart.js, Tailwind CSS) directly from public Content Delivery Networks (CDNs).
2.  **In-Browser Transpilation:** The application's React JSX code is written directly inside a `<script type="text/babel">` tag in the `index.html` file. The **Babel Standalone** library is loaded from a CDN, and its job is to translate this modern JSX code into plain JavaScript that the browser can understand on-the-fly when the page is loaded.
3.  **No Build Step Required:** Because the "compilation" happens in the user's browser, there is no need to run `npm run build` or use Vite. The `index.html` file is the final, distributable product.

### Legacy Build Files (`package.json`, `vite.config.ts`)
The project contains files like `package.json`, `vite.config.ts`, and empty `App.tsx`/`index.tsx` files. **These are legacy files from the previous architecture and are no longer used to run the application.** They are kept for historical context or in case a future developer wishes to revert to a standard Vite-based development setup.

---

## 3. File Structure

The project structure is intentionally simple:

-   `index.html`: **The core of the entire application.** It contains the HTML structure, loads all CDN dependencies, and holds all the React application logic. This is the only file you need to run or modify.
-   `App.tsx`, `index.tsx`: These files are empty placeholders and are not used. All their logic has been consolidated into `index.html`.
-   `package.json`, `vite.config.ts`, `tsconfig.json`: Legacy configuration files that are not used in the current standalone architecture.
-   `README.md`: This documentation file.

---

## 4. Core Application Logic

All the logic resides within the `<script type="text/babel">` block in `index.html`.

### State Management
-   **`useState`:** Used to manage all user inputs (e.g., salary, deductions, property details).
-   **`useEffect`:**
    -   One `useEffect` hook is responsible for **loading** saved data from `localStorage` when the component first mounts.
    -   A second `useEffect` hook **saves** the current state to `localStorage` whenever any input value changes. The `STORAGE_KEY` constant defines the key used in `localStorage`.
-   **`useRef`:** Used to hold a reference to the HTML `<canvas>` element for Chart.js.

### Calculation Engine
-   **`useMemo`:** The primary calculation logic is wrapped in a `useMemo` hook. This is a critical performance optimization. The entire tax calculation is only re-run when one of its dependencies (an input value) changes.
-   **Key Calculation Functions:**
    -   `calculateOldRegimeTax()`: Computes slab-based tax for the old regime, considering age.
    -   `calculateNewRegimeTax()`: Computes slab-based tax for the new regime.
    -   `calculateSurchargeAndRelief()`: Calculates applicable surcharge and marginal relief for high-income earners.
    -   The main `results` object within `useMemo` aggregates all calculations, from gross income to final net payable/refund for both regimes.

### Internationalization (i18n)
-   A `translations` object at the top of the script contains all the UI strings in English.
-   A `t()` helper function is used throughout the component to retrieve these strings, allowing for easy translation into other languages in the future by simply adding new keys to the `translations` object.

---

## 5. Development Workflow (How to Make Changes)

Modifying the application is straightforward:

1.  **Open `index.html`** in your preferred code editor (e.g., VS Code).
2.  **Locate the `<script type="text/babel">` tag.** This is where the entire React application lives.
3.  **Edit the React code** as you would in a standard `.tsx` file. You can change state variables, update calculation logic, or modify the JSX that renders the UI.
4.  **Save the `index.html` file.**
5.  **Open or refresh the `index.html` file in your web browser** to see the changes. There is no dev server or hot-reloading; you must manually refresh the page.

---

## 6. Deployment

Deployment is trivial:

-   **Simply copy, share, or upload the `index.html` file.**
-   It can be opened directly from a local file system (`file:///...`).
-   It can be hosted on any static web hosting service (like GitHub Pages, Netlify, or Vercel) by just uploading the single file. If you name it `index.html`, it will serve as the default page for its directory.
