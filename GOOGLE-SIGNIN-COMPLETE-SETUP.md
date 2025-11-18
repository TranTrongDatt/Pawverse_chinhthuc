# 🚀 **HOÀN THIỆN GOOGLE SIGN-IN**

## **✅ ĐÃ HOÀN THÀNH**

### **Backend API** ✅
```
✅ Endpoint: POST /api/auth/external-login
✅ Google token verification
✅ Auto create user
✅ JWT token generation
```

### **Mobile App Code** ✅
```
✅ AuthService với Google Sign-In
✅ AuthProvider integration
✅ AuthRepository cập nhật endpoint
✅ Login screen với Google button
✅ UI flow hoàn chỉnh
```

---

## **📋 CÒN LẠI: GOOGLE CLOUD CONFIGURATION**

### **Bước 1: Tạo Google Cloud Project**

1. **Truy cập:** https://console.cloud.google.com/
2. **Tạo project mới:**
   - Click "New Project"
   - Tên: `PawVerse`
   - Click "Create"

---

### **Bước 2: Enable Google Sign-In API**

1. **APIs & Services** → **Library**
2. Tìm "Google Sign-In API"
3. Click **Enable**

---

### **Bước 3: Tạo OAuth 2.0 Credentials**

#### **3a. Android OAuth Client**

1. **APIs & Services** → **Credentials**
2. Click **Create Credentials** → **OAuth client ID**
3. Chọn **Android**

**Cấu hình:**
```
Package name: com.example.pawversemobile
SHA-1 certificate fingerprint: [Xem bước 4]
```

#### **3b. Web OAuth Client (Optional - cho backend)**

1. Create Credentials → OAuth client ID
2. Chọn **Web application**
3. Authorized redirect URIs: `https://localhost:7089/signin-google`

---

### **Bước 4: Lấy SHA-1 Fingerprint**

#### **Windows (Debug keystore):**
```bash
keytool -list -v -keystore %USERPROFILE%\.android\debug.keystore -alias androiddebugkey -storepass android -keypass android
```

#### **Output:**
```
Certificate fingerprints:
	 SHA1: A1:B2:C3:D4:E5:F6:...  ← Copy dòng này
```

**Paste SHA-1 vào Google Cloud Console**

---

### **Bước 5: Download google-services.json (Firebase - Optional)**

**Nếu dùng Firebase:**

1. Truy cập: https://console.firebase.google.com/
2. Create/Select project
3. Add Android app:
   - Package: `com.example.pawversemobile`
   - Download `google-services.json`
4. Copy file vào: `android/app/google-services.json`

**Nếu KHÔNG dùng Firebase:**
- Skip bước này
- Google Sign-In vẫn hoạt động với OAuth credentials

---

### **Bước 6: Cấu hình Android**

#### **android/build.gradle:**
```gradle
buildscript {
    ext.kotlin_version = '1.7.10'
    repositories {
        google()
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:7.3.0'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        
        // ✅ Add this (only if using Firebase)
        // classpath 'com.google.gms:google-services:4.3.15'
    }
}
```

#### **android/app/build.gradle:**

**Thêm vào cuối file:**
```gradle
dependencies {
    // Google Sign-In
    implementation 'com.google.android.gms:play-services-auth:20.7.0'
}

// ✅ Add this line at the very end (only if using Firebase)
// apply plugin: 'com.google.gms.google-services'
```

---

### **Bước 7: Cập nhật AndroidManifest.xml**

**android/app/src/main/AndroidManifest.xml:**

```xml
<manifest>
    <application>
        <!-- Your existing config -->
        
        <!-- ✅ Add Google Sign-In metadata -->
        <meta-data
            android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
    </application>
</manifest>
```

---

### **Bước 8: Hot Restart App**

```bash
# Clean
flutter clean

# Get dependencies
flutter pub get

# Run
flutter run
```

---

## **🧪 TESTING**

### **Test Flow:**

```
1. Mở app
   ↓
2. Màn hình Login
   ↓
3. Thấy nút "Đăng nhập với Google"
   ↓
4. Tap button
   ↓
5. Chọn Google account
   ↓
6. ✅ Đăng nhập thành công
   ↓
7. Redirect về Home screen
```

---

## **🔧 TROUBLESHOOTING**

### **Error: "sign_in_failed"**

**Nguyên nhân:**
- SHA-1 fingerprint sai
- Package name không khớp
- OAuth client chưa enable

**Fix:**
```bash
# 1. Verify SHA-1
keytool -list -v -keystore %USERPROFILE%\.android\debug.keystore -alias androiddebugkey -storepass android

# 2. Check package name in android/app/build.gradle
applicationId "com.example.pawversemobile"

# 3. Xóa OAuth client cũ, tạo mới
```

---

### **Error: "DEVELOPER_ERROR"**

**Nguyên nhân:**
- Wrong OAuth client ID
- Package name mismatch

**Fix:**
1. Google Cloud Console
2. Check OAuth client package name = app package name
3. Check SHA-1 matches

---

### **Error: "Error 10"**

**Nguyên nhân:**
- Google Play Services not installed/outdated

**Fix:**
- Update Google Play Services on device/emulator

---

### **Error: "ApiException: 12500"**

**Nguyên nhân:**
- Debug keystore SHA-1 không khớp
- OAuth client configuration sai

**Fix:**
```bash
# Get SHA-1
keytool -list -v -keystore %USERPROFILE%\.android\debug.keystore -alias androiddebugkey -storepass android -keypass android

# Add to Google Cloud Console:
APIs & Services → Credentials → Edit OAuth client → Add SHA-1
```

---

## **📊 VERIFICATION CHECKLIST**

Sau khi setup:

- [ ] Google Cloud Project created
- [ ] OAuth 2.0 Client ID (Android) created
- [ ] SHA-1 fingerprint added
- [ ] Package name matches: `com.example.pawversemobile`
- [ ] `play-services-auth` dependency added
- [ ] App rebuilt with `flutter clean && flutter pub get`
- [ ] Google button visible on login screen
- [ ] Tap button shows Google account picker
- [ ] Can select account
- [ ] Redirects to home after login
- [ ] User info displays in profile

---

## **🎯 ALTERNATIVE: NO FIREBASE SETUP**

Nếu không muốn dùng Firebase:

1. ✅ Chỉ cần OAuth 2.0 Client ID (Android)
2. ✅ Không cần `google-services.json`
3. ✅ Không cần `com.google.gms.google-services` plugin
4. ✅ Chỉ cần `play-services-auth` dependency

**Minimal setup:**
- Google Cloud OAuth credentials
- SHA-1 fingerprint
- play-services-auth dependency
- Package name match

---

## **📖 REFERENCES**

- [Google Sign-In Plugin](https://pub.dev/packages/google_sign_in)
- [Google Cloud Console](https://console.cloud.google.com/)
- [Get SHA-1 Guide](https://developers.google.com/android/guides/client-auth)
- [Backend OAuth Implementation](../../PawVerseAPI/Controllers/AuthController.cs)

---

## **💡 QUICK START COMMANDS**

```bash
# 1. Get SHA-1
keytool -list -v -keystore %USERPROFILE%\.android\debug.keystore -alias androiddebugkey -storepass android -keypass android

# 2. Clean & rebuild
flutter clean
flutter pub get

# 3. Run
flutter run

# 4. Test
Tap "Đăng nhập với Google" button
```

---

## **🎉 SUCCESS INDICATORS**

Khi setup thành công:

✅ **Login screen:**
- Button "Đăng nhập với Google" hiển thị
- Click button → Google account picker xuất hiện

✅ **After selecting account:**
- Loading indicator
- Redirect to home screen
- User info populated in profile

✅ **Backend logs:**
```
POST /api/auth/external-login
Provider: Google
IdToken verified ✓
User created/found ✓
JWT generated ✓
Response: 200 OK
```

---

## **🚦 NEXT STEPS**

Sau khi Google Sign-In hoạt động:

1. ✅ Test with multiple Google accounts
2. ✅ Test logout → login again
3. ✅ Verify avatar from Google displays
4. ✅ Check profile info (name, email) from Google
5. ✅ Test on physical device (not just emulator)

---

**READY TO CONFIGURE GOOGLE CLOUD!** 🚀
