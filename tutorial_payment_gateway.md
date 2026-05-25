# Tutorial Integrasi Payment Gateway (Midtrans/Xendit)
## 🆓 Development & Testing - 100% GRATIS

## Daftar Isi
- [Pendahuluan](#pendahuluan)
- [Pilihan Payment Gateway](#pilihan-payment-gateway)
- [Integrasi Midtrans](#integrasi-midtrans)
- [Integrasi Xendit](#integrasi-xendit)
- [Testing & Simulasi](#testing--simulasi)
- [Best Practices](#best-practices)

---

## Pendahuluan

Payment gateway adalah layanan yang memproses pembayaran online untuk e-commerce. Tutorial ini akan membahas integrasi dua payment gateway populer di Indonesia: Midtrans dan Xendit.

> ⚠️ **PENTING**: Tutorial ini fokus pada **SANDBOX/TEST ENVIRONMENT** yang **100% GRATIS** untuk development dan testing. Tidak ada biaya sama sekali untuk mengikuti tutorial ini.

---

## Pilihan Payment Gateway

### Midtrans
- **Kelebihan**: Banyak metode pembayaran (kartu kredit, e-wallet, bank transfer, dll)
- **Sandbox**: ✅ **GRATIS** - Unlimited testing
- **Dokumentasi**: https://docs.midtrans.com
- **Dashboard Sandbox**: https://dashboard.sandbox.midtrans.com

### Xendit
- **Kelebihan**: API yang mudah digunakan, support berbagai metode pembayaran
- **Test Mode**: ✅ **GRATIS** - Unlimited testing
- **Dokumentasi**: https://docs.xendit.co
- **Dashboard Test**: https://dashboard.xendit.co

> 💡 **Catatan**: Kedua payment gateway menyediakan environment testing yang sepenuhnya gratis untuk development. Anda bisa test semua fitur tanpa biaya apapun.

---

## Integrasi Midtrans

### 1. Registrasi dan Setup

1. Daftar akun di [Midtrans Dashboard](https://dashboard.midtrans.com)
2. Pilih **Sandbox Environment** (untuk testing gratis)
3. Dapatkan **Server Key** dan **Client Key** dari Sandbox Dashboard
   - Login ke dashboard
   - Pilih **Sandbox** environment (pojok kanan atas)
   - Pergi ke **Settings** → **Access Keys**
   - Copy **Sandbox Server Key** dan **Sandbox Client Key**

> ✅ **GRATIS**: Sandbox environment tidak memerlukan verifikasi bisnis dan bisa langsung digunakan setelah registrasi.

### 2. Instalasi SDK

#### Untuk Node.js/Express:
```bash
npm install midtrans-client
```

#### Untuk PHP:
```bash
composer require midtrans/midtrans-php
```

### 3. Konfigurasi Backend (Node.js)

```javascript
const midtransClient = require('midtrans-client');

// Inisialisasi Snap API - SANDBOX MODE (GRATIS)
let snap = new midtransClient.Snap({
    isProduction: false, // FALSE = Sandbox (gratis untuk testing)
    serverKey: process.env.MIDTRANS_SERVER_KEY, // Sandbox Server Key
    clientKey: process.env.MIDTRANS_CLIENT_KEY  // Sandbox Client Key
});

// Endpoint untuk membuat transaksi
app.post('/api/payment/create', async (req, res) => {
    try {
        const { orderId, amount, customerDetails, items } = req.body;
        
        let parameter = {
            transaction_details: {
                order_id: orderId,
                gross_amount: amount
            },
            customer_details: {
                first_name: customerDetails.firstName,
                last_name: customerDetails.lastName,
                email: customerDetails.email,
                phone: customerDetails.phone
            },
            item_details: items,
            callbacks: {
                finish: 'https://yourdomain.com/payment/finish',
                error: 'https://yourdomain.com/payment/error',
                pending: 'https://yourdomain.com/payment/pending'
            }
        };

        const transaction = await snap.createTransaction(parameter);
        
        res.json({
            token: transaction.token,
            redirect_url: transaction.redirect_url
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});
```

> 💡 **Tips**: Dengan `isProduction: false`, semua transaksi adalah simulasi dan tidak ada uang sungguhan yang diproses.

### 4. Implementasi Frontend

```html
<!DOCTYPE html>
<html>
<head>
    <!-- SANDBOX SNAP.JS - untuk testing gratis -->
    <script src="https://app.sandbox.midtrans.com/snap/snap.js" 
            data-client-key="YOUR_SANDBOX_CLIENT_KEY"></script>
</head>
<body>
    <button id="pay-button">Bayar Sekarang (Test Mode)</button>

    <script>
        document.getElementById('pay-button').addEventListener('click', async () => {
            try {
                // Request token dari backend
                const response = await fetch('/api/payment/create', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        orderId: 'ORDER-' + Date.now(),
                        amount: 100000,
                        customerDetails: {
                            firstName: 'John',
                            lastName: 'Doe',
                            email: 'john@example.com',
                            phone: '081234567890'
                        },
                        items: [{
                            id: 'ITEM1',
                            price: 100000,
                            quantity: 1,
                            name: 'Product Name'
                        }]
                    })
                });

                const data = await response.json();
                
                // Tampilkan Snap payment page (SANDBOX)
                snap.pay(data.token, {
                    onSuccess: function(result) {
                        console.log('Payment success:', result);
                        alert('Pembayaran berhasil! (Test Mode)');
                        window.location.href = '/payment/success';
                    },
                    onPending: function(result) {
                        console.log('Payment pending:', result);
                        alert('Pembayaran pending (Test Mode)');
                        window.location.href = '/payment/pending';
                    },
                    onError: function(result) {
                        console.log('Payment error:', result);
                        alert('Pembayaran gagal (Test Mode)');
                        window.location.href = '/payment/error';
                    },
                    onClose: function() {
                        console.log('Payment popup closed');
                    }
                });
            } catch (error) {
                console.error('Error:', error);
            }
        });
    </script>
</body>
</html>
```

> ⚠️ **Penting**: Gunakan URL sandbox `https://app.sandbox.midtrans.com/snap/snap.js` untuk testing gratis.

### 5. Webhook/Notification Handler

```javascript
const crypto = require('crypto');

app.post('/api/payment/notification', (req, res) => {
    try {
        const notification = req.body;
        
        // Verifikasi signature
        const serverKey = 'YOUR_SERVER_KEY';
        const orderId = notification.order_id;
        const statusCode = notification.status_code;
        const grossAmount = notification.gross_amount;
        const signatureKey = notification.signature_key;
        
        const hash = crypto.createHash('sha512')
            .update(orderId + statusCode + grossAmount + serverKey)
            .digest('hex');
        
        if (hash !== signatureKey) {
            return res.status(401).json({ message: 'Invalid signature' });
        }
        
        // Update status order di database
        const transactionStatus = notification.transaction_status;
        const fraudStatus = notification.fraud_status;
        
        if (transactionStatus === 'capture') {
            if (fraudStatus === 'accept') {
                // Payment berhasil
                updateOrderStatus(orderId, 'paid');
            }
        } else if (transactionStatus === 'settlement') {
            // Payment berhasil
            updateOrderStatus(orderId, 'paid');
        } else if (transactionStatus === 'cancel' || 
                   transactionStatus === 'deny' || 
                   transactionStatus === 'expire') {
            // Payment gagal
            updateOrderStatus(orderId, 'failed');
        } else if (transactionStatus === 'pending') {
            // Payment pending
            updateOrderStatus(orderId, 'pending');
        }
        
        res.status(200).json({ message: 'OK' });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});
```

---

## Integrasi Xendit

### 1. Registrasi dan Setup

1. Daftar akun di [Xendit Dashboard](https://dashboard.xendit.co)
2. Pilih **Test Mode** (untuk testing gratis)
3. Dapatkan **Test API Key** dari dashboard
   - Login ke dashboard
   - Toggle ke **Test Mode** (pojok kanan atas)
   - Pergi ke **Settings** → **Developers** → **API Keys**
   - Copy **Test Secret Key**

> ✅ **GRATIS**: Test mode tidak memerlukan verifikasi bisnis dan bisa langsung digunakan setelah registrasi.

### 2. Instalasi SDK

#### Untuk Node.js:
```bash
npm install xendit-node
```

### 3. Konfigurasi Backend (Node.js)

```javascript
const Xendit = require('xendit-node');

// Inisialisasi dengan TEST API KEY (GRATIS)
const xenditClient = new Xendit({
    secretKey: process.env.XENDIT_TEST_SECRET_KEY // Test Secret Key
});

const { Invoice } = xenditClient;
const invoiceSpecificOptions = {};
const i = new Invoice(invoiceSpecificOptions);

// Endpoint untuk membuat invoice
app.post('/api/payment/xendit/create', async (req, res) => {
    try {
        const { orderId, amount, customerDetails, items } = req.body;
        
        const invoice = await i.createInvoice({
            externalID: orderId,
            amount: amount,
            payerEmail: customerDetails.email,
            description: 'Payment for Order ' + orderId + ' (TEST MODE)',
            customer: {
                given_names: customerDetails.firstName,
                surname: customerDetails.lastName,
                email: customerDetails.email,
                mobile_number: customerDetails.phone
            },
            items: items.map(item => ({
                name: item.name,
                quantity: item.quantity,
                price: item.price
            })),
            successRedirectURL: 'https://yourdomain.com/payment/success',
            failureRedirectURL: 'https://yourdomain.com/payment/failed'
        });
        
        res.json({
            invoice_id: invoice.id,
            invoice_url: invoice.invoice_url,
            expiry_date: invoice.expiry_date
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});
```

> 💡 **Tips**: Dengan Test API Key, semua invoice yang dibuat adalah simulasi dan tidak ada uang sungguhan yang diproses.

### 4. Implementasi Frontend

```javascript
// Redirect user ke invoice URL
async function processPayment() {
    try {
        const response = await fetch('/api/payment/xendit/create', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                orderId: 'ORDER-' + Date.now(),
                amount: 100000,
                customerDetails: {
                    firstName: 'John',
                    lastName: 'Doe',
                    email: 'john@example.com',
                    phone: '+6281234567890'
                },
                items: [{
                    name: 'Product Name',
                    quantity: 1,
                    price: 100000
                }]
            })
        });

        const data = await response.json();
        
        // Redirect ke halaman pembayaran Xendit
        window.location.href = data.invoice_url;
    } catch (error) {
        console.error('Error:', error);
    }
}
```

### 5. Webhook Handler

```javascript
const crypto = require('crypto');

app.post('/api/payment/xendit/webhook', (req, res) => {
    try {
        // Verifikasi webhook token
        const webhookToken = req.headers['x-callback-token'];
        const expectedToken = 'YOUR_WEBHOOK_VERIFICATION_TOKEN';
        
        if (webhookToken !== expectedToken) {
            return res.status(401).json({ message: 'Invalid token' });
        }
        
        const notification = req.body;
        const externalId = notification.external_id;
        const status = notification.status;
        
        // Update status order di database
        if (status === 'PAID') {
            updateOrderStatus(externalId, 'paid');
        } else if (status === 'EXPIRED') {
            updateOrderStatus(externalId, 'expired');
        }
        
        res.status(200).json({ message: 'OK' });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});
```

### 6. Alternatif: Payment Link

```javascript
// Membuat payment link sederhana
app.post('/api/payment/xendit/link', async (req, res) => {
    try {
        const { orderId, amount, customerEmail } = req.body;
        
        const invoice = await i.createInvoice({
            externalID: orderId,
            amount: amount,
            payerEmail: customerEmail,
            description: 'Payment for Order ' + orderId
        });
        
        res.json({
            payment_url: invoice.invoice_url
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});
```

---

## Testing & Simulasi

### Midtrans Sandbox - Test Cards & Accounts

#### 💳 Test Credit Cards (GRATIS)
```
Card Number: 4811 1111 1111 1114
CVV: 123
Exp: 01/25
OTP/3DS: 112233

Card Number: 5211 1111 1111 1117 (Mastercard)
CVV: 123
Exp: 01/25
```

#### 🏦 Test Virtual Accounts
- **BCA VA**: Otomatis sukses setelah dibuat
- **BNI VA**: Otomatis sukses setelah dibuat
- **Mandiri VA**: Otomatis sukses setelah dibuat

#### 📱 Test E-Wallets
- **GoPay**: Gunakan nomor test: 081234567890
- **ShopeePay**: Otomatis sukses di sandbox

#### Cara Testing:
1. Buat transaksi dari aplikasi Anda
2. Pilih metode pembayaran
3. Gunakan test credentials di atas
4. Transaksi akan otomatis berhasil/gagal sesuai skenario

> 📖 **Dokumentasi Lengkap**: https://docs.midtrans.com/docs/testing-payment-on-sandbox

---

### Xendit Test Mode - Test Accounts

#### 💳 Test Credit Cards (GRATIS)
```
Card Number: 4000 0000 0000 0002 (Success)
CVV: 123
Exp: 12/25

Card Number: 4000 0000 0000 0127 (Declined)
CVV: 123
Exp: 12/25
```

#### 🏦 Test Virtual Accounts
- Semua VA yang dibuat di test mode otomatis bisa di-simulate payment
- Gunakan "Simulate Payment" button di dashboard

#### 📱 Test E-Wallets
- **OVO**: Gunakan nomor test: +6281234567890
- **Dana**: Gunakan nomor test: +6281234567890
- **LinkAja**: Gunakan nomor test: +6281234567890

#### Cara Testing:
1. Buat invoice dari aplikasi Anda
2. Buka invoice URL
3. Pilih metode pembayaran
4. Untuk VA: Gunakan "Simulate Payment" di dashboard
5. Untuk e-wallet: Gunakan nomor test di atas

> 📖 **Dokumentasi Lengkap**: https://developers.xendit.co/api-reference/#test-scenarios

---

### Tips Testing

#### 1. Gunakan Webhook Testing Tools
```bash
# Install ngrok untuk expose localhost
npm install -g ngrok

# Jalankan ngrok
ngrok http 3000

# Gunakan URL ngrok untuk webhook URL di dashboard
# Contoh: https://abc123.ngrok.io/api/payment/notification
```

#### 2. Test Berbagai Skenario
- ✅ Payment Success
- ❌ Payment Failed
- ⏳ Payment Pending
- ⏰ Payment Expired
- 🔄 Payment Refund (jika tersedia)

#### 3. Monitor di Dashboard
- Cek semua transaksi di Sandbox/Test Dashboard
- Lihat webhook logs
- Debug jika ada error

#### 4. Simulasi Manual
Kedua payment gateway menyediakan fitur untuk simulate payment secara manual dari dashboard:
- **Midtrans**: Gunakan Simulator di dashboard
- **Xendit**: Gunakan "Simulate Payment" button

---

## Best Practices

### 1. Keamanan
- **Jangan expose API keys** di frontend
- Simpan API keys di environment variables
- Gunakan HTTPS untuk semua komunikasi
- Validasi signature/token pada webhook
- Implementasi rate limiting pada API endpoints

### 2. Error Handling
```javascript
try {
    // Payment logic
} catch (error) {
    // Log error untuk debugging
    console.error('Payment error:', error);
    
    // Kirim response yang user-friendly
    res.status(500).json({
        error: 'Terjadi kesalahan saat memproses pembayaran',
        message: 'Silakan coba lagi atau hubungi customer service'
    });
}
```

### 3. Database Management
```javascript
// Simpan semua transaksi di database
const transaction = {
    orderId: 'ORDER-123',
    paymentGateway: 'midtrans', // atau 'xendit'
    amount: 100000,
    status: 'pending',
    token: 'payment-token',
    createdAt: new Date(),
    updatedAt: new Date()
};

// Update status berdasarkan webhook
function updateOrderStatus(orderId, status) {
    // Update di database
    db.transactions.update(
        { orderId: orderId },
        { 
            status: status,
            updatedAt: new Date()
        }
    );
    
    // Kirim notifikasi ke customer jika perlu
    if (status === 'paid') {
        sendEmailNotification(orderId);
    }
}
```

### 4. Testing
- Gunakan sandbox/test environment untuk development
- Test berbagai skenario: success, pending, failed, expired
- Test webhook dengan tools seperti ngrok untuk local development
- Verifikasi semua metode pembayaran yang didukung

### 5. Monitoring
- Log semua transaksi
- Monitor webhook failures
- Set up alerts untuk transaksi gagal
- Track conversion rate

### 6. User Experience
- Tampilkan loading indicator saat processing payment
- Berikan feedback yang jelas untuk setiap status
- Implementasi retry mechanism untuk failed payments
- Sediakan customer support contact

---

## Contoh Environment Variables

```env
# Midtrans SANDBOX (GRATIS untuk testing)
MIDTRANS_SERVER_KEY=SB-Mid-server-xxxxxxxxxxxxx
MIDTRANS_CLIENT_KEY=SB-Mid-client-xxxxxxxxxxxxx
MIDTRANS_IS_PRODUCTION=false

# Xendit TEST MODE (GRATIS untuk testing)
XENDIT_TEST_SECRET_KEY=xnd_development_xxxxxxxxxxxxx
XENDIT_WEBHOOK_TOKEN=your_test_webhook_token

# Application
APP_URL=http://localhost:3000
NODE_ENV=development
```

> ⚠️ **Penting**: 
> - Midtrans Sandbox keys dimulai dengan `SB-Mid-`
> - Xendit Test keys dimulai dengan `xnd_development_`
> - Jangan pernah commit API keys ke git!

---

## Resources

### Midtrans
- [Dokumentasi Official](https://docs.midtrans.com)
- [API Reference](https://api-docs.midtrans.com)
- [Snap Integration Guide](https://docs.midtrans.com/en/snap/overview)

### Xendit
- [Dokumentasi Official](https://docs.xendit.co)
- [API Reference](https://developers.xendit.co/api-reference)
- [Invoice Guide](https://developers.xendit.co/api-reference/#create-invoice)

---

## Troubleshooting

### Midtrans
- **Error: Invalid signature** → Periksa Sandbox Server Key (harus dimulai dengan `SB-Mid-`)
- **Payment popup tidak muncul** → Periksa Sandbox Client Key dan pastikan menggunakan sandbox snap.js URL
- **Webhook tidak diterima** → Gunakan ngrok untuk expose localhost, set webhook URL di dashboard

### Xendit
- **Error: Unauthorized** → Periksa Test API Key (harus dimulai dengan `xnd_development_`)
- **Invoice tidak dibuat** → Validasi format data request
- **Webhook tidak diterima** → Gunakan ngrok untuk expose localhost, verifikasi webhook token

### General Tips
- Selalu cek console browser untuk error
- Monitor network tab untuk melihat API requests
- Cek dashboard untuk melihat transaksi yang dibuat
- Gunakan Postman untuk test API endpoints

---

## 🎓 Checklist Development

Sebelum mulai coding, pastikan:
- [ ] Sudah registrasi akun Midtrans/Xendit
- [ ] Sudah dapat Sandbox/Test API Keys
- [ ] Sudah install dependencies (npm packages)
- [ ] Sudah setup environment variables
- [ ] Sudah baca dokumentasi test scenarios

Saat development:
- [ ] Gunakan Sandbox/Test environment (isProduction: false)
- [ ] Test dengan test cards/accounts yang disediakan
- [ ] Setup webhook dengan ngrok
- [ ] Test semua skenario (success, failed, pending)
- [ ] Monitor dashboard untuk debug

Sebelum production (di luar scope tutorial ini):
- [ ] Verifikasi bisnis di payment gateway
- [ ] Ganti ke Production API Keys
- [ ] Setup webhook URL production (HTTPS)
- [ ] Test sekali lagi di production sandbox
- [ ] Monitor transaksi real

---

**Catatan**: Tutorial ini fokus pada development dan testing yang **100% GRATIS**. Untuk production, Anda perlu verifikasi bisnis dan akan ada transaction fees. Namun untuk belajar dan development, semua fitur bisa digunakan gratis tanpa batas!
