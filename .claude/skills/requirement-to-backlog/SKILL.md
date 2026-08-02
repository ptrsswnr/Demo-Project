---
name: requirement-to-backlog
description: Turn a raw/informal requirement or feature idea for the coffee-shop self-order project (โจทย์ 1) into a formal requirement document under docs/01-requirements/01-spec/, update docs/01-requirements/backlog.md, and log the change. Use whenever the user gives a raw requirement and wants it captured as documentation — ask clarifying questions (with at least 3 concrete options each) whenever anything is ambiguous, and check whether it should extend an existing spec doc instead of creating a new one.
---

# Requirement → Backlog

Skill นี้รับ requirement ดิบจากผู้ใช้ แล้วแปลงเป็นเอกสาร requirement ทางการ + อัปเดต Product Backlog + บันทึก log ให้ครบตามขั้นตอน

หมายเหตุเรื่อง path: เอกสารทั้งหมดอยู่ใต้ `docs/` — เมื่อเอกสารนี้พูดถึง `01-requirements/...` หรือ `05-log/...` หมายถึง `docs/01-requirements/...` และ `docs/05-log/...` ตามลำดับ

## ขั้นตอน

### 1. ทำความเข้าใจ requirement ดิบ

อ่าน requirement ที่ผู้ใช้ให้มา แล้วเทียบกับโจทย์หลักของโปรเจกต์ (โจทย์ 1: ลูกค้าสั่ง Order เองจากที่โต๊ะ — ดู CLAUDE.md หัวข้อ "Project context") ถ้า requirement ไม่เกี่ยวกับโจทย์นี้เลย ให้ตั้งข้อสังเกตกับผู้ใช้ก่อนดำเนินการต่อ แทนที่จะเดาเองว่ายังอยู่ในเฟสนี้

### 2. ตรวจสอบเอกสารเดิม

ใช้ Glob/Grep สแกน `docs/01-requirements/01-spec/*.md` ว่ามีเอกสารที่ครอบคลุมหัวข้อเดียวกันอยู่แล้วหรือไม่ หรือ requirement ดิบมีการอ้างอิงถึงเอกสารที่มีอยู่แล้ว ถ้าเจอ ให้วิเคราะห์ว่า:

- เป็นเรื่องเดียวกันจริงๆ ที่ควร **แก้ไขเอกสารเดิม** (เพิ่ม user story, ขยาย scope ฯลฯ), หรือ
- เป็นฟีเจอร์คนละเรื่องที่บังเอิญเกี่ยวข้องกัน ควร **สร้างเอกสารใหม่** แล้วอ้างอิงกันด้วย wikilink

ถ้าวิเคราะห์แล้วไม่มั่นใจ ให้รวมเป็นคำถามในขั้นตอนที่ 3

### 3. ถามเมื่อไม่แน่ใจ — ต้องมีตัวเลือกอย่างน้อย 3 แนวทางเสมอ

ถ้ามีส่วนไหนของ requirement ที่ไม่ชัดเจน (scope, priority, สร้างเอกสารใหม่ vs แก้ไขเอกสารเดิม, business rule ที่ขาดหาย) ให้ใช้ AskUserQuestion โดย**ทุกคำถามต้องมีตัวเลือกอย่างน้อย 3 แนวทางให้เลือก** (ระบบเพิ่มตัวเลือก "Other" ให้เองอัตโนมัติอยู่แล้ว ไม่ต้องเพิ่มเอง)

ตัวอย่างคำถาม:

- "ควรสร้างเอกสารใหม่ หรือแก้ไขเอกสารเดิม `{doc}`?" → ["สร้างเอกสารใหม่แยกต่างหาก", "แก้ไข/เพิ่มเติมในเอกสารเดิม", "รวมเป็น section ใหม่ในเอกสารเดิมแต่แยก user story"]
- "ควรรองรับช่องทางการชำระเงินแบบไหนบ้าง?" → ["เงินสดอย่างเดียว", "เงินสด + พร้อมเพย์/QR", "เงินสด + พร้อมเพย์ + บัตรเครดิต"]
- "requirement นี้ควรมีความสำคัญระดับไหนใน backlog?" → ["สูง (กระทบ core flow การสั่งของลูกค้าโดยตรง)", "กลาง (เสริมประสบการณ์แต่ไม่ block การสั่งอาหาร)", "ต่ำ (nice-to-have ทำทีหลังได้)"]

ถามให้ครบก่อนไปขั้นตอนถัดไป — อย่าเดาเองในส่วนที่กระทบ scope หรือการตัดสินใจสร้าง/แก้ไขเอกสาร

### 4. มอบหมายให้ subagent `requirement-writer` เขียนเอกสารจริง

เมื่อ requirement ชัดเจนครบแล้ว:

1. หาวันที่ปัจจุบันในรูปแบบ YYYYMMDD (Bash: `date +%Y%m%d` หรือ PowerShell: `Get-Date -Format yyyyMMdd`)
2. เรียก Agent tool ด้วย `subagent_type: "requirement-writer"` พร้อม prompt ที่ประกอบด้วย:
   - เนื้อหา requirement ที่ clarify แล้วแบบละเอียด (subagent ไม่มีบริบทของบทสนทนานี้ ต้องเขียนให้ครบ ไม่ใช่แค่สรุปสั้นๆ)
   - วันที่ (YYYYMMDD) ที่คำนวณไว้
   - การตัดสินใจ: สร้างเอกสารใหม่ หรือแก้ไขเอกสารเดิม (ระบุ path เอกสารเดิมถ้ามี)
   - priority ที่ตกลงกับผู้ใช้แล้ว
3. เรียกแบบรอผลลัพธ์ (`run_in_background: false`) เพราะต้องรายงานผลให้ผู้ใช้ในเทิร์นเดียวกัน

### 5. รายงานผลผู้ใช้

สรุปให้ผู้ใช้ทราบ: path เอกสารที่สร้าง/แก้ไข, แถว backlog ที่อัปเดต, path ของ log — ใส่เป็น markdown link ให้กดเปิดได้
