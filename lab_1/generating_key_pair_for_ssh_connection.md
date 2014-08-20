# Generate SSH key pair

เราจะใช้ SSH Public Key authentication ในการเข้าใช้ระบบ ซึ่งการใช้งานแบบนี้มีความปลอดภัยสูงกว่าการใช้งานแบบ Username/password ทั่วไป และเป็นวิธีการเข้าถึงวิธีเดียวที่สามารถใช้ได้สำหรับเครื่อง Linux บน Azure

## ความรู้เกี่ยวกับ RSA Key pair
เราจำเป็นต้องสร้าง Key pair 1 คู่ ซึ่งประกอบด้วย Public และ Private Key ตัว Public Key จะเหมือนกับแม่กุญแจซึ่งจะต้องนำไปติดตั้งบนเครื่อง VM ส่วนตัว Private Key จะเหมือนกับลูกกุญแจที่สามารถทำให้เราไขเปิดแม่กุญแจเข้าไปใช้งานเครื่องได้

ในทางปฏิบัติ Private key จะมีลักษณะคล้ายกับ Password แต่มีความปลอดภัยมากกว่าเพราะมีความยาวถึง 2048 บิต โดยเราสามารถใช้ Private key เพื่อ login ไปยังเครื่องที่มี Public key ที่คู่กันอยู่ได้ แต่เนื่องจาก Private key นี้ถูกเก็บอยู่ในไฟล์บนฮาร์ดดิสก์ ซึ่งถ้ามีคนหรือมัลแวร์ที่สามารถอ่านไฟล์นี้ได้ก็เหมือนกับคน ๆ นั้นมี password ของเรา เราจึงควรที่จะป้องกัน Private key ด้วย passphrase อีกชั้นหนึ่ง

การตั้ง passphrase เป็นการเข้ารหัสของ Private key ซึ่งเป็นสิ่งที่ควรจะทำทุกครั้งเมื่อสร้าง key pair ข้อดีคือ Passphrase จะช่วยให้เราพอมีเวลาไปเปลี่ยนหรือลบ Public key ออกจากเครื่อง ในกรณีที่ผู้บุกรุกสามารถเอาไฟล์ Private key ของเราไปได้ เพราะผู้บุกรุกต้องเสียเวลา crack passphrase ให้ได้ private key ก่อนที่จะสามารถ login ได้

## สำหรับผู้ใช้ Linux
ใช้ `ssh-keygen`เพื่อสร้าง RSA key pair
```bash
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa): [กด Enter]
Enter passphrase (empty for no passphrase): [ใส่ passphrase]
Enter same passphrase again: [ใส่ passphrase]
Your identification has been saved in /home/user/.ssh/id_rsa.
Your public key has been saved in /home/user/.ssh/id_rsa.pub.
...
```

โดย Private key จะถูกเก็บอยู่ใน `~/.ssh/id_rsa` และ Public key ที่เราจะนำไปใช้ถูกเก็บอยู่ใน `~/.ssh/id_rsa.pub`

ในกรณีที่ไม่ต้องการใส่ passphrase ทุกครั้งที่ใช้ SSH ให้ใช้โปรแกรม `ssh-agent` เพือเก็บ passphrase และใช้ `ssh-add` เพื่อโหลด Private Key

## สำหรับผู้ใช้ Windows
ใช้โปรแกรม[`PuTTYgen`](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) ที่มาคู่กับ [`PuTTY`](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

![PuttyGen build key](http://goo.gl/xkNeMR)

หลังจากสร้างเสร็จแล้วให้ใส่ passphrase แล้วกด `Save private key`

![Set passphrase and get public key](http://goo.gl/KVEPGD)

เมื่อต้องการใช้ key pair ให้ใช้โปรแกรม[`Pageant`](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) โดยหลังจากเรียกใช้โปรแกรมแล้ว ให้คลิกขวาที่ System tray icon ของ Pageant และเลือก `Add Key` จากนั้นให้เลือกไฟล์ Private key ที่เราสร้างไว้และใส่ Passphrase
