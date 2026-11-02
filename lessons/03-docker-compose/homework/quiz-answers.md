# בוחן שיעור 3: חיה מנדלמן

1. ב
2. ג
3. reverse proxy הוא שרת שעומד לפני ה-services, מקבל את הבקשות מהלקוח, מעביר כל בקשה ל-service המתאים ומחזיר ללקוח את התשובה.
4. ב
5. ה-backend מקבל `/products`, כי ה-`/` בסוף ה-`proxy_pass` ב-nginx.conf גורם ל-Nginx להוריד את הקידומת `/api` לפני שהוא מעביר את הבקשה.
6. כי רק Nginx מדבר עם ה-backend, דרך הרשת הפנימית של Compose. `ports:` נחוץ רק כדי להגיע מבחוץ, ובלעדיו אי אפשר להגיע ל-backend ישירות מהמחשב, וזה גם בטוח יותר.
7. `docker compose down` מוחק את ה-containers ואת הרשת. `down -v` מוחק גם את ה-volumes, כלומר גם את הנתונים.
8. ב
9. `depends_on` מחכה רק שה-container יתחיל, לא שמסד הנתונים מוכן לקבל חיבורים. צריך להוסיף healthcheck ל-postgres ו-`condition: service_healthy` ב-depends_on.
10. ב
11. הראשון הוא named volume: Docker יוצר ומנהל אותו, ומשמש לשמירת נתוני מסד הנתונים לאורך זמן. השני הוא bind mount: קובץ מהמחשב (מתיקיית הפרויקט) שממופה לתוך ה-container.
12. ב
13. `docker compose config`
14. כי הקובץ נשמר ב-Git וכל מי שרואה את הריפו רואה את הסיסמה. במקום זה שמים את הסיסמאות בקובץ `.env` (שמוחרג ב-`.gitignore`), ו-Compose קורא ממנו את המשתנים.
15. כי ב-nginx.conf יש הפניה לשמות `backend` ו-`stats`. כשמריצים לבד עם `docker run` אין רשת של Compose ואין שמות כאלה, אז Nginx לא מוצא אותם ונכשל בעלייה.
