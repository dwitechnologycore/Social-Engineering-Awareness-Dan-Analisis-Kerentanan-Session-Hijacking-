## Social Engineering Awareness Dan Analisis Kerentanan Session Hijacking 
> Evaluasi ini menyoroti berbagai teknik manipulasi seperti phishing, typosquatting, dan pretexting. 
Analisis dilakukan di lingkungan yang telah ditetapkan dan berizin. Pemeriksaan meliputi keamanan koneksi, pengelolaan session ID, konfigurasi cookie, masa aktif sesi, proses logout, dan validasi sesi. Tujuannya adalah mengungkap celah yang bisa dimanfaatkan oleh pihak tak berwenang untuk mengambil alih sesi pengguna.
<br></br>
### Ringkasan
- [Tools](#tools)
- [Tujuan evaluasi](#tujuan-evaluasi)
- [Hasil Temuan](#hasil-temuan)
- [Rekomendasi Mitigasi](#rekomendasi-mitigasi)
- [Kesimpulan](#kesimpulan)
<br></br>
### Tools
- [cURL](https://www.kali.org/tools/curl/)
- [FFuF](https://www.kali.org/tools/ffuf/)
- [web server simulasi](https://website-cybersecurity-testing-lab--bagussudung1602.replit.app/login)
- [Web Server Phising](https://website-cybersecurity-testing-lab--bagussudung1602.replit.app/it-update)
- [Web Server Kerentanan](https://website-cybersecurity-testing-lab--bagussudung1602.replit.app/security/session-audit)
- [Web Server Pemantauan sesi](https://website-cybersecurity-testing-lab--bagussudung1602.replit.app/security/session-audit )
<br></br>
### Tujuan Evaluasi
1.	Mengukur tingkat pemahaman anggota divisi terhadap konsep social engineering, termasuk phishing, impersonation, baiting, pretexting, dan bentuk manipulasi lainnya.
2.	Menilai kemampuan dalam mengenali indikator serangan, seperti tautan mencurigakan, permintaan kredensial, pesan yang mendesak, identitas pengirim palsu, serta aktivitas login yang tidak wajar.
3.	Mengevaluasi kepatuhan terhadap prosedur keamanan, terutama dalam penggunaan kata sandi, autentikasi multifaktor, pengelolaan session, pelaporan insiden, dan perlindungan informasi sensitif.
4.	Mengidentifikasi kerentanan session hijacking pada sistem yang digunakan, termasuk kelemahan pada cookie session, pengaturan atribut Secure dan HttpOnly, durasi session, mekanisme logout, serta validasi session ID.
5.	Menganalisis potensi dampak serangan session hijacking, seperti pengambilalihan akun, akses tanpa izin, pencurian data, perubahan informasi, dan penyalahgunaan hak akses.
6.	Menilai efektivitas kontrol keamanan yang telah diterapkan dalam mencegah, mendeteksi, dan menangani serangan social engineering maupun session hijacking.
7.	Mengukur perubahan perilaku keamanan anggota, misalnya kemampuan untuk tidak membuka tautan mencurigakan, tidak memberikan kredensial, serta melaporkan indikasi serangan dengan cepat. Kecepatan dan kualitas pelaporan merupakan indikator penting dalam menilai keberhasilan program awareness.
8.	Menentukan tingkat risiko keamanan berdasarkan hasil simulasi, observasi, pengujian kerentanan, dan analisis terhadap sistem.
9.	Menyusun rekomendasi mitigasi baik dari segi aspek teknis aplikasi maupun tata kelola dan SDM.
<br></br>
### Hasil Temuan
Pengecekan dilakukan melalui terminal Kali Linux dengan perintah curl untuk mentransfer data dari server menggunakan sintaks URL. Kemudian dengan menggunakan Fuzz Faster U Foll (FFUF) untuk memeriksa direktori atau file tersembunyi di website tersebut. Ditemukan 3 kejanggalan dari website yakni 

1. Logo yang digunakan mengarah ke sumber mencurigakan: src = "https://cdn.totally-not-phishing-assets.ru/logo-tas.png". bukan  src="https://tas-corp.id/assets/images/logo-official.png"
2. Pelaku menggunakan prinsip pretexting, yakni Scarcity & Urgency Attack. Di mana pelaku menyamar sebagai sumber domain resmi untuk meyakinkan bahwa akun target akan segera dihapus disertai penghitung waktu mundur (countdown)  untuk membuat target merasa panik, takut dan ingin segera mengganti password mereka.
3. Pada teks bagian bawah (footer) halaman, terdapat kesalahan penulisan nama entitas korporat (typosquatting), yakni teknologi menjadi teknolgi.

Setelah dilakukan uji coba pengiriman data formulir dengan data sembarang ke web simulasi, terjadi pengalihan halaman ke [portal edukasi](https://website-cybersecurity-testing-lab--bagussudung1602.replit.app/awareness-education) . Kemudian pada [dashboard web simulasi](https://website-cybersecurity-testing-lab--bagussudung1602.replit.app/security/campaign-dashboard) terdapat beberapa metrik yang terdiri dari total interaksi/kunjungan, banyak karyawan yang terjebak/kredensial terinput beserta tingkat kompromi, banyak karyawan yang melaporkan (vigilant) dan tingkat pelaporan, serta banyak karyawan yang membaca edukasi. Data-data yang ada dapat digunakan untuk mengetahui seberapa banyak karyawan yang memiliki edukasi mengenai phishing serta seberapa banyak karyawan yang memiliki tingkat kesadaran mengenai phishing.

Selanjutnya dilakukan pengujian Manajemen Sesi Dan Kerentanan Session Hijacking dengan menyuntikkan cookie pada [web simulasi](https://website-cybersecurity-testing-lab--bagussudung1602.replit.app/login) untuk mencoba masuk tanpa melakukan login. Hasil uji coba menunjukkan bahwa web sedang tidak berada dalam status HttpOnly, hal ini menyebabkan penyerang dapat mengakses data pengguna dengan mudah. 
<br></br>
### Rekomendasi Mitigasi
- Terapkan konfigurasi cookie sesi yang aman dengan mengaktifkan atribut HttpOnly, Secure, SameSite=Strict.
- Terapkan mekanisme Session Invalidation dan Session Regeneration saat proses login untuk mencegah serangan Session Fixation.
- Terapkan mekanisme Session Binding (mengikat token sesi dengan sidik jari IP atau perangkat pengguna).
- Buat Prosedur standar operasional (SOP) bagi pegawai ketika menerima komunikasi mencurigakan yang mengatasnamakan bagian TI internal.
- Buat Kebijakan pelaksanaan simulasi phishing berkala dan evaluasi kesadaran keamanan tanpa sistem hukuman (no-blame culture).
<br></br>
### Kesimpulan 
Praktik ini menunjukkan bahwa pembajakan sesi dapat terjadi karena kombinasi kesalahan manajemen sesi, seperti tidak adanya regenerasi session ID dan penggunaan cookie tanpa fitur keamanan, serta faktor manusia dari segi manipulasi psikologis, phishing maupun rekayasa sosial. Pelaku dapat memanfaatkan dan menggunakan kembali session ID untuk mengambil alih akun pengguna. Rekomendasi yang diperlukan untuk pencegahan yang efektif dari segi teknis aplikasi meliputi penggunaan atribut HttpOnly, Secure, dan SameSite, regenerasi ID sesi, serta penggunaan batas waktu (timeout). Sedangkan dari segi tata kelola dan SDM, rekomendasi yang efektif ialah membuat prosedur standar operasional (SOP) bagi pegawai ketika menerima komunikasi mencurigakan yang mengatasnamakan bagian TI internal serta membuat Kebijakan pelaksanaan simulasi phishing berkala dan melakukan evaluasi kesadaran keamanan tanpa sistem hukuman (no-blame culture).
