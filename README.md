# ⚡ KOMIKU — GitHub + Cloudflare Pages Deployment Guide

## 📁 Project Folder Structure

```
mangazone/
├── index.html        ← Main site (provided above)
├── _redirects        ← Cloudflare SPA routing fix
└── README.md         ← This file
```

## 📄 `_redirects` File Content

Create a file named `_redirects` (no extension) with this single line:
```
/* /index.html 200
```
This ensures all routes redirect to index.html correctly.

---

## 🚀 Step-by-Step Deployment

### Step 1 — GitHub Repository үүсгэх
1. [github.com](https://github.com) руу орж **New repository** дарна
2. Repository нэр өгнө (жнь: `mangazone`)
3. **Public** сонгоно → **Create repository** дарна

### Step 2 — Files upload хийх
```bash
# Terminal ашиглавал:
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/mangazone.git
git push -u origin main
```
Эсвэл GitHub дээр **Upload files** товчоор шууд drag & drop хийж болно.

### Step 3 — Cloudflare Pages холбох
1. [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** → **Create**
2. **Pages** tab → **Connect to Git**
3. GitHub account authorize хийнэ
4. `mangazone` repository сонгоно
5. Build settings:
   - **Framework preset:** `None`
   - **Build command:** *(хоосон орхино)*
   - **Build output directory:** `/` эсвэл хоосон
6. **Save and Deploy** дарна

### Step 4 — Custom Domain (заавал биш)
Cloudflare Pages → **Custom domains** tab → өөрийн domain нэмнэ

---

## 🔄 Update хийх (код өөрчлөхөд)
```bash
git add .
git commit -m "Update"
git push
```
GitHub push хийхэд Cloudflare автоматаар шинэ version deploy хийнэ. ✅

---

## ⚠️ Анхаарах зүйлс
- MangaDex API нь **rate limit** тай — хэт олон request илгээхгүй байх
- Зөвхөн `contentRating[]=safe` контент харуулна (default)
- API нь заримдаа удаан байж болно — энэ нормаль

## 🛠 Өргөтгөх санаанууд
- [ ] User authentication (Cloudflare Workers + KV)
- [ ] Bookmark/favorites (localStorage)
- [ ] Reading history
- [ ] PWA (offline support)
- [ ] Dark/Light mode toggle
