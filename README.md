# ⚡ KOMIKU — Setup Guide

## 📁 Folder Structure
```
komiku/
├── index.html     ← Main site
├── _redirects     ← Cloudflare routing (/* /index.html 200)
└── README.md
```

---

## 🗄️ STEP 1 — Supabase тохируулах (Auth + Database)

### 1.1 Project үүсгэх
1. [supabase.com](https://supabase.com) → **Start your project** → GitHub-аар нэвтрэх
2. **New project** → нэр өгнө, нууц үг тавина, region: **Southeast Asia** сонгоно
3. Project үүсэхийг хүлээнэ (1-2 мин)

### 1.2 Database tables үүсгэх
**SQL Editor** → **New query** → дараах кодыг paste хийж **Run** дарна:

```sql
-- Reading history table
CREATE TABLE reading_history (
  id          uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id     uuid REFERENCES auth.users(id) ON DELETE CASCADE,
  manga_id    text NOT NULL,
  manga_title text,
  manga_genres text[] DEFAULT '{}',
  chapter_id  text,
  read_at     timestamptz DEFAULT now(),
  UNIQUE(user_id, manga_id)
);

-- Bookmarks table
CREATE TABLE bookmarks (
  id          uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id     uuid REFERENCES auth.users(id) ON DELETE CASCADE,
  manga_id    text NOT NULL,
  manga_title text,
  cover_url   text,
  manga_type  text DEFAULT 'manga',
  added_at    timestamptz DEFAULT now(),
  UNIQUE(user_id, manga_id)
);

-- Row Level Security (хэрэглэгч зөвхөн өөрийн өгөгдлийг харна)
ALTER TABLE reading_history ENABLE ROW LEVEL SECURITY;
ALTER TABLE bookmarks ENABLE ROW LEVEL SECURITY;

CREATE POLICY "own_history" ON reading_history FOR ALL USING (auth.uid() = user_id);
CREATE POLICY "own_bookmarks" ON bookmarks FOR ALL USING (auth.uid() = user_id);
```

### 1.3 URL болон Key авах
**Project Settings** → **API** → дараах 2 утгыг copy хийнэ:
- **Project URL** → `https://xxxx.supabase.co`
- **anon / public key** → `eyJhbGci...` (урт key)

### 1.4 index.html дотор тохируулах
`index.html`-ийн дээд хэсэгт:
```javascript
const SB_URL = 'https://xxxx.supabase.co';      // ← өөрийнхөө URL
const SB_KEY = 'eyJhbGciOiJIUzI1NiIs...';       // ← өөрийнхөө anon key
```

### 1.5 Email auth идэвхжүүлэх (заавал)
**Authentication** → **Providers** → **Email** → **Enable** → Save

> 💡 "Confirm email" унтраавал шууд нэвтрэх боломжтой болно:
> Auth → Settings → **Email Confirmations** → OFF

---

## 🚀 STEP 2 — GitHub дээр байршуулах

```bash
git init
git add .
git commit -m "Initial KOMIKU commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/komiku.git
git push -u origin main
```

---

## ☁️ STEP 3 — Cloudflare Pages deploy

1. [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** → **Create**
2. **Pages** tab → **Connect to Git** → GitHub authorize
3. `komiku` repository сонгоно
4. Build settings:
   - Framework preset: `None`
   - Build command: *(хоосон)*
   - Output directory: *(хоосон)*
5. **Save and Deploy** ✅

Deploy дууссаны дараа: `komiku.pages.dev` хаягаар нээнэ

---

## ✅ Засагдсан зүйлс

| Асуудал | Шийдэл |
|---|---|
| "Failed to load" | `availableTranslatedLanguage` filter хасав, retry logic нэмэв |
| Site нэр | KOMIKU болгов |
| Login / Register | Supabase Auth (email + password) |
| Bookmark ♥ | Supabase-д хадгалагдана |
| Reading history | Бүлэг нээхэд автоматаар хадгалагдана |
| Recommendations ✨ | Уншсан жанруудад тулгуурлан санал болгоно |
| Mobile design | 3-column grid, bottom navigation |

---

## 🔄 Код шинэчлэх

```bash
git add .
git commit -m "Update"
git push
```
GitHub push → Cloudflare автоматаар шинэ version deploy хийнэ ✅
