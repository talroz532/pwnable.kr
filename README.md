# pwnable.kr
## fd

זהו מדריך כיצד לפתור את האתגר הראשון-fd


![Capture1](https://user-images.githubusercontent.com/67608539/144739918-5ec4eda8-a12b-4def-b77f-9704ca84ea0d.PNG)

נעתיק את קוד ה 'ssh' לטרמינל של לינוקס, אני משתמש ב- 'ubuntu-20.04.3'
ומיד לאחר מכן את הסיסמא- 'guest' כפי שמצויין.

![Capture2](https://user-images.githubusercontent.com/67608539/144739990-ee683c1e-22fd-4d91-bca2-c34611f46f69.PNG)


כעת, פעולה ראשונה שנעשה היא לבדוק אילו קבצים נמצאים, נכתוב את הפקודה 'ls' על מנת לראות אילו ספריות וקבצים יש במיקום

![Capture3](https://user-images.githubusercontent.com/67608539/144740069-b1909b2c-b7f3-4f22-ac1e-006e64dcfc32.PNG)


ישנם שלושה קבצים, אחד מהם קובץ C.
המטרה שלנו להגיע ל 'flag' מסויים, 
ניתן לראות שיש קובץ 'flag' ננסה לראות מה יש בו באמצעות הפקודה 'cat' 
על מנת לראות את תוכנו של הקובץ
![Capture9](https://user-images.githubusercontent.com/67608539/144740230-027723f7-953c-4d1d-9fe3-896ceb65cef2.PNG)

```bash
cat: flag: permission denied```
אין לנו מספיק הרשאות כדי להיכנס לקובץ.

ננסה להריץ עכשיו את הפקודה 'cat' על הקובץ 'fd.c'
![Capture4](https://user-images.githubusercontent.com/67608539/144740294-e9bb5014-e7b2-436d-bee1-efee4873d38f.PNG)

ניתן לראות שהתוכנית אמורה לקבל ארגומנט כלשהו, ננסה להריץ מספר ניסיונות.

![Capture5](https://user-images.githubusercontent.com/67608539/144743474-d17bbbcc-886f-4257-8ee4-aacd55a227ef.PNG)
כאשר לא מזינים ארגומנט כלשהו, נקבל הודעה שאמור להיות ארגומנט.

ננסה שנית אך הפעם נזין ארגומנט כלשהו

![Capture6](https://user-images.githubusercontent.com/67608539/144743499-c5ba09c1-913d-4d32-9ee9-f0b84250bd0c.PNG)
ניתן לראות שאנחנו מקבלים הודעה שונה מהקודמת.

נסתכל על השורה הזאת בקוד- '''int fd = atoi( argv[1] ) - 0x1234;'''

 נמיר את '0x1234' למספר דצימלי.
 נקבל את המספר 4660.
 נשים כארגומנט את המספר 4660 כך שפעולת החיסור תיתן לנו-0.
 ```bash
 int fd = 0;```
 
 ניתן לראות שבניגוד לניסיונות הקדומים, כאן אנו מקבלים אופציה לקלט, ננסה להזין את המילה שמושוואת בתנאי- '"LETMEWIN"'
 ![Capture7](https://user-images.githubusercontent.com/67608539/144743866-4a3408b4-4db3-4c46-933a-7f766575327a.PNG)
![Capture8](https://user-images.githubusercontent.com/67608539/144743874-1c2d02e7-bb97-494a-8d94-df4b9aea573a.PNG)
הגענו לפיתרון!
flag- “mommy! I think I know what a file descriptor is!!”

 הפעולה read-
        read() attempts to read up to count bytes from file descriptor fd
       into the buffer starting at buf.
 להמשך קריאה https://man7.org/linux/man-pages/man2/read.2.html
