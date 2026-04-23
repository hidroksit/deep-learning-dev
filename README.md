# YZM304 Derin Öğrenme Proje Raporu: Evrişimli Sinir Ağları (CNN) ile Görüntü Sınıflandırma

Bu rapor, Ankara Üniversitesi Yapay Zeka ve Veri Mühendisliği Bölümü 2025-2026 Bahar Dönemi YZM304 Derin Öğrenme dersi I. Proje Modülü II kapsamında hazırlanmıştır.

## 1. Giriş (Introduction)
Bu çalışmanın temel amacı, evrişimli sinir ağları (CNN) kullanarak özellik çıkarma ve sınıflandırma süreçlerini analiz etmektir. Projede, basit mimarilerden derin literatür modellerine ve klasik makine öğrenmesi algoritmalarıyla birleştirilmiş hibrit yapılara kadar geniş bir yelpaze test edilmiştir.

### Veri Seti: CIFAR-10
* **İçerik:** 10 farklı sınıfa ait 32x32 boyutlarında RGB görüntüler.
* **Ön İşleme:** Veriler eğitim öncesinde normalize edilmiş ve literatür mimarilerine uyum sağlaması amacıyla 224x224 boyutlarına yeniden boyutlandırılmıştır.

## 2. Yöntem (Method)
Çalışmada beş farklı modelleme yaklaşımı geliştirilmiştir:

* **Model 1 (Temel CNN):** LeNet-5 mimarisine benzer şekilde; evrişimli (Conv2d), havuzlama (MaxPool), aktivasyon ve tam bağlantılı (Linear) katmanlardan oluşan özgün bir sınıftır.
* **Model 2 (İyileştirilmiş CNN):** Model 1 ile aynı hiperparametrelere sahip olup, ağı iyileştirmek amacıyla Batch Normalization ve Dropout (0.3) katmanları eklenmiştir.
* **Model 3 (Literatür Mimarisi):** PyTorch `torchvision.models` modülünden önceden eğitilmiş (pretrained=True) **AlexNet** mimarisi kullanılarak transfer öğrenme uygulanmıştır.
* **Model 4 (Hibrit Yapı):** AlexNet mimarisinin özellik çıkarımı mekanizması kullanılarak görüntülerden özellik vektörleri elde edilmiş ve bu verilerle bir **Destek Vektör Makinesi (SVM)** eğitilmiştir.
* **Model 5:** Tam bir CNN mimarisi ile hibrit modelin performansı aynı veri setleri üzerinde karşılaştırılmıştır.

### Hiperparametreler
* **Kayıp Fonksiyonu:** `nn.CrossEntropyLoss()`.
* **Optimizer:** Adam Optimizer (Dengeli yakınsama ve adaptif öğrenme oranı için).
* **Öğrenme Oranı:** 0.001.

## 3. Sonuçlar (Results)

### 3.1. Eğitim Kayıplarının Karşılaştırılması
Eğitim sürecinde elde edilen kayıp (loss) değerlerinin modellere göre değişimi aşağıdaki grafikte sunulmuştur:

![Modellerin Eğitim Kayıp Karşılaştırması](loss_plot.png)

* **Model 1:** En düşük kayıp değerine en istikrarlı şekilde ulaşan model olmuştur.
* **Model 2:** Batch Normalization etkisiyle hızla düşüş göstermiştir.
* **Model 3:** Önceden eğitilmiş ağırlıklar sayesinde belirli bir seviyede stabilize olmuştur.

<img width="2546" height="1653" alt="loss_plot" src="https://github.com/user-attachments/assets/316da8c3-5e28-4189-b23a-63a78ce17385" />


<img width="1000" height="600" alt="loss_final" src="https://github.com/user-attachments/assets/22bf3ef4-efef-4871-bfd0-82718a7d03a6" />

### 3.2. Hibrit Model Veri Yapısı
Model 4 için özellik çıkarımı aşamasında oluşturulan dosyaların boyutları doğrulanmıştır:
* **features.npy:** [50000, 4096] (Örnek sayısı x Özellik vektörü)
* **labels.npy:** [50000] (Etiket seti)

## 4. Tartışma (Discussion)
* **Özel Katmanların Etkisi:** Batch Normalization ve Dropout katmanlarının eklenmesi, Model 1'e kıyasla aşırı öğrenmeyi (overfitting) azaltmış ve test setinde daha güvenilir sonuçlar verilmesini sağlamıştır.
* **Hibrit Model Verimliliği:** CNN tabanlı özellik çıkarımı ile klasik makine öğrenmesi modellerinin birleştirilmesi başarılı sonuçlar üretmiştir.
* **Transfer Öğrenme:** Hazır mimarilerin kullanımı, özellikle karmaşık sınıfların ayırt edilmesinde manuel modellerden daha yüksek performans sergilemiştir.

## 5. Referanslar (References)
1. Ankara Üniversitesi Yapay Zeka ve Veri Mühendisliği, YZM304 Derin Öğrenme Ders Notları.
2. PyTorch Documentation (torchvision.models).
3. CIFAR-10 Dataset Resource.
