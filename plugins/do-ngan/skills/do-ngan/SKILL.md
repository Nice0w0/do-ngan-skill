---
name: do-ngan
description: ใช้เมื่อผู้ใช้พูดถึงดูงาน (do-ngan.com) หรือบอร์ดงานของทีมในเรื่องใดก็ตาม — งาน โปรเจค โน้ต แผ่นงานย่อย การแจ้งเตือน ประวัติ บริษัท/ทีม/พนักงาน เป็นแผนที่รวม tool ทั้งหมดและกติกากลาง
---

# ดูงาน (Do Ngan)

บอร์ดติดตามงานทีม เชื่อมผ่าน MCP server `do-ngan` (https://do-ngan.com/mcp) ทำงานในนามบัญชีของผู้ใช้
สิทธิ์เท่ากับบนหน้าเว็บทุกอย่าง — เห็นและแก้ได้เท่าที่ผู้ใช้ทำได้บนเว็บ

## ก่อนเริ่ม

- ถ้า tool ของ `do-ngan` ไม่มีให้เรียก หรือตอบว่ายังไม่ได้ยืนยันตัว ให้บอกผู้ใช้ล็อกอิน:
  - Claude Code: พิมพ์ `/mcp` → เลือก `do-ngan` → Authenticate
  - Codex: รัน `codex mcp login do-ngan`
  แล้วล็อกอินด้วยบัญชีดูงาน กด "อนุญาต" ในหน้าขอสิทธิ์
- ไม่แน่ใจว่าเป็นใคร/อยู่บริษัทไหน → `whoami`

## คำศัพท์

| คำที่ผู้ใช้พูด | ในระบบ |
|---|---|
| งานรูทีน | `lane=routine` (ค่าเริ่มต้น) |
| งานจิกปะทะ / งานด่วนแทรก | `lane=urgent` |
| ยังไม่เริ่ม / กำลังทำ / รอรีวิว / เสร็จ | `open` / `doing` / `review` / `done` |
| ด่วนมาก / ด่วน / ปกติ / ไม่รีบ / ทำเมื่อว่าง | priority `hotfix` / `urgent` / `normal` / `low` / `someday` |
| กล่องพนักงาน | employee — แบบ `lanes` (รูทีน+จิกปะทะ) หรือ `queue` (คิวเดียว บังคับเลนรูทีน) |
| ผู้ถือหลัก / ผู้ถือร่วม | คนแรกใน `assignees` / คนที่เหลือ |
| กอง | งานที่ยังไม่มีผู้รับ รอคนหยิบ · โฟลเดอร์ = ที่เก็บงานไม่มีผู้รับแบบไม่ลอย |
| พิมพ์เขียว | ลำดับงานในโปรเจค |

## กติกากลาง (ใช้กับทุกเรื่อง)

- ระบุคน โปรเจค ทีม บริษัท ด้วยชื่อได้เลย (`me` = ตัวผู้ใช้เอง) ถ้าชื่อซ้ำ tool จะคืนตัวเลือกพร้อม id → **ถามผู้ใช้** ห้ามเดาเลือกเอง
- งานและโน้ตต้องอ้างด้วย id — หาด้วย `list_tasks` / `my_tasks` / `get_board` / `list_notes` ก่อนเสมอ
- วันที่เป็นเวลาไทย (Asia/Bangkok) รูปแบบ `YYYY-MM-DD` · แปลง "พรุ่งนี้" "ศุกร์นี้" เป็นวันที่จริงก่อนส่ง
- ส่ง `null` เพื่อล้างค่า (เช่น ลบวันครบกำหนด) · ฟิลด์ที่เป็นรายการ (assignees, members, links, editors) = **แทนที่ทั้งชุด** ต้องใส่ของเดิมที่จะเก็บไว้ด้วย
- **ปิดงาน = `review` (รอรีวิว) เสมอ** ใช้ `done` เฉพาะเมื่อผู้ใช้สั่งชัด ๆ ว่าให้เป็นเสร็จ/เสร็จสิ้น/ปิดงาน หรือคนตรวจบอกว่าผ่านใน `review-queue` — ใช้กับแถวในแผ่นงานด้วย
- **ยืนยันกับผู้ใช้ก่อน**: `delete_task` (กู้คืนไม่ได้), `switch_company` (หน้าเว็บสลับตาม), เปลี่ยนผู้ถือหรือสมาชิกทั้งชุด
- ตอบผู้ใช้เป็นภาษาไทย สั้น ไม่แสดง id เว้นแต่ถาม · ห้ามแต่งข้อมูลที่ไม่มีในผลของ tool

## แผนที่ tool → skill ที่ใช้

| เรื่อง | tool | skill |
|---|---|---|
| ตัวเอง / บริษัท / สลับบริษัท | `whoami` `list_companies` `switch_company` | `company` |
| ทีม / พนักงาน / ภาพบอร์ด / ค้นงาน | `list_teams` `list_employees` `get_board` `list_tasks` `get_task` | `board` |
| งานของฉัน | `my_tasks` `list_notifications` | `today` |
| สร้างงาน | `parse_task_text` `create_task` | `create-task` |
| แก้ / มอบหมาย / สถานะ / % / ลบ | `update_task` `assign_task` `set_task_status` `set_task_progress` `delete_task` | `update-task` |
| เตือน / งานทำซ้ำ | `set_task_reminder` | `reminder` |
| ลงงานจากโค้ดที่เพิ่งทำ | (รวมหลายตัว) | `log-work` |
| โปรเจค | `list_projects` `get_project` `create_project` `update_project` `set_project_progress` `add_task_to_project` | `projects` |
| โน้ตบนบอร์ดทีม | `list_notes` `create_note` `update_note` | `notes` |
| แผ่นงานย่อย (ตาราง) | `read_sheet` `update_sheet` | `sheet` |
| ซิงก์รายการจากไฟล์ในโค้ด → แผ่นงาน | `read_sheet` `update_sheet` + git | `sync-sheet` |
| จัดแผ่นงานให้เป็นระเบียบ | `read_sheet` `update_sheet` | `tidy-sheet` |
| คนตรวจไล่ดูงาน/แถวที่รอรีวิว แล้วกดผ่าน | `list_tasks` `read_sheet` `set_task_status` `update_sheet` | `review-queue` |
| แจ้งเตือนในกระดิ่ง | `list_notifications` `mark_notifications_read` | `notifications` |
| ใครทำอะไรเมื่อไร | `get_activity` | `activity` |
| สรุปภาพรวม | `summarize_work` `get_activity` | `standup` |
