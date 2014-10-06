# Infrastructure Management
ในการทดลองนี้เราจะให้นักศึกษาทดลองฝึกใช้ PowerCLI เพื่อจัดการกับ VM และ Host สำหรับคำสั่งของ PowerCL
สามารถดูตัวอย่างการใช้งานได้โดยใช้ Get-Help
```
Get-Help <คำสั่ง PowerCLI> -examples
```
หรือใช้ online-help
```
Get-Help <คำสั่ง PowerCLI> -online
```
1. ทดลองจัดเก็บรหัสผ่านโดยใช้ `New-VICredentialStoreItem` และ `Get-VICredentialStoreItem`
2. ให้นักศึกษาทดลองเชื่อมต่อกับ ESXi โดยใช้ `Connect-VIServer <vCenter IP>`
   หลังจากเชื่อมต่อแล้วนักศึกษาสามารถใช้ `vi:` หรือ `vmstore:` เพื่อใช้ตรวจดู Inventory หรือ datastore ได้
3. ทดลองเพิ่ม Host เข้าไปใน vCenter โดยใช้ `Add-VMHost`
4. ตรวจดูสถานะของ Host ด้วย `Get-VMHost` จากนั้นให้ตรวจดู Properties ของ VMHost
5. ตรวจดูสถานะของ VM ด้วย `Get-VM`
6. ตรวจดูสถานะของ Account ที่อยู่บน Host ด้วย `Get-VMHostAccount`
7. ตรวจดูสถานะของ iSCSI ด้วย `Get-VMHostStorage` (สามารถใช้ `Set-VMHostStorage` เพื่อสร้าง iSCSI Adapter ได้)
8. ทดลองใช้ Export-CSV เพื่อจัดเก็บผลลัพธ์

หลังจากทดลองใช้แล้วให้นักศึกษาสร้างไฟล์ `lab3.ps1` (PowerShell script จะมี extension เป็น .ps1)
โดยในไฟล์นี้ประกอบด้วยสคริปที่สามารถต่อเข้าไปใน vCenter Server (โดยไม่ต้องมี Prompt ให้ใส่ Username/Password)
และแสดงรายชื่อของ Host ที่ต่ออยู่กับ vCenter Server เรียงตามชื่อ (name) ของ host
พร้อมทั้งแสดงขนาดของหน่วยความจำทั้งหมดสำหรับแต่ละ Host (MemoryTotalMB)
ให้ลองรันสคริปโดยเรียก
```
PowerCLI C:\Users\Student> .\lab3.ps1
```
หรือใช้
```
PowerCLI C:\Users\Student> & “C:\your directory\lab3.ps1”
```
ตัวอย่างผลลัพธ์
```
Host           : 161.246.70.222
MemoryTotalGB  : 3.0

Host           : 161.246.70.223
MemoryTotalGB  : 2.8
```
