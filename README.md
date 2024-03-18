# miniprojek4
Muhammad Fahrei nim 2309116088


from prettytable import PrettyTable
class Node:
    def __init__(self, pajak):
        self.pajak = pajak
        self.next = None

class PajakLinkedList:
    def __init__(self):
        self.head = None

    def tambah_di_awal(self, pajak):
        new_node = Node(pajak)
        new_node.next = self.head
        self.head = new_node

    def tambah_di_akhir(self, pajak):
        new_node = Node(pajak)
        if not self.head:
            self.head = new_node
            return
        last = self.head
        while last.next:
            last = last.next
        last.next = new_node

    def tambah_di_antara(self, pajak, posisi):
        if posisi == 0:
            self.tambah_di_awal(pajak)
            return
        new_node = Node(pajak)
        temp = self.head
        for i in range(posisi - 1):
            if temp.next:
                temp = temp.next
            else:
                print("Posisi yang dimasukkan melebihi panjang list.")
                return
        new_node.next = temp.next
        temp.next = new_node

    def hapus_di_awal(self):
        if not self.head:
            print("List kosong. Tidak ada yang bisa dihapus.")
            return
        self.head = self.head.next

    def hapus_di_akhir(self):
        if not self.head:
            print("List kosong. Tidak ada yang bisa dihapus.")
            return
        if not self.head.next:
            self.head = None
            return
        second_last = self.head
        while second_last.next.next:
            second_last = second_last.next
        second_last.next = None

    def hapus_di_antara(self, posisi):
        if not self.head:
            print("List kosong. Tidak ada yang bisa dihapus.")
            return
        temp = self.head
        if posisi == 0:
            self.head = temp.next
            return
        for i in range(posisi - 1):
            if temp.next:
                temp = temp.next
            else:
                print("Posisi yang dimasukkan melebihi panjang list.")
                return
        if not temp or not temp.next:
            print("Posisi yang dimasukkan melebihi panjang list.")
            return
        temp.next = temp.next.next

    def tampilkan_list_pajak(self):
        temp = self.head
        if not temp:
            print("Tidak ada data pajak yang tersedia.")
        else:
            print("List Pajak:")
            while temp:
                pajak = temp.pajak
                print(f"No: {pajak.no}, Nama: {pajak.nama_pajak}, Terkena Pajak: {pajak.yang_terkena_pajak}, Penghasilan Pertahun: {pajak.penghasilan_pertahun}, Potongan Persen: {pajak.potongan_persen}")
                temp = temp.next


class Pajak:
    def __init__(self, no, nama_pajak, yang_terkena_pajak, penghasilan_pertahun, potongan_persen):
        self.no = no
        self.nama_pajak = nama_pajak
        self.yang_terkena_pajak = yang_terkena_pajak
        self.penghasilan_pertahun = penghasilan_pertahun
        self.potongan_persen = potongan_persen

from prettytable import PrettyTable

class PajakManager:
    def __init__(self):
        self.pajak_list = PajakLinkedList()

    def tambah_pajak(self, pajak, posisi="akhir"):
        if posisi == "awal":
            self.pajak_list.tambah_di_awal(pajak)
        elif posisi == "akhir":
            self.pajak_list.tambah_di_akhir(pajak)
        else:
            posisi = int(posisi)
            self.pajak_list.tambah_di_antara(pajak, posisi)
        print(f"Pajak {pajak.nama_pajak} telah ditambahkan.")

    def tampilkan_list_pajak(self):
        temp = self.pajak_list.head
        if not temp:
            print("Tidak ada data pajak yang tersedia.")
        else:
            print("List Pajak:")
            table = PrettyTable(["No", "Nama Pajak", "Yang Terkena Pajak", "Penghasilan Pertahun", "Potongan Persen"])
            while temp:
                pajak = temp.pajak
                table.add_row([pajak.no, pajak.nama_pajak, pajak.yang_terkena_pajak, pajak.penghasilan_pertahun, pajak.potongan_persen])
                temp = temp.next
            print(table)

    def perbarui_pajak(self, no_pajak, field, value):
        temp = self.pajak_list.head
        while temp:
            if temp.pajak.no == no_pajak:
                setattr(temp.pajak, field, value)
                print("Data pajak telah diperbarui.")
                return
            temp = temp.next
        print("Nomor pajak tidak ditemukan.")

    def hapus_pajak(self, posisi="akhir"):
        if posisi == "awal":
            self.pajak_list.hapus_di_awal()
        elif posisi == "akhir":
            self.pajak_list.hapus_di_akhir()
        else:
            posisi = int(posisi)
            self.pajak_list.hapus_di_antara(posisi)
        print("Data pajak telah dihapus.")

    def sort_pajak(self, key, reverse=False):
        temp = self.pajak_list.head
        pajak_data = []
        while temp:
            pajak_data.append([temp.pajak.no, temp.pajak.nama_pajak, temp.pajak.yang_terkena_pajak, temp.pajak.penghasilan_pertahun, temp.pajak.potongan_persen])
            temp = temp.next

        # Sorting pajak_data berdasarkan key
        pajak_data.sort(key=lambda x: x[0] if key == "no" else x[1], reverse=reverse)

        # Menampilkan data yang sudah diurutkan
        print("List Pajak (Setelah diurutkan):")
        table = PrettyTable(["No", "Nama Pajak", "Yang Terkena Pajak", "Penghasilan Pertahun", "Potongan Persen"])
        for pajak in pajak_data:
            table.add_row(pajak)
        print(table)

    def jumpSearch_by_id(self, pajak_id):
        temp = self.pajak_list.head
        while temp:
            if temp.pajak.no == pajak_id:
                return temp.pajak
            temp = temp.next
        return None

    def jumpSearch_by_name(self, pajak_name):
        temp = self.pajak_list.head
        while temp:
            if temp.pajak.nama_pajak == pajak_name:
                return temp.pajak
            temp = temp.next
        return None

def main():
    manager = PajakManager()
    pembayaran_pajak = [
        {"no": 1, "nama_pajak": "Pajak_PPh", "yang_terkena_pajak": "upah, gaji, tunjangan", "penghasilan_pertahun": 5000, "potongan_persen": "5%"},
        {"no": 2, "nama_pajak": "Pajak_PPn", "yang_terkena_pajak": "pengusaha, perusahaan", "penghasilan_pertahun": 2000, "potongan_persen": "5%"}
    ]
    
    for pajak_data in pembayaran_pajak:
        pajak_baru = Pajak(**pajak_data)
        manager.tambah_pajak(pajak_baru)
    
    while True:
        print("\nMenu Sistem Pendataan Perpajakan:")
        print("1. Tampilkan List Pajak")
        print("2. Tambah Pajak")
        print("3. Perbarui Pajak")
        print("4. Hapus Pajak")
        print("5. Sorting")
        print("6. Pencarian Berdasarkan ID atau Nama Pajak")
        print("7. Keluar")
        try:
            pilihan = int(input("Pilih menu (1-7): "))
        except ValueError:
            print("Masukkan angka yang valid.")
            continue

        if pilihan == 1:
            manager.tampilkan_list_pajak()

        elif pilihan == 2:
            try:
                no = int(input("Masukkan nomor pajak: "))
                nama_pajak = input("Masukkan nama pajak: ")
                yang_terkena_pajak = input("Masukkan yang terkena pajak: ")
                penghasilan_pertahun = int(input("Masukkan penghasilan pertahun: "))
                potongan_persen = input("Masukkan potongan persen: ")
                posisi = input("Masukkan posisi tambahan (awal/akhir/antara): ")
                if posisi == "antara":
                    posisi = int(input("Masukkan posisi antara: "))
                manager.tambah_pajak(Pajak(no, nama_pajak, yang_terkena_pajak, penghasilan_pertahun, potongan_persen), posisi)
            except ValueError:
                print("Input tidak valid. Pastikan Anda memasukkan nomor pada input yang membutuhkannya.")

        elif pilihan == 3:
            try:
                no_pajak = int(input("Masukkan nomor pajak yang akan diperbarui: "))
                field = input("Masukkan field yang akan diperbarui: ")
                value = input("Masukkan nilai baru: ")
                manager.perbarui_pajak(no_pajak, field, value)
            except ValueError:
                print("Input tidak valid. Pastikan Anda memasukkan nomor pada input yang membutuhkannya.")
        
        elif pilihan == 4:
            try:
                posisi = input("Masukkan posisi (awal/akhir/antara): ")
                manager.hapus_pajak(posisi)
            except ValueError:
                print("Input tidak valid. Pastikan Anda memasukkan nomor pada input yang membutuhkannya.")
        
        elif pilihan == 5:
            try:
                key = input("Masukkan atribut yang ingin diurutkan (no/nama_pajak): ")
                order = input("Masukkan urutan pengurutan (ascending/descending): ")
                if order.lower() == "ascending":
                    reverse = False
                elif order.lower() == "descending":
                    reverse = True
                else:
                    print("Pilihan urutan tidak valid.")
                    continue
                # Memeriksa apakah key yang dimasukkan valid
                if key not in ["no", "nama_pajak"]:
                    print("Atribut yang dimasukkan tidak valid.")
                    continue
                manager.sort_pajak(key, reverse)
                print(f"Data pajak telah diurutkan berdasarkan {key} secara {order}.")
            except ValueError:
                print("Input tidak valid.")
        
        elif pilihan == 6:
            search_type = input("Pilih tipe pencarian (id/nama): ")
            search_query = input("Masukkan query pencarian: ")
            if search_type == "id":
                result = manager.jumpSearch_by_id(int(search_query))
            elif search_type == "nama":
                result = manager.jumpSearch_by_name(search_query)
            else:
                print("Tipe pencarian tidak valid.")
                continue
            if result:
                print("Data ditemukan:")
                print(f"No: {result.no}, Nama: {result.nama_pajak}, Terkena Pajak: {result.yang_terkena_pajak}, Penghasilan Pertahun: {result.penghasilan_pertahun}, Potongan Persen: {result.potongan_persen}")
            else:
                print("Data tidak ditemukan.")

        elif pilihan == 7:
            print("Terima kasih telah menggunakan program Sistem Pendataan Perpajakan.")
            break

if __name__ == "__main__":
    main()
