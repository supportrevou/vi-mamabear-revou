# Tutorial Integrasi RajaOngkir Shipping API
## 🆓 Development & Testing - 100% GRATIS

## Daftar Isi
- [Pendahuluan](#pendahuluan)
- [Registrasi dan Setup](#registrasi-dan-setup)
- [Fitur RajaOngkir](#fitur-rajaongkir)
- [Implementasi Backend](#implementasi-backend)
- [Implementasi Frontend](#implementasi-frontend)
- [Testing & Simulasi](#testing--simulasi)
- [Best Practices](#best-practices)

---

## Pendahuluan

RajaOngkir adalah API untuk mengecek ongkos kirim dari berbagai ekspedisi di Indonesia seperti JNE, TIKI, POS Indonesia, dan lainnya. API ini sangat berguna untuk e-commerce yang membutuhkan kalkulasi biaya pengiriman otomatis.

**Website**: https://rajaongkir.com

> ⚠️ **PENTING**: Tutorial ini menggunakan **PAKET STARTER** yang **100% GRATIS** dengan 1000 request/bulan. Cukup untuk development dan testing tanpa biaya apapun.

---

## Registrasi dan Setup

### 1. Daftar Akun

1. Kunjungi [RajaOngkir](https://rajaongkir.com)
2. Klik **Daftar** atau **Sign Up**
3. Pilih paket **Starter** (GRATIS)
   - ✅ **0 Rupiah/bulan**
   - ✅ 1000 request/bulan (cukup untuk development)
   - ✅ Akses 3 ekspedisi: JNE, TIKI, POS Indonesia
   - ✅ Cek ongkir kota/kabupaten
   - ❌ Tidak ada tracking resi (hanya di paket berbayar)
   - ❌ Tidak ada cek ongkir kecamatan (hanya di paket Pro)

> 💡 **Tips**: 1000 request/bulan = sekitar 33 request/hari, sangat cukup untuk development dan testing!

### 2. Dapatkan API Key

1. Login ke [Dashboard RajaOngkir](https://rajaongkir.com/akun)
2. Pergi ke menu **Akun**
3. Copy **API Key** Anda
4. Simpan dengan aman (jangan commit ke git!)

> ✅ **GRATIS**: Paket Starter langsung aktif setelah registrasi, tidak perlu verifikasi atau pembayaran.

### 3. Endpoint Base URL

Untuk paket **Starter** (GRATIS):
```
https://api.rajaongkir.com/starter
```


> ⚠️ **Penting**: Jangan gunakan URL Basic atau Pro jika Anda pakai paket Starter. Akan error!

---

## Fitur RajaOngkir

### Fitur Paket Starter (GRATIS)

| Fitur | Starter (GRATIS) |
|-------|------------------|
| Request/bulan | 1000 |
| Cek Ongkir | ✅ |
| Data Provinsi | ✅ |
| Data Kota/Kabupaten | ✅ |
| Data Kecamatan | ❌ |
| Internasional | ❌ |
| Waybill Tracking | ❌ |
| Jumlah Ekspedisi | 3 (JNE, POS, TIKI) |

### Ekspedisi yang Didukung (Paket Starter)

✅ **JNE** - Jalur Nugraha Ekakurir
✅ **POS** - POS Indonesia  
✅ **TIKI** - Titipan Kilat

> 💡 **Catatan**: Tiga ekspedisi ini sudah cukup untuk sebagian besar kebutuhan e-commerce di Indonesia!

---

## Implementasi Backend

### 1. Setup Environment Variables

```env
# RajaOngkir STARTER (GRATIS)
RAJAONGKIR_API_KEY=your_api_key_here
RAJAONGKIR_BASE_URL=https://api.rajaongkir.com/starter
```

> ⚠️ **Penting**: Pastikan menggunakan URL `/starter` untuk paket gratis!


### 2. Implementasi dengan Node.js/Express

#### Install Dependencies

```bash
npm install axios dotenv
```

#### Buat Service untuk RajaOngkir

```javascript
// services/rajaongkir.service.js
const axios = require('axios');

class RajaOngkirService {
    constructor() {
        this.apiKey = process.env.RAJAONGKIR_API_KEY;
        this.baseUrl = process.env.RAJAONGKIR_BASE_URL;
        this.headers = {
            'key': this.apiKey,
            'content-type': 'application/x-www-form-urlencoded'
        };
    }

    // Get semua provinsi
    async getProvinces() {
        try {
            const response = await axios.get(`${this.baseUrl}/province`, {
                headers: this.headers
            });
            return response.data.rajaongkir.results;
        } catch (error) {
            throw new Error('Failed to fetch provinces: ' + error.message);
        }
    }

    // Get kota berdasarkan provinsi
    async getCities(provinceId = null) {
        try {
            const url = provinceId 
                ? `${this.baseUrl}/city?province=${provinceId}`
                : `${this.baseUrl}/city`;
            
            const response = await axios.get(url, {
                headers: this.headers
            });
            return response.data.rajaongkir.results;
        } catch (error) {
            throw new Error('Failed to fetch cities: ' + error.message);
        }
    }


    // Cek ongkos kirim
    async checkShippingCost(origin, destination, weight, courier) {
        try {
            const params = new URLSearchParams();
            params.append('origin', origin);
            params.append('destination', destination);
            params.append('weight', weight);
            params.append('courier', courier);

            const response = await axios.post(
                `${this.baseUrl}/cost`,
                params,
                { headers: this.headers }
            );
            
            return response.data.rajaongkir.results;
        } catch (error) {
            throw new Error('Failed to check shipping cost: ' + error.message);
        }
    }

    // Tracking resi (TIDAK TERSEDIA di paket Starter)
    async trackWaybill(waybill, courier) {
        // ⚠️ Fitur ini hanya untuk paket Basic dan Pro
        throw new Error('Tracking tidak tersedia di paket Starter. Upgrade ke Basic/Pro untuk fitur ini.');
        
        /* Uncomment jika sudah upgrade ke Basic/Pro
        try {
            const params = new URLSearchParams();
            params.append('waybill', waybill);
            params.append('courier', courier);

            const response = await axios.post(
                `${this.baseUrl}/waybill`,
                params,
                { headers: this.headers }
            );
            
            return response.data.rajaongkir.result;
        } catch (error) {
            throw new Error('Failed to track waybill: ' + error.message);
        }
        */
    }
}

module.exports = new RajaOngkirService();
```

> 💡 **Catatan**: Tracking resi tidak tersedia di paket Starter. Untuk development, Anda bisa simulate tracking dengan data dummy.


#### Buat API Routes

```javascript
// routes/shipping.routes.js
const express = require('express');
const router = express.Router();
const rajaOngkir = require('../services/rajaongkir.service');

// GET /api/shipping/provinces
router.get('/provinces', async (req, res) => {
    try {
        const provinces = await rajaOngkir.getProvinces();
        res.json({
            success: true,
            data: provinces
        });
    } catch (error) {
        res.status(500).json({
            success: false,
            message: error.message
        });
    }
});

// GET /api/shipping/cities?province_id=1
router.get('/cities', async (req, res) => {
    try {
        const { province_id } = req.query;
        const cities = await rajaOngkir.getCities(province_id);
        res.json({
            success: true,
            data: cities
        });
    } catch (error) {
        res.status(500).json({
            success: false,
            message: error.message
        });
    }
});

// POST /api/shipping/cost
router.post('/cost', async (req, res) => {
    try {
        const { origin, destination, weight, courier } = req.body;
        
        // Validasi input
        if (!origin || !destination || !weight || !courier) {
            return res.status(400).json({
                success: false,
                message: 'Missing required parameters'
            });
        }

        const costs = await rajaOngkir.checkShippingCost(
            origin,
            destination,
            weight,
            courier
        );
        
        res.json({
            success: true,
            data: costs
        });
    } catch (error) {
        res.status(500).json({
            success: false,
            message: error.message
        });
    }
});


// POST /api/shipping/track
// ⚠️ TIDAK TERSEDIA di paket Starter
router.post('/track', async (req, res) => {
    res.status(403).json({
        success: false,
        message: 'Tracking tidak tersedia di paket Starter. Upgrade ke Basic/Pro untuk fitur ini.'
    });
    
    /* Uncomment jika sudah upgrade ke Basic/Pro
    try {
        const { waybill, courier } = req.body;
        
        if (!waybill || !courier) {
            return res.status(400).json({
                success: false,
                message: 'Waybill and courier are required'
            });
        }

        const tracking = await rajaOngkir.trackWaybill(waybill, courier);
        
        res.json({
            success: true,
            data: tracking
        });
    } catch (error) {
        res.status(500).json({
            success: false,
            message: error.message
        });
    }
    */
});

module.exports = router;
```

#### Setup di App.js

```javascript
// app.js
const express = require('express');
require('dotenv').config();

const app = express();
const shippingRoutes = require('./routes/shipping.routes');

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Routes
app.use('/api/shipping', shippingRoutes);

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
```

---


## Implementasi Frontend

### 1. HTML Form untuk Cek Ongkir

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cek Ongkos Kirim</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        select, input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        #results {
            margin-top: 20px;
        }
        .shipping-option {
            border: 1px solid #ddd;
            padding: 15px;
            margin-bottom: 10px;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <h1>Cek Ongkos Kirim</h1>
    
    <form id="shippingForm">
        <div class="form-group">
            <label for="originProvince">Provinsi Asal:</label>
            <select id="originProvince" required>
                <option value="">Pilih Provinsi</option>
            </select>
        </div>


        <div class="form-group">
            <label for="originCity">Kota Asal:</label>
            <select id="originCity" required>
                <option value="">Pilih Kota</option>
            </select>
        </div>

        <div class="form-group">
            <label for="destProvince">Provinsi Tujuan:</label>
            <select id="destProvince" required>
                <option value="">Pilih Provinsi</option>
            </select>
        </div>

        <div class="form-group">
            <label for="destCity">Kota Tujuan:</label>
            <select id="destCity" required>
                <option value="">Pilih Kota</option>
            </select>
        </div>

        <div class="form-group">
            <label for="weight">Berat (gram):</label>
            <input type="number" id="weight" min="1" required>
        </div>

        <div class="form-group">
            <label for="courier">Kurir:</label>
            <select id="courier" required>
                <option value="">Pilih Kurir</option>
                <option value="jne">JNE</option>
                <option value="pos">POS Indonesia</option>
                <option value="tiki">TIKI</option>
            </select>
            <small>* Paket Starter hanya support 3 ekspedisi</small>
        </div>

        <button type="submit">Cek Ongkir</button>
    </form>

    <div id="results"></div>

    <script src="shipping.js"></script>
</body>
</html>
```


### 2. JavaScript untuk Handle Form

```javascript
// shipping.js
const API_BASE_URL = 'http://localhost:3000/api/shipping';

// Load provinces saat halaman dimuat
document.addEventListener('DOMContentLoaded', async () => {
    await loadProvinces();
    
    // Event listeners untuk province changes
    document.getElementById('originProvince').addEventListener('change', (e) => {
        loadCities(e.target.value, 'originCity');
    });
    
    document.getElementById('destProvince').addEventListener('change', (e) => {
        loadCities(e.target.value, 'destCity');
    });
    
    // Form submit
    document.getElementById('shippingForm').addEventListener('submit', handleSubmit);
});

// Load semua provinsi
async function loadProvinces() {
    try {
        const response = await fetch(`${API_BASE_URL}/provinces`);
        const data = await response.json();
        
        if (data.success) {
            const originSelect = document.getElementById('originProvince');
            const destSelect = document.getElementById('destProvince');
            
            data.data.forEach(province => {
                const option1 = new Option(province.province, province.province_id);
                const option2 = new Option(province.province, province.province_id);
                originSelect.add(option1);
                destSelect.add(option2);
            });
        }
    } catch (error) {
        console.error('Error loading provinces:', error);
        alert('Gagal memuat data provinsi');
    }
}

// Load kota berdasarkan provinsi
async function loadCities(provinceId, targetSelectId) {
    try {
        const response = await fetch(`${API_BASE_URL}/cities?province_id=${provinceId}`);
        const data = await response.json();
        
        if (data.success) {
            const select = document.getElementById(targetSelectId);
            select.innerHTML = '<option value="">Pilih Kota</option>';
            
            data.data.forEach(city => {
                const option = new Option(
                    `${city.type} ${city.city_name}`,
                    city.city_id
                );
                select.add(option);
            });
        }
    } catch (error) {
        console.error('Error loading cities:', error);
        alert('Gagal memuat data kota');
    }
}


// Handle form submit
async function handleSubmit(e) {
    e.preventDefault();
    
    const originCity = document.getElementById('originCity').value;
    const destCity = document.getElementById('destCity').value;
    const weight = document.getElementById('weight').value;
    const courier = document.getElementById('courier').value;
    
    if (!originCity || !destCity || !weight || !courier) {
        alert('Mohon lengkapi semua field');
        return;
    }
    
    try {
        const response = await fetch(`${API_BASE_URL}/cost`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                origin: originCity,
                destination: destCity,
                weight: weight,
                courier: courier
            })
        });
        
        const data = await response.json();
        
        if (data.success) {
            displayResults(data.data);
        } else {
            alert('Gagal mengecek ongkir: ' + data.message);
        }
    } catch (error) {
        console.error('Error checking shipping cost:', error);
        alert('Terjadi kesalahan saat mengecek ongkir');
    }
}

// Display hasil cek ongkir
function displayResults(results) {
    const resultsDiv = document.getElementById('results');
    resultsDiv.innerHTML = '<h2>Hasil Cek Ongkir</h2>';
    
    if (results.length === 0) {
        resultsDiv.innerHTML += '<p>Tidak ada hasil ditemukan</p>';
        return;
    }
    
    results.forEach(result => {
        const courierName = result.name;
        
        result.costs.forEach(cost => {
            const div = document.createElement('div');
            div.className = 'shipping-option';
            
            const costValue = cost.cost[0].value;
            const etd = cost.cost[0].etd;
            
            div.innerHTML = `
                <h3>${courierName} - ${cost.service}</h3>
                <p><strong>Biaya:</strong> Rp ${costValue.toLocaleString('id-ID')}</p>
                <p><strong>Estimasi:</strong> ${etd} hari</p>
                <p><strong>Deskripsi:</strong> ${cost.description}</p>
            `;
            
            resultsDiv.appendChild(div);
        });
    });
}
```

---

## Testing & Simulasi

### Test Data untuk Development

#### 🏙️ Contoh City IDs untuk Testing

**Jakarta (Origin - Toko Anda):**
- Jakarta Pusat: `151`
- Jakarta Selatan: `152`
- Jakarta Timur: `153`
- Jakarta Barat: `154`
- Jakarta Utara: `155`

**Destinasi Populer untuk Testing:**
- Bandung: `23`
- Surabaya: `444`
- Yogyakarta: `501`
- Semarang: `398`
- Medan: `152`
- Bali (Denpasar): `114`

#### 📦 Test Scenarios

**Scenario 1: Pengiriman Lokal (Jakarta - Bandung)**
```javascript
{
    origin: 151,        // Jakarta Pusat
    destination: 23,    // Bandung
    weight: 1000,       // 1 kg
    courier: 'jne'
}
// Expected: Ongkir sekitar Rp 15.000 - 30.000
```

**Scenario 2: Pengiriman Jarak Jauh (Jakarta - Bali)**
```javascript
{
    origin: 151,        // Jakarta Pusat
    destination: 114,   // Denpasar
    weight: 2000,       // 2 kg
    courier: 'tiki'
}
// Expected: Ongkir sekitar Rp 50.000 - 100.000
```

**Scenario 3: Paket Berat (Jakarta - Surabaya)**
```javascript
{
    origin: 151,        // Jakarta Pusat
    destination: 444,   // Surabaya
    weight: 5000,       // 5 kg
    courier: 'pos'
}
// Expected: Ongkir sekitar Rp 40.000 - 80.000
```

### Monitoring Quota (Paket Starter - 1000 requests/bulan)

```javascript
// Tambahkan counter untuk monitor usage
let apiCallCount = 0;
const MAX_CALLS_PER_MONTH = 1000;

async function checkShippingCostWithMonitoring(origin, destination, weight, courier) {
    if (apiCallCount >= MAX_CALLS_PER_MONTH) {
        throw new Error('Monthly quota exceeded. Wait for next month or upgrade to Basic/Pro.');
    }
    
    const result = await rajaOngkir.checkShippingCost(origin, destination, weight, courier);
    apiCallCount++;
    
    console.log(`API Calls used: ${apiCallCount}/${MAX_CALLS_PER_MONTH}`);
    
    return result;
}
```

### Tips Menghemat Quota

1. **Cache Data Statis** (Provinsi & Kota)
   - Load sekali saja saat aplikasi start
   - Simpan di memory atau database
   - Hemat ~100-200 requests

2. **Batch Check Multiple Couriers**
   - Gunakan Promise.all() untuk parallel requests
   - Tetap count sebagai 3 requests (1 per courier)

3. **Implement Rate Limiting**
   - Batasi user hanya bisa cek ongkir 5x per session
   - Prevent spam/abuse

4. **Show Cached Results**
   - Jika user cek ongkir yang sama dalam 1 jam
   - Tampilkan hasil cache
   - Hemat quota

### Simulasi Tracking (Paket Starter)

Karena tracking tidak tersedia di paket Starter, gunakan dummy data untuk development:

```javascript
// Dummy tracking data untuk development
function simulateTracking(waybill, courier) {
    return {
        waybill: waybill,
        courier: courier.toUpperCase(),
        status: 'DELIVERED',
        history: [
            {
                date: '2024-01-15 09:00',
                description: 'Paket diterima di [JAKARTA]'
            },
            {
                date: '2024-01-15 14:30',
                description: 'Paket dalam perjalanan ke [BANDUNG]'
            },
            {
                date: '2024-01-16 10:15',
                description: 'Paket tiba di [BANDUNG]'
            },
            {
                date: '2024-01-16 15:45',
                description: 'Paket sedang diantar'
            },
            {
                date: '2024-01-16 17:20',
                description: 'Paket telah diterima oleh [PENERIMA]'
            }
        ]
    };
}
```

---


## Best Practices

### 1. Caching Data Provinsi dan Kota (PENTING untuk Paket Starter!)

Data provinsi dan kota jarang berubah, sebaiknya di-cache untuk mengurangi API calls dan menghemat quota 1000 requests/bulan.

```javascript
// Implementasi simple cache dengan Redis
const redis = require('redis');
const client = redis.createClient();

async function getProvincesWithCache() {
    const cacheKey = 'rajaongkir:provinces';
    
    // Cek cache dulu
    const cached = await client.get(cacheKey);
    if (cached) {
        console.log('Using cached provinces - saved 1 API call!');
        return JSON.parse(cached);
    }
    
    // Jika tidak ada di cache, fetch dari API
    const provinces = await rajaOngkir.getProvinces();
    
    // Simpan ke cache (expire 30 hari)
    await client.setEx(cacheKey, 2592000, JSON.stringify(provinces));
    
    return provinces;
}

// Atau gunakan simple in-memory cache (tanpa Redis)
let provincesCache = null;
let citiesCache = {};

async function getProvincesWithMemoryCache() {
    if (provincesCache) {
        console.log('Using cached provinces - saved 1 API call!');
        return provincesCache;
    }
    
    provincesCache = await rajaOngkir.getProvinces();
    return provincesCache;
}
```

> 💡 **Hemat Quota**: Dengan caching, Anda hanya perlu 1 API call untuk provinces (bukan setiap kali user load halaman). Hemat ~500 requests/bulan!

### 2. Error Handling yang Baik

```javascript
async function checkShippingCost(origin, destination, weight, courier) {
    try {
        // Validasi input
        if (weight < 1) {
            throw new Error('Berat minimal 1 gram');
        }
        
        if (weight > 30000) {
            throw new Error('Berat maksimal 30 kg untuk cek ongkir otomatis');
        }
        
        const result = await rajaOngkir.checkShippingCost(
            origin, destination, weight, courier
        );
        
        return result;
    } catch (error) {
        // Log error untuk debugging
        console.error('Shipping cost check failed:', error);
        
        // Return user-friendly error
        throw new Error('Gagal mengecek ongkir. Silakan coba lagi.');
    }
}
```


### 3. Rate Limiting (Penting untuk Paket Starter!)

Implementasi rate limiting untuk menghindari over-quota dan menghemat 1000 requests/bulan.

```javascript
const rateLimit = require('express-rate-limit');

// Limit per user
const shippingLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 menit
    max: 10, // maksimal 10 request per 15 menit per user
    message: 'Terlalu banyak request, silakan coba lagi nanti'
});

app.use('/api/shipping/cost', shippingLimiter);
```

> 💡 **Hemat Quota**: Dengan rate limiting, Anda mencegah abuse dan memastikan 1000 requests cukup untuk sebulan.

### 4. Logging dan Monitoring

```javascript
// Middleware untuk logging
app.use('/api/shipping', (req, res, next) => {
    console.log(`[${new Date().toISOString()}] ${req.method} ${req.path}`);
    next();
});

// Track API usage
async function trackApiUsage(endpoint) {
    // Simpan ke database untuk monitoring
    await db.apiUsage.create({
        endpoint: endpoint,
        timestamp: new Date(),
        service: 'rajaongkir'
    });
}
```

### 5. Optimasi Berat Paket

```javascript
// Helper function untuk optimasi berat
function optimizeWeight(weight) {
    // RajaOngkir menghitung per kg, jadi bulatkan ke atas
    // Contoh: 1100 gram = 2 kg
    if (weight <= 1000) {
        return weight;
    }
    
    const kg = Math.ceil(weight / 1000);
    return kg * 1000;
}
```

### 6. Multiple Courier Comparison (Hemat Quota)

```javascript
// Cek ongkir dari multiple courier sekaligus
// Paket Starter: jne, tiki, pos (3 couriers)
async function compareShippingCosts(origin, destination, weight) {
    const couriers = ['jne', 'tiki', 'pos']; // Hanya 3 courier di Starter
    
    const promises = couriers.map(courier => 
        rajaOngkir.checkShippingCost(origin, destination, weight, courier)
            .catch(error => {
                console.error(`Error checking ${courier}:`, error);
                return null;
            })
    );
    
    const results = await Promise.all(promises);
    
    // Filter out null results (failed requests)
    return results.filter(result => result !== null).flat();
}

// Usage: 3 API calls untuk dapat semua opsi pengiriman
const allOptions = await compareShippingCosts(151, 23, 1000);
```

> 💡 **Efisien**: Dengan 3 API calls, user dapat semua opsi pengiriman sekaligus. Lebih baik daripada user cek satu-satu (bisa 9+ API calls).

---


## Contoh Response API

### Response Get Provinces

```json
{
    "success": true,
    "data": [
        {
            "province_id": "1",
            "province": "Bali"
        },
        {
            "province_id": "2",
            "province": "Bangka Belitung"
        }
    ]
}
```

### Response Get Cities

```json
{
    "success": true,
    "data": [
        {
            "city_id": "1",
            "province_id": "1",
            "province": "Bali",
            "type": "Kabupaten",
            "city_name": "Badung",
            "postal_code": "80351"
        }
    ]
}
```

### Response Check Shipping Cost

```json
{
    "success": true,
    "data": [
        {
            "code": "jne",
            "name": "JNE",
            "costs": [
                {
                    "service": "OKE",
                    "description": "Ongkos Kirim Ekonomis",
                    "cost": [
                        {
                            "value": 18000,
                            "etd": "4-5",
                            "note": ""
                        }
                    ]
                },
                {
                    "service": "REG",
                    "description": "Layanan Reguler",
                    "cost": [
                        {
                            "value": 20000,
                            "etd": "2-3",
                            "note": ""
                        }
                    ]
                }
            ]
        }
    ]
}
```

---


## Integrasi dengan E-Commerce Flow

### 1. Saat Checkout

```javascript
// Contoh integrasi di halaman checkout
async function calculateCheckout(cartItems, destination) {
    // Hitung total berat
    const totalWeight = cartItems.reduce((sum, item) => {
        return sum + (item.weight * item.quantity);
    }, 0);
    
    // Get shipping options
    const shippingOptions = await compareShippingCosts(
        STORE_CITY_ID, // ID kota toko
        destination.cityId,
        totalWeight
    );
    
    // Format untuk ditampilkan ke user
    return shippingOptions.map(option => ({
        courier: option.name,
        service: option.costs[0].service,
        cost: option.costs[0].cost[0].value,
        etd: option.costs[0].cost[0].etd,
        description: option.costs[0].description
    }));
}
```

### 2. Simpan Shipping Info di Order

```javascript
// Schema untuk order
const orderSchema = {
    orderId: String,
    items: Array,
    customer: Object,
    shipping: {
        courier: String,
        service: String,
        cost: Number,
        etd: String,
        origin: {
            cityId: String,
            cityName: String
        },
        destination: {
            cityId: String,
            cityName: String,
            address: String,
            postalCode: String
        },
        weight: Number,
        trackingNumber: String // diisi setelah barang dikirim
    },
    totalAmount: Number,
    status: String,
    createdAt: Date
};
```

### 3. Tracking Setelah Pengiriman

```javascript
// Endpoint untuk tracking
router.get('/orders/:orderId/tracking', async (req, res) => {
    try {
        const order = await db.orders.findOne({ orderId: req.params.orderId });
        
        if (!order || !order.shipping.trackingNumber) {
            return res.status(404).json({
                success: false,
                message: 'Tracking number not found'
            });
        }
        
        const tracking = await rajaOngkir.trackWaybill(
            order.shipping.trackingNumber,
            order.shipping.courier.toLowerCase()
        );
        
        res.json({
            success: true,
            data: tracking
        });
    } catch (error) {
        res.status(500).json({
            success: false,
            message: error.message
        });
    }
});
```

---


## Troubleshooting

### Error: "Invalid API Key"
- Pastikan API key sudah benar (copy dari dashboard)
- Cek apakah API key untuk paket Starter
- Pastikan header 'key' sudah di-set dengan benar

### Error: "Origin/Destination not found"
- Pastikan menggunakan city_id, bukan city_name
- Verifikasi city_id valid dengan endpoint /city
- Gunakan city_id dari response API provinces/cities

### Error: "Courier not supported"
- ⚠️ **Paket Starter hanya support**: jne, pos, tiki (lowercase!)
- Jika butuh courier lain (sicepat, jnt, dll), harus upgrade ke Basic/Pro
- Gunakan lowercase untuk courier code

### Error: "Quota Exceeded"
- Anda sudah menggunakan 1000 requests di bulan ini
- Tunggu bulan depan untuk reset quota
- Atau upgrade ke paket Basic (10.000 requests) atau Pro (100.000 requests)
- Implementasi caching untuk menghemat quota

### Hasil Kosong
- Pastikan kombinasi origin-destination valid
- Beberapa ekspedisi tidak melayani rute tertentu
- Coba dengan courier lain (jne/tiki/pos)
- Cek apakah weight valid (minimal 1 gram)

### Tracking Tidak Bisa
- ⚠️ **Tracking TIDAK tersedia di paket Starter**
- Gunakan dummy data untuk development
- Upgrade ke Basic/Pro jika butuh tracking real

---

## 🎓 Checklist Development (Paket Starter - GRATIS)

Sebelum mulai coding:
- [ ] Sudah registrasi akun RajaOngkir
- [ ] Sudah pilih paket Starter (GRATIS)
- [ ] Sudah dapat API Key
- [ ] Sudah install dependencies (axios, dotenv)
- [ ] Sudah setup environment variables
- [ ] Sudah baca dokumentasi Starter

Saat development:
- [ ] Gunakan endpoint `/starter` (bukan /basic atau /pro)
- [ ] Hanya gunakan 3 courier: jne, tiki, pos
- [ ] Implementasi caching untuk provinsi & kota
- [ ] Implementasi rate limiting
- [ ] Monitor quota usage (1000/bulan)
- [ ] Test dengan city IDs yang valid

Optimasi untuk hemat quota:
- [ ] Cache data provinsi & kota (hemat ~500 requests)
- [ ] Implement rate limiting per user
- [ ] Cache hasil cek ongkir untuk kombinasi yang sama
- [ ] Batch check multiple couriers dengan Promise.all()
- [ ] Monitor dan log setiap API call

---

## 💰 Estimasi Penggunaan Quota

### Breakdown 1000 Requests/Bulan:

**Setup Awal (1x saja):**
- Get Provinces: 1 request → **CACHE!**
- Get Cities: ~34 requests (34 provinsi) → **CACHE!**
- Total: ~35 requests (hanya sekali, lalu cache selamanya)

**Per User Session:**
- Cek ongkir 3 courier: 3 requests
- Jika 100 user/bulan, masing-masing cek 3x: 900 requests

**Total: 35 + 900 = 935 requests** ✅ Masih di bawah 1000!

### Tips Maksimalkan 1000 Requests:
1. **Cache agresif** untuk data statis
2. **Rate limit** per user (max 5 cek ongkir per session)
3. **Batch requests** untuk multiple couriers
4. **Reuse results** jika user cek kombinasi yang sama

---

## Tips Optimasi

1. **Cache Data Statis**: Provinsi dan kota jarang berubah, cache selama mungkin (30 hari atau lebih)
2. **Batch Requests**: Jika perlu cek multiple courier, gunakan Promise.all() - hemat waktu
3. **Lazy Loading**: Load kota hanya saat provinsi dipilih - hemat bandwidth
4. **Debouncing**: Untuk autocomplete, gunakan debounce untuk mengurangi API calls
5. **Fallback**: Sediakan opsi manual input jika API gagal atau quota habis
6. **Monitor Quota**: Track berapa request sudah digunakan, warn user jika mendekati limit
7. **Smart Caching**: Cache hasil cek ongkir untuk kombinasi yang sering dicek

---

## Resources

- **Dokumentasi Official**: https://rajaongkir.com/dokumentasi
- **Dashboard**: https://rajaongkir.com/akun
- **Support**: support@rajaongkir.com
- **Paket & Pricing**: https://rajaongkir.com/dokumentasi/paket

---

## Kesimpulan

RajaOngkir Paket Starter adalah solusi **100% GRATIS** yang sempurna untuk development dan testing shipping cost calculation. Dengan 1000 requests/bulan dan 3 ekspedisi utama (JNE, TIKI, POS), Anda bisa belajar dan develop aplikasi e-commerce tanpa biaya apapun.

**Checklist Implementasi (Paket Starter - GRATIS):**
- ✓ Setup API key dari dashboard (gratis)
- ✓ Implementasi service layer untuk RajaOngkir
- ✓ Buat API endpoints untuk frontend
- ✓ **Implementasi caching untuk data statis (PENTING!)**
- ✓ Error handling dan validation
- ✓ **Rate limiting (PENTING untuk hemat quota!)**
- ✓ Logging dan monitoring quota usage
- ✓ Testing dengan berbagai skenario
- ✓ Gunakan dummy data untuk tracking (tidak tersedia di Starter)

**Quota Management:**
- 1000 requests/bulan cukup untuk development
- Dengan caching yang baik, bisa support 100+ user sessions/bulan
- Monitor usage dan optimize jika perlu
- Upgrade ke Basic/Pro hanya jika sudah production dan butuh lebih banyak requests

Selamat mengimplementasikan! 🚀 **100% GRATIS untuk development!**
