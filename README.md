
```markdown
# YAML (YAML Ain't Markup Language) Rehberi

Bu rehber; YAML'ın ne olduğunu, nerede kullanıldığını, temel sözdizimini ve ileri seviye özelliklerini kapsayan bir başvuru kaynağıdır.

## 📋 İçindekiler
- [YAML Nedir?](#yaml-nedir)
- [Neden ve Nerede Kullanılır?](#neden-ve-nerede-kullanılır)
- [Temel Kurallar](#temel-kurallar)
- [Veri Yapıları](#veri-yapıları)
  - [Scalar (Tekil Değerler)](#scalar-tekil-değerler)
  - [Diziler (Lists/Sequences)](#diziler-listssequences)
  - [Sözlükler (Dictionaries/Maps)](#sözlükler-dictionariesmaps)
- [İleri Seviye Özellikler](#ileri-seviye-özellikler)
  - [Çok Satırlı Metinler](#çok-satırlı-metinler)
  - [Anchor ve Alias (Tekrarları Önleme)](#anchor-ve-alias-tekrarları-önleme)
- [YAML vs JSON](#yaml-vs-json)

---

## 🧐 YAML Nedir?

YAML (**Y**AML **A**in't **M**arkup **L**anguage), insanlar tarafından kolayca okunabilen ve yazılabilen bir **veri serileştirme** (data serialization) dilidir. XML veya JSON gibi dillerin aksine, daha az karmaşık bir yapıya sahiptir ve görsel olarak daha temizdir.

Temel amacı, verileri uygulamalar arasında taşımak veya yapılandırma dosyalarını (config files) oluşturmaktır.

---

## 🚀 Neden ve Nerede Kullanılır?

YAML, sadeliği nedeniyle özellikle modern yazılım geliştirme süreçlerinde standart haline gelmiştir.

* **Yapılandırma Dosyaları (Configuration Files):** Uygulamaların ayarlarını tutmak için.
* **Docker & Kubernetes:** Konteyner orkestrasyonu ve tanımlamaları için (`docker-compose.yaml`).
* **CI/CD Süreçleri:** GitHub Actions, GitLab CI, Jenkins gibi araçlarda pipeline tanımları için.
* **Log Dosyaları:** Okunabilir log çıktıları oluşturmak için.

---

## 📏 Temel Kurallar

YAML yazarken dikkat edilmesi gereken en kritik kurallar şunlardır:

1.  **Girintileme (Indentation):** Hiyerarşiyi belirtmek için kullanılır.
    * ⚠️ **Asla TAB tuşu kullanmayın.** Sadece boşluk (space) kullanın.
    * Genellikle her seviye için **2 boşluk** standardı benimsenir.
2.  **Büyük/Küçük Harf Duyarlılığı:** `Ad` ile `ad` farklı anahtarlardır (Case sensitive).
3.  **Anahtar-Değer İlişkisi:** Veriler `anahtar: değer` şeklinde saklanır. `:` işaretinden sonra mutlaka bir boşluk bırakılmalıdır.
4.  **Yorum Satırları:** `#` işareti ile başlar. Derleyici bu satırları yok sayar.

```yaml
# Bu bir yorum satırıdır
anahtar: değer # Satır sonuna da yorum eklenebilir

```

---

## 🧱 Veri Yapıları

### Scalar (Tekil Değerler)

Stringler (metin), sayılar (integer/float) ve boolean (doğru/yanlış) değerlerdir.

```yaml
isim: "Ahmet"       # Çift tırnak (isteğe bağlı)
meslek: Mühendis    # Tırnaksız kullanım yaygındır
yas: 25             # Integer
boy: 1.78           # Float
ogrenci_mi: true    # Boolean (true/false, yes/no, on/off)
bos_deger: null     # Null değeri (~ sembolü de null anlamına gelir)

```

### Diziler (Lists/Sequences)

Bir liste oluşturmak için her öğenin başına tire (`-`) ve bir boşluk konur.

```yaml
programlama_dilleri:
  - Python
  - JavaScript
  - Go
  - C++

# Alternatif (Inline) yazım şekli (JSON benzeri):
alisveris_listesi: [Elma, Armut, Süt]

```

### Sözlükler (Dictionaries/Maps)

İç içe geçmiş anahtar-değer yapılarıdır. Nesne (Object) tanımlamak için kullanılır.

```yaml
kullanici:
  id: 101
  ad: Zeynep
  adres:
    sehir: İzmir
    posta_kodu: 35000
    ulke: Türkiye

```

---

## 🛠 İleri Seviye Özellikler

### Çok Satırlı Metinler

Uzun metinleri yönetmek için iki farklı operatör kullanılır: `|` ve `>`.

1. **Literal Style (`|`):** Satır sonlarını (yeni satır karakterini) olduğu gibi korur.

```yaml
siir: |
  Gözlerin,
  Gözlerin zindandakilere,
  Umut verir.
# Çıktıda her satır alt alta görünür.

```

2. **Folded Style (`>`):** Satır sonlarını birleştirir, metni tek bir satıra dönüştürür. Sadece okunabilirlik için editörde alt alta yazarsınız.

```yaml
aciklama: >
  Bu çok uzun bir cümledir ve editörde
  yer kaplamaması için alt alta yazılmıştır
  fakat işlendiğinde tek satır olacaktır.
# Çıktı: "Bu çok uzun bir cümledir ve editörde..."

```

### Anchor ve Alias (Tekrarları Önleme)

YAML, verileri tekrar yazmayı önlemek için `&` (anchor/çapa) ve `*` (alias/referans) kullanımını destekler. (DRY - Don't Repeat Yourself).

```yaml
# Tanımlama (& ile işaretle)
varsayilan_ayarlar: &base
  adapter: postgres
  host: localhost
  port: 5432

# Kullanma (* ile çağır)
development:
  database: dev_db
  <<: *base  # base içindeki her şeyi buraya kopyalar

test:
  database: test_db
  <<: *base

# Sonuçta hem development hem test ortamı, adapter/host/port bilgilerine sahip olur.

```

---

## 🆚 YAML vs JSON

JSON ve YAML benzer işleri yapsa da bazı temel farklar vardır:

| Özellik | YAML | JSON |
| --- | --- | --- |
| **Okunabilirlik** | Çok Yüksek (İnsan odaklı) | Yüksek (Makine odaklı) |
| **Yorum Satırı** | Destekler (`#`) | Desteklemez |
| **Sözdizimi** | Girinti tabanlı | Parantez `{}` ve tırnak `""` tabanlı |
| **Dosya Boyutu** | Genellikle daha küçük | Biraz daha büyük (karakter fazlalığı) |
| **Hız** | Parse etmesi nispeten yavaştır | Parse etmesi çok hızlıdır |

---

*Bu rehber, GitHub reponuzda YAML kullanımını standartlaştırmak ve temel bilgileri sağlamak amacıyla oluşturulmuştur.*

```

```