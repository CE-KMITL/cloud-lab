# Create a VM
## สร้าง Virtual Machine
1.คลิกปุ่ม `+ New` ที่มุมล่างซ้ายของ Portal เพื่อสร้าง VM ตัวใหม่ จากนั้นให้เลือก Ubuntu Server 14.04 LTS

![New Virtual Machine](http://goo.gl/dATiIL)

2.ระบุ Host name ที่ต้องการ พร้อมทั้งใส่ User name และ Authentication Key (ไม่ใช่ Password) เพื่อใช้ในการเชื่อมต่อกับ VM โดยเราจะใช้ SSH ในการเชื่อมต่อกับ VM

![Fill in VM information](http://goo.gl/SU4lF5)

3.ให้เปลี่ยน Location ของ VM ที่จะสร้างขึ้นเป็น South East Asia

![Switch region](http://goo.gl/g2A7Fs)

4.ระบุชื่อ Resource group ที่ต้องการ โดย Resource group จะเป็นหน่วยที่รวบรวมทรัพยากรที่ใช้ในการทำงานหลายประเภท เช่น VM, Storage, Network เข้าไว้ด้วยกัน การใช้ Resource group จะทำให้การจัดการง่ายขึ้น เช่น เราสามารถลบ Resource ทั้งหมดที่อยู่ภายใน Resource group ไปพร้อม ๆ กันได้โดยไม่จำเป็นต้องไปคอยลบทีละตัว

![Set resource group name](http://goo.gl/uwv6GI)

5.สร้าง Storage account เพื่อใช้จัดเก็บข้อมูล เช่น ฮาร์ดดิสค์ของ VM โดยคุณสามารถตั้งชื่อ account ที่ต้องการได้ ในกรณีที่ใช้ Azure Pass คุณจะสามารถสร้าง Storage account ได้เพียงหนึ่ง account เท่านั้น ถ้าสร้าง VM หลาย ๆ ตัว จะต้องใช้ Storage account รวมกัน

![Create storage account](http://goo.gl/BemidQ)

6.คุณสามารถใส่ชื่อ Virtual network ที่ต้องการได้ โดยเราสามารถนำ Virtual network นี้ไปใช้ร่วมกับ VM ตัวอื่นได้ โดย VM ที่อยู่บน Virtual Network เดียวกันจะเหมือนกับอยู่บน Broadcast domain เดียวกัน และสามารถติดต่อกันได้โดยใช้ Private IP address

![Configure virtual network](http://goo.gl/0D2Cyl)

7.ระบุ Domain name ของ VM เพื่อใช้เชื่อมต่อจากภายนอก โดย Domain name ที่ได้จะมี suffix เป็น `.cloudapp.net`

![Configure domain name](http://goo.gl/J6kfhU)

8.ยืนยันการแก้ไข Configuration ของ VM (คลิก OK) แล้วสร้าง VM โดยคลิก Create

![Create VM](http://goo.gl/021xfE)
