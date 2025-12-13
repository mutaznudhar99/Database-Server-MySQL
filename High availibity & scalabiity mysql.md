pada sesi kali ini, saya akan melakukan high availibility database dan clustering terintegrasi menggunakan beberapa fitur native dari mysql atau disebut dengan innodb cluster mysql. hal ini bertujuan untuk:
- menyediakan ketersediaan database yang tinggi (HA)
- skalabilitas baca
- perpindahan peran secara otomatis (failover automatic)
- memastikan konsistensi data yang kuat

mysql innodb cluster menggabungkan beberapa komponen native, diantaranya:
- mysql group replication, yang terdiri dari beberapa server replikasi (minimal 3) 1 server primary/multi master primary dan beberapa node replika yang dijadikan dalam sebuah kelompok/group
- mysql shell, sebagai command line adminAPI setiap server
- mysql router, sebagai perantara/proxy yang berdiri di antara aplikas(client) dan innodb cluster. berperan menyediakan high availibility dan load balancing.

hal-hal yang perlu dipersiapkan sebelum melakukan clustering data.
- ruang disk yang cukup
- 1 server primary mysql + mysqlshell
- 1 server replica mysql + mysqlshell
- 1 server replica mysql + mysqlshell
- 1 server mysql router


1. memastikan instalasi database mysql server dan mysql shell sudah terinstall di setiap server.
   <img width="1181" height="112" alt="Screenshot (353)" src="https://github.com/user-attachments/assets/ccd4f504-9e4e-426d-8bc6-1c54dc92de82" />
   <img width="1136" height="98" alt="Screenshot (354)" src="https://github.com/user-attachments/assets/63e3d279-761c-48ad-aa45-a7a9b78e0f48" />
   <img width="1217" height="115" alt="Screenshot (355)" src="https://github.com/user-attachments/assets/ad8867a7-1c16-46cf-bfb3-c39efff9c993" />


2. menambahkan ip dan hostname server lain ke dalam node primary.
   - dalam konsep innodb cluster, menambahkan ip dan hostname ke dalam file hosts node primary merupakan cara terbaik untuk mempermudah mysql shell dan mysql router menghubungkan antara node primary dengan node lainnya.
   <img width="781" height="68" alt="Screenshot (358)" src="https://github.com/user-attachments/assets/7db4f29d-2af7-40b1-a307-f1c59b57c105" />
   <img width="1061" height="248" alt="Screenshot (357)" src="https://github.com/user-attachments/assets/a381637d-a9bb-4fb7-89c9-a7f9bf0608b6" />
   <img width="1036" height="122" alt="Screenshot (359)" src="https://github.com/user-attachments/assets/d9684d7f-04b2-41ea-813d-b53253427eea" />
   <img width="819" height="119" alt="Screenshot (360)" src="https://github.com/user-attachments/assets/4106103f-30e9-49d0-a442-c04535e77a93" />


3. login ke dalam database server menggunakan mysqlsh/mysql shell dan mengkonfigurasinya untuk mendaftarkan server menjadi bagian dari innodb cluster. hal ini dilakukan pada setiap server yang akan menjadi bagian dari innodb cluster.
   <img width="1281" height="527" alt="Screenshot (362)" src="https://github.com/user-attachments/assets/a0b932c5-1cbd-4554-85d8-80d7374707f8" />
   - perintah selanjutnya, direkomendasikan untuk membuat user baru sebagai pengubung antar node server. karena user root tidak bisa diandalkan apabial hanya dalam cluster localhost.
   <img width="1345" height="540" alt="Screenshot (363)" src="https://github.com/user-attachments/assets/9f9ef4f1-748f-4856-bb23-1f50e7a68263" />
   - mengaktifkan konfigurasi parameter database seperti binlog dan gtid sebagai salah satu pra-syarat melakukan innodb cluster.
   <img width="1322" height="442" alt="Screenshot (365)" src="https://github.com/user-attachments/assets/59f17db6-b9fa-461e-9242-e1bfd5f0b22e" />


4. login kembali ke database server menggunakan user cluster yang sudah dibuat sebelumnya dengan mysqlsh/mysqlshell
   <img width="1699" height="788" alt="Screenshot (369)" src="https://github.com/user-attachments/assets/fc8d31ec-5b32-43df-8092-1046ac3f6975" />
   - melakukan hal di atas pada setiap server database yang sudah didaftarkan untuk innodb cluster
   - memastikan status database server status **ok**



5. selanjutnya membuat cluster di server primary menggunakan mysqlsh sebagai administrasi api innodb cluster
   <img width="1338" height="525" alt="Screenshot (372)" src="https://github.com/user-attachments/assets/76f22407-8e30-4c55-987e-bc28ab773eb2" />


6. cek status server primary
   <img width="1380" height="757" alt="Screenshot (373)" src="https://github.com/user-attachments/assets/ac2db45c-5007-411a-829f-3722d8f41b0f" />
   - memastikan status server sebagai server primary untuk clustering.


7. menambahkan server database lain ke dalam cluster
   <img width="1595" height="882" alt="Screenshot (374)" src="https://github.com/user-attachments/assets/e32b4463-4312-4c80-8069-0bccd6c7c4fb" />
   - melakukan hal yang sama pada server lain yang akan ditambahkan ke dalam cluster


8. cek status server yang baru saja ditambahkan ke dalam cluster
   <img width="1080" height="708" alt="Screenshot (375)" src="https://github.com/user-attachments/assets/fe6361c9-d342-482a-b861-3d3fff3ea926" />
   - member_role = status database server
   - mode = R/W (read-write) untuk server primary, dan R/0 (read only) untuk server standby
   - cek ulang status saat menambahkan server baru ke dalam cluster


9. setelah membuat cluster dan menambahkan seluruh server ke dalam cluster, database server secara otomatis akan membuat database/schema baru **mysql_innodb_cluster_metadata** sebagai single source of truth yang menyimpan semua informasi kritis mengenai cluster yang dibuat.
   <img width="1073" height="260" alt="Screenshot (376)" src="https://github.com/user-attachments/assets/c26a3216-f8b9-4448-84ed-e50827941495" />


10. selanjutnya, instal mysql router di server berbeda dari node server. bertujuan supaya router tetap aktif apabila salah satu node server mengalami crash/kerusakan system.
    <img width="724" height="63" alt="Screenshot (1)" src="https://github.com/user-attachments/assets/2b56b89f-48d7-4d4a-9036-27589f39146e" />


11. konfigurasi mysql router untuk bisa terhubung ke node server cluster yang sudah ada
    <img width="1017" height="464" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/b7af48c5-8c1b-4316-ba80-f8d9324b0869" />
    - mysql classic protocol menunjukkan, konfigurasi mysql router telah berhasil dilakukan untuk menghubungkan aplikasi(client) kepada port lokal sebagai server read-write atau read-only untuk mendapatkan fitur high availibility dan load balancing.


12. selanjutnya verifikasi jaringan mysql protocol apakah sudah benar-benar dijalankan oleh mysql router
    - install net-tools
      <img width="1030" height="519" alt="Screenshot (3)" src="https://github.com/user-attachments/assets/429c6a2c-7a62-4449-a9c8-9557177712bc" />

    - cek jaringan mysql protocol untuk memastikan sudah berjalan di atas program mysql router
      <img width="903" height="78" alt="Screenshot (5)" src="https://github.com/user-attachments/assets/f1c80163-fabf-4c7f-9842-cc8bd6f6360d" />
      <img width="997" height="331" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/2e82cd64-1cba-44c5-93e9-51e7dbfd0e66" />


13. membuat user aplikasi baru dengan all permission seperti root di server primary untuk digunakan oleh mysql router sebagai user utama untuk monitoring cluster dari mysql router.
    <img width="955" height="178" alt="Screenshot (378)" src="https://github.com/user-attachments/assets/63b00a80-31da-45d6-9d0b-13ad66aa9cf2" />
    <img width="1693" height="257" alt="Screenshot (379)" src="https://github.com/user-attachments/assets/d279a17c-9474-4281-9707-dc2f4322dfc3" />



14. install mysql client di server mysql router untuk terkoneksi ke dalam mysql protocol read-write atau read-only.
    <img width="1093" height="223" alt="Screenshot (6)" src="https://github.com/user-attachments/assets/766907c3-a7df-4e4e-8928-5063270c26a1" />


15. login ke dalam database node server primary dari mysql router menggunakan host ip lokal dan port dari mysql protocol.
    <img width="917" height="556" alt="Screenshot (8)" src="https://github.com/user-attachments/assets/7c46690f-574e-4b92-9de5-3738ec986a6f" />
    <img width="826" height="546" alt="Screenshot (9)" src="https://github.com/user-attachments/assets/11917aef-2e83-4b10-922d-ec57da58843d" />


16. uji database server node primary dengan membuat schema baru dan cek konsistensi data di server node standby.
    <img width="730" height="422" alt="Screenshot (10)" src="https://github.com/user-attachments/assets/3a985b43-499a-43fb-aa08-0809899db34b" />
    <img width="858" height="372" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/b9f70306-9167-4776-a202-a727d6bebb49" />


17. cek database server node standby dalam mode read-only atau read-write untuk memastikan server node standby hanya dalam mode read-only.
    <img width="1069" height="453" alt="Screenshot (12)" src="https://github.com/user-attachments/assets/40ed64e5-4804-47a4-b52d-afa7dad4202e" />


18. monitoring cluster di server mysql router menggunakan database server read-only
    <img width="1042" height="385" alt="Screenshot (13)" src="https://github.com/user-attachments/assets/56b7ca35-914a-4ee0-8c7e-5a03ad0421e4" />





##secara teknis, innodb cluster bisa dilakukan. untuk level enterprise perlu mempertimbangkan hal fundamental seperti keamanan, kemudahan pengoperasian dan sebagainya.

    







    
    
      

    

    











   

   











