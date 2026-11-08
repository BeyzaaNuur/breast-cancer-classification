# 🩺 Meme Kanseri Sınıflandırması (Breast Cancer Classification)

Bu proje, meme kanseri biyopsi verilerini kullanarak tümörlerin **İyi Huylu (Benign)** veya **Kötü Huylu (Malignant)** olduğunu tahmin eden bir makine öğrenmesi çalışmasıdır.

## 📌 Proje Kapsamı ve Yöntemler
- **Veri Ön İşleme:**
  - Analize katkısı olmayan `id` ve tamamen boş `Unnamed: 32` sütunları temizlendi.
  - Hedef değişken (`diagnosis`) ikili formata dönüştürüldü ($M \rightarrow 1, B \rightarrow 0$).
  - Veri seti, sınıf dengesini korumak için `stratify=y` parametresiyle **%70 Eğitim (Train)**, **%15 Doğrulama (Validation)** ve **%15 Test** olarak 3 parçaya ayrıldı.
  - Veri sızıntısını (Data Leakage) önlemek amacıyla `MinMaxScaler` yalnızca eğitim verisiyle eğitildi (`fit_transform`), doğrulama ve test kümelerine ise sadece `transform` uygulandı.

- **Kullanılan Algoritmalar:**
  1. **k-En Yakın Komşu (k-NN)** (Ölçeklenmiş veri ile)
  2. **Karar Ağacı (Decision Tree)** (`max_depth=3`)
  3. **Rastgele Orman (Random Forest)** (`n_estimators=100`)

---

## 📊 Model Sonuçları ve Kritik Muhakeme

Tıbbi teşhis senaryolarında en maliyetli ve tehlikeli hata türü **Yanlış Negatif (False Negative - FN)** hatasıdır (kanserli hastaya sağlıklı denmesi). Bu nedenle model seçiminde **Duyarlılık (Recall)** metriği birincil önceliktir.

- **Aşırı Öğrenme (Overfitting) Şüphesi:** k-NN ve Karar Ağacı modellerinin doğrulama kümesinde gösterdiği %100 performans, veri hacminin sınırlı olmasından kaynaklanabilir. Modellerin genelleme yeteneğini doğrulamak için tarafsız test seti sonuçları esastır.
- **Sağlık Odaklı Model Seçimi:** Rastgele Orman modeli 4 kanser vakasını gözden kaçırdığı için ($Recall = \%87.5$), mevcut doğrulama sonuçlarında kanserli hastaları kaçırmayan k-NN ve Karar Ağacı tıbbi açıdan daha güvenli bir profil çizmiştir.
