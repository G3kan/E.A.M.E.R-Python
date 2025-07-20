<!-- @format -->

# Python Temelleri - Özet Notlar (Cheatsheet)

Bu döküman, `Python Temeller-1` klasöründeki notların bir özetidir.

---

## 1. Değişkenler ve Temel Veri Tipleri

- **Değişken Tanımlama:** `degisken_adi = deger`

- **Temel Veri Tipleri:**
  - `int`: Tam sayılar (örn: `10`, `-5`)
  - `float`: Ondalıklı sayılar (örn: `3.14`, `-0.5`)
  - `str`: Metinler (örn: `"Merhaba"`, `'Python'`)
  - `bool`: Mantıksal değerler (`True`, `False`)

- **Veri Tipini Öğrenme:**
  ```python
  x = 10
  print(type(x))  # <class 'int'>
  ```

- **Tip Dönüşümü:**
  ```python
  str(123)      # '123'
  int("456")    # 456
  float("3.14") # 3.14
  ```

---

## 2. String (Metin) Methodları

- `upper()` / `lower()`: Tüm harfleri büyük/küçük yapar.
- `title()`: Her kelimenin ilk harfini büyük yapar.
- `capitalize()`: Sadece cümlenin ilk harfini büyük yapar.
- `strip()`: Baş ve sondaki boşlukları siler.
- `replace(eski, yeni)`: Metin içindeki bir bölümü yenisiyle değiştirir.
- `split(ayrac)`: Metni belirtilen ayraca göre bölerek bir liste oluşturur.
- `len(metin)`: Metnin karakter sayısını (uzunluğunu) verir.

---

## 3. Veri Yapıları

### a. Listeler (List)

Sıralı ve **değiştirilebilir** veri koleksiyonlarıdır.

- **Tanımlama:** `liste = [1, "elma", True]`
- **Elemana Erişim:** `liste[0]` (ilk eleman), `liste[-1]` (son eleman)
- **Dilimleme (Slicing):** `liste[baslangic:bitis:adim]`
- **Temel Methodlar:**
  - `append(x)`: Sona eleman ekler.
  - `insert(index, x)`: Belirtilen indekse eleman ekler.
  - `remove(x)`: Belirtilen **değeri** siler.
  - `pop(index)`: Belirtilen **indeksteki** elemanı siler ve döndürür.
  - `sort()`: Sıralar.
  - `reverse()`: Ters çevirir.

### b. Demetler (Tuple)

Sıralı ve **değiştirilemez** veri koleksiyonlarıdır.

- **Tanımlama:** `demet = (1, "elma", True)`
- **Tek Elemanlı Tuple:** `tekli = ("elma",)` (sondaki virgül zorunludur)
- **Temel Methodlar:**
  - `count(x)`: Belirtilen değerin kaç kez tekrar ettiğini sayar.
  - `index(x)`: Belirtilen değerin ilk bulunduğu indeksi verir.

### c. Kümeler (Set)

Sırasız ve **benzersiz** elemanlardan oluşan koleksiyonlardır.

- **Tanımlama:** `kume = {1, "elma", True}`
- **Temel Methodlar:**
  - `add(x)`: Eleman ekler.
  - `remove(x)`: Elemanı siler (eleman yoksa hata verir).
  - `discard(x)`: Elemanı siler (eleman yoksa hata vermez).
- **Küme İşlemleri:**
  - `|` veya `union()`: Birleşim
  - `&` veya `intersection()`: Kesişim
  - `-` veya `difference()`: Fark
  - `^` veya `symmetric_difference()`: Simetrik fark (her iki kümede olup kesişimde olmayanlar)

### d. Sözlükler (Dictionary)

`anahtar:değer` (key:value) çiftlerinden oluşan sırasız ve değiştirilebilir koleksiyonlardır.

- **Tanımlama:** `sozluk = {"ad": "Ali", "yas": 25}`
- **Değere Erişim:**
  - `sozluk["ad"]` (anahtar yoksa hata verir)
  - `sozluk.get("ad")` (anahtar yoksa `None` döner)
- **Eleman Ekleme/Güncelleme:** `sozluk["sehir"] = "Ankara"`
- **Temel Methodlar:**
  - `keys()`: Tüm anahtarları verir.
  - `values()`: Tüm değerleri verir.
  - `items()`: Tüm `(anahtar, değer)` çiftlerini verir.
  - `pop(anahtar)`: Belirtilen anahtardaki elemanı siler ve değerini döndürür.

---

## 4. Kontrol Yapıları ve Döngüler

- **if-elif-else Yapısı:**
  ```python
  if yas < 18:
      print("Genç")
  elif yas >= 18 and yas < 65:
      print("Yetişkin")
  else:
      print("Yaşlı")
  ```

- **for Döngüsü:** (Yinelenebilir nesneler üzerinde gezinir)
  ```python
  for meyve in ["elma", "armut", "kiraz"]:
      print(meyve)
  ```

- **while Döngüsü:** (Koşul doğru olduğu sürece çalışır)
  ```python
  sayac = 0
  while sayac < 3:
      print(sayac)
      sayac += 1
  ```

---

## 5. Fonksiyonlar

- **Fonksiyon Tanımlama (`def`):**
  ```python
  def topla(a, b):
      return a + b

  sonuc = topla(5, 3) # sonuc = 8
  ```

- **Lambda (Anonim) Fonksiyonlar:** (Tek satırlık, isimsiz fonksiyonlar)
  ```python
  carpma = lambda x, y: x * y
  sonuc = carpma(4, 5) # sonuc = 20
  ```
