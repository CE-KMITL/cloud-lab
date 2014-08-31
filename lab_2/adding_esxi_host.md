# ESXi Host Management

ในขั้นตอนต่อไปเราจะเพิ่มเซิร์ฟเวอร์ที่รัน ESXi hypervisor เข้าไปให้ vCenter บริหารจัดการ

## Boot ESXi host
ปกติแล้ว เราจะติดตั้ง ESXi ลงบนเครื่องเซิร์ฟเวอร์เหมือนการติดตั้ง OS ทั่วไป แต่ในกรณีนี้เราจะใช้วิธีบูท ESXi ผ่าน network โดยใช้ PXE (Preboot Execution Environment)

การบูทผ่าน Network จำเป็นต้องใช้ DHCP และ TFTP server (สำหรับ network card ทั่วไป) หรือ HTTP server (สำหรับ iPXE) ซึงต้องใช้เวลาในการติดตั้งพอสมควร

ใน lab นี้ คุณสามารถบูทเครื่อง workstation ที่ไม่ถูกใช้งานอยู่ผ่าน PXE เพื่อใช้เป็น ESXi host ได้ โดยเลือกการบูทผ่าน network จากนั้นเลือก boot ESXi 5.1 แทน local disk

## Add ESXi host to vCenter
ก่อนที่ vCenter จะควบคุม ESXi host ได้ เราจะต้องเพิ่ม Host เข้าไปใน vCenter ก่อน
1. จาก Home เลือก vCenter -> Inventory Trees -> Hosts and Clusters
ภายใต้ vCenterCE708 จะมี Datacenter ชื่อ DCLab708 ซึ่งจะประกอบไปด้วย Cluster หลาย Cluster ให้คลิกขวาที่ **Workstations** แล้วเลือก Add Host...
![Add Host](http://goo.gl/Ikv0hZ)
2. ใส่ IP address ของ ESXi host
3. ใช้ User name `root` และไม่มี password (ยกเว้นคุณได้ตั้งค่า password ของ ESXi host ไว้ก่อนหน้านี้)
4. เลือก (No License Key) Evaluation Mode
5. ไม่ต้อง check `Enable lockdown mode` ถ้าคุณเลือก Enable lockdown mode จะทำให้คุณไม่สามารถเข้าใช้งานเครื่อง ESXi host นั้นผ่าน network โดยตรงได้
6. เลือก Put all of this host... ซึ่งจะเป็นการรวบรวมทรัพยากรทั้งหมดของ Host นี้ให้ไว้กับ cluster
7. จากนั้นกด Finish และรอให้ vCenter เพิ่ม Host เสร็จ

ลองตรวจดู Host ที่คุณเพิ่มเข้าไป (กดดู Summary tab) และสังเกตว่า Host นี้มีปัญหาอะไรบ้าง

## Add Storage Adapter
เนื่องจาก Host ที่เราบูทขึ้นมานั้นไม่มี Hard disk เราจะขอใช้เนื้อที่จากส่วนกลางผ่านโปรโตคอล iSCSI

1. เราต้องเพิ่ม iSCSI Adapter ให้ ESXi ผ่าน Manage->Storage->Storage Adapters->Add New Storage Adapter->Software iSCSI Adapter แล้วคลิก OK
![Add Adapter](http://goo.gl/E04mN1)
2. เลือก iSCSI target (เครื่องทีทำหน้าที่เป็นพื้นที่เก็บข้อมูลส่วนกลาง) ที่ 161.246.70.231:3260
![Add target](http://goo.gl/KT4L6b)
3. โปรแกรมจะเตือนให้ Rescan storage ให้เลือก Rescan all storage...
![Scan](http://goo.gl/oLFvL3)
4. ตรวจสอบจาก Storage Devices ว่าเรามี iSCSI Disk ขนาด 600 GB ต่อกับ ESXi host นี้
