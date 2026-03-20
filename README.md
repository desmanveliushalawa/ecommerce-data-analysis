# 📊 E-Commerce Business Analysis

## Project Overview

Project ini bertujuan untuk menganalisis performa bisnis e-commerce menggunakan dataset Brazilian E-Commerce (Olist).

Analisis difokuskan pada:

* Tren penjualan (sales trend)
* Performa produk
* Perilaku customer
* Kinerja pengiriman (delivery performance)

---

## Business Problem

Perusahaan ingin memahami kondisi bisnis saat ini untuk meningkatkan revenue dan efisiensi operasional.

Pertanyaan utama:

* Apakah penjualan meningkat dari waktu ke waktu?
* Produk atau kategori apa yang paling berkontribusi terhadap revenue?
* Bagaimana perilaku customer (repeat vs new)?
* Apakah terdapat masalah dalam proses pengiriman?

---

## Dataset

Dataset yang digunakan adalah Brazilian E-Commerce Dataset (Olist).

Tabel utama yang digunakan:

* orders
* customers
* order_items
* products
* payments

---

## Tools

* PostgreSQL (SQL untuk analisis data)
* Python (data processing & eksplorasi)
* Excel / Tableau (visualisasi & dashboard)
<p>
  <!-- PostgreSQL -->
  <img src="https://www.postgresql.org/media/img/about/press/elephant.png" alt="PostgreSQL" width="42" height="42"/>
  
  <!-- Python -->
  <img src="https://www.python.org/static/community_logos/python-logo.png" alt="Python" width="42" height="42"/>
  
  <!-- Power BI -->
  <img src="https://upload.wikimedia.org/wikipedia/commons/c/cf/New_Power_BI_Logo.svg" alt="Power BI" width="42" height="42"/>
  
  <!-- Excel (PNG dari StickPNG) -->
  <img src="https://img.icons8.com/?size=100&id=117561&format=png&color=000000" alt="Excel" width="42" height="42"/>
</p>
---

## Analysis Plan

### 1. Sales Trend Analysis

Menganalisis jumlah order dan revenue per bulan untuk melihat pertumbuhan bisnis.

### 2. Product Performance

Mengidentifikasi kategori dan produk dengan kontribusi penjualan tertinggi.

### 3. Customer Behavior

Menganalisis jumlah customer, repeat customer, dan distribusi berdasarkan kota.

### 4. Delivery Performance

Mengukur waktu pengiriman dan mengidentifikasi potensi keterlambatan.

---

## Expected Insights

* Pola pertumbuhan penjualan dari waktu ke waktu
* Kategori produk yang paling berkontribusi terhadap revenue
* Persentase repeat customer
* Performa pengiriman dan potensi bottleneck

---

## Expected Outcome

Dari analisis ini diharapkan dapat memberikan:

* Insight berbasis data untuk pengambilan keputusan bisnis
* Rekomendasi untuk meningkatkan revenue
* Identifikasi area yang perlu dioptimasi

---

## 📊 Dashboard

Dashboard akan dibuat untuk menampilkan:

* Sales trend
* Top product category
* Customer distribution
* Delivery performance

---



## Analysis

### 1. Monthly Revenue Analysis

Analisis dilakukan untuk melihat pertumbuhan revenue dari waktu ke waktu.

Query yang digunakan:

```sql
SELECT
    DATE_TRUNC('month', o.order_purchase_timestamp) AS month,
    SUM(oi.price) AS total_revenue
FROM orders o
JOIN order_items oi
ON o.order_id = oi.order_id
GROUP BY month
ORDER BY month;
```

**Hasil:**
| Month                | Total Revenue |
|-----------------------|---------------|
| 2016-09-01 00:00:00.000 |        267.36 |
| 2016-10-01 00:00:00.000 |     49507.66 |
| 2016-12-01 00:00:00.000 |        10.90 |
| 2017-01-01 00:00:00.000 |    120312.87 |
| 2017-02-01 00:00:00.000 |    247303.02 |
| 2017-03-01 00:00:00.000 |    374344.30 |
| 2017-04-01 00:00:00.000 |    359927.23 |
| 2017-05-01 00:00:00.000 |    506071.14 |
| 2017-06-01 00:00:00.000 |    433038.60 |
| 2017-07-01 00:00:00.000 |    498031.48 |
| 2017-08-01 00:00:00.000 |    573971.68 |
| 2017-09-01 00:00:00.000 |    624401.69 |
| 2017-10-01 00:00:00.000 |    664219.43 |
| 2017-11-01 00:00:00.000 |   1010271.37 |
| 2017-12-01 00:00:00.000 |    743914.17 |
| 2018-01-01 00:00:00.000 |    950030.36 |**


**Insight:**
* Revenue menunjukkan pertumbuhan yang sangat signifikan dari awal 2017 hingga pertengahan 2018, menandakan fase ekspansi bisnis yang cepat.
* Terdapat lonjakan tajam pada November 2017 (±1M), kemungkinan dipengaruhi oleh event besar seperti promo atau seasonal event (misalnya Black Friday).
* Setelah mencapai puncak di awal–pertengahan 2018, revenue cenderung stabil dengan sedikit fluktuasi.
* Penurunan ekstrem pada September 2018 kemungkinan disebabkan oleh data yang belum lengkap (incomplete month), bukan penurunan bisnis nyata.

### 2. Top Product Category Analysis

#### Objective

Mengidentifikasi kategori produk dengan kontribusi revenue tertinggi.

#### SQL Query

```sql id="tcz9n7"
SELECT
    p.product_category_name,
    SUM(oi.price) AS total_revenue
FROM order_items oi
JOIN products p
ON oi.product_id = p.product_id
GROUP BY p.product_category_name
ORDER BY total_revenue DESC
LIMIT 10;
```

#### Result
|product_category_name |total_revenue|
|----------------------|---------------
|beleza_saude          |   1258681.34|
|relogios_presentes    |   1205005.68|
|cama_mesa_banho       |   1036988.68|
|esporte_lazer         |    988048.97|
|informatica_acessorios|    911954.32|
|moveis_decoracao      |    729762.49|
|cool_stuff            |    635290.85|
|utilidades_domesticas |    632248.66|
|automotivo            |    592720.11|
|ferramentas_jardim    |    485256.46|

Kategori produk dengan revenue tertinggi adalah *beleza_saude* dengan total revenue sekitar 1.25M, diikuti oleh beberapa kategori lain dengan nilai yang relatif berdekatan.

#### Insights

* Kategori *beleza_saude* menjadi kontributor revenue terbesar, namun tidak mendominasi secara signifikan dibanding kategori lainnya.
* Top 10 kategori menunjukkan distribusi revenue yang cukup merata, dengan selisih antar kategori yang tidak terlalu besar.
* Hal ini menunjukkan bahwa bisnis tidak bergantung pada satu kategori saja, melainkan memiliki portofolio produk yang cukup terdiversifikasi.

#### Business Implication

* Perusahaan dapat mempertahankan strategi multi-category karena kontribusi revenue tersebar.
* Kategori dengan performa tinggi seperti *beleza_saude* dan *relogios_presentes* dapat dijadikan fokus untuk campaign marketing.
* Kategori dengan performa lebih rendah dalam top 10 masih memiliki potensi untuk ditingkatkan melalui promosi atau bundling produk.

### 3. Customer Behavior Analysis

#### Objective

Menganalisis perilaku customer untuk memahami tingkat repeat purchase dan loyalitas customer.

#### SQL Query

```sql
SELECT
    COUNT(*) FILTER (WHERE total_orders = 1) AS one_time_customer,
    COUNT(*) FILTER (WHERE total_orders > 1) AS repeat_customer
FROM (
    SELECT
        c.customer_unique_id,
        COUNT(o.order_id) AS total_orders
    FROM orders o
    JOIN customers c
    ON o.customer_id = c.customer_id
    GROUP BY c.customer_unique_id
) t;
```

####  Result
one_time_customer|repeat_customer|
|-----------------|----------------
|            93099|           2997|
* One-time customer: 93,099
* Repeat customer: 2,997
* Repeat rate: ~3%

#### Insights

* Sebagian besar customer (~97%) hanya melakukan satu kali pembelian, menunjukkan tingkat retensi yang sangat rendah.
* Hanya sekitar 3% customer yang melakukan pembelian ulang, yang mengindikasikan rendahnya loyalitas customer.
* Pola ini menunjukkan bahwa bisnis lebih bergantung pada akuisisi customer baru dibanding mempertahankan customer lama.

#### Business Implication

* Perusahaan perlu meningkatkan strategi customer retention, seperti loyalty program, email marketing, atau personalized promotion.
* Fokus tidak hanya pada akuisisi, tetapi juga pada meningkatkan lifetime value dari customer yang sudah ada.
* Analisis lebih lanjut diperlukan untuk memahami faktor yang menyebabkan rendahnya repeat purchase (misalnya: pengalaman pelanggan, harga, atau kualitas produk).

  ### 4. Delivery Performance Analysis

#### Objective

Menganalisis performa pengiriman untuk mengetahui tingkat ketepatan waktu delivery.

#### 🧾 SQL Query

```sql
SELECT
    SUM(CASE 
        WHEN order_delivered_customer_date <= order_estimated_delivery_date 
        THEN 1 ELSE 0 
    END) AS on_time_delivery,

    SUM(CASE 
        WHEN order_delivered_customer_date > order_estimated_delivery_date 
        THEN 1 ELSE 0 
    END) AS late_delivery

FROM orders
WHERE order_status = 'delivered';
```

####  Result

|on_time_delivery|late_delivery|
|----------------|--------------
|           88644|         7826|

* On-time delivery: 88,644
* Late delivery: 7,826
* On-time rate: ~92%
* Late rate: ~8%

#### Insights

* Sebagian besar pengiriman (±92%) berhasil dilakukan tepat waktu, menunjukkan performa operasional yang cukup baik.
* Namun, sekitar 8% pengiriman mengalami keterlambatan, yang dapat berdampak pada kepuasan customer.
* Meskipun terlihat kecil, persentase keterlambatan ini cukup signifikan dalam skala besar dan berpotensi mempengaruhi customer retention.

####  Business Implication

* Perusahaan perlu mengidentifikasi faktor penyebab keterlambatan, seperti lokasi geografis, performa seller, atau logistik.
* Optimalisasi supply chain dan kerja sama dengan partner logistik dapat membantu mengurangi keterlambatan.
* Monitoring delivery performance secara berkala penting untuk menjaga kualitas layanan dan kepuasan pelanggan.

  ## 📁 Dataset

Dataset yang digunakan dalam project ini adalah Brazilian E-Commerce Public Dataset (Olist).

Source:
https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce


  



