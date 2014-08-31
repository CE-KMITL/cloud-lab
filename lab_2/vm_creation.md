# VM Creation

ในขั้นตอนสุดท้าย เราจะสร้าง Tiny Core Linux VM เพื่อยืนยันว่าเราสามารถสร้าง VM ผ่าน vCenter ได้จริง

## Create a tiny VM
1. คลิกขวาที่ Host ของคุณแล้วเลือก New Virtual Machine...
![New VM](http://goo.gl/206e0S)
2. เลือก Create a new virtual machine
3. ใส่ชื่อ VM เป็น tiny-yourname เลือก Folder เป็น `vCenterCE708 / DCLab708 / CloudLab`
![Select VM Folder](http://goo.gl/fOJNDw)
4. เลือก ESXi ของคุณเพื่อใช้เป็น Compute Resource
5. เลือก ds2-cloudlab เป็น Storage
6. เลือก Compatible with ESXi 5.1
7. เลือก Linux, Other Linux (32 bit)
8. ให้แก้ไข Hardware ที่ใช้
 * Memory 256 MB
 * Hard disk 1 GB
 * CD/DVD Drive
    * Datastore ISO File
    * ใช้ Media `[ds2-cloudlab] iso/TinyCore-current.iso`
    * เลือก Connect At Power On
![Edit Hardware](http://goo.gl/z5NGrD)
9. เลือก Finish เพื่อสร้าง VM

## Open VM Console
1. คลิกขวาบน VM ที่สร้างขึ้นและเลือก Power On
2. คลิกขวาที่ VM และเลือก Open Console เพื่อเชื่อมต่อกับ VM (หรือคลิกที่ VM icon เลือก Summary tab และ launch console)
 * ให้ตรวจดู Pop-up blocker ถ้าคุณไม่เห็นหน้าจอของ VM
 * ในกรณีที่เมาส์ติดอยู่บน Console ใช้ปุ่ม Ctrl-Alt เพื่อออกจากหน้าจอของ Console
![Open Console](http://goo.gl/x2bpQv)
3. ภายใน Console ของ VM คลิกขวาเพื่อเลือก Applications->Terminal แล้วรันคำสั่ง `uname -a` เพื่อดู version ของ Linux Kernel ที่คุณใช้
