# שיעור 3 - תשובות

## 1. כמה containers ירוצו? כמה images ייבנו?

ירוצו **2 containers**, אחד לכל service. כל אחד מהם הוא תוכנה נפרדת שרצה במקביל, עם GREETING ופורט משלו.

ייבנו **2 images** (שני שמות), כי לכל service יש `build` משלו. אבל שניהם נבנים מאותו Dockerfile ומאותם קבצים, אז בזכות ה-cache הבנייה השנייה לא בונה כלום מחדש. שני השמות מצביעים על אותו image בפועל, עם אותו IMAGE ID.

## 2. מה יחזירו הכתובות?

- `http://localhost:9001/hello/Rivka` מגיע ל-`greeter-he` ומחזיר:
  `{"message": "Shalom, Rivka!"}`
- `http://localhost:9002/hello/Rivka` מגיע ל-`greeter-en` ומחזיר:
  `{"message": "Hello, Rivka!"}`

התשובה נבנית לפי ה-GREETING של כל service, וחוזרת כ-JSON כי `app.py` משתמש ב-`jsonify`.

## 3. למה אי אפשר אותו פורט במחשב, אבל אפשר אותו פורט ב-container?

כל container הוא סביבת ריצה נפרדת עם רשת משלו (בידוד), ולכן פורט 8000 בתוך container אחד לא מתנגש עם פורט 8000 בתוך container אחר.

המחשב לעומת זאת הוא סביבה אחת, ופורט במחשב יכול להיות תפוס רק על ידי תהליך אחד. אם שני services ינסו לתפוס את אותו פורט במחשב, Docker לא יצליח להרים את השני ויציג שגיאה (`port is already allocated`). וגם אם זה היה אפשרי, בקשה שמגיעה לפורט לא הייתה יודעת לאיזה container ללכת.

## 4. מה מוסיפים כדי להוסיף greeter בצרפתית?

מוסיפים service חדש מתחת ל-`services:`, באותה רמת הזחה כמו `greeter-he` ו-`greeter-en` (לא `services:` חדש):

```yaml
  # שירות ברכה בצרפתית - מחזיר "Bonjour" ונגיש בפורט 9003 במחשב
  greeter-fr:
    build:
      context: ../../02-docker-basics/homework/greeter
    environment:
      GREETING: Bonjour
    ports:
      - "9003:8000"
```

מה נשאר אותו דבר: ה-`context` (אותו Dockerfile) והפורט בתוך ה-container (8000).
מה משתנה: שם ה-service, ערך ה-GREETING והפורט במחשב.

## 5. למה greeter לא צריך volume, ו-postgres כן?

greeter הוא **stateless**: הוא לא שומר שום מידע בין הפעלה להפעלה, רק מקבל בקשה ומחזיר תשובה. אם מוחקים את ה-container ומרימים חדש, לא הולך לאיבוד כלום.

postgres הוא **stateful**: הוא מסד נתונים ושומר מידע (למשל רשימות הקניות של ShopList) שחייב לשרוד בין הפעלות. כשמוחקים container, כל מה שהיה בתוכו נמחק. volume שומר את הנתונים **מחוץ** ל-container, כך שגם אם ה-container נמחק ונוצר מחדש, הנתונים נשארים.
