# עיצוב דף אינטרנט לפרויקט נגרות

הקובץ הזה מגדיר איך נראה **כל** דף/ארטיפקט שנבנה לפרויקט נגרות. לא להמציא עיצוב חדש בכל פעם — לפתוח את הקובץ הזה ולבנות לפיו.

הדף נועד להיקרא **מהטלפון, בחוץ, עם כפפות ואבק** — ולכן: ניגודיות גבוהה, בלי צללים רכים, בלי טקסט זעיר, וכל מספר בגופן עם ספרות בעלות רוחב קבוע.

---

## 1. טוקנים — להעתיק כמו שהם

תמיד שלושת המצבים: `:root` נקי (בהיר), מדיה כהה עם המגן `:not([data-theme="light"])`, וגם `[data-theme="dark"]`. **אף צבע לא מוגדר רק בתוך בלוק כהה.**

```css
:root{
  --ground:#F4F7F2; --surface:#FFFFFF; --ink:#16261B; --muted:#5A6B5E; --line:#D6E0D6; --shade:#E9EFE8;
  --timber:#2F6B45;
  --alert:#A4473A;
  --p1:#2F6B45; --p2:#2F5E9E; --p3:#A86F12; --p4:#6D4C93; --p5:#0F6E73;
  --display:"Secular One","Rubik","Arial Hebrew",system-ui,sans-serif;
  --body:"IBM Plex Sans Hebrew","Arial Hebrew","Segoe UI",system-ui,sans-serif;
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){
    --ground:#0E1712; --surface:#15221A; --ink:#E4EEE6; --muted:#98AC9E; --line:#26392E; --shade:#1A2B21;
    --timber:#4FB87A;
    --alert:#E48F7F;
    --p1:#4FB87A; --p2:#7FA8E0; --p3:#E6B252; --p4:#B39AD6; --p5:#46B9B1;
  }
}
:root[data-theme="dark"]{
  /* אותם ערכים בדיוק כמו בבלוק הכהה למעלה */
}
```

**משמעות הצבעים:**

| טוקן | תפקיד |
|---|---|
| `--timber` | צבע המותג של הדף. ה־eyebrow, הפס בסרגל ההתקדמות, מסגרות החלקים בתרשים החיתוכים |
| `--alert` | **שמור אך ורק להערות בטיחות ולשער בדיקת המשטח.** לא להשתמש בו לשום דבר אחר, אחרת הבטיחות מפסיקה לבלוט |
| `--p1`…`--p5` | חמשת שלבי העבודה: פירוק · שיוף ומדידה · חיתוך · הרכבה · גימור. כל שלב מקבל את הצבע שלו בכותרת, בריבוע המפתח, בציר הממוספר ובתיבות הסימון |

אם הפרויקט לא מתחלק לחמישה שלבים — להשתמש רק בצבעים שצריך, לפי הסדר.

---

## 2. טיפוגרפיה

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Secular+One&family=IBM+Plex+Sans+Hebrew:wght@400;500;600;700&display=swap">
```

- **כותרות:** Secular One, `font-weight:400` תמיד (זה המשקל היחיד שיש לו), `text-wrap:balance`, `line-height:1.25`
- **גוף:** IBM Plex Sans Hebrew, 16px, `line-height:1.65`
- גדלים: `h1{clamp(2.1rem,4.6vw,3rem)}` · `h2{1.8rem}` · `h3{1.3rem}`
- **בלי הדגשות במשקל 800/900.** הדגשה בגוף הטקסט היא `font-weight:600`

---

## 3. מבנה בסיס

```css
*{box-sizing:border-box}
body{background:var(--ground);color:var(--ink);font-family:var(--body);font-size:16px;line-height:1.65}
.page{max-width:1040px;margin:0 auto;padding:52px 28px 96px}
section{padding-top:60px;display:grid;gap:20px}
.sec-head{display:grid;gap:8px}
```

העטיפה תמיד `<div class="page" dir="rtl" lang="he">`.

**פריסה ב־grid/flex עם `gap`** — לא margins בין אחים.

---

## 4. כללי הברזל של הסגנון

1. **בלי `border-radius`** על קופסאות, טבלאות, כרטיסים ותרשימים. פינות חדות. היוצאים מן הכלל היחידים: צ׳יפים (`999px`) והעיגולים הממוספרים בציר.
2. **בלי `box-shadow`.** בכלל. ההפרדה נעשית ב־`1px solid var(--line)`.
3. **בלי גרדיאנטים** ובלי טקסט עם גרדיאנט.
4. **בלי אימוג׳י** ככותרת חלק או כסמן סעיף.
5. קו מפריד בין תאי גריד נוצר מ־`gap:1px` עם `background:var(--line)` על המכל — לא מ־border על כל תא.
6. כל מספר: `font-variant-numeric:tabular-nums`.

---

## 5. רכיבים קבועים

### פתיח
`eyebrow` (‎.8rem · 600 · `letter-spacing:.06em` · בצבע `--timber`) ← `h1` ← `lede` (‎1.14rem · `--muted` · `max-width:64ch`).

### רצועת נתונים
גריד של 6 תאים (3 מתחת ל־900px, 2 מתחת ל־520px), מופרדים ב־`border-inline-start`, בתוך מסגרת אחת. כל תא: שם (‎.82rem · 600 · muted) ← מספר (גופן הכותרות · 1.9rem) ← יחידה (‎.8rem · muted).
מה להכניס: מידות חיצוניות · חומר גלם · מספר חלקים · ברגים · עלות · זמן.

### שער בדיקת המשטח
קופסה במסגרת `--alert` **לפני** החלק הראשון, תמיד. מכילה את הסבר חותמת ה־IPPC ושלושה תגי קוד: `HT` ו־`KD` בירוק, `MB` באדום.

### טבלאות
בתוך `.scroll{overflow-x:auto;border:1px solid var(--line);background:var(--surface)}` עם `min-width` על הטבלה. `thead` על רקע `--ground`. עמודות מספרים `text-align:end`. שורות קיבוץ ברקע `--ground` ובאותיות קטנות. שורת סיכום עם `border-top:2px solid var(--ink)`.

### תרשים חיתוכים
פס אופקי לכל קרש, והחלקים בתוכו ב־`flex:<אורך בס״מ>` — כך הרוחב בתרשים פרופורציונלי לאורך האמיתי. חלק = `color-mix(in srgb,var(--timber) 13%,var(--surface))`; פחת = פסים אלכסוניים ב־`--shade`, בלי טקסט.
תמיד לציין ליד התרשים שהמסור אוכל 2–3 מ״מ ושזה כבר כלול.

### תרשים חיבור
SVG מוטבע, מבט מלמעלה, `max-width:460px`. קרשים ב־`--shade`, חלקי העמוד ב־`--timber` שקוף, **ברגים כנקודות ב־`--alert`**, תוויות ב־`--muted` עם קווי הובלה דקים. לוודא שהתוויות נכנסות בתוך ה־`viewBox`.

### שלבי עבודה
לכל שלב עבודה: `border-top:1px solid var(--line)`, כותרת עם ריבוע מפתח בצבע השלב וצ׳יפ קצב.
בתוכו ציר אנכי: `border-inline-start:2px solid var(--c)`, ולכל שלב עיגול ממוספר (`content:attr(data-n)`) שיושב על הקו. שלב מסומן → העיגול מתמלא בצבע והטקסט מקבל קו חוצה.

### הערות
`border-inline-start:3px solid` על רקע `--surface`:
- `.warn` בצבע `--alert` — **אחרי כל שלב שיש בו סיכון, ספציפי לשלב ולא הערה כללית**
- `.hint` בצבע `--timber` — טיפ, אזהרה מטעות, או המלצה איפה לשים את חותמת הלוגו

### זוג קופסאות
גריד 2 עמודות עם `gap:1px` על רקע `--line`. לכותרת הקטנה בכל תא: ‎.74rem · 600 · muted. שימושים: סיכום מול הסבר, כלים שיש מול כלים שכדאי, שתי אפשרויות גימור.

### טעויות מתחילים
3 עמודות, כל אחת עם `border-top:2px solid var(--ink)`, כותרת בגופן הכותרות וטקסט ב־muted.

### פוטר
`border-top:1px solid var(--line)` · ‎.84rem · muted.

---

## 6. סימון שלבים

רשימת סימון עם סרגל התקדמות היא חלק מהדף, לא תוספת — בונים פרויקט תוך כדי קריאה.

- תיבות סימון מרובעות, ללא `appearance`, מתמלאות בצבע שלב העבודה
- מצב נשמר ב־`localStorage` בתוך `try/catch` בשני הכיוונים, והדף חייב להיראות תקין גם בלי אחסון
- כפתור איפוס
- לציין בפוטר שהסימונים נשמרים בדפדפן הזה בלבד

---

## 7. עברית ומספרים

- כל רצף לטיני או נוסחה בתוך עברית עוטפים ב־`<bdi dir="ltr">`: `HT`, `KD`, `MB`, `FFP2`, `D3`, `UV`, `EUR`, `IPPC`, `5×50`, `72×30×29`, `120 → 180`, `3–4`
- בלי זה המידות והקודים נשברים בכיוון הפוך והמשתמש חותך לפי מספר שגוי
- גרשיים עבריים: `ס״מ`, `מ״מ`, `מ״ר`, `סה״כ`
- מחירים בסוף, `₪` אחרי המספר, ותמיד לציין שזו הערכה

---

## 8. הדפסה

```css
@media print{
  body{background:#fff}
  .page{padding:0}
  .phase{break-inside:avoid-page}
  .prog,.tick{display:none}
}
```

מישהו ידפיס את זה ויתלה בסדנה.
