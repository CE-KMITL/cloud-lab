# Endpoint Creation

## Endpoint
เมื่อ login เข้าไปใช้งาน VM จะพบว่าตัว VM เองมองเห็นเฉพาะ Private IP address ทั้งนี้เนื่องจาก Azure จะเป็นฝ่ายจัดการ Public IP address และคอยส่งผ่านข้อมูลต่อไปยัง VM ดังนั้น Network traffic ทั้งหมดต้องผ่านระบบของ Azure ก่อน โดยตัว VM เองจะไม่รับรู้ว่ามี Public IP address อยู่

```
login as: username
Authenticating with public key "rsa-key-username-kmitl" from agent
Welcome to Ubuntu 14.04 LTS (GNU/Linux 3.13.0-29-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Wed Aug 20 06:00:55 UTC 2014

  System load:  0.0               Processes:           217
  Usage of /:   3.4% of 28.80GB   Users logged in:     0
  Memory usage: 5%                IP address for eth0: 10.0.0.4
  Swap usage:   0%
  ...
```
ดังนั้นการส่ง Traffic ผ่านเข้าไปยัง VM ต้องมีการตั้งค่าบน Azure เพื่อให้เปิดช่องทางสื่อสาร ซึ่งเราเรียกช่องทางนี้ว่า Endpoint

สำหรับ VM ที่สร้างขึ้นใหม่จะมี Endpoint ที่ถูกสร้างขึ้นโดยอัตโนมัติคือ SSH endpoint เพื่อให้ผู้ใช้สามารถเชื่อมต่อกับ VM ได้

## Running Services on VM

ในขั้นตอนนี้ เราจะทดสอบการรัน Web Server บน VM ที่สร้างขึ้น
1. ติดตั้ง nginx เป็น web server ด้วยคำสั่ง `sudo apt-get install nginx-light`
2. Ubuntu จะติดตั้ง nginx บน VM ให้ หลังจากติดตั้งแล้วให้ทดสอบว่ามี Web Server รันอยู่จริงด้วย `curl http://localhost/`
3. ทดสอบจากเว็บบราวเซอร์เพื่อเรียกใช้ web server จากภายนอก โดยใช้ URL http://yourdomain.cloudapp.net (yourdomain คือ domain name ที่ตั้งขึ้นตอนสร้าง VM)
4. จะพบว่าเราไม่สามารถต่อไปหา web server ได้เนื่องจาก Azure ไม่ได้สร้าง Endpoint สำหรับ web server ให้ เราจะสร้าง HTTP endpoint เพิ่มสำหรับ VM ตัวนี้ผ่าน Portal โดยคลิกที่ส่วนของ Endpoints ในเบลดที่แสดงข้อมูล VM จากนั้นเพิ่ม HTTP Endpoint โดยระบุชื่อ (HTTP), โปรโตคอลที่ใช้ (TCP), Public Port เพื่อเรียกใช้จากภายนอก (80) และ Private Port ที่ web server รอรับข้อมูลอยู่บน VM (80)
![Add an endpoint](http://goo.gl/VrhOH2)
5. ทดสอบเข้าใช้ web server ผ่านเว็บบราวเซอร์ใหม่ จะเห็นว่าเราสามารถเข้าใช้ web server ได้ตามปกติ
