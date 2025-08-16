# Python Decorators & İleri Seviye Fonksiyonlar Cheatsheet

Bu rehber, Python'daki decorator yapısını ve temelini oluşturan kavramları özetlemektedir.

---

### 1. Fonksiyonların Birinci Sınıf Nesne Olması (First-Class Functions)

Python'da fonksiyonlar birer nesnedir. Bu, şu anlama gelir:
- Bir değişkene atanabilirler.
- Başka bir fonksiyona argüman olarak verilebilirler.
- Bir fonksiyonun içinde tanımlanabilir ve o fonksiyondan geri döndürülebilirler.

```python
def merhaba():
    print("Merhaba!")

selamla = merhaba  # Fonksiyonu bir değişkene atama
selamla()  # Merhaba!

def calistir(fonksiyon):
    fonksiyon() # Fonksiyonu argüman olarak alıp çalıştırma

calistir(merhaba) # Merhaba!
```

---

### 2. Kapanımlar (Closures)

Bir fonksiyonun, kendi dışındaki bir (enclosing) scop'taki değişkenleri hatırlaması durumudur. İç içe fonksiyonlarda görülür. Dıştaki fonksiyon çalışmasını bitirse bile, içteki fonksiyon dıştaki fonksiyonun değişkenlerine erişebilir.

```python
def dis_fonksiyon(mesaj):
    # 'mesaj' değişkeni, iç fonksiyon tarafından "hatırlanır"
    def ic_fonksiyon():
        print(mesaj)
    
    return ic_fonksiyon

selam_ver = dis_fonksiyon("Selam!")
merhaba_de = dis_fonksiyon("Merhaba!")

# dis_fonksiyon çoktan bitti ama iç fonksiyon 'mesaj'ı hatırlıyor
selam_ver()    # Selam!
merhaba_de()  # Merhaba!
```

---

### 3. Dekoratörler (Decorators)

Dekoratör, mevcut bir fonksiyonun kodunu değiştirmeden ona yeni bir özellik eklememizi sağlayan bir fonksiyondur. Aslında, bir fonksiyonu alıp onu "sarmalayan" (wrap) ve geriye yeni, değiştirilmiş bir fonksiyon döndüren bir yapıdır.

#### Temel Dekoratör Yapısı

```python
import time
import functools

# Bu bizim dekoratörümüz
def zaman_olc(func):
    @functools.wraps(func) # orjinal fonksiyonun bilgilerini korur
    def wrapper(*args, **kwargs):
        baslangic = time.time()
        sonuc = func(*args, **kwargs) # Orjinal fonksiyonu çalıştır
        bitis = time.time()
        print(f'{func.__name__} fonksiyonu {bitis - baslangic:.4f} saniyede çalıştı.')
        return sonuc
    return wrapper
```

#### Dekoratör Kullanımı (`@` sembolü)

`@` sembolü, bir fonksiyonu bir dekoratör ile sarmalamak için kullanılan "sözdizimsel şekerdir" (syntactic sugar).

```python
@zaman_olc
def buyuk_sayilari_topla(n):
    toplam = 0
    for i in range(n):
        toplam += i
    return toplam

print(f'Sonuç: {buyuk_sayilari_topla(10000000)}')

# Çıktı:
# buyuk_sayilari_topla fonksiyonu 0.3456 saniyede çalıştı.
# Sonuç: 49999995000000
```

`@zaman_olc` kullanmak, aşağıdaki kod ile tamamen aynı anlama gelir:

```python
# Uzun yol:
def buyuk_sayilari_topla(n):
    # ... (fonksiyon içeriği aynı)

# buyuk_sayilari_topla = zaman_olc(buyuk_sayilari_topla)
```

---

### Neden `functools.wraps` Kullanmalıyız?

Bir dekoratör kullandığınızda, aslında orjinal fonksiyon yerine `wrapper` fonksiyonunu çağırırsınız. Bu, orjinal fonksiyonun ismini (`__name__`) ve dökümanını (`__doc__`) kaybetmenize neden olur. `@functools.wraps(func)` dekoratörü, bu bilgileri orjinal fonksiyondan kopyalayarak bu sorunu çözer.
