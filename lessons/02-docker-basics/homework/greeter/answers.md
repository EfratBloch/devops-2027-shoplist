docker build -t greeter:v1 .
docker run -d --name greeter -p 9000:8000 greeter:v1
-e GREETING="Shalom",`docker run -d --name greeter -p 9000:8000 -e GREETING="Shalom" greeter:v1
{"message": "Shalom, Rivka!"}
**יסומנו כ-CACHED:** השלבים המוקדמים ב-Dockerfile (כגון `FROM`, `WORKDIR` והתקנת חבילות כמו Flask), מפני שהקבצים או ההגדרות הקשורים אליהם לא עברו שינוי, ולכן דוקר חוסך זמן ומסתמך על זיכרון המטמון.- **ירוצו מחדש:** שלב העתקת הקוד (`COPY . .`) והשלבים שאחריו, מפני שדוקר מזהה שקובץ המקור (`app.py`) השתנה, מה שגורם לפקיעת תוקף ה-cache החל משלב זה ואילך.
