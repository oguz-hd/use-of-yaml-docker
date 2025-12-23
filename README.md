# YAML ve Docker Compose Yapılandırma Rehberi

Bu belge, **YAML** dilinin sözdizimini, kurallarını ve özellikle **Docker** konteyner mimarisindeki (Docker Compose) kritik rolünü detaylıca açıklar.

**Hazırlayanlar:** Oğuz & Kerem  
**Ders:** İnternet Programcılığı

---

## 1. YAML Nedir?
**YAML** (YAML Ain't Markup Language), insan tarafından okunabilirliği en yüksek olan **veri serileştirme** dilidir. 

Genellikle şu amaçlarla kullanılır:
* **Konfigürasyon Dosyaları:** Docker, Kubernetes, Ansible vb.
* **Veri Transferi:** Uygulamalar arası veri taşıma.

### Neden JSON veya XML değil de YAML?
* **Daha Temiz:** Parantez `{}`, noktalı virgül `;` veya etiket `< >` karmaşası yoktur.
* **Hiyerarşik:** Girintiler (boşluklar) ile yapı kurulur, bu da okunmayı kolaylaştırır.

---

## 2. Temel YAML Sözdizimi (Syntax)

YAML yazarken uyulması gereken en önemli kural: **ASLA TAB TUŞU KULLANMA!** Her zaman boşluk (space) kullanılmalıdır.

### Anahtar-Değer Çiftleri (Key-Value)
YAML'ın temeli, iki nokta üst üste `:` ile ayrılmış anahtar ve değerlerdir.

**YAML Kodu:**
```yaml
ad: "Oğuz"
ders: "İnternet Programcılığı"
aktif_mi: true
port: 8080
Girinti ve Hiyerarşi (Indentation)Alt özellikleri belirtmek için genellikle 2 boşluk bırakılır.YAML Kodu:YAMLsunucu:
  ip: 192.168.1.1
  durum: aktif
  ayarlar:
    zaman_asimi: 30
    dil: tr
Listeler (Lists/Sequences)Bir listenin elemanlarını belirtmek için tire - işareti kullanılır.YAML Kodu:YAMLprogramlama_dilleri:
  - Python
  - JavaScript
  - Go
Sözlükler ve Listelerin BirleşimiGenellikle Docker Compose dosyalarında bu yapı kullanılır.YAML Kodu:YAMLogrenciler:
  - isim: Ahmet
    not: 90
  - isim: Ayşe
    not: 85
3. Docker Compose ve YAMLDocker'da docker-compose.yml dosyası, çoklu konteyner uygulamalarını tanımlamak ve çalıştırmak için kullanılır. İşte gerçek bir senaryo üzerinden inceleme:Örnek docker-compose.yml DosyasıAşağıdaki kod, bir Web Uygulaması ve bir Veritabanı (Redis) servisini aynı anda ayağa kaldırır.YAMLversion: '3.8'  # Docker Compose sürümü

services:       # Servislerin (Konteynerlerin) listesi
  
  web_uygulamasi:
    image: python:alpine           # Kullanılacak İmaj
    ports:
      - "5000:5000"                # Port Yönlendirme (Host:Container)
    volumes:
      - .:/code                    # Dosya Paylaşımı (Host klasörünü container'a bağlama)
    environment:
      FLASK_ENV: development       # Ortam Değişkenleri
    depends_on:
      - redis_db                   # Önce veritabanının başlamasını bekle

  redis_db:
    image: redis:alpine
    restart: always                # Hata verirse yeniden başlat
Docker Compose Anahtar KelimeleriAnahtar KelimeAçıklamaversionCompose dosyasının format sürümünü belirtir.servicesÇalıştırılacak konteynerlerin tanımlandığı ana bölümdür.imageKonteynerin hangi imajdan (Docker Hub) oluşturulacağını belirtir.buildHazır imaj yerine, bir Dockerfile dosyasından imaj oluşturulacaksa kullanılır.portsHost_Port:Container_Port şeklinde dışarıya kapı açar.volumesVerilerin kalıcı olmasını sağlar veya kod dosyasını içeri aktarır.environmentKonteyner içine değişken (şifre, API key vb.) göndermek için kullanılır.4. İleri Seviye ÖzelliklerÇok Satırlı Dizeler (Multi-line Strings)YAML'da uzun metinleri yönetmek için | ve > operatörleri kullanılır.| (Literal Style): Satır sonlarını korur.YAMLmesaj: |
  Bu birinci satır.
  Bu ikinci satır.
> (Folded Style): Satır sonlarını boşluğa çevirir (tek satır yapar).YAMLozet: >
  Bu çok uzun bir cümle ama
  YAML bunu okurken tek bir satır
  olarak birleştirecek.
Çapalar ve Referanslar (Anchors & Aliases)Tekrar eden kodları yazmamak için & (tanımlama) ve * (kullanma) işaretleri kullanılır. Docker Compose'da "ortak ayarlar" için çok faydalıdır.YAML Kodu:YAML# Ortak ayarı tanımla
ortak_ayar: &base
  restart: always
  logging: true

services:
  web:
    <<: *base       # Ortak ayarı buraya kopyala
    image: nginx
  
  db:
    <<: *base       # Ortak ayarı buraya da kopyala
    image: postgres
5. Sık Yapılan Hatalar ve En İyi UygulamalarYAML hataya çok açık bir dildir. Docker projenizin çalışması için aşağıdaki kurallara dikkat edin.✅ Bunu Yapın (Doğru)❌ Bunu Yapmayın (Yanlış)Boşluk (Space) kullanın.TAB tuşu kullanmayın. (YAML tab karakterini tanımaz).anahtar: değer (iki noktadan sonra boşluk).anahtar:değer (Bitişik yazmayın).Hiyerarşi için alt alta hizalayın.Hizalamayı kaydırırsanız kod çalışmaz.Metinleri tırnak içine almak zorunlu değildir.Ancak içinde özel karakter (:, {, [) varsa tırnak "..." kullanın.JSON vs YAML KarşılaştırmasıJSON:JSON{
  "servis": "web",
  "port": [80, 443]
}
YAML:YAMLservis: web
port:
  - 80
  - 443