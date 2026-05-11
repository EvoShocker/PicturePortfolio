# Cade Photography Portfolio

Two ways to edit:
1. **Admin dashboard** at `yoursite.com/admin` — drag-drop photos, edit captions, no code (use this after setup)
2. **Direct edit of `photos.json`** — for quick fixes

After deployment, you'll only ever use the admin dashboard.

---

## Files

- `index.html` — the website (don't edit)
- `photos.json` — your photo data (admin writes to this)
- `images/` — image files (admin uploads here)
- `admin/` — dashboard code (don't edit except as noted below)

---

## Part 1: Deploy + set up admin (one time, ~30 min)

### Step 1: GitHub

1. [github.com](https://github.com) → sign in / sign up.
2. **+** top right → **New repository**.
3. Name: `portfolio`. Set **Public**. Don't add README/license. Create.
4. On next page, click **uploading an existing file**.
5. Drag entire portfolio folder contents (`index.html`, `photos.json`, `README.md`, `images/`, `admin/`).
6. **Commit changes**.

### Step 2: Update the admin config

1. In your repo, open `admin/config.yml` → click pencil to edit.
2. Find: `repo: YOUR-USERNAME/YOUR-REPO-NAME`
3. Replace with your actual values, e.g. `repo: cade42/portfolio`.
4. **Commit changes**.

### Step 3: Vercel

1. [vercel.com](https://vercel.com) → **Continue with GitHub**.
2. **Add New** → **Project** → find your repo → **Import** → **Deploy**.
3. Wait ~30 sec. You get a URL like `cade-photo.vercel.app`.

### Step 4: Connect admin to GitHub (auth)

The admin needs permission to save changes. Decap uses Netlify Identity (free) for this.

1. [netlify.com](https://netlify.com) → sign up.
2. **Add new site** → **Deploy manually** → drag your portfolio folder in. (Just for auth; real site stays on Vercel.)
3. Your Netlify site → **Identity** → **Enable Identity**.
4. **Settings** → **Registration** → set to **Invite only**.
5. **Services** → **Git Gateway** → **Enable Git Gateway**.
6. **Identity** → **Invite users** → invite your own email.
7. Check email, click invite link, set password.
8. In your **GitHub** repo, edit `admin/index.html`. Add before `</head>`:
   ```html
   <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
   ```
   Add before `</body>`:
   ```html
   <script>
     if (window.netlifyIdentity) {
       window.netlifyIdentity.on("init", user => {
         if (!user) {
           window.netlifyIdentity.on("login", () => document.location.href = "/admin/");
         }
       });
     }
   </script>
   ```
9. Commit. Vercel rebuilds.

### Step 5: Use it

Go to `yoursite.com/admin/`. Login.

---

## Part 2: Daily editing (after setup)

**Add a photo:** Photos → Gallery → scroll to bottom → **Add Photos** → upload + fill fields → Save → Publish.

**Edit caption:** Click photo in list → change text → Save → Publish.

**Reorder:** Drag photos by handle in the list.

**"Print Available" badge:** Toggle **Featured** on the photo.

**Edit hero/about/footer text:** Site Text section in the gallery editor.

Site rebuilds in ~30 sec after each save.

---

## Part 3: Image prep before uploading

Lightroom export: JPEG, sRGB, quality 80–85, long edge 2000px, under 1MB target.

---

## Visual changes I (Claude) handle

Dashboard handles content. Design changes need code — ask me when you want:
- Fonts, colors, layout, animations
- New page types (blog, series pages)

5 min each. Just describe.

---

## Troubleshooting

**Admin blank** — Check `admin/config.yml` `repo:` line matches your username/repo exactly.

**"Failed to load entries"** — Git Gateway not enabled, or you're not invited. Recheck Step 4.

**Site shows "Could not load photos.json"** — You opened `index.html` from hard drive. Only works deployed. To preview locally: Terminal in portfolio folder, run `python3 -m http.server`, visit `http://localhost:8000`.

**Photo missing after upload** — Wait 1-2 min for rebuild. If still gone, check `src` in `photos.json` matches actual filename.
