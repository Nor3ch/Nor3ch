- 👋 Hi, I’m @Nor3ch
- 👀 I’m interested in game development and security engineer
- 🌱 I’m currently learning c#
- 💞️ I’m looking to collaborate on game development
- 📫 How to reach me 08382651767
- 😄 Pronouns:
- ⚡ Fun fact:
- 
![alt text](?![Editing Nor3ch_README md at main · Nor3ch_Nor3ch - Google Chrome 26_09_2024 14 11 28](https://github.com/user-attachments/assets/77218c89-b35c-49d9-bbd2-03e83e043941)
raw=true)

APA ITU CSRF
cross-site requet forgery (CSRF) adalah sebuah serangan cyber pada suatu website tertentu yang tentunya sangat berbahaya bagi pengguna website tersebut.
Serangan ini sering tidak dikenali oleh pengguna dan akhirnya tidak sadar saat sudah terkena serangan cyber tersebut.

Untuk melindungi aplikasi web dari serangan CSRF, beberapa komponen dan teknik dapat diterapkan:
Token CSRF:
Menggunakan token unik yang dihasilkan untuk setiap sesi pengguna. Token ini harus disertakan dalam setiap permintaan yang mengubah data (POST, PUT, DELETE).
Server memverifikasi token tersebut sebelum memproses permintaan.
SameSite Cookies:
Menetapkan atribut SameSite pada cookies dapat membantu mencegah pengiriman cookie dalam permintaan lintas situs. 
Tipe ini membatasi pengiriman cookie hanya untuk permintaan yang berasal dari situs yang sama.
Header Khusus:
Menggunakan header khusus seperti X-Requested-With yang biasanya hanya digunakan oleh permintaan AJAX.
Server dapat memeriksa keberadaan header ini untuk memvalidasi permintaan.
Validasi Referer:
Memeriksa header Referer untuk memastikan bahwa permintaan berasal dari halaman yang sah. 
Namun, pendekatan ini tidak sepenuhnya dapat diandalkan karena beberapa browser atau kebijakan privasi mungkin menghapus header ini.
Penggunaan Framework yang Aman:
Banyak framework modern sudah menyertakan mekanisme perlindungan CSRF secara default. Menggunakan framework yang sudah menerapkan praktik terbaik dapat meminimalkan risiko.
Pengaturan Pengguna dan Sesi:
Membatasi masa aktif sesi dan memberikan opsi untuk keluar dari sesi secara otomatis setelah periode tidak aktif.
Audit dan Monitoring:
Melakukan audit keamanan secara berkala dan memantau aktivitas pengguna untuk mendeteksi pola yang mencurigakan.

gorila/csrf
![alt text](![gorilla_csrf_ Package gorilla_csrf provides Cross Site Request Forgery (CSRF) prevention middleware for Go web applications   services 🔒 - Google Chrome 26_09_2024 18 06 17](https://github.com/user-attachments/assets/1dfa8809-fd0b-43fe-8384-41c4e9564ac9)
?raw=true)
gorila/csrf merupakan HTTP library middleware yang menyediakan layanan pencegahan/perlindungan terhadap CSRF
project ini berisikan:
Middleware/handler csrf.Protect memberikan perlindungan CSRF pada rute yang terhubung ke router atau sub-router.
Fungsi csrf.Token yang menyediakan token untuk diteruskan ke respons Anda, baik itu formulir HTML atau isi respons JSON.
dan juga csrf.TemplateField yang dapat Anda masukkan ke dalam template html/template Anda untuk menggantikan tag template {{ .csrfField }} dengan kolom masukan tersembunyi.

Berikut adalah langkah-langkah dasar untuk mengimplementasikan Gorilla CSRF dalam aplikasi Go:

Tautan GitHub yang kamu berikan mengarah ke repositori gorilla/csrf, yang merupakan sebuah paket untuk menangani serangan Cross-Site Request Forgery (CSRF) dalam aplikasi web yang dibangun menggunakan bahasa pemrograman Go. Berikut adalah beberapa fungsi utama dari paket ini:

Proteksi CSRF: Paket ini membantu mengamankan aplikasi dari serangan CSRF dengan memvalidasi token yang dikirim bersama permintaan HTTP. Token ini harus disertakan dalam permintaan untuk dianggap valid.

Integrasi yang Mudah: Paket ini dirancang agar mudah diintegrasikan ke dalam aplikasi Go, dengan menyediakan middleware yang dapat digunakan langsung di router Go.

Pengelolaan Token: Paket ini memungkinkan pengelolaan token yang mudah, termasuk pembuatan dan pengiriman token kepada klien, serta validasi token yang diterima dari klien.

Dukungan untuk Berbagai Metode HTTP: Paket ini dapat digunakan untuk melindungi berbagai jenis permintaan HTTP, baik itu POST, PUT, DELETE, dan lain-lain.

Konfigurabilitas: Pengguna dapat mengonfigurasi opsi seperti nama cookie, waktu kedaluwarsa token, dan lain-lain sesuai dengan kebutuhan aplikasi mereka.

Dengan menggunakan paket ini, pengembang dapat lebih mudah melindungi aplikasi mereka dari potensi serangan CSRF, yang dapat mengeksploitasi kredensial pengguna yang sudah terautentikasi.

1. Instal Paket
Pertama, pastikan Anda menginstal paket gorilla/csrf. Anda bisa menggunakan go get untuk menginstalnya:
go get github.com/gorilla/csrf

2. Import Paket
Di file Go Anda, import paket yang diperlukan:
import (
    "net/http"
    "github.com/gorilla/csrf"
    "github.com/gorilla/mux"
)

3.Konfigurasi CSRF
Setelah menginstal, Anda dapat menyiapkan middleware CSRF dalam aplikasi Anda. Berikut adalah contoh penerapan kodenya:
func main() {
    // Secret key untuk CSRF
    csrfSecret := []byte("your-secret-key")

    // Middleware CSRF
    csrfMiddleware := csrf.Protect(csrfSecret)

    // Setup router dan handler
    http.HandleFunc("/", homeHandler)

    // Gunakan middleware CSRF
    http.ListenAndServe(":8080", csrfMiddleware(http.DefaultServeMux))
}

func homeHandler(w http.ResponseWriter, r *http.Request) {
    // Menghasilkan token CSRF
    token := csrf.Token(r)

    // Mengirim token ke client (misalnya dalam form)
    w.Header().Set("Content-Type", "text/html")
    w.Write([]byte(`<form action="/submit" method="POST">
                        <input type="hidden" name="csrf_token" value="` + token + `">
                        <input type="submit" value="Submit">
                    </form>`))
}

4.Validasi CSRF Token
Saat Anda menerima permintaan POST, Anda harus memvalidasi token CSRF:
func submitHandler(w http.ResponseWriter, r *http.Request) {
    // Validasi CSRF token
    if err := csrf.ForceSameSite(r); err != nil {
        http.Error(w, "Invalid CSRF Token", http.StatusForbidden)
        return
    }

    // Lakukan proses jika token valid
    w.Write([]byte("Form submitted successfully!"))
}

5. Tambahkan Handler untuk Submit
Jangan lupa untuk menambahkan handler untuk memproses permintaan POST:
http.HandleFunc("/submit", submitHandler)

6. Jalankan Aplikasi
Sekarang Anda dapat menjalankan aplikasi Go Anda:
go run main.go
