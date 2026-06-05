# Football-Player-Market-Value-Prediction-Machine-Learning
# Makine Öğrenmesi İle Futbolcu Piyasa Değeri Tahminleme

Bu proje, **Maçkolik (mackolik.com)** üzerinden web kazıma (web scraping) yöntemleriyle derlenen özgün bir veri seti kullanılarak, futbolcuların güncel transfer piyasa değerlerini tahmin etmek amacıyla geliştirilmiş bir Makine Öğrenmesi (Machine Learning) projesidir.Dataset' Github üzerindeki Nazmi Cirim adlı kullanıcının yaptığı bir çalışmada kullandığı dataset üzerinden erişilmiştir.

---

##  Proje ve Kullanılan Dataset

Bu projenin amacı, Maçkolik sitesinden çekilen gerçek futbolcu verilerini kullanarak oyuncuların transfer piyasasındaki değerlerini tahmin eden bir makine öğrenmesi modeli geliştirmektir. Projede, futbol pazarındaki karmaşık ve tahmin etmesi zor olan fiyat dinamiklerini XGBoost ve Gradient Boosting gibi güçlü algoritmalarla modelleyerek, oyuncu performanslarına göre en az hatayla çalışan bir tahmin sistemi kurmak hedeflenmiştir

* **Veri Kaynağı:** Maçkolik üzerinden çekilen güncel oyuncu veri tabanı (`/DataSet` klasörü altında yer almaktadır).
* **Hedef Değişken (Target):** `Piyasa Değeri` (Euro - €). Transfer piyasasındaki süperstarların yarattığı yüksek varyansı dengelemek amacıyla hedef değişkene **logaritmik dönüşüm** uygulanmıştır.
* **Dataset İçindeki Öznitelikler(Features)**:Datasetimdeki kolonlar oyuncunun kimliği ve temel verileri(adı, milliyeti, yaşı, oynadığı pozisyon, takımı ,doğum tarihi ) bilgilerinin  yanı sıra oyuncunun istatistiklerini içeren (maç, gol, asist, maç başına isabetli/isabetsiz şut, dribbling  oranı … )gibi teknik verileri ve son olarak hedef değişken “Piyasa Değeri (€)” olan sütununu içeriyor.

---

## 🛠️ Veri Ön İşleme Adımları
 * **Sütun İsimlerini Anlaşılabilir Hale Getirme**:Anlaşılması güç olan "T.K/M" gibi sütun isimleri "Maç Başına Top Kazanma" gibi daha anlaşılabilir bir hale getirildi.

* **Modeli Eğitmemize Bir Katkısı Olmayan Gereksiz Olan 2 Sütun Silme**: "Oyuncu Adı" ve "Doğum Tarihi" sütunları dataframe'imizden silindi.

* **Hedef Değişkenin Olduğu Sütun İle İlgili Adımlar**:Hedef değişkendeki(Piyasa Değeri) sütunu null olan satırlar silinip, modelimizin eğitiminde gerekli olduğu için gereksiz nokta işaretleri kaldırıldı ve integer veri tipine dönüşüm sağlandı.  

* **Hedef Değişken Harici(Bağımlı Değişkenler)Sütunlar Ön İşleme Sürecine Alınması**:Dataset oluşum sürecinde olan sütun kaymasından kaynaklanan hatalı satırları temizleme işlemi yapıldı ayrıca ilgili sütundaki gereksiz noktalama işaretleri kaldırılıp,uygun veri tipine dönüşüm sağlanıp NaN olan satırlar uygun değerlerce dolduruldu.

 
* **Hedef Tabanlı Kodlama (Target Encoding) Ve One-Hot Kodlama:** `Milliyeti` ve `Oynadığı Takım` gibi yüksek kardinaliteye sahip metinsel kategorik veriler, veri sızıntısını (data leakage) önlemek adına **`LeaveOneOutEncoder`** mimarisi ve `sigma=0.05` oranında rastgele gürültü (noise) eklenerek sayısal forma dönüştürüldü.`Pozisyon`verisi ise one-hot encoding yöntemi ile encode edildi.
 
---
## 🎨Veri Görselleştirme
  Veriyi daha yakından tanımak amacıyla ve modelleri eğitme aşamasına geçmeden aşağıdaki gibi gerekli grafikler çizdirildi.
  * **Öznitelikler Arasındaki Korelasyonu Gösteren Heat-Map**
  * **Futbolcuların Yaş,Oynadığı Pozisyon ve Piyasa Değeri Dağılımı**
  * **En Çok Oyuncusu Olan 30 Takım ve Ülke**
  * **Özniteliklerin Hedef Değişkenle İlişkisini Gösteren Saçılım Grafikleri(Scatter Plot)** 
  ---

## ⚙️ Veri Bölme ve Modellerin Eğitilmesi
Datasetimiz bu aşamada %80 Eğitim(Train) ve %20 Test olacak şekilde bölünmüştür.Ayrıca random-state=36 olarak belirlenmiştir.Bu aşamada hedef değişkene logaritmik dönüşüm uygulanmıştır.
Projede model eğitimi için kullanılan algoritmalar:
* **Multiple Linear Regression**
* **Random Forest Regression**
* **Gradient Boosting Regression**
* **XGBoost**
---
## 🎛️ Gradient Boosting İçin Hiperparametre Optimizasyonu
Eğitilen en iyi 2 modelden biri olan  Gradient Boosting modeli üzerinde GridSearchCV algoritması kullanılarak hiperparametre ayarlaması ile modelin başarısı arttırılmıştır.

---

## 📈 Modellerin Tahmin Performanslarının Değerlendirilmesi ve Yorum
Raporun son aşamasında, geliştirilen modellerin gerçek dünya verileri üzerindeki tahmin yeteneğini görselleştirmek ve analiz etmek amacıyla iki kritik grafik çizdirilmiştir:
* Gerçek Değerler -Tahmin Edilen Değerler Grafiği
* Piyasa Değerini En Çok Etkileyen En Önemli 10 Öznitelik(Feature)
---

## 🎯 Problemi Çözen Modeller ve Başarı Metrikleri

Modellerin başarısı varyans açıklama gücü (**$R^2$ Skoru**), ortalama sapma (**MAE**) ve büyük hataları cezalandıran **RMSE** metrikleri üzerinden doğrulanmıştır:

| Performans Ölçütleri | Optimize Edilmiş Gradient Boosting | XGBoost (Extreme Gradient Boosting) |
| :--- | :---: | :---: |
| **R² Skor (Başarı Oranı)** | **0,7191** | 0,7127 |
| **Mean Absolute Error (MAE)** | 5.928.250 € *(5,93 Milyon €)* | **5.738.599 € *(5,74 Milyon €)*** |
| **Root Mean Square Error (RMSE)** | 11.185.085 € *(11,19 Milyon €)* | **10.573.884 € *(10,57 Milyon €)*** |
| **Adjusted R² Skor** | **0,7068** | 0,7002 |
| **Logaritmik Dönüşümsüz R² Skor** | 0,6171 | **0,6578** |
| **Ortalama Cross Validation R² Skoru** | **0,6567** | 0,6488 |
| **Cross Validation Standart Sapması** | 0,0354 | **0,0326** |

### 💡Yorumlama:
* **Gradient Boosting**, veri setindeki genel piyasa varyansını açıklama konusunda **%71,91** ile en yüksek kararlılığı sunan model olmuştur.
* **XGBoost**, dahili regülasyon (L1/L2) mekanizmaları sayesinde transfer piyasasındaki "süperstarların" yarattığı uç değer (outlier) gürültülerini daha iyi baskılamış ve oyuncu başına ortalama mutlak hatayı (**5.738.599 €**) en düşük seviyeye indiren model olmuştur.
* **5-Fold Cross-Validation (Çapraz Doğrulama)** analizlerinde standart sapmanın `0.03` bandında kalması, modellerin **Overfitting (Ezberleme)** yapmadığını  kanıtlar niteliktedir.

---
