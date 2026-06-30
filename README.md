# Fresnel-planck-spektral-analiz
# Fresnel Soğurma & Planck Radyasyon Spektral Analiz Modeli

Opak bir yüzeyden, **geliş açısına** ve **dalgaboyuna** bağlı olarak yayılan
termal radyasyonu hesaplayan bir Python modeli. İki temel fiziksel ilke
birleştirilir:

1. **Fresnel Denklemleri** — yüzeyin s- ve p-polarizasyonları için yansıma
   katsayılarını hesaplar; buradan soğurma (`A = 1 - R`) elde edilir.
2. **Kirchhoff Yasası** — termal dengede `Emissivity = Absorbance`
   olduğunu söyler. Bu sayede Planck kara cisim spektrumu, yüzeyin gerçek
   soğurma katsayısıyla ölçeklenerek **gerçekten yayılan** spektral ışıma
   şiddeti elde edilir.

Sonuç, `(Geliş Açısı x Dalgaboyu)` düzleminde iki ısı haritası (colormap)
olarak görselleştirilir.

![Örnek çıktı](fresnel_planck_analiz.png)

## Fiziksel Model

- **Soğurma:** Her `(θ, λ)` çifti için s- ve p-polarizasyon Fresnel yansıma
  katsayıları hesaplanır, ortalamaları alınır ve `A = 1 - R_ortalama` ile
  soğurma bulunur.
- **Radyasyon:** Planck kara cisim formülü ile her dalgaboyu için spektral
  ışıma şiddeti hesaplanır, ardından Kirchhoff Yasası gereği o dalgaboyu ve
  açıdaki soğurma katsayısıyla çarpılarak gerçek yayılan ışıma elde edilir.
- **Malzeme (n, k):** Bu modelde gümüş/metal benzeri davranışı taklit eden,
  dalgaboyuna doğrusal bağlı **hipotetik** bir kompleks kırılma indisi
  (`n + i·k`) kullanılır. Gerçek bir malzeme için bu fonksiyonun yerine
  ölçülmüş n, k verileri (örn. bir `.csv` dosyasından okutularak)
  konulabilir — ilgili yer kodda `hipotetik_kirilma_indisi()` fonksiyonudur.

## Kurulum

```bash
git clone <bu-repo-url>
cd fresnel-planck-spektral-analiz
pip install -r requirements.txt
```

## Kullanım

```bash
python fresnel_planck_spektral_analiz.py
```

Çalıştırıldığında:

- Hesaplama ilerlemesi konsola yazdırılır.
- İki panelli bir grafik (`fresnel_planck_analiz.png`) çalışma dizinine
  kaydedilir.
- Grafik ekranda bir pencere olarak da gösterilir.

### Kendi parametrelerinizle çalıştırma

Modül fonksiyonları doğrudan içe aktarılıp farklı sıcaklık, açı/dalgaboyu
aralıklarıyla kullanılabilir:

```python
import numpy as np
from fresnel_planck_spektral_analiz import spektral_analiz_calistir, grafikleri_ciz

dalgaboyu_um, acilar_derece, A, E = spektral_analiz_calistir(
    t_yuzey=1500.0,
    dalgaboyu_um=np.linspace(0.3, 10.0, 200),
    acilar_derece=np.linspace(0, 89, 180),
)

fig = grafikleri_ciz(dalgaboyu_um, acilar_derece, A, E, t_yuzey=1500.0)
fig.savefig("ozel_analiz.png", dpi=200)
```

## Dosya Yapısı

```
.
├── fresnel_planck_spektral_analiz.py   # Ana hesaplama ve görselleştirme kodu
├── requirements.txt                     # Python bağımlılıkları
└── README.md
```

## Gereksinimler

- Python 3.9+
- NumPy >= 1.24
- Matplotlib >= 3.7

## Lisans

Bu proje kişisel/akademik kullanım için paylaşılmıştır. Dilediğiniz gibi
kullanabilir ve değiştirebilirsiniz.
