🌱 Akıllı Sulama Tahmin Sistemi

Toprak ve iklim verilerinden yola çıkarak bir bitkinin sulanmaya ihtiyacı olup olmadığını tahmin eden, uçtan uca bir makine öğrenmesi projesi (veri temizleme → keşifçi analiz → model eğitimi → değerlendirme → etkileşimli tahmin aracı).

Sonuç: DecisionTreeClassifier ile %71,8 test doğruluğu, aşırı öğrenme yok (eğitim %72,3 vs test %71,8).

Genel Bakış

Bu proje, toprak nemi, sıcaklık ve toprak nem oranı verilerinden yola çıkarak bir bitkinin sulanmaya ihtiyacı olup olmadığını (Evet/Hayır) tahmin eden bir sınıflandırma modeli geliştirir. Ham veriden başlayıp çalışan bir tahmin aracına kadar uzanan uçtan uca bir makine öğrenmesi sürecini kapsar.

Veri Seti
Kaynak: Kaggle — "Plants" veri seti
Boyut: 100.000 satır
Kullanılan özellikler: Soil Moisture, Temperature, Soil Humidity
Hedef değişken: sulama_gerekli (orijinal Status ON/OFF pompa sütunundan türetildi) → Evet / Hayır
Yoğun eksik veri içeren sütunlar (hava nemi, rüzgâr, pH, yağış, N/P/K) çıkarıldı; sütun adlarındaki gizli boşluklar temizlendi.
Keşifçi Veri Analizi
En belirleyici özellik toprak nemi: sınıflar arasında ortalama ~17 puanlık fark var (%37,7 vs %54,5).
Sıcaklık da anlamlı: 26,2°C (sulama gerekli) vs 18,2°C (gerekli değil).
Toprak nem oranı sınıflar arasında neredeyse fark göstermiyor, katkısı düşük.
Sınıflar doğrusal olarak ayrılamıyor → karar ağacı modelinin tercih edilmesinin gerekçesi.
Model
Algoritma: DecisionTreeClassifier (scikit-learn), aşırı öğrenmeyi sınırlamak için max_depth=5
Bölünme: 80.000 eğitim / 20.000 test satırı
Sonuçlar
Ölçüt	Eğitim	Test
Doğruluk	%72,3	%71,8

Precision/recall değerleri her iki sınıfta da dengeli (~0,70–0,74); eğitim ve test doğrulukları birbirine yakın — model ezberlemek yerine genelleşebiliyor.

Etkileşimli Araç

tahmin_et() fonksiyonu; toprak nemi, sıcaklık ve toprak nem oranını girdi olarak alır, anında okunabilir bir sulama önerisi döndürür.

Kullanılan Teknolojiler

Python · Pandas · NumPy · Matplotlib · scikit-learn · Google Colab

Nasıl Çalıştırılır
akilli_tarim.ipynb dosyasını Google Colab'da aç.
Hücreleri sırayla baştan sona çalıştır.
Son hücrede kendi değerlerinle tahmin dene.



🌱 Smart Irrigation Prediction

An end-to-end machine learning project that predicts whether a crop needs irrigation using soil and climate data (data cleaning → EDA → model training → evaluation → interactive prediction tool).

Result: 71.8% test accuracy with DecisionTreeClassifier, no overfitting (train 72.3% vs test 71.8%).

Overview

This project builds a binary classifier that predicts whether irrigation is needed (Yes / No) based on three features: soil moisture, temperature, and soil humidity. It follows a full ML workflow, from raw data to a working command-line prediction tool.

Dataset
Source: Kaggle — "Plants" dataset
Size: 100,000 rows
Features used: Soil Moisture, Temperature, Soil Humidity
Target: sulama_gerekli (derived from the original Status ON/OFF pump column) → Yes / No
Columns with heavy missing data (air humidity, wind, pH, rainfall, N/P/K) were dropped; hidden whitespace in column names was cleaned.
Exploratory Data Analysis
Soil moisture is the strongest signal: ~17-point average gap between classes (37.7% vs 54.5%).
Temperature is also informative: 26.2°C (irrigation needed) vs 18.2°C (not needed).
Soil humidity showed almost no difference between classes and contributes little.
Classes are not linearly separable → motivated the choice of a decision tree over a linear model.
Model
Algorithm: DecisionTreeClassifier (scikit-learn), max_depth=5 to limit overfitting
Split: 80,000 train / 20,000 test rows
Results
Metric	Train	Test
Accuracy	72.3%	71.8%

Precision/recall are balanced across both classes (~0.70–0.74), and train/test accuracy are close — indicating the model generalizes rather than memorizing.

Interactive Tool

A tahmin_et() function takes soil moisture, temperature, and soil humidity as input and returns a human-readable irrigation recommendation in real time.

Tech Stack

Python · Pandas · NumPy · Matplotlib · scikit-learn · Google Colab

How to Run
Open akilli_tarim.ipynb in Google Colab.
Run all cells top to bottom.
Use the last cell to try your own inputs.

