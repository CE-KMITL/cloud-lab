# Infrastructure Lab 1

ใน Lab นี้เราจะศึกษาการใช้งาน VMware vSphere เพื่อบริหารจัดการ Virtual Infrastructure ผ่าน VMware vCenter เริ่มตั้งแต่การจัดการผู้ใช้ การเพิ่ม ESXi Hypervisor และการสร้าง Virtual Machine เบื้องต้น

## vCenter Virtual Appliance
โปรแกรม vCenter เป็นชุดโปรแกรมที่ใช้บริหารจัดการเครื่องเซิร์ฟเวอร์ที่รัน ESXi ซึ่งโปรแกรมนี้เป็นส่วนหนึ่งของชุดแอพพลิเคชันที่เรียกว่า vSphere ใน Lab นี้เราจะใช้ vSphere 5.5 ผู้ใช้งาน vCenter สามารถควบคุมเซิร์ฟเวอร์หลายตัวพร้อม กันได้

สำหรับการติดตั้ง เราสามารถติดตั้ง vCenter Server บน Windows ได้เหมือนกับแอพพลิเคชันทั่วไป นอกจากนี้ ทาง VMware ก็มีชุดโปรแกรม vCenter ที่ถูกติดตั้งอยู่บน Linux VM ไว้แล้ว เรียกว่า vCenter Virtual Appliance (vCVA) ทั้ง 2 เวอร์ชัน (vCenter Server บน Windows และ vCVA บน Linux) สามารถใช้งานได้เหมือนกัน จะต่างกันที่วิธีการติดตั้งเท่านั้น ซึ่งใน Lab นี้เราจะใช้ vCVA ในการจัดการเครื่องเซิร์ฟเวอร์

นักศึกษาสามารถศึกษาวิธีการติดตั้ง vCenter ได้จาก
[vCenter Server Deployment Guide](http://www.vmware.com/files/pdf/vcenter/VMware-vCenter-Server-5.5-Technical-Whitepaper.pdf)

## ESXi
ESXi หรือ vSphere เป็น Hypervisor ที่ผลิตโดย VMware โปรแกรม ESXi สามารถถูกติดตั้งบนเครื่องเซิร์ฟเวอร์ได้เลยโดยไม่จำเป็นต้องมี OS ตัวอื่นก่อน เนื่องจากโปรแกรมนี้ถูกออกแบบให้ใช้กับเครื่องเซิร์ฟเวอร์เป็นหลัก ส่วนของ Console จะสามารถใช้ได้เพียงตั้งค่า Network เบื้องต้นเท่านั้น การควบคุม ESXi ส่วนใหญ่เป็นการควบคุมระยะไกลผ่าน vCenter หรือ vSphere Client
