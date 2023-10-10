from prettytable import PrettyTable

# Data barang yang dijual
data_barang = {
    '1': {'nama': 'Repaint Scoopy Putih', 'harga': 900000},
    '2': {'nama': 'Repaint Scoopy Merah', 'harga': 1200000},
    '3': {'nama': 'Repaint Scoopy Putih Lembayung', 'harga': 1600000},
    '4': {'nama': 'Repaint Scoopy Hitam', 'harga': 1000000},
    '5': {'nama': 'Repaint Scoopy Bunglon', 'harga': 2000000}
}

# Fungsi pembantu untuk membaca data transaksi dari file
def baca_transaksi():
    try:
        file = open('transaksi.csv', 'r')
        transaksi = []
        for line in file.readlines():
            kode, nama, harga, jumlah = line.strip().split(';')
            transaksi.append({
                'kode': kode,
                'nama': nama,
                'harga': int(harga),
                'jumlah': int(jumlah)
            })
        file.close()
        return transaksi
    except FileNotFoundError:
        return []

# Fungsi pembantu untuk menyimpan data transaksi ke file
def simpan_transaksi(transaksi):
    file = open('transaksi.csv', 'w')
    for barang in transaksi:
        file.write(f"{barang['kode']};{barang['nama']};{barang['harga']};{barang['jumlah']}\n")
    file.close()

# Fungsi untuk menampilkan data barang yang dijual
def tampilkan_barang():
    table = PrettyTable(['Kode', 'Nama Barang', 'Harga'])
    for kode in data_barang:
        nama_barang = data_barang[kode]['nama']
        harga_barang = data_barang[kode]['harga']
        table.add_row([kode, nama_barang, f"Rp {harga_barang:,d}"])
    print(table)

# Fungsi untuk menambah data barang
def tambah_barang():
    nama_barang = input("Masukkan nama barang: ")
    harga_barang = int(input("Masukkan harga barang: "))
    kode_barang = str(len(data_barang) + 1)
    data_barang[kode_barang] = {'nama': nama_barang, 'harga': harga_barang}
    print("Barang berhasil ditambahkan.")

# Fungsi untuk memperbarui data barang
def perbarui_barang():
    kode_barang = input("Masukkan kode barang yang ingin diperbarui: ")
    if kode_barang in data_barang:
        nama_barang = input("Masukkan nama barang baru: ")
        harga_barang = int(input("Masukkan harga barang baru: "))
        data_barang[kode_barang] = {'nama': nama_barang, 'harga': harga_barang}
        print("Barang berhasil diperbarui.")
    else:
        print("Barang tidak ditemukan.")

# Fungsi untuk menghapus data barang
def hapus_barang():
    kode_barang = input("Masukkan kode barang yang ingin dihapus: ")
    if kode_barang in data_barang:
        del data_barang[kode_barang]
        print("Barang berhasil dihapus.")
    else:
        print("Barang tidak ditemukan.")

# Fungsi untuk menampilkan data transaksi
def tampilkan_transaksi():
    transaksi = baca_transaksi()
    table = PrettyTable(['Kode', 'Nama Barang', 'Harga', 'Jumlah', 'Total Harga'])
    for barang in transaksi:
        kode_barang = barang['kode']
        nama_barang = barang['nama']
        harga_barang = barang['harga']
        jumlah_barang = barang['jumlah']
        total_harga = harga_barang * jumlah_barang
        table.add_row([kode_barang, nama_barang, f"Rp {harga_barang:,d}", jumlah_barang, f"Rp {total_harga:,d}"])
    print(table)

# Fungsi untuk melakukan transaksi
def transaksi():
    tampilkan_barang()
    kode_barang = input("Masukkan kode barang yang ingin dibeli: ")
    if kode_barang in data_barang:
        jumlah_barang = int(input("Masukkan jumlah barang yang ingin dibeli: "))
        barang = {
            'kode': kode_barang,
            'nama': data_barang[kode_barang]['nama'],
            'harga': data_barang[kode_barang]['harga'],
            'jumlah': jumlah_barang
        }
        transaksi_sebelumnya = baca_transaksi()
        transaksi_sebelumnya.append(barang)
        simpan_transaksi(transaksi_sebelumnya)
        print(f"Transaksi sukses! Total harga yang harus dibayar adalah Rp {barang['harga'] * barang['jumlah']:,d}.")
    else:
        print("Barang tidak ditemukan.")

# Fungsi untuk menampilkan menu pembeli
def menu_pembeli():
    while True:
        print("\n=== MENU PEMBELI ===")
        print("1. Lihat Daftar Barang")
        print("2. Beli Barang")
        print("3. Lihat Transaksi")
        print("4. Keluar")

        pilihan = input("\nPilih menu: ")

        if pilihan == '1':
            tampilkan_barang()
        elif pilihan == '2':
            transaksi()
        elif pilihan == '3':
            tampilkan_transaksi()
        elif pilihan == '4':
            print("\nTerima kasih telah menggunakan program ini.")
            break
        else:
            print("\nPilihan tidak valid.")

# Fungsi untuk menampilkan menu penjual
def menu_penjual():
    while True:
        print("\n=== MENU PENJUAL ===")
        print("1. Lihat Barang")
        print("2. Tambah Barang")
        print("3. Perbarui Barang")
        print("4. Hapus Barang")
        print("5. Keluar")

        pilihan = input("\nPilih menu: ")

        if pilihan == '1':
            tampilkan_barang()
        elif pilihan == '2':
            tambah_barang()
        elif pilihan == '3':
            perbarui_barang()
        elif pilihan == '4':
            hapus_barang()
        elif pilihan == '5':
            print("\nTerima kasih telah menggunakan program ini.")
            break
        else:
            print("\nPilihan tidak valid.")

# Fungsi untuk menampilkan menu dan melihat role yang login
def tampilkan_menu(role):
    if role == 'pembeli':
        menu_pembeli()
    elif role == 'penjual':
        menu_penjual()
    else:
        print("Role tidak ditemukan.")

# Fungsi utama untuk menginisialisasi login dan menampilkan menu
def main():
    while True:
        role = input("Masukkan role (penjual/pembeli): ")
        if role in ['penjual', 'pembeli']:
            break
        else:
            print("Role tidak valid.")
    tampilkan_menu(role)

if _name_ == '_main_':
    main()
