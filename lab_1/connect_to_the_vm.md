# Connect to the VM

หลังจากสร้าง VM แล้ว ให้คลิกดู Properties ของ VM เพื่อตรวจสอบ IP Address ของ VM ที่สร้างขึ้น โดย VM ที่ถูกสร้างขึ้น ปกติจะมี 2 IP Address คือ Public และ Private IP address

* **Public IP Address** ใช้สำหรับเข้าถึง VM หรือ Service ที่รันอยู่บน VM จาก Internet
* **Private IP Address** ใช้สำหรับการเข้าถึง VM จาก VM ตัวอื่น ๆ ที่อยู่บน Virtual Network เดียวกัน

![View Properties](http://goo.gl/G6uijy)

คุณสามารถใช้ ssh-client (ควบคู่กับ ssh-agent) หรือ PuTTY (ควบคู่กับ Pageant) เพื่อ login เข้าใช้งาน VM ด้วย User name ที่ระบุไว้ตอนสร้าง VM
