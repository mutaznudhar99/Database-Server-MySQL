pada sesi kali ini, saya akan melakukan high availibility database dan clustering terintegrasi menggunakan beberapa fitur native dari mysql atau disebut dengan innodb cluster mysql. hal ini bertujuan untuk:
- menyediakan ketersediaan database yang tinggi (HA)
- skalabilitas baca
- perpindahan peran secara otomatis (failover automatic)
- memastikan konsistensi data yang kuat

mysql innodb cluster menggabungkan beberapa komponen native, diantaranya:
- mysql group replication, yang terdiri dari beberapa server replikasi (minimal 3) 1 server primary/multi master primary dan beberapa node replika yang dijadikan dalam sebuah kelompok/group
- mysql shell, sebagai command line administrasi setiap server
- mysql router, sebagai penghubung/connection pooling antara client dengan node database server.

hal-hal yang perlu dipersiapkan sebelum melakukan clustering data.
- 1 server primary mysql + mysqlshell
- 1 server replica mysql + mysqlshell
- 1 server replica mysql + mysqlshell
- 1 server mysql router




