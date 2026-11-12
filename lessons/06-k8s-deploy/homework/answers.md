1. השמות ש-Compose מייצר תלויים בשם הפרויקט, ולכן הגדרנו שמות קבועים שישמשו את Kubernetes. ההגדרה imagePullPolicy: Never מבטיחה שימוש ב-Image המקומי בלבד, ללא ניסיון להוריד אותו מ-Registry. וללא זה הוא היה מנסה להוריד.

2. [Text.Encoding]::UTF8.GetString([Convert]::FromBase64String("c2hvcHBhc3M=")) 
הפקודה מחזירה shoppass. מכאן ניתן להבין ש-Base64 הוא קידוד ולא הצפנה, ולכן יש להגן על ה-Secret באמצעות הרשאות מתאימות.

3. הקובץ postgres-pvc.yml מגדיר אחסון קבוע שאליו מתחבר ה-Pod החדש, ולכן הנתונים נשמרים גם לאחר מחיקת ה-Pod הישן.

4. ה-Backend עלול להיכשל בחיבור למסד הנתונים עד שהוא יהיה זמין. ה-readinessProbe בודק אם ה-Backend מוכן, וכל עוד הבדיקה נכשלת, ה-Service אינו מעביר אליו בקשות.

5. לאחר שינוי ה-Selector ל-app: backnd, התקשורת עם ה-Backend נכשלה והנורית הפכה לאדומה. ה-Pods המשיכו לרוץ, אך ה-Endpoints היו ריקים משום שה-Selector לא התאים ל-Labels שלהם.

6. שתני הסביבה נקראים בעת הפעלת הקונטיינר, ולכן שינוי ה-ConfigMap אינו מעדכן אוטומטית קונטיינרים שכבר רצים.
הפתרון הוא יצירת Pods חדשים באמצעות:
kubectl rollout restart deployment/backend -n shoplist

7. ב-12 הקבצים המקוריים שם ה-Namespace shoplist מוגדר 12 פעמים. כדי ליצור עותק נוסף ב-Namespace אחר, נדרש לשנות את הגדרת ה-Namespace בכל 12 הקבצים, ובפרויקט הסופי גם בקובץ ה-ConfigMap הנוסף.