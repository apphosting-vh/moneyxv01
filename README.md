# finsight — Modular Build

## File Structure
```
/
├── index.html          # Shell: head, CSS, SW registration, protection layer
├── sw.js               # Service Worker (cache: mm-v3-48-1)
├── changelog.js        # Lazy-loaded on Info tab open
└── js/
    ├── app-utils.js         # Hooks, formatting, price/NAV fetchers
    ├── app-state.js         # INIT, constants, icons, categories, reducer
    ├── app-ui-base.js       # Charts, UI primitives, TxLedger, VirtualList
    ├── app-transactions.js  # TxModal, TxPanel, PayeeCombobox, CatCombobox
    ├── app-dashboard.js     # Dashboard, search modal, calendar widget
    ├── app-accounts.js      # BankSection, CardSection, CashSection
    ├── app-invest.js        # InvestDashboard, InvestSection, MF/FD modals
    ├── app-loans.js         # LoanSection, PrepaySimModal, ConfirmModal
    ├── app-reports.js       # ReportsSection, ExportReportModal
    ├── app-settings.js      # SettingsSection, LS_KEY, loadState/saveState
    ├── app-sections.js      # CalendarSection, GoalsSection, InsightsSection
    └── app-main.js          # InfoSection, TaxEstimatorSection, App, ReactDOM
```

## GitHub Pages Deployment
1. Push all files maintaining the directory structure above
2. Enable GitHub Pages from Settings → Pages → source: main branch / root
3. Clear any old service worker in Chrome: DevTools → Application → SW → Unregister

## What Changed from Monolith
- `index.html`: the single 28,000-line `<script type="text/babel">` block replaced
  with 12 `<script type="text/babel" src="./js/...">` tags (no `defer` — Babel
  standalone fetches and executes external babel scripts sequentially via XHR)
- `sw.js`: cache name bumped mm-v3-48-0 → mm-v3-48-1 (forces old cache purge)
  + all 12 JS files added to PRECACHE_URLS for offline support

## Integrity Verification (done)
- ✅ 1,777,482 chars reassembled exactly match original 1:1
- ✅ All 30 cross-boundary symbol checks passed
- ✅ Load order verified: all top-level consts used only in later files
- ✅ No top-level forward references (backward refs are all inside function bodies)
- ✅ LS_KEY, loadState, saveState, usePersistentReducer all in app-settings.js
   which loads before app-main.js (App function) ✅
