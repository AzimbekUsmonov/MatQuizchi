# 📐 MatQuizchi PWA — O'rnatish qo'llanmasi

## 📁 Fayllar tarkibi

```
matquizchi-pwa/
├── index.html        ← Asosiy ilova (foydalanuvchilar uchun)
├── admin.html        ← Admin panel (alohida sahifa)
├── manifest.json     ← PWA manifest
├── sw.js             ← Service Worker (offline rejim)
├── firestore.rules   ← Firebase xavfsizlik qoidalari
└── README.md         ← Shu fayl
```

---

## 🔥 1-QADAM: Firebase loyiha yaratish

1. [console.firebase.google.com](https://console.firebase.google.com) ga kiring
2. **Add project** → loyiha nomi: `matquizchi`
3. **Firestore Database** → Create database → **Production mode**
4. **Authentication** → Sign-in method → **Anonymous** ni yoqing
5. **Project settings** → **Your apps** → Web app qo'shing (</> belgisi)
6. Berilgan `firebaseConfig` ni nusxalab oling

---

## ⚙️ 2-QADAM: Firebase config ni qo'yish

`index.html` va `admin.html` fayllarida shu joyni toping va o'zgartiring:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",           // ← shu yerga
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

---

## 👑 3-QADAM: Admin ID sozlash

`admin.html` faylida shu qatorni toping:

```javascript
const ADMIN_IDS = ['8296061905']; // ← o'zingizning Telegram ID ingiz
```

**Telegram ID ni qanday bilsa bo'ladi?**
- [@userinfobot](https://t.me/userinfobot) ga `/start` yuboring

---

## 🔐 4-QADAM: Firestore xavfsizlik qoidalari

Firebase Console → Firestore → **Rules** tabiga o'ting va `firestore.rules` faylidagi kodni to'liq ko'chiring.

---

## 🌐 5-QADAM: GitHub Pages ga deploy qilish

### GitHub repository yaratish:
1. [github.com](https://github.com) → **New repository**
2. Nom: `matquizchi-pwa` (yoki boshqa nom)
3. Public qiling

### Fayllarni yuklash:
```bash
git init
git add .
git commit -m "MatQuizchi PWA"
git remote add origin https://github.com/SIZNING_USERNAME/matquizchi-pwa.git
git push -u origin main
```

### GitHub Pages yoqish:
- Repository → **Settings** → **Pages**
- Source: **Deploy from a branch** → `main` → `/ (root)`
- **Save** bosing

✅ Bir necha daqiqadan keyin sayt tayyor: `https://USERNAME.github.io/matquizchi-pwa/`

---

## 🤖 6-QADAM: Telegram bot bilan ulash

`MatQuizchibot` da `/setmenubutton` buyrug'i orqali:

```
Menu button URL: https://USERNAME.github.io/matquizchi-pwa/
Menu button text: 🎮 O'ynash
```

Yoki BotFather orqali:
1. `/mybots` → botni tanlang
2. **Bot Settings** → **Menu Button**
3. URL: sayt manzilini kiriting

---

## 📊 Firestore kolleksiyalar tuzilmasi

```
users/
  {telegramUserId}/
    uid, name, username, avatar
    correct, wrong, xp, level, streak
    pvpWins, pvpLoss, pvpDraw
    seasonScore, referrals
    plan (free/silver/gold/diamond)
    planExpiry, planGivenBy
    clanId, clanTag
    bio, achievements[]
    createdAt, lastActive

questions/
  {autoId}/
    question, opt_a, opt_b, opt_c, opt_d
    correct (A/B/C/D)
    explanation
    createdAt

topics/
  {autoId}/
    title, content, createdAt

tournaments/
  {autoId}/
    name, description, status (upcoming/active/finished)
    maxParticipants, participantCount
    questionsPerMatch, timeLimit
    participants[]
    createdAt
  
  {tId}/matches/{mId}/
    player1, player2, player1Name, player2Name
    round, p1Score, p2Score, winner
    status

quizHistory/
  {autoId}/
    uid, type (regular/speed)
    correct, wrong, xp, time
    createdAt

speedStats/
  {uid}/
    uid, name, username
    bestScore, totalGames, avgTime

pvpBattles/
  {battleId}/
    player1, player2
    player1Name, player2Name
    scores{}, status (waiting/active/finished)
    winner, createdAt

clans/
  {clanId}/
    name, tag, description
    leaderId, leaderName
    xp, memberCount, members[]
    createdAt

dailyUsage/
  {uid_date}/
    quiz, pvp

broadcasts/
  {autoId}/
    text, target, recipientCount
    sentBy, createdAt

seasonHistory/
  {autoId}/
    name, number, winner, endedAt

settings/
  season/
    name, number, startedAt
  config/
    requiredChannels[], dailyLimit, xpMultiplierFree
  admins/
    ids[]
  globalStats/
    totalUsers, totalCorrect, totalAnswers
```

---

## 🎮 Ilova funksiyalari

### Foydalanuvchi tomonida:
- ✅ Telegram orqali kirish (WebApp)
- 🎯 Quiz (10 savol, A/B/C/D)
- ⚡ Tezkor Quiz (5 savol, 15 soniya limit)
- ⚔️ PvP Battle (havola orqali)
- 🏆 Reyting (Umumiy / Mavsum / PvP / Tezkor)
- 👤 Profil (statistika, yutuqlar, klan, tarix)
- 🛡 Klan (yaratish, qo'shilish, boshqarish)
- 📚 O'rganish (mavzular)
- 🌟 Mavsumiy reyting
- 💎 Premium rejalar
- 👥 Referal tizimi
- 🏅 Yutuqlar (12 ta achievement)
- ⚡ XP va daraja tizimi (10 daraja)

### Admin tomonida:
- 📊 Dashboard (statistika, chartlar, top o'yinchilar)
- ❓ Savollar (qo'shish, o'chirish, qidirish)
- 📚 Mavzular (qo'shish, o'chirish)
- 🏆 Turnirlar (yaratish, boshlash, yakunlash)
- 👥 Foydalanuvchilar (ko'rish, premium berish, ban)
- 💎 Premium boshqaruvi (berish, olib tashlash)
- 🛡 Klan boshqaruvi
- 📢 Xabar yuborish (hammaga / premium / free / faol)
- 🌟 Mavsum boshqaruvi (reset, yangi mavsum)
- ⚙️ Sozlamalar (admin qo'shish, limitlar)

---

## ❓ Savollar bazasini to'ldirish

Admin panel → **Savollar** → **➕ Savol qo'shish**

Yoki bot orqali ham qo'shish mumkin (bot DB → Firestore import kerak bo'ladi).

---

## 🆘 Muammolar

| Muammo | Yechim |
|--------|--------|
| "Siz admin emassiz" | `admin.html` dagi `ADMIN_IDS` ga Telegram ID ni qo'shing |
| Savollar ko'rinmaydi | Firestore da `questions` kolleksiyasiga kamida 1 savol qo'shing |
| Firebase error | `firebaseConfig` ni to'g'ri kiritganingizni tekshiring |
| PWA o'rnatilmaydi | HTTPS kerak (GitHub Pages ishlatilsa muammo yo'q) |
