class produk:
  def __init__(self, kode, nama, kategori, harga):
    self.kode = kode
    self.nama = nama
    self.kategori = kategori
    self.harga = harga

  def __str__ (self):
    return f"[{self.kode}] {self.nama:15} | {self.kategori:10} | Rp{self.harga:,}"

class TokoManager:
  def __init__(self):
    self.katalog = [] # Topik 1: list of Objects

  def tambah_produk(self, produk_baru): # Topik 2: Create
    self.katalog.append(produk_baru)

  def lihat_semua(self): # Tpokik 3: Read
    for p in self.katalog:
      print(p)

  def cari_by_kode(self, kode_cari): # Topik 4: Searching
    for p in self.katalog:
      if p.kode == kode_cari:
        return p
    return None

  def filter_by_kategori(self, kat): # Topik 5: Filtering
      hasil = [p for p in self.katalog if p.kategori.lower() == kat.lower()]
      return hasil

store = TokoManager()
store.tambah_produk(Produk("A01", "Laptop", "Elektronik", 7000000))
store.tambah_produk(Produk("B05", "Meja Kerja", "Furniture", 500000))
store.tambah_produk(Produk("A02", "Mouse", "Elektronik", 150000))

print("Semua Produk:")
store.lihat_semua()

print("\nFilter Elektronik:")
for p in store.filter_by_kategori("Elektronik"):
    print(p)


class Karyawan:
  def __init__(self, nik, nama, jabatan):
    self.nik = nik
    self.nama = nama
    self.jabatan = jabatan

    def __str__(self):
        # Agar saat diprint muncul teks yang rapi, bukan alamat memori
        return f"{self.nik:<10} | {self.nama:<15} | {self.jabatan:,}"


class SistemKaryawan:
    def __init__(self):
        self.database_karyawan = []

    def tambah_karyawan(self, karyawan_baru):
        # Cek apakah NIK sudah ada
        untuk_cek = self.cari_karyawan(karyawan_baru.nik)
        if untuk_cek:
            print("❌ Gagal: NIK sudah terdaftar!")
        else:
            self.database_karyawan.append(karyawan_baru)
            print("✅ Karyawan berhasil ditambahkan!")

    def cari_karyawan(self, nik_cari):
        for k in self.database_karyawan:
            if k.nik == nik_cari:
                return k
        return None

    def tampilkan_semua(self):
        print("\n=== DAFTAR KARYAWAN ===")
        print(f"{'NIK':<10} | {'Nama':<15} | {'Jabatan'}")
        print("-" * 40)
        for k in self.database_karyawan:
            print(k)

  sistem = SistemKaryawan()

while True:
    print("\n1. Tambah Karyawan")
    print("2. Cari Karyawan (NIK)")
    print("3. Lihat Semua")
    print("4. Keluar")
    pilihan = input("Pilihan: ")

    if pilihan == "1":
        nik = input("Masukkan NIK: ")
        nama = input("Masukkan Nama: ")
        jabatan = input("Masukkan Jabatan: ")
        # Buat objek baru dan masukkan ke sistem
        baru = Karyawan(nik, nama, jabatan)
        sistem.tambah_karyawan(baru)

    elif pilihan == "2":
        nik_cari = input("Masukkan NIK yang dicari: ")
        hasil = sistem.cari_karyawan(nik_cari)
        if hasil:
            print(f"Data ditemukan: {hasil}")
        else:
            print("Data tidak ditemukan.")

    elif pilihan == "3":
        sistem.tampilkan_semua()

    elif pilihan == "4":
        print("Terima kasih!")
        break

class Karyawan:
    def __init__(self, nik, nama, jabatan):
        self.nik = nik
        self.nama = nama
        self.jabatan = jabatan


class SistemKaryawan:
    def __init__(self):
        self.database_karyawan = []

    def tambah_karyawan(self, karyawan_baru):
        # Validasi NIK tidak boleh duplikat
        if any(k.nik == karyawan_baru.nik for k in self.database_karyawan):
            print("❌ Error: NIK sudah terdaftar!")
        else:
            self.database_karyawan.append(karyawan_baru)
            print("✅ Karyawan berhasil ditambahkan!")

    def cari_karyawan(self, nik):
        for karyawan in self.database_karyawan:
            if karyawan.nik == nik:
                return karyawan
        return None

    def tampilkan_semua(self):
        print("\n--- Data Karyawan ---")
        if not self.database_karyawan:
            print("Belum ada data.")
        else:
            print(f"{'NIK':<10} | {'Nama':<15} | {'Jabatan':<10}")
            print("-" * 40)
            for k in self.database_karyawan:
                print(f"{k.nik:<10} | {k.nama:<15} | {k.jabatan:<10}")


# Program utama (menu sederhana)
sistem = SistemKaryawan()

while True:
    print("\n1. Tambah Karyawan")
    print("2. Cari Karyawan (NIK)")
    print("3. Lihat Semua")
    print("4. Keluar")

    pilihan = input("Pilihan: ")

    if pilihan == "1":
        nik = input("Masukkan NIK: ")
        nama = input("Masukkan Nama: ")
        jabatan = input("Masukkan Jabatan: ")

        karyawan = Karyawan(nik, nama, jabatan)
        sistem.tambah_karyawan(karyawan)

    elif pilihan == "2":
        nik = input("Masukkan NIK yang dicari: ")
        hasil = sistem.cari_karyawan(nik)

        if hasil:
            print("\nData ditemukan:")
            print(f"NIK     : {hasil.nik}")
            print(f"Nama    : {hasil.nama}")
            print(f"Jabatan : {hasil.jabatan}")
        else:
            print("❌ Karyawan tidak ditemukan.")

    elif pilihan == "3":
        sistem.tampilkan_semua()

    elif pilihan == "4":
        print("Terima kasih!")
        break

    else:
        print("Pilihan tidak valid!")
