# pwnable.kr
## fd

זהו מדריך כיצד לפתור את האתגר הראשון.
נתחיל ב


![Capture1](https://user-images.githubusercontent.com/67608539/144739918-5ec4eda8-a12b-4def-b77f-9704ca84ea0d.PNG)

נעתיק את קוד ה 'ssh' לטרמינל של לינוקס, אני משתמש ב- 'ubuntu-20.04.3'
ומיד לאחר מכן את הסיסמא- 'guest' כפי שמצויין.

![Capture2](https://user-images.githubusercontent.com/67608539/144739990-ee683c1e-22fd-4d91-bca2-c34611f46f69.PNG)


כעת, פעולה ראשונה שנעשה היא לבדוק אילו קבצים נמצאים, נכתוב את הפקודה 'ls' על מנת לראות אילו ספריות וקבצים יש במיקום

![Capture3](https://user-images.githubusercontent.com/67608539/144740069-b1909b2c-b7f3-4f22-ac1e-006e64dcfc32.PNG)

ישנם שלושה קבצים, אחד מהם קובץ C.
המטרה שלנו להגיע ל 'flag' מסויים, ניתן לראות שיש קובץ 'flag' ננסה לראות מה יש בו באמצעות הפקודה 'cat' על מנת לראות את תוכנו של הקובץ
