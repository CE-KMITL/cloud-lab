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
## ใช้ PowerCLI เพื่อบริหารจัดการ Host
1. ทดลองจัดเก็บรหัสผ่านโดยใช้ `New-VICredentialStoreItem` และ `Get-VICredentialStoreItem`
   * สังเกตว่าชนิดของข้อมูลที่ได้จาก Get-VICredentialStoreItem เป็นชนิดใด (ดูชนิดจากการเรียกใช้ method `getType()`)
   * ให้สร้าง *PSCredential* Object จาก Object ที่คุณสร้างขึ้น
   ```
   $securePassword = ConvertTo-SecureString "PlainPassword" -AsPlainText -Force
   $creds = New-Object System.Management.Automation.PSCredential("username",$securePassword)
   ```
2. ให้นักศึกษาทดลองเชื่อมต่อกับ ESXi โดยใช้ `Connect-VIServer -Server vcsa.soup.ce.kmitl.ac.th -Credential $creds`
   หลังจากเชื่อมต่อแล้วนักศึกษาสามารถใช้ `vi:` หรือ `vmstore:` เพื่อใช้ตรวจดู Inventory หรือ datastore ได้
3. ตรวจดูสถานะของ Host ด้วย `Get-VMHost` จากนั้นให้ตรวจดู Properties ของ VMHost
4. ตรวจดูสถานะของ VM ด้วย `Get-VM`
5. ทดลองใช้ Export-CSV เพื่อจัดเก็บผลลัพธ์

## ใช้ PowerCLI เพื่อบริหารจัดการ VM
1. ทดสอบการสร้าง VM อย่างง่าย (ใช้ชื่อเป็น cli-username) กรณีนี้เราจะสร้าง VM ที่มีหน่วยความจำ 512 MB และดิสค์ขนาด 1 GB ใน Resource pool "lab2" 
   ```
   New-VM -ResourcePool lab2 -Name cli-username -DiskGB 1 -MemoryMB 512
   ```
2. ตรวจสอบสถานะของ VM ที่สร้างขึ้น
   ```
   Get-VM cli-username | Format-List
   ```
3. ใช้คำสั่ง `Move-VM` เพื่อย้าย VMHost ไปอีกเครื่อง
4. ใช้คำสั่ง `New-CDDrive`, `Get-CDDrive`, `Set-CDDrive`เพื่อสร้างไดรฟ์ CD-Rom และโหลด `iso/tahr64-6.0.5.iso` ให้กับ VM ที่สร้างขึ้นขณะบูท (Hint: ตั้ง option StartConnected ของ VM ให้เป็น 1)
5. ใช้คำสั่ง `Start-VM` เพื่อเปิด VM ดังกล่าว ตรวจสอบว่า VM สามารถ boot ได้ถูกต้องหรือไม่

## Task
หลังจากทดลองใช้แล้วให้นักศึกษาสร้างไฟล์ `lab2.ps1` (PowerShell script จะมี extension เป็น .ps1)
โดยในไฟล์นี้ประกอบด้วยสคริปต์ที่สามารถต่อเข้าไปใน vCenter Server (โดยไม่ต้องมี Prompt ให้ใส่ Username/Password)
และแสดงรายชื่อของ VM ที่ต่ออยู่กับ vCenter Server เรียงตามชื่อ (name) ของ VM
พร้อมทั้งแสดง Host ที่ใช้รัน VM และขนาดของหน่วยความจำ(MB) สำหรับ VM แต่ละตัว
ให้ลองรันสคริปโดยเรียก
```
PowerCLI C:\Users\Student> .\lab2.ps1
```
หรือใช้
```
PowerCLI C:\Users\Student> & “C:\your directory\lab2.ps1”
```
ตัวอย่างผลลัพธ์
```
Name           : vm1
VMHost         : 161.246.70.228
MemoryMB       : 2048

Name           : vm2
VMHost         : 161.246.70.229
MemoryMB       : 256
```