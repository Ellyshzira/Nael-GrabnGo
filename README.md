# Nael Snacks Bar — Stock & Cash-Flow App

A phone app for both partners to record **stock checks** and **money (cash flow)** for the drinks & snacks boxes at Office, Tasik Cyberjaya and Putrajaya.

**Total cost: RM0.** It uses:

| Part | What it does | Free service |
|---|---|---|
| Google Sheet + Apps Script | Stores all data, shared by both of you | Google (your Gmail) |
| `web/` folder | The app itself (installs on the phone like a normal app) | GitHub Pages |

It works **offline** too: at the lakeside with weak signal, save as normal and it uploads when you're back online.

```
grabngo/
├── apps-script/Code.gs     ← paste into Google Apps Script (the backend)
└── web/                    ← upload to GitHub Pages (the app)
    ├── index.html, logo.png, icons
    ├── manifest.json
    ├── sw.js
    ├── icon-192.png
    └── icon-512.png
```

---

## Step 1 — Create the Google Sheet backend (about 10 minutes, use a laptop once)

1. Go to **sheets.google.com** → create a **Blank spreadsheet**. Name it `Nael Snacks Bar Data`.
2. Menu **Extensions → Apps Script**. A code editor opens.
3. Delete everything in `Code.gs`, then paste the whole content of `apps-script/Code.gs`.
4. On line 10, change `const APP_PIN = '1234';` to your own PIN (e.g. `'738241'`). Only you and your partner should know it.
5. Click 💾 **Save**.
6. In the toolbar, select the function **`setup`** and press **▶ Run**.
   - Google asks for permission → **Review permissions** → pick your account.
   - If you see "Google hasn't verified this app": click **Advanced → Go to (project name) (unsafe)** → **Allow**. This is normal: it's your own script, accessing only your own sheet.
   - Go back to the spreadsheet tab: you'll now see tabs `Products`, `Locations`, `Visits`, `Stock`, `Money`, `Settings`.
7. Click **Deploy → New deployment**.
   - Click the ⚙️ next to "Select type" → **Web app**.
   - Description: `v1`
   - Execute as: **Me**
   - Who has access: **Anyone** *(needed so the app can reach it; the PIN protects your data)*
   - Click **Deploy** → copy the **Web app URL** (ends in `/exec`). Keep it for Step 3.
8. Share the Google Sheet with your partner (**Share** button) so both of you can also open the raw data any time.

> **If you edit Code.gs later:** Deploy → **Manage deployments** → ✏️ edit → Version: **New version** → Deploy. The URL stays the same.

---

## Step 2 — Put the app online for free with GitHub Pages (about 10 minutes)

1. Create a free account at **github.com** (if you don't have one).
2. Click **+ → New repository**. Name: `grabngo`. Set **Public** (GitHub Pages is free for public repos). Click **Create repository**.
3. On the new repo page click **uploading an existing file**. Drag in **all 8 files inside the `web/` folder** (`index.html`, `manifest.json`, `sw.js`, `logo.png`, `apple-touch-icon.png`, `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`) — the files themselves, not the folder. Click **Commit changes**.
4. Go to **Settings → Pages**. Under "Build and deployment": Source = **Deploy from a branch**, Branch = **main**, folder **/ (root)** → **Save**.
5. Wait 1–2 minutes and refresh. Your app link appears at the top, like:
   `https://YOUR-USERNAME.github.io/grabngo/`

> **Is it safe that the repo is public?** Yes. The code contains **no URL and no PIN** — those are typed into each phone and stored only on that phone. Anyone who finds the page just sees an empty demo.

**No GitHub?** Alternative: go to **app.netlify.com/drop** and drag the `web` folder in. Also free, gives you a link instantly.

---

## Step 3 — Install on both phones

1. Open your app link on the phone.
   - **Android:** use **Chrome** → ⋮ menu → **Add to Home screen / Install app**.
   - **iPhone:** use **Safari** → Share button → **Add to Home Screen**.
2. Open the app from the home screen icon. Tap your name (Lysa / Meo).
3. Go to **More → Connection**: paste the **Web app URL** from Step 1 and your **PIN** → **Save & connect**. The top pill should say **Synced ✓**.
4. **First phone only:** in **More**, tap **Import old Excel records**. This loads your 11 products, 3 locations and the 46 money records from your Excel (up to 18 Aug). Your balance should show **RM835.97**, with the business owing **Lysa RM56.80** and **Meo RM418.41**, same as the Excel.
5. Second phone: just connect (step 3); the data appears automatically.

---

## Step 4 — Daily use

### 📦 At a location → Stock check
1. Home → tap the location (or **Stock check** tab → pick location).
2. For each item: type or tap −/+ for **Left in box**, and **Added now** if you top up.
3. Enter **Cash in box** and **QR received** since the last check.
4. **Save**. The app works out:
   - **Sold** = last count − left now
   - **Expected money** = sold × price
   - **Short / Fully paid** = collected − expected (this tells you how honest the honesty box is)
   - New stock = left + added

> The **first** check at each location only sets the starting stock (nothing counted as sold). Start counting properly from the second visit.

### 💰 Money tab — same rules as your Excel
| Type | When | Effect |
|---|---|---|
| Sales Deposit | Sales money goes into the business account (added automatically by a stock check if ticked) | Account ↑ |
| Restock | Buying stock | Business account → account ↓ · Own money → business owes you ↑ |
| Expense | Ice, box, poster, fees | Same as Restock |
| Reimbursement | Business account pays a partner back | Account ↓ and owed ↓ |
| Capital In | A partner puts own money into the account | Account ↑ and owed ↑ |
| Adjustment | Make the app match the bank (minus amount to reduce) | Account ↑/↓ |

### 🏷️ Products
Enter **units per pack** and **pack cost**; the app calculates cost per unit, margin, and suggested prices for 30/40/50% margin. Untick "show in stock check" for items you stop selling.

---

## Best practices for an honesty-box business

1. **Count at every visit, never estimate.** Your sales figures come from counts, so a 2-minute count each visit is what makes everything else accurate.
2. **Fixed rhythm per location.** E.g. Tasik daily/every 2 days (busy, outdoor), Putrajaya every 2–3 days, office twice a week. The Home screen shows a "Due" badge after 3 days.
3. **Track the paid % (Home screen).** Honesty-box spots commonly lose some money. If one location keeps falling below ~85%:
   - put the price list + QR at eye level, large and clear ("RM2 each — scan or drop cash"),
   - use round prices (RM1 / RM2 / RM3) so people don't skip paying for lack of change,
   - add a lockable cash slot instead of an open box,
   - sell more of the items that pay well there, fewer of the ones that go missing.
4. **Prefer QR (DuitNow).** It goes straight to the business account, no cash handling, and leaves a record. Check the bank app's transaction list when you do the stock check.
5. **One business bank account for everything.** Personal purchases → record as *Own money* the same day, and repay with *Reimbursement*. No mixing.
6. **Weekly 5-minute review** (e.g. Sunday): bank balance vs app balance (fix with Adjustment), the top-up list, and which items have the best margin per location.
7. **Margins:** aim for at least 30–40% on snacks and 40%+ on drinks (ice and spoilage cost extra — enter them as Expense, or as "extra cost per pack").
8. **Rotate stock:** older stock on top/front (first in, first out), and check expiry dates during counts — especially instant food and milk drinks in the heat.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| "Wrong PIN" | The PIN in the app must equal `APP_PIN` in Code.gs. If you changed it after setup, run `setup` again. |
| "Sync failed" | Check the URL ends with `/exec` and the deployment's access is **Anyone**. Edits are kept on the phone and retried automatically. |
| Changes to Code.gs don't apply | Deploy → Manage deployments → edit → **New version**. |
| App doesn't show my changes to index.html | In `sw.js` change `grabngo-v1` to `grabngo-v2`, upload both files, then close and reopen the app. |
| Wrong record | Tap it in Money to edit or delete. Wrong stock check: Stock check tab → tap it → Delete. |

You can always open the Google Sheet directly to see or fix raw data. Don't rename the tabs or the header row.
