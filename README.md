# Lady Land — فروشگاه اینترنتی لاکچری

فروشگاه اینترنتی مستقل برای برند فشن زنانه Lady Land.  
ساخته‌شده با **React + Vite**، بدون نیاز به سرور یا پایگاه داده خارجی.

---

## ✅ ویژگی‌ها

| بخش | وضعیت |
|-----|--------|
| فروشگاه با فیلتر و جستجو | ✅ |
| صفحه محصول با گالری تصویر | ✅ |
| سبد خرید (ماندگار بعد از رفرش) | ✅ |
| فرآیند پرداخت ۳ مرحله‌ای | ✅ |
| لیست علاقه‌مندی‌ها | ✅ |
| پنل مدیریت کامل | ✅ |
| آپلود تصویر واقعی (input type=file) | ✅ |
| ذخیره داده در localStorage | ✅ |
| آماده اتصال به Supabase | ✅ |

---

## 🚀 اجرای محلی

### پیش‌نیازها
- Node.js نسخه ۱۸ یا بالاتر
- npm یا yarn

### مراحل

```bash
# ۱. کلون یا دانلود پروژه
cd ladyland

# ۲. نصب وابستگی‌ها
npm install

# ۳. اجرای سرور توسعه
npm run dev
```

سایت روی `http://localhost:3000` باز می‌شود.

### ساخت نسخه Production

```bash
npm run build
```

فایل‌های آماده در پوشه `dist/` قرار می‌گیرند.

### پیش‌نمایش نسخه Build‌شده

```bash
npm run preview
```

---

## 🔑 ورود به پنل مدیریت

پایین صفحه (Footer) روی لینک **ADMIN ↗** کلیک کنید.  
هیچ رمزعبوری لازم نیست — در حال حاضر احراز هویت پیاده‌سازی نشده است.

> **نکته امنیتی:** قبل از انتشار عمومی، سیستم احراز هویت اضافه کنید.

---

## 🗄️ پایگاه داده

### وضعیت فعلی — localStorage

تمام داده‌ها در `localStorage` مرورگر ذخیره می‌شوند:

| کلید | محتوا |
|------|-------|
| `ladyland__products` | محصولات |
| `ladyland__orders` | سفارش‌ها |
| `ladyland__settings` | تنظیمات فروشگاه |
| `ladyland__cart` | سبد خرید |
| `ladyland__wishlist` | علاقه‌مندی‌ها |

### آماده‌سازی برای Supabase

تمام منطق پایگاه داده در **یک فایل** قرار دارد:

```
src/services/database.js
```

برای انتقال به Supabase:

1. نصب کتابخانه:
   ```bash
   npm install @supabase/supabase-js
   ```

2. ایجاد فایل `src/services/supabase.js`:
   ```js
   import { createClient } from '@supabase/supabase-js'
   export const supabase = createClient(SUPABASE_URL, SUPABASE_KEY)
   ```

3. جایگزینی توابع در `database.js`:
   ```js
   export async function getProducts() {
     const { data } = await supabase.from('products').select('*')
     return data
   }
   ```

4. برای تصاویر، `uploadImages()` را به Supabase Storage وصل کنید — بقیه کد تغییری نمی‌کند.

---

## 🌐 Deploy روی Vercel

### روش اول — GitHub + Vercel (توصیه‌شده)

```bash
# Push به GitHub
git init
git add .
git commit -m "feat: Lady Land store"
git push origin main
```

سپس در [vercel.com](https://vercel.com):
1. **Add New Project**
2. مخزن GitHub را انتخاب کنید
3. تنظیمات پیش‌فرض را تأیید کنید (Vite را شناسایی می‌کند)
4. **Deploy** کنید

### روش دوم — Vercel CLI

```bash
npm install -g vercel
vercel --prod
```

### روش سوم — فایل‌های dist

```bash
npm run build
# پوشه dist را روی هر هاست استاتیک آپلود کنید
# Netlify, GitHub Pages, Firebase Hosting ...
```

---

## 📁 ساختار پروژه

```
ladyland/
├── index.html
├── package.json
├── vite.config.js
├── vercel.json
└── src/
    ├── App.jsx              ← نقطه ورود، بارگذاری داده، روتر
    ├── main.jsx
    ├── index.css            ← استایل‌های global (CSS variables)
    │
    ├── constants/
    │   └── index.js         ← دسته‌بندی‌ها، رنگ‌ها، داده اولیه
    │
    ├── services/
    │   ├── database.js      ← لایه DB (localStorage → Supabase‌آماده)
    │   └── helpers.js       ← توابع کمکی (money, salePrice, uid)
    │
    ├── store/
    │   ├── context.js       ← React Context
    │   └── reducer.js       ← App State (useReducer)
    │
    ├── components/
    │   ├── Header.jsx
    │   ├── Footer.jsx       ← شامل Ticker
    │   ├── CartDrawer.jsx
    │   ├── Toast.jsx
    │   ├── ProductCard.jsx
    │   ├── ProductForm.jsx  ← فرم افزودن/ویرایش محصول
    │   └── ImageUploader.jsx← آپلود واقعی با FileReader
    │
    └── pages/
        ├── ShopPage.jsx     ← فروشگاه اصلی + فیلترها
        ├── ProductPage.jsx  ← صفحه محصول
        ├── CheckoutPage.jsx ← فرآیند خرید ۳ مرحله
        ├── SuccessPage.jsx  ← تأیید سفارش
        ├── WishPage.jsx     ← علاقه‌مندی‌ها
        └── AdminPage.jsx    ← پنل مدیریت
```

---

## 🛤️ نقشه راه آینده

- [ ] احراز هویت (Supabase Auth)
- [ ] درگاه پرداخت (زرین‌پال / میلیون)
- [ ] پروفایل مشتری
- [ ] سیستم کوپن تخفیف
- [ ] اعلان‌های ایمیل
- [ ] داشبورد تحلیلی

---

## 📄 لایسنس

MIT — Lady Land 1404
