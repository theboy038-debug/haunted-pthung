# THE HAUNTED P'THUNG 🎃
*A Little Halloween Surprise for Bam — A little spooky. A little silly. A whole lot of orange.*

เว็บเซอร์ไพรส์ฮาโลวีนหน้าเดียว (Static Website) มีพี่ถุงเป็นตัวเอก
ใช้ HTML + Tailwind CDN + Vanilla JavaScript ไม่มี Backend / Database / Tracking / การเก็บข้อมูลใด ๆ

## โครงสร้างไฟล์
```
project/
├── index.html          ← ไฟล์หลัก (HTML + CSS + JS อยู่ในไฟล์เดียว แก้ไขง่าย)
├── vercel.json         ← บอก Vercel ให้เสิร์ฟจาก root (ดูหัวข้อ "ทำไมต้องมี vercel.json")
├── README.md
└── public/images/      ← รูปพี่ถุง thung-01.jpg ... thung-13.jpg (ครบ 13 รูป)
```

## ดูตัวอย่างในเครื่อง
เปิด `index.html` ด้วยเบราว์เซอร์ได้เลย หรือรันเซิร์ฟเวอร์:
```bash
python3 -m http.server 5500
# เปิด http://localhost:5500
```
> ต้องต่ออินเทอร์เน็ตเพื่อโหลดฟอนต์ Prompt และ Tailwind CDN (ถ้าโหลดไม่ได้ เว็บยังใช้งานได้ด้วยฟอนต์ไทยของเครื่อง)

## Deploy บน Vercel (ไม่ต้องใช้รหัสผ่านเข้าชม)
**วิธีที่ 1 — ผ่านเว็บ (ง่ายสุด)**
1. อัปโหลดโฟลเดอร์ทั้งหมดขึ้น GitHub (ต้องมี `index.html`, `vercel.json`, `public/images/` อยู่ใน root ของ repo)
2. เข้า vercel.com → **Add New… → Project** → เลือก repo
3. Framework Preset: **Other** · Build Command: เว้นว่าง · Output Directory: เว้นว่าง (ใช้ค่าจาก `vercel.json`)
4. กด **Deploy** แล้วจะได้ลิงก์ `xxx.vercel.app` ส่งให้แบมได้เลย

**วิธีที่ 2 — Vercel CLI**
```bash
npm i -g vercel
cd project
vercel deploy --prod
```

### ทำไมต้องมี vercel.json
Vercel (preset "Other") จะใช้โฟลเดอร์ `public/` เป็น Output Directory โดยอัตโนมัติถ้ามีโฟลเดอร์นี้อยู่
ทำให้ `index.html` ที่อยู่ root ไม่ถูกเสิร์ฟ (ขึ้น 404) — ไฟล์ `vercel.json` (`"outputDirectory": "."`) แก้ปัญหานี้
รูปจึงอ้างอิงด้วย path แบบ relative `public/images/thung-XX.jpg` ซึ่งใช้ได้ทั้งในเครื่องและบน Vercel
(ไม่ใช่ `/images/...` ตามที่ร่างไว้เดิม เพราะ path นั้นจะใช้ได้ก็ต่อเมื่อ Vercel เสิร์ฟจาก `public/` เท่านั้น)

## วิธีแก้ไขเนื้อหา
ค้นหาคอมเมนต์ `CONFIG` ใน `index.html` (ในแท็ก `<script>` ท้ายไฟล์)
- `PHOTOS` — ขนาดรูป, `pos` (จุดโฟกัสไม่ให้ครอปหน้า/หูพี่ถุง) และ alt text
- `GALLERY` — ชื่อภาพและแคปชัน 13 รายการ
- `ORACLE` — คำทำนาย 15 รายการ (`photo` คือเลขรูปที่จับคู่กับคำทำนาย)
- ข้อความจดหมาย อยู่ในแท็ก `<article class="paper">`
- เปลี่ยนรูป: วางไฟล์ทับชื่อเดิม (แนะนำ JPG ด้านยาวไม่เกิน 1600px) ถ้าขนาดรูปเปลี่ยนให้แก้ `w`/`h` ใน `PHOTOS`

## เรื่องเสียง
เสียงทั้งหมด (เหมียว, เซียมซี, ดนตรีกล่องเพลงเบา ๆ) สร้างสดด้วย Web Audio API ไม่มีไฟล์เสียงลิขสิทธิ์
เริ่มหลังผู้ใช้กดปุ่มแรกเท่านั้น และปิดเสียงได้ที่ปุ่มมุมขวาบน ถ้าเบราว์เซอร์บล็อกเสียง ทุกอย่างยังทำงานต่อได้ตามปกติ

## ข้อจำกัดที่ควรรู้
- **Tailwind Play CDN** เหมาะกับงานเล็กที่ไม่ต้องมีขั้นตอน Build แต่ไม่ใช่ตัวเลือกที่ดีที่สุดสำหรับ Production ทุกกรณี
  (ต้องโหลดสคริปต์จากภายนอกทุกครั้งที่เปิด) งานนี้ใช้ Tailwind เป็นส่วนเสริม เลย์เอาต์หลักเขียนใน CSS ของไฟล์เอง จึงไม่พังถ้า CDN โหลดไม่ได้
- ชื่อฟีเจอร์ "15 Haunted Paws" คงไว้ตามแนวคิดเดิม แต่มีรูปจริง 13 รูป (ไม่สร้างรูปเพิ่ม) หน้าเว็บอธิบายว่า "อีก 2 กรอบเป็นผีล่องหน"
- เว็บใส่ `noindex` เพื่อไม่ให้ค้นเจอผ่าน Google แต่ใครมีลิงก์ก็เปิดได้
