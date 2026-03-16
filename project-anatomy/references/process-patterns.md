# Process Patterns

How to identify processes and entry files for common project types. Read this if you're unsure about Steps 1-2 of the analysis workflow.

## Electron Apps

**Processes:** Always 2 — main process (Node.js) + renderer process (Chromium browser). They are OS-level separate processes with separate memory.

**Entry files:**
- Main: look in `package.json` → `main` field, or in the Electron builder config. Usually something like `src/main/index.ts`
- Renderer: look in the Vite/Webpack config for the HTML entry, then find the `<script>` tag or the framework's entry point. Usually `src/renderer/main.tsx` or `src/renderer/index.tsx`
- Preload: a bridge script between the two. Defined in `BrowserWindow` → `webPreferences.preload`. Not a separate process — it's part of main but runs in a special context

**Communication:** IPC (Inter-Process Communication) via `ipcMain.handle` / `ipcRenderer.invoke`. This is the Electron-specific mechanism, conceptually similar to chrome.messages.

**Key gotcha:** Code in separate `packages/` directories that gets imported into main process runs INSIDE the main process. It's not a third process. Always verify by checking where it's actually imported and executed.

## Browser Extensions (Manifest V3)

**Processes:** Typically 3 independent contexts:
- Content script — injected into the web page, shares the page's DOM
- Service worker (background.js) — runs independently, handles events and API calls
- Popup or sidepanel — a separate small web page with its own HTML/JS

**Entry files:**
- All defined in `manifest.json`:
  - `background.service_worker` → the service worker entry
  - `content_scripts[].js` → content script entries
  - `side_panel.default_path` or `action.default_popup` → popup/sidepanel HTML, then find its `<script>`

**Communication:** `chrome.runtime.sendMessage` / `chrome.runtime.onMessage` between all contexts. Content scripts can also use `chrome.runtime.connect` for persistent connections.

## Web Apps (Client + Server)

**Processes:** At minimum 2 — the browser tab (client) + the server process. Could be more if there are background workers, WebSocket servers, etc.

**Entry files:**
- Client: find the bundler config (Vite, Webpack, etc.) → entry point. Usually `src/main.ts` or `src/index.tsx`
- Server: look at `package.json` → `scripts.start` or `scripts.dev`. Usually `src/server.ts`, `src/index.ts`, or `app.ts`

**Communication:** HTTP requests (REST/GraphQL) and/or WebSocket connections.

## Next.js / Remix / Full-Stack Frameworks

**Processes:** Blurry line — the framework runs both server and client code, but they're still separate runtimes. The server runs in Node.js, the client runs in the browser.

**Entry files:**
- These frameworks use file-based routing, so there's no single entry file in the traditional sense
- Server logic: look for `app/api/` routes, `getServerSideProps`, server actions, or `loader` functions
- Client logic: page components and client components

**How to handle:** Focus on the route handlers (server functions) and page components (client components) as the objective units, rather than trying to find entry files. The framework IS the entry point.

## Mobile Apps (React Native / Flutter)

**Processes:** Usually 1 main process (the app), but could have background services.

**Entry files:**
- React Native: `index.js` → registers the root component, usually `App.tsx`
- Flutter: `lib/main.dart` → `runApp()`

## Monorepos

**Key question:** How many of the packages in the monorepo actually run as separate processes?

Often a monorepo has one app and several library packages. The library packages are NOT separate processes — they get compiled into whatever app imports them. Treat them as part of the process that imports them.

Only count a package as a separate process if it has its own entry point that runs independently (its own server, its own CLI, etc.).
