# Duct Sizing Pro – แอพมือถือ (Android / iOS)

แปลงจากเว็บ `Duct Pro code 3.html` เป็นแอพมือถือ โดยคงสูตรคำนวณเดิมทั้งหมด
(Equal Friction ตามสูตร ASHRAE และ Velocity Constant)

## โครงสร้าง

```
www/                  ตัวแอพ (HTML/JS) – แก้ไขที่นี่
  index.html
  manifest.webmanifest   ข้อมูลสำหรับติดตั้งแบบ PWA
  sw.js                  service worker ใช้งานออฟไลน์
  icons/                 ไอคอนแอพ
resources/            ไอคอนต้นฉบับ 1024px สำหรับสร้างไอคอน native
capacitor.config.json ตั้งค่า Capacitor (เปลี่ยน appId ก่อนขึ้นสโตร์)
package.json
```

## ทางที่ 1: ติดตั้งแบบ PWA (ง่ายสุด ไม่ต้องผ่านสโตร์)

1. อัปโหลดโฟลเดอร์ `www/` ขึ้นเว็บโฮสต์ที่เป็น **HTTPS** (เช่น GitHub Pages, Netlify, Cloudflare Pages ใช้ฟรีได้)
2. เปิดลิงก์บนมือถือ
   - **Android (Chrome):** เมนู ⋮ → "ติดตั้งแอป" / "Add to Home screen"
   - **iPhone (Safari):** ปุ่มแชร์ → "เพิ่มลงในหน้าจอโฮม"
3. เปิดจากไอคอนบนหน้าจอโฮมได้เลย ใช้งานออฟไลน์ได้หลังเปิดครั้งแรก

ทดสอบในเครื่อง: `npm install` แล้ว `npm run serve` จากนั้นเปิด http://localhost:8080

## ทางที่ 2: Build เป็นแอพ native ด้วย Capacitor (ขึ้น Play Store / App Store)

ต้องติดตั้ง Node.js 20+ ก่อน

```bash
npm install
npm run add:android     # สร้างโปรเจกต์ Android (ทำครั้งเดียว)
npm run add:ios         # สร้างโปรเจกต์ iOS (ทำครั้งเดียว ต้องใช้ macOS)
npm run assets          # สร้างไอคอน/splash native จาก resources/
npm run sync            # คัดลอก www/ เข้าโปรเจกต์ native (รันทุกครั้งที่แก้โค้ด)
```

**Android**
- ต้องมี Android Studio
- `npm run open:android` → Build → Generate Signed Bundle / APK
- ส่งไฟล์ `.aab` ขึ้น Google Play Console (ค่าสมัครนักพัฒนา $25 ครั้งเดียว)

**iOS**
- ต้องใช้ Mac + Xcode และบัญชี Apple Developer (ราว $99 ต่อปี)
- `npm run open:ios` → ตั้งค่า Signing & Team → Product → Archive → Distribute

## ก่อนขึ้นสโตร์

- เปลี่ยน `appId` ใน `capacitor.config.json` เป็นของคุณ เช่น `com.yourname.ductpro` (เปลี่ยนไม่ได้หลังเผยแพร่)
- เปลี่ยนไอคอนได้โดยแทนที่ `resources/icon.png` (1024×1024) แล้วรัน `npm run assets`

## หมายเหตุ

- ฟอนต์ Kanit เดิมโหลดจาก Google Fonts ซึ่งใช้ออฟไลน์ไม่ได้ จึงเปลี่ยนเป็นฟอนต์ระบบของเครื่อง
  ถ้าอยากได้ Kanit ในแอพ ให้ดาวน์โหลดไฟล์ฟอนต์ไว้ใน `www/fonts/` แล้วประกาศด้วย `@font-face`
- ผลคำนวณเป็นค่าประมาณเพื่อช่วยออกแบบเบื้องต้น ควรตรวจสอบกับมาตรฐานและวิศวกรผู้รับผิดชอบก่อนใช้งานจริง
