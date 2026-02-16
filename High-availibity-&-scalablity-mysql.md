pada sesi ini, saya akan mendemonstrasikan konfigurasi MySQL InnoDB Cluster untuk mencapai high availability, redundansi data, dan read scalability menggunakan fitur native MySQL


Architecture component:
- MySQL Group Replication : Inti dari cluster. Memastikan replikasi sinkron dan konsistensi data antar node (1 Primary, 2 Standby).
- MySQL Shell             : Interface admin utama untuk mengelola cluster melalui AdminAPI.
- MySQL Router            : Middleware yang bertindak sebagai proxy cerdas untuk direct traffic aplikasi ke node yang tepat (Read-Write atau Read-Only)



1. Instalasi mysql-server dan mysql-shell pada seluruh node. Memastikan setiap server memiliki mesin database dan alat administrasi yang diperlukan
   
   <img width="1181" height="112" alt="Screenshot (353)" src="https://github.com/user-attachments/assets/ccd4f504-9e4e-426d-8bc6-1c54dc92de82" />
   <img width="1136" height="98" alt="Screenshot (354)" src="https://github.com/user-attachments/assets/63e3d279-761c-48ad-aa45-a7a9b78e0f48" />
   <img width="1217" height="115" alt="Screenshot (355)" src="https://github.com/user-attachments/assets/ad8867a7-1c16-46cf-bfb3-c39efff9c993" />


2. Set file /etc/hosts agar setiap server bisa saling mengenal melalui hostname. Mempermudah komunikasi antar-node untuk tujuan cluster
   
   <img width="781" height="68" alt="Screenshot (358)" src="https://github.com/user-attachments/assets/7db4f29d-2af7-40b1-a307-f1c59b57c105" />
   <img width="1061" height="248" alt="Screenshot (357)" src="https://github.com/user-attachments/assets/a381637d-a9bb-4fb7-89c9-a7f9bf0608b6" />
   <img width="1036" height="122" alt="Screenshot (359)" src="https://github.com/user-attachments/assets/d9684d7f-04b2-41ea-813d-b53253427eea" />
   <img width="819" height="119" alt="Screenshot (360)" src="https://github.com/user-attachments/assets/4106103f-30e9-49d0-a442-c04535e77a93" />


3. login ke mysqlsh untuk mendaftarkan semua server ke cluster menggunakan user khusus cluster. Parameter GTID dan Binary Log diaktifkan sebagai "identitas unik" setiap transaksi agar data tidak tertukar
   
   <img width="1281" height="527" alt="Screenshot (362)" src="https://github.com/user-attachments/assets/a0b932c5-1cbd-4554-85d8-80d7374707f8" />
   <img width="1345" height="540" alt="Screenshot (363)" src="https://github.com/user-attachments/assets/9f9ef4f1-748f-4856-bb23-1f50e7a68263" />
   <img width="1322" height="442" alt="Screenshot (365)" src="https://github.com/user-attachments/assets/59f17db6-b9fa-461e-9242-e1bfd5f0b22e" />


4. Validasi status cluster dengan melakukan pengecekan ulang koneksi menggunakan user cluster yang baru dibuat. Memastikan status setiap node adalah "OK" sebelum merger node
   
   <img width="1699" height="788" alt="Screenshot (369)" src="https://github.com/user-attachments/assets/fc8d31ec-5b32-43df-8092-1046ac3f6975" />


5. Inisialisasi Cluster (Bootstrap) dengan menunjuk satu server sebagai primary melalui perintah createCluster. Di tahap ini, metadata cluster resmi terbentuk
   
   <img width="1338" height="525" alt="Screenshot (372)" src="https://github.com/user-attachments/assets/76f22407-8e30-4c55-987e-bc28ab773eb2" />


6. Verifikasi status primary server. Memastikan primary siap memimpin proses replikasi dan menerima data (Read-Write)
   
   <img width="1380" height="757" alt="Screenshot (373)" src="https://github.com/user-attachments/assets/ac2db45c-5007-411a-829f-3722d8f41b0f" />


7. Add standby server ke dalam cluster untuk sinkronisasi data dari primary (high availibility)
   
   <img width="1595" height="882" alt="Screenshot (374)" src="https://github.com/user-attachments/assets/e32b4463-4312-4c80-8069-0bccd6c7c4fb" />


8. Verifikasi status role & mode setiap node
   
   <img width="1080" height="708" alt="Screenshot (375)" src="https://github.com/user-attachments/assets/fe6361c9-d342-482a-b861-3d3fff3ea926" />
   - member_role : status database server
   - mode        : R/W (read-write) untuk server primary, dan R/0 (read only) untuk server standby


9. Verifikasi metadata cluster. Sistem otomatis membuat database mysql_innodb_cluster_metadata yang menyimpan seluruh informasi konfigurasi cluster
    
   <img width="1073" height="260" alt="Screenshot (376)" src="https://github.com/user-attachments/assets/c26a3216-f8b9-4448-84ed-e50827941495" />


10. Instalasi MySQL Router di server terpisah. Bertujuan sebagai application gateway agar koneksi tetap avalibale jika salah satu database mati
    
    <img width="724" height="63" alt="Screenshot (1)" src="https://github.com/user-attachments/assets/2b56b89f-48d7-4d4a-9036-27589f39146e" />


11. Konfigurasi mysql router untuk bisa terkoneksi dengan node server cluster. Router akan menyediakan port khusus untuk memisahkan write traffic dan read traffic secara otomatis
    
    <img width="1017" height="464" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/b7af48c5-8c1b-4316-ba80-f8d9324b0869" />
    

12. Verifikasi port menggunakan net-tools untuk memastikan port 6446 (Read-Write) dan 6447 (Read-Only) sudah aktif dan siap melayani aplikasi
   
      <img width="1030" height="519" alt="Screenshot (3)" src="https://github.com/user-attachments/assets/429c6a2c-7a62-4449-a9c8-9557177712bc" />
      <img width="903" height="78" alt="Screenshot (5)" src="https://github.com/user-attachments/assets/f1c80163-fabf-4c7f-9842-cc8bd6f6360d" />
      <img width="997" height="331" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/2e82cd64-1cba-44c5-93e9-51e7dbfd0e66" />


13. Membuat user monitoring aplikasi di primary server. User ini digunakan oleh MySQL Router untuk memantau kesehatan cluster secara real-time
    
    <img width="955" height="178" alt="Screenshot (378)" src="https://github.com/user-attachments/assets/63b00a80-31da-45d6-9d0b-13ad66aa9cf2" />
    <img width="1693" height="257" alt="Screenshot (379)" src="https://github.com/user-attachments/assets/d279a17c-9474-4281-9707-dc2f4322dfc3" />



14. Koneksi client (aplikasi) melalui MySQL client dari server Router. Memastikan aplikasi bisa "berbicara" dengan database melalui path yang benar
    
    <img width="1093" height="223" alt="Screenshot (6)" src="https://github.com/user-attachments/assets/766907c3-a7df-4e4e-8928-5063270c26a1" />    
    <img width="917" height="556" alt="Screenshot (8)" src="https://github.com/user-attachments/assets/7c46690f-574e-4b92-9de5-3738ec986a6f" />
    <img width="826" height="546" alt="Screenshot (9)" src="https://github.com/user-attachments/assets/11917aef-2e83-4b10-922d-ec57da58843d" />


15. Test data consistency dengan membuat data baru di primary dan validasi di standby. Memastikan proses sinkronsisasi data secara real-time guna mendukung high avalibility dan scalability
    
    <img width="730" height="422" alt="Screenshot (10)" src="https://github.com/user-attachments/assets/3a985b43-499a-43fb-aa08-0809899db34b" />
    <img width="858" height="372" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/b9f70306-9167-4776-a202-a727d6bebb49" />


18. Memastikan node standby menolak perintah tulis (write). Menjaga konsistensi data di standby server agar tetap sinkron dengan data primary
    
    <img width="1069" height="453" alt="Screenshot (12)" src="https://github.com/user-attachments/assets/40ed64e5-4804-47a4-b52d-afa7dad4202e" />


19. Melakukan monitoring melalui MySQL Router. Memastikan seluruh sistem (node) berjalan harmonis dalam mode high availability
    
    <img width="1042" height="385" alt="Screenshot (13)" src="https://github.com/user-attachments/assets/56b7ca35-914a-4ee0-8c7e-5a03ad0421e4" />




    







    
    
      

    

    











   

   











