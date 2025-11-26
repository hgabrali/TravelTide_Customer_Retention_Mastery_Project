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


## 🧩 SQL Sorgu İşlevi: Analitik Üs Tablosu (ABT) Oluşturma 📊

SQL sorgusunun temel amacı, sonraki ML modellemesi için dört ana tabloyu birleştirerek tek, kapsamlı bir **Analitik Üs Tablosu (ABT)** oluşturmaktır.

| SQL Bileşeni ⚙️ | Teknik İşlevsellik 💡 | Proje Önemi 📈 |
| :--- | :--- | :--- |
| **SELECT Clause** (SEÇİM Yan Tümcesi) | **Özellik Seçimi.** Özellik Mühendisliği (**Feature Engineering**) ve Makine Öğrenmesi modelleri için gerekli her sütunu açıkça seçer. | Kritik veri noktalarını toplar: kullanıcı davranışı (`s.page_clicks`), demografik veriler (`u.gender`) ve işlem verileri (`f.base_fare_usd`, `h.hotel_price_per_room_night_usd`). |
| **FROM Clause** (KAYNAK Yan Tümcesi) | Birleştirme işleminin başlangıç noktasını (`sessions_spark` tablosu) belirtir. | Birincil ayrıntı düzeyini (granularity) tanımlar: çıktı tablosu **oturum düzeyinde** olacaktır. |
| **JOIN Türü: INNER JOIN** (İÇ BİRLEŞTİRME) | `sessions` ve `users` tablolarını `user_id` üzerinden birleştirmek için kullanılır. Yalnızca **her iki** tabloda da eşleşmenin olduğu satırları döndürür. | Kullanıcı bağlamı **her zaman esas** olduğu için, ABT'deki her oturum kaydının geçerli bir kullanıcıya bağlı olmasını sağlar. |
| **JOIN Türü: LEFT JOIN** (SOL BİRLEŞTİRME) | `flights` ve `hotels` tablolarıyla `trip_id` üzerinden birleştirmek için kullanılır. *Sol* tablodaki (`sessions` ve `users`) tüm kayıtları ve *sağ* tablodan eşleşen kayıtları döndürür. | **Segmentasyon** analizi için çok önemlidir. Kullanıcının göz attığı ancak **rezervasyon yapmadığı** oturumları korur ve bu durumlarda uçuş/otel detayları için `NULL` değerler döndürür. |


## II. 💾 Büyük Veride SQL Neden Zorunludur? (PySpark Bağlamı) 🧠

PySpark ortamında (`%%sql` ile çalıştırıldığında), dönüşüm için SQL kullanmak isteğe bağlı değildir; **yüksek performans ve ölçeklenebilirlik** elde etmek için bir **zorunluluktur**.

| Sebep 🌟 | Teknik Açıklama 📝 | Standart Python/Pandas'a Göre Avantajı 🚀 |
| :--- | :--- | :--- |
| **Performans (Vektörizasyon)** | SQL sorguları ve Spark SQL, veriyi tüm düğümler ve çekirdekler arasında **sütun bazlı ve dağıtık bir şekilde** işler, böylece **vektörizasyondan** yararlanır. | Özellikle büyük veri setlerinde, standart Python/Pandas'taki geleneksel satır bazlı döngülere göre **önemli ölçüde daha hızlıdır**. |
| **Optimizasyon (Catalyst Optimizer)** | Spark, arka planda akıllı **Catalyst Optimizer** motorunu kullanır. Bu motor, SQL sorgunuzu otomatik olarak en **verimli fiziksel yürütme planına** dönüştürür. | Elle yazılmış Python/Pandas kodunun ulaşmasının zor veya imkansız olduğu düzeyde bir hız ve verimlilik sağlar. |
| **Bellek Yönetimi** | Spark SQL, veri yükleme, akış ve ara sonuçları kümenin belleğinde ve diskinde verimli bir şekilde yönetir. | **"Bellek Hatası" riskini azaltır.** Tüm veriyi tek bir makinenin RAM'ine yüklemeye çalışan Pandas'ın aksine, Spark, yerel makinenin bellek kapasitesinden çok daha büyük veri setlerini işleyebilir. |

## 🛠️ İşe Yarayan SQL Araçları ve Bunları Tespit Etme Yolları 🔍

Büyük veri bağlamında (özellikle Spark SQL/PySpark kullanırken) işinize en çok yarayacak SQL yapıları ve fonksiyonları aşağıdadır.

| SQL Aracı (Tool) 🔧 | Ne İşe Yarar? 💡 | Nasıl Tespit Edilir? (Analiz Sorusu) 🤔 |
| :--- | :--- | :--- |
| **JOIN Türleri** (`INNER`, `LEFT`, `RIGHT`) | Farklı tablolardaki bilgileri anahtarlar (`user_id`, `trip_id`) üzerinden birleştirmeyi sağlar. | **Analiz Sorusu:** Hangi verileri bir arada görmeniz gerekiyor? *Örneğin, "Tüm kullanıcıları, rezervasyon yapmış olsalar da olmasalar da görmek istiyorsam **LEFT JOIN** kullanmalıyım."* |
| **WINDOW Fonksiyonları** (`ROW_NUMBER()`, `LAG()`, `OVER (PARTITION BY...)`) | Bir tablonun tamamına bakmak yerine, belirli gruplar (`partition`) içinde sıralama, kümülatif toplam alma veya önceki/sonraki satırlara erişme imkanı verir. | **Analiz Sorusu:** "Her bir kullanıcı için yaptığı son 3 uçuşu bulmalıyım" veya "Aylık kümülatif satışları hesaplamalıyım." |
| **Aggregation Fonksiyonları** (`AVG`, `SUM`, `COUNT`, `MAX`) | Veri grupları üzerinde özet istatistikler üretir (*Örn: `GROUP BY user_id` ile her kullanıcı için toplam tıklama sayısını bulmak*). | **Analiz Sorusu: Feature Engineering:** Bir kullanıcının davranışını tek bir satırda özetlemem gerekiyor mu? *(Evet ise, `SUM/AVG` kullanın)*. |
| **CAST / DATE Fonksiyonları** (`CAST()`, `DATE_FORMAT()`) | Veri tiplerini dönüştürme ve tarih-saat verilerini işleme (*örn: "yaşı hesaplamak için `birthdate` sütununu kullanmak"*). | **Veri Kalitesi Kontrolü:** Sütunların veri tipleri doğru mu? `session_start` gibi bir zaman damgasından gün, ay, yıl gibi yeni özellikler türetmek gerekiyor mu? |



<img width="644" height="643" alt="image" src="https://github.com/user-attachments/assets/9d851284-dba5-4e3b-bcaf-1f6a59ed24af" />

## 💾 SQL Kod Analizi: Analitik Üs Tablosu (ABT) Oluşturma 📊

Bu PySpark SQL kodu, dört farklı tablo üzerinde kapsamlı **veri birleştirme** (Özellik Seçimi ve Veri Entegrasyonu) gerçekleştirmek için `CREATE OR REPLACE TABLE` komutunu kullanır ve sonuç olarak **Özellik Mühendisliği (Feature Engineering)** ve **ML tabanlı Müşteri Segmentasyonu** için hazır, birleşik bir veri seti oluşturur.

| Bileşen | Fonksiyon / Komut 💻 | Projedeki Teknik Önemi & Rolü 🌟 |
| :--- | :--- | :--- |
| **Başlık (Header)** | `%%sql`<br>`CREATE OR REPLACE TABLE sessions_joined AS` | **Veri Kalıcılığı ve SQL Erişimi.** PySpark ortamına kodu bir SQL sorgusu olarak çalıştırmasını ve sonucu `sessions_joined` adında yeni, sorgulanabilir bir tablo olarak kaydetmesini söyler. Bu tablo, **Analitik Üs Tablosu (ABT)** olarak hizmet eder. |
| **SELECT Clause** | `s.session_id, s.user_id, ...`<br>*(s, u, f, h'den sütunlar içerir)* | **Özellik Seçimi ve Entegrasyonu.** Dört ayrı tablodan gerekli tüm ham özellikleri açıkça seçer. Bu adım, ML modelleri için gerekli olan özellikleri (örneğin kullanıcı bağlamı, oturum davranışı ve işlem detayları) standartlaştırır. |
| **FROM Clause** | `FROM sessions_spark s` | **Ayrıntı Düzeyini Tanımlama.** Birleştirme işleminin temeli olarak `sessions_spark` tablosunu ayarlar. Bu nedenle ortaya çıkan ABT, **oturum düzeyinde** olacaktır. |
| **INNER JOIN** | `INNER JOIN users_spark u ON s.user_id = u.user_id` | **Temel Bağlamı Sağlama.** ABT'ye dahil edilen her oturumun geçerli bir kullanıcı kaydına bağlı olmasını garanti eder. Kullanıcının demografik ve statik verileri (`u.*` sütunları) segmentasyon için hayati öneme sahiptir. |
| **LEFT JOIN** | `LEFT JOIN flights_spark f ON s.trip_id = f.trip_id`<br>`LEFT JOIN hotels_spark h ON s.trip_id = h.trip_id` | **İşlem Dışı Verileri Korumak (Segmentasyon Odağı).** Bu çok önemlidir:<br> 1. TÜM oturumları korur (hiçbir rezervasyonun gerçekleşmediği oturumlar dahil).<br> 2. Bir oturumun eşleşen uçuş/otel verisi yoksa, işlem sütunları (`f.*`, `h.*`) `NULL` içerecektir.<br>**Önemi:** Bu `NULL` değerinin kendisi, segmentleri tanımlamak için gerekli olan yalnızca göz atma davranışını gösteren önemli bir özelliktir. |
| **Sütun Takma Adı** | `h.hotel_price_per_room_night_usd AS hotel_per_room_usd` | **Veri Temizleme ve Standardizasyon.** Sonraki Özellik Mühendisliği ve modelleme adımlarında kullanım kolaylığı için karmaşık veya uzun bir sütun adını daha basit, standartlaştırılmış bir formata (`hotel_per_room_usd`) yeniden adlandırır. |

---

### 📝 Önem Özeti

Bu tek SQL bloğu, iş akışınızdaki **en temel adımdır** ve şunları sağlar:

1.  **Veri Entegrasyonu:** Farklı veri kaynaklarını tutarlı bir analitik görünüme birleştirir.
2.  **Ölçeklenebilirlik:** Bu karmaşık birleştirmeyi Spark'ın dağıtık hesaplama gücünü kullanarak verimli bir şekilde yürütür.
3.  **ML İçin Temel:** Sonraki tüm **Özellik Mühendisliği** ve **ML tabanlı Müşteri Segmentasyonu** adımları için gerekli olan nihai giriş tablosunu (ABT) oluşturur.


## 💾 Pre-Filtering and Sampling Analysis (PySpark SQL) 🚀

<img width="679" height="521" alt="image" src="https://github.com/user-attachments/assets/728e98b7-3412-4c49-8d0e-196e60c91131" />

## 💾 Ön Filtreleme ve Örnekleme Analizi (PySpark SQL) 🚀

Bu SQL kodu, ana birleştirme işlemi **öncesinde** başlangıç veri filtrelemesi ve kullanıcı örneklemesi yapmak için **Ortak Tablo İfadelerini (CTE'ler)** kullanır. Bu, Büyük Veri analizinde kritik bir performans optimizasyon tekniğidir.

| Bileşen / İşlem ⚙️ | Teknik İşlevsellik ve Amacı 💡 | Analitik Katkısı ve Önemi 🌟 |
| :--- | :--- | :--- |
| **WITH sessions_2023 AS (...)** | **Zamana Dayalı Filtreleme (Temporal Filtering).** Bu CTE, `sessions_spark` temel tablosunu yalnızca `session_start` değeri belirli bir tarihten (`'2023-01-04'`) sonra olan kayıtları içerecek şekilde filtreler. | **Veri Hacmini Azaltır.** İşlenen veri setinin boyutunu önemli ölçüde azaltır, sonraki karmaşık birleştirmeler için performansı artırır ve bellek yükünü düşürür. Analizin yalnızca en **güncel ve ilgili verilere** odaklanmasını sağlar (segmentasyon için güncel davranış daha tahmin edicidir). |
| **WITH filtered_users AS (...)** | **Kullanıcı Örneklemesi (Davranışsal Filtre).** Bu CTE, veriyi `user_id`'ye göre gruplar ve oturum sayısı (`session_count`) 7'den az olan kullanıcıları (`HAVING COUNT(*) > 7`) filtreler. | **Etkileşimli Kullanıcılara Odaklanma (Pasif Kullanıcıların Çıkarılması).** Bu, **Özellik Mühendisliği (Feature Engineering)** için temel bir adımdır: <br> 1. **Gürültüyü Giderir:** Tek seferlik ziyaretçileri veya botları filtreler. <br> 2. **Veri Kalitesini Sağlar:** **Müşteri Segmentasyonu** modelinin, sağlam ve anlamlı bir segment profili oluşturmak için yalnızca **yeterli davranışsal veriye** sahip kullanıcılar üzerinde eğitilmesini garanti eder. |
| **Neden SQL'de?** | **PySpark Catalyst ile Optimizasyon.** Bu filtrelerin başlangıçtaki `WITH` ifadeleri içinde yürütülmesi, Spark'ın Catalyst Optimizer'ının filtreleme mantığını veri kaynağına (veritabanı/veri gölü) itmesini sağlar. | **Maksimum Performans.** Büyük birleştirme işleminden **önce** veri hacmini azaltmanın en verimli yoludur. Bu karmaşık filtrelemeyi birleştirmeden *sonra* Pandas'ta yapmak, büyük performans darboğazlarına ve bellek sorunlarına yol açacaktır. |

---

## 📝 Ön İşlemenin Önemi Özeti (Spark SQL'deki CTE'ler) 🚀

Filtreleme ve örnekleme işlemlerini Spark SQL sorgusunun içinde (Ortak Tablo İfadeleri veya CTE'ler kullanarak) gerçekleştirmek **hayati** öneme sahiptir, çünkü:

| Fayda Kategorisi 🌟 | Detay ve Sonuç 💡 |
| :--- | :--- |
| **Performans Optimizasyonu** | Spark'ın dağıtık işleme gücünden yararlanarak büyük veri setlerinin filtrelemesini **verimli bir şekilde** yönetir, böylece sonraki karmaşık işlemler için belleğe yüklenmesi gereken veri miktarını önemli ölçüde azaltır. |
| **Artan Model Kalitesi** | Ortaya çıkan Analitik Üs Tablosunun (`session_base`), yalnızca aktif, yakın zamanda etkileşimde bulunmuş kullanıcılardan türetilen **yüksek kaliteli verileri** içermesini sağlar, bu da ML aşamasında daha **uygulanabilir ve tahmin gücü yüksek** müşteri segmentlerine yol açar. |

## 🛑 Negatif veya Anormal Değer Analizi (Hata Tespiti) 🔎

Sağlanan istatistiksel özette, bağlamına göre hatalı veya mantıksal olarak anormal sayılabilecek değerlerin tespiti ve nedenleri aşağıdadır.

| Sütun Adı 📝 | Negatif/Anormal Değer Tespiti 💡 | Neden Olabilecekler ve Analiz Katkısı 🚀 |
| :--- | :--- | :--- |
| **home_airport_lon** (Ev Havaalanı Boylamı) | **MIN:** $-157.927000$ (Negatif) | **COĞRAFİ BEKLENTİ.** Boylam (Longitude) değerleri, dünyanın batı yarım küresi için doğal olarak negatiftir (örn. ABD, Kanada, Güney Amerika). Bu değerler **hata değil**, coğrafi konumun doğru temsilidir. |
| **destination_airport_lon** (Varış Havaalanı Boylamı) | **MIN:** $-157.927000$ (Negatif) | **COĞRAFİ BEKLENTİ.** Aynı şekilde, bu değerler de coğrafi konumu temsil eder ve bu aralık, veri setindeki uçuşların Batı Yarımküre'deki uzak noktalara yapıldığını gösterir. |
| **destination_airport_lat** (Varış Havaalanı Enlemi) | **MIN:** $-37.008000$ (Negatif) | **COĞRAFİ BEKLENTİ.** Enlem (Latitude) değerleri güney yarım küre için negatiftir (örn. Arjantin, Avustralya, Güney Afrika). Bu, kullanıcıların Güney Yarımküre'deki yerlere uçuş rezervasyonu yaptığını gösteren **geçerli bir veridir**. |
| **checked_bags** (Kontrol Edilen Bagaj Sayısı) | **MIN:** $0.000000$ (Sıfır) | **BEKLENEN DEĞER.** Bagaj sayısının sıfır olması, kullanıcının hiç bagaj kontrol ettirmediği anlamına gelir. Bu, negatif bir değer olmadığı için **hata değil**, veri setindeki bir durumu (özellik değeri) temsil eder. |
| **base_fare_usd** (Temel Ücret USD) | **MIN:** $2.410000$ (Pozitif) | **ANORMAL DÜŞÜK DEĞER.** Minimum değer negatiftir. Ancak bu tabloya göre en düşük değer $2.41$ USD'dir. Bu fiyat çok düşük olsa da (hata veya promosyon olabilir), teknik olarak pozitif bir ücrettir. **EĞER MIN değeri negatif olsaydı**, bu bir veri girişi hatasını (base\_fare'in negatif olması anlamsızdır) veya iade/iade işlemini gösterebilirdi. |
| **nights** (Gece Sayısı) | **MIN:** $-2.000000$ (Negatif) | **VERİ GİRİŞ HATASI / ANORMALLİK.** Bir otelde geçirilen gece sayısının negatif olması **mantıksal olarak imkansızdır**. Bu, kesinlikle bir **veri temizleme (Data Cleaning)** adımı gerektiren bir **veri girişi/birleştirme hatasıdır (data entry/join error)**. Bu kayıtlar ya çıkarılmalı ya da `NaN` olarak ayarlanmalıdır. |
| **flight_discount_amount** | **MIN:** $0.050000$ (Pozitif) | **BEKLENEN DEĞER.** İndirim oranıdır. Negatif bir indirim (yani zam) beklenmez. Minimum indirim oranının $\%5$ olması beklenir. |


## 📊 Categorical-Numeric Relationship Analysis Insights 🔎

### 1. Grouped by `is_transactional` (0: Browsing, 1: Transaction-Focused) 🎯

| Metric | Non-Transactional (0) | Transactional (1) | Fark (Kat / % Fark) | Analiz |
| :--- | :--- | :--- | :--- | :--- |
| **Avg Log Clicks** | 2.23 | 3.23 | **%44 Daha Yüksek** | **Doğrulandı:** İşlemsel oturumlar, işlemsel olmayanlara göre **%44 daha fazla** sayfa tıklaması (browsing effort) içerir. |
| **Avg Session Duration** | 85.12 saniye | 386.04 saniye | **4.5 Kat Daha Uzun** | **Çok Güçlü Doğrulama:** İşlemsel oturumlar, işlemsel olmayanlardan **4.5 kat daha uzun sürer.** Bu, işlem odağının kullanıcı etkileşimini dramatik bir şekilde artırdığını gösterir. |
| **Avg Log Hotel Price** | NULL | 5.01 | N/A | **Doğrulandı:** İşlemsel olmayan oturumlarda ortalama fiyat hesaplanamaz (NULL). İşlemsel oturumlar, ortalama $e^{5.01} - 1 \approx \$149.3$ fiyatında otel içerir. |

## 📉 2. Grouped by `has_flight_discount` (Presence of Discount)

| Metric | Discount Applied | No Discount Applied (Eksik) | Fark | Analiz |
| :--- | :--- | :--- | :--- | :--- |
| **Avg Log Clicks** | 2.57 | Eksik | N/A | **Yorum:** Sadece indirim uygulanan oturumların ortalaması (2.57) hesaplanmıştır. İndirim uygulanmayan oturumların ortalamasını görmek, bu göstergenin etkisini daha net anlamamızı sağlardı. |
| **Avg Session Duration** | 187.25 saniye | Eksik | N/A | **Yorum:** İndirim uygulanan oturumlar ortalama 187 saniye sürer. Bu süre, işlemsel olmayan oturumlar (85s) ile işlemsel oturumlar (386s) arasında bir yerde yer alır. |

---
---

# ⚓ Algoritmik Çapa: İleri Düzey Makine Öğrenimi ile TravelTide İçin Veri Odaklı Müşteri Tutundurma Stratejisi

## 📋 Bölüm I: Temel Değerlendirme: Global OTA Ekosisteminde TravelTide

### 1.1 TravelTide'ın Mevcut Konumu ve Temel Teklifleri 🚀

2023'te kurulan TravelTide, yüksek rekabetli Çevrimiçi Seyahat Acentesi (OTA) pazarında **çevik, fonlanmamış bir giriş** olarak faaliyet göstermektedir.¹ Başlangıç fonu olmaması, şirketin **verimlilik, maliyet yönetimi ve hızlı gelir üretimi** üzerine aşırı odaklanmasını gerektirmektedir. Yaklaşık 243 aktif rakibin bulunduğu bir ortamda, TravelTide'ın stratejisi, **Müşteri Yaşam Boyu Değeri (LTV)** maksimizasyonuna odaklanmalıdır; zira yüksek maliyetli geniş çaplı müşteri edinme (acquisition) kampanyaları finansal olarak sürdürülemezdir. **Üstün müşteri tutundurma (retention) bu nedenle temel bir hayatta kalma mekanizmasıdır.**

| Özellik | Açıklama | Stratejik Gereklilik |
| :--- | :--- | :--- |
| **Temel Teknolojik Avantaj** | **Yapay Zekâ (YZ) destekli seyahat güzergâhı oluşturucu**¹ | Platformu, basit bir işlem portalı olmaktan çıkarıp **akıllı bir hizmet sağlayıcı** olarak konumlandırır. |
| **Birincil Tutundurma Stratejisi** | Temel YZ yeteneğini sistematik olarak kullanarak ödülleri ve hizmet sunumunu **kişiselleştirmelidir**. | Kişiselleştirmenin **ölçeklenebilir, yüksek derecede ilgili ve maliyet-etkin** olmasını sağlar. |
| **LTV Odaklanma Zorunluluğu** | Rakipler arasındaki başlangıçtaki düşük karşılaştırmalı referans puanı (10/100) ile desteklenir.¹ | Kaynak tahsisi, düşük verimli geniş çaplı edinme çabaları yerine **mevcut kullanıcılardan kârlılığı maksimize etmeye** yönlendirilmelidir. |
| **Kurumsal DNA** | Kodlanmış amaç, **Reklam ajansları, Pazar araştırması ve İhtisaslaşmış tasarım faaliyetleri** üzerine yoğunlaşmıştır.² | **Titiz veri analizi ve stratejik iletişime** yatkın olup, ML (Makine Öğrenimi) odaklı bir tutundurma stratejisine geçişi kolaylaştırır. |

---

### 1.2 Operasyonel Karmaşıklık ve Verimlilik Gerekliliği ⚙️

TravelTide'ın fonlanmamış durumu, **Operasyonel Karmaşıklığın** titizlikle yönetilmesini gerektirir.¹ Hesaplama yoğunluğu yüksek ML modellerini içeren herhangi bir harcama, **anlık ve ölçülebilir bir Yatırım Getirisi (ROI)** göstermelidir.

| Operasyonel Zorluk | Gereklilik | Tutundurma Etkisi |
| :--- | :--- | :--- |
| **Verimlilik Gerekliliği** | Tutundurma stratejisi, genelleştirilmiş pazarlama yerine, başlangıçta **yüksek etkili, yüksek ROI'li öngörüsel analitikleri** hedeflemelidir. | Sadece kârlı segmentlere odaklanarak bütçe optimizasyonu. |
| **Hizmet Önkoşulu** | **Envanter Yönetiminde Etkinlik** ve harici envanter API'ları ile sağlam **Gerçek Zamanlı Entegrasyon** zorunlu ön koşullardır. | Kullanıcı sürtünmesini (friction) en aza indirmek için doğru fiyatlandırma ve uygunluk sağlar. |
| **Maliyetten Kaçınma** | Envanter hatalarından kaynaklanan müşteri hizmetleri sorunlarının yüksek operasyonel maliyeti, YZ geliştirme maliyetinden fazladır. | **Kusursuz envanter senkronizasyonu**, rezervasyon terkini önleyen ve müşteri memnuniyetini maksimize eden temel bir gerekliliktir. |

---

### 1.3 Pazar Rolü, Komisyon Erozyonu ve Yüksek Komisyon Dengesi 📉

TravelTide'ın temel **Pazar Rolü**, üstün YZ hizmeti aracılığıyla gezginlerin doğrudan rezervasyon alternatifleri yerine bu platformu seçmesini sağlayacak yeterli algılanan değer yaratmaktır.

| Pazar Dinamiği | Açıklama | Tutundurma Çözümü |
| :--- | :--- | :--- |
| **Pazar Rolü Zorluğu** | Tedarikçilerin rekabetçi fiyatlandırma yoluyla doğrudan rezervasyonu teşvik etmesiyle artan **Komisyon Erozyonu** trendi. | Yapay zekâ üstünlüğü ile benzersiz değer teklifleri oluşturmak. |
| **Hayatta Kalma Stratejisi** | Hayatta kalmak için her işlemde marjı maksimize ederek **Yüksek Komisyon Dengesi** için aktif olarak çabalamalıdır.¹ | Yüksek marjın sürdürülmesini sağlamak. |
| **Tutundurma Kaldıracı** | Sadık, tekrar eden müşteriler genellikle daha düşük fiyat hassasiyeti gösterir ve **Yan Gelir (Ancillary Revenue)** artışlarına daha açıktır. | Kapsamlı ML tutundurma stratejisi, kârlı ve değer odaklı bir müşteri tabanı yetiştirerek **komisyon erozyonunu doğrudan hafifletir**. |

***

## 💡 Bölüm II: Makro-Ekonomik Dinamikler ve OTA'lardaki Rekabet Kolları

### 2.1 Sofistike Değer Yönetimi ile Fiyatlandırma Çatışmasını Ele Alma 💲

OTA sektöründeki kalıcı **Fiyatlandırma Çatışması**, kullanıcıları çekmek için rekabetçi fiyatlar sunma ile kârlılık için gereken **Yüksek Komisyon Dengesi'ni** sürdürme arasındaki gerilimdir.¹

| Strateji | Mekanizma | Amaç |
| :--- | :--- | :--- |
| **ML Odaklı Fiyat Farklılaştırması** | Fiyat kararları, iki öngörücü faktöre göre dinamik, özelleştirilmiş ve optimize edilmelidir: kullanıcının **öngörülen LTV'si**³ ve gösterilen **fiyat hassasiyeti** (özellikle genç gezginlerde yüksek).⁴ | Mümkün olan en yüksek marjla rezervasyonu güvence altına alan dinamik fiyatlandırma uygulamak. |
| **Uygulama Örneği** | Yüksek LTV'li bir müşteri, maksimum indirim sunulmadan tutundurulabilir; yüksek riskli kayıp (churn) müşterisi, anlık işi ve gelecekteki sadakati güvence altına almak için **hassas hesaplanmış bir indirim** alabilir. |

### 2.2 Dinamik Paketleme ve Yan Gelir Optimizasyonu 📦

YZ güzergâhı oluşturma işlevi, sofistike **Dinamik Paketleme** yapmak için kullanılmalıdır.

| Konsept | Fonksiyon | Kârlılık Etkisi |
| :--- | :--- | :--- |
| **Dinamik Paketleme** | Uçuşları, konaklamayı, aktiviteleri ve kara taşımacılığını akıllıca **tescilli paketler** halinde birleştirmek. | Tescilli paketlerin, tedarikçi sitelerindeki bireysel bileşenlerle kolayca fiyat karşılaştırması yapılamayacağı için **marjları korur**. |
| **Yan Gelir (Ancillary Revenue)** | Kârlılığın hayati bir bileşeni olarak, **yüksek hedefli upsell'ler** için ayrıntılı segmentasyon bulgularını kullanır. | Ailelerin ortalama 0.83 bagaj kontrol etmesi gibi bulgular (**Çapraz Satış Potansiyeli**), premium bagaj hizmetleri veya yüksek marjlı sigorta paketlerinin proaktif olarak entegrasyonunu sağlar. |

### 2.3 Hizmet Genişletme ve Deneyim Geliştirme 🌟

Sürdürülebilir tutundurma, ilk işlemin ötesine geçen değer sağlamayı gerektirir.

| Alan | Odaklanma | Mekanizma/Teknoloji |
| :--- | :--- | :--- |
| **Hizmet Genişletme** | **Öngörücü ihtiyaç analizine** yoğunlaşarak çekirdek olmayan gelir akışlarına yol açmak. | Seyahat planlamasından¹ **aktif seyahat içi risk mitigasyonuna** (göçmenlik darboğazları, uçuş gecikme modelleri, vize belgeleri) geçiş.⁵ |
| **Deneyim Geliştirme** | YZ'nin bir planlama aracından **aktif bir seyahat destek ajansına** kesintisiz geçişi. | Kullanıcı memnuniyetsizliği kaynaklarını ele alarak kayıp (churn) öncesi sorunları öngörmek. |

***

## 🧠 Bölüm III: Veri Odaklı Tutundurma Gerekliliği: TravelTide Müşterisini Tanımlama

### 3.1 Mevcut Müşteri Segmentasyon Analizinin İncelenmesi ve Uygulanabilirliği ✅

TravelTide, 2024'te müşteri segmentasyon analizini başarıyla yürüterek sağlam bir veri temeli oluşturmuştur. Bu çalışma, **müşteri tutundurmayı artırmak** için tasarlanmış özelleştirilmiş bir ödül programının zeminini hazırlamıştır.⁴

| Segment | Uygulanabilir Bulgular | Tutundurma Kampanyası |
| :--- | :--- | :--- |
| **Aile Segmenti (%66 otel rezervasyonu)** | **Ücretsiz Kayıtlı Bagaj** ve **Ücretsiz Otel Yemeği** gibi lojistik ayrıcalıklara yüksek değer verirler.⁴ | Daha karmaşık ML modelleri eğitilirken bu kârlı segmenti hedefleyen **kural tabanlı tutundurma kampanyalarının** acil uygulanması. |
| **Genç/Bütçe Segmenti (15-35 Yaş)** | **Yüksek fiyat hassasiyeti** gösterir (daha düşük ortalama baz ücretler). | **Özel İndirimler** ve esnek bütçe paketlerini vurgulayan özel pazarlama. |
| **Yaşlı/Konfor Segmenti (55+ Yaş)** | Ortalama baz ücretlerde artış eğilimi gösterir, konfora ve deneyim kalitesine öncelik verirler.⁴ | **Her Şey Dahil paketler** ve premium hizmetler ile hedeflemek için ideal segment. |
| **Mevcut Proje Çıktısı** | `cancellation_rate`, `total_trips`, `total_hotel_spend` gibi zengin **mühendislik edilmiş özellikler**⁶ sağlar. | LTV/Kayıp tahmininin ilk ML modellerine (Faz I) hemen beslenebilecek yüksek kaliteli girdiler. |

***

### Segment Odaklı Avantajlar ve Yan Gelir Optimizasyonu Tablosu

| Müşteri Segmenti | Birincil Motivasyon/Özellik | Yüksek ROI'li Avantaj Stratejisi | Çapraz Satış / Yan Gelir Odağı |
| :--- | :--- | :--- | :--- |
| **Çocuklu Aileler** | Konfor, lojistik kolaylık, maliyet hassasiyeti ⁴ | **Ücretsiz Kayıtlı Bagaj, Ücretsiz Otel Yemeği** ⁴ | Premium koltuk, seyahat sigortası (aile paketleri), transfer hizmetleri. |
| **Gençler (15-35)** | Fiyat hassasiyeti, bütçe seyahati ⁴ | **Özel İndirimler**, Flaş Satışlar | Bütçe dostu aktiviteler, sırt çantalı gezgin sigortası, hostel yükseltmeleri. |
| **Yaşlılar (55+)** | Deneyim Geliştirme, konfor ⁴ | **Her Şey Dahil paketler**, Konsiyerj Hizmeti | Lüks turlar, premium dinlenme salonları, uzmanlaşmış tıbbi seyahat sigortası. |

***

## 🤖 Bölüm IV: Öngörücü Tutundurma ve LTV Maksimizasyonu İçin Makine Öğrenimini Kullanma

### 4.1 YZ Merkezi Rolü: Öngörücü Çerçevenin Uygulanması 🧠

YZ Merkezi Rolü, bir güzergâh oluşturma aracından **öngörücü analiz merkezine** dönüşmelidir. Amaç, müşteri davranışını tahmin ederek kaynak tahsisini optimize etmek ve pazar duyarlılığını artırmaktır.⁵

### 4.2 Müşteri Yaşam Boyu Değeri (LTV) Tahmin Modelini Geliştirme 💰

LTV skoru, hedefli fiyatlandırma, pazarlama bütçesi tahsisi ve müşteri desteği önceliklendirmesi için temel metriktir.³

| Bileşen | Açıklama | Önerilen Yaklaşım |
| :--- | :--- | :--- |
| **Özellik Mühendisliği** | Yerleşik finansal metrikleri (parasal değer, satın alma sıklığı, müşteri ömrü) ve spesifik davranışsal özellikleri (`cancellation_rate`, `total_trips`, detaylı harcama kalıpları) birleştirmelidir.³ | **Model Seçimi** Topluluk yöntemlerini (Ensemble Methods) desteklemelidir. XGBoost veya birleşik ML boru hatları, hassas değer katmanları atamak için endüstri standardıdır.³ |

### 4.3 Kayıp (Churn) Tahmin Çerçevesi 🛑

Kayıp Tahmin Modeli, tanımlanmış bir zaman ufkunda (örn. 90 gün) yüksek riskli müşterileri belirlemek ve proaktif tutundurma stratejilerini tetiklemek için esastır.

| Strateji | Mekanizma | Müdahale Tetikleyicisi |
| :--- | :--- | :--- |
| **Önerilen Model** | **Gradient Boosting Machines (GBM)**, yüksek riskli tahmin görevi için şiddetle tavsiye edilir.⁸ | Kayıp skoru, **Müdahale Tetikleyicilerini** tanımlamalıdır: yüksek riskli, yüksek LTV'li müşteriler, doğrudan konsiyerj erişimi veya ısmarlama **Özel İndirimler** gibi anlık, yüksek değerli tutundurma çabalarını tetiklemelidir.⁴ |
| **GBM Avantajı** | Satın alma geçmişi ve gerçek zamanlı **tıklama akışı** verileri gibi dağınık özelliklerin entegrasyonu yoluyla karmaşık, doğrusal olmayan ilişkileri yakalamada üstündür.⁸ | Yüksek değerli müşterileri hassas bir şekilde hedefleme ihtiyacı nedeniyle hesaplama yoğunluğunu haklı çıkarır. |

### 4.4 Tutundurma ROI Optimizasyonu ve Hedefli Fiyatlandırma 🎯

Birleşik LTV ve Kayıp skorları, bütçe yeniden tahsisini yönlendiren sürekli bir optimizasyon döngüsü sağlar.³

| Uygulama | Mekanizma | Finansal Sonuç |
| :--- | :--- | :--- |
| **ROI Optimizasyonu** | Daha önce genel marka bilinirliğine yönlendirilen pazarlama harcaması, orta-yüksek kayıp riski taşıyan yüksek LTV'li müşterileri güvence altına almak için stratejik olarak yeniden yönlendirilir.³ | Promosyon harcamasının yalnızca öngörülen tutundurma faydasının daha büyük gelecekteki kârlılığı (Yüksek Komisyon Dengesi) sağladığı zaman yapılmasını sağlar. |
| **Hedefli Fiyatlandırma** | Dinamik fiyatlandırma motoru, bir rezervasyonu güvence altına almak ve müşteriyi tutundurmak için gereken **minimum indirimi** belirlemek için ML çıktısını kullanır. | Promosyon harcamalarının verimliliğini maksimize eder. |

***

### 💻 TravelTide Önerilen ML Model Dağıtımı ve Veri Gereksinimleri

| ML Hedefi 🎯 | Önerilen Model | Gerekli Özellikler (Özellik Mühendisliği) | İş Uygulaması |
| :--- | :--- | :--- | :--- |
| **Müşteri Yaşam Boyu Değeri (LTV) Tahmini** | XGBoost, Birleşik Boru Hattı ³ | Parasal Değer, Sıklık, Ömür, Toplam Otel Harcaması ⁶, Demografik Segment.⁴ | Destek ve özel erişim için yüksek değerli müşterilerin önceliklendirilmesi. |
| **Kayıp Riski Tahmini** 📉 | Gradient Boosting Machines (GBM) ⁸ | İptal Oranı ⁶, Oturum Sıklığı, Avantaj Kullanımı, **Tıklama Akışı Niyet Verisi**. | Anlık, yüksek maliyetli tutundurma müdahalelerini (örn. hedefli indirimler) tetikleme. |
| **Hiper-Kişiselleştirme Motoru** ✨ | Derin Öğrenme Tavsiye Sistemleri | Tıklama Akışı Verisi, Geçmiş Güzergâh Tercihleri, Gerçek Zamanlı Arama Sorguları. | **Dinamik Paketleme** ve optimize edilmiş **çapraz satış potansiyeli**. |

***

## 🌐 Bölüm V: Hiper-Kişiselleştirme ve Deneyim Geliştirme Çerçevesi

### 5.1 Tavsiye Sistemleri aracılığıyla YZ Destekli Hiper-Kişiselleştirme ✨

TravelTide, bireysel düzeyde **YZ Destekli Hiper-Kişiselleştirme** sağlamak için özel bir **Derin Öğrenme Tavsiye Sistemi (DRS)** uygulamalıdır.

| Mekanizma | Fonksiyon | Örnek | Sonuç |
| :--- | :--- | :--- | :--- |
| **DRS Öğrenimi** | Gerçek zamanlı **Tıklama Akışı Verisi** ve geçmiş rezervasyon sonuçlarını analiz ederek yüksek derecede spesifik, ayrıntılı tercihleri öğrenir. | Yüksek LTV'li gezginin (örn. 55+ yaş) premium hizmetleri sürekli olarak güzergahlara dahil ettiğini tespit etmek. | **Proaktif önsezi**, müşteriye derinlemesine anlama gösterir ve anlamlı **Deneyim Geliştirme** sağlar. |

### 5.2 Sadakat Programlarının Evrimi 🏆

Sadakat, işlemsel puan sisteminden uzaklaşarak **öngörücü değere** dayalı bir sisteme geçmelidir.

| Yaklaşım | Tahsis Temeli | Hedef ve Fayda |
| :--- | :--- | :--- |
| **LTV Katmanlı Sadakat** | Katmanlar ve ödül tahsisleri, müşterinin **öngörülen gelecek değerine (LTV skoru)** göre belirlenir. | **En yüksek LTV'li müşteriler**, yakın zamandaki rezervasyon geçmişine bakılmaksızın en değerli, **deneyimsel ödülleri** (özel kişisel hizmet yöneticileri, esnek iptal feragatleri) almalıdır. |
| **Stratejik Hedef** | En yüksek değerli kohortun tutundurulmasına öncelik vererek platformun gelecekteki kârlılığını güvence altına alır. |

### 5.3 Hizmet Genişletme ve Çapraz Satış Potansiyeli (İleri Düzey) ➕

YZ sisteminin öngörücü kapasitesi, seyahat yolculuğundaki potansiyel hizmet boşluklarını belirlemek ve **Yan Gelir** yakalamayı maksimize etmek için kullanılmalıdır.

| Strateji | Tetikleyici Mekanizma | Eylem/Öneri | Fayda |
| :--- | :--- | :--- | :--- |
| **İleri Düzey Çapraz Satış** | Rezervasyonun **karmaşıklığını** (örn. karmaşık uluslararası güzergâh) tetikleyici olarak kullanır. | Özel seyahat sigortası, yerel kara taşımacılığı veya gerekli vize işlemleri için proaktif, **YZ destekli uyarılar** ve öneriler sağlar. | Müşteri bağlılığını artırır ve **Yan Gelir** yakalamayı maksimize eder. |

### 5.4 Rezervasyon Sonrası Deneyim Geliştirme 📞

Tutundurma, rezervasyon sonrası ve seyahat içi aşamalarda güvence altına alınır veya kaybedilir.

| Girişim | Mekanizma | Stratejik Amaç | Tutundurma Kolu |
| :--- | :--- | :--- | :--- |
| **Akıllı Müşteri Hizmetleri** | Rutin sorguları çözmek için YZ kullanımı. | Operasyonel verimliliği artırmak ve maliyetleri düşürmek.⁵ | |
| **Müşteri Desteği Önceliklendirmesi** | Uzmanlaşmış insan destek kaynakları, stratejik olarak **yüksek LTV'li müşteriler** ve **yüksek riskli** müşteriler için ayrılır.³ | Platformun en değerli kullanıcılarının **özel, hızlandırılmış destek** almasını sağlar; güçlü, parasal olmayan bir tutundurma kaldıracıdır. |

***

## 🛣️ Bölüm VI: Stratejik Yol Haritası ve YZ Destekli Hizmetlerin Uygulanması

### 6.1 Aşamalı Veri Altyapısı Gereksinimleri 💾

| Aşama | Zaman Çizelgesi | Temel Amaç | Kritik Çıktı |
| :--- | :--- | :--- | :--- |
| **Faz I: Veri Merkezileştirme** | 0–6 Ay | SQL ile çıkarılmış işlemsel verileri ve ilk **Tıklama Akışı** veri akışlarını entegre etmek için sağlam bir veri ortamı oluşturmak. | İlk LTV ve Kayıp modelleri için **özellik tanım kümesini** resmileştirmek.⁶ |
| **Faz II: Öngörücü Model Geliştirme ve Dağıtım** | 6–18 Ay | Basitleştirilmiş LTV ve Kayıp tahmin modellerini (örn. XGBoost) geliştirmek ve dağıtmak. | **Merkezileştirilmiş Özellik Mağazası (Feature Store)** kurmak; gerçek zamanlı fiyatlandırma müdahaleleri için gerekli olan yüksek model doğruluğunu ve ölçeklenebilirliği sağlamak. |

### 6.2 Kurumsal Uyum ve Veri Yönetişimi 🧑‍💻

| Alan | Gereklilik | Gerekçe / Risk Azaltma |
| :--- | :--- | :--- |
| **Yetenek Kazanımı** | Topluluk yöntemlerinde (GBM/XGBoost) uzmanlaşmış **Veri Bilimcileri**⁸ ve modelleri üretim ortamında konuşlandıracak **ML Mühendisleri'nin** öncelikli olarak işe alınması. | Karmaşık, öngörücü analitik sistemleri dağıtmak için zorunludur. |
| **Veri Yönetişimi** | YZ'nin fiyatlandırma ve tavsiye sistemlerindeki kullanımı konusunda şeffaflık ve veri gizliliğini sağlamak için açık protokoller oluşturmak. | **Algoritmik önyargı** ve kullanıcı güveni eksikliği gibi endüstriyel zorlukları önleyerek kullanıcı sadakatini teşvik etmek.⁵ |

### 6.3 Temel Performans Göstergeleri (KPI) ve Ölçüm Çerçevesi 📈

Bu veri odaklı stratejinin başarısı, gelecekteki kârlılık ve öngörücü doğruluk üzerindeki etkisiyle ölçülecektir.

| Çekirdek Ölçüm Metriği | Tanım | Stratejik Amaç |
| :--- | :--- | :--- |
| **LTV/CAC Oranı** | Birincil kârlılık metriği. | Tutundurma yatırımlarının müşteri edinme maliyetlerine göre verimliliğini doğrulamak. |
| **Öngörülen Kayıp Oranı Doğruluğu** | Öngörücü modellerin doğrulanması: tahmin edilen kayıp ile gerçekleşen müşteri kaybının karşılaştırılması. | **Hassas kaynak hedeflemesini** ve yüksek model güvenilirliğini sağlamak. |
| **Müdahale ROI'si** | Tutundurma müdahalelerinin maliyetine karşılık sağlanan finansal getiriyi (güvence altına alınan gelecek LTV) izlemek. | Bütçe yeniden tahsisinin verimliliğini teyit etmek. |
