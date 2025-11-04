## 📝 Week 1: Raw Data Inspection and Initial Cleaning

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
