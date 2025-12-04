setelah sebelumnya saya sudah melakukan instalasi database mysql server dan mengkonfigurasinya dalam perilaku database server yang sudah disesuaikan dengan kebutuhan database non produksi, selanjutnya saya akan membuat user, permission, dan security database yang bisa dilakukan pada level middle-enterprise.

dalam individual project ini, saya tidak akan melakukan operasional database menggunakan user 'root'. saya akan membuat user operasional yang dibagi menjadi user **read** and **writer**.


1. login ke dalam database menggunakan user 'root' karena belum membuat user operasional lain.
   <img width="909" height="466" alt="Screenshot (153)" src="https://github.com/user-attachments/assets/046f4352-73e7-4f3e-9c0e-e0891035346a" />


2. cek validasi password yang disetujui oleh server saat membuat user
   - memastikan password yang dibuat untuk user sesuai dengan keamanan database server
   - membuat kemanan menggunakan autentifikasi plugin
     
     <img width="986" height="287" alt="Screenshot (154)" src="https://github.com/user-attachments/assets/a68f34af-a64b-4986-817c-bd4850c911c8" />
     - length : minimal karakter
     - mix_case_count : minimal 1 huruf besar dan 1 huruf kecil
     - number_count : minimal mempunyai 1 angka
     - special_char : minimal harus mempunyai 1 karakter khusus (@, #, dll)
     - cek_user_name : tidak boleh mengandung nama user


4. selanjutnya membuat user operasional dengan role **user_reader** dan **user_writer** menggunakan validasi password yang disetujui oleh server.
   - memastikan user operasional yang login ke dalam database server seminimal mungkin diberikan izin hak akses.
   - merupakan security database level middle enterprise yang wajib diterapkan.

     - user 'reporting' dengan role 'user_reader'
       <img width="1106" height="413" alt="Screenshot (157)" src="https://github.com/user-attachments/assets/07319fc3-829f-4627-9169-efc9186c5b96" />

     - user 'transaction' dengan role 'user_writer'
       <img width="990" height="386" alt="Screenshot (158)" src="https://github.com/user-attachments/assets/cfb56c2e-4525-418f-9e70-084bd8201734" />
       - grant : memberikan izin kepada user untuk level tertentu
       - *.* : database.table
       - set default role : mengaktifkan peran secara default ketika user login
       - 'localhost' : digunakan untuk mengizinkan koneksi dari aplikasi yang berjalan di server fisik yang sama dengan database server
       - '%' : digunakan untuk mengizinkan koneksi dari ip atau nama host manapun di jaringan
       - specific ip : digunakan hanya untuk mengizinkan koneksi dari satu alamat ip yang ditentukan


5. Monitoring user exist/yang ada di dalam database server beserta dengan role nya.

   - menampilkan user yang ada di dalam server database
     <img width="841" height="300" alt="Screenshot (161)" src="https://github.com/user-attachments/assets/3f3ab12c-ccfb-4821-b273-975457715780" />

   - menampilan user beserta dengan role nya di dalam database server
     <img width="1014" height="387" alt="Screenshot (159)" src="https://github.com/user-attachments/assets/fa005377-e973-413a-86b9-f7b610244edb" />
     - bisa dilakukan yang sama pada user/role yang terdaftar di dalam server


6. di level enterprise, bisa menambahkan enkripsi data seperti (SSL/TLS) dan audit.log untuk melindungi komunikasi internet antara client (aplikasi) dengan database server dan melakukan semua pencatatan aktifitas yang dilakukan oleh user. 
     
     
     



   


