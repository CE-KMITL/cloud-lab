# PowerShell / PowerCLI

ในส่วนนี้ เราจะทดลองศึกษาการใช้งาน PowerShell เบื้องต้นเพื่อควบคุมระบบคอมพิวเตอร์

## เตรียมการใช้งาน PowerCLI
1. เรียกใช้โปรแกรม PowerCLI
2. เนื่องจาก PowerCLI จำเป็นต้องเพิ่มคำสั่งหรือ CmdLet ให้กับ PowerShell ดังนั้นนักศึกษาต้องตั้งค่า
Execution Policy เป็น RemoteSigned เพื่อให้ PowerCLI สามารถทำงานได้
`Set-ExecutionPolicy RemoteSigned`

## PowerShell เบื้องต้น
ใน PowerShell จะมีหน่วยที่ใช้จัดการส่วนต่าง ๆ ของระบบเรียกว่า Provider
1. ใช้คำสั่ง `Get-PSProvider` เพื่อเรียกดู Provider ที่ถูกติดตั้งไว้ Provider แต่ละตัวสามารถสร้าง drive ได้
เช่น FileSystem provider จะมี drive C:, D:, … ส่วน registry provider จะมี drive
HKLM กับ HKCU เพื่อใช้เข้าถึง Registry HKEY_LOCAL_MACHINE และ HKEY_CURRENT_USER
```
PowerCLI C:\> Get-PSProvider
```
2. ให้เรียกดู Drive ที่มีอยู่ทั้งหมดได้ด้วยคำสั่ง Get-PSDrive แล้วเปลี่ยนเข้าไปอ่าน Registry โดยใช้คำสั่ง
`Set-Location`
```
PowerCLI C:\> Get-PSDrive
PowerCLI C:\> Set-Location HKLM:
PowerCLI HKLM:\> _
```
3. นักศึกษาสามารถใช้งาน Registry Drive ได้คล้ายกับ Drive ทั่วไป โดยใช้ `dir` (`Get-ChildItem`)
เพื่อเรียกดูข้อมูล และใช้ `cd` (`Set-Location`) เพื่อเปลี่ยนเข้าไปใน directory (หรือ registry)
```
PowerCLI HKLM:\> cd SOFTWARE
```
4. ให้นักศึกษาหา Version ของ Windows ที่ใช้อยู่ ซึ่งโดยปกติแล้วข้อมูลนี้จะอยู่ใน registry
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion`
  * ให้เปลี่ยน directory ไปเป็น HKLM:\SOFTWARE\Microsoft\Windows NT
  ```
  PowerCLI HKLM:\> cd '\SOFTWARE\Microsoft\Windows NT'
  ```
  * ใช้คำสั่ง `Get-ChildItem | Get-Member` จะพบว่า Object นี้มี Type เป็น `Microsoft.Win32.RegistryKey` ซึ่งมี Method และ Property ต่างๆ เราสามารถเรียกใช้ method และ property ของผลลัพธ์ได้
  * เรียกใช้ `(Get-ChildItem).GetValue(“CurrentVersion”)` ผลลัพธ์ที่ได้มีค่าเท่าไหร่? และเป็น Type อะไร?
5. เราสามารถใช้ตัวแปรเพื่อเก็บค่าและนำไปใช้ภายหลังได้ เช่น
```
PowerCLI HKLM:\Software\Microsoft\Windows NT> $d = Get-ChildItem
PowerCLI HKLM:\Software\Microsoft\Windows NT> $d.getValue("CurrentVersion")
```
6. บางทีข้อมูลที่พบอาจจะยาวเกินไป สามารถใช้ `Out-Host –Paging` เพื่อแบ่งหน้าแสดงได้
```
PowerCLI HKLM:\Software\Microsoft\Windows NT> Get-ChildItem CurrentVersion | Out-Host –Paging
```
7. นอกจากนี้เรายังสามารถเรียงลำดับข้อมูลได้ กลับไปที่ Home directory ( เรียก `C:` แล้วใช้ `cd ~` ) แล้วใช้
`Get-ChildItem | Sort-Object Length –Descending`
8. หลาย ๆ คำสั่งที่เราเคยใช้ใน cmd ยังสามารถใช้ได้อยู่เนื่องจากคำสั่งเหล่านี้ถูกเปลี่ยนให้เป็น PowerShell Cmdlet ส
มารถตรวจคำสั่งได้โดยใช้ alias เช่น `alias cd` จะแจ้งว่า `cd` เป็นคำสั่งเดียวกับ `Set-Location`
9. ถ้ามีปัญหาสงสัยในการใช้งาน PowerShell มี built-in help โดยใช้ `Get-Help` เช่น `Get-Help Set-Location`

## PowerShell Object
เราสามารถสร้าง Custom Object (*PSObject*) ใน PowerShell ได้จากการใช้ Hash table
```
$dict = @{title = 'title'; author = 'author'}
New-Object -TypeName PSObject -Property $dict
```

## Array
```
$myArray = @(1,2,3,4)
$myArray += 5
$myArray[2]
```

## For Loops / For Each
```
$array = @(1,2,3,4)
for ($i=0; $i -lt $array.length; $i++) {
  $array[$i]
}
foreach ($element in $array) {
  $element
}
$array | foreach {
  $_
}
```
