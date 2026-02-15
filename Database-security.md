pada sesi kali ini, saya akan mengimplementasikan database security melalui manajemen identitas yang ketat, kebijakan password complexity, dan pemisahan tugas (separation of duties) antara user Reader dan Writer


1. Menggunakan akun root sebagai otoritas tertinggi untuk mengelola seluruh konfigurasi security dan privileges pada database server
   
   <img width="909" height="466" alt="Screenshot (153)" src="https://github.com/user-attachments/assets/046f4352-73e7-4f3e-9c0e-e0891035346a" />


2. Melakukan verifikasi parameter validate_password. Memastikan kredensial user memenuhi standar keamanan panjang karakter, penggunaan angka, huruf besar/kecil, dan karakter spesial

     <img width="921" height="126" alt="Screenshot (182)" src="https://github.com/user-attachments/assets/644d0afe-ac83-41ce-b60a-ef98c299c99b" />     
    <img width="986" height="287" alt="Screenshot (154)" src="https://github.com/user-attachments/assets/a68f34af-a64b-4986-817c-bd4850c911c8" />
     - length : minimal karakter
     - mix_case_count : minimal 1 huruf besar dan 1 huruf kecil
     - number_count : minimal mempunyai 1 angka
     - special_char : minimal harus mempunyai 1 karakter khusus (@, #, dll)
     - cek_user_name : tidak boleh mengandung nama user


3. Deployment role  reader-writer & user operasional. Menerapkan prinsip least privilege dengan memisahkan hak akses

   <img width="1106" height="413" alt="Screenshot (157)" src="https://github.com/user-attachments/assets/07319fc3-829f-4627-9169-efc9186c5b96" />
   <img width="990" height="386" alt="Screenshot (158)" src="https://github.com/user-attachments/assets/cfb56c2e-4525-418f-9e70-084bd8201734" />


4. Melakukan monitoring pada tabel sistem. Memastikan semua user terdaftar dengan plugin autentikasi yang aman (seperti caching_sha2_password)

     <img width="921" height="323" alt="Screenshot (167)" src="https://github.com/user-attachments/assets/a17e7c36-0275-42ac-996d-ef2781ef902d" />


5. Menampilkan pemetaan antara user dan role. Memastikan tidak ada hak akses berlebih (Privilege Creep) di dalam database server.

   <img width="1014" height="387" alt="Screenshot (159)" src="https://github.com/user-attachments/assets/fa005377-e973-413a-86b9-f7b610244edb" />





   



     
   
     
     
     



   


