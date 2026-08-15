---
name: backlog-to-feature-journey
description: Audit docs/01-requirements/backlog.md against docs/01-requirements/01-spec/*.md, then create or update docs/01-requirements/feature-list.md (a MoSCoW-prioritized feature list) and docs/02-design/01-prototypes/user-journey-self-order.md (a Mermaid flowchart of the self-order journey mapped back to requirements) for the coffee-shop self-order project (โจทย์ 1). Use whenever the user wants the feature list or user journey generated/refreshed from the current backlog/spec state — ask clarifying questions (with at least 3 concrete options each) only when something is genuinely ambiguous, otherwise proceed with documented defaults.
---

# Backlog → Feature List + User Journey

Skill นี้อ่านสถานะปัจจุบันของ Product Backlog + เอกสาร requirement ทั้งหมด แล้ว **ตรวจสอบความสอดคล้อง (audit)** ก่อนสร้าง/อัปเดต 2 เอกสารถัดไปในสาย: Feature List (พร้อม MoSCoW) และ User Journey (Mermaid diagram)

หมายเหตุเรื่อง path: เอกสารทั้งหมดอยู่ใต้ `docs/` — เมื่อเอกสารนี้พูดถึง `01-requirements/...`, `02-design/...` หรือ `05-log/...` หมายถึง `docs/01-requirements/...`, `docs/02-design/...`, `docs/05-log/...` ตามลำดับ

## ขั้นตอน

### 1. อ่านสถานะปัจจุบันทั้งหมด

อ่าน `01-requirements/backlog.md`, ทุกไฟล์ใน `01-requirements/01-spec/*.md`, และถ้ามีอยู่แล้ว: `01-requirements/feature-list.md` กับ `02-design/01-prototypes/user-journey-*.md`

### 2. ตรวจสอบ (Audit) ความสอดคล้อง

เทียบ backlog/spec ปัจจุบันกับเอกสาร feature-list.md/user-journey ที่มีอยู่แล้ว (ถ้ามี) หาจุดที่ drift ออกจากกัน:

- **Spec ใหม่ที่ยังไม่มีแถวใน feature-list.md** → ต้องเพิ่ม
- **Feature ใน feature-list.md ที่ spec ต้นทางถูกย้ายไป `00-archived/` แล้ว** → ต้อง mark สถานะ "Deprecated" ห้ามลบแถวทิ้ง (ตาม convention ของ vault นี้ที่ห้ามลบเอกสาร ให้ archive แทน)
- **ขั้นตอนใน user-journey ที่ไม่ตรงกับ user story ปัจจุบันของ spec แล้ว** (เช่น spec ถูกแก้ไข business rule หลังจากวาด journey ครั้งล่าสุด)

จดสรุปผล audit ไว้เพื่อส่งต่อให้ subagent ทั้งสองใช้ประกอบการเขียน

### 3. ถามเมื่อไม่แน่ใจ — ต้องมีตัวเลือกอย่างน้อย 3 แนวทางเสมอ

ถามเฉพาะจุดที่กำกวมจริงๆ เท่านั้น (อย่าถามเรื่องที่ตัดสินใจได้เองอย่างสมเหตุสมผล) เช่น:

- 1 spec doc อธิบายหลาย sub-flow ไม่ชัดว่าควรนับเป็นกี่ feature ในตาราง → ["แยกเป็นหลาย feature ตาม user story", "รวมเป็น feature เดียวเพราะเป็น flow ต่อเนื่องกัน", "แยกเฉพาะ sub-flow ที่มี priority ต่างกันชัดเจน"]
- Priority ในตาราง backlog ขัดกับเนื้อหา Scope ของ spec เอง (เช่น backlog บอกสูง แต่ spec ระบุว่าเป็น out-of-scope สำหรับเวอร์ชันนี้) → ["ยึดตาม backlog (สูง) แล้วตั้งข้อสังเกตไว้ในหมายเหตุ", "ยึดตามเนื้อหา spec (out-of-scope → Won't) แล้วแจ้งว่า backlog อาจต้องแก้", "ถามผู้ใช้ก่อนว่าอันไหนคือค่าที่ถูกต้อง"]
- พบขั้นตอนใน journey ที่ไม่มี spec ไหนรองรับเลย → ["ตัดขั้นตอนนั้นออกจาก diagram ไปก่อน", "ใส่ไว้ใน diagram แต่ flag เป็น Open Question", "หยุดรอจนกว่าจะมี spec รองรับก่อนค่อยวาด"]

ใช้ `AskUserQuestion` (ระบบเพิ่มตัวเลือก "Other" ให้เองอัตโนมัติ ไม่ต้องเพิ่มเอง) ถ้าไม่มีจุดกำกวม ให้ข้ามขั้นตอนนี้ไปเลย

### 4. มอบหมายให้ subagent ทั้งสองตัวเขียนเอกสารจริง (รันขนาน)

เรียก Agent tool 2 ครั้งในข้อความเดียวกัน (ขนาน ไม่มี dependency ระหว่างกัน):

**`feature-list-writer`** — ส่ง: เนื้อหา backlog.md + สเปกทั้งหมดแบบเต็ม (subagent ไม่มีบริบทของบทสนทนานี้), feature-list.md เดิมถ้ามี, ผล audit จากขั้นตอน 2, กติกา MoSCoW mapping (ดูหัวข้อ "กติกา MoSCoW" ด้านล่าง)

**`user-journey-writer`** — ส่ง: สเปกทั้งหมดแบบเต็ม (โดยเฉพาะ User Stories/Business Rules/Scope), journey doc เดิมถ้ามี, ผล audit จากขั้นตอน 2, ขอบเขต = วาดเป็น **1 master diagram** ครอบคลุม flow หลักของ self-order (ไม่แยกเป็น diagram ย่อยรายฟีเจอร์ในตอนนี้ เพราะ backlog ยังมี spec doc จำนวนน้อย — เพิ่ม diagram แยกได้เองในอนาคตเมื่อมี flow ที่ไม่เกี่ยวข้องกันจริงๆ เช่น flow ฝั่งพนักงาน/ครัว)

ทั้งสองต้องรันแบบรอผลลัพธ์ (`run_in_background: false`) เพราะต้องรายงานผู้ใช้ในเทิร์นเดียวกัน

### 5. เขียน log entry เอง (ไม่ให้ subagent เขียน)

รวบรวมสรุปที่ subagent ทั้ง 2 ตัวส่งกลับมา แล้ว**ตัว skill เองเป็นคนเปิด/อัปเดต** `05-log/{YYYYMMDD}-log.md` เพิ่ม entry เดียวที่สรุปการเปลี่ยนแปลงทั้งสองฝั่ง (ห้ามให้ subagent เขียน log เอง — เพราะ subagent 2 ตัวรันขนานกันและเขียนไฟล์เดียวกันพร้อมกันจะชนกัน)

```markdown
## อัปเดต Feature List และ User Journey จาก Backlog
- Feature List: [[../01-requirements/feature-list|feature-list.md]] ({สร้างใหม่ | อัปเดต}) — {สรุปสั้นๆ}
- User Journey: [[../02-design/01-prototypes/user-journey-self-order|user-journey-self-order.md]] ({สร้างใหม่ | อัปเดต}) — {สรุปสั้นๆ}
- Audit findings: {สรุป drift ที่เจอจากขั้นตอน 2 ถ้ามี ไม่มีก็เขียนว่า "ไม่พบ" }
```

(ห้ามลบ entry เดิมที่มีอยู่ในวันเดียวกัน ถ้าไฟล์ยังไม่มีให้สร้างพร้อม heading `# Log — {YYYY-MM-DD}` ก่อน)

### 6. รายงานผู้ใช้

สรุปให้ผู้ใช้ทราบเป็น markdown link ที่กดเปิดได้: path ของ feature-list.md, path ของ user-journey-self-order.md, path ของ log, และสรุป audit findings (ถ้ามี drift ที่เจอ ให้บอกด้วยว่าแก้ไขอะไรไปบ้าง)

## กติกา MoSCoW (ส่งให้ `feature-list-writer` ใช้)

Auto-map จากคอลัมน์ "ความสำคัญ" ใน backlog.md เป็นค่าเริ่มต้น: สูง→Must, กลาง→Should, ต่ำ→Could ปรับลดเป็น Should/Could/Won't ต่อ feature ย่อยได้เองเมื่อเนื้อหา Scope/Business Rules ของ spec เองระบุไว้ชัดว่าเป็นส่วนรอง/out-of-scope — ทุกแถวต้องมี "เหตุผล MoSCoW" กำกับเสมอ ห้ามถามผู้ใช้ ห้าม copy priority ระดับเอกสารมาใช้ตรงๆ โดยไม่พิจารณาเนื้อหาจริงของแต่ละ feature ย่อย

## กติกาตายตัวสำหรับ User Journey (ส่งให้ `user-journey-writer` ใช้)

**ห้ามวาด branch หรือ node ใดๆ ที่มีความหมายว่า "เรียกพนักงานมารับออเดอร์ / แจ้งพนักงานเพื่อสั่งอาหาร" เด็ดขาด** ไม่ว่าจะเป็น master diagram หรือ diagram เสริมในอนาคต เพราะขัดกับโจทย์ 1 ที่ต้องการให้ระบบ self-order **แทนที่** การเรียกพนักงานรับออเดอร์โดยสมบูรณ์ — พนักงานเข้ามาเกี่ยวข้องในภาพได้เฉพาะขั้นตอนที่ไม่ใช่การรับออเดอร์ (เช่น เสิร์ฟ, จัดเตรียมอาหาร) และต้องระบุชัดในคำอธิบายว่าไม่ใช่การรับออเดอร์
