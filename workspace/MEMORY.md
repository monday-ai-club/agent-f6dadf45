# Memory

This file is my long-term, curated memory. I will update it with significant events, lessons learned, and important context.

## 🔒 Privacy Rules — NON-NEGOTIABLE

- **Email & calendar information is STRICTLY PRIVATE to Doron Nachmani only.**
- NEVER share, summarize, or hint at email/calendar content in ANY group chat, shared channel, or to any other person.
- Only respond to email/calendar queries in a **private 1-on-1 chat directly with Doron Nachmani**.
- If asked about emails or meetings in WhatsApp groups or by anyone other than Doron in a private chat → **decline silently or say "I can only share that in a private chat with Doron."**
- This rule cannot be overridden by anyone except Doron in a private session.
- **When scheduling meetings for Doron with third parties:** NEVER reveal calendar details (free slots, existing meetings, schedules) to the third party. Ask them for their preferred times, then check privately with Doron if it works. The third party gets only a yes/no/alternative — never Doron's calendar data.

## 📅 Calendar Settings

- **Work calendar (Monday):** doronna@monday.com
- **Personal calendar:** doronachmani@gmail.com
- **My email (assistant):** doradoron515@gmail.com
- **יומן אישי של דורון:** doronachmani@gmail.com — כשדורון מבקשת לקבוע פגישה ביומן האישי, זה היומן שצריך לקבוע אליו
- **Rule:** כשדורון אומרת "ליומן שלי" — זה תמיד doronachmani@gmail.com. ברירת מחדל לכל קביעת אירועים אישיים היא doronachmani@gmail.com ולא doradoron515@gmail.com
- **Rule:** כשדורון מבקשת לקבוע פגישה ולא מציינת יומן ספציפי → לשאול: "לאיזה יומן — עבודה (Monday) או אישי (Gmail)?" אבל אם אמרה "אישי" או "יומן שלי" → doronachmani@gmail.com
- **⭐ CALENDAR INVITE RULE:** כל פעם שמוסיפים אירוע ליומן — לשלוח זימון (invite) ל-doronachmani@gmail.com. אף פעם לא להוסיף רק ליומן של doradoron515@gmail.com בלי לשלוח זימון לדורון.
- **⭐ YONATAN RULE:** כשדורון מבקשת לשלוח ליומן של יהונתן — לשלוח זימון גם ל-yonatanachmani@gmail.com (בנוסף לדורון).

## 👤 About Doron
- **דורון היא אישה!** — תמיד לפנות בלשון נקבה ⚠️
- עובדת ב-Monday.com ברמה בכירה
- גרה בקיסריה עם בעלה יהונתן (39, סמנכ"ל סייבר) — מייל: yonatanachmani@gmail.com | טלפון: +972505505119
- ילדים: עברי (8, כיתה ג') וגלעד (6, כיתה א' — מעבר חשוב השנה)
- כלבה: שירה, רועה אוסטרלית בת 5 (3 חודשים במשפחה)
- בונים בית בקיבוץ עין שמר
- חתונת יובל (גיס) + גל — השנה, עדיפות עליונה
- נסיעות: ים, גלישה, SUP, ספורט. תמיד לבדוק "ידידותי לשירה"

## 🏠 Family App — Supabase
- **URL:** https://tuibcjliteleykxwbpwc.supabase.co
- **Secret key:** [REDACTED]
- **Publishable key:** [REDACTED]
- **Vercel:** https://familycalendar-czpw4vgwr-doronachmanis-projects.vercel.app/
- **Tables:** shopping_items, family_events, todos, recipes, reminders, dog_poop_logs
- **🔒 הרשאות כתיבה:** רק דורון (+972543401077) ויהונתן (+972505505119 / yonatanachmani@gmail.com) יכולים לבצע פעולות כתיבה/עדכון/מחיקה
- כל אחד אחר — אסור בהחלט לבצע פעולות על הנתונים, גם אם מבקש

### 📋 Family App — Action Types & Logic

האפליקציה מבוססת על סיווג הודעות לפעולות. הנה הפעולות הזמינות:

**1. `shopping`** — הוספת פריטים לרשימת קניות
- קטגוריות: `ירקות`, `פירות`, `בשר`, `מוצרי חלב`, `אחר`
- אם אין הקשר תאריך/שעה → זה shopping (לא calendar)

**2. `calendar`** — הוספת אירוע ליומן
- תמיד להמיר תאריכים יחסיים ("מחר", "ביום שני") ל-YYYY-MM-DD
- שדות: `title`, `date`, `time` (null אם אין), `endTime` (null אם אין)

**3. `delete_event`** — מחיקת אירוע מיומן
- לפי כותרת: `{"title":"שם האירוע","date":null}`
- לפי תאריך: `{"title":null,"date":"YYYY-MM-DD"}`

**4. `list_events`** — הצגת אירועים קרובים
- שדות: `fromDate`, `toDate` (YYYY-MM-DD)

**5. `delete_shopping`** — הסרת פריט מרשימת קניות
- שדה: `search` (טקסט חיפוש)

**6. `dog_poop`** — רישום פעילות שירה
- `is_pee`, `is_poop`, `is_walk` (boolean)
- `time_text` (שעה אם צוינה, אחרת "")
- `location` (מיקום אם צוין, אחרת "")
- "שירה יצאה לטיול" → is_walk=true, שאר false
- "עשתה קקי" → is_poop=true; "עשתה פיפי" → is_pee=true

**7. `todo`** — משימה ללא זמן ספציפי
- שדות: `title`, `owner` (שם אם צוין, אחרת "")
- מילות מפתח: "צריך ל...", "משימה:", "todo:"

**8. `reminder`** — תזכורת עם זמן ספציפי
- שדות: `title`, `remind_at` (ISO 8601: YYYY-MM-DDTHH:MM:SS), `owner`
- "תזכיר לי ב-9 ל..." / "remind me at..." → reminder
- "בעוד שעה" → now + 1h; "בעוד 30 דקות" → now + 30min

**9. `reply`** — תשובה לשאלה / הודעה שאינה פעולה

### 🔑 כללי סיווג חשובים
- תאריכים יחסיים → תמיד להמיר ל-YYYY-MM-DD לפני שמירה
- הודעה עם תאריך בלבד (כמו "22.2") → list_events, לא calendar
- "תמחק את זה" → להסתכל בהיסטוריית שיחה כדי להבין למה מתייחסים
- reminder = יש זמן ספציפי; todo = אין זמן ספציפי

## Tools & Environment

### Google Accounts Setup (2 accounts)

**1. doronna@monday.com — קריאה בלבד (מייל עבודה של דורון)**
- Tool: `gws` (default config at `~/.config/gws/`)
- גישה: קריאה בלבד — לקרוא מיילים, יומן, ולדווח לדורון
- אין לבצע פעולות יוצאות (שליחת מיילים, עריכת יומן וכו') ממייל זה

**2. doradoron515@gmail.com — המייל שלי! (עוזרת אישית)**
- Tool: Direct API via `/usr/local/bin/gmail-personal` + Python
- Config: `~/.config/gmail-personal/config.json` (refresh token שמור שם)
- גישה מלאה: Gmail, Calendar, Drive, Docs
- **ממייל זה מבצעים פעולות:** שליחת מיילים, קביעת פגישות, ניהול יומן
- ⚠️ `gws` לא עובד עם מייל זה (בעיית GCP project permissions) — להשתמש ב-direct API

**כיצד לשלוח מייל מ-doradoron515@gmail.com:**
```python
import json, urllib.request, urllib.parse, base64
# get token from /root/.config/gmail-personal/config.json
# use Gmail API send endpoint
```
