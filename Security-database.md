pada sesi kali ini, saya akan melakukan pembuatan user, role, dan permission berdasarkan role-based access control (RBAC) dan least privilage untuk menjaga keamanan database server.

dalam testing ini, saya akan membuat user operasional atau pemisahan tugas antara user **reader** and **writer**.


1. login ke dalam database menggunakan user 'root' sebagai user administratif tertinggi database server.
   
   <img width="909" height="466" alt="Screenshot (153)" src="https://github.com/user-attachments/assets/046f4352-73e7-4f3e-9c0e-e0891035346a" />


3. Validasi kebijakan password yang disetujui oleh database server untuk memenuhi standar keamanan.
   - memastikan password yang dibuat untuk user sesuai dengan keamanan database server

     <img width="921" height="126" alt="Screenshot (182)" src="https://github.com/user-attachments/assets/644d0afe-ac83-41ce-b60a-ef98c299c99b" />

   - membuat kemanan menggunakan autentifikasi plugin 
     
     <img width="986" height="287" alt="Screenshot (154)" src="https://github.com/user-attachments/assets/a68f34af-a64b-4986-817c-bd4850c911c8" />
     - length : minimal karakter
     - mix_case_count : minimal 1 huruf besar dan 1 huruf kecil
     - number_count : minimal mempunyai 1 angka
     - special_char : minimal harus mempunyai 1 karakter khusus (@, #, dll)
     - cek_user_name : tidak boleh mengandung nama user


4. Deployment User Operasional Berbasis RBAC
   - memastikan user operasional yang login ke dalam database memiliki hak akses seminimal mungkin.

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


5. Monitoring dan audit identitas user.

   - menampilkan user yang ada di dalam server database
   - memastikan user yang dibuat memiliki autentifikasi security yang disetujui oleh database server
     <img width="921" height="323" alt="Screenshot (167)" src="https://github.com/user-attachments/assets/a17e7c36-0275-42ac-996d-ef2781ef902d" />

   - menampilan user beserta dengan role nya di dalam database server
     <img width="1014" height="387" alt="Screenshot (159)" src="https://github.com/user-attachments/assets/fa005377-e973-413a-86b9-f7b610244edb" />
     - bisa dilakukan yang sama pada user/role yang terdaftar di dalam server





   



     
   
     
     
     



   


