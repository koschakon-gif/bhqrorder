# BH-ORDER Admin v3

เวอร์ชันนี้ต่อยอดจาก v2 โดยยังไม่ใส่ข้อมูลเมนูจริงล่วงหน้า และไม่มีตัวเลือก Size

## ความสามารถเพิ่มใน v3
- เพิ่มและแก้ไขเมนูผ่าน Supabase
- เปิด/ปิดขายแต่ละเมนู
- เปิด/ปิดหมวดหมู่
- อัปโหลดรูปไปที่ Supabase Storage bucket `menu-images`
- เลื่อนเมนูขึ้น/ลงเพื่อจัดลำดับ
- ตรวจสอบชื่อและราคา พร้อมแสดง error/success message
- ไม่มีฟิลด์ Size S/M/L

## ติดตั้ง/อัปเดต
1. สำรองไฟล์เดิมก่อน
2. แตก ZIP และคัดลอกไฟล์ทั้งหมดในโฟลเดอร์ `bh-order-v3` ไปที่ root ของ GitHub repo `bh-order` โดยยอมให้แทนที่ไฟล์เดิม
3. ตรวจสอบ Environment Variables ใน Vercel: `NEXT_PUBLIC_SUPABASE_URL` และ `NEXT_PUBLIC_SUPABASE_ANON_KEY`
4. Push ไป GitHub เพื่อให้ Vercel deploy
5. หากตั้งค่า schema ของ v2 เรียบร้อยแล้ว ไม่ต้องรัน schema.sql ซ้ำโดยอัตโนมัติ ให้ตรวจสอบก่อน เพราะการรันซ้ำอาจแจ้งว่า policy มีอยู่แล้ว

## หมายเหตุ
- ต้องตั้งค่า Supabase Auth, staff_profiles และ storage policy ตาม schema ที่ให้มา
- ห้ามใส่ `service_role` key ในตัวแปรที่ขึ้นต้นด้วย `NEXT_PUBLIC_` หรือใน client code
- ทดสอบกับโปรเจกต์ Supabase ของร้านจริงก่อนเปิดใช้งาน
- ตัวเลือกเสริมของสินค้า (เช่น ความหวาน/เพิ่มช็อต) และ QR ordering ยังเป็นงานในเวอร์ชันถัดไป

## v4 — Realtime Admin order alerts
- Admin Orders subscribes to Supabase Realtime `INSERT` and `UPDATE` events on `public.orders`.
- New orders appear without refreshing. Optional browser notification and in-page sound can be enabled by the admin.
- Admin can advance order status from the order card.
- Run the updated `supabase/schema.sql` in Supabase SQL Editor to add `public.orders` to the `supabase_realtime` publication.
- In Supabase Dashboard, confirm Database > Replication / Publications includes `orders` in `supabase_realtime` if the SQL cannot add it.
- Browser notifications require the admin to grant permission. Sound may require tapping “เปิดเสียง” first due to browser autoplay restrictions.
- This is the admin-side notification step. Customer checkout still must insert a row into `public.orders` for an order to appear.
