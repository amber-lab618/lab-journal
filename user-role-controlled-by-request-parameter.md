## Server-side vulnerabilities

### Lab: user role controlled by request parameter


#### Tujuan:
- mengakses admin panel dengan menggunakan akun `wiener:peter`
- menghapus akun carlos


#### Langkah-langkah
- buka burpsuite -> open browser -> salin dan tempel alamat labnya
- klik `My account` lalu aktifkan intercept di burpsuite
- masukkan `username:wiener` dan `password:peter` lalu login
- pada bagian response intercept ubah `Admin=false` menjadi `Admin=true` lalu forward
- setelah itu di browser akan muncul admin panel, klik `admin panel` dan di intercept akan muncul respon `GET /admin` lalu forward
- matikan intercept lalu cari di http history request `/admin` dan send to repeater
- di tab repeater ubah `Admin=false` menjadi `Admin=true` lalu send
- setelah itu klik kanan di kolom request dan pilih `request in browser > in original session`, salin url lalu tempel di browser
- klik admin panel dan akan muncul nama user dan tombol delete, klik `delete` pada akun carlos
- kembali ke burpsuite, cari di http history request `/admin/delete?username=carlos` lalu send to repeater
- di tab repeater ubah `Admin=false` menjadi `Admin=true` lalu send
- request berhasil dan akun carlos berhasil dihapus


#### Note:
- untuk bisa melakukan delete akun pada admin panel harus melakukan beberapa kali request menggunakan repeater, karena Cookie: Admin=false yang telah diubah menjadi true akan direset setelah satu kali request sehingga saat kita akan klik admin panel atau delete akun pasti tidak bisa karena cookie nya bernilai false. setelah diubah menjadi true maka request nya berhasil dan akun carlos terhapus
- kenapa pakai browser bawaan burpsuite karena menurut saya lebih fokus karena http history tidak tercampur dengan iklan atau request-request yang berasal dari program sebuah website
- jika terasa ribet bisa memakai foxyproxy, sebuah extention di firefox yang dapat menghubungkan request sebuah website langsung di burpsuite
