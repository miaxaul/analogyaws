# analogyaws
analogy service aws in real life


# 1. CloudFormation
Analogi: Rencana pembangunan restoran.
Penjelasan: Kamu bikin blueprint restoran, seperti lokasi pintu, dapur, meja makan, dan toilet. Semua ini otomatis dibuat sesuai rancangan.
Contoh: Kamu membuat template untuk restoran dengan:
	Nama: "Restoran Sederhana"
	Alamat: 10.10.0.0/16 (IP jaringan VPC)
	Ruangan: Dapur (10.10.1.0/24), ruang makan (10.10.2.0/24).


# 2. VPC (Virtual Private Cloud)
Analogi: Bangunan restoran dan pagarnya.
Penjelasan: Restoran ini punya area privat yang cuma bisa diakses orang tertentu, aman dari gangguan luar.
Contoh:
	Nama VPC: "RestoranPrivat"
	IP CIDR: 10.10.0.0/16 (area besar restoran).


# 3. Subnet
Analogi: Ruangan di restoran, seperti dapur, ruang makan, atau gudang.
Penjelasan: Setiap ruangan punya fungsi tertentu. Dapur (subnet privat), ruang makan (subnet publik).
Contoh:
	Subnet Dapur: 10.10.1.0/24 (private).
	Subnet Ruang Makan: 10.10.2.0/24 (public).


# 4. Internet Gateway (IGW)
Analogi: Pintu utama restoran.
Penjelasan: Pelanggan bisa datang lewat pintu ini.
Contoh:
	Nama IGW: "IGW-Restoran".
	Route Table: Semua trafik dari luar diarahkan ke 10.10.2.0/24 (ruang makan).


# 5. Route Table
Analogi: Papan petunjuk arah di restoran.
Penjelasan: Mengatur siapa bisa ke mana. Misalnya pelanggan ke ruang makan, supplier ke gudang.
Contoh:
	Route ke Public: 0.0.0.0/0 → Internet Gateway.
	Route ke Private: Tidak ada akses langsung (melalui NAT Gateway).


# 6. NAT Gateway
Analogi: Pintu belakang restoran untuk staf.
Penjelasan: Memungkinkan staf (dapur/subnet privat) memesan bahan tanpa terlihat pelanggan.
Contoh:
	Nama NAT: "NAT-Dapur".
	Route Table: Subnet dapur (10.10.1.0/24) ke NAT.


# 7. Security Group
Analogi: Satpam restoran.
Penjelasan: Menentukan siapa yang boleh masuk dan keluar restoran.
Contoh:
	Inbound (masuk):
	Port 80 (HTTP): Akses ke ruang makan untuk pelanggan.
	Port 22 (SSH): Akses admin untuk staf IT.
	Outbound (keluar):
	Semua trafik diizinkan untuk staf restoran.
	

# 8. RDS (Relational Database Service)
Analogi: Buku kas besar restoran.
Penjelasan: Di sini kamu mencatat data pelanggan, pesanan, dan transaksi keuangan.
Contoh:
	Nama Database: "KasRestoran".
	Tipe: MySQL.
	Akses IP: Hanya subnet privat (10.10.1.0/24).


# 9. DynamoDB
Analogi: Whiteboard dapur untuk mencatat pesanan harian.
Penjelasan: Data simpel yang nggak perlu hubungan rumit, seperti daftar pesanan hari ini.
Contoh:
	Tabel: PesananHariIni.
	Kolom: Nama, Pesanan, Status.
	

# 10. S3 (Simple Storage Service)
Analogi: Lemari arsip restoran.
Penjelasan: Tempat menyimpan dokumen penting, seperti foto makanan, laporan bulanan, atau video promosi.
Contoh:
	Bucket Name: "Dokumen-Restoran".
	Akses: Publik (untuk promosi), Privat (untuk dokumen internal).


# 11. EFS (Elastic File System)
Analogi: Laci penyimpanan yang bisa diakses semua staf.
Penjelasan: Semua staf (admin, pelayan, dapur) bisa buka file yang sama.
Contoh:
	Folder: "MenuRestoran".
	Akses: Subnet dapur, ruang makan, dan kasir.


# 12. Lambda
Analogi: Robot chef yang aktif pas ada pesanan.
Penjelasan: Nggak standby terus, tapi langsung kerja begitu dibutuhkan.
Contoh:
	Runtime : Python 3.10 (kode etik)
	Trigger: Ada pesanan baru (data masuk ke DynamoDB).
	Tugas(script code): Kirim notifikasi ke dapur.


# 13. API Gateway
Analogi: Pelayan restoran.
Penjelasan: Mengambil pesanan dari pelanggan, menyampaikannya ke dapur, lalu mengembalikan hasil ke pelanggan.
Contoh:
	Endpoint: /pesanan.
	Method: GET (lihat pesanan), POST (buat pesanan).


# 14. SNS (Simple Notification Service)
Analogi: Sistem pengumuman restoran.
Penjelasan: Umumin hal penting ke banyak orang, seperti "Promo Diskon 50%" atau "Pesanan meja 5 sudah siap".
Contoh:
	Topic: PengumumanPromo.
	Subscriber: Email pelanggan.


# 15. SQS (Simple Queue Service)
Analogi: Antrian pesanan di dapur.
Penjelasan: Memastikan dapur kerjain pesanan satu per satu tanpa rebutan.
Contoh:
	Queue Name: AntrianPesanan.
	Isi: "Pesanan Nasi Goreng untuk Meja 5".


# 16. IAM (Identity and Access Management)
Analogi: Sistem kartu akses dan job desk karyawan restoran.
Penjelasan: Semua karyawan punya peran masing-masing. Ada yang hanya bisa masuk dapur, ada yang khusus kasir, atau manager yang punya akses ke semua ruangan.
Contoh:
	Roles:
		Chef: Bisa akses dapur dan buku resep (RDS).
		Kasir: Bisa akses data pelanggan (DynamoDB).
		Manager: Punya akses penuh ke semua bagian restoran.
	Policy:
		Chef hanya bisa baca dan tulis file di EFS (resep).
		Kasir hanya bisa baca data di RDS (transaksi).
👉 Fungsi: Mengelola siapa yang bisa melakukan apa dan di mana di restoranmu.


# 17. EC2 (Elastic Compute Cloud)
Analogi: Kompor atau oven di dapur restoran.
Penjelasan: Kompor ini adalah alat utama yang digunakan untuk memasak makanan. Bisa dihidupkan dan dimatikan sesuai kebutuhan. Kalau pesanan banyak, kamu bisa tambahkan kompor baru (scale out).
Contoh:
	Nama Kompor: "KomporUtama".
	IP: 10.10.1.5 (terhubung ke subnet dapur).
	Tipe : Kompor gas versi terbaru
	Inbound Rules:
		Port 22 (SSH): Hanya untuk admin restoran.
		Port 80 (HTTP): Untuk pelanggan yang memesan makanan online.
		Outbound Rules: Semua trafik diizinkan (misalnya untuk pesan bahan dari supplier).
👉 Fungsi: Menyediakan sumber daya komputasi (seperti server untuk aplikasi web restoranmu).




# Alur Operasional Restoran (Final Version)
Bangun restoran:

CloudFormation: Membuat blueprint restoran dengan semua ruangan dan fasilitasnya.
Siapkan area restoran:

VPC: Bangunan restoran lengkap dengan pagar (area privat).
Subnet: Membagi ruangan ke dapur, ruang makan, dan gudang.
Route Table: Petunjuk jalan di restoran (pelanggan ke ruang makan, staf ke dapur).
Pasang pintu akses:

IGW: Pintu utama untuk pelanggan.
NAT Gateway: Pintu belakang untuk staf memesan bahan.
Tentukan akses masuk dan keluar:

Security Group: Satpam menjaga keamanan, hanya pelanggan dan staf tertentu yang bisa masuk.
Operasional restoran:

IAM: Mengatur peran dan akses setiap karyawan (chef, kasir, manager).
EC2: Kompor atau oven sebagai mesin utama restoran (server aplikasi).
Kelola data:

RDS: Buku kas besar untuk mencatat data pelanggan dan transaksi.
DynamoDB: Whiteboard dapur untuk mencatat pesanan harian.
S3: Lemari arsip untuk dokumen dan file promosi.
EFS: Laci bersama untuk file yang bisa diakses semua staf.
Otomatisasi proses:

Lambda: Robot chef yang aktif pas ada pesanan, misalnya memproses pesanan online.
API Gateway: Pelayan yang menyampaikan pesanan pelanggan ke dapur.
Komunikasi dan antrian:

SNS: Sistem pengumuman untuk promo atau info penting.
SQS: Sistem antrian pesanan di dapur, supaya semuanya rapi dan terorganisir.
Contoh Konfigurasi Sederhana (Bagian IP dan Aturan):

Subnet Ruang Makan (Public):

IP: 10.10.2.0/24.
Route ke IGW untuk akses pelanggan.
Security Group: Port 80 (HTTP) untuk pelanggan.
Subnet Dapur (Private):

IP: 10.10.1.0/24.
Route ke NAT Gateway untuk akses keluar.
Security Group: Port 22 (SSH) untuk admin, Port 2049 (NFS) untuk EFS.
Kompor EC2:

Nama: "ServerPesanan".
IP: 10.10.1.5.
Inbound: SSH (admin), HTTP (pelanggan).
Dengan alur ini, restoranmu jadi sistematis, efisien, dan aman! 😊
