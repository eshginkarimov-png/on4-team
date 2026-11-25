# ON4 TEAM tətbiqi — quraşdırma təlimatı

## Lazım olanlar
- Node.js (nodejs.org, LTS versiya)
- iOS üçün: Mac + Xcode (App Store-dan pulsuz) + Apple Developer hesabı (ildə ~99 USD)
- Android üçün: Android Studio (pulsuz) + Google Play hesabı (birdəfəlik ~25 USD)

## Addımlar (bu qovluqda terminal açın)
1. npm install @capacitor/core @capacitor/cli @capacitor/ios @capacitor/android
2. npm install -D @capacitor/assets
3. npx capacitor-assets generate      (assets/ qovluğundakı şəkillərdən ikon və açılış ekranı yaradır)
4. npx cap add ios
5. npx cap add android
6. npx cap sync
7. npx cap open ios        → Xcode açılır
   npx cap open android    → Android Studio açılır

## iPhone-da sınaqdan keçirmək
Xcode-da Signing & Capabilities → Team seçin (Apple ID ilə), iPhone-u kabellə qoşun, yuxarıda telefonu seçib ▶ basın.

## App Store-a göndərmək
Xcode: Product → Archive → Distribute App → App Store Connect. Sonra appstoreconnect.apple.com-da ad, təsvir, ekran şəkilləri və məxfilik siyasəti linkini doldurub "Review"-a göndərin.

## Məzmunu dəyişmək
www/index.html faylını redaktə edin (portfolyo layihələri "const J" siyahısındadır), sonra: npx cap sync

---
# ADMİN PANELİ (Firebase) — məzmunu tətbiqin içindən dəyişmək

Admin girişi müştərilərə görünmür. Profil səhifəsinin ən altındakı "© 2026 ON4 TEAM" yazısına 7 dəfə ardıcıl toxunun, giriş forması açılacaq. Giriş etdikdən sonra portfolyo, paketlər, qeyd və əlaqə məlumatlarını dəyişib "Yadda saxla" basırsınız. Dəyişiklik bütün istifadəçilərdə dərhal görünür, tətbiqi yenidən App Store-a göndərmək lazım deyil.

## Qurulum (bir dəfə, pulsuz Spark planı kifayətdir)
1. console.firebase.google.com → Add project → "on4-team".
2. Project settings → Your apps → Web (</>) → app qeydiyyatdan keçirin. Çıxan `apiKey, authDomain, projectId, appId` dəyərlərini `www/index.html` faylında `const FB={...}` içinə yazın.
3. Build → Authentication → Get started → Sign-in method → "Email/Password" aktiv edin. Users bölməsində Add user ilə admin e-poçtu və parol yaradın.
4. Build → Firestore Database → Create database (production mode, region: europe-west).
5. Firestore → Rules bölməsinə aşağıdakını yapışdırıb Publish edin (ADMIN_EMAIL əvəzinə öz e-poçtunuzu yazın):

rules_version = '2';
service cloud.firestore {
  match /databases/{d}/documents {
    match /app/content {
      allow read: if true;
      allow write: if request.auth != null && request.auth.token.email == "ADMIN_EMAIL";
    }
    match /images/{id} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.token.email == "ADMIN_EMAIL";
    }
  }
}

6. `npx cap sync` yazın və tətbiqi yenidən qurun (bu, yalnız bir dəfə lazımdır).
7. Tətbiqdə admin kimi daxil olub "Yadda saxla" basın. İlk saxlamada standart məzmun bazaya yazılır.

## Qeydlər
- Firebase ayarları boş qalsa, tətbiq demo rejimdə işləyir (parol "admin", dəyişikliklər yalnız həmin cihazda qalır). Real tətbiqdə mütləq ayarları doldurun.
- Portfolyo fotoları telefondan/kompüterdən fayl seçilərək əlavə olunur. Foto avtomatik kiçildilir (maks. 900 px) və Firestore-da "images" kolleksiyasında saxlanır, ona görə ödənişli plan lazım deyil.
- Firebase apiKey gizli deyil. Təhlükəsizliyi yuxarıdakı Rules təmin edir.


---
# VEB VERSİYA (link ilə açılır, proqram yükləmək lazım deyil)

Eyni kod brauzerdə də tam işləyir. Müştəri linkə toxunur, səhifə açılır, istəsə proqram kimi quraşdırır. Bio-ya, saytınıza və Instagram-a bu linki qoya bilərsiniz.

## Pulsuz yerləşdirmə (Firebase Hosting)
1. Terminalda: npm install -g firebase-tools
2. firebase login
3. Bu qovluqda: firebase use --add   (admin üçün yaratdığınız Firebase layihəsini seçin)
4. firebase deploy --only hosting
5. Link hazırdır: https://LAYIHƏ-ADI.web.app

## Öz ünvanınızla (məsələn app.on4team.az)
Firebase Console → Hosting → Add custom domain → ünvanı yazın, göstərilən DNS qeydlərini domen təchizatçısında əlavə edin.
Sonra Authentication → Settings → Authorized domains bölməsinə həmin ünvanı əlavə edin (admin girişi üçün lazımdır).

## Quraşdırma (istəyə bağlı)
- iPhone/iPad: Safari → Paylaş → Ana ekrana əlavə et
- Android: Chrome → ⋮ → Tətbiqi quraşdır
- Windows/macOS: Chrome/Edge ünvan çubuğunda "Quraşdır" düyməsi

## Alternativ
www qovluğunu Netlify (app.netlify.com/drop) saytına sürükləyib buraxmaq da kifayətdir.

Qeyd: web versiya və mobil proqram eyni Firebase məlumatını göstərir. Admin paneldə bir dəfə dəyişiklik etsəniz, hər ikisində görünür.

---
# GITHUB-DA QURMAQ

## 1. Repozitoriya yaradın
github.com → New repository → ad: on4-team → Public (Pages pulsuz planda yalnız public repoda işləyir) → Create.

## 2. Kodu yükləyin (terminalda, bu qovluqda)
git init
git add .
git commit -m "ON4 TEAM tətbiqi"
git branch -M main
git remote add origin https://github.com/İSTİFADƏÇİ-ADI/on4-team.git
git push -u origin main

(Terminal istəmirsinizsə: GitHub Desktop proqramı ilə qovluğu əlavə edib "Publish repository" basın.)

## 3. Veb link (avtomatik)
Repo → Settings → Pages → Source: "GitHub Actions" seçin. Sonra Actions bölməsində "Veb versiya" işi bitəndə link hazır olur:
https://İSTİFADƏÇİ-ADI.github.io/on4-team/
Hər dəfə kodu push edəndə sayt avtomatik yenilənir.
Firebase admin girişi üçün: Firebase Console → Authentication → Settings → Authorized domains → "İSTİFADƏÇİ-ADI.github.io" əlavə edin.

## 4. Android APK (avtomatik)
Repo → Actions → "Android APK" → Run workflow. 5–10 dəqiqədən sonra işin səhifəsində "on4-team-apk" faylını endirin, içindən app-debug.apk çıxacaq. Telefona göndərib quraşdırmaq olar.
(Google Play üçün imzalanmış .aab lazımdır, bunu Android Studio-dan edin.)

## iOS
iOS proqramı üçün Mac və Xcode lazımdır (yuxarıdakı təlimat). GitHub-da yalnız kodu saxlayırsınız.

## Önəmli
- Firebase apiKey gizli deyil, repoda qala bilər. Təhlükəsizliyi Firestore qaydaları təmin edir.
- Admin e-poçtunu və parolunu heç vaxt koda yazmayın.
