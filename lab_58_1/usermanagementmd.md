# User Management

ในขั้นตอนนี้เราจะใช้ vSphere Web Client เพื่อสร้าง User และใช้งาน vCenter เพื่อจัดการศูนย์ข้อมูล

## Single Sign-On
ถึงแม้ว่าเราจะสามารถใช้ root account (root/vmware) เพื่อใช้งาน vCVA ได้ แต่เราไม่ควรที่จะใช้ root เนื่องจากทำให้ไม่สามารถตรวจสอบได้ว่าใครเป็นผู้ใช้งานหรือแก้ไขระบบ ภายใน vCenter เองมีส่วนที่จัดการกับผู้ใช้โดยเฉพาะเรียกว่า vCenter Single Sign On (SSO) ซึ่งระบบนี้สามารถดึง User Account จากระบบหลายระบบ (เราเรียกแต่ละระบบว่า **Domain**) เช่น Active Directory, OpenLDAP หรือ local account ของ OS เอง

โปรแกรม SSO นั้นถูกติดตั้งมากับ vCVA แล้ว โดยปกติ SSO จะสร้าง domain ขึ้นมา 2 domain คือ

1. **localos** เพื่อใช้เชื่อมต่อกับ account บน OS ที่ตัว SSO ถูกติดตั้งอยู่
2. **vsphere.local** เพื่อใช้เชื่อมต่อกับ account ที่ถูกสร้างขึ้นด้วย SSO เอง

ดังนั้นเราสามารถสร้าง account ใหม่ได้ทั้งจาก SSO หรือจาก OS
ในการใช้งานจริงนั้น SSO มักจะผูกกับ Active Directory (AD) ผู้ดูแลระบบสามารถสร้าง Account บน AD เพื่อใช้กับระบบหลายระบบได้ แต่เนื่องจากระบบของเราไม่มี AD เราจะใช้วิธีการสร้าง Account บน SSO แทน ในขั้นตอนนี้เราจะสร้าง account ใหม่บน SSO และกำหนดสิทธิให้ใช้งานได้เป็นผู้ดูแลระบบ

## Create User (Lab Admin) Account
โดยปกติ SSO จะสร้าง Administrator@vsphere.local เพื่อใช้จัดการ Account ที่ถูกสร้างโดย SSO เอง

เพื่อความสะดวกในการดูแลระบบที่ใช้ร่วมกัน เราขอสงวนสิทธิ์การเข้าถึง Administrator account แต่สำหรับ lab นี้ เราจะเปิดให้คุณใช้ account พิเศษเพื่อสร้างผู้ใช้โดยเฉพาะ

### Create SSO User
ในขั้นตอนนี้เราจะเพิ่ม SSO user สำหรับ Log-in เข้าใช้ vSphere Web Client
1. เปิด Web browser เพื่อเข้าใช้งาน vSphere Webclient ผ่าน URL https://vcsa.soup.ce.kmitl.ac.th:9443/vsphere-client/ โดย Login ด้วย **sso@vsphere.local**
2. จาก Navigation Panel ทางด้านซ้าย เลือก Administration -> Single Sign-On -> Users and Groups ![SSO Administration](http://goo.gl/t1pUUu)
3. จาก Users tab กด `+` เพื่อสร้าง account ใหม่ ![Create user](http://goo.gl/MXBDBG)
4. สร้าง Account ใหม่สำหรับคุณโดยกรอก Username ตามต้องการ พร้อมทั้งกรอก **First name, Last name, Email address** เป็นภาษาอังกฤษ

### Assigning Roles and Groups
Account ที่ถูกสร้างขึ้นสามารถใช้ login เข้า vSphere Web Client ได้ แต่ยังไม่สามารถใช้งาน vCenter ได้ คุณต้องเพิ่มความสามารถในการใช้ vCenter ให้กับ Account ที่ถูกสร้างขึ้น ใน lab นี้เราจะเพิ่ม Account เข้าไปในกลุ่ม **LabUsers** ซึ่งสามารถใช้งาน vCenter ได้อยู่แล้ว (เพราะเหตุใด?)
1. เลือก Groups tab จากนั้นคลิกที่กลุ่ม LabUsers และเลือก Add Member ![Add group member](http://goo.gl/OLGbN1)
2. ภายใน Add Principals dialog ให้เลือก Domain vsphere.local และหา Account ที่คุณสร้างขึ้น กด Add หรือป้อนค่าใน Users เลือก Check names เพื่อตรวจสอบความถูกต้องของ Account แล้วคลิก OK ![Add prinicipal](http://goo.gl/h0yjsa)
3. คุณควรจะเห็นชื่อ Account ของคุณภายใต้ Group Members
4. log-out จาก sso@vsphere.local แล้ว log-in ด้วย account ที่คุณสร้างขึ้นพร้อมทั้งแจ้งให้อาจารย์ที่คุม lab ทราบ
5. ลองตรวจดูความสามารถในการเข้าถึงของ LabUsers โดยเลือก Administration -> Roles สังเกตว่าตอนนี้เราจะมี Roles provider เป็น vCenterCE708
![Check roles](http://goo.gl/C8iQbW)
6. คลิกที่ vCenterCE708 เพื่อตรวจสอบดูว่าผู้ใช้ หรือกลุ่มใดมีสิทธิ์เข้าใช้งาน vCenter เครื่องนี้บ้าง

สิทธิ์ในการเข้าใช้ vCenter นั้นถูกกำหนดเป็นแบบ Role-based Access Control (RBAC) โดยมีการนิยามบทบาท (Roles) และผู้ใช้แยกจากกัน
จากนั้นเราจะใช้ vCenter หรืออุปกรณ์อื่น ๆ ที่เราจำเป็นต้องกำหนดสิทธิ์ (Privileges) กำหนดความสัมพันธ์ระหว่างผู้ใช้กับบทบาทเหล่านั้น (เรียกว่าการ Set Permissions) ยกตัวอย่างเช่น
* **Lab Users role** เป็นสิทธิ์ที่ถูกนิยามขึ้นสำหรับ lab นี้เท่านั้น โดยบทบาทนี้มีสิทธิ์เกือบทุกอย่างในการใช้ vCenter (ยกเว้นความสามารถในการ Set permissions) โดยผู้ที่มีบทบาทนี้คือกลุ่มของผู้ใช้ที่ชื่อ LabUsers และคุณก็เป็นหนึ่งในผู้ใช้กลุ่มนั้น
* **Administrator role** สามารถเข้าใช้งานทุกอย่างของ vCenter ได้โดยไม่มีข้อกำหนด
