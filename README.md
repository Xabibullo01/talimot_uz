# Ta'limTop — Xususiy ta'lim platformasi

O'zbekistondagi (hozircha Toshkent bo'yicha) xususiy universitet, maktab va bog'chalarni qidirish, solishtirish va tanlashni osonlashtiruvchi **bitta HTML fayldan iborat** (single-file) frontend prototip (MVP).

Loyihaning maqsadi — foydalanuvchiga bitta saytda barcha xususiy ta'lim muassasalarini ko'rsatish, ularni filtrlash/qidirish, 2–3 tasini yonma-yon solishtirish va kelajakda pullik (Premium) xizmatlar orqali qo'shimcha imkoniyatlar (masalan xarita) taqdim etish.

> ⚠️ Bu — **frontend-only demo**. Backend, baza (database) yoki haqiqiy to'lov tizimi yo'q. Barcha "foydalanuvchi", "ro'yxatdan o'tish", "admin", "Premium" kabi holatlar brauzerning `localStorage`'ida saqlanadi (pastda batafsil tushuntirilgan).

---

## 1. Loyihada nima bor (fayllar tuzilishi)

```
talimot_uz/
├── index.html     ← butun sayt shu bitta faylda: HTML + CSS + JavaScript
├── logo.jpg        ← "Ta'limTop" logotipi (navbar, footer va browser tab ikonkasi — favicon sifatida ishlatiladi)
└── README.md       ← shu fayl
```

Loyihada **hech qanday build tizimi** (npm, webpack, React va h.k.) ishlatilmagan. Hammasi toza HTML/CSS/JavaScript bo'lgani uchun uni ishga tushirish uchun faqat brauzer kifoya.

---

## 2. Qanday ishga tushirish mumkin

### A) Eng oddiy usul — faylni to'g'ridan-to'g'ri ochish
`index.html` faylini ikki marta bosib, istalgan brauzerda (Chrome, Edge, Firefox) oching. Internet aloqasi faqat quyidagilar uchun kerak:
- Muassasalar kartochkalaridagi rasmlar (Unsplash'dan yuklanadi),
- Premium xarita (OpenStreetMap orqali).

Qolgan hammasi (qidiruv, filtr, solishtirish, login, admin) **internetsiz ham ishlaydi**.

### B) Lokal server orqali (tavsiya etiladi)
Ba'zi brauzer xususiyatlari (masalan `localStorage`) `file://` protokolida cheklanishi mumkin, shuning uchun kichik lokal server orqali ochish yaxshiroq:

```bash
# Python o'rnatilgan bo'lsa
cd talimot_uz
python -m http.server 8000
# so'ng brauzerda: http://localhost:8000
```

yoki VS Code'da **Live Server** kengaytmasi orqali ham ochish mumkin.

### C) Internetga joylash (deploy)
Loyiha GitHub'ga ulangan: **https://github.com/Xabibullo01/talimot_uz**

**Vercel** orqali joylashtirilgan — statik sayt bo'lgani uchun build sozlamalari (build command, output directory) shart emas, Vercel `index.html`ni avtomatik asosiy sahifa sifatida taniydi. GitHub'dagi `main` branchga har bir push avtomatik yangi deploy'ni ishga tushiradi.

---

## 3. Texnologiyalar

| Texnologiya | Qayerda ishlatilgan |
|---|---|
| **HTML5** | Sahifa tuzilishi |
| **CSS3** (vanilla, freymvorksiz) | Dizayn, grid/flex layout, responsive (mobil) ko'rinish, CSS o'zgaruvchilar (`:root` orqali) |
| **Vanilla JavaScript** (freymvorksiz — React/Vue yo'q) | Barcha interaktivlik: qidiruv, filtr, modal oynalar, login/admin, Premium, tillar |
| **localStorage (Web Storage API)** | "Baza" o'rnini bosadi — foydalanuvchi, tanlangan tillar, solishtirish ro'yxati, Premium holati, admin qo'shgan muassasalar shu yerda saqlanadi |
| **OpenStreetMap** (`iframe embed`) | Premium xususiyatidagi haqiqiy, interaktiv xarita |
| **Unsplash** rasm linklari | Muassasa kartochkalaridagi fon rasmlari |

Hech qanday tashqi JS kutubxona (jQuery, Bootstrap va h.k.) ulanmagan — bu sahifani juda tez yuklanadigan qiladi.

---

## 4. Sahifa bo'limlari (yuqoridan pastga)

1. **Navbar (yuqori menyu)** — logotip, "Muassasalar / Solishtirish / Hamkorlar" havolalari, til tanlash, **⭐ Premium sotib oling** tugmasi, Kirish / Ro'yxatdan o'tish / Profil tugmalari.
2. **Hero (bosh banner)** — sarlavha, qidiruv maydoni (nom bo'yicha) + tur bo'yicha filtr (`Barchasi / Universitet / Maktab / Bog'cha`), umumiy statistika (necha ta universitet/maktab/bog'cha bor).
3. **"Ta'limni tanlang"** — 3 ta katta kategoriya kartochkasi (Universitet / Maktab / Bog'cha), bosilganda pastdagi ro'yxat avtomatik shu turga filtrlanadi.
4. **"Muassasalar" ro'yxati** (`#muassasalar`) — barcha 30 ta demo muassasa kartochka ko'rinishida, har birida: rasm, turi, nomi, joylashuvi, reytingi, **"⚖️ Solishtirish"** va **"Batafsil"** tugmalari.
5. **"Solishtirish" (`#compare`)** — Solishtirish funksiyasini tushuntiruvchi blok + "Solishtirishni ochish" tugmasi (bu — **Premium'ning bir qismi sifatida belgilangan** xususiyat: cheksiz solishtirish).
6. **"Hamkorlar" (`#partners`)** — bu yerda **xususiy universitetlar ro'yxati** avtomatik chiqadi (bazadagi barcha `university` turidagi yozuvlardan olinadi, har birida "Xususiy universitet" yorlig'i bilan) + "Hamkor bo'lish" tugmasi (demo — faqat xabar chiqaradi).
7. **Admin panel (`#admin`)** — faqat admin sifatida kirilganda ko'rinadi (pastda tushuntirilgan). Statistika va yangi muassasa qo'shish formasi bor.
8. **Footer** — logotip va qisqa tavsif.
9. **Pastki o'ng burchakdagi 💬 Support tugmasi** — doim ko'rinadi, bosilganda Telegram'dagi **[@diyorbek_zoirov](https://t.me/diyorbek_zoirov)** shaxsiga yangi oynada o'tkazadi.

---

## 5. Asosiy funksiyalar batafsil

### 🔎 Qidiruv va filtr
- `searchNow()` — qidiruv maydoniga yozilgan matn bo'yicha (nom, joylashuv, tavsif ichidan) va tanlangan turi bo'yicha ro'yxatni filtrlaydi, so'ng avtomatik ro'yxat bo'limiga skroll qiladi.
- `setType(turi, tugma)` — kategoriya kartasi yoki chip (filtr tugmasi) bosilganda shu turga filtrlaydi.
- `render()` — asosiy funksiya: joriy qidiruv so'zi + tanlangan turga mos muassasalarni kartochka qilib HTML'ga chizadi. Hech narsa topilmasa — "Hech narsa topilmadi" xabari chiqadi.

### ⚖️ Solishtirish
- Har bir kartochkada **"Solishtirish"** tugmasi bor — bosilganda muassasa solishtirish ro'yxatiga (`compareIds`) qo'shiladi (maksimal **3 ta**).
- `#compare` bo'limidagi **"Solishtirishni ochish"** tugmasi — tanlangan muassasalarni jadval ko'rinishida (Tur, Joylashuv, Turi, Baholash) yonma-yon chiqaradi.
- Tanlov `localStorage`'da saqlanadi, sahifa qayta ochilganda ham saqlanib qoladi.

### 👤 Kirish / Ro'yxatdan o'tish (demo autentifikatsiya)
- Haqiqiy backend yo'q — ro'yxatdan o'tganda ism, telefon va parol `localStorage`'ga yoziladi (`registeredUser`).
- Keyingi safar "Kirish" orqali shu telefon+parol bilan kirish mumkin.
- **Admin rejimi (demo):** "Kirish" oynasida **"Admin sifatida kirish"** belgisini qo'yib, quyidagi login bilan kiring:
  - Telefon: **`admin`**
  - Parol: **`admin123`**
  - Bu orqali **Admin panel** ochiladi (statistikalar + yangi muassasa qo'shish formasi).

> ⚠️ Bu login/parol faqat demo maqsadida ochiq kodda yozilgan — **haqiqiy (production) loyihada bunday qilib bo'lmaydi**, albatta backend orqali xavfsiz autentifikatsiya kerak bo'ladi.

### 🛠 Admin panel
- Yangi muassasa (universitet/maktab/bog'cha) qo'shish formasi bor — qo'shilgan muassasa darhol asosiy ro'yxatda ham ko'rinadi va `localStorage`'dagi `customInstitutions`'ga saqlanadi.
- "Ro'yxatdan o'tgan" va "Solishtirish tanlovi" statistikalari — joriy brauzerdagi holatni ko'rsatadi (ko'p foydalanuvchili haqiqiy statistika emas).

### ⭐ Premium (demo, to'lovsiz)
- Navbardagi **"Premium sotib oling"** tugmasi bosilganda modal oyna ochiladi:
  - Premium xususiyatlari ro'yxati: **interaktiv xarita** (yaqin atrofdagi universitet/muassasalarni ko'rish) va **cheksiz solishtirish**.
  - Narx: **50 000 so'm / oy**, **birinchi oy — bepul**.
  - **"Premiumni faollashtirish"** tugmasi — karta/to'lov so'ramaydi, bosilishi bilanoq `isPremium=true` qilib `localStorage`'ga yoziladi (demo rejim).
- Premium faollashtirilgandan so'ng, shu tugma qayta bosilsa — **haqiqiy, interaktiv OpenStreetMap xaritasi** (Toshkent hududi) `iframe` orqali ko'rsatiladi.
- Navbardagi tugma matni avtomatik **"⭐ Premium faol"**ga o'zgaradi.

> 💡 Hozircha to'lov tizimi ulanmagan — bu keyingi bosqichda (masalan Payme/Click orqali) qo'shiladi. Hozirgi holat faqat **demo/prototip** sifatida ishlaydi.

### 🌐 Ko'p tillilik (uz / en / ru)
- Navbardagi til tanlagich orqali sayt matnlari **O'zbek, English, Русский** tillariga almashadi (`setLang()` funksiyasi, `T` obyektidagi tarjimalar lug'ati orqali).
- Tanlangan til `localStorage`'da saqlanadi, sahifa qayta ochilganda eslab qoladi.
- Eslatma: hozircha faqat interfeys matnlari tarjima qilingan — muassasa nomlari va tavsiflari hamma tilda bir xil (O'zbekcha/inglizcha original nomlar).

### 💬 Support (Telegram orqali yordam)
- Sahifaning istalgan joyida pastki o'ng burchakda doim ko'rinadigan aylana tugma.
- Bosilganda yangi tab'da Telegram'dagi **[@diyorbek_zoirov](https://t.me/diyorbek_zoirov)** shaxsiy chatiga o'tkazadi — foydalanuvchi savolini to'g'ridan-to'g'ri shu yerda yozishi mumkin.

---

## 6. Ma'lumotlar qayerda saqlanadi (`localStorage` kalitlari)

Loyihada backend/baza bo'lmagani uchun barcha holat brauzer xotirasida saqlanadi. Bu degani — **ma'lumotlar faqat shu brauzer + shu qurilmada** turadi, boshqa foydalanuvchiga yoki boshqa qurilmaga ko'rinmaydi.

| `localStorage` kaliti | Nima uchun ishlatiladi |
|---|---|
| `talimUser` | Joriy tizimga kirgan foydalanuvchi (ism, telefon, admin bo'lish/emasligi) |
| `registeredUser` | Ro'yxatdan o'tgan foydalanuvchining login ma'lumotlari (demo, bitta foydalanuvchi uchun) |
| `customInstitutions` | Admin panel orqali qo'shilgan yangi muassasalar |
| `compareIds` | Solishtirish uchun tanlangan muassasa ID'lari |
| `lang` | Tanlangan til (`uz` / `en` / `ru`) |
| `isPremium` | Premium faollashtirilgan-faollashtirilmaganligi |

**Eslatma:** Brauzer keshi/cookie'larini tozalasangiz yoki boshqa brauzer/qurilmadan kirsangiz — bu holatlar qayta "default"ga qaytadi (login, Premium va h.k. o'chadi). Bu — haqiqiy backend bo'lmagani uchun kutilgan holat.

---

## 7. Dizayn tizimi (CSS)

Barcha asosiy ranglar va o'lchamlar `index.html`ning `<style>` qismida, `:root` ichida CSS-o'zgaruvchilar sifatida belgilangan:

```css
--p: #5b5ce2;      /* asosiy (binafsha-ko'k) rang — tugmalar, havolalar */
--p2: #7c3aed;      /* qo'shimcha aksent rang */
--dark: #111827;    /* asosiy matn rangi */
--muted: #667085;   /* ikkinchi darajali (xira) matn */
--bg: #f6f7fb;       /* sahifa foni */
--line: #e6e8ef;     /* chegaralar rangi */
--shadow: ...        /* kartochkalar soyasi */
```

Ranglarni o'zgartirish uchun shu joydagi qiymatlarni almashtirish kifoya — butun sayt bo'ylab avtomatik yangilanadi.

**Responsive (mobil moslashuv):** `@media(max-width:900px)` va `@media(max-width:620px)` orqali planshet va telefon o'lchamlariga moslashtirilgan (menyu yig'iladi, grid ustunlar kamayadi, qidiruv paneli vertikal bo'ladi va h.k.).

---

## 8. Qanday o'zgartirish kiritish mumkin (tez-tez kerak bo'ladigan ishlar)

### Yangi muassasa qo'shish (kod orqali, doimiy)
`index.html` ichidagi `const data=[...]` massiviga quyidagi formatda yangi qator qo'shing:

```js
['Muassasa nomi','university','Toshkent','Tavsif (masalan: Xususiy universitet)','4.8'],
```
(`'university'` o'rniga `'school'` yoki `'kindergarten'` yozish mumkin)

### Logotipni almashtirish
`logo.jpg` faylini xohlagan rasm bilan almashtiring (fayl nomi bir xil bo'lsa, kod o'zgartirish shart emas).

### Premium narxini o'zgartirish
`index.html` ichida `renderPremiumContent()` funksiyasidagi `<b>50 000</b>` qismini xohlagan narxga almashtiring.

### Support (Telegram) username'ni o'zgartirish
Fayl oxiridagi:
```html
<a class="supportBtn" href="https://t.me/diyorbek_zoirov" ...>
```
qatoridagi `diyorbek_zoirov`ni yangi usernamega almashtiring.

---

## 9. Ma'lum cheklovlar (hozirgi holat)

- ❌ Backend/server yo'q — hamma narsa frontend va `localStorage` orqali ishlaydi.
- ❌ Haqiqiy to'lov tizimi ulanmagan (Premium — demo, bepul faollashadi).
- ❌ Login/parol xavfsiz emas — production uchun yaroqsiz (faqat demo maqsadida).
- ❌ Ma'lumotlar bazasi yo'q — barcha muassasa ma'lumotlari kod ichida qattiq yozilgan (`hardcoded`).
- ❌ Bitta brauzer/qurilmada saqlanadi — turli qurilmalar orasida sinxronizatsiya yo'q.
- ✅ Lekin bu — **tez ishlab chiqilgan, to'liq ishlaydigan frontend prototip**, keyingi bosqichda backend (masalan Node.js/Express + MongoDB yoki Firebase) ulash orqali haqiqiy loyihaga aylantirish mumkin.

---

## 10. Kelajakdagi rejalar (roadmap g'oyalar)

- [ ] Haqiqiy backend va baza (foydalanuvchilar, muassasalar server tomonida saqlansin)
- [ ] Haqiqiy to'lov tizimi (Payme / Click) orqali Premium obuna
- [ ] Xaritada har bir muassasaning aniq joylashuvi (koordinatalar bilan pin'lar)
- [ ] Muassasa nomlari/tavsiflarini ham tilga qarab tarjima qilish
- [ ] Rasmlarni haqiqiy muassasa suratlari bilan almashtirish

---

## 11. Muallif / Yordam

Savol yoki yordam kerak bo'lsa — saytning pastki o'ng burchagidagi 💬 tugma orqali yoki to'g'ridan-to'g'ri Telegram'da: **[@diyorbek_zoirov](https://t.me/diyorbek_zoirov)**
