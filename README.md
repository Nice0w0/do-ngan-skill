# do-ngan-skill

ปลั๊กอิน [ดูงาน](https://do-ngan.com) สำหรับ **Claude Code** และ **Codex** — ให้ AI ในเทอร์มินัลอ่านและจัดการบอร์ดงานของทีมแทนคุณ
ทำงานในนามบัญชีดูงานของคุณเอง เห็นและแก้ได้เท่าที่คุณทำได้บนหน้าเว็บ

มีให้ในชุด:

- **MCP server** `do-ngan` → `https://do-ngan.com/mcp` (ล็อกอินด้วยบัญชีดูงานผ่าน OAuth ไม่ต้องใช้ API key)
- **Skills**
  - `do-ngan` — คำศัพท์และกติกาของบอร์ด (AI หยิบใช้เองเมื่อคุณพูดถึงงาน)
  - `today` — งานของฉันวันนี้ อะไรเลยกำหนด ควรทำอะไรก่อน
  - `log-work` — อัปเดตงานบนบอร์ดจากสิ่งที่เพิ่งเขียนโค้ด (ตั้งเป็นรอรีวิว เสร็จสิ้นเฉพาะเมื่อคุณสั่ง)
  - `standup` — สรุปงานของฉัน/ของทีมเป็นข้อความพร้อมส่ง

## Claude Code

```sh
claude plugin marketplace add Nice0w0/do-ngan-skill
claude plugin install do-ngan@do-ngan
```

หรือพิมพ์ใน Claude Code: `/plugin marketplace add Nice0w0/do-ngan-skill` แล้ว `/plugin install do-ngan@do-ngan`

จากนั้นล็อกอิน: พิมพ์ `/mcp` → เลือก `do-ngan` → **Authenticate** → ล็อกอินบัญชีดูงาน → กด **อนุญาต**

เรียก skill ตรง ๆ ได้ด้วย `/do-ngan:today`, `/do-ngan:log-work`, `/do-ngan:standup`

## Codex

```sh
codex plugin marketplace add Nice0w0/do-ngan-skill
codex plugin add do-ngan@do-ngan
codex mcp login do-ngan
```

## ลองพิมพ์

- วันนี้ฉันมีงานอะไรบ้าง
- ให้ปอนด์แก้เซิร์ฟเวอร์ล่ม ก่อนเที่ยง ด่วนมาก
- ลงงานที่เพิ่งทำให้หน่อย
- สรุปงานทีมสัปดาห์นี้

## อัปเดต

```sh
claude plugin marketplace update do-ngan && claude plugin update do-ngan@do-ngan   # Claude Code
codex plugin marketplace upgrade do-ngan     # Codex
```

## ถอนสิทธิ์

ดูงาน → ตั้งค่า → **เชื่อมต่อ AI ภายนอก** → ถอนสิทธิ์ หรือถอดปลั๊กอินด้วย
`claude plugin uninstall do-ngan@do-ngan` / `codex plugin remove do-ngan@do-ngan`

---

รีโปนี้มีแค่ไฟล์ตั้งค่าปลั๊กอินและ skill ตัวระบบดูงานไม่ได้อยู่ที่นี่
