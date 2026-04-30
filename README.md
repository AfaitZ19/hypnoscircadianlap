ก

README.md
README.md
+30
-1

# hypnoscircadianlap
# hypnoscircadianlap

## สรุป Flow งานในระบบ (จาก `skyline_cpap_flow.html`)

1. **รับข้อมูลจาก SD Card เครื่อง CPAP**  
   เจ้าหน้าที่โรงพยาบาลเสียบ SD Card ของเครื่อง CPAP เข้าคอมพิวเตอร์
2. **Bridge App ตรวจจับการ์ดอัตโนมัติ**  
   แอป Bridge ที่รันพื้นหลังจะตรวจพบการ์ด, ระบุแบรนด์อุปกรณ์ และเลือก parser ให้ตรงรุ่น
3. **แปลงไฟล์ดิบเป็นข้อมูลกลาง (Normalize)**  
   ข้อมูลจากไฟล์ดิบ (`.SAP`, `.bin`, `EDF`) ถูกแปลงเป็น JSON มาตรฐานเดียวกัน
4. **ยืนยันตัวตนและผูกกับคนไข้ก่อนส่งข้อมูล**  
   ผู้ใช้งานค้นหาคนไข้/ยืนยันความถูกต้องก่อนกด upload เพื่อป้องกันข้อมูลผิดคน
5. **ส่งขึ้น Cloud API ด้วย HTTPS + JWT**  
   Bridge ส่ง payload เข้าระบบ API กลาง, ระบบ validate และบันทึกลงฐานข้อมูล
6. **แสดงผลตามสิทธิ์ผู้ใช้งานใน Web Portal**  
   แพทย์, คนไข้, เจ้าหน้าที่ รพ., และ Skyline เห็นข้อมูลไม่เหมือนกันตาม role

## มุมมองการทำงานแบบ End-to-End

- **Input Layer:** SD Card จากเครื่อง CPAP
- **Edge Processing Layer:** Bridge App (detect, parse, queue, upload)
- **Cloud Layer:** API + Database (patients, sessions, uploads)
- **Presentation Layer:** Portal สำหรับ Doctor / Patient / Hospital Admin / Skyline Dashboard

## จุดเสี่ยงสำคัญที่ควรตรวจเพิ่ม

- ความซับซ้อน parser บางแบรนด์ (Beyond/ResMed/Philips)
- การ match คนไข้ก่อน upload (ห้ามผิดคน)
- PDPA/Sensitive health data compliance
- ความสามารถทำงานต่อเนื่องเมื่อ offline และ sync ภายหลัง
