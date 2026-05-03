# Tugas Praktikum - Dataset
# Kelas Induk: Produk
class Produk:
    def __init__(self, nama, harga, kategori, stok):
        self.nama = nama
        self.harga = harga
        self.kategori = kategori
        self.stok = stok

    def tampilkan_info(self):
        print(f"Nama Produk : {self.nama}")
        print(f"Harga       : Rp{self.harga:,}")
        print(f"Kategori    : {self.kategori}")
        print(f"Stok        : {self.stok}")

# Kelas Turunan: Elektronik (Menambah atribut garansi)
class Elektronik(Produk):
    def __init__(self, nama, harga, kategori, stok, garansi):
        super().__init__(nama, harga, kategori, stok)
        self.garansi = garansi

    def tampilkan_elektronik(self):
        self.tampilkan_info()
        print(f"Garansi     : {self.garansi} Bulan")

# Kelas Turunan: Pakaian (Menambah atribut ukuran)
class Pakaian(Produk):
    def __init__(self, nama, harga, kategori, stok, ukuran):
        super().__init__(nama, harga, kategori, stok)
        self.ukuran = ukuran

    def tampilkan_pakaian(self):
        self.tampilkan_info()
        print(f"Ukuran      : {self.ukuran}")

# --- Menampilkan Hasil Dataset ---

print("=== DATA PRODUK MARKETPLACE ===\n")

# 1. Laptop Gaming (Elektronik)
laptop = Elektronik("Laptop Gaming", 15000000, "Elektronik", 5, 24)
laptop.tampilkan_elektronik()
print("-" * 30)

# 2. Kemeja Flanel (Pakaian)
kemeja = Pakaian("Kemeja Flanel", 200000, "Pakaian", 10, "M")
kemeja.tampilkan_pakaian()
print("-" * 30)

# 3. Smart TV (Elektronik)
tv = Elektronik("Smart TV", 7000000, "Elektronik", 3, 12)
tv.tampilkan_elektronik()
print("-" * 30)

# 4. Celana Jeans (Pakaian)
celana = Pakaian("Celana Jeans", 300000, "Pakaian", 7, "L")
celana.tampilkan_pakaian()

# Tugas Praktikum - Instruksi
# 1. Parent Class
class Produk:
    def __init__(self, nama, harga):
        self.nama = nama
        self.harga = harga

    # Method Parent menampilkan info barang
    def tampilkan_info(self):
        print(f"Produk: {self.nama}")
        print(f"Harga: {self.harga}")

    # 3. Fungsi hitung total harga
    def hitung_total(self, jumlah):
        total = self.harga * jumlah
        return total

# 2. Child Class
class Elektronik(Produk):
    def __init__(self, nama, harga, garansi):
        super().__init__(nama, harga)
        self.garansi = garansi

    def tampilkan_elektronik(self, jumlah_beli):
        self.tampilkan_info()
        print(f"Garansi: {self.garansi} bulan")
        # 4. Alur Sistem: Menghitung total harga berdasarkan jumlah beli
        total = self.hitung_total(jumlah_beli)
        print(f"Total beli {jumlah_beli}: {total}")

class Pakaian(Produk):
    def __init__(self, nama, harga, ukuran):
        super().__init__(nama, harga)
        self.ukuran = ukuran

    def tampilkan_pakaian(self, jumlah_beli):
        self.tampilkan_info()
        print(f"Ukuran: {self.ukuran}")
        # 4. Alur Sistem: Menghitung total harga berdasarkan jumlah beli
        total = self.hitung_total(jumlah_beli)
        print(f"Total beli {jumlah_beli}: {total}")

# --- Eksekusi Sistem (Contoh Hasil Sistem) ---

# User membeli produk Elektronik
laptop = Elektronik("Laptop Gaming", 15000000, 24)
laptop.tampilkan_elektronik(2)

print("-" * 30)

# User membeli produk Pakaian
kaos = Pakaian("Kemeja Flanel", 200000, "M")
kaos.tampilkan_pakaian(3)

