# DataForge-agri-yield-prediction

Bu proje, çevresel, coğrafi ve zamansal faktörleri kullanarak farklı tarım ürünlerinin üretim miktarlarını (rekoltelerini) tahmin etmeyi amaçlayan bir Makine Öğrenmesi (Regression) modelleme çalışmasıdır.

## 🎯 Projenin Amacı
Veri setinde yer alan farklı şehirler, yıllar, mevsimler ve arazi büyüklükleri gibi değişkenleri kullanarak, hasat edilecek ürün miktarını en düşük **Ortalama Mutlak Hata (MAE)** ile tahmin etmek hedeflenmiştir.

## 🛠️ Kullanılan Teknolojiler
* **Dil:** Python
* **Veri İşleme:** Pandas, NumPy
* **Modelleme:** CatBoost, LightGBM, Scikit-Learn
* **Görselleştirme:** Matplotlib, Seaborn

## 🧠 Geliştirme Süreci ve Stratejiler

### 1. Veri Ön İşleme ve Varyans Yönetimi (Handling Variance)
* Veri setindeki "Ton" ve "Adet" gibi farklı üretim birimlerinin yarattığı devasa varyansı yönetmek için hedef değişkene öncelikle **Logaritmik Dönüşüm (log1p)** uygulandı.
* MAE metrik optimizasyonuna geçildiğinde ise "Gruba Özel Tıraşlama" (**Group-wise Clipping**) yöntemiyle her ürünün uç değerleri kendi içinde (99th percentile) sınırlandırıldı.

### 2. Özellik Mühendisliği (Feature Engineering)
Modelin tahmin gücünü artırmak için ham veriden şu yeni değişkenler türetildi:
* **Target Encoding:** Ürün (Crop) ve Şehir (State) bazında geçmiş ortalama üretim miktarları modele matematiksel olarak öğretildi.
* **Beklenen Üretim (Expected Yield):** Hektar başına düşen ortalama verim hesaplanarak, test setindeki arazilerin büyüklükleriyle çarpıldı ve sentetik bir "Beklenen Üretim" skoru oluşturuldu.
* **Etkileşim Değişkenleri:** Birim farklılıklarını ayırmak adına `Crop_x_Unit` (Örn: Fındık_Adet) kombinasyonları yaratıldı.

### 3. Modelleme ve Çapraz Doğrulama
* Modeller, aşırı öğrenmeyi (overfitting) engellemek ve genellenebilirliği artırmak adına **5-Fold Cross Validation** stratejisiyle eğitildi.
* Tek bir algoritmaya bağlı kalmamak için **CatBoost** (simetrik ağaçlar) ve **LightGBM** (yaprak odaklı ağaçlar) modelleri aynı anda eğitilerek tahminleri **Ensemble (Güç Birleşimi)** yöntemiyle birleştirildi.

## 🚀 Sonuçlar
* **Değerlendirme Metriği:** Gerçek Ölçekte MAE (Mean Absolute Error)
* Optimizasyon süreçleri sonucunda modelin private MAE skoru 794,073.95 seviyesine indirilmiştir.

## 📂 Dosya Yapısı
* `dataforge_kodu.ipynb`: EDA, Feature Engineering ve Modelleme adımlarını içeren Colab kodu.(kodun başındaki kısımlar dosyaların drive'dan çekilme aşamalarıdır.)
* veri setlerini burda paylaşmadım isteyen bana ulaşabilir.
