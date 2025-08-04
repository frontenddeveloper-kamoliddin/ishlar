# Ish Olami - Job Marketplace Platform

Ish Olami - bu ish izlovchilar va ish beruvchilar uchun zamonaviy va samarali platforma. Platforma Firebase yordamida ishlaydi va quyidagi asosiy funksiyalarni taqdim etadi:

## 🚀 Asosiy funksiyalar

### 👥 Foydalanuvchilar uchun
- **Ro'yxatdan o'tish va tizimga kirish** - Xavfsiz autentifikatsiya
- **Profil boshqaruvi** - Shaxsiy ma'lumotlarni tahrirlash
- **Reyting tizimi** - Foydalanuvchilarni baholash va sharh berish

### 💼 Ish izlovchilar uchun
- **Ish qidirish** - Kategoriya va joylashuv bo'yicha filtrlash
- **Ariza berish** - Bir necha bosqichda ishga ariza berish
- **Arizalar tarixi** - Barcha arizalarni ko'rish va kuzatish

### 🏢 Ish beruvchilar uchun
- **Ish joylashtirish** - Batafsil ma'lumotlar bilan ish e'lon qilish
- **Arizalarni ko'rish** - Kelgan arizalarni boshqarish
- **Nomzodlarni tanlash** - Eng mos nomzodlarni qabul qilish

### 💬 Aloqa tizimi
- **Real-time chat** - Ish beruvchi va ish izlovchi o'rtasida suhbatlashish
- **Xabar almashish** - Tezkor xabar yuborish va qabul qilish
- **Bildirishnomalar** - Yangi xabarlar haqida ma'lumot

## 🛠 Texnologiyalar

- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Styling**: Tailwind CSS
- **Backend**: Firebase (Authentication, Firestore)
- **Icons**: Font Awesome
- **Responsive Design**: Mobile-first approach

## 📁 Loyiha tuzilishi

```
santexnik/
├── index.html              # Bosh sahifa (login/register)
├── dashboard.html          # Asosiy dashboard
├── profile.html            # Profil sahifasi
├── admin.html              # Admin panel
├── css/
│   └── styles.css          # Maxsus stillar
├── js/
│   ├── auth.js             # Autentifikatsiya tizimi
│   ├── jobs.js             # Ish boshqaruvi
│   ├── chat.js             # Chat tizimi
│   ├── profile.js          # Profil boshqaruvi
│   ├── dashboard.js        # Dashboard boshqaruvi
│   └── utils.js            # Yordamchi funksiyalar
├── firebase/
│   └── firebase-config.js  # Firebase sozlamalari
└── README.md               # Loyiha ma'lumotlari
```

## ⚙️ O'rnatish va sozlash

### 1. Firebase loyihasini yarating

1. [Firebase Console](https://console.firebase.google.com/) ga kiring
2. Yangi loyiha yarating
3. Authentication yoqib qo'ying (Email/Password)
4. Firestore Database yarating
5. Loyiha sozlamalaridan API kalitlarini oling

### 2. Firebase sozlamalarini yangilang

`firebase/firebase-config.js` faylini oching va o'zingizning Firebase ma'lumotlaringiz bilan almashtiring:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### 3. Firestore qoidalarini sozlang

Firestore Database > Rules bo'limida quyidagi qoidalarni qo'shing:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users collection
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Jobs collection
    match /jobs/{jobId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
    
    // Applications collection
    match /applications/{applicationId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
    
    // Chats collection
    match /chats/{chatId} {
      allow read, write: if request.auth != null && 
        request.auth.uid in resource.data.participants;
      
      match /messages/{messageId} {
        allow read, write: if request.auth != null && 
          get(/databases/$(database)/documents/chats/$(chatId)).data.participants[request.auth.uid] != null;
      }
    }
  }
}
```

### 4. Loyihani ishga tushiring

1. Barcha fayllarni web server ga yuklang
2. Yoki mahalliy server ishlatib, loyihani oching
3. `index.html` faylini brauzerda oching

## 📱 Foydalanish bo'yicha ko'rsatmalar

### Ro'yxatdan o'tish
1. Bosh sahifada "Ro'yxatdan o'tish" tugmasini bosing
2. To'liq ism, email va parol kiriting
3. Rolni tanlang (Ish izlovchi yoki Ish beruvchi)
4. "Ro'yxatdan o'tish" tugmasini bosing

### Ish joylashtirish (Ish beruvchilar uchun)
1. Dashboard da "Ish joylashtirish" tugmasini bosing
2. Barcha maydonlarni to'ldiring
3. "Joylashtirish" tugmasini bosing

### Ish qidirish (Ish izlovchilar uchun)
1. Dashboard da "Ishlar" tabini oching
2. Qidiruv maydonida kalit so'zlarni kiriting
3. Kategoriya va joylashuv bo'yicha filtrlash
4. Kerakli ishni topib, "Ariza berish" tugmasini bosing

### Chat tizimi
1. "Chat" tabini oching
2. Suhbatlashmoqchi bo'lgan foydalanuvchini tanlang
3. Xabar yozing va yuboring

## 🔧 Maxsus sozlamalar

### Yangi kategoriyalar qo'shish
`js/jobs.js` faylida `getCategoryName` funksiyasini yangilang:

```javascript
getCategoryName(category) {
  const categories = {
    'technology': 'Texnologiya',
    'design': 'Dizayn',
    'marketing': 'Marketing',
    'sales': 'Sotuv',
    'education': 'Ta\'lim',
    'healthcare': 'Sog\'liqni saqlash',
    'other': 'Boshqa',
    'your-category': 'Sizning kategoriyangiz' // Yangi kategoriya
  };
  return categories[category] || category;
}
```

### Yangi joylashuvlar qo'shish
Dashboard HTML faylida joylashuvlar ro'yxatini yangilang.

## 🚀 Keyingi versiyalar uchun rejalar

- [ ] Real-time bildirishnomalar
- [ ] Fayl yuklash (CV, rasmlar)
- [ ] Video intervyu tizimi
- [ ] Ish tarixi va tavsiyalar
- [ ] To'lov tizimi integratsiyasi
- [ ] Mobile app (React Native)
- [ ] AI-powered ish tavsiyalari
- [ ] Ko'p tilli qo'llab-quvvatlash

## 🤝 Hissa qo'shish

Loyihaga hissa qo'shmoqchi bo'lsangiz:

1. Repository ni fork qiling
2. Yangi branch yarating
3. O'zgarishlaringizni qiling
4. Pull request yuboring

## 📞 Aloqa

Savollar yoki takliflar uchun:
- Email: [your-email@example.com]
- Telegram: [@your-username]

## 📄 Litsenziya

Bu loyiha MIT litsenziyasi ostida tarqatiladi.

---

**Ish Olami** - O'zbekistondagi eng yaxshi ish platformasi! 🚀 