# Payment Link Generator - Frontend

Frontend untuk Payment Link Generator menggunakan plain HTML, CSS, dan vanilla JavaScript.

## Files

```
frontend/
├── pay.html       # Halaman pembayaran
├── success.html   # Halaman sukses setelah bayar
└── README.md
```

## Setup

Frontend ini adalah **static HTML files** yang bisa langsung dibuka di browser atau di-serve menggunakan web server sederhana.

### Metode 1: Langsung Buka di Browser

Cukup buka file `pay.html` atau `success.html` di browser. Namun perlu modifikasi `API_URL` jika backend tidak di localhost:5000.

### Metode 2: Menggunakan Live Server (Recommended)

#### Menggunakan Python

```bash
# Python 3
python -m http.server 3000

# Python 2
python -m SimpleHTTPServer 3000
```

Akses di: `http://localhost:3000`

#### Menggunakan Node.js http-server

Install http-server:
```bash
npm install -g http-server
```

Jalankan:
```bash
http-server -p 3000
```

#### Menggunakan VS Code Live Server Extension

1. Install extension "Live Server" di VS Code
2. Right-click pada `pay.html` atau `success.html`
3. Pilih "Open with Live Server"

## Pages

### 1. pay.html - Halaman Pembayaran

**URL Format:**
```
http://localhost:3000/pay.html?code=AbC91x
```

**Fitur:**
- Menampilkan harga pembayaran
- Tombol "Bayar Sekarang"
- Redirect ke Midtrans payment gateway
- Loading state saat proses pembayaran

**Flow:**
1. User membuka link payment
2. Frontend fetch payment info dari backend
3. User klik "Bayar Sekarang"
4. Frontend hit API untuk create charge
5. User di-redirect ke Midtrans
6. Setelah bayar, Midtrans redirect ke success.html

### 2. success.html - Halaman Sukses

**URL Format:**
```
http://localhost:3000/success.html?order_id=ORDER-xxx
```

**Fitur:**
- Menampilkan status pembayaran berhasil
- Menampilkan detail order (Order ID, harga, waktu)
- Menampilkan link tujuan
- Tombol untuk buka link
- Tombol untuk copy link

**Flow:**
1. Midtrans redirect ke page ini setelah payment sukses
2. Frontend fetch order info dari backend
3. Tampilkan link tujuan yang bisa dibuka user

## Konfigurasi

### Update Backend URL

Jika backend berjalan di URL yang berbeda, update `API_URL` di kedua file:

**pay.html:**
```javascript
const API_URL = 'http://localhost:5000/api';
```

**success.html:**
```javascript
const API_URL = 'http://localhost:5000/api';
```

Ganti dengan URL backend Anda, misalnya:
```javascript
const API_URL = 'https://api.yourdomain.com/api';
```

### CORS Issue

Jika terjadi CORS error, pastikan backend sudah enable CORS untuk frontend URL Anda:

```javascript
// Di backend app.js
app.use(cors({
  origin: 'http://localhost:3000'
}));
```

## Styling

Frontend menggunakan **inline CSS** dengan desain modern:
- Gradient background (purple theme)
- Glassmorphism card design
- Smooth animations
- Responsive design
- Loading states
- Error handling UI

## Responsive Design

Halaman sudah responsive dan bisa diakses dari:
- Desktop
- Tablet
- Mobile phone

## Testing

### Test Payment Flow

1. Jalankan backend di `http://localhost:5000`
2. Jalankan frontend di `http://localhost:3000`
3. Buat payment link via API:
```bash
curl -X POST http://localhost:5000/api/create-link \
  -H "Content-Type: application/json" \
  -d '{
    "price": 25000,
    "target_url": "https://google.com"
  }'
```
4. Copy `payment_link` dari response
5. Buka link di browser
6. Klik "Bayar Sekarang"
7. Di Midtrans Sandbox, gunakan test card:
   - Card Number: `4811 1111 1111 1114`
   - CVV: `123`
   - Exp Date: `01/25`
8. Submit payment
9. Akan redirect ke success page
10. Link tujuan akan muncul

## Troubleshooting

### Payment Link Tidak Muncul

- Cek console browser (F12)
- Pastikan backend running
- Cek API_URL benar
- Cek CORS setting di backend

### Success Page Tidak Muncul

- Pastikan Midtrans redirect URL sudah benar
- Cek di Midtrans Dashboard > Settings > Configuration
- Finish Redirect URL harus: `http://localhost:3000/success.html?order_id={order_id}`

### Link Tujuan Tidak Muncul

- Cek order_id di URL
- Cek status pembayaran di database
- Pastikan webhook sudah jalan (untuk update status paid)

## Customization

### Ubah Warna Theme

Edit gradient di kedua file:

```css
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
```

Ganti dengan warna favorit Anda.

### Ubah Text/Copy

Semua text bisa diubah langsung di HTML:

```html
<h1>Halaman Pembayaran</h1>
```

### Tambah Logo/Image

Tambahkan tag `<img>` untuk logo:

```html
<img src="logo.png" alt="Logo" style="width: 100px;">
```

## Production Deployment

### Deploy ke Vercel/Netlify

1. Push ke GitHub
2. Connect repository ke Vercel/Netlify
3. Deploy otomatis
4. Update `API_URL` ke production backend URL

### Deploy ke Static Hosting

Upload file ke:
- AWS S3 + CloudFront
- Google Cloud Storage
- Azure Blob Storage
- Firebase Hosting

### Update Environment

Jangan lupa update:
- Backend URL di JavaScript
- Midtrans redirect URL di dashboard

## License

MIT
