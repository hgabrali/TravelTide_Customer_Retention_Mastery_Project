## 📝 Week 1: Raw Data Inspection and Initial Cleaning

## 🎯 Project Goals and Objectives

The primary objective of this project is to develop a data-driven customer retention strategy for TravelTide by leveraging advanced analytical and Machine Learning techniques.

### I. Data Validation and Foundation
1.  **Establish Data Integrity:** Successfully connect to the PostgreSQL database, fetch raw data, and perform comprehensive cleaning (e.g., date conversion, imputing missing discount values with 0) to ensure high data quality.
2.  **Generate Core Metrics (Feature Engineering):** Engineer meaningful user-level attributes (e.g., tenure, booking frequency, discount sensitivity) from session-level data to enable robust behavioral analysis.

### II. Customer Segmentation and Insight Generation
3.  **Validate Business Hypothesis:** Use clustering methods (e.g., KMeans) to objectively test Elena Tarrant's hypothesis regarding the existence of distinct customer segments sensitive to specific rewards/perks.
4.  **Identify Meaningful Segments:** Divide the entire customer base into distinct, interpretable groups based on behavioral patterns and discount sensitivity (ML-Based Segmentation).

### III. Strategic Recommendation and Delivery
5.  **Assign Optimal Perks:** For each identified customer segment, assign the corresponding "favorite perk" from the proposed list (e.g., Free Cancellation, Free Hotel Meal).
6.  **Develop Actionable Strategy:** Deliver a clear, data-backed strategic recommendation to the Head of Marketing (Elena Tarrant) on how to personalize rewards invitations to maximize customer sign-ups and improve long-term retention.



### Objective
Before proceeding to Feature Engineering, the goal of this stage was to understand the structure, scale, and quality of the raw data fetched from the TravelTide PostgreSQL database.

### 1. Initial Data Status (Raw State)

| Table | Size (Rows) | Initial Observations |
| :--- | :--- | :--- |
| **df_users** | 1,020,926 | Contains demographic and location data. NO Missing Values. |
| **df_sessions** | *(To be determined)* | Expected to contain records of user interactions/bookings. |

#### **df_users Key Findings (Raw State):**

* **Data Completeness:** The table is exceptionally clean, with **NO Missing Values (NaN)**.
* **Data Types:** Two critical date columns, `birthdate` and `sign_up_date`, were incorrectly stored as `object` (string) type, requiring conversion.

---

### 2. Initial Data Transformations (Cleaning)

Based on the initial inspection, only data type conversion was required to prepare `df_users` for metric calculation.

| Transformation | Target Column(s) | Method | Result |
| :--- | :--- | :--- | :--- |
| **Type Conversion** | `birthdate`, `sign_up_date` | `pd.to_datetime()` | Converted from `object` to **`datetime64[ns]`** type. |

### Next Steps
The next critical step is to complete the initial inspection of the **df_sessions** table, address its data types and missing values, and then perform the necessary joins.


#### **df_sessions Key Findings (Raw State):**

* **Scale:** The table contains over **5.4 million entries**, confirming its session-level granularity and the need for **mandatory aggregation** to the user level.
* **Missing Data (Critical):** Columns `flight_discount_amount` and `hotel_discount_amount` have substantial missing data (around 4.5 million NaN values).
* **Assumption:** We hypothesize that these missing values represent transactions where **NO discount was applied.**
* **Action:** Missing values in discount columns **MUST be imputed with 0** before aggregation to avoid losing valuable session data.

* ### 📊 İstatistik Analizi ve Gözlemlenen Farklılıklar (df_sessions)

Bu tablo, oturum verileri (`df_sessions`) üzerindeki özet istatistiklerden elde edilen kritik bulguları ve temizlik için alınması gereken aksiyonları özetlemektedir.

| Sütun | Değer | Gözlem (Anomaly/Discrepancy) | Çıkarım ve Eylem Planı |
| :--- | :--- | :--- | :--- |
| **user_id (Count)** | 5,408,063 | `df_users` tablosundaki kullanıcı sayısından (1,020,926) **çok daha büyük** bir sayı. | `df_sessions` tablosu devasa! Bu, her kullanıcının ortalama 5'ten fazla oturum/işlem yaptığı anlamına gelir. **Agregasyon (toplama) zorunludur.** |
| **flight_discount_amount (Count)** | 885,796 | Toplam satır sayısından (5,408,063) **çok daha küçük**. | ⚠️ **Büyük Miktarda Eksik Değer (Missing Values)!** Yaklaşık 4.5 milyon satırda bu indirim miktarı bilgisi eksik. Bu, çoğu işlemin indirim kullanmadan yapıldığı anlamına gelir. |
| **hotel_discount_amount (Count)** | 691,380 | Yine toplam satır sayısından **çok daha küçük**. | ⚠️ **Büyük Miktarda Eksik Değer!** Otel indirimi de çoğu işlemde mevcut değil. Bu eksik değerleri **0 (sıfır) ile doldurmamız (impute) gerekebilir.** |
| **flight_discount_amount (Mean)** | 0.139765 | — | Ortalama uygulanan indirim oranı yaklaşık %14. |
| **hotel_discount_amount (Mean)** | 0.110950 | — | Ortalama uygulanan indirim oranı yaklaşık %11. |
| **Min Değerler** | 0.05 | Min indirim oranının 0.05 (%5) olması, indirimlerin 0 (sıfır) değil, belirli bir minimum değere sahip olduğunu gösteriyor. | Bu, indirim mekanizmasının yapısını anlamak için önemlidir. NaN'ların 0 olması gerektiği varsayımımızı güçlendirir. |

### 🔎 df_sessions: Sayısal İstatistik Analizi

#### 1. Sayısal Sütunların Eksiksizliği

| Sütun | Satır Sayısı (Count) | Toplam Satır | Eksik Veri Durumu |
| :--- | :--- | :--- | :--- |
| **user_id** | 5,408,063 | 5,408,063 | ✅ Tamam (No Missing Data) |
| **page_clicks** | 5,408,063 | 5,408,063 | ✅ Tamam (No Missing Data) |
| **İndirim Miktarları** | < 1 milyon | 5,408,063 | ⚠️ Büyük Eksiklik (Imputation Gerekli) |

#### 2. 🖱️ `page_clicks` Sütunu Analizi

Bu analiz, oturum başına tıklama sayısındaki aykırı değerleri (outliers) ve kullanıcı davranışını ortaya koymaktadır.

| İstatistik | Değer | İş Çıkarımı |
| :--- | :--- | :--- |
| **Min** | 0 | Bazı oturumların **hiç tıklama yapmadan** sona erdiğini gösterir. Bu, hemen çıkış (bounce) veya hata nedeniyle oluşan oturumlar olabilir. |
| **Ortalama (Mean)**| ~18.76 | Ortalama bir oturumda 18-19 tıklama yapılıyor. |
| **Max** | **2,421** | 🚨 Bu, potansiyel bir **aykırı değer (outlier)** işaretidir. Bir oturumda 2421 tıklama, normal bir kullanıcı davranışı değildir (örneğin, bot veya hatalı kayıt olabilir). |
| **75%** | 23 | Satırların %75'i 23 veya daha az tıklamaya sahipken, maksimum değer (2421) çok uzaktadır. **Eylem Planı:** Bu aykırı değerleri Feature Engineering aşamasında ele almalıyız. |


### 🎯 Customer Segmentation Comparison: Traditional vs. ML Methods

This table outlines the key methodological differences between conventional, rule-based segmentation and data-driven clustering methods required for the TravelTide project.

| Feature | 🏛️ Traditional Segmentation (Rule-Based) | 🤖 Machine Learning Segmentation (Clustering) |
| :--- | :--- | :--- |
| **Definition (Method)** | Grouping based on predefined business rules (demographics, geography) or subjective market research (surveys). | Grouping where algorithms (KMeans, Hierarchical Clustering, etc.) automatically discover natural patterns and clusters within the data. |
| **Data Focus** | Primarily **demographic** (Age, Gender, Income) and **geographic** data. | Primarily **behavioral** (Clicks, Purchase Frequency, Spending) and features derived through **Feature Engineering**. |
| **Group Count (k)** | **Subjectively** determined by the analyst or business need (e.g., must be 4 groups). | **Objectively** determined using metrics (e.g., Elbow Method, Silhouette Score) or driven by specific business objectives. |
| **Flexibility & Dynamics**| **Low.** Groups are static and slow to adapt to changing customer behavior or market trends. | **High.** Groups are dynamic, reflecting complex, evolving data patterns, and can be rapidly adjusted. |
| **Complexity (Dimensionality)** | Most effective in **low-dimensional** (2-4 features) datasets. Relationships found are often **simple** (e.g., over 30 + urban resident). | Strong in **high-dimensional** (10+ features) data, capable of identifying complex, non-linear relationships. |
| **Core Insight** | **Causality** ("Customers buy this product BECAUSE they live in this region."). | **Association/Clustering** ("These customers exhibit 5 distinct behaviors concurrently."). |

### 📊 Tüketici Segmentasyonunda Karşılaştırma

| Özellik | Geleneksel Segmentasyon (Traditional Methods) | Makine Öğrenimi Segmentasyonu (ML-Based Methods) |
| :--- | :--- | :--- |
| **Tanım (Yöntem)** | Önceden tanımlanmış iş kuralları (hukuk, coğrafya, demografi) veya pazar araştırması (anketler) ile yapılan kural tabanlı gruplama. | Algoritmaların (KMeans, Hiyerarşik Kümeleme vb.) verideki doğal desenleri otomatik olarak keşfetmesiyle yapılan gruplama. |
| **Veri Tipi Odak** | Temelde **demografik** (Yaş, Cinsiyet, Gelir) ve **coğrafi** veriler. | Temelde **davranışsal** (Tıklama, Satın Alma Sıklığı, Harcama) ve **Feature Engineering** ile üretilmiş veriler. |
| **Grup Sayısı (k)** | Analist tarafından **öznel** (subjective) olarak belirlenir (Örn: 4 grup olmalı). | **Nesnel** (objective) metrikler (Örn: Dirsek Metodu, Silhouette Skoru) veya iş hedeflerine göre belirlenir. |
| **Esneklik ve Dinamiklik** | **Düşük.** Gruplar statiktir ve değişen müşteri davranışına yavaş adapte olur. | **Yüksek.** Gruplar dinamik olarak değişen veri desenlerini yansıtır ve hızla adapte edilebilir. |
| **Karmaşıklık (Boyut)** | **Düşük boyutlu** (2-4 özellik) verilerde en etkilidir. İlişkiler genellikle **basittir** (örneğin, 30 yaş üstü + şehirli). | **Yüksek boyutlu** (10+ özellik) verilerde ve karmaşık, doğrusal olmayan ilişkilerde güçlüdür. |
| **Temel Çıkarım** | **Neden-Sonuç İlişkisi** ("Bu müşteriler bu şehirde yaşadıkları için bu ürünü alıyor"). | **Birliktelik** ("Bu müşteriler aynı anda bu 5 davranışı sergiliyor"). |


<img width="563" height="672" alt="image" src="https://github.com/user-attachments/assets/b8e82b56-513b-4ec9-b081-a01a424aea6f" />


