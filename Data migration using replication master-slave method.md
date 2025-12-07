pada sesi ini, saya akan melakukan migrasi data dari server primary ke server standby menggunakan metode replikasi master-slave untuk pengaplikasian migrasi data near zero downtime.
hal ini penting dilakukan untuk memigrasi data critical. hal yang dibutuhkan saat memigrasi data menggunakan metode rpelikasi master-slave:
1. menyediakan 1 server primary dan server standby
2. menggunakan tools xtrabackup
3. mengaktifkan binar log + gtid (global transaction identifier), gtid menjadi dasar yang penting untuk replikasi modern, pemulihan, dan failover.
metode ini berupa metode kelanjutan dari backup & recovery pitr menggunakan xtrabackup + binary log. 


