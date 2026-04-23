# YZM304 Derin Öğrenme Projesi: Evrişimli Sinir Ağları ile Özellik Çıkarımı ve Sınıflandırma

Ankara Üniversitesi Yapay Zeka ve Veri Mühendisliği Bölümü 2025-2026 Bahar Dönemi YZM304 Derin Öğrenme dersi kapsamında hazırlanan bu proje, evrişimli sinir ağları (CNN) mimarilerinin görüntü sınıflandırma performansını ve hibrit modelleme yaklaşımlarını incelemektedir.

## 1. Giriş (Introduction)
Bu çalışmanın temel amacı, CNN modelleri kullanarak benchmark veri setleri üzerinde özellik çıkarma ve sınıflandırma işlemlerini gerçekleştirmektir. Proje; temel CNN yapılarından, literatürde kabul görmüş karmaşık mimarilere ve derin öğrenme ile klasik makine öğrenmesini birleştiren hibrit sistemlere kadar geniş bir yelpazeyi kapsamaktadır.

### Veri Seti
Çalışmada benchmark veri seti olarak **CIFAR-10** kullanılmıştır. 
* **İçerik:** 10 farklı sınıfa (uçak, otomobil, kuş, kedi vb.) ait 32x32 boyutlarında renkli (RGB) görüntülerden oluşmaktadır.
* **Ön İşleme:** Görüntüler eğitim öncesinde normalize edilmiş ve literatür mimarilerine uyum sağlaması amacıyla 224x224 boyutlarına yeniden boyutlandırılmıştır.

## 2. Yöntem (Method)
Çalışma kapsamında beş farklı modelleme yaklaşımı geliştirilmiştir:

### Model Mimarileri
* **Model 1 (Temel CNN):** LeNet-5 mimarisine benzer yapıda; evrişimli (Conv2d), havuzlama (MaxPool) ve tam bağlantılı (Linear) katmanlardan oluşan özgün bir sınıftır.
* **Model 2 (İyileştirilmiş CNN):** Model 1 ile aynı hiperparametrelere sahip olup, ağı iyileştirmek amacıyla **Batch Normalization** ve **Dropout** (0.3) katmanları eklenmiştir.
* **Model 3 (Literatür Mimarisi):** PyTorch kütüphanesinden önceden eğitilmiş (**pretrained=True**) **AlexNet** mimarisi kullanılarak transfer öğrenme (transfer learning) uygulanmıştır.
* **Model 4 (Hibrit Yapı):** AlexNet mimarisinin özellik çıkarımı mekanizması kullanılarak görüntülerden özellik vektörleri elde edilmiş; bu verilerle kanonik bir makine öğrenmesi modeli olan **Destek Vektör Makinesi (SVM)** eğitilmiştir.
* **Model 5:** Tam bir CNN mimarisi ile hibrit modelin performansı aynı veri setleri üzerinde karşılaştırılmıştır.

### Hiperparametreler ve Eğitim
* **Kayıp Fonksiyonu:** Eğitim aşamasında **Cross Entropy Loss** (`nn.CrossEntropyLoss()`) kullanılmıştır.
* **Optimizer:** Gradyan tabanlı optimizasyon için yüksek verimlilik sunan **Adam** optimizer tercih edilmiştir.
* **Öğrenme Oranı (Learning Rate):** Dengeli bir yakınsama sağlamak amacıyla 0.001 olarak belirlenmiştir.

## 3. Sonuçlar (Results)
Eğitim sürecinden elde edilen temel bulgular şunlardır:

* **Eğitim Kaybı (Loss):** Modellerin öğrenme süreçlerine ait kayıp grafikleri `loss_plot_fixed.png` dosyasında sunulmuştur. AlexNet tabanlı transfer öğrenme modelinin en düşük kayıp değerlerine en hızlı sürede ulaştığı görülmüştür.
* **Hibrit Veri Setleri:** Model 4 için oluşturulan özellik ve etiket setleri `.npy` uzantılı dosyalarda saklanmıştır.
    * **features.npy:** Çıkarılan özellik vektörlerini içerir.
    * **labels.npy:** Özelliklere karşılık gelen etiketleri içerir.
    * Bu veri setlerinin boyut ve uzunlukları projede açıkça yazdırılarak doğrulanmıştır.

## 4. Tartışma (Discussion)
Yapılan karşılaştırmalar sonucunda elde edilen yorumlar:
* **Özel Katmanların Etkisi:** Batch Normalization ve Dropout katmanlarının eklenmesi (Model 2), Model 1'e kıyasla aşırı öğrenmeyi (overfitting) azaltmış ve test setinde daha kararlı sonuçlar vermiştir.
* **Transfer Öğrenme:** Önceden eğitilmiş literatür mimarilerinin kullanımı, sınırlı veri setlerinde bile çok daha yüksek doğruluk oranlarına ulaşılmasını sağlamıştır.
* **Hibrit Model Verimliliği:** CNN tabanlı özellik çıkarımı ile klasik makine öğrenmesi modellerinin (SVM) birleştirilmesi, derin öğrenmenin karmaşık özellik çıkarma yeteneğini klasik algoritmaların kararlılığı ile başarılı bir şekilde harmanlamıştır.

## Referanslar (References)
1. Ankara Üniversitesi Yapay Zeka ve Veri Mühendisliği, YZM304 Derin Öğrenme Ders Notları.
2. PyTorch Torchvision Models Documentation: [torchvision.models](https://pytorch.org/vision/0.9/models.html).
3. CIFAR-10 Dataset.
