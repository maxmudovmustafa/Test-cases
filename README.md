# Sotuv oqimi — test keyslar (online + offline)

> **Qoida: faqat telefon ekranida ko'rinadigan narsa tekshiriladi.**
> Bu hujjatda serverda, admin panelda yoki bazada tekshirish **yo'q**. Har bir
> "Kutilgan" natija inspektor ekranidan o'qiladi: xabar matni, status, hisoblagich,
> tugmaning holati. Shuning uchun testni QA o'zi, hech kimdan so'ramasdan yopa oladi.

Inspektor kun davomida haqiqatan duch keladigan holatlar. Avval **online sotuv**, keyin
**offline sotuv**, keyin **ikkisining chegarasi**, oxirida **edge keyslar** — kam uchraydigan,
lekin yiqilsa qimmatga tushadigan holatlar.

**Qisqa tekshiruv** (har build uchun, 25 daqiqa): [`SMOKE-TEST-RGS-SALE.md`](SMOKE-TEST-RGS-SALE.md).
**Offline mexanikasi chuqur** (dvigatel, navbat, imzo): [`OFFLINE-SALE-TEST-CASES.md`](OFFLINE-SALE-TEST-CASES.md).
**Nima nima uchun shunday qilingan:** [`OFFLINE-SALE.md`](OFFLINE-SALE.md), [`SALE-FLOW.md`](SALE-FLOW.md).

Hujjat **2026-09-22** da qayta tekshirildi: xabar matnlari, chegaralar va vaqtlar
ilovadagi haqiqiy qiymatlarga solishtirildi.

---

## Keys formati

Har bir keys to'rt qismdan iborat:

| Qism | Nima yoziladi |
|---|---|
| **Holat** | Real vaziyat — testni boshlashdan oldin qurilma qanday turgan bo'lishi kerak |
| **Qadamlar** | Raqamlangan amallar. Faqat bosiladigan/kiritiladigan narsa |
| **Kutilgan** | Ekranda ko'rinishi shart bo'lgan natija. Har bir nuqta alohida tekshiriladi |

**Ustuvorlik:**

| Belgi | Ma'nosi |
|---|---|
| `P0` | Pul yoki ballon yo'qoladi. Prodga chiqishdan oldin **ikkala qurilmada** majburiy |
| `P1` | Inspektor ishlay olmaydi yoki noto'g'ri xabar ko'radi |
| `P2` | Ko'rinish, matn, qulaylik |

**Belgilash:** ☐ sinalmagan · ✅ o'tdi · ❌ yiqildi · ➖ bu qurilmaga tegishli emas

**Ballon narxi** quyida `NARX` deb yuritiladi.

---

## Ikkita qurilma

1–4-bo'limlar **har bir qurilmada alohida** o'tkaziladi. Ikkitasi ataylab bir-biriga
o'xshamasligi kerak — kamchiliklarning yarmi eski/kichik telefonda chiqadi.

**5-bo'lim boshqacha:** u ikkala telefonni **bir vaqtda** ishlatadi va bir marta
o'tkaziladi — bitta hisob yoki bitta ballon ikki telefonda bo'lganda nima bo'lishini
tekshiradi.

| | **Qurilma 1** — eski / kichik | **Qurilma 2** — yangi / katta |
|---|---|---|
| Tavsiya | Android 6–10, ~5", 2–3 GB RAM | Android 13+, ≥6.5", 6 GB+ RAM |
| Model | | |
| Android versiyasi | | |
| Ekran (dyuym / px) | | |
| Build / versionCode | | |
| Tester | | |
| Sana | | |

Ilovaning eng past qo'llab-quvvatlanadigan versiyasi — **Android 6.0 (minSdk 23)**.
Qurilma 1 shunga yaqin bo'lgani ma'qul.

**Tayyorgarlik (ikkala qurilma uchun):** RGS inspektor hisobi, ochiq zayavka, kamida 3 ta
sotilmagan ballon, 2 ta abonent (biri `face_id_free` mahalladan — prefetchga tushadigan).

**5-bo'lim uchun qo'shimcha:** ikkala telefon **bir vaqtda** yonida turishi, bitta zayavka
va bitta ballon ikkalasida ham ko'rinishi kerak. Ikkinchi inspektor hisobi bo'lsa — yanada
yaxshi (TD-01, TD-02 ni ikkala variantda sinash mumkin).

---

# 1. Online sotuv — kundalik holatlar

## 1.1 Oddiy sotuvlar

### ON-01 — Oddiy sotuv, hammasi joyida `P0`

**Holat.** Shaharda, internet bor. Abonent o'zi kelgan, depoziti `NARX` dan ko'p, limiti ochiq.

**Qadamlar.**

1. Dashboard → zayavka → **Amalga oshirish**.
2. Abonent kodi kiritiladi.
3. Ballon ro'yxatdan tanlanadi.
4. JShShIR kiritiladi, narx va muddat to'ldiriladi.
5. **Davom etish** → kamera ochiladi → surat olinadi.

**Kutilgan.**

- Natija ekranida status **"Muvaffaqiyatli"**.
- Chekda to'g'ri ko'rsatiladi: abonent F.I.Sh., ballon kodi, narx, JShShIR, sana va vaqt, inspektor.
- Dashboardga qaytilganda zayavkadagi **sotilmagan ballonlar soni 1 taga kamaydi**.
- O'sha ballon ballonlar ro'yxatida qayta ko'rinmaydi.

### ON-02 — Ballonni qarindoshi olib ketdi `P0`

**Holat.** Abonentning o'zi yo'q, o'g'li kelgan. Uning JShShIRi abonentning `relations[]` ida bor.

**Qadamlar.**

1. Sotuv ekranida JShShIR maydoni bosiladi.
2. **Qarindoshlar ro'yxatidan** o'g'li tanlanadi.
3. Sotuv oxirigacha olib boriladi.

**Kutilgan.**

- Ro'yxatda qarindoshning F.I.Sh. va JShShIRi ko'rinadi.
- Tanlangach JShShIR maydoni **o'sha qarindoshning** raqami bilan to'ladi.
- Natija **"Muvaffaqiyatli"**; chekdagi JShShIR — abonentniki emas, **qarindoshniki**.

### ON-03 — Ro'yxat emas, skaner bilan `P1`

**Holat.** Inspektor ballon ro'yxati ishlamaydigan hududda (region/tuman sozlamasi bo'yicha).

**Qadamlar.**

1. Abonent kartasidan sotuvga o'tiladi.

**Kutilgan.**

- Ro'yxat emas, **kod skaneri** o'zi ochiladi.
- Skaner qilingan kod zayavkadan topiladi, sotuv davom etadi.
- Ro'yxat ishlaydigan hududda esa **ro'yxat** ochiladi — skaner o'z-o'zidan **hech qachon** ochilmaydi.

### ON-04 — NFC plombali ballonni qaytarish bilan `P0`

**Holat.** Abonentda NFC plombali eski ballon bor (`ballon_nfc` to'lgan).

**Qadamlar.**

1. Qaytariladigan ballon maydonini **bo'sh qoldirib** **Davom etish** bosiladi.
2. Keyin qaytariladigan ballon kiritilib, qayta bosiladi.

**Kutilgan.**

- 1-urinishda: **"NFC plombali ballon mavjud. Uni qaytaring!"** — kamera ochilmaydi.
- 2-urinishda: kamera ochiladi, sotuv o'tadi.
- Natija chekida qaytarilgan ballon ko'rinadi.

### ON-05 — Kun davomida ketma-ket 5 ta sotuv `P1`

**Holat.** Oddiy ish kuni, 5 xil abonent, orada ilovadan chiqilmaydi.

**Qadamlar.**

1. Beshta sotuv ketma-ket qilinadi.
2. Har bir sotuvdan keyin **"Yana amalga oshirish"** bosiladi.

**Kutilgan.**

- Har yangi sotuvda maydonlar **bo'sh**: oldingi sotuvning narxi, JShShIRi, ballon kodi ko'chib o'tmaydi.
- Beshta chek ham o'z abonenti va o'z ballon kodi bilan.
- Dashboarddagi sotilmagan ballonlar soni **5 taga** kamaydi.

### ON-23 — Maydonlar to'lmaguncha "Davom etish" o'chiq `P1`

**Holat.** Sotuv ekrani yangi ochilgan, hech narsa to'ldirilmagan.

**Qadamlar.**

1. Hech narsa kiritmasdan **Davom etish** ga qarash.
2. Faqat ballon tanlanadi — yana qarash.
3. JShShIR kiritiladi — yana qarash.
4. Narx va muddat to'ldiriladi.

**Kutilgan.**

- 1–3-qadamlarda tugma **o'chiq** (bosilmaydi, rangi so'lg'in).
- Hamma qiymat to'lgandan keyingina **yonadi**.
- O'chiq tugmani bosish hech narsa qilmaydi — ekran o'zgarmaydi, xato chiqmaydi.

## 1.2 Abonent sabab rad etiladigan holatlar

### ON-06 — Depozit yetmaydi `P0`

**Holat.** Abonent depoziti `NARX` dan kam.

**Qadamlar.**

1. Abonent kodi kiritiladi.

**Kutilgan.**

- **Abonent kartasidayoq** ogohlantirish chiqadi, kartada depozit ko'rinadi.
- Sotuv tugmasi **o'chiq**.
- Inspektor ballon skaner qilib, JShShIR terib, keyin rad javobini olmaydi — rad **birinchi ekranda**.

### ON-07 — Abonent bloklangan `P0`

**Holat.** Abonent statusi `Active` emas.

**Qadamlar.**

1. Abonent kodi kiritiladi.

**Kutilgan.**

- Kartada "abonent faol emas" ogohlantirishi.
- Sotuv tugmasi **o'chiq**.

### ON-08 — Abonent shu oy ballon olgan (limit) `P0`

**Holat.** `avai_qty = 1`, oxirgi sotuv 10 kun oldin, `limit_days = 30`.

**Qadamlar.**

1. Abonent kodi kiritiladi.
2. Ekran ochiq holda 10 soniya kutiladi.

**Kutilgan.**

- Kartada **turg'un satr** (snackbar emas — ekran ochiq turganda doim ko'rinadi):
  **"Abonent oxirgi 30 kun ichida ballon olgan."**
- Ostida **"Keyin berishim mumkin: dd.MM.yyyy"** sanasi.
- Tugma **o'chiq**.

### ON-09 — JShShIR noto'g'ri terildi `P0`

**Holat.** Inspektor JShShIRning bitta raqamini xato terdi.

**Qadamlar.**

1. Xato JShShIR bilan **Davom etish** bosiladi.
2. Xabar o'qiladi, JShShIR to'g'rilanadi, qayta bosiladi.

**Kutilgan.**

- 1-qadamda rad xabari chiqadi va **kamera ochilmaydi**.
- 2-qadamda kamera ochiladi, sotuv o'tadi.

### ON-10 — Ballon bu zayavkada yo'q `P1`

**Holat.** Boshqa zayavkadan qolgan ballon skaner qilindi.

**Qadamlar.**

1. Begona ballon kodi skaner qilinadi yoki qo'lda kiritiladi.

**Kutilgan.**

- Rad xabari chiqadi, ballon maydoni **to'lmaydi**.
- **Davom etish** o'chiqligicha qoladi.

### ON-11 — Bitta ballon ikki marta `P0`

**Holat.** Ballon bugun allaqachon sotilgan (ON-01 dagi ballon).

**Qadamlar.**

1. O'sha ballonni yana sotishga urinish.

**Kutilgan.**

- Ballon ro'yxatda **ko'rinmaydi** (sotilganlar filtrlanadi), yoki
- Kod qo'lda kiritilsa — rad xabari chiqadi.
- **Ikkinchi natija ekrani "Muvaffaqiyatli" bo'lmaydi** — hech qanday holatda.

## 1.3 Server javoblariga ilovaning reaksiyasi

> Bu yerda serverning o'zi tekshirilmaydi — **ilova server javobini qanday ko'rsatishi**
> tekshiriladi. Holatni yasash usullari: [`OFFLINE-SALE-TEST-CASES.md`](OFFLINE-SALE-TEST-CASES.md) §0.

### ON-12 — FaceID xizmati band `P0`

**Holat.** Server oldidagi cheklovchi yopiq ("Xizmat band, 30 soniyadan keyin qaytadan urinib ko'ring").

**Qadamlar.**

1. Surat olinadi va yuboriladi.
2. Natija ekrani o'qiladi.
3. Orqaga chiqishga urinib ko'riladi.

**Kutilgan.**

- Sarlavha **"Xizmatga vaqtincha cheklov"** — **"Amal bajarilmadi"** EMAS.
- Status chipi **"Vaqtincha cheklov"**.
- Ostida **serverning matni aynan** (ilova o'zidan matn yozmaydi).
- **"Qayta urinish mumkin: s : d : s"** sanoq teskari ketadi (eng ko'pi bilan 5 daqiqa).
- Sanoq ketayotganda **orqaga chiqib bo'lmaydi** (tugma ham, tizim orqasi ham ishlamaydi).
- Sanoq tugagach ekran ochiladi.

### ON-13 — Yuz mos kelmadi `P0`

**Holat.** Suratdagi odam abonent ham, qarindoshi ham emas.

**Qadamlar.**

1. Boshqa odamning surati bilan yuboriladi.

**Kutilgan.**

- Sarlavha **"Amal bajarilmadi"**, status chipi **"Bajarilmadi"**.
- Serverning sababi ko'rsatiladi.
- **Sanoq yo'q**, ekrandan **chiqib ketish mumkin**.
- ON-12 dan aniq farq qiladi: biri "kut", biri "yo'q".

### ON-14 — Play Integrity kvotasi tugadi (−8) `P0` · **release build**

**Holat.** Google kvotasi tugagan (kuniga 10 000 / daqiqada 5).

**Qadamlar.**

1. Surat yuboriladi.

**Kutilgan.**

- **Bloklash dialogi chiqmaydi.**
- Sotuv **bir marta** ketadi, natija ekrani ko'rinadi (server nima desa — shu).
- Qayta urinish taklif qilinmaydi.

### ON-15 — Play Integrity boshqa xato `P0` · **release build**

**Holat.** Attestatsiya −8 dan boshqa sabab bilan yiqildi.

**Qadamlar.**

1. Surat yuboriladi.
2. Taklif qilingan **qayta urinish** bosiladi.
3. Yana yiqiladi.

**Kutilgan.**

- **Bloklash dialogi** chiqadi, matni sabab bilan.
- Natija ekrani **ochilmaydi** — sotuv yuborilmaydi (fail-closed).
- Dialog yopilgach ekran **orqaga** qaytadi.

### ON-16 — Server texnik ishda (503 / timeout) `P1`

**Holat.** Realization so'rovi paytida server javob bermadi.

**Qadamlar.**

1. Surat yuboriladi, javob kutiladi.
2. Xatolikdan keyin **"Yana amalga oshirish"** bilan qaytadan urinib ko'riladi.

**Kutilgan.**

- Natija ekranida xatolik ko'rinadi, spinner **osilib qolmaydi**.
- Ballon ro'yxatda **hali ham sotilmagan** ko'rinadi (sotuv o'tmaganini shundan bilish mumkin).
- Qayta urinishda ikkinchi chek **yasalmaydi**.

### ON-17 — Sessiya eskirgan `P1`

**Holat.** Telefon bir necha kun ochilmagan, token eskirgan.

**Qadamlar.**

1. Ilova ochiladi va darhol sotuv boshlanadi.

**Kutilgan.**

- Ilova qayta kirishni so'raydi **yoki** o'zi yangilaydi va davom etadi.
- Sotuv oqimi **yarim holatda qolmaydi**: bo'sh ekran, tugamaydigan spinner, tushunarsiz xato yo'q.

## 1.4 Qurilma va foydalanuvchi xatti-harakati

### ON-18 — Zaif internet (1 tayoqcha) `P1`

**Holat.** Signal bor, tezlik juda past. So'rov 20–40 soniya ketadi.

**Qadamlar.**

1. Surat yuboriladi.
2. Kutish paytida tugma **yana bir necha marta** bosiladi.

**Kutilgan.**

- Spinner va bosqich matnlari ko'rinadi: "Ma'lumotlar tekshirilmoqda" → "Ballon rasmiylashtirilmoqda" → "Serverga yuborilmoqda".
- Qayta bosish **hech narsa qilmaydi** — ikkinchi so'rov ketmaydi, ikkinchi chek chiqmaydi.
- Oxirida bitta natija: muvaffaqiyat yoki xatolik. Ekran osilib qolmaydi.
- **Offline rejimga o'tib ketmaydi** (so'rov ketyapti, faqat sekin).

### ON-19 — Yuborish paytida ekran burildi `P1`

**Holat.** Inspektor telefonni aylantirdi yoki klaviatura ochilib ekran qayta qurildi.

**Qadamlar.**

1. Surat yuboriladi.
2. Javob kelgunicha telefon **gorizontal**ga buriladi, keyin qaytariladi.

**Kutilgan.**

- **Ikkinchi sotuv boshlanmaydi** — ekran ketayotgan so'rovga qayta ulanadi.
- Natija bitta, chek bitta.
- Sanoq/spinner **qaytadan boshlanmaydi**.

### ON-20 — Yuborish paytida qo'ng'iroq keldi `P1`

**Holat.** Realization ketayotganda ilova fonga tushdi.

**Qadamlar.**

1. Surat yuboriladi.
2. Darhol Home bosiladi (yoki qo'ng'iroq qabul qilinadi), 30 soniya kutiladi.
3. Ilovaga qaytiladi.

**Kutilgan.**

- Natija ekranida **yakuniy holat** ko'rinadi (muvaffaqiyat yoki xatolik).
- Sotuv yo'qolmaydi, ikkilanmaydi, ekran bo'sh qolmaydi.

### ON-21 — Kamera: qorong'i yoki quyosh `P2`

**Holat.** Kirish yo'lagi qorong'i, yoki quyosh to'g'ridan tushyapti.

**Qadamlar.**

1. Kamera ochiladi, yuz turli yorug'likda ushlab turiladi.

**Kutilgan.**

- Yuz topilmasa — tushunarli ko'rsatma chiqadi, suratga olish bloklanadi.
- Ko'zlar juda tor bo'lsa ham (keksa abonent) **"Ko'zlaringizni oching"** da qotib qolmaydi.
- Surat olingach oqim normal davom etadi.

### ON-22 — Sotuv o'rtasida orqaga chiqish `P1`

**Holat.** Inspektor kamera ekranidan orqaga qaytdi.

**Qadamlar.**

1. Sotuv ekrani to'ldiriladi, **Davom etish** bosiladi.
2. Kamera ekranida **orqaga** bosiladi.

**Kutilgan.**

- Sotuv boshlanmaydi, natija ekrani ochilmaydi.
- Sotuv ekraniga qaytadi va **JShShIR, narx, muddat, ballon joyida** turadi.

---

# 2. Offline sotuv — kundalik holatlar

## 2.1 Tayyorgarlik va oddiy oqim

### OF-01 — Hududga chiqishdan oldin yuklab olish `P0`

**Holat.** Ertalab ofisda, internet bor. Inspektor qishloqqa chiqmoqchi.

**Qadamlar.**

1. Sinxronizatsiya ekrani ochiladi.
2. **Sinxronizatsiya** bosiladi.

**Kutilgan.**

- Yashil xabar: **"Offline uchun N ta abonent yuklandi"**, N > 0.
- Xabar tugagach ekran normal holatda qoladi.

### OF-02 — Qishloqda sotuv (umuman aloqa yo'q) `P0`

**Holat.** Aviarejim yoqilgan, Wi-Fi ham o'chiq.

**Qadamlar.**

1. Abonent kodi kiritiladi.
2. Sotuvga o'tiladi, ballon skaner qilinadi.
3. JShShIR kiritiladi, **Davom etish**.
4. Surat olinadi.

**Kutilgan.**

- Karta keshdan ochiladi (shartnoma maydonlari bo'sh bo'lishi normal).
- Ro'yxat o'rniga **kod skaneri**; ustida **"Internet yo'q — ballonni skaner orqali tanlang."**
- Kamera **tez** ochiladi — uzoq kutish yo'q (attestatsiya so'ralmaydi).
- Natija: status **"Offline (sinxronizatsiya kutilmoqda)"** (sariq) va
  **"Bu sotuv qurilmada saqlandi. Internet qaytganda avtomatik yuboriladi."**
- Ballon fizik beriladi.

### OF-03 — "Signal bor, internet yo'q" `P0`

**Holat.** Eng ko'p uchraydigan holat: 1–2 tayoqcha, `E` belgisi, lekin hech qayerga chiqmaydi.
(Yasash: mobil internetni o'chirib, Wi-Fi ni internetsiz routerga ulash.)

**Qadamlar.**

1. Abonent kodi kiritiladi va kutiladi.

**Kutilgan.**

- So'rov urinib ko'riladi (spinner), ketmaydi → **karta keshdan ochiladi**.
- Offline sotuv davom etadi.
- Ilova "internet bor" deb turib **qotib qolmaydi**.

### OF-04 — Bir qishloqda 3 ta sotuv `P0`

**Holat.** Aloqasiz hududda uch xil abonentga uchta ballon.

**Qadamlar.**

1. Uchta sotuv ketma-ket qilinadi.
2. Dashboardga qaytiladi.
3. **Offline sotuvlar** kartasi bosiladi.

**Kutilgan.**

- Dashboard kartasida **3** ko'rsatiladi, ostida "Sinxronizatsiya kutilmoqda".
- Navbat ekranida uchta qator, hammasi **"Kutilmoqda"**.
- Uchtasining abonenti, ballon kodi va sanasi **har xil va to'g'ri**.

### OF-05 — Sotuvdan keyin telefon o'chdi `P0`

**Holat.** Batareya tugadi yoki ilova majburan yopildi (sotuv saqlangandan keyin).

**Qadamlar.**

1. Offline sotuv qilinadi.
2. Ilova Recents dan **majburan yopiladi** (yoki telefon o'chirib yoqiladi).
3. Ilova ochiladi, navbat ekraniga kiriladi.

**Kutilgan.**

- Sotuv **joyida**, statusi **"Kutilmoqda"**.
- Tafsilotda **surat ko'rinadi**.
- Dashboard hisoblagichi o'zgarmagan.

## 2.2 Offline'da rad etiladigan holatlar

> Hammasi bitta qoidaga bo'ysunadi: **rad javobi ballon qo'ldan chiqishidan oldin** chiqadi.
> Xabar matni aynan shu ko'rinishda bo'lishi kerak.

### OF-06 — Depozit yetmaydi (offline) `P0`

**Holat.** Keshdagi depozit `NARX` dan kam. Aloqa yo'q.

**Qadamlar.**

1. Sotuv ekrani to'ldiriladi, **Davom etish** bosiladi.

**Kutilgan.**

- **"Depozit yetarli emas."**
- **Kamera ochilmaydi**, navbatga hech narsa qo'shilmaydi.

### OF-07 — Abonent shu oy olgan (offline) `P0`

**Holat.** Keshdagi `last_real` 10 kun oldin, `limit_days = 30`.

**Kutilgan.**

- **"Abonent oxirgi 30 kun ichida ballon olgan."** (kun soni abonentning `limit_days` iga mos).
- Kamera ochilmaydi.

### OF-08 — Bugun offline sotib bo'lingan abonent `P0`

**Holat.** Ertalab shu abonentga offline sotildi (hali yuborilmagan), tushdan keyin yana so'rayapti.

**Qadamlar.**

1. Aloqa yo'q holatda o'sha abonentga yana sotishga urinish.

**Kutilgan.**

- **"Abonent oxirgi 30 kun ichida ballon olgan."** — navbatdagi sotuv ham limitga kiradi.
- Kamera ochilmaydi.

### OF-09 — Abonent offline ro'yxatda yo'q `P0`

**Holat.** Abonent `face_id_free` bo'lmagan mahalladan — unga offline umuman ruxsat yo'q.

**Qadamlar.**

1. Aloqa yo'q holatda shu abonent bilan sotuvga urinish.

**Kutilgan.**

- Oddiy tarmoq xatosi: **"Tarmoq xatosi"**.
- Navbat, offline rejim, "ro'yxatni yangilang" — **hech narsa aytilmaydi**, chunki taklif
  qiladigan narsa yo'q.

### OF-10 — Ballon boshqa zayavkadan `P1`

**Kutilgan.** **"Bu ballon shu zayavkaga biriktirilmagan."**, kamera ochilmaydi.

### OF-11 — Ballon allaqachon berilgan `P0`

**Holat.** Shu ballon bugun offline sotilgan.

**Kutilgan.** **"Bu ballon allaqachon sotilgan."**, kamera ochilmaydi.

### OF-12 — JShShIR mos kelmadi (offline) `P0`

**Holat.** Kelgan odamning JShShIRi abonentniki ham, qarindoshiniki ham emas.

**Qadamlar.**

1. Begona JShShIR bilan **Davom etish**.
2. Keyin qarindoshning JShShIRi bilan qaytadan.

**Kutilgan.**

- 1-qadamda: **"JSHSHIR abonentga mos kelmadi."**
- 2-qadamda: kamera ochiladi, sotuv o'tadi.

### OF-21 — Abonent bloklangan (offline) `P0`

**Holat.** Keshdagi abonent statusi `Active` emas. Aloqa yo'q.

**Kutilgan.** **"Abonent faol emas."**, kamera ochilmaydi.

### OF-22 — Abonentda kvota yo'q (`avai_qty = 0`) `P0`

**Holat.** Abonentning kvotasi nol (`avai_qty = 0`) — davr ichida ballon umuman ruxsat emas.

**Kutilgan.**

- **"Abonent uchun ballon limiti yo'q."**
- Bu OF-07 dan farq qiladi: bu yerda **kutadigan sana yo'q**, matnda kun soni aytilmaydi.

## 2.3 Internet qaytgach

### OF-13 — Shaharga qaytdi, sinxronizatsiya `P0`

**Holat.** Navbatda 3 ta sotuv, internet qaytdi.

**Qadamlar.**

1. Sinxronizatsiya ekrani → **Sinxronizatsiya**.
2. Navbat ekraniga kiriladi.
3. Dashboardga qaytiladi.

**Kutilgan.**

- Xabar: **"Offline sotuvlar yuborildi: 3 ta"**.
- Navbatda uchtasi ham **"Yuborildi"**, **"Sotilgan"** filtrida ko'rinadi.
- Dashboard hisoblagichi **0** — karta yo'qoladi yoki nol ko'rsatadi.

### OF-14 — Avtomatik ketdi `P1`

**Holat.** Navbatda sotuv bor, internet qaytgan. Inspektor hech narsa bosmaydi.

**Qadamlar.**

1. Faqat Sinxronizatsiya ekrani ochiladi va kutiladi.

**Kutilgan.**

- Navbat **o'zi** yuboriladi, natija xabari chiqadi.
- Tugma bosish shart emas.

### OF-15 — Yo'lda internet kelib-ketib turadi `P1`

**Holat.** Mashinada ketyapti. 3 tadan 1 tasi ketdi, keyin aloqa uzildi.

**Kutilgan.**

- Ketgani **"Yuborildi"**, qolgan 2 tasi **"Kutilmoqda"**.
- Dashboard hisoblagichi **2** ga tushadi (0 ga emas).
- Keyingi ulanishda qolgani ketadi. Yarim status yo'q, ikkilangan qator yo'q.

### OF-16 — Bittasi rad etildi `P0`

**Holat.** 3 tadan 2 tasi o'tdi, bittasini server rad etdi.

**Kutilgan.**

- Xabar rad etilganini **ustun qo'yadi**: **"1 ta offline sotuv yuborilmadi — ro'yxatni tekshiring"**
  (muvaffaqiyat xabari emas).
- Navbatda: 2 ta **"Yuborildi"**, 1 ta **"Muvaffaqiyatsiz"**.
- Dashboard kartasi ostida **"Yuborilmadi — tekshiring"**.
- Rad etilganning tafsilotida **surati saqlanib qolgan**.

### OF-17 — Rad etilganni qayta yuborish `P1`

**Holat.** Rad sababi vaqtinchalik bo'lgan.

**Qadamlar.**

1. Navbat → **Muvaffaqiyatsiz** filtri → sotuv ochiladi.
2. **Qayta yuborish** bosiladi.
3. Tasdiq dialogida **Ha**.

**Kutilgan.**

- Tasdiq dialogi chiqadi: **"Sotuv navbatga qaytariladi va serverga yana bir marta yuboriladi. Davom etasizmi?"**
- Tasdiqlangach status **"Kutilmoqda"** ga o'tadi va darhol yuboriladi.
- O'tsa — **"Yuborildi"**. Yana rad etilsa — **yangi** sabab matni bilan (eskisi qolib ketmaydi).

### OF-18 — Xatolik sababini ko'rish `P1`

**Holat.** Inspektor "nega ketmadi?" deb so'rayapti.

**Qadamlar.**

1. Navbat → **Muvaffaqiyatsiz** filtri → sotuvni ochish.

**Kutilgan.** Tafsilotda hammasi bor:

- **Serverning matni aynan** (ilova o'zidan qayta yozmagan).
- Xatolik turi: **"Server rad etdi"** / **"Muddati o'tgan (7 kun)"** / **"Yuborish kutilmoqda"**.
- Abonent kodi, JShShIR, ballon kodi, sana va vaqt, summa.
- **"Urinishlar: N"**.
- Sotuv surati.

### OF-19 — Smena topshirildi `P0`

**Holat.** Inspektor A navbatida 2 ta yuborilmagan sotuv bilan chiqdi; telefonni inspektor B oldi.

**Qadamlar.**

1. A chiqadi (logout), B kiradi.
2. B navbat ekraniga qaraydi va Sinxronizatsiya bosadi.
3. B chiqadi, A qaytib kiradi va Sinxronizatsiya bosadi.

**Kutilgan.**

- B da navbat **bo'sh** — A ning sotuvlari ko'rinmaydi, dashboard hisoblagichi **0**.
- B yuborishga urinsa ham A ning sotuvi ketmaydi
  (**"Bu sotuv boshqa inspektorga tegishli — u o'z hisobi bilan kirganda yuboriladi."**).
- B da offline ro'yxat ham bo'sh — B o'zinikini yuklab olishi kerak.
- A qaytib kirganda **2 tasi joyida** va yuboriladi.

### OF-20 — Ertasi kuni `P1`

**Holat.** Kecha yuklangan kesh bilan bugun yana qishloqqa chiqmoqchi.

**Qadamlar.**

1. Ertalab **Sinxronizatsiya** bosiladi.

**Kutilgan.**

- Ro'yxat qayta yuklanadi: **"Offline uchun N ta abonent yuklandi"** (depozit va limitlar yangilanadi).
- Bosilmasa va kesh 24 soatdan oshsa — sotuv rad etiladi (EG-07).

### OF-23 — Bitta sotuvni alohida yuborish `P1`

**Holat.** Navbatda bir nechta sotuv bor, inspektor faqat bittasini yubormoqchi.

**Qadamlar.**

1. Navbat → sotuvni ochish.
2. **Shu sotuvni yuborish** bosiladi.

**Kutilgan.**

- Faqat **o'sha** sotuv yuboriladi: **"Sotuv yuborildi."**
- Qolganlari **"Kutilmoqda"** da qoladi, hisoblagich 1 taga kamayadi.
- Umumiy sinxronizatsiya ketayotgan bo'lsa — **"Sotuvlar hozir yuborilmoqda — birozdan keyin qayta urinib ko'ring."**

### OF-24 — Bo'sh navbatda Sinxronizatsiya `P2`

**Holat.** Navbat bo'sh, internet bor.

**Qadamlar.**

1. Sinxronizatsiya ekrani → **Sinxronizatsiya**.

**Kutilgan.**

- **"Yuboriladigan sotuv yo'q."** — xato emas, oddiy xabar.
- Ekran o'zgarmaydi, hech narsa yiqilmaydi.

---

# 3. Online ↔ offline chegarasi

### MX-01 — Sotuv o'rtasida internet uzildi `P0`

**Holat.** Karta online ochildi, ballon tanlandi. **Davom etish** bosilgan payt aloqa yo'qoldi.

**Qadamlar.**

1. Sotuv ekrani to'ldiriladi.
2. **Davom etish** bosilishidan bir soniya oldin aviarejim yoqiladi.

**Kutilgan.**

- Oqim uzilmaydi: offline gate ishlaydi, o'tsa **kamera offline rejimda** ochiladi.
- Natija **"Offline (sinxronizatsiya kutilmoqda)"**.
- Gate rad etsa — o'z matni chiqadi (OF-06…OF-12).

### MX-02 — Surat olingandan keyin internet uzildi `P0`

**Holat.** Online sotuv: challenge olingan, surat olingan, realization ketayotganda aloqa yo'qoldi.

**Kutilgan.**

- Natija ekranida **xatolik**.
- Sotuv **navbatga tushmaydi** — bu online oqim, dashboarddagi offline hisoblagich **o'zgarmaydi**.
- Ballon ro'yxatda **sotilmagan** bo'lib qoladi — qaytadan sotish mumkin.

**Izoh — inspektorga qoida:** natija ekranida **"Muvaffaqiyatli"** yoki
**"Offline (sinxronizatsiya kutilmoqda)"** chiqmaguncha **ballonni bermaslik**.

### MX-03 — Navbat bor, lekin inspektor yangi sotuv qilmoqda `P0`

**Holat.** Navbatda yuborilmagan sotuvlar bor; shu payt kamera ochiq.

**Qadamlar.**

1. Navbatda sotuv bor holatda yangi sotuv boshlanadi, kamerada turiladi.
2. Shu paytda internet qaytariladi.
3. Kamerada 1 daqiqa turiladi, keyin sotuv tugatiladi.

**Kutilgan.**

- Kamera ochiq turganda navbat **yuborilmaydi** (hisoblagich o'zgarmaydi).
- Sotuv tugagach navbat odatdagidek ketadi.

### MX-04 — Sinxronizatsiya ketayotganda sotuv boshlandi `P1`

**Holat.** Navbat yuborilyapti, inspektor shu payt yangi sotuvni boshladi.

**Kutilgan.**

- Ketayotgan sotuv oxirigacha yuboriladi.
- Yangi sotuv normal ketadi (kamera ochiladi, kutish yo'q).
- Qolgan navbat **keyin** yuboriladi.

### MX-05 — Offline sotuvdan keyin darhol online sotuv `P1`

**Holat.** Bir abonentga offline sotildi, keyin aloqa qaytdi va boshqa abonentga online sotilmoqda.

**Kutilgan.**

- Online sotuv normal ketadi, natija **"Muvaffaqiyatli"**.
- Maydonlar aralashmaydi: yangi chekda oldingi abonentning JShShIRi yoki narxi yo'q.
- Offline navbatdagi sotuv o'z vaqtida yuboriladi.

### MX-06 — Aloqa bor, lekin server o'zi rad etdi `P0`

**Holat.** Internet ishlayapti, server abonentni rad etdi (404, bloklangan, hash xato).

**Kutilgan.**

- **Offline rejimga o'tilmaydi** — keshdan karta ochilmaydi.
- Serverning javobi ko'rsatiladi.
- Aks holda API xostini bloklay olgan har kim online tekshiruvlarni chetlab o'tardi.

---

# 4. Edge keyslar

Kam uchraydi, lekin yiqilsa qimmat. Ko'pchiligi qo'lda holat yasashni talab qiladi (SQL yoki
sozlamalar) — usullari [`OFFLINE-SALE-TEST-CASES.md`](OFFLINE-SALE-TEST-CASES.md) §0 da.

## 4.1 Chegaraviy qiymatlar

### EG-01 — Depozit aynan narxga teng `P0`

**Holat.** `deposit = NARX` (masalan aynan 40 000.00).

**Kutilgan.**

- Sotuv **o'tadi**.
- `NARX − 0.01` bo'lsa — **rad** ("Depozit yetarli emas.").
- Online va offline **bir xil** javob beradi.

### EG-02 — Oxirgi sotuv aynan 30 kun oldin `P0`

**Holat.** `last_real` bugundan roppa-rosa `limit_days` kun oldin.

**Kutilgan.**

- **30 kun → sotuv o'tadi** (oyna tashqarisi).
- **29 kun → rad**.
- Abonent kartasi va offline gate **bir xil** javob beradi — aks holda karta ruxsat berib,
  gate to'rt ekran keyin rad etadi.

### EG-03 — `avai_qty = 2` `P1`

**Holat.** Abonentga davr ichida 2 ta ballon ruxsat.

**Kutilgan.**

- Birinchi va ikkinchi sotuv o'tadi, **uchinchisi rad** etiladi.
- "Keyin berishim mumkin" sanasi **ikkinchi** (kvota-inchi) sotuvdan hisoblanadi, eng eskisidan emas.

### EG-04 — Nol bilan boshlanadigan ballon kodi `P1`

**Holat.** Kod `0123456` ko'rinishida.

**Kutilgan.**

- Ballon topiladi va sotiladi.
- Topilmasa — bu ma'lum tuzoq, kamchilik sifatida yozib qo'ying.

### EG-05 — JShShIR maydoni: bo'sh, qisqa, harfli `P2`

**Qadamlar.**

1. Maydon bo'sh qoldiriladi.
2. 5 ta raqam kiritiladi.
3. Harf va belgi kiritishga urinib ko'riladi.
4. 14 tadan ko'p raqam kiritishga urinib ko'riladi.

**Kutilgan.**

- Bo'sh — **Davom etish** o'chiq.
- 14 tadan kam — rad xabari, kamera ochilmaydi.
- Harf va belgi **kiritilmaydi** (klaviatura faqat raqam).
- 14 tadan ortiq raqam kiritilmaydi.
- Ilova hech bir holatda yiqilmaydi.

### EG-06 — Juda uzun ism / manzil `P2`

**Holat.** Abonent ismi yoki mahalla nomi juda uzun.

**Kutilgan.**

- Karta va chek buzilmaydi — matn **qirqiladi yoki o'raladi**.
- Tugmalar ekrandan chiqib ketmaydi, boshqa matn ustiga chiqmaydi.
- **Qurilma 1 da (kichik ekran) alohida tekshiriladi.**

## 4.2 Vaqt va kesh

### EG-07 — Kesh 24 soatdan eski `P0`

**Kutilgan.**

- **"Offline ro'yxat eskirgan (24 soatdan ortiq). Internet bor joyda yangilang."**
- Sotuv boshlanmaydi, kamera ochilmaydi.
- Kesh 23 soat bo'lsa — o'tadi.

### EG-08 — Navbatdagi sotuv 7 kundan oshdi `P0`

**Holat.** Inspektor bir hafta aloqasiz hududda qolib ketdi.

**Kutilgan.**

- Sotuv turi **"Muddati o'tgan (7 kun)"** bo'ladi.
- Serverga **yuborilmaydi**.
- Tafsilotda **"Qayta yuborish" tugmasi yo'q**.
- Surati saqlanadi — bu qo'lda hal qilinadigan holat.

### EG-09 — Qurilma soati noto'g'ri `P1`

**Holat.** Telefon soati 10 daqiqa oldinda (yoki 2 kun orqada).

**Kutilgan.**

- Sotuv baribir qabul qilinadi va navbatga tushadi.
- **Ochiq kamchilik:** soat juda katta farq qilsa (8 kun orqada) sotuv "Muddati o'tgan"
  bo'lib qolishi mumkin — shuni tekshirib, chiqsa yozib qo'yish.

### EG-10 — Kesh yangilanayotganda aloqa uzildi `P0`

**Holat.** Prefetch yarmida aviarejim yoqildi.

**Kutilgan.**

- **Eski to'liq ro'yxat joyida qoladi** — darhol offline sotish mumkin.
- Ro'yxat bo'shab qolmaydi ("Bu abonent offline ro'yxatda yo'q" chiqmaydi).

## 4.3 Navbat va yuborish

### EG-11 — `duplicate` javobi `P0`

**Holat.** Sotuv serverga yetib borgan, lekin javobi yo'lda yo'qolgan; ilova qayta yubordi.

**Kutilgan.**

- Sotuv **"Yuborildi"** bo'ladi (Muvaffaqiyatsiz emas).
- Navbatda **ikkinchi qator yaratilmaydi**, cheksiz urinish yo'q.

### EG-12 — `OFFLINE_IN_PROGRESS` `P0`

**Holat.** Shu `offline_id` bo'yicha serverda parallel so'rov ketyapti.

**Kutilgan.**

- Sotuv **"Kutilmoqda"** qoladi (rad emas).
- Xabar: **"N ta sotuv navbatda. Server kutishni so'radi — birozdan keyin qayta urinib ko'ring."**
- 150 soniya ichida "Yuborish" bosilsa — qayta yuborilmaydi.

### EG-13 — FaceID cheklovchisi navbatda `P0`

**Holat.** Navbat yuborilayotganda cheklovchi yopildi.

**Kutilgan.**

- Sotuv **"Kutilmoqda"** qoladi va kamida 15 soniyadan keyin qayta urinadi.
- **"Muvaffaqiyatsiz" bo'lishi — jiddiy xato**: ballon berilgan sotuv cheklovchi tufayli o'ladi.

### EG-14 — "Yuborish" ni ikki marta bosish `P0`

**Holat.** Inspektor sabrsizlik bilan tugmani ikki marta bosdi, yoki navbat ekranidan ham,
tafsilotdan ham bosdi.

**Kutilgan.**

- **Bitta** yuborish ketadi.
- Ikkinchi bosish mavjudiga qo'shiladi yoki **"Sotuvlar hozir yuborilmoqda…"** deydi.
- Hisoblagich manfiyga tushmaydi, qator ikkilanmaydi.

### EG-15 — Yuborish o'rtasida ilova yopildi `P0`

**Qadamlar.**

1. Sinxronizatsiya boshlanadi.
2. Yuborilayotganda ilova Recents dan majburan yopiladi.
3. Ilova qayta ochiladi, Sinxronizatsiya bosiladi.

**Kutilgan.**

- "Yuborilmoqda" holatida qolgan sotuv **qayta yuboriladi**.
- Sotuv yo'qolmaydi, navbatda ikkilanmaydi.

### EG-16 — Navbat 200 taga yetdi `P1`

**Kutilgan.**

- **"Yuborilmagan sotuvlar juda ko'p (200). Internetga ulanib, navbatni yuboring."**
- Yangi offline sotuv boshlanmaydi.
- Lekin **navbatni yuborish ishlaydi** — inspektor qulflanib qolmaydi.

### EG-17 — Sotuv surati o'chirilgan `P1`

**Holat.** Fayl qandaydir sabab bilan yo'qolgan.

**Kutilgan.**

- Sotuv **"Muvaffaqiyatsiz"**, sababi: surat topilmagani.
- Cheksiz urinish yo'q.

### EG-18 — Telefon xotirasi to'la `P0`

**Holat.** Xotira tugagan, surat nusxalanmaydi.

**Kutilgan.**

- Natija ekranida **xatolik** (muvaffaqiyat ham, "offline saqlandi" ham emas) — inspektor
  ballonni bermaydi.
- Navbatga **yarim qator yozilmaydi**: hisoblagich oshmaydi.

## 4.4 Xavfsizlik va tizim

### EG-19 — MITM / sertifikat mos emas `P0`

**Holat.** Proksi yoki sertifikat almashtirilgan tarmoq.

**Kutilgan.**

- Sertifikat xatosi ko'rsatiladi.
- **Offline rejim ochilmaydi** — keshdan karta ochilmaydi, gate ishga tushmaydi.

### EG-20 — Mehmonxona/kafe Wi-Fi (captive portal) `P1`

**Kutilgan.** Offline ochilmaydi; oddiy xatolik ko'rsatiladi.

### EG-21 — Ilova yangilanishi `P0`

**Holat.** Navbatda yuborilmagan sotuvlar bilan yangi APK o'rnatildi.

**Qadamlar.**

1. Navbatda 2 ta sotuv qoldiriladi.
2. Ustiga yangi APK o'rnatiladi (o'chirmasdan).
3. Ilova ochiladi, navbat ekraniga kiriladi.

**Kutilgan.**

- Navbat va suratlar **joyida**, dashboard hisoblagichi **o'zgarmagan**.

### EG-22 — Ilova keshini tozalash `P0`

**Qadamlar.**

1. Navbatda sotuv qoldiriladi.
2. Settings → Apps → E-Gaz → **Clear cache**.
3. Ilova ochiladi.

**Kutilgan.**

- Navbatdagi sotuvlar va **suratlari saqlanadi**.
- `Clear data` esa hammasini o'chiradi — bu kutilgan, kamchilik emas.

## 4.5 Qurilma va ko'rinish

> Bu bo'lim **ikkala qurilmada ham** to'liq o'tkaziladi — aynan shu yerda ikkita qurilma
> farq qiladi.

### EG-23 — Tilni almashtirish (uz ↔ ru) `P2`

**Qadamlar.**

1. Ilova tili rus tiliga o'tkaziladi.
2. Sotuv oqimi, natija ekrani, offline navbat va tafsilot ochiladi.
3. Offline gate rad javobi chiqariladi (masalan OF-06).

**Kutilgan.**

- Sotuv, natija va navbat ekranlaridagi matnlar **tarjima qilingan**.
- `%1$d` kabi o'rin egalari ikkala tilda to'g'ri to'ladi (masalan "Urinishlar: 2").
- Matn tugmadan chiqib ketmaydi, qirqilib qolmaydi.
- **Ma'lum kamchilik:** offline gate xabarlari (OF-06…OF-12, OF-21, OF-22, EG-07, EG-16)
  hozir faqat o'zbekchada — ular tarjima fayllariga kiritilmagan.
  Rus tilida ham o'zbekcha chiqsa — bu **kutilgan**, yangi kamchilik emas.

### EG-24 — Katta shrift `P2`

**Qadamlar.**

1. Settings → Display → **Font size** eng kattaga qo'yiladi
   (va bo'lsa **Display size** ham kattalashtiriladi).
2. Quyidagi ekranlar ochiladi: abonent kartasi, sotuv ekrani, natija ekrani,
   offline navbat, sotuv tafsiloti, sinxronizatsiya.

**Kutilgan.**

- Har bir ekranda **asosiy tugma ko'rinadi va bosiladi** (ekrandan chiqib ketmaydi).
- Natija ekranidagi chek satrlari bir-birining ustiga chiqmaydi.
- Navbatdagi status ("Kutilmoqda" / "Muvaffaqiyatsiz") **to'liq o'qiladi**, qirqilmaydi.
- Uzun xabarlar (EG-07, EG-16) to'liq ko'rinadi yoki aylantirib o'qiladi.

### EG-25 — Qorong'i rejim (dark mode) `P2`

**Qadamlar.**

1. Tizimda qorong'i rejim yoqiladi.
2. Sotuv oqimi boshidan oxirigacha o'tiladi (kamera ekrani ham).
3. Offline navbat va tafsilot ochiladi.

**Kutilgan.**

- Hech qayerda **oq matn oq fonda** yoki qora matn qora fonda qolmaydi.
- Status chiplari (yashil / sariq / qizil) ajralib turadi.
- Kamera ekranidagi ko'rsatma matni o'qiladi.
- **E'tibor bering:** kamera foni va ogohlantirish bloklari uchun qorong'i rejim rangi
  alohida berilmagan — birinchi navbatda aynan shu ikkisi tekshiriladi.

### EG-26 — Gorizontal holat (landscape) `P2`

**Qadamlar.**

1. Har bir asosiy ekranda telefon gorizontalga buriladi: abonent kartasi, sotuv ekrani,
   kamera, natija, navbat, tafsilot.

**Kutilgan.**

- Ekran yiqilmaydi, kontent **aylantirib ko'riladi** (scroll).
- Tugmalar ko'rinmay qolmaydi.
- Kamera ekrani buzilmaydi, yuz ramkasi joyida.
- Burilishda kiritilgan qiymatlar (JShShIR, narx) **yo'qolmaydi**.

### EG-27 — Klaviatura maydonni yopib qo'ymaydi (kichik ekran) `P1`

**Holat.** Qurilma 1 — kichik ekran.

**Qadamlar.**

1. Sotuv ekranida JShShIR maydoni bosiladi — klaviatura ochiladi.
2. Narx va muddat maydonlariga o'tiladi.
3. Klaviatura ochiq holda **Davom etish** ni topishga urinib ko'riladi.

**Kutilgan.**

- Tahrirlanayotgan maydon **klaviatura ostida qolmaydi** — ekran o'ziga tortadi.
- Aylantirib **Davom etish** gacha yetish mumkin.
- Klaviatura yopilganda ekran joyiga qaytadi, maydonlar to'lganicha qoladi.

### EG-28 — Eski va sekin telefon `P1`

**Holat.** Qurilma 1 — Android 6–9, 2–3 GB RAM.

**Qadamlar.**

1. To'liq online sotuv o'tkaziladi.
2. To'liq offline sotuv o'tkaziladi.
3. Navbat ekrani ochiladi va aylantiriladi.

**Kutilgan.**

- Kamera ochiladi va yuzni topadi (sekinroq bo'lishi mumkin — lekin topadi).
- Ballon ro'yxati aylantirilganda qotib qolmaydi.
- Ilova hech qayerda yiqilmaydi va "Application not responding" chiqmaydi.
- Natija ekrani to'liq ko'rinadi.

### EG-29 — Tizim orqaga imo-ishorasi (gesture back) `P1`

**Holat.** Qurilma 2 — imo-ishorali navigatsiya yoqilgan.

**Qadamlar.**

1. Sotuv ekranida chetdan surib orqaga qaytiladi.
2. Kamera ekranida shunday qilinadi.
3. Natija ekranida (muvaffaqiyatli sotuvdan keyin) shunday qilinadi.
4. ON-12 dagi sanoq ketayotganda shunday qilinadi.

**Kutilgan.**

- 1–2: tugma bilan orqaga bosgandek ishlaydi (ON-22 dagi natija).
- 3: natija ekranidan tasodifan chiqib ketib, yarim holatda qolib ketmaydi.
- 4: **chiqib ketmaydi** — cheklov sanog'i tugamaguncha ekran ushlab turiladi.

---

# 5. Ikkita telefon bir vaqtda

Bu bo'limda ikkala telefon **birga** ishlatiladi: bittasi **T1**, ikkinchisi **T2** deb
yuritiladi. Har bir keys **bir marta** o'tkaziladi (har qurilmada alohida emas).

**Nega kerak.** Ballon fizik narsa — u faqat bitta odamga beriladi. Ikki telefon bir
ballonni yoki bir abonentni bir vaqtda "sotib" qo'ysa, ballon berilib ketadi va keyin
tuzatib bo'lmaydi. Shuning uchun bu bo'limdagi deyarli hamma keys `P0`.

**Asosiy qoida — hammasiga tegishli:** **hech qachon ikkala telefonda ham
"Muvaffaqiyatli" chiqmasligi kerak.** Biri o'tsa, ikkinchisi rad javobini ko'rsatadi.
Ikkalasi "Muvaffaqiyatli" bo'lsa — bu darhol yozib qo'yiladigan `P0` kamchilik.

## 5.1 Bitta hisob, ikkita telefon

### TD-01 — Bir inspektor hisobi ikkala telefonda kirilgan `P0`

**Holat.** T1 da inspektor kirgan va ishlab turibdi.

**Qadamlar.**

1. T2 da **o'sha** inspektor hisobi bilan kiriladi.
2. T1 ga qaytiladi va oddiy amal qilinadi (zayavkani ochish, abonent kodini kiritish).
3. T2 da ham o'sha amal qilinadi.

**Kutilgan.** Ikki xil natijaning **bittasi** bo'lishi kerak, uchinchisi emas:

- **Yoki** ikkala telefon ham normal ishlaydi;
- **yoki** T1 chiqarib yuboriladi — serverning matni bilan tushunarli xabar chiqadi va
  kirish ekraniga qaytadi.
- **Bo'lmasligi kerak:** bo'sh ekran, tugamaydigan spinner, "http error" kabi xom xato,
  yoki T1 ning qotib qolishi.
- Natija qaysi bo'lsa — jadvalning **Izoh** ustuniga yozib qo'yiladi.

### TD-02 — T1 da sotuv ketayotganda T2 da kirish `P0`

**Holat.** T1 da surat yuborilgan, natija kutilyapti.

**Qadamlar.**

1. T1 da surat yuboriladi.
2. Javob kelgunicha T2 da o'sha hisob bilan kiriladi.
3. T1 ning natija ekrani kuzatiladi.

**Kutilgan.**

- T1 da **aniq yakun** ko'rinadi: yo "Muvaffaqiyatli", yo tushunarli xatolik.
- Yarim holat yo'q: spinner osilib qolmaydi, ekran bo'shab qolmaydi.
- T1 da "Muvaffaqiyatli" chiqmasa — **ballon berilmaydi**.

## 5.2 Bitta ballon / bitta abonent, ikkala telefon online

### TD-03 — Bir ballonni ikki telefondan sotish `P0`

**Holat.** Ikkala telefonda ham bitta zayavka ochiq, ro'yxatda **X** ballon ko'rinadi.

**Qadamlar.**

1. T1 da X ballon sotiladi — oxirigacha, natija "Muvaffaqiyatli".
2. T2 da (ro'yxat yangilanmagan holda) o'sha X ballon tanlanib, sotuv oxirigacha olib boriladi.

**Kutilgan.**

- T2 da **"Muvaffaqiyatli" chiqmaydi**.
- Rad javobi shu uch joydan birida chiqadi: ballon ro'yxatida yo'q; ballon tanlanganda
  rad xabari; yoki natija ekranida serverning rad javobi.
- Xabar tushunarli — inspektor "ballon allaqachon sotilgan" ekanini tushunadi.
- T2 da ro'yxat yangilangandan keyin X ballon **umuman ko'rinmaydi**.

### TD-04 — Bir abonentga ikki telefondan sotish `P0`

**Holat.** Ikkala telefonda ham o'sha abonentning kartasi ochilgan (depoziti bitta ballonga yetadi).

**Qadamlar.**

1. T1 da abonentga sotuv qilinadi — "Muvaffaqiyatli".
2. T2 da (karta yangilanmagan, eski ma'lumot bilan) **boshqa** ballon bilan sotuvga urinish.

**Kutilgan.**

- T2 da **"Muvaffaqiyatli" chiqmaydi** — limit yoki depozit sababli rad etiladi.
- T2 da karta qayta ochilsa — **"Abonent oxirgi 30 kun ichida ballon olgan."** va
  "Keyin berishim mumkin" sanasi ko'rinadi, tugma o'chiq.

### TD-05 — Deyarli bir vaqtda bosish (poyga) `P0`

**Holat.** Ikkala telefon ham bitta X ballon bilan sotuv ekranida turibdi, hamma maydon to'lgan.

**Qadamlar.**

1. Ikkala telefonda **Davom etish** 1–2 soniya farq bilan bosiladi.
2. Ikkala telefonda ham surat olinadi va yuboriladi.

**Kutilgan.**

- **Aniq bittasida** "Muvaffaqiyatli", ikkinchisida xatolik.
- Ikkalasida ham muvaffaqiyat chiqsa — bu eng jiddiy kamchilik, darhol yozib qo'yiladi
  (ballon kimga berilgani, chek raqamlari bilan).
- Xatolik chiqqan telefonda ekran yiqilmaydi, ballon **berilmaydi**.

## 5.3 Ikkala telefon offline

### TD-06 — Ikkala telefon offline, bitta ballon `P0`

**Holat.** Ikkala telefonda ham offline ro'yxat yuklangan, ikkalasi ham aviarejimda.

**Qadamlar.**

1. T1 da X ballon offline sotiladi — "Offline (sinxronizatsiya kutilmoqda)".
2. T2 da **o'sha X ballon** offline sotiladi — u ham saqlanadi.
3. Avval T1 internetga ulanib sinxronizatsiya qilinadi.
4. Keyin T2 sinxronizatsiya qilinadi.

**Kutilgan.**

- 2-qadamda T2 sotuvni **qabul qiladi** — u T1 ni ko'rmaydi, bu kutilgan
  (shuning uchun bir ballonni ikki inspektorga bermaslik ish tartibi bilan hal qilinadi).
- 3-qadamda T1 da: **"Offline sotuvlar yuborildi: 1 ta"**, status **"Yuborildi"**.
- 4-qadamda T2 da: xabar **"1 ta offline sotuv yuborilmadi — ro'yxatni tekshiring"**,
  status **"Muvaffaqiyatsiz"**.
- T2 da tafsilot ochilsa: **serverning matni aynan**, xatolik turi **"Server rad etdi"**,
  va **surat saqlanib qolgan**.
- T2 dagi sotuv **"Yuborildi" bo'lib ketmaydi**.

### TD-07 — Ikkala telefon offline, bitta abonent `P0`

**Holat.** Yuqoridagidek, lekin har bir telefon **boshqa** ballon bilan, **bitta** abonentga.

**Qadamlar.**

1. T1 da abonentga offline sotiladi.
2. T2 da o'sha abonentga boshqa ballon bilan offline sotiladi.
3. T1, keyin T2 sinxronizatsiya qilinadi.

**Kutilgan.**

- T1 — **"Yuborildi"**.
- T2 — **"Muvaffaqiyatsiz"**, sababi serverning o'z matni bilan (limit).
- T2 da ham surat saqlanadi, **"Qayta yuborish"** tugmasi bor.

### TD-08 — T1 online sotdi, T2 offline navbatda `P0`

**Holat.** Eng ko'p uchraydigan aralash holat: T1 shaharda, T2 qishloqda.

**Qadamlar.**

1. T2 aviarejimga o'tkaziladi, unda X ballon offline sotiladi.
2. T1 da (internet bor) o'sha X ballon sotiladi — "Muvaffaqiyatli".
3. T2 internetga ulanib sinxronizatsiya qilinadi.

**Kutilgan.**

- T2 dagi sotuv **"Muvaffaqiyatsiz"** bo'ladi, serverning matni ko'rsatiladi.
- T2 da dashboard hisoblagichi 0 ga tushmaydi — karta ostida **"Yuborilmadi — tekshiring"**.
- Sotuv jimgina yo'qolib qolmaydi va "Yuborildi" bo'lib ketmaydi.

### TD-09 — Ikkala telefon bir vaqtda sinxronizatsiya `P1`

**Holat.** Har ikkalasida 2 tadan yuborilmagan sotuv bor (har xil ballonlar), bir hisob.

**Qadamlar.**

1. Ikkala telefonda **Sinxronizatsiya** deyarli bir vaqtda bosiladi.

**Kutilgan.**

- Har bir telefon **faqat o'z navbatini** yuboradi: **"Offline sotuvlar yuborildi: 2 ta"**.
- Hech bir telefonda ikkinchisining sotuvi **ko'rinmaydi**.
- Ikkala hisoblagich ham **0** ga tushadi.
- Ikkilangan qator, "sabab yo'q" xatolik yoki manfiy hisoblagich chiqmaydi.

## 5.4 Xizmat cheklovi va navbatning joyi

### TD-10 — FaceID cheklovi bir telefonda yoqildi `P1`

**Holat.** T1 da FaceID cheklovi ishga tushdi (ON-12).

**Qadamlar.**

1. T1 da cheklov chiqariladi — **"Xizmatga vaqtincha cheklov"**.
2. Darhol T2 da o'sha hisob bilan sotuvga urinib ko'riladi.

**Kutilgan.**

- T2 da ham cheklov chiqsa — **"Xizmatga vaqtincha cheklov"** va sanoq ko'rsatiladi.
- T2 da **"Yuz mos kelmadi"** deb ko'rsatilmaydi — cheklov rad javobi emas.
- T2 normal ishlasa ham mayli; muhimi — cheklov hech qachon "yuz mos kelmadi" bo'lib
  ko'rinmasligi.

### TD-11 — Yuborilmagan navbat hisob bilan ko'chib yurmaydi `P0`

**Holat.** T1 da 2 ta yuborilmagan offline sotuv bor.

**Qadamlar.**

1. T2 da o'sha inspektor hisobi bilan kiriladi.
2. T2 da dashboard va navbat ekrani ochiladi, Sinxronizatsiya bosiladi.
3. T1 ga qaytib, chiqishga (logout) urinib ko'riladi.

**Kutilgan.**

- T2 da navbat **bo'sh**, hisoblagich **0** — navbat telefonda qoladi, hisob bilan ko'chmaydi.
- T2 da Sinxronizatsiya **"Yuboriladigan sotuv yo'q."** deydi.
- T1 da chiqish dialogida **yuborilmagan sotuvlar soni** (2) ogohlantirish sifatida ko'rinadi.
- T1 dagi 2 ta sotuv chiqib-kirgandan keyin ham joyida.

---

# 6. Belgilash jadvali

☐ sinalmagan · ✅ o'tdi · ❌ yiqildi · ➖ tegishli emas

**Qurilma 1:** ________________  **Qurilma 2:** ________________
**Build / versionCode:** ________  **Sana:** ________  **Tester:** ________

| # | Keys | Prio | Qurilma 1 | Qurilma 2 | Izoh |
|---|---|:---:|:---:|:---:|---|
| ON-01 | Oddiy sotuv | P0 | [ ] | [ ] | |
| ON-02 | Qarindosh JShShIRi | P0 | [] | ☐ | |
| ON-03 | Skaner bilan | P1 | ☐ | ☐ | |
| ON-04 | NFC plomba qaytarish | P0 | ☐ | ☐ | |
| ON-05 | Ketma-ket 5 ta sotuv | P1 | ☐ | ☐ | |
| ON-23 | CTA to'lmaguncha o'chiq | P1 | ☐ | ☐ | |
| ON-06 | Depozit yetmaydi | P0 | ☐ | ☐ | |
| ON-07 | Abonent bloklangan | P0 | ☐ | ☐ | |
| ON-08 | Limit (30 kun) | P0 | ☐ | ☐ | |
| ON-09 | JShShIR xato terildi | P0 | ☐ | ☐ | |
| ON-10 | Ballon zayavkada yo'q | P1 | ☐ | ☐ | |
| ON-11 | Ballon ikki marta | P0 | ☐ | ☐ | |
| ON-12 | FaceID band | P0 | ☐ | ☐ | |
| ON-13 | Yuz mos kelmadi | P0 | ☐ | ☐ | |
| ON-14 | Play Integrity −8 | P0 | ☐ | ☐ | |
| ON-15 | Integrity boshqa xato | P0 | ☐ | ☐ | |
| ON-16 | Server 503 / timeout | P1 | ☐ | ☐ | |
| ON-17 | Sessiya eskirgan | P1 | ☐ | ☐ | |
| ON-18 | Zaif internet | P1 | ☐ | ☐ | |
| ON-19 | Ekran burildi | P1 | ☐ | ☐ | |
| ON-20 | Qo'ng'iroq keldi | P1 | ☐ | ☐ | |
| ON-21 | Qorong'i / quyosh | P2 | ☐ | ☐ | |
| ON-22 | O'rtada orqaga chiqish | P1 | ☐ | ☐ | |
| OF-01 | Chiqishdan oldin yuklash | P0 | ☐ | ☐ | |
| OF-02 | Qishloqda sotuv | P0 | ☐ | ☐ | |
| OF-03 | Signal bor, internet yo'q | P0 | ☐ | ☐ | |
| OF-04 | Uchta sotuv | P0 | ☐ | ☐ | |
| OF-05 | Telefon o'chdi | P0 | ☐ | ☐ | |
| OF-06 | Depozit yetmaydi | P0 | ☐ | ☐ | |
| OF-07 | Limit | P0 | ☐ | ☐ | |
| OF-08 | Bugun olgan abonent | P0 | ☐ | ☐ | |
| OF-09 | Ro'yxatda yo'q | P0 | ☐ | ☐ | |
| OF-10 | Ballon boshqa zayavkadan | P1 | ☐ | ☐ | |
| OF-11 | Ballon berilgan | P0 | ☐ | ☐ | |
| OF-12 | JShShIR mos emas | P0 | ☐ | ☐ | |
| OF-21 | Abonent bloklangan (offline) | P0 | ☐ | ☐ | |
| OF-22 | Kvota yo'q (`avai_qty = 0`) | P0 | ☐ | ☐ | |
| OF-13 | Sinxronizatsiya | P0 | ☐ | ☐ | |
| OF-14 | Avtomatik yuborish | P1 | ☐ | ☐ | |
| OF-15 | Aloqa kelib-ketib turadi | P1 | ☐ | ☐ | |
| OF-16 | Bittasi rad etildi | P0 | ☐ | ☐ | |
| OF-17 | Qayta yuborish | P1 | ☐ | ☐ | |
| OF-18 | Sababni ko'rish | P1 | ☐ | ☐ | |
| OF-19 | Smena topshirildi | P0 | ☐ | ☐ | |
| OF-20 | Ertasi kuni | P1 | ☐ | ☐ | |
| OF-23 | Bitta sotuvni yuborish | P1 | ☐ | ☐ | |
| OF-24 | Bo'sh navbatda sinxronizatsiya | P2 | ☐ | ☐ | |
| MX-01 | O'rtada internet uzildi | P0 | ☐ | ☐ | |
| MX-02 | Suratdan keyin uzildi | P0 | ☐ | ☐ | |
| MX-03 | Sotuv paytida navbat | P0 | ☐ | ☐ | |
| MX-04 | Sinxronizatsiya paytida sotuv | P1 | ☐ | ☐ | |
| MX-05 | Offline keyin online | P1 | ☐ | ☐ | |
| MX-06 | Server rad etdi | P0 | ☐ | ☐ | |
| EG-01 | Depozit aynan teng | P0 | ☐ | ☐ | |
| EG-02 | Aynan 30 kun | P0 | ☐ | ☐ | |
| EG-03 | `avai_qty = 2` | P1 | ☐ | ☐ | |
| EG-04 | Nolli ballon kodi | P1 | ☐ | ☐ | |
| EG-05 | JShShIR maydoni | P2 | ☐ | ☐ | |
| EG-06 | Uzun matnlar | P2 | ☐ | ☐ | |
| EG-07 | Kesh 24 soatdan eski | P0 | ☐ | ☐ | |
| EG-08 | 7 kundan oshdi | P0 | ☐ | ☐ | |
| EG-09 | Soat noto'g'ri | P1 | ☐ | ☐ | |
| EG-10 | Prefetch uzildi | P0 | ☐ | ☐ | |
| EG-11 | `duplicate` | P0 | ☐ | ☐ | |
| EG-12 | `OFFLINE_IN_PROGRESS` | P0 | ☐ | ☐ | |
| EG-13 | FaceID cheklovchisi | P0 | ☐ | ☐ | |
| EG-14 | Ikki marta bosish | P0 | ☐ | ☐ | |
| EG-15 | Yuborish o'rtasida yopildi | P0 | ☐ | ☐ | |
| EG-16 | Navbat 200 ta | P1 | ☐ | ☐ | |
| EG-17 | Surat o'chirilgan | P1 | ☐ | ☐ | |
| EG-18 | Xotira to'la | P0 | ☐ | ☐ | |
| EG-19 | MITM / sertifikat | P0 | ☐ | ☐ | |
| EG-20 | Captive portal | P1 | ☐ | ☐ | |
| EG-21 | Ilova yangilanishi | P0 | ☐ | ☐ | |
| EG-22 | Kesh tozalash | P0 | ☐ | ☐ | |
| EG-23 | Til almashtirish | P2 | ☐ | ☐ | |
| EG-24 | Katta shrift | P2 | ☐ | ☐ | |
| EG-25 | Qorong'i rejim | P2 | ☐ | ☐ | |
| EG-26 | Gorizontal holat | P2 | ☐ | ☐ | |
| EG-27 | Klaviatura / kichik ekran | P1 | ☐ | ☐ | |
| EG-28 | Eski va sekin telefon | P1 | ☐ | ☐ | |
| EG-29 | Gesture back | P1 | ☐ | ☐ | |

**Ikkita telefon bir vaqtda (5-bo'lim).** Bu keyslar ikkala telefonni birga ishlatadi,
shuning uchun **bir marta** belgilanadi.

| # | Keys | Prio | Natija | Izoh |
|---|---|:---:|:---:|---|
| TD-01 | Bir hisob ikkala telefonda | P0 | ☐ | |
| TD-02 | Sotuv ketayotganda T2 da kirish | P0 | ☐ | |
| TD-03 | Bir ballon, ikki telefon (online) | P0 | ☐ | |
| TD-04 | Bir abonent, ikki telefon (online) | P0 | ☐ | |
| TD-05 | Deyarli bir vaqtda bosish (poyga) | P0 | ☐ | |
| TD-06 | Ikkalasi offline, bitta ballon | P0 | ☐ | |
| TD-07 | Ikkalasi offline, bitta abonent | P0 | ☐ | |
| TD-08 | T1 online sotdi, T2 navbatda | P0 | ☐ | |
| TD-09 | Bir vaqtda sinxronizatsiya | P1 | ☐ | |
| TD-10 | FaceID cheklovi ikkinchi telefonda | P1 | ☐ | |
| TD-11 | Navbat hisob bilan ko'chmaydi | P0 | ☐ | |

**Yakun:**

| | Qurilma 1 | Qurilma 2 | Ikkalasi birga |
|---|---|---|---|
| P0 o'tdi / jami | / 46 | / 46 | / 9 |
| P1 o'tdi / jami | / 28 | / 28 | / 2 |
| P2 o'tdi / jami | / 8 | / 8 | ➖ |
| **Jami** | **/ 82** | **/ 82** | **/ 11** |
| Umumiy natija | ☐ PASS  ☐ FAIL | ☐ PASS  ☐ FAIL | ☐ PASS  ☐ FAIL |

**PASS sharti:** ikkala qurilmada ham, va ikkita telefon keyslarida ham **hamma P0**
o'tgan bo'lishi. P1/P2 yiqilsa — PASS, lekin kamchilik yozib qo'yiladi.

**Darhol FAIL:** ikkala telefonda ham bir ballon yoki bir abonent uchun "Muvaffaqiyatli"
chiqsa (TD-03…TD-08) — ustuvorligidan qat'i nazar, build prodga chiqmaydi.

---

# 7. Avtomatlashtirilgan testlar

Yuqoridagi keyslarning bir qismi endi kodda. Qolganlari qo'lda qoladi.

**Qayerda:**

| Papka | Nima |
|---|---|
| `app/src/androidTest/java/uz/ssd/egaz/offline/` | Integratsion testlar — haqiqiy Room bazasi, haqiqiy interaktorlar, stub qilingan tarmoq |
| `app/src/androidTest/java/uz/ssd/egaz/uitest/` | E2E UI testlar — ilova ekranlari bo'ylab |
| `app/src/androidTest/java/uz/ssd/egaz/presenter/` | Presenter darajasidagi integratsion testlar |
| `app/src/androidTest/java/uz/ssd/egaz/screen/` | KScreen'lar (Kaspresso/Kakao) |
| `app/src/androidTest/java/uz/ssd/egaz/util/RgsUiFlow.kt` | Login → dashboard → sotuv qadamlari, bir joyda |
| `app/src/test/java/uz/ssd/egaz/model/system/offline/` | Toza unit testlar |

**Ishga tushirish:**

```bash
# hammasi
./gradlew :app:connectedStagingDebugAndroidTest

# bitta klass
./gradlew :app:connectedStagingDebugAndroidTest \
  -Pandroid.testInstrumentationRunnerArguments.class=uz.ssd.egaz.offline.OfflineSyncEngineIntegrationTest
```

**Shartlar — buzilsa testlar yolg'on yiqiladi:**

1. **Debug build.** `MockInterceptor` faqat debuggable buildda OkHttp zanjiriga ulanadi, va
   dvigatel attestatsiya xatosini faqat debug'da kechiradi.
2. **Internetli qurilma.** Dvigatel rejalashtirishdan oldin `NetworkManager` dan so'raydi;
   tarmoqsiz qurilmada hamma narsa "no network" bo'lib o'tib ketadi.
3. **QA qurilmasi.** `OfflineQueueUiTest` ilovaning haqiqiy navbatiga qator yozadi —
   `OfflineSaleDao` da `DELETE` ataylab yo'q. Teardown ularni `DONE` qilib yopadi, lekin
   qatorlar "Sotilgan" tabida qoladi. Ular `99000000001` abonent kodi bilan yoziladi.
   `OfflineSubscriberCardUiTest` esa prefetch keshini tozalaydi (qayta yuklab olsa bo'ladi).

**Qaysi test qaysi keysni yopadi:**

| Test klassi | Nimani tekshiradi | Keyslar |
|---|---|---|
| `OfflineQueueDaoTest` | Navbat jadvali: atomik `claim`, backoff bo'yicha rejalashtirish, hisoblagichlar, `requeue`/`unclaim`/`releaseStuckSending` | OF-08, OF-11, OF-17, OF-23, EG-08, EG-14, EG-15, EG-16 |
| `OfflineSaleQueueIntegrationTest` | Gate + haqiqiy bazalar: `local_balloons` qidiruvi (id emas, nomer), navbat limitga kirishi, depozit chegarasi, `enqueue` tartibi | OF-06…OF-12, OF-21, OF-22, EG-01, EG-07, EG-16 |
| `OfflinePrefetchIntegrationTest` | Sahifalash, uzilgan prefetch eski ro'yxatni buzmasligi, eskirish, kesh → karta o'tkazish | OF-01, OF-20, EG-10 |
| `OfflinePhotoIntegrationTest` | Surat `filesDir` ga ko'chishi, yetimlarni tozalash, EXIF qayta imzolash | EG-17, EG-22 |
| `OfflineRealizationRequestTest` | So'rovda `offline_id` va `offline_dt` borligi, `duplicate`/rad/cheklovchi/`OFFLINE_IN_PROGRESS` klassifikatsiyasi, boshqa inspektorning sotuvi yuborilmasligi | OF-19, MX-06, EG-11…EG-13 |
| `OfflineSyncEngineIntegrationTest` | To'liq yuborish oqimi: qabul, duplicate, rad (surat qoladi), cheklovchi (navbatda qoladi!), 7 kunlik muddat, surat yo'qligi, ketma-ketlik, sotuv paytida bloklanish | OF-13, OF-15, OF-16, MX-03, EG-08, EG-11…EG-14, EG-17 |
| `OfflineQueueUiTest` | Dashboard kartasi → navbat → filtrlar → tafsilot: serverning matni aynan, `EXPIRED` da "Qayta yuborish" yo'qligi | OF-16…OF-18, OF-23, EG-08 |
| `OfflineSubscriberCardUiTest` | Karta serverdanmi yoki keshdanmi; ro'yxatda yo'q abonentda oddiy xatolik; **TLS xatosi offline'ni ochmasligi** | ON-01, OF-03, OF-09, MX-06, EG-19 |
| `SubscriberCardPresenterTest` | Abonent kartasining qarorlari: depozit chegarasi, blok, 30 kunlik limit + "keyin berish" sanasi, kesh/xato/TLS tarmoqlari | ON-01, ON-06, ON-07, ON-08, OF-03, OF-09, MX-06, EG-19 |
| `SaleScreenPresenterTest` | Sotuv ekranining qarorlari: ballon ro'yxati qayerdan, NFC plomba talabi, gate rad javobi o'z so'zlari bilan, ro'yxatda yo'q abonentda oddiy xatolik | ON-03, ON-04, ON-09, OF-03, OF-06, OF-09, MX-01, MX-06 |
| `SaleScreenUiTest` | Sotuv ekrani UI: sarlavha, ballon tanlash + narx paneli, muddat tanlash, NFC qaytarish maydoni, **CTA hamma qiymat to'lmaguncha o'chiq** | ON-01, ON-03, ON-04, ON-09, ON-23, EG-05 |
| `PhotoLivenessUiTest` | Kamera ekrani: `OfflineSaleLock` ochilishi va chiqilganda yopilishi, offline'da attestatsiya ham, challenge ham so'ralmasligi | ON-01, MX-03, OF-02 |
| `RgsSaleChainE2eTest` | To'liq zanjir `Dashboard→Orders→Abonent→Sotuv→Surat→Natija`: online muvaffaqiyat, offline saqlash + sinxronizatsiya, serverning rad javobi ([`SALE-CHAIN-TEST-CASES.md`](SALE-CHAIN-TEST-CASES.md) Z1, Z2, Z3, Z10) | ON-01, OF-02, OF-13, MX-02 |
| `FaceServiceGuardTest` | Cheklovchi va oddiy rad javobini ajratish, 15 soniyalik pol, sanoq arifmetikasi | ON-12, ON-13, EG-13 |
| `PlayIntegrityThrottleTest` | −8 dan keyingi kutish: 1/2/4/8 daqiqa, 10 daqiqa shift, restartda tozalanishi | ON-14 |

**`MockInterceptor` ga qo'shilgan (additiv, test-only fayl):** `enqueueFailure`,
`enqueueOffline`, `enqueueTlsFailure`, `requestCount`. Ularsiz offline yo'lni umuman sinab
bo'lmaydi — offline rejim hech qanday javob bilan emas, faqat **ketmagan so'rov** bilan
ochiladi. `requestCount` — so'rov **umuman qilinmaganini** isbotlash uchun.

**Qo'lda qoladigan keyslar** (avtomatlashtirish qimmat yoki imkonsiz):

- Kamera va liveness ekrani (ON-21) — emulyatorda yuz aniqlash ishonchsiz.
- Haqiqiy Play Integrity −8 va fail-closed (ON-14, ON-15) — Google kvotasi kerak va
  **release buildda** tekshiriladi.
- Xotira to'lishi (EG-18), telefon o'chishi (OF-05), haqiqiy MITM proksi (EG-19 to'liq versiyasi).
- Butun 4.5 bo'limi (EG-23…EG-29) — qurilma sozlamalariga bog'liq, ikkita qurilmada qo'lda.
- Ilova yangilanishidan keyin navbat saqlanishi (EG-21) — ikki APK kerak; `MigrationTestHelper`
  bilan yopish mumkin, lekin avval `app/schemas/.../OfflineDatabase/1.json` bir marta build
  qilinib commit qilinishi shart.
