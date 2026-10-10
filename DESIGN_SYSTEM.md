# DESIGN_SYSTEM.md — سیستم طراحی Mohammad Mehdi Sadeghi

> مستند رسمی و بسیار دقیق سیستم طراحی پورتفولیوی شخصی — استخراج‌شده از کدبیس واقعی پروژه (`src/`).
> هر عدد، هر کلاس Tailwind، هر keyframe و هر الگو در اینجا بر مبنای فایل‌های واقعی پروژه ثبت شده است.
> نسخه: 1.0 — تاریخ: ۱۸ مهر ۱۴۰۵ (October 10, 2026)

---

## ۱. فلسفه و اصول بنیادی

### ۱.۱ زیبایی‌شناسی (Aesthetic)
- **Theme**: یک‌تمپلیت «Dark Cyber-Dev Terminal». رنگ‌ها از پالت Tailwind `slate` + آکسان‌های نئونی (`#615FFF` بنفش، `#00D5BE` فیروزه‌ای، `#FFB86A` نارنجی گرم).
- **الهام‌بخش**: VS Code / IDE مدرن (ردیف دکمه‌های ترمینال، شماره‌ خطوط، monogram، کدهای رنگی syntax-highlight، تگ‌های `//` و `>`).
- **لحن (Tone)**: «حرفه‌ای اما شخصی» — مخاطب حس می‌کند با یک دولوپر واقعی طرف است، نه یک قالب شرکتی.

### ۱.۲ اصول طراحی (Design Principles)
1. **ترمینال-محور بودن**: تقریباً هر کامپوننت بزرگ یک «هدر ترمینال» با سه دکمه‌ی قرمز/زرد/سبز دارد.
2. **زبان برنامه‌نویسی به‌عنوان زبان UI**: شماره خطوط، کامنت‌های `//`، پرامپت‌های `$>`, `const`, `=>` به‌جای تیترهای ساده استفاده می‌شوند.
3. **شفافیت (Glass)**: کارت‌ها با backdrop-filter و blur روی پس‌زمینه‌های blur واقعی کار می‌کنند.
4. **عدم استفاده از رنگ‌های متفرقه**: تمام رنگ‌ها در ۱۵ توکن اصلی خلاصه می‌شوند (به بخش ۳ مراجعه کنید).
5. **انیمیشن با صرفه‌جویی در مصرف CPU**: تمام keyframeها GPU-friendly (`transform`/`opacity`) هستند و در `prefers-reduced-motion: reduce` غیرفعال می‌شوند.
6. **شخصی‌سازی عمیق Cursor**: نشانگر ماوس به‌کلی حذف و با یک liquid-glass bead + ring جایگزین می‌شود (فقط روی `pointer: fine`).

---

## ۲. ستون‌های تکنولوژی (Tech Stack)

| لایه | تکنولوژی | نسخه |
|---|---|---|
| Framework UI | React | 19.2 |
| Router | react-router-dom | 7.14 |
| Build tool | Vite | 7.2 |
| CSS framework | Tailwind CSS (v4) | 4.2 |
| Server | Express | 5.2 |
| DB | postgres (نود درایور) | 3.4 |
| File uploads | multer + adm-zip | latest |
| Tel/Geo | libphonenumber-js + react-select-country-list | latest |
| Animation helpers | typeit-react | 2.7 |
| Lint | ESLint v9 (flat config) | 9.39 |

### ۲.۱ نکات مهم Tailwind v4
- **بدون فایل `tailwind.config.js`** — همه‌چیز در `@import "tailwindcss";` در `src/index.css` و کلاس‌های utility تعریف می‌شود.
- استایل سراسری با CSS سفارشی زیر همان `@import` قرار دارد.
- از پلاگین `@tailwindcss/vite` (نه PostCSS) استفاده می‌شود.

---

## ۳. توکن‌های رنگ (Color Tokens)

> **قانون طلایی**: هر رنگی که در پروژه استفاده می‌شود یا یکی از این ۱۵ توکن است یا از روی آن‌ها با opacity ساخته شده (`#90a1b977`، `#615FFF11` و ...). هیچ رنگ تصادفی یا متفرقه‌ای وجود ندارد.

### ۳.۱ توکن‌های پایه

| نام معنایی | HEX | RGB | کاربرد |
|---|---|---|---|
| **bg-primary** | `#0F172B` | (15, 23, 43) | پس‌زمینه اصلی تمام صفحات، Header، Footer |
| **bg-secondary** | `#081224` | (8, 18, 36) | کارت‌ها، Sidebar، آیتم‌های inactive، Skill cards |
| **bg-deep** | `#030712` | (3, 7, 18) | BootLoader، غشای صفحه‌ی transition |
| **bg-input** | `#020618` | (2, 6, 24) | input/textarea/select در فرم‌ها |
| **bg-card-translucent** | `#0B1222` (با opacity 95%) | – | کارت پلیر موزیک |
| **bg-modal** | `#091122` | (9, 17, 34) | Modal پیام موفقیت، ترمینال کارت |
| **bg-overlay** | `#060D1A` (با opacity 80%) | – | هدر Modal |
| **text-primary** | `#FFFFFF` | (255, 255, 255) | تیترها، متن فعال، اسامی |
| **text-secondary (gray)** | `#90A1B9` | (144, 161, 185) | متن ثانویه، لیبل‌ها، inactive nav |
| **text-muted** | `#68768C` | (104, 118, 140) | شماره‌خطوط، placeholder meta، timestamp |
| **text-fade** | `#4B576D` | (75, 87, 109) | Placeholder input، متن خیلی کم‌رنگ |
| **text-fade-2** | `#3d4f6b` | (61, 79, 107) | شماره آیتم Skills/Education timeline، فوتر ثانویه |
| **border-default** | `#90a1b933` / `#90a1b955` / `#90a1b966` / `#90a1b977` / `#90a1b920` | – | border استاندارد (شفافیت‌های مختلف) |
| **border-strong** | `#314158` | (49, 65, 88) | border input، کارت‌های عمیق‌تر |
| **accent-purple** | `#615FFF` | (97, 95, 255) | رنگ اصلی برند — دکمه‌ها، لینک فعال، Progress bar، focus، monogram پروژه‌ها |
| **accent-purple-2** | `#7C7BFF` / `#7C6CF6` | – | gradient همراه با accent-purple |
| **accent-purple-hover** | `#7573FF` | – | hover روی دکمه‌های بنفش |
| **accent-purple-gradient-end** | `#4F46E5` | – | انتهای gradient در دکمه success modal |
| **accent-turquoise** | `#00D5BE` | (0, 213, 190) | Mood OK، success badge، لینک github، تایم‌لاین Education |
| **accent-orange** | `#FFB86A` | (255, 184, 106) | border فعال Tab nav، اکسان loading dots قدیمی |
| **accent-pink** | `#FFA1AD` | – | آیتم لینک GitHub در Hero (InformationText) |
| **accent-purple-text** | `#A5B4FC` | – | متن subtitle در success modal |
| **accent-cyan-dot** | `#38BDF8` | – | دات سوم loading در BootLoader |
| **danger** | `#FF637E` / `#FF6B6B` | – | 404 page، error text |
| **success** | `#4ADE80` | – | پیام موفقیت فرم contact |
| **danger-bg-soft** | `#FF6B6B14` / `#FF6B6B33` | – | background خطا |
| **warning-amber** | `#F59E0B` | – | nav admin، دکمه‌ی زرد ترمینال |
| **error-red** | `#EF4444` | – | نقطه‌ی قرمز ترمینال |
| **success-green** | `#10B981` / `#27C93F` | – | نقطه‌ی سبز ترمینال |
| **macos-red** | `#FF5F56` | – | دکمه‌ی قرمز پنجره (Admin/Modal/terminal) |
| **macos-yellow** | `#FFBD2E` | – | دکمه‌ی زرد پنجره |
| **macos-green** | `#27C93F` | – | دکمه‌ی سبز پنجره |
| **transparent-purple** | `#615FFF11` / `#615FFF25` / `#615FFF66` | – | background hover/active روی کارت‌ها، corner brackets |

### ۳.۲ پس‌زمینه‌های شفاف رایج
- Hover روی آیتم‌های sidebar: `#7888a033` (با opacity 20%)
- Hover ضعیف: `#7888a01a` (با opacity 10%)
- hover بسیار ضعیف: `#7888a00d`
- Background active filter: `#7888a033` + متن سفید

### ۳.۳ سایه‌های آکسان (Glow shadows)
```css
/* drop shadow روی monogram */
box-shadow: 0 6px 24px rgba(97,95,255,0.4);

/* hover glow روی کارت پروژه */
box-shadow: 0 0 35px -5px rgba(97,95,255,0.25);

/* glow نقاط تنظیم‌کننده صدا */
box-shadow: 0 0 6px rgba(97,95,255,0.7);

/* glow نقاط ترمینال */
box-shadow: 0 0 6px #EF4444 / #F59E0B / #10B981;

/* shadow pulse روی success badge */
box-shadow: 0 0 25px rgba(0, 213, 190, 0.35);   (pulse 3s infinite)
```

---

## ۴. تایپوگرافی (Typography)

### ۴.۱ فونت اصلی (Font Family)
```css
@font-face {
  font-family: "Fira";
  src: url(/assets/Fonts/FiraCode-Medium.ttf);
}
* { font-family: "Fira"; }
```
- **فونت**: `Fira Code Medium` (یک monospace از مجموعه Fira)
- **فایل**: `public/assets/Fonts/FiraCode-Medium.ttf`
- **اعمال سراسری**: با `* { font-family: "Fira"; }` در `src/index.css` (نه Tailwind).
- **استثناها**: `font-mono` (Tailwind) و `fontFamily: "monospace"` در شماره‌ آیتم‌ها.

### ۴.۲ Scale (اندازه‌ها)
| کلاس Tailwind | px | کاربرد |
|---|---|---|
| `text-[8px]` | 8 | badge زبان روی کارت پروژه |
| `text-[9px]` | 9 | – |
| `text-[10px]` | 10 | badge، placeholder مدیریت، metric |
| `text-[11px]` | 11 | شماره‌خط، شماره index، sub-label |
| `text-[12px]` | 12 | متن کمکی، کامنت، label |
| `text-[13px]` | 13 | body متن، توضیح کارت |
| `text-[14px]` | 14 | nav موبایل، body، دکمه |
| `text-[15px]` | 15 | تیتر skill، توضیح project card |
| `text-[16px]` | 16 | تیتر H3 پروژه، تیتر Modal، page heading |
| `text-[18px]` | 18 | – |
| `text-[20px]` | 20 | success heading |
| `text-[22px]` | 22 | – |
| `text-[26px]` | 26 | H1 موبایل Hero |
| `text-[38px]` | 38 | H1 تبلت Hero |
| `text-[46px]` | 46 | H1 دسکتاپ کوچک Hero |
| `text-[54px]` | 54 | H1 دسکتاپ Hero |
| `text-[58px]` | 58 | H1 دسکتاپ بزرگ Hero (xl) |

### ۴.۳ وزن‌ها (Font Weights)
- `font-mono` — استفاده در همه‌ی کدها، شماره‌خطوط، شمارنده‌ها
- `font-bold` — تیترها (Hero name، Project title، Modal heading، Monogram)
- `font-semibold` — sub-titles (Skill name، Timeline title، Card title)
- `font-medium` — دکمه‌ها، لینک‌ها

### ۴.۴ استایل‌های خاص
- `tracking-[0.06em]` — Wordmark پروژه (حروف باز)
- `tracking-[0.15em]` — Monogram در BootLoader
- `tracking-wider` — badgeهای status
- `tracking-widest` — درصد progress در PageTransition
- `leading-tight` — H1 Hero
- `leading-6` / `leading-7` / `leading-8` — line-height در code-blockها (با ریسپانسیو)

### ۴.۵ تگ‌های معنایی
- `<h1>` فقط برای نام اصلی در Hero (InformationText)
- `<h2>` برای عنوان سایدبار (personal-info, contacts, projects, mini-projects)
- `<h3>` برای عنوان کارت‌ها (Skill name, Education title, Project title)
- `<h4>` برای sub-titles در player و کارت‌ها

---

## ۵. فاصله‌گذاری، سایزبندی و Grid

### ۵.۱ Container اصلی صفحات
تمام صفحات از فرمول ثابت زیر استفاده می‌کنند:
```jsx
<section className="bg-[#0F172B] min-h-[calc(100vh-116px)] ...">
```
- **ارتفاع محاسبه‌شده**: `100vh - 116px` (58px Header + 58px Footer = 116px)
- **دسکتاپ**: `md:h-[calc(100vh-116px)]` با `md:overflow-hidden` + children اسکرول داخلی
- **موبایل**: `min-h-[calc(100vh-116px)]` (اسکرول صفحه‌ای طبیعی)

### ۵.۲ عرض سایدبار (Sidebar widths)
| بریک‌پوینت | عرض |
|---|---|
| mobile (<md) | `w-full` |
| md | `w-60` (240px) |
| lg | `w-72` (288px) |
| xl | `w-[360px]` |
| 2xl | `w-[457px]` |

### ۵.۳ عرض ستون SnakeBar (نوار چپ)
```jsx
<div className="hidden lg:block relative w-14 border-r-[1px] border-[#90a1b977] h-[calc(100vh-116px)]">
```
- فقط در `lg` و بالاتر نمایش داده می‌شود.
- عرض ثابت: `w-14` (56px).

### ۵.۴ سایز Header
```jsx
<header className="sticky top-0 z-50 h-[58px] w-full bg-[#0F172B]">
```
- ارتفاع ثابت 58px در همه‌ی سایزها.
- `z-50` (روی محتوا، زیر Modal و Loader).

### ۵.۵ سایز Footer
```jsx
<footer className="w-full min-h-[58px] bg-[#0F172B] flex items-center justify-center">
```
- ارتفاع حداقل 58px.

### ۵.۶ Padding Patterns
**سکشن اصلی**:
```jsx
// Hero
className="px-4 sm:px-8 py-8 sm:py-12 lg:py-0"

// Sidebar
className="px-4 sm:px-6 md:px-0"      // header wrapper
className="px-4 sm:px-7 py-3.5"        // title in sidebar
className="px-4 sm:px-8 md:px-6 lg:px-10 py-3"  // active items
className="px-5 lg:px-7 py-3.5"       // desktop title
className="px-5 lg:px-8 xl:px-12 py-3"  // desktop items

// Content pane (ProjectSection, Skills, Education, AboutMe)
className="py-6 sm:py-8 px-3 sm:px-6 md:px-8 lg:px-10 lg:px-12"
```

### ۵.۷ Gap Patterns
- **بین المان‌های مرتبط (tight)**: `gap-1`، `gap-1.5`، `gap-2`
- **بین پاراگراف‌ها / کارت‌ها**: `gap-3.5 sm:gap-4 lg:gap-5`
- **بین ستون‌های اصلی**: `gap-8 sm:gap-10 lg:gap-12 xl:gap-16`
- **بین بخش‌های بزرگ**: `gap-2.5 sm:gap-4` در Hero، `gap-8 sm:gap-12 lg:gap-20 xl:gap-28`

### ۵.۸ Grid Systems
**Skills (آیتم‌های مهارت)**:
```jsx
className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-2 xl:grid-cols-3 gap-3.5 sm:gap-4 lg:gap-5"
```

**Project Cards (با fallback برای admin و سایدبار موبایل)**:
```jsx
// ProjectSection (default):
className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-1 lg:grid-cols-2 2xl:grid-cols-3 gap-4 sm:gap-6"

// Poster skeleton (loading):
className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 sm:gap-6"
```

**Skills Loading**:
```jsx
className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-2 xl:grid-cols-3 gap-3.5 sm:gap-4 lg:gap-5"
```

### ۵.۹ Mini/Micro spacing tokens (رایج در پروژه)
- `1`: 4px
- `1.5`: 6px
- `2`: 8px
- `2.5`: 10px
- `3`: 12px
- `3.5`: 14px
- `4`: 16px
- `5`: 20px
- `6`: 24px
- `7`: 28px
- `8`: 32px

### ۵.۱۰ Vertical Center Helper
```jsx
className="md:h-[calc(100vh-116px)] md:overflow-y-auto"
```
+ محتوا با `my-auto` در فرم‌ها.

---

## ۶. گوشه‌های گرد (Radius)

| کلاس | px | کاربرد |
|---|---|---|
| `rounded-sm` | 2 | – |
| `rounded` | 4 | checkbox، small inline |
| `rounded-md` | 6 | input، select، دکمه contact، badge |
| `rounded-lg` | 8 | Skill card، آیتم sidebar hover، sub-box |
| `rounded-xl` | 12 | کارت پروژه، کارت Modal، Music player، Music card |
| `rounded-2xl` | 16 | MusicSearch hero card (`box-3d`) |
| `rounded-full` | 9999 | pills، badge، cursor، دکمه play، visualizer bars |
| `rounded-9999` (custom) | – | progress slider track |

### ۶.۱ corner brackets (ویژه‌ی کارت پروژه)
```jsx
className="absolute top-2.5 left-2.5 w-4 h-4 border-t-2 border-l-2 ..."
// 4 گوشه‌ی کارت پروژه با border-2 تک‌سمت
style={{ borderColor: "#615FFF66", opacity: hovered ? 1 : 0.6 }}
```

---

## ۷. مرزها (Borders)

### ۷.۱ عرض‌های مرز استاندارد
- `border-[1px]` — همه‌ی جداکننده‌ها (sidebar، header، footer، کارت‌ها)
- `border-l-2` — active state در admin sidebar (`border-l-[#FFB86A]`)
- `border-l-4` — active state در mobile menu (`border-l-[#FFB86A]`)
- `border-t-2` / `border-l-2` / `border-r-2` / `border-b-2` — corner brackets کارت پروژه
- `border-b-4` — active nav link (خط زیر لینک فعال)

### ۷.۲ رنگ‌های مرز استاندارد
| شفافیت | HEX | کاربرد |
|---|---|---|
| 5% | `#90a1b920` | border ضعیف در ترمینال |
| 20% | `#90a1b933` | border کارت‌ها inactive |
| 30% | `#90a1b955` | border کارت لینک |
| 40% | `#90a1b966` | checkbox border inactive |
| 47% | `#90a1b977` | border اصلی sidebar/header/footer |
| – | `#314158` | border input، کارت عمیق |

### ۷.۳ الگوهای مرز ویژه
- **Active Sidebar item**:
  ```jsx
  className="border-b border-b-[1px] border-[#90A1B9] border-t-[1px]"
  ```
- **Active Filter Checkbox**:
  ```jsx
  className="bg-[#615FFF] border-[#615FFF]"  // fill + border یک‌رنگ
  ```
- **Hover Skill Card**:
  ```jsx
  style={{ border: `1px solid ${hovered ? purple : "#90a1b933"}` }}
  ```
- **Dashed (showcase placeholder)**:
  ```jsx
  className="border-dashed border-[#90a1b933]"
  ```

---

## ۸. سایه‌ها و Glow

### ۸.۱ Card Shadow پایه
```css
/* کارت MusicSearch (hero) — تنها جایی که shadow اصلی کارت وجود دارد */
.box-3d {
  box-shadow:
    0 20px 40px -12px rgba(0, 0, 0, 0.4),
    0 4px 12px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.2);
}
```

### ۸.۲ Shadow های استاندارد
- `shadow-sm` — دکمه‌های submit (small)
- `shadow-lg` — Modal، Music player، Monogram
- `shadow-2xl` — modal backdrop، header drop
- `shadow-xl` — فرم MusicSearch (ورودی/select)
- `shadow-md` — دکمه Play در Music

### ۸.۳ Glow / Neon Shadow
```jsx
// Monogram badge (همیشه)
boxShadow: "0 6px 24px rgba(97,95,255,0.4)"

// Hover کارت پروژه
hover:shadow-[0_0_35px_-5px_rgba(97,95,255,0.25)]

// Progress thumb در Music player
boxShadow: "0 0 6px rgba(97,95,255,0.7)"

// Admin sidebar badge
shadow-[0_0_8px_rgba(255,95,86,0.6)]

// Modal پیام موفقیت (combined)
boxShadow: "0 25px 60px -15px rgba(0,0,0,0.8), 0 0 35px rgba(97,95,255,0.15)"

// pulse glow در success modal (3s loop)
@keyframes pulseGlow {
  0%,100% { box-shadow: 0 0 25px rgba(0, 213, 190, 0.35); }
  50%     { box-shadow: 0 0 45px rgba(97, 95, 255, 0.55); }
}

// نقاط ترمینال (neon)
shadow-[0_0_6px_#EF4444]
shadow-[0_0_6px_#F59E0B]
shadow-[0_0_6px_#10B981]
```

### ۸.۴ حلقه‌ی focus (Focus Ring)
```css
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button { -webkit-appearance: none; margin: 0; }
input[type="number"] { appearance: textfield; -moz-appearance: textfield; }
```

---

## ۹. گرادیانت‌ها

### ۹.۱ Monogram یکدست (همه‌ی کارت‌های پروژه)
```jsx
const monogramGradient =
  "linear-gradient(135deg, #615FFF 0%, #7C6CF6 100%)";
```

### ۹.۲ خط آکسان بالای کارت (Top accent line)
```jsx
background:
  "linear-gradient(90deg, transparent 0%, #615FFF 30%, #7C6CF6 70%, transparent 100%)"
```

### ۹.۳ Progress Bar در PageTransition
```jsx
background: `linear-gradient(90deg, ${meta.color}99 0%, ${meta.color} 100%)`
```

### ۹.۴ دکمه‌ی success modal
```jsx
className="bg-gradient-to-r from-[#615FFF] to-[#4F46E5]
           hover:from-[#7573FF] hover:to-[#5B54F6]"
```

### ۹.۵ Badge Success (modal)
```jsx
className="bg-gradient-to-tr from-[#00D5BE]/20 to-[#615FFF]/20 border border-[#00D5BE]/40"
```

### ۹.۶ Skeleton Shimmer
```css
.skeleton-bar {
  background-color: #90a1b91a;
  background-image: linear-gradient(
    90deg,
    transparent 0%,
    rgba(97, 95, 255, 0.16) 45%,
    rgba(124, 108, 246, 0.20) 55%,
    transparent 100%
  );
  background-size: 220% 100%;
  background-repeat: no-repeat;
  animation: skeletonSweep 1.5s ease-in-out infinite;
}
@keyframes skeletonSweep {
  0%   { background-position: 140% 0; }
  100% { background-position: -60% 0; }
}
```

### ۹.۷ پس‌زمینه‌ی home (Background blurs)
```css
.home-page-div {
  background-image: url("/assets/Images/background blurs.png");
  background-repeat: no-repeat;
  background-position: center center;
  background-size: 100%;          /* موبایل/تبلت */
}
@media (min-width: 1024px) {
  .home-page-div { background-size: 45%; }   /* دسکتاپ */
}
```

### ۹.۸ بک‌گراند BootLoader (radial)
```css
.boot-glow {
  background: radial-gradient(circle,
    rgba(97,95,255,0.18) 0%,
    rgba(56,189,248,0.08) 50%,
    transparent 70%);
}
```

### ۹.۹ Gradient هدر Nav Link (transparent → orange → transparent)
```jsx
// border-bottom لینک فعال: یک border ساده نارنجی
className="border-b-[#FFB86A]"
```

---

## ۱۰. پس‌زمینه‌های اختصاصی

### ۱۰.۱ پس‌زمینه‌ی Body
```css
body {
  min-height: 100vh;
  overflow-x: hidden;
  overflow-y: auto;
}
html { -webkit-text-size-adjust: 100%; }
```
- بدنه scroll عمودی طبیعی دارد و scroll افقی بسته شده.
- کلیه رنگ پس‌زمینه با `bg-[#0F172B]` روی `<section>` اعمال می‌شود.

### ۱۰.۲ پس‌زمینه‌ی Grid Faint (درون کارت‌ها)
```jsx
backgroundImage:
  "linear-gradient(rgba(144,161,185,0.07) 1px, transparent 1px), linear-gradient(90deg, rgba(144,161,185,0.07) 1px, transparent 1px)",
backgroundSize: "22px 22px"
```
- در `ProjectCard` (preview area) و در `PosterSkeleton` (loading state) استفاده می‌شود.
- خط‌چین بسیار کم‌رنگ، فقط ایجاد بافت می‌کند.

### ۱۰.۳ غشای شیشه‌ای Cursor
```css
.cursor-ring {
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.22);
  backdrop-filter: blur(6px) saturate(1.5);
  -webkit-backdrop-filter: blur(6px) saturate(1.5);
  box-shadow:
    inset 0 1px 6px rgba(255, 255, 255, 0.18),
    inset 0 -2px 6px rgba(0, 0, 0, 0.22),
    0 8px 24px -8px rgba(0, 0, 0, 0.5);
}
```

### ۱۰.۴ User-Select Strategy
```css
header, footer, nav, button, label, select {
  -webkit-user-select: none; user-select: none;
}
input, textarea {
  -webkit-user-select: text; user-select: text;
}
```
- یعنی: کروم تزئینی (هدر/فوتر/ناوبری/دکمه‌ها) انتخاب نمی‌شود، اما محتوای واقعی متن و ورودی‌ها قابل انتخاب هستند.

---

## ۱۱. انیمیشن‌ها و Keyframes

### ۱۱.۱ کلید-فریم‌های اصلی

```css
/* چشمک‌زن BootLoader text */
@keyframes bootBlink {
  0%, 49% { opacity: 1; }
  50%, 100% { opacity: 0; }
}

/* fade-in اولیه BootLoader */
@keyframes bootFadeIn {
  from { opacity: 0; transform: translateY(6px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* pulse rings در BootLoader */
@keyframes boot-pulse {
  0%   { transform: scale(0.55); opacity: 0.85; }
  80%  { opacity: 0; }
  100% { transform: scale(1.5); opacity: 0; }
}

/* سه‌نقطه loading در BootLoader */
@keyframes boot-dot {
  0%, 80%, 100% { transform: scale(0.6); opacity: 0.35; }
  40%           { transform: scale(1); opacity: 1; }
}

/* wipe-in bar (برای curtain effect) */
@keyframes barWipeIn  { from { transform: translateY(-100%); } to { transform: translateY(0); } }
@keyframes barWipeOut { to { transform: translateY(100%); } }

/* chip-in (نمایش چیپ‌ها) */
@keyframes chipIn {
  from { opacity: 0; transform: scale(0.85) translateY(8px); }
  to   { opacity: 1; transform: scale(1) translateY(0); }
}

/* card-in (ترمینال کارت PageTransition) */
@keyframes cardIn {
  from { opacity: 0; transform: translateY(14px) scale(.97); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}

/* shimmer در progress bar */
@keyframes shimmer {
  0%   { left: -35%; }
  100% { left: 105%; }
}

/* shutter transition (drop in/out) */
@keyframes shutterDropIn  { 0% { transform: translateY(-100%); } 100% { transform: translateY(0%); } }
@keyframes shutterDropOut { 0% { transform: translateY(0%); } 100% { transform: translateY(100%); } }

/* veil in/out (modal backdrop) */
@keyframes veilIn  { from { opacity: 0; } to { opacity: 1; } }
@keyframes veilOut { 0% { opacity: 1; } 100% { opacity: 0; visibility: hidden; } }

/* page reveal (هنگام navigation) */
@keyframes pageReveal {
  from { opacity: 0; transform: translateY(6px); }
  to   { opacity: 1; transform: translateY(0); }
}
.page-reveal { animation: pageReveal 0.25s ease-out both; }

/* trailing dots loading */
@keyframes dots {
  0%   { content: ""; }
  25%  { content: "."; }
  50%  { content: ".."; }
  75%  { content: "..."; }
  100% { content: ""; }
}
.loading-dots::after { content: ""; animation: dots 1.2s steps(4) infinite; }

/* fade-slide-up (project cards mount) */
@keyframes fadeSlideUp {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* skeleton shimmer */
@keyframes skeletonSweep {
  0%   { background-position: 140% 0; }
  100% { background-position: -60% 0; }
}

/* Typewriter caret blink */
@keyframes tiBlink { 0%, 49% { opacity: 1; } 50%, 100% { opacity: 0; } }

/* Modal pop animation */
@keyframes modalPop {
  0%   { transform: scale(0.92) translateY(16px); opacity: 0; }
  100% { transform: scale(1) translateY(0); opacity: 1; }
}

/* Success badge pulse glow */
@keyframes pulseGlow {
  0%, 100% { box-shadow: 0 0 25px rgba(0, 213, 190, 0.35); }
  50%      { box-shadow: 0 0 45px rgba(97, 95, 255, 0.55); }
}

/* Success icon bounce */
@keyframes iconBounce {
  0%   { transform: scale(0); opacity: 0; }
  60%  { transform: scale(1.15); opacity: 1; }
  100% { transform: scale(1); opacity: 1; }
}
```

### ۱۱.۲ انیمیشن‌های استاندارد برای حالت‌ها

**Hover کارت پروژه**:
```jsx
className="transition-all duration-300 hover:border-[#615FFF] hover:-translate-y-1
           hover:shadow-[0_0_35px_-5px_rgba(97,95,255,0.25)]"
style={{ opacity: 0, animation: "fadeSlideUp 0.4s ease forwards" }}
```

**Hover Skill/Education card**:
```jsx
style={{
  transform: hovered === name ? "translateY(-3px)" : "translateY(0)",
}}
className="duration-300"
```

**Hover آیتم‌های sidebar**:
```jsx
className="hover:bg-[#7888a033] duration-100"   // کوتاه
className="hover:text-white hover:bg-[#7888a01a] transition-all duration-150"
```

**Hover دکمه‌های submit**:
```jsx
// فرم contact:
className="bg-[#90A1B9] hover:bg-[#0E1528] outline-1 outline-[#90A1B9]
           text-[#0E1528] hover:text-[#90A1B9] duration-150"

// دکمه‌ی Music Search:
className="bg-[#1D293D] hover:bg-[#615FFF] hover:ring-[#615FFF] duration-150"

// دکمه‌ی play در music:
className="bg-[#615FFF] hover:bg-[#7573FF] active:scale-95 duration-150"
```

**Active/Press**:
```jsx
active:scale-[0.98]    // modal button
active:scale-95        // play button
```

### ۱۱.۳ استاندارد زمان‌بندی (Timing)
- `duration-100` = 100ms (تغییرات رنگ خیلی سریع مثل active sidebar)
- `duration-150` = 150ms (اکثر hover ها)
- `duration-200` = 200ms (modal transitions)
- `duration-300` = 300ms (کارت hover، border)
- `duration-400` = 400ms (BootLoader fade-out)
- `ease-out`, `ease-in-out` رایج‌ترین easing ها

### ۱۱.۴ prefers-reduced-motion
```css
@media (prefers-reduced-motion: reduce) {
  .page-reveal { animation: none; }
  .cursor-ring { backdrop-filter: none; -webkit-backdrop-filter: none;
                 background: rgba(255, 255, 255, 0.1); }
  .boot-ring, .boot-dots .dot { animation: none; }
  .boot-ring { opacity: 0.3; transform: scale(1); }
  .boot-dots .dot { opacity: 0.7; transform: scale(1); }
}
```

---

## ۱۲. Cursor سفارشی (Liquid-Glass)

### ۱۲.۱ فعال‌سازی
```jsx
// فقط روی pointer: fine فعال می‌شود (PC ها، نه touch)
if (!window.matchMedia("(pointer: fine)").matches) return;
root.classList.add("has-cursor");  // حذف cursor پیش‌فرض
```

### ۱۲.۲ ساختار
- دو div: `.cursor-dot` (نقطه‌ی کوچک frosted bead) و `.cursor-ring` (دیسک شیشه‌ای).
- هر دو: `position: fixed; top: 0; left: 0; z-index: 9999; pointer-events: none; border-radius: 9999px; will-change: transform;`

### ۱۲.۳ مشخصات بصری
| المان | width/height | background | effect |
|---|---|---|---|
| cursor-dot | 8×8 px | rgba(226, 232, 240, 0.9) + inset shadow | frosted bead |
| cursor-ring | 38×38 px | rgba(255,255,255,0.06) + backdrop-filter blur(6px) saturate(1.5) + inset shadows | glass disc |

### ۱۲.۴ رفتار (Physics)
- **Dot**: با ease 0.65 (تقریباً چسبیده به ماوس)
- **Ring**: با ease 0.24 (trail آرام)
- **Scale**: ring از 1 → 1.25 در hover روی المان interactive، از 1 → 0.85 در کلیک
- **Stretch**: در حرکت سریع، ring کشیده می‌شود (`stretch = 1 + min(speed * 0.09, 0.9)`) و در جهت حرکت angle می‌گیرد
- **Bead clamp**: dot هیچ‌وقت از دیسک فراتر نمی‌رود (max distance = ring radius - bead radius - margin)

### ۱۲.۵ عناصر تعاملی (Hoverable)
```js
const HOVERABLE =
  "a,button,select,label,summary,[role=button],[role=link],input[type=submit],input[type=button]";
```

### ۱۲.۶ مخفی‌سازی ماوس بومی
```css
html.has-cursor, html.has-cursor * { cursor: none !important; }
```

---

## ۱۳. کامپوننت‌های رابط کاربری

### ۱۳.۱ Catalog سراسری
| نام | مسیر | نقش |
|---|---|---|
| `BootLoader` | `src/Components/BootLoader` | صفحه‌ی بارگذاری اولیه (MMS monogram + 3dot + glow) |
| `Cursor` | `src/Components/Cursor` | نشانگر سفارشی liquid-glass |
| `Header` | `src/Components/Header` | نوار بالایی با nav + لوگو + hamburger |
| `Footer` | `src/Components/Footer` | نوار پایینی با لینک‌های اجتماعی |
| `PageTransition` | `src/Components/PageTransition` | کارت ترمینال هنگام تغییر route |
| `Loading` | `src/Components/Loading` | ۵ variant اسکلتون (cards, article, skills, music, lines) |
| `Music` | `src/Components/Music` | پلیر MP3 + visualizer spectrum |
| `SnakeBar` | `src/Components/SnakeBar` | خط متحرک رنگین سمت چپ سایدبار |
| `Typewriter` | `src/Components/Typewriter` | تایپ تک‌خطی با caret |
| `ErrorBoundary` | `src/Components/ErrorBoundary` | گیرنده‌ی خطا |
| `VisitTracker` | `src/Components/VisitTracker` | ارسال pageview و heartbeat |

### ۱۳.۲ Catalog صفحات (Public)
| نام | مسیر | ساختار |
|---|---|---|
| `Home` | `Page/Home` | `<InformationText/> + <MusicSearch/>` با پس‌زمینه blur |
| `About` | `Page/About` | `<SubjectBox/> + <Bio/>` (تب‌ها: aboutMe, skills, education) |
| `Project` | `Page/Project` | `<FilterBox/> + <ProjectSection/>` با 6 دسته + mini |
| `Contact` | `Page/Contact` | `<ContactBox/> + <Form/> + <CodeView/>` + Modal |
| `Blog` | `Page/Blog` | لیست پست‌ها با search |
| `Blog/Post` | `Page/Blog/Post` | پست تکی |
| `NotFound` | `Page/NotFound` | کد مصور 404 + لینک بازگشت |
| `AdminApp` | `Page/Admin` | روت‌های ادمین (login, stats, projects, blog, skills, site, messages, telegram, moods, security) |

### ۱۳.۳ Header (الگوی دقیق)
```jsx
<header className="sticky top-0 z-50 h-[58px] w-full bg-[#0F172B]">
  <div className="relative w-full h-[58px] flex items-center justify-between
                  px-3 sm:px-6 md:px-0 border-b-[1px] border-[#90a1b977]">
    <input type="checkbox" id="nav-toggle" className="nav-toggle" />
    
    {/* Logo column (با border-r در md+) */}
    <Link to="/" className="text-[13px] sm:text-[15px] md:text-[16px] text-[#90A1B9]
                            py-[16px] px-3 sm:px-5 md:px-5 lg:px-6
                            md:w-60 lg:w-[344px] xl:w-[416px] 2xl:w-[513px]
                            md:border-r-[1px] md:border-[#90a1b977]">
      Mohammad-Mehdi-Sadeghi
    </Link>
    
    {/* Middle nav (hidden <md) */}
    <ul className="hidden md:flex items-center h-full">
      {middleLinks.map((link) => (
        <li className="flex justify-center items-center h-full border-r/l border-[#90a1b977]">
          <Link className="py-[16px] px-2 sm:px-2.5 md:px-3 lg:px-4 xl:px-6
                          text-[13px] md:text-[14px] lg:text-[15px] text-[#90A1B9]
                          transition-all duration-300 border-b-4
                          ${isActive ? "border-b-[#FFB86A] text-white" : "border-b-transparent"}">
            {link.label}    {/* لیبل‌ها: _Home, _About, _Project, _Blog, _Contact-me */}
          </Link>
        </li>
      ))}
    </ul>
    
    {/* Right contact link (hidden <md) */}
    <ul className="hidden md:flex items-center h-full shrink-0">
      <li className="border-l-[1px] border-[#90a1b977] flex ...">
        <Link to="/contact" className="... border-b-4 ${active ? "border-b-[#FFB86A]" : "border-b-transparent"}">
          _Contact-me
        </Link>
      </li>
    </ul>
    
    {/* Hamburger (mobile only) */}
    <label htmlFor="nav-toggle" className="mobile-menu md:hidden flex ...">
      <div className="hamburger-icon"><span/><span/><span/></div>
    </label>
    
    {/* Mobile panel */}
    <div className="mobile-nav-panel md:hidden absolute top-[55px] left-0 w-full
                    bg-[#0F172B] border-t-[1px] border-b-[1px] border-[#90a1b977] z-40 shadow-2xl">
      <ul className="flex flex-col">
        {links.map((link) => (
          <li className="border-b-[1px] border-[#90a1b977]">
            <Link className="block w-full py-4 px-6 text-[14px] text-[#90A1B9]
                            transition-all duration-300
                            ${active ? "text-white bg-[#7888a033] border-l-4 border-l-[#FFB86A]" : ""}">
              {link.label}
            </Link>
          </li>
        ))}
      </ul>
    </div>
  </div>
</header>
```

### ۱۳.۴ Hamburger (CSS-only، بدون JS)
```css
.nav-toggle {
  position: absolute; opacity: 0; pointer-events: none;  /* hidden checkbox */
}
.hamburger-icon { width: 24px; height: 18px; position: relative; }
.hamburger-icon span {
  position: absolute; left: 0; width: 100%; height: 2px;
  background: #90a1b9; border-radius: 2px;
  transition: transform 0.3s ease, opacity 0.3s ease, top 0.3s ease;
}
.hamburger-icon span:nth-child(1) { top: 0; }
.hamburger-icon span:nth-child(2) { top: 8px; }
.hamburger-icon span:nth-child(3) { top: 16px; }

/* وقتی چک‌باکس تیک خورد → ضربدر */
.nav-toggle:checked ~ .mobile-menu .hamburger-icon span:nth-child(1) {
  top: 8px; transform: rotate(45deg); background: #ffb86a;
}
.nav-toggle:checked ~ .mobile-menu .hamburger-icon span:nth-child(2) {
  opacity: 0;
}
.nav-toggle:checked ~ .mobile-menu .hamburger-icon span:nth-child(3) {
  top: 8px; transform: rotate(-45deg); background: #ffb86a;
}

/* باز/بسته شدن پنل موبایل */
.mobile-nav-panel { max-height: 0; overflow: hidden; transition: max-height 0.35s ease; }
.nav-toggle:checked ~ .mobile-nav-panel { max-height: 500px; }
```

### ۱۳.۵ Footer
```jsx
<footer className="w-full min-h-[58px] bg-[#0F172B] flex items-center justify-center">
  <nav className="w-full h-full flex flex-row justify-between items-center
                  text-[#90A1B9] border-t-[1px] border-[#90a1b977]
                  px-2 sm:px-4 md:px-0 py-1.5 sm:py-0">
    
    {/* Left: لینک‌های اجتماعی */}
    <ul className="flex items-center gap-1 sm:gap-2 lg:gap-0">
      <li className="py-2 sm:py-[15px] px-2 sm:px-4 lg:px-[24px]
                     text-[12px] sm:text-[14px] lg:text-[16px]
                     border-r-0 lg:border-r-[1px] border-[#90a1b977]">
        <p>find-me-in :</p>
      </li>
      <li className="py-1.5 sm:py-[8px] px-2 sm:px-[14px]
                     border-r-0 lg:border-r-[1px] border-[#90a1b977]
                     bg-[#1D293D] rounded-md lg:bg-transparent lg:rounded-none
                     flex items-center justify-center">
        <a href="https://www.linkedin.com/in/mohammad-mehdi-sadeghi">
          <img className="w-6 sm:w-[32px] lg:w-[38px]" src="/assets/Images/RiLinkedinFill.png" />
        </a>
      </li>
      <li className="...">  {/* Telegram */}
        <a href="https://t.me/Mohammad_sadeghi34">
          <img className="w-6 sm:w-[30px] lg:w-[34px]" src="/assets/Images/BasilTelegramSolid.png" />
        </a>
      </li>
    </ul>
    
    {/* Right: GitHub */}
    <ul className="shrink-0">
      <li className="py-2 sm:py-[11px] px-2.5 sm:px-4 lg:px-[24px]
                     sm:border-l-[1px] text-[12px] sm:text-[14px]
                     border-[#90a1b977] flex items-center justify-center gap-2 sm:gap-4">
        <p className="hidden lg:block"><span className="m-1">@</span>MohammadMehdiSadeghi</p>
        <p className="lg:hidden block text-[12px] sm:text-[13px]">Github :</p>
        <a href="https://github.com/MohammadMehdiSadeghi">
          <img className="w-6 sm:w-7" src="/assets/Images/MdiGithub.png" />
        </a>
      </li>
    </ul>
  </nav>
</footer>
```

### ۱۳.۶ Sidebar (SubjectBox / FilterBox / ContactBox)
الگوی مشترک سایدبار (در ۳ صفحه: About، Project، Contact):
```jsx
<section className="flex justify-start md:h-full border-b md:border-b-0 border-[#90a1b977]">
  
  {/* SnakeBar (فقط lg+) */}
  <div className="hidden lg:block relative w-14 border-r-[1px] border-[#90a1b977]
                  h-[calc(100vh-116px)]">
    <SnakeBar />
  </div>
  
  {/* خود سایدبار */}
  <nav className="w-full md:w-60 lg:w-72 xl:w-[360px] 2xl:w-[457px] shrink-0
                  md:border-r-[1px] border-[#90a1b977]
                  md:max-h-none md:h-[calc(100vh-116px)] md:overflow-y-auto
                  text-[#90A1B9]">
    
    {/* عنوان دسته (h2 سفید با خط زیر) */}
    <h2 className="px-4 sm:px-7 py-3.5 w-full flex items-center gap-2.5
                   border-b-[1px] text-white border-[#90a1b977]">
      personal-info
      <img className="w-3 max-md:rotate-90" src="/assets/Images/Vector.svg" />
    </h2>
    
    {/* آیتم‌ها */}
    <ul className="flex flex-col h-fit">
      {items.map((item) => (
        <li className={`px-4 sm:px-8 md:px-6 lg:px-10 py-3 duration-100 cursor-pointer
                       ${active ? "bg-[#7888a033] text-white border-b border-b-[1px] border-[#90A1B9]" : ""}
                       hover:bg-[#7888a033]`}>
          <img src="/assets/Images/icon folder.svg" />
          {item.label}
        </li>
      ))}
    </ul>
  </nav>
</section>
```

### ۱۳.۷ Sidebar Item Active States
- **Active (پایه)**: `bg-[#7888a033] text-white`
- **Active border در تماس با آیتم بعدی**: `border-b-[1px] border-[#90A1B9]`
- **Active در حالت hover**: ترکیب `bg-[#7888a033]` با `text-white`
- **موقعی که active نیست**: رنگ پیش‌فرض `#90A1B9`

### ۱۳.۸ Card: Project Card
```jsx
<div className="rounded-xl group cursor-pointer overflow-hidden flex flex-col
            transition-all duration-300 hover:border-[#615FFF] hover:-translate-y-1
            border-[#90a1b933] border
            hover:shadow-[0_0_35px_-5px_rgba(97,95,255,0.25)]"
     style={{ background: "#081224", opacity: 0, animation: "fadeSlideUp 0.4s ease forwards" }}>
  
  {/* Preview area (180px height) */}
  <div className="w-full h-[180px] relative overflow-hidden" style={{ background: "#0a1628" }}>
    
    {/* Top accent gradient line */}
    <div className="absolute top-0 left-0 right-0 h-[2px]"
         style={{
           background: "linear-gradient(90deg, transparent 0%, #615FFF 30%, #7C6CF6 70%, transparent 100%)",
           opacity: hovered ? 1 : 0.4,
         }} />
    
    {/* Purple glow بالا-راست */}
    <div className="absolute -top-12 -right-12 w-44 h-44 rounded-full blur-3xl"
         style={{
           background: "radial-gradient(circle, rgba(97,95,255,0.35) 0%, transparent 70%)",
           opacity: hovered ? 0.9 : 0.4,
         }} />
    
    {/* Purple glow پایین-چپ */}
    <div className="absolute -bottom-14 -left-10 w-40 h-40 rounded-full blur-3xl"
         style={{ background: "radial-gradient(circle, rgba(124,108,246,0.2) 0%, transparent 70%)" }} />
    
    {/* Faint grid texture */}
    <div className="absolute inset-0"
         style={{
           backgroundImage:
             "linear-gradient(rgba(144,161,185,0.07) 1px, transparent 1px), linear-gradient(90deg, rgba(144,161,185,0.07) 1px, transparent 1px)",
           backgroundSize: "22px 22px",
         }} />
    
    {/* Watermark monogram بسیار کم‌رنگ */}
    <div className="absolute inset-0 flex items-center justify-center pointer-events-none">
      <span className="text-[128px] font-bold select-none leading-none"
            style={{ color: "#615FFF", opacity: 0.07 }}>{monogram}</span>
    </div>
    
    {/* App-icon monogram box */}
    <div className="absolute inset-0 flex flex-col items-center justify-center gap-2.5 px-4">
      <div className="w-10 h-10 rounded-xl flex items-center justify-center shadow-lg"
           style={{
             background: monogramGradient,
             boxShadow: "0 6px 24px rgba(97,95,255,0.4)",
             transform: hovered ? "scale(1.1) rotate(-6deg)" : "scale(1) rotate(0deg)",
           }}>
        <span className="text-white font-bold text-[13px] leading-none tracking-tight">{monogram}</span>
      </div>
      
      {/* Wordmark */}
      <h4 className="text-white text-[15px] sm:text-[18px] lg:text-[20px]
                     font-bold tracking-[0.06em] uppercase text-center truncate">
        {project.title}
      </h4>
    </div>
    
    {/* Language badges (top center) */}
    <div className="absolute top-2.5 inset-x-0 z-20 flex items-center justify-center gap-1.5 flex-wrap px-6">
      {project.category.map((cat) => (
        <span className="text-[8px] sm:text-[9px] px-2 py-0.5 rounded-full uppercase tracking-wide"
              style={{
                color: gray,
                background: "rgba(97,95,255,0.12)",
                border: "1px solid rgba(97,95,255,0.3)",
              }}>{cat}</span>
      ))}
    </div>
    
    {/* 4 corner brackets */}
    {[TL,TR,BL,BR].map((corner) => (
      <div className={`absolute ${corner} w-4 h-4 border-${corner.includes("top")?"t":"b"}-2 border-${corner.includes("left")?"l":"r"}-2`}
           style={{ borderColor: "#615FFF66", opacity: hovered ? 1 : 0.6 }} />
    ))}
  </div>
  
  {/* Info section */}
  <div className="p-5 flex flex-col gap-4 flex-1">
    <div>
      <p className="text-[11px] mb-2 text-[#615FFF]">
        Project {String(index + 1).padStart(2, "0")} // <span className="text-[#90A1B9]">_{title}</span>
      </p>
      <h3 className="text-white text-[16px]">{project.title}</h3>
    </div>
    <p className="text-[13px] leading-6 text-[#90A1B9]">{project.description}</p>
    <div className="mt-auto">
      {/* لینک یا showcase placeholder */}
    </div>
  </div>
</div>
```

**Monogram Logic** (ساخت حروف اول عنوان):
```js
const getMonogram = (title) => {
  const words = title?.trim().split(/\s+/);
  if (words.length === 1) return words[0].length >= 2 ? words[0].substring(0, 2).toUpperCase() : words[0][0].toUpperCase();
  return (words[0][0] + words[1][0]).toUpperCase();
};
```

### ۱۳.۹ Card: Skill Card
```jsx
<div className="relative flex items-center gap-5 p-5 rounded-lg cursor-pointer duration-300"
     style={{
       background: hovered === skill.name ? "#0F172B" : "#081224",
       border: `1px solid ${hovered === skill.name ? purple : "#90a1b933"}`,
       transform: hovered === skill.name ? "translateY(-3px)" : "translateY(0)",
     }}>
  
  {/* شماره index بالا-راست */}
  <span className="absolute top-3 right-3 text-[11px] select-none"
        style={{ color: "#3d4f6b", fontFamily: "monospace" }}>
    {String(i + 1).padStart(2, "0")}
  </span>
  
  <img className="w-14 h-14 object-contain flex-shrink-0" src={skill.img} alt={skill.name} />
  
  <div className="flex flex-col gap-1">
    <h3 className="text-[15px] font-semibold duration-300"
        style={{ color: hovered === skill.name ? "#ffffff" : gray }}>
      {skill.name}
    </h3>
  </div>
  
  {/* نقطه‌ی سبز پایین-راست در hover */}
  <div className="absolute bottom-3 right-3 w-2 h-2 rounded-full duration-300"
       style={{
         background: hovered === skill.name ? "#00D5BE" : "transparent",
         boxShadow: hovered === skill.name ? "0 0 6px #00D5BE" : "none",
       }} />
</div>
```

### ۱۳.۱۰ Card: Education Timeline
- همان قالب Skill card، با timeline اختصاصی:
  - شماره‌ی id (`01`, `02`, ...) به‌جای index
  - یک خط عمودی کم‌رنگ پشت timeline: `position: absolute; left: 52px; top: 0; bottom: 0; width: 1px; background: #90a1b922`
  - یک نقطه‌ی timeline با ring: `position: absolute; left: 46px; top: 26px; width: 12px; height: 12px; border-radius: 9999px`
  - badge وضعیت (`In Progress`, `Completed`, `Upcoming`) با رنگ اختصاصی
- رنگ‌ها:
  - مدرسه‌ی فعلی: `purple` (#615FFF)
  - دوره‌های تمام‌شده: `turquoise` (#00D5BE)
  - آینده: `gray` (#3d4f6b)

### ۱۳.۱۱ Card: Music Search (Hero)
```jsx
<div className="box-3d w-full max-w-full sm:max-w-md lg:max-w-[420px]
            bg-[#0F172B] p-5 sm:p-6 rounded-2xl border border-[#1E293B] shadow-xl">
  {/* فرم با input + دکمه submit */}
</div>
```
- این تنها کارت با کلاس اختصاصی `box-3d` است.

### ۱۳.۱۲ Card: Music Player
```jsx
<div className="w-full mt-3 bg-[#0B1222]/95 rounded-xl p-3 sm:p-3.5
            border border-[#1E293B] shadow-lg flex flex-col gap-2.5
            overflow-hidden box-border">
  
  {/* Header: Track info (left) + Visualizer canvas (right) */}
  <div className="flex items-center justify-between gap-2">
    <div className="min-w-0 flex-1">
      <h4 className="text-[13px] sm:text-[14px] font-semibold text-[#E2E8F0] truncate">{name}</h4>
      {artist && <p className="text-[11px] text-[#90A1B9] truncate">{artist}</p>}
    </div>
    <div className="w-20 sm:w-24 h-6 shrink-0 bg-[#020618] rounded border border-[#1E293B] overflow-hidden">
      <canvas className="w-full h-full block" />
    </div>
  </div>
  
  {/* Progress */}
  <div className="flex flex-col gap-1 px-0.5">
    <input type="range" className="music-progress w-full cursor-pointer bg-[#1E293B]"
           style={{ background: `linear-gradient(to right, #615FFF ${pct}%, #1E293B ${pct}%)` }} />
    <div className="flex justify-between text-[#68768C] font-mono text-[10px]">
      <span>{formatTime(currentTime)}</span>
      <span>{formatTime(duration)}</span>
    </div>
  </div>
  
  {/* Controls */}
  <div className="flex items-center justify-between gap-2 pt-1 border-t border-[#1E293B]/60">
    <div className="flex items-center gap-2">
      <button className="bg-[#615FFF] hover:bg-[#7573FF] text-white rounded-lg p-2 ...">
        {/* Play / Pause SVG icon */}
      </button>
      <button className="text-[#90A1B9] hover:text-white p-2 rounded-lg hover:bg-[#1E293B] ...">
        {/* Download icon */}
      </button>
    </div>
    <div className="flex items-center gap-1.5 px-1 shrink-0 max-w-[110px] sm:max-w-[130px]">
      {/* Volume mute button + slider */}
    </div>
  </div>
</div>
```

### ۱۳.۱۳ Visualizer (Music Spectrum)
- ۳۶ bar روی canvas با `dpr` handling.
- هر bar با `roundedRect` کشیده می‌شود، رنگ: `hsla(${hue}, 95%, ${light}%, 0.95)` که hue بین 240-180 تغییر می‌کند (طیف بنفش).
- وقتی play نیست، sine wave کم‌رنگ: `0.15 + 0.12 * Math.sin(now/500 + b*0.5)`.
- Smoothing: `attack=0.40, release=0.25` (حمله سریع، رهاسازی آرام).
- Energy gain از روی `song.analysis.energy` تنظیم می‌شود.

### ۱۳.۱۴ SnakeBar
```jsx
<canvas ref={canvasRef} width={4} height={600}
        style={{ display: "block", position: "absolute", left: 0, top: 0 }} />
```
- انیمیشن ۶۰fps با `setInterval(step, 16)`.
- Hue چرخان (1.5°/frame)، trail 180px، شدت رنگ با shadowBlur=18 و alpha.
- یک خط ۴px × 3px که بالا-پایین می‌رود.

### ۱۳.۱۵ Typewriter
```jsx
<Typewriter
  text="Mohammad Mehdi Sadeghi"
  speed={65}
  cursor={true}
  onDone={callback}
  lifeLike={true}  // default
  as="span"        // default
  {...rest} />
```
- State داخلی: `shown` (شمارنده‌ی کاراکتر).
- هر کاراکتر در `delay = speed * (0.6 + Math.random() * 0.8)`.
- اگر text تغییر کند، از ۰ ری‌استارت.
- Caret: `<span className="ti-cursor" aria-hidden="true" />` با `tiBlink 0.8s infinite`.
- برای حذف caret پس از اتمام تایپ: `data-typed="done"` روی المان والد.

### ۱۳.۱۶ Code Block ها (CodeView در Contact و NotFound)
```jsx
<p className="px-3 mt-2 text-[12px] leading-6 2xl:text-[13px] 2xl:leading-7">
  <span className="mr-5" style={{ color: gray }}>01</span>  {/* شماره خط */}
  <span style={{ color: pink }}>const</span>                {/* keyword */}
  <span style={{ color: purple }}>button</span>             {/* identifier */}
  <span style={{ color: pink }}>=</span>                    {/* operator */}
  <span style={{ color: purple }}>document</span>
  <span style={{ color: gray }}>.</span>                    {/* punctuation */}
  <span style={{ color: yellow }}>'#sendBtn'</span>         {/* string */}
  <span style={{ color: gray }}>;</span>
</p>
```
**Syntax Highlighting Palette**:
| رنگ | HEX | نقش |
|---|---|---|
| gray | `#68768C` | شماره خط، punctuation (`;` `.` `(` `)` `{` `}` `:`) |
| pink | `#C27AFF` | `const`, `=>`, `=` |
| purple | `#615FFF` | identifier (متغیرها، توابع، آبجکت‌ها) |
| yellow | `#FFB86A` | رشته‌ها |
| green | `#00D5BE` | رشته‌ها (در 404) |
| red | `#FF637E` | console.log، findPage، throw new Error |

### ۱۳.۱۷ SnakeBar (در هر ۳ سایدبار)
- فقط `lg` و بالاتر.
- یک `<div class="relative w-14 border-r-[1px] border-[#90a1b977]">` که `<SnakeBar/>` درونش قرار می‌گیرد.

### ۱۳.۱۸ Filter Box (در Project)
- **در موبایل**: `<nav className="w-full md:hidden ...">` با دو دسته‌ی projects و mini-project، هر کدام collapsible با Chevron (Vector.svg rotate).
- **در دسکتاپ**: `<nav className="hidden md:block w-60 lg:w-72 xl:w-[360px] 2xl:w-[457px] ...">` با checkbox‌ها.
- Checkbox active: `<span className="w-4 h-4 rounded-sm border flex items-center justify-center bg-[#615FFF] border-[#615FFF]">` با SVG چک‌مارک.

---

## ۱۴. الگوهای لایه‌بندی (Layout Patterns)

### ۱۴.۱ ساختار کلی اپ
```
<App>
  <Cursor />                      ← فقط یکی در root
  <Routes>
    /admin/* → <AdminApp/>        ← ادمین (route جدا)
    /* → <PublicSite/>            ← سایت عمومی
  </Routes>

<PublicSite>
  <VisitTracker />                ← null، فقط API call می‌کند
  {!booted && <BootLoader/>}      ← فقط در session اول
  <Header />                      ← sticky
  <PageTransition active path/>   ← هنگام تغییر route
  <ErrorBoundary key=pathname>
    <div className="page-reveal" style={{minHeight:"calc(100vh - 116px)"}}>
      <Routes>
        / → <Home/>
        /about → <About/>
        /project → <Project/>
        /blog → <Blog/>
        /blog/:slug → <BlogPost/>
        /contact → <Contact/>
        * → <NotFound/>
      </Routes>
    </div>
  </ErrorBoundary>
  <Footer />
</PublicSite>
```

### ۱۴.۲ الگوی صفحه با Sidebar + Content (در About/Project/Contact)
```
<section className="bg-[#0F172B] min-h-[calc(100vh-116px)]
                   flex flex-col md:flex-row
                   md:h-[calc(100vh-116px)] md:overflow-hidden">
  
  <Sidebar />                     {/* fixed-width on md+, full-width on mobile */}
  <Content />                     {/* flex-1 min-w-0, md:overflow-y-auto */}
</section>
```

### ۱۴.۳ الگوی صفحه Full-Screen (Hero)
```
<section className="bg-[#0F172B] min-h-[calc(100vh-116px)] lg:h-[calc(100vh-116px)]
                   flex items-center justify-center">
  <div className="home-page-div w-full flex flex-col lg:flex-row
                  items-center justify-center gap-8 sm:gap-10 lg:gap-12 xl:gap-16
                  px-4 sm:px-8 py-8 sm:py-12 lg:py-0
                  min-h-[calc(100vh-116px)] lg:min-h-0 lg:h-full">
    <LeftContent />                {/* InformationText */}
    <RightContent />               {/* MusicSearch card */}
  </div>
</section>
```

### ۱۴.۴ الگوی Admin Layout
```
<div className="min-h-screen w-full bg-[#0B0F1A] text-[#90A1B9] flex flex-col">
  
  {/* Title bar */}
  <header className="h-11 shrink-0 flex items-center justify-between
                     px-3 sm:px-4 bg-[#0F172B] border-b border-[#1E293B] relative z-20">
    {/* traffic light + admin@portfolio:<span class="purple">~/dashboard</span> + view-site / _logout() */}
  </header>
  
  <div className="flex flex-1 min-h-0 relative">
    {/* mobile overlay */}
    {menuOpen && <div className="md:hidden fixed inset-0 top-11 bg-black/50 z-10" />}
    
    {/* Sidebar (slide-in در موبایل) */}
    <aside className={`${menuOpen ? "translate-x-0" : "-translate-x-full"}
                       md:translate-x-0 transition-transform duration-200
                       flex flex-col w-64 shrink-0 bg-[#0F172B] border-r border-[#1E293B]
                       fixed md:static top-11 md:top-auto bottom-0 md:bottom-auto left-0 z-20
                       overflow-y-auto`}>
      <p className="px-4 pt-4 pb-2 text-[10px] tracking-wider text-[#4B576D] uppercase">// explorer</p>
      <nav className="flex flex-col px-2 gap-0.5">
        {navItems.map(item => (
          <NavLink to={item.to}
            className={({isActive}) => `flex flex-col gap-0.5 rounded-md px-3 py-2.5 text-[12px]
                                        duration-150 border-l-2 relative
                                        ${isActive ? "bg-[#7888a01a] border-l-[#FFB86A] text-white"
                                                   : "border-l-transparent text-[#90A1B9] hover:bg-[#7888a00d] hover:text-white"}`}>
            <div className="flex items-center justify-between w-full">
              <span className="flex items-center gap-2">
                <span className="text-[#615FFF]">#</span>
                <span>{item.label}</span>   {/* stats.tsx, projects.json, blog, ... */}
              </span>
              {item.isInbox && unseenMessagesCount > 0 && (
                <span className="... animate-pulse">{count}</span>
              )}
            </div>
            <span className="text-[10px] text-[#68768C] pr-4">{item.hint}</span>
          </NavLink>
        ))}
      </nav>
      <div className="mt-auto p-4 text-[10px] text-[#4B576D] border-t border-[#1E293B]">
        admin-panel v1.0.0
      </div>
    </aside>
    
    {/* Main */}
    <main className="flex-1 min-w-0 overflow-y-auto">
      <div className="p-4 sm:p-6 lg:p-8 max-w-[1400px] mx-auto">
        <Outlet />
      </div>
    </main>
  </div>
</div>
```

### ۱۴.۵ الگوی Admin Login
```jsx
<div className="min-h-screen w-full bg-[#0B0F1A] flex items-center justify-center px-4">
  <div className="w-full max-w-md rounded-lg border border-[#1E293B] bg-[#0F172B]
                  overflow-hidden shadow-2xl">
    
    {/* ترمینال هدر با traffic lights */}
    <div className="h-10 flex items-center gap-2 px-4 bg-[#0b1220] border-b border-[#1E293B]">
      <span className="w-2.5 h-2.5 rounded-full bg-[#FF5F56]" />
      <span className="w-2.5 h-2.5 rounded-full bg-[#FFBD2E]" />
      <span className="w-2.5 h-2.5 rounded-full bg-[#27C93F]" />
      <p className="ml-2 text-[10px] text-[#68768C]">admin-login.sh</p>
    </div>
    
    <form className="p-6 sm:p-8 flex flex-col gap-6">
      <div>
        <p className="text-[#615FFF] text-[12px]">$ ./login --panel=admin</p>
        <h1 className="text-white text-[18px] mt-2">Admin Panel</h1>
        <p className="text-[#68768C] text-[11px] mt-1">Enter your credentials to access the dashboard</p>
      </div>
      {/* inputs با کلاس مشابه فرم contact */}
    </form>
  </div>
</div>
```

---

## ۱۵. فرم‌ها، ورودی‌ها و دکمه‌ها

### ۱۵.۱ کلاس پایه‌ی Input
```jsx
const inputClass =
  "bg-[#020618] py-2.5 px-2 border-0 outline-[#314158] outline-1
   hover:outline-[#90A1B9] focus:text-[#90A1B9] duration-150
   rounded-md w-full text-[#90a1b9c7]";
```
- **رنگ متن**: `text-[#90a1b9c7]` (آبی-خاکستری با 78% opacity)
- **Focus**: outline به `#90A1B9`، متن به `#90A1B9`
- **Hover**: outline به `#90A1B9`
- **Border**: صفر، فقط outline-1
- **پس‌زمینه**: `#020618` (تیره‌ترین)

### ۱۵.۲ Input با حالت Focused (MusicSearch input)
```jsx
style={{ border: `1px solid ${isFocused ? "#615FFF" : "#314158"}` }}
```
- border بنفش در focus.

### ۱۵.۳ Select (در فرم Contact)
```jsx
<select className="bg-[#020618] py-2.5 px-2 ... w-[90px] shrink-0 text-[13px] cursor-pointer">
  {countryOptions.map(c => (
    <option className="bg-[#020618] text-[#90A1B9]">{c.value} {c.code}</option>
  ))}
</select>
```

### ۱۵.۴ Textarea (فرم Contact)
```jsx
<textarea className={`${inputClass} h-28 resize-none`} />
```

### ۱۵.۵ دکمه‌ی Submit اصلی (دو طرح)
**طرح A — دکمه‌ی معکوس (فرم contact و login)**:
```jsx
className="w-full py-2.5 rounded-md cursor-pointer duration-150
           bg-[#90A1B9] hover:bg-[#0E1528] outline-1 outline-[#90A1B9]
           text-[#0E1528] hover:text-[#90A1B9]
           disabled:opacity-60 disabled:cursor-not-allowed"
```
- حالت پیش‌فرض: پس‌زمینه‌ی خاکستری روشن، متن تیره.
- در hover: پس‌زمینه‌ی بسیار تیره، متن خاکستری روشن (معکوس می‌شود).

**طرح B — دکمه‌ی بنفش MusicSearch**:
```jsx
className="rounded-lg bg-[#1D293D] w-full text-[13px] font-medium text-white py-2.5 px-3
           cursor-pointer ring-1 ring-[#314158] hover:bg-[#615FFF] hover:ring-[#615FFF]
           duration-150 disabled:opacity-50 transition-all flex items-center justify-center gap-2 shadow-sm"
```
- حالت عادی: پس‌زمینه‌ی آبی-تیره با ring 1px خاکستری
- در hover: پس‌زمینه و ring بنفش

**طرح C — دکمه‌ی بنفش gradient (success modal)**:
```jsx
className="w-full py-3 px-6 rounded-xl font-medium text-[14px] text-white
           transition-all duration-200
           bg-gradient-to-r from-[#615FFF] to-[#4F46E5]
           hover:from-[#7573FF] hover:to-[#5B54F6]
           shadow-[0_4px_20px_rgba(97,95,255,0.4)]
           hover:shadow-[0_6px_25px_rgba(97,95,255,0.6)]
           active:scale-[0.98] cursor-pointer"
```

**طرح D — لینک پروژه**:
```jsx
className="text-[13px] inline-flex items-center gap-1.5 px-4 py-1.5 rounded-lg
           text-[#90A1B9] border border-[#90a1b955] bg-[#0F172B]
           transition-all duration-150 hover:border-[#615FFF] hover:text-white hover:bg-[#615FFF11]"
```

### ۱۵.۶ Spinner (درون دکمه)
```jsx
<svg className="animate-spin h-3.5 w-3.5 text-white" fill="none" viewBox="0 0 24 24">
  <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4" />
  <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8H4z" />
</svg>
```

### ۱۵.۷ پیام‌های خطا/موفقیت فرم
```jsx
{statusMessage && (
  <p className={`text-[12px] -mt-3
                 ${status === "error" ? "text-[#FF6B6B]" : "text-[#4ADE80]"}`}>
    // {statusMessage}
  </p>
)}
```
- پیشوند `//` برای لحن کد-مانند.

### ۱۵.۸ دکمه‌های کوچک پلیر (Music)
```jsx
{/* Play/Pause بنفش */}
<button className="bg-[#615FFF] hover:bg-[#7573FF] text-white rounded-lg p-2
                   transition-all duration-150 cursor-pointer shrink-0
                   flex items-center justify-center shadow-md active:scale-95">

{/* Download خاکستری */}
<button className="text-[#90A1B9] hover:text-white p-2 rounded-lg hover:bg-[#1E293B]
                   transition-all duration-150 cursor-pointer disabled:opacity-50 shrink-0">

{/* Volume mute */}
<button className="text-[#68768C] hover:text-[#90A1B9] p-1 rounded transition-colors cursor-pointer shrink-0">
```

### ۱۵.۹ Slider (Range)
**Progress slider (موسیقی)**:
```css
.music-progress {
  -webkit-appearance: none; appearance: none;
  height: 6px; border-radius: 9999px; outline: none;
}
.music-progress::-webkit-slider-thumb {
  -webkit-appearance: none; appearance: none;
  width: 12px; height: 12px; border-radius: 50%;
  background: #615FFF; cursor: pointer;
  box-shadow: 0 0 6px rgba(97,95,255,0.7);
  border: 2px solid #ffffff;
}
```

**Volume slider**:
```css
.music-vol {
  -webkit-appearance: none; appearance: none;
  height: 4px; border-radius: 9999px; outline: none;
}
.music-vol::-webkit-slider-thumb {
  width: 10px; height: 10px; border-radius: 50%;
  background: #A5B4FC; cursor: pointer; border: 1.5px solid #ffffff;
}
```
- پر بودن با linear-gradient: `linear-gradient(to right, #615FFF ${pct}%, #1E293B ${pct}%)`

### ۱۵.۱۰ Country Code Selector (Contact Form)
- استفاده از `libphonenumber-js` (`AsYouType`, `getCountryCallingCode`).
- کشور پیش‌فرض: `IR`.
- پس از موفقیت فرم، فقط کنترل‌شده‌ها ریست می‌شوند (نه با `form.reset()` چون select را خراب می‌کند).

---

## ۱۶. Modal و Overlay

### ۱۶.۱ Success Modal (پیام موفقیت فرم Contact)
```jsx
<div className="fixed inset-0 z-50 flex items-center justify-center p-4
            bg-black/70 backdrop-blur-md animate-fadeIn" onClick={onClose}>
  
  <div className="relative w-full max-w-md bg-[#091122]
              border border-[#314158] rounded-2xl shadow-2xl overflow-hidden"
       style={{
         animation: "modalPop 0.35s cubic-bezier(0.16, 1, 0.3, 1) forwards",
         boxShadow: "0 25px 60px -15px rgba(0,0,0,0.8), 0 0 35px rgba(97,95,255,0.15)",
       }}
       onClick={(e) => e.stopPropagation()}>
    
    {/* ترمینال هدر با traffic lights */}
    <div className="flex items-center justify-between px-5 py-3.5
                border-b border-[#1E293B] bg-[#060D1A]/80">
      <div className="flex items-center gap-2">
        <span className="w-2.5 h-2.5 rounded-full bg-[#FF5F56]/80" />
        <span className="w-2.5 h-2.5 rounded-full bg-[#FFBD2E]/80" />
        <span className="w-2.5 h-2.5 rounded-full bg-[#27C93F]/80" />
        <span className="text-[11px] font-mono text-[#68768C] ml-2 tracking-wider uppercase">
          // status: message_dispatched
        </span>
      </div>
      <button onClick={onClose}
              className="w-7 h-7 rounded-lg text-[#90A1B9] hover:text-white hover:bg-[#1E293B]
                         flex items-center justify-center transition-colors text-[14px]"
              title="Close (ESC)">
        <svg className="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" strokeWidth="2.5">
          <path strokeLinecap="round" strokeLinejoin="round" d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>
    </div>
    
    {/* Body */}
    <div className="p-6 sm:p-8 flex flex-col items-center text-center">
      
      {/* موفقیت Badge */}
      <div className="relative mb-5">
        <div className="w-20 h-20 rounded-full flex items-center justify-center
                    bg-gradient-to-tr from-[#00D5BE]/20 to-[#615FFF]/20
                    border border-[#00D5BE]/40"
             style={{ animation: "pulseGlow 3s infinite ease-in-out" }}>
          <div className="w-14 h-14 rounded-full bg-[#00D5BE] flex items-center justify-center
                      text-black shadow-lg"
               style={{ animation: "iconBounce 0.5s ease-out forwards" }}>
            <svg className="w-8 h-8 text-[#020618]" ...><path d="M5 13l4 4L19 7" /></svg>
          </div>
        </div>
      </div>
      
      <h3 className="text-white text-[20px] sm:text-[22px] font-bold mb-2">
        Message Sent Successfully!
      </h3>
      <p className="text-[13px] text-[#A5B4FC] font-mono mb-4">
        Thank you {name}! Message received.
      </p>
      
      {/* Card message */}
      <div className="bg-[#020618]/70 border border-[#1E293B] rounded-xl p-4 w-full text-left mb-6">
        <p className="text-[13px] sm:text-[14px] text-[#CBD5E1] leading-7">{...}</p>
        <div className="mt-3 pt-3 border-t border-[#1E293B] flex items-center justify-between text-[11px] text-[#68768C]">
          <span className="font-mono text-[#00D5BE] flex items-center gap-1">...Delivery Confirmed</span>
          <span>Fast Response</span>
        </div>
      </div>
      
      <button className="w-full py-3 px-6 rounded-xl font-medium text-[14px] text-white
                     transition-all duration-200
                     bg-gradient-to-r from-[#615FFF] to-[#4F46E5]
                     hover:from-[#7573FF] hover:to-[#5B54F6]
                     shadow-[0_4px_20px_rgba(97,95,255,0.4)]
                     hover:shadow-[0_6px_25px_rgba(97,95,255,0.6)]
                     active:scale-[0.98] cursor-pointer">
        Got it (Close)
      </button>
    </div>
  </div>
</div>
```

### ۱۶.۲ رفتارهای Modal
- کلیک روی backdrop → بستن
- کلیک روی محتوا → `e.stopPropagation()` (نمی‌بندد)
- کلید ESC → بستن (با `keydown` listener)
- `bg-black/70 backdrop-blur-md animate-fadeIn`
- `z-50` (زیر BootLoader و PageTransition که z-99999 هستند)

### ۱۶.۳ ErrorBoundary Fallback
```jsx
<div className="flex-1 flex flex-col items-center justify-center bg-[#0F172B]
            min-h-[calc(100vh-116px)] text-[#90A1B9] gap-4">
  <p className="text-[16px]">Something went wrong.</p>
  <button onClick={() => { setState({hasError:false}); window.location.reload(); }}
          className="px-4 py-2 rounded-md border border-[#90A1B9] text-[#90A1B9]
                     hover:text-white hover:border-white transition-all duration-200 cursor-pointer">
    Reload page
  </button>
</div>
```

---

## ۱۷. Loading / Skeleton

### ۱۷.۱ Component: `<Loading variant="..." />`
5 نوع اسکلتون:

| variant | چه چیزی را شبیه‌سازی می‌کند | count پیش‌فرض |
|---|---|---|
| `cards` (default) | کارت پروژه (با grid 22px، 2px accent line بالا) | 4 |
| `article` | پاراگراف مقاله (عرض‌های متغیر 92%, 100%, 86%, 97%, 64%) | – |
| `skills` | کارت مهارت (با آیکون مربعی + 2 خط) | 6 |
| `music` | پلیر موسیقی (header + visualizer + progress + controls) | – |
| `lines` | فقط 4 خط ساده | – |

### ۱۷.۲ Base Bar Component
```jsx
function Bar({ w = "100%", h = 10, r = 4, delay = 0, style }) {
  return <span className="skeleton-bar block shrink-0"
                style={{ width: w, height: h, borderRadius: r, animationDelay: `${delay}ms`, ...style }} />;
}
```

### ۱۷.۳ PosterSkeleton (کارت پروژه)
```jsx
<div className="flex flex-col overflow-hidden"
     style={{ background: CARD_BG, border: `1px solid ${BORDER}`, borderRadius: 12 }}>
  
  {/* preview area (132px) با grid 22px */}
  <div className="relative flex items-center justify-center"
       style={{
         height: 132,
         borderBottom: `1px solid ${BORDER}`,
         backgroundImage:
           "linear-gradient(to right, rgba(144,161,185,0.05) 1px, transparent 1px), linear-gradient(to bottom, rgba(144,161,185,0.05) 1px, transparent 1px)",
         backgroundSize: "22px 22px",
       }}>
    {/* 2px accent line بالا */}
    <span style={{
      position: "absolute", top: 0, left: 0, right: 0, height: 2,
      background: `linear-gradient(90deg, transparent, ${PURPLE}66, transparent)`,
    }} />
    <Bar w={38} h={38} r={10} delay={delay} style={{ opacity: 0.5 }} />
  </div>
  
  {/* body */}
  <div className="flex flex-col gap-2 p-4">
    <Bar w="70%" h={12} delay={delay + 60} />
    {Array.from({length: lines}).map((_, i) => (
      <Bar key={i} w={i === lines - 1 ? "45%" : "100%"} h={9} delay={delay + 120 + i * 60} />
    ))}
  </div>
</div>
```

### ۱۷.۴ Skeleton Grid
```jsx
className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 sm:gap-6"
```

### ۱۷.۵ Skeleton Tokens
```js
const CARD_BG = "#081224";
const BORDER  = "#90a1b933";
const PURPLE  = "#615FFF";
```

### ۱۷.۶ Loading Empty State
```jsx
<div className="flex items-center justify-center h-40">
  <span className="text-[13px]" style={{ color: gray }}>// no projects found</span>
</div>
```

### ۱۷.۷ a11y
```jsx
<div role="status" aria-live="polite" ...>
  <span className="sr-only">loading</span>
  ...
</div>
```

### ۱۷.۸ کلاس sr-only (screen-reader only)
```css
.sr-only {
  position: absolute; width: 1px; height: 1px; padding: 0;
  margin: -1px; overflow: hidden; clip: rect(0, 0, 0, 0);
  white-space: nowrap; border-width: 0;
}
```

### ۱۷.۹ Empty State Box
```jsx
<div className="flex items-center justify-center h-40">
  <span className="text-[13px]" style={{ color: gray }}>// no projects found</span>
</div>
```

---

## ۱۸. بریک‌پوینت‌ها و ریسپانسیو

### ۱۸.۱ Breakpoint ها (Tailwind v4 - موبایل-فرست)
| نام | px | کاربرد در پروژه |
|---|---|---|
| base | 0 | موبایل |
| `sm` | 640 | تبلت کوچک |
| `md` | 768 | تبلت / لپتاپ کوچک |
| `lg` | 1024 | لپتاپ |
| `xl` | 1280 | دسکتاپ |
| `2xl` | 1536 | مانیتور بزرگ |

### ۱۸.۲ استراتژی ریسپانسیو
**Mobile-First**: پایه‌ی کلاس‌ها موبایل است؛ `sm:`, `md:`, `lg:`, `xl:`, `2xl:` به ترتیب اضافه می‌کنند.

### ۱۸.۳ الگوهای ریسپانسیو پرتکرار
**اندازه‌ی متن تطبیقی**:
```jsx
text-[12px] sm:text-[14px]              // sub-text
text-[13px] sm:text-[16px]              // greeting
text-[16px] sm:text-[20px] md:text-[24px] lg:text-[28px]   // "> Front-end developer"
text-[26px] sm:text-[38px] md:text-[46px] lg:text-[54px] xl:text-[58px]   // H1 Hero
text-[10px] sm:text-[11px]              // badge
text-[12px] sm:text-[13px]              // button
text-[12px] sm:text-[14px] lg:text-[16px]   // footer
```

**Padding تطبیقی**:
```jsx
px-2 sm:px-4 lg:px-0       // nav
px-3 sm:px-6 md:px-8 lg:px-10 lg:px-12      // content
p-5 sm:p-6                 // card
py-2.5                     // input
py-3 sm:py-4 lg:py-0       // mobile-to-desktop vertical centering
px-4 sm:px-8               // section horizontal
py-8 sm:py-12 lg:py-0      // section vertical
```

**Gap تطبیقی**:
```jsx
gap-3.5 sm:gap-4 lg:gap-5
gap-1.5 sm:gap-2.5
gap-2 sm:gap-3 lg:gap-4
gap-4 sm:gap-6
gap-8 sm:gap-10 lg:gap-12 xl:gap-16
gap-8 sm:gap-12 lg:gap-20 xl:gap-28
```

**عرض سایدبار تطبیقی**:
```jsx
w-full md:w-60 lg:w-72 xl:w-[360px] 2xl:w-[457px]
```

### ۱۸.۴ الگوهای Show/Hide
```jsx
hidden md:flex              // فقط دسکتاپ
md:hidden                   // فقط موبایل
hidden lg:block             // فقط دسکتاپ بزرگ
```

### ۱۸.۵ الگوهای Flex Direction تطبیقی
```jsx
flex flex-col lg:flex-row
```

### ۱۸.۶ الگوهای مین/مکس width
```jsx
max-w-md lg:max-w-[420px]               // MusicSearch card
max-w-[24rem]                           // Contact form
max-w-[1400px] mx-auto                  // Admin main content
max-w-[410px]                           // PageTransition card
min-w-0 flex-1                          // Sidebar+content (الگوی اصلی)
```

---

## ۱۹. آیکون‌ها

### ۱۹.۱ آیکون‌های SVG درون‌خطی (Heroicons-style)
استفاده از SVG های inline برای کنترل‌های کوچک:
- Play/Pause: `<rect>` های چاقویی (4×16 و 14×4)
- Download: مسیر فلش رو به پایین + خط زیرین
- Volume mute/low/high: 3 مسیر مختلف
- چک‌مارک: `<path d="M1 4L3.5 6.5L9 1">`
- ضربدر: `<path d="M6 18L18 6M6 6l12 12">`
- چک‌مارک موفقیت: `<path d="M5 13l4 4L19 7">`
- مثلث warning: مسیر هشدار

### ۱۹.۲ آیکون‌های فایلی (در `/public/assets/Images/`)
| فایل | کاربرد |
|---|---|
| `Vector.svg` | Chevron برای collapse (با rotate-90 در حالت باز) |
| `icon folder.svg` | آیتم bio در SubjectBox |
| `icon folder2.svg` | آیتم education |
| `icon folder3.svg` | آیتم skills |
| `icon message.svg` | ایمیل |
| `icon phone.svg` | تلفن |
| `RiLinkedinFill.png` | لوگو LinkedIn |
| `BasilTelegramSolid.png` | لوگو Telegram |
| `TablerBrandInstagram.png` | لوگو Instagram |
| `MdiGithub.png` | لوگو GitHub |
| `404.png` | تصویر صفحه‌ی 404 |

### ۱۹.۳ آیکون‌های فیلتر پروژه
| فایل | دسته |
|---|---|
| `reavt filter (5).png` | React |
| `js filter (2).png` | JavaScript |
| `tailwind filter (1).png` | Tailwind |
| `html filter (3).png` | HTML |
| `css filter (4).png` | CSS |

### ۱۹.۴ آیکون‌های مهارت
- `html_1051277.png`
- `css_919826.png`
- `js_5968292.png`
- `react.png`
- `tailwind.png`
- `typescript.png`
- `wordpress_174881.png`

---

## ۲۰. الگوهای تعامل و Micro-interactions

### ۲۰.۱ Hover روی آیتم‌های Sidebar
```jsx
className="hover:bg-[#7888a033] duration-100 cursor-pointer"  // tight (100ms)
className="hover:text-white hover:bg-[#7888a01a] transition-all duration-150 cursor-pointer min-w-0"  // medium (150ms)
```

### ۲۰.۲ Hover روی کارت
```jsx
// پروژه
className="hover:border-[#615FFF] hover:-translate-y-1 hover:shadow-[0_0_35px_-5px_rgba(97,95,255,0.25)] transition-all duration-300"

// مهارت / Education
style={{
  background: hovered ? "#0F172B" : "#081224",
  border: `1px solid ${hovered ? purple : "#90a1b933"}`,
  transform: hovered ? "translateY(-3px)" : "translateY(0)",
}}
```

### ۲۰.۳ Hover روی Monogram (پروژه)
```jsx
style={{
  transform: hovered ? "scale(1.1) rotate(-6deg)" : "scale(1) rotate(0deg)",
}}
```

### ۲۰.۴ Active Visual Feedback
```jsx
active:scale-[0.98]    // modal button
active:scale-95        // play button
```

### ۲۰.۵ Disable State
```jsx
disabled:opacity-50 disabled:cursor-not-allowed
```

### ۲۰.۶ Click Tracking Hook
تمام المان‌های تعاملی (nav، فیلتر، کارت، لینک) با `useClickTrack` track می‌شوند:
```jsx
const { trackClick, withTracking } = useClickTrack();

// Manual:
trackClick({ targetType: "button", targetId: "music-search", targetLabel: "Music Search" });

// HOF:
<a onClick={withTracking({
  targetType: "project",
  targetId: project.title?.toLowerCase().replace(/\s+/g, "-") || "unknown",
  targetLabel: project.title || "Unknown Project",
})} />
```
- Throttling: ۵۰۰ms در هر کلید (`targetType::targetId`).
- در صورت وجود `admin_token` در localStorage: track نمی‌شود.

### ۲۰.۷ Hover state (Monogram gradient)
```jsx
opacity: hovered ? 1 : 0.4,  // top accent
opacity: hovered ? 0.9 : 0.4, // top-right glow
opacity: hovered ? 0.8 : 0.3, // bottom-left glow
transition: "opacity 0.4s",
```

### ۲۰.۸ Corner Brackets (پروژه)
```jsx
className="absolute ${pos} w-4 h-4 border-${dir}-2 transition-all duration-300"
style={{ borderColor: "#615FFF66", opacity: hovered ? 1 : 0.6 }}
```

### ۲۰.۹ ترتیب z-index
| لایه | z-index | المان |
|---|---|---|
| PageTransition + BootLoader | 99999 | `<aside>` / `<div>` با z-[99999] |
| Cursor | 9999 | `.cursor-ring`, `.cursor-dot` |
| Admin mobile overlay | 10 | – |
| Admin sidebar (mobile) | 20 | – |
| Admin header | 20 | – |
| Modal | 50 | Success Modal |
| Header (سکشن عمومی) | 50 | `<header>` |
| mobile-nav-panel | 40 | – |
| mobile-menu label | 50 | – |

---

## ۲۱. دسترسی‌پذیری (Accessibility)

### ۲۱.۱ اصول کلی
- هر دکمه‌ی آیکون‌دار: `aria-label` یا `title`.
- لینک‌های خارجی: `target="_blank" rel="noopener noreferrer"`.
- فیلدها: `<label>` با `flex-col gap-1.5`، متن label قبل از input.
- تصاویر معنادار: `alt` کامل، تصاویر تزئینی: `alt=""`.
- وضعیت‌ها: `aria-live="polite"` روی Loading و PageTransition.
- نقش‌ها: `role="status"` روی Loading، `role="status" aria-live="polite" aria-label="Loading portfolio"` روی BootLoader.

### ۲۱.۲ prefers-reduced-motion
تمام انیمیشن‌های اصلی با media query مربوطه غیرفعال می‌شوند (بخش ۱۱.۴).

### ۲۱.۳ Typewriter Caret
- پس از اتمام تایپ، با `data-typed="done"` روی والد، caret حذف می‌شود:
```css
[data-typed="done"] .ti-cursor {
  display: none !important; opacity: 0 !important; visibility: hidden !important;
}
```

### ۲۱.۴ keyboard
- تمام دکمه‌ها `<button>` هستند (نه `<div onClick>`).
- Form contact: `<select>` بومی، `<input>` بومی، `<textarea>` بومی.
- ESC در Modal: `window.addEventListener("keydown", ...)`.
- Tab navigation طبیعی در همه‌ی فرم‌ها.

### ۲۱.۵ Screen Reader only
- `.sr-only` در همه‌ی Loading state ها: `<span className="sr-only">loading</span>`.

### ۲۱.۶ Pointer
- `prefers-reduced-motion` غیرفعال نمی‌کند cursor، فقط backdrop-filter blur را برمی‌دارد.
- Cursor فقط در `pointer: fine` فعال می‌شود، در موبایل/تاچ کاملاً غیرفعال.

### ۲۱.۷ Color Contrast
- متن اصلی: `#FFFFFF` روی `#0F172B` → contrast ratio بسیار بالا
- متن secondary: `#90A1B9` روی `#0F172B` → قابل قبول (AA)
- متن muted: `#68768C` روی `#0F172B` → فقط برای متن کم‌اهمیت

---

## ۲۲. SEO و Metadata

### ۲۲.۱ Hook: `usePageSEO`
- در هر page component فراخوانی می‌شود.
- title، description، canonical، og:image، og:title، twitter:card، JSON-LD schema.

### ۲۲.۲ الگوی صفحات دارای SEO
```jsx
usePageSEO({
  title: "About Me | Mohammad Mehdi Sadeghi - Bio, Skills & Education",
  description: "Learn more about Mohammad Mehdi Sadeghi...",
  canonical: "https://mohammad-mehdi-sadeghi.vercel.app/about",
  image: "/og-preview.png",
  schema: {
    "@context": "https://schema.org",
    "@type": "AboutPage",
    ...
  }
});
```

### ۲۲.۳ Schema types استفاده‌شده
- `AboutPage` (در /about)
- `CollectionPage` (در /project)
- `ContactPage` (در /contact)
- `Person` (در همه‌ی entity ها)

---

## ۲۳. کنوانسیون‌های کدنویسی

### ۲۳.۱ نام‌گذاری فایل
- Components: PascalCase + index.jsx
  - مثلاً `src/Components/Header/index.jsx`
- Hooks: camelCase + .js / .jsx
  - مثلاً `src/Hooks/useActiveNav.jsx`
- Pages: PascalCase + index.jsx
  - مثلاً `src/Page/Home/index.jsx`
- utils: camelCase + .js
  - مثلاً `src/lib/blog.js`

### ۲۳.۲ ساختار پوشه
```
src/
├── App.jsx               (router اصلی)
├── main.jsx              (entry)
├── index.css             (CSS سراسری + tailwind import)
├── App.css               (خالی - فقط import)
├── Components/           (shared components)
│   ├── Header/index.jsx
│   ├── Footer/index.jsx
│   ├── BootLoader/index.jsx
│   ├── Cursor/index.{jsx,css}
│   ├── Music/index.jsx
│   ├── PageTransition/index.jsx
│   ├── Loading/index.jsx
│   ├── SnakeBar/index.jsx
│   ├── Typewriter/index.jsx
│   ├── ErrorBoundary/index.jsx
│   └── VisitTracker/index.jsx
├── Hooks/                (custom hooks)
│   ├── useActiveNav.jsx
│   ├── useAdminAuth.jsx
│   ├── useClickTrack.js
│   ├── usePageSEO.js
│   └── useSiteInfo.jsx
├── Page/                 (route pages)
│   ├── Home/
│   ├── About/
│   ├── Project/
│   ├── Blog/
│   ├── Contact/
│   ├── NotFound/
│   └── Admin/
└── lib/                  (utilities)
    ├── blog.js
    ├── imageCompress.js
    ├── moodEngine.js
    └── musicUrl.js
```

### ۲۳.۳ استایل
- **بدون CSS-in-JS**: فقط Tailwind utilities + CSS سفارشی در `index.css` و فایل‌های `.css` کنار کامپوننت.
- **استایل‌های داینامیک**: inline `style={{...}}` با token های ثابت.
- **Animation های پیچیده**: در `<style>{`...`}</style>` درون JSX.
- **بدون classnames utility**: از template literal و ternary استفاده می‌شود.

### ۲۳.۴ Constant Token درون فایل
```jsx
// رایج در همه‌ی فایل‌ها:
const gray = "#90A1B9";
const purple = "#615FFF";
const turquoise = "#00D5BE";
```
- به‌جای تماس با تمیز، هر component ثابت‌های خودش را تعریف می‌کند.

### ۲۳.۵ کامنت‌ها
- همه‌ی component های پیچیده کامنت‌های `/* ==== ... ==== */` دارند.
- توضیح تصمیم‌ها به‌صورت WHY (نه WHAT) نوشته شده.

### ۲۳.۶ Imports
- React: named import (`import { useState } from "react"`).
- Icons: inline SVG (Heroicons style).
- Styles: relative path (`./index.css`).
- Components: relative (`../../Components/Foo`).

---

## ۲۴. تنظیمات Build و Deploy

### ۲۴.۱ Vite Config (`vite.config.js`)
```js
base: process.env.VERCEL ? "/" : "./"
server: { host: '0.0.0.0', port: 5173 }
plugins: [react(), tailwindcss(), mockApiPlugin(), stripServerSources()]
```

### ۲۴.۲ ESLint Flat Config
- `eslint.config.js` با flat config.
- پلاگین‌ها: `@eslint/js`, `eslint-plugin-react-hooks`, `eslint-plugin-react-refresh`.

### ۲۴.۳ Vercel-specific
- در Vercel، `base: "/"` (absolute path) برای جلوگیری از خطا در sub-path ها.
- پلاگین `stripServerSources` فایل‌های `.php`, `.sql`, `.bak`, `.ini`, `.env`, `.log`, `.htaccess` را از dist حذف می‌کند.

### ۲۴.۴ Scripts
```json
{
  "dev": "vite",
  "build": "vite build",
  "lint": "eslint .",
  "preview": "vite preview",
  "start": "node server.js"
}
```

---

## ۲۵. چک‌لیست استفاده از Design System

هنگام ساخت یک کامپوننت جدید، این چک‌لیست را دنبال کنید:

- [ ] پس‌زمینه: `bg-[#0F172B]` (سکشن اصلی) یا `bg-[#081224]` (کارت) یا `bg-[#020618]` (input)
- [ ] متن primary: `text-white`
- [ ] متن secondary: `text-[#90A1B9]` یا constant `gray`
- [ ] متن muted: `text-[#68768C]` یا `#3d4f6b`
- [ ] border: `border-[#90a1b977]` (default) یا `border-[#314158]` (input) یا `border-[#90a1b933]` (card)
- [ ] radius: `rounded-lg` (cards), `rounded-xl` (modals, big cards), `rounded-md` (inputs)
- [ ] hover: `duration-150` یا `duration-300` با تغییر رنگ/transform
- [ ] active: `bg-[#7888a033] text-white border-b-[#90A1B9]`
- [ ] focus: `outline-[#90A1B9]` (input)
- [ ] کلیک tracking: `useClickTrack().withTracking({...})`
- [ ] ریسپانسیو: پایه موبایل، sm/md/lg/xl/2xl به‌ترتیب
- [ ] اگر loading دارید: `<Loading variant="..." />`
- [ ] اگر دکمه‌ی submit دارید: از یکی از ۴ طرح
- [ ] اگر متن‌کد دارید: از syntax highlighting palette (بخش ۱۳.۱۶)
- [ ] اگر انیمیشن جدید دارید: GPU-friendly (`transform`, `opacity`) نه `width`/`height`
- [ ] اگر تصویر مهم است: `alt` کامل بنویسید
- [ ] اگر المان تعاملی آیکونیک است: `aria-label` اضافه کنید
- [ ] اگر لینک خارجی است: `target="_blank" rel="noopener noreferrer"`
- [ ] اگر فرم است: `<label>` + `<input>` بومی + error state یکپارچه
- [ ] اگر صفحه‌ی جدید است: `usePageSEO({...})` فراخوانی کنید
- [ ] اگر state پیچیده است: `useReducer` نه چند `useState`
- [ ] اگر داده از API می‌آید: loading + error + empty state هر سه وجود داشته باشد
- [ ] اگر breakpoint جدید نیاز دارید: اول از الگوهای بخش ۱۸.۳ استفاده کنید
- [ ] اگر ثابت رنگ تکراری نوشتید: آن را به constant در بالای فایل تبدیل کنید
