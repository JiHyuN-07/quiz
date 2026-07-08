# Ground School Quiz Site — คู่มือประจำโปรเจกต์

> ไฟล์นี้ Claude อ่านอัตโนมัติทุกครั้งที่เปิดโฟลเดอร์นี้ ทำให้ไม่ต้องอธิบายเว็บใหม่ทุกครั้ง
> เจ้าของ: ศิษย์การบิน ทอ. (เครื่อง CT-4E) กำลังเรียน Ground School — ใช้เว็บนี้ทบทวนสอบ

## เว็บนี้คืออะไร
เว็บรวม **สรุปบทเรียน + แบบทดสอบ** สำหรับ Private Pilot Ground School (อ้างอิง Jeppesen 2024 + เอกสาร ศบ.)
- Live: https://jihyun-07.github.io/quiz/
- Repo: https://github.com/JiHyuN-07/quiz.git (branch `main`)
- Host: **GitHub Pages** — push ขึ้น `main` แล้วเว็บอัปเดตอัตโนมัติภายใน ~1 นาที
- เป็นเว็บ **static HTML ล้วน** ไม่มี build step ไม่มี framework ไม่มี dependency — แต่ละหน้าคือไฟล์ `.html` เดี่ยวๆ ที่ inline CSS+JS ครบในตัว เปิดตรงๆ ในเบราว์เซอร์ได้เลย
- ภาษาเนื้อหา: ไทย (คำศัพท์เทคนิคการบินเป็นอังกฤษ) — `<html lang="th">` เสมอ
- ใช้บนมือถือเป็นหลัก (เปิดจาก Home Screen) → ทุกหน้าต้อง responsive

## โครงสร้างไฟล์
```
quiz-site/
├── index.html              ← หน้า HUB หลัก (3 คอลัมน์) จุดเริ่มของทุกอย่าง
├── .nojekyll               ← บอก GitHub Pages ไม่ต้อง process ด้วย Jekyll (ห้ามลบ)
├── CLAUDE.md               ← ไฟล์นี้
│
├── chapter2/3/4/5/6/8.html ← Quiz กดตอบ (Test Guide)
├── quiz-met.html           ← Quiz Meteorology
├── uprt-quiz.html          ← Quiz แบบเขียน keyword
├── emergency-quiz*.html    ← Emergency/Limit CT-4E
├── Quiz-Aeromed-152.html   ← โผเวชศาสตร์
│
├── summary-ch5a/5b/5c.html ← หน้าสรุปบท (Jeppeson Summarize)
├── summary-ch8a/8b.html
├── summary-rtf107.html
├── summary-uprt.html
└── Aviation-Medicine-Ground-School-101.html
```

## index.html = HUB (จุดสำคัญที่สุด)
หน้าแรกจัดเป็น 3 คอลัมน์ แต่ละหน้าย่อยต้องมี "การ์ด" ลิงก์อยู่ในคอลัมน์ที่ถูกต้อง:
| คอลัมน์ | class | สีการ์ด (tag) | ใส่อะไร |
|---|---|---|---|
| 📘 **Jeppeson Summarize** | `.summarize` | เหลือง-ส้ม | หน้าสรุปบทเรียน |
| 📝 **Test Guide** | `.testguide` | เขียว | Quiz กดตอบ + เฉลย |
| 🚨 **Emergency/Limit** | `.emergency` | แดง-ชมพู | ข้อสอบฉุกเฉิน/ข้อจำกัด CT-4E |

**ทั้ง 2 คอลัมน์เรียงการ์ดตามเลขวิชา (101 → 113)** — เพิ่มการ์ดใหม่ให้แทรกตามลำดับเลข tag เสมอ
รูปการ์ด 1 อัน:
```html
<a class="card" href="ชื่อไฟล์.html">
  <div class="tag">109</div>              <!-- เลขวิชา เช่น 101, 108-A (ดูกฎ tag ด้านล่าง) -->
  <div class="info">
    <div class="title">ชื่อหัวข้อ</div>       <!-- ดูกฎการตั้งชื่อ title ด้านล่าง -->
    <div class="sub">คำอธิบายย่อ · จำนวนข้อ</div>
  </div>
  <div class="arrow">›</div>
</a>
```

### กฎการตั้งชื่อการ์ด (สำคัญ — เจ้าของกำหนด)
- **tag (กล่องซ้าย)** = เลขวิชาเสมอ · ถ้าเป็น section ย่อยของบท Jeppesen ใช้ `เลข-A/B/C` (เช่น `106-A`, `108-B`)
- **title (ชื่อหัวข้อ)** ตั้งตามกฎนี้:
  1. **Quiz** (Ground Lesson Test) → ใช้ **ชื่อโฟลเดอร์วิชานั้น** (ตัดเลขหน้าออก) เช่น โฟลเดอร์ `109 Meteorology` → title `Meteorology`
  2. **สรุปจากบทหนังสือ Jeppesen** → ใช้ **ชื่อบท/section นั้น** (เช่น `ATC Services`, `Weight and Balance`)
  3. **สรุปจากไฟล์ในโฟลเดอร์ (ไม่ใช่ Jeppesen)** → ใช้ **ชื่อโฟลเดอร์** (ตัดเลขหน้าออก) เช่น `Radio Telephony and Morse Code`, `UPRT`
- ชื่อโฟลเดอร์วิชา (แม่) อยู่ที่ `../../<เลข ชื่อวิชา>/` เช่น `../../109 Meteorology/`

## Template หน้า 3 แบบ (ลอกจากไฟล์จริงเสมอ อย่าคิดใหม่)

### 1. Quiz กดตอบ = **Template A มาตรฐาน** — copy จาก `chapter5.html` (มีรูป) หรือ `chapter4.html` (ไม่มีรูป)
> ⚠️ มี template quiz อยู่ 2 แบบในประวัติเว็บ: **Template A** (ฟ้าอ่อน `#1a3c6e→#2d6abf`, `.choice-btn`, progress bar, end-screen) = **มาตรฐานที่ใช้ต่อไป** (101,103,105,106,108,109) · **Template B** (ฟ้าเข้ม `#1e3a5f→#2c5282`, `.options li`, feedback ใต้ข้อ, banner) = แบบเก่า **เลิกใช้แล้ว** (chapter6, และ chapter3/104 ที่ยังไม่ได้แปลง)
- **Data-driven**: แก้แค่ array `QUESTIONS` (หรือ `questions`) ไม่ต้องแตะ HTML/logic
- โครงสร้างข้อ: `{ s:"A", q:"คำถาม", o:["ตัวเลือก1","ตัวเลือก2","ตัวเลือก3"], a:0, ex:"คำอธิบายเฉลย", fig:5 }`
  - `s` = section, `q` = คำถาม (ใส่ HTML/รูปในตัวได้), `o` = ตัวเลือก (2–4 ข้อ), `a` = index คำตอบถูก (เริ่ม 0)
  - `ex` (บางไฟล์ใช้ `e`) = คำอธิบายเฉลย (optional) · `fig` = เลขรูป map เข้ากับ `FIGIMG` (chapter8 ใช้ `fig:[array]` หลายรูป/ข้อ + `given` = ข้อมูลที่โจทย์ให้)
- ฟังก์ชันหลัก Template A: `buildQuiz()` `answer()` `showEnd()` `restartQuiz()` (+ `openLightbox/closeLightbox` ถ้ามีรูป) — ปกติไม่ต้องแก้
- section: กำหนดชื่อใน object `SECTIONS`/`sections` (คีย์ต้องตรงกับ `s` ในข้อมูล — เป็น "A/B/C" หรือ "1/2" ก็ได้)
- **UI**: header + `#score-bar` sticky (Answered / progress bar / Correct / Score %) + `#end-screen` สรุปคะแนนแยก section เมื่อทำครบ
- อย่าลืมอัปเดตจำนวนข้อใน `<header>` และ `#total-count` ให้ตรงจำนวนจริง
- ภาษา UI: ไฟล์เนื้อหาอังกฤษ (Jeppesen/FAA) ใช้ UI อังกฤษ · ไฟล์เนื้อหาไทย (เช่น quiz-met) ใช้ UI ไทยได้

### 2. หน้าสรุป — copy จาก `summary-ch5a.html`
- ใช้ CSS variables ใน `:root` (พื้นสว่าง `--bg:#f4f7fb`)
- มี `nav.toc` (สารบัญ) → `<section>` แต่ละหัวข้อ
- helper classes: `.term` (คำศัพท์ไทยเน้น), `.en` (อังกฤษเอียง), `.key` (กล่องจุดสำคัญ), `.warn` (กล่องเตือนแดง), `.freq`/`.squawk` (ความถี่/code), `<details>` (เฉลยซ่อน/เปิด)

### 3. Quiz แบบเขียน keyword — copy จาก `uprt-quiz.html`
- ธีม navy (`--navy:#1e2761`) + ส้ม (`--accent:#f9a826`)
- แต่ละข้อมี `<details>` เปิดดู `.kw-box` (keyword ที่ต้องเขียนให้ติด) + `.model-ans` (คำตอบตัวอย่าง)

## ธีมสี (ให้ทุกหน้าดูเป็นชุดเดียวกัน)
- **น้ำเงินหลัก (Template A มาตรฐาน)**: header gradient `#1a3c6e` → `#2d6abf` · พื้นหน้า body `#f0f4f8` (สว่าง)
  - ⚠️ อย่าใช้ `#1e3a5f→#2c5282` (ฟ้าเข้มแบบเก่า Template B) กับ quiz กดตอบอีก
- ถูก = เขียว `#27ae60` / `#38a169` · ผิด = แดง `#e74c3c` / `#e53e3e`
- Accent ส้ม-เหลือง: `#f59e0b` / `#fbbf24`
- font stack: `"Segoe UI","Sarabun",Tahoma,sans-serif` (Sarabun รองรับไทย)
- การ์ด/กล่อง: `border-radius:12px`, เงานุ่ม, hover ยกขึ้น `translateY(-2px)`

## วิธีเพิ่มหน้าใหม่ (checklist)
1. **สร้างไฟล์** โดย copy template ที่ตรงประเภท (ดูด้านบน) แล้วแก้เนื้อหา
2. ตั้งชื่อไฟล์: quiz ใช้ `chapterN.html`/`ชื่อ-quiz.html`, สรุปใช้ `summary-xxx.html`
3. **เพิ่มการ์ดใน `index.html`** ในคอลัมน์ที่ถูก (Test Guide เรียงตามเลขวิชา)
4. อัปเดตจำนวนข้อ/ข้อความ header-footer ให้ตรง
5. เปิดไฟล์ในเบราว์เซอร์เช็คก่อน commit (กดตอบดูว่าเฉลยถูก, นับข้อครบ, ดูบนจอมือถือ)
6. **Deploy** (ดูด้านล่าง)

## วิธี Deploy (สำคัญ — เจ้าของอยากให้ง่าย)
เว็บอยู่บน GitHub Pages push แล้วขึ้นเอง ทำ 3 คำสั่งนี้จบ:
```bash
git add -A
git commit -m "ข้อความสั้นๆ บอกว่าทำอะไร"
git push origin main
```
- Commit message style ของ repo นี้: อังกฤษ, ขึ้นต้นด้วยกริยา, สั้น (เช่น `Add Chapter 8 quiz`, `Sort Test Guide cards by tag number`)
- **AUTO-DEPLOY (คำสั่งถาวรจากเจ้าของ):** ทุกครั้งที่แก้/เพิ่มอะไรกับเว็บนี้เสร็จ ให้ commit+push ขึ้น GitHub **ทันทีโดยไม่ต้องถาม** เจ้าของต้องการการทำงานที่ราบรื่น
- หลัง push บอกเจ้าของว่า "อัปแล้ว รอ ~1 นาทีแล้วรีเฟรช jihyun-07.github.io/quiz"
- ระวังรูปภาพขนาดใหญ่ (chapter5/8, summary-ch8 หนัก 1–2MB) → ถ้าเพิ่มรูป ให้ compress ก่อน เพื่อให้โหลดเร็วบนมือถือ

## แปลง PDF/สไลด์ → quiz (งานที่เจ้าของทำบ่อย)
มีไฟล์ต้นฉบับ (PDF โผข้อสอบ, PowerPoint บรรยาย) อยู่ในโฟลเดอร์วิชาแม่ `../..//<เลขวิชา ...>/`
เช่น `../../109 Meteorology/`, `../../101 Aviation Physiology/`
ขั้นตอน: อ่าน PDF/pptx → ดึงคำถาม-ตัวเลือก-เฉลย → แปลงเป็น array `QUESTIONS` → ใส่ใน template quiz กดตอบ
- ถ้าเอกสารเป็นอังกฤษ (Jeppesen) เก็บคำถามอังกฤษไว้ (ตรงกับข้อสอบจริง)
- ถ้าเป็นโผไทย/บรรยายไทย ทำเป็นภาษาไทย
- ตรวจเฉลยให้แน่ใจก่อนเสมอ — ผิดเฉลยคือปัญหาใหญ่สำหรับคนใช้ทบทวนสอบ

## กฎเหล็ก
- **commit+push อัตโนมัติทุกครั้งที่ทำงานเสร็จ โดยไม่ต้องถาม** (คำสั่งถาวรจากเจ้าของ — ต้องการงานที่ราบรื่น)
- ลอก template จากไฟล์จริง อย่าประดิษฐ์โครงใหม่ — ความสม่ำเสมอสำคัญกว่าความหวือหวา
- ทุกหน้าต้องเปิดได้แบบ standalone (inline CSS/JS, ไม่พึ่งไฟล์ภายนอก)
- เช็คบนมือถือเสมอ (เจ้าของใช้มือถือเป็นหลัก)
- ตอบเจ้าของเป็นภาษาไทย
