# Product Backlog

Backlog นี้สรุปมาจากเอกสาร requirement แต่ละฉบับใน [[01-spec/index|01-spec]] เรียงตามลำดับความสำคัญที่ควรทำก่อน-หลัง อัปเดตทุกครั้งที่มีการสร้างหรือแก้ไขเอกสาร requirement ฉบับใดฉบับหนึ่ง

ดู [[feature-list|Feature List]] สำหรับรายละเอียด feature ย่อยพร้อมลำดับความสำคัญแบบ MoSCoW ที่แตกมาจากตารางนี้

| ลำดับ | หัวข้อ | เอกสารอ้างอิง | ความสำคัญ | สถานะ | อัปเดตล่าสุด |
|---|---|---|---|---|---|
| 1 | ลูกค้าสั่งอาหาร/เครื่องดื่มเองผ่านการสแกน QR code ที่โต๊ะ (รวม data retention/PDPA สำหรับ order log — ส่วนนี้ priority กลาง) | [[01-spec/20260802-01-table-qr-self-order|20260802-01-table-qr-self-order]] | สูง | Draft | 2026-08-02 |
| 2 | การแจ้งเตือนครัว/บาร์ และการจัดการออเดอร์หลังยืนยัน (สลิปพิมพ์อัตโนมัติ, สถานะออเดอร์, แก้ไข/ยกเลิกก่อนครัวรับทราบ, เปิด-ปิดขายเมนู) | [[01-spec/20260815-01-kitchen-notification-order-handling|20260815-01-kitchen-notification-order-handling]] | สูง | Draft | 2026-08-15 |
| 3 | การแจ้งเตือนสถานะออเดอร์กลับไปยังลูกค้าแบบ real-time บนหน้าจอที่ใช้สั่งอาหาร | [[01-spec/20260816-01-order-status-notification-to-customer|20260816-01-order-status-notification-to-customer]] | สูง | Draft | 2026-08-16 |
| 4 | รองรับการชำระเงินผ่าน QR payment ที่เคาน์เตอร์ (นอกเหนือจากเงินสด) | [[01-spec/20260816-02-counter-qr-payment|20260816-02-counter-qr-payment]] | กลาง | Draft | 2026-08-16 |
| 5 | Back-office จัดการเมนู/ราคา (CRUD พื้นฐาน: เพิ่ม/แก้ไข/ลบชื่อ, ราคา, หมวดหมู่) สำหรับพนักงาน/เจ้าของร้าน — ไม่รวมนับสต๊อก | [[01-spec/20260816-03-back-office-menu-price-crud|20260816-03-back-office-menu-price-crud]] | กลาง | Draft | 2026-08-16 |
