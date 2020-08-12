## Room : easyPeasy


เริ่มด้วย Scan port<br>
_**Nmap –sS –sV –p- <IP_machine>**_

![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/1.PNG)
จากผลลัพธ์จะเห็นได้ว่า เปิดเว็บ ไว้ 2 port และ ssh อีก 1 port <br>
เริ่มจาก port 80 ก่อน<br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/2.png)
ในหน้าเว็บ ไม่มีอะไร <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/3.png)
ในหน้า Source code ก็ไม่มีอะไร<br>
ต่อไปจะใช้ gobuster ในการ directory search <br>
_**gobuster dir –u <target_url>  -w /usr/share/dirb/wordlists/common.txt –e**_
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/4.png)
ไปดูใน /robots.txt กันก่อน <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/5.png)
<br>ไม่มีอะไร <br>
ต่อไปก็ /hidden <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/6.png)
ได้รูปบ้านร้างมารูปนึง <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/7.png)
ในหน้า Source code ก็ไม่มีอะไรอีกเช่นเคย <br>
แต่มันไม่ควรจบแบบนี้ เลย gobuster /hidden อีกครั้ง <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/8.png)
ได้ /whatever เพิ่มมา <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/9.png)
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/10.png)
คราวนี้ใน source code มี <p> ที่มี string แปลกๆ อยู่ จะเห็นได้ว่ามันลงท้ายด้วย ==  มันคือ base64 <br>
นำไป decode <br>
_**echo <string> | base64 --decode**_
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/11.png)
ก็จะได้ flag แรก <br>
คราวนี้ไปดู เว็บบน port 65524 บ้าง <br>
  ![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/12.png)
ใน source code รอบนี้มีถึง 2 อย่าง
1.	String แปลกๆ 
   ![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/13.png)
2.	Flag ที่ 3
   ![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/14.png)

String แปลกๆ ที่ว่า น่าจะ encode ด้วย base อะไรซักอย่าง <br>
ใช้ cyberchef ช่วย <br>
มันคือ base62 และ ได้ ไดเรกทอรี่มา <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/15.png)

ต่อไป gobuster เว็บ บน port 62254 <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/16.png)
/robots.txt <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/17.png)
<br> String แปลกมาอีกแล้ว <br>
นำไป decode จะได้ flag 2 <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/18.png)

ทีนี่กลับมาที่ ไดเรกทอรี่ที่ได้จาก base62 <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/19.png)
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/20.png)
ใน source code จะเห็น รูป binary…. .jpg กับ string แปลกๆ   <br>
ก่อนอื่นนำ string ไป decode ก่อน  <br>
ตอนแรกนั้นเอาไป decode ใน online tool แต่ มันช้าเกิน ช้าจนผิดสังเกต เลยเปลี่ยนมาใช้ hashcat แทน  <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/21.png)
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/22.png)
ก็จะได้ password สำหรับอะไรซักอย่าง <br>
ทีนี้มาที่รูป ลอง extract รูป <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/23.png)
มีการขอ password ด้วย ใส่ password ก่อนหน้านี้เข้าไป ก็จะได้ <br>
Secrettext.txt <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/24.png)
มันคือ user และ password ที่เป็น binary <br>
นำ binary ไป decode <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/25.png)
เอา username & password ไป login ssh
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/26.png)
ก็จะเจอ user flag แต่ format ของ flag มันแปลกๆ <br>
แก้ได้ด้วย rot13  <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/27.png)
<br> ใช้ sudo –l ในการดูว่ามีโปรแกรมไหนรันด้วยสิทธิ์ root บ้าง <br>
แต่ไม่ได้ผล <br>
เลยใช้ linpease.sh ในการดูว่ามีช่องโหว่อะไรบ้างที่ให้เราอัพสิทธิ์เป็น root ได้ <br>
คำอธิบายของห้องนี้ คือการอัพสิทธิ์ด้วย cronjob <br>
น่าจะเจอแล้วละ <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/28.png)
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/29.png)
<br> จากข้อความข้างใน ไม่ว่าอะไรอยู่ข้างใน จะรันด้วย root  <br>
ก็ reverse shell ซะเลย <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/30.png)
รอรับ shell ที่ port 7878 <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/31.png)
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/32.png)
 
<br>ก็จะได้ shell และ มีสิทธิ์เป็น root  <br>
ไปยัง home ของ root  <br>
ก็จะเจอ root flag ที่เป็น hidden file  <br>
![](https://github.com/darknezs/thm/blob/master/easyPeasy/images/33.png)


