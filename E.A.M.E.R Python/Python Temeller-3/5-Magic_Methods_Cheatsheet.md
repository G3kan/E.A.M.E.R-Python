# Python Magic Methods (Dunder) Cheatsheet

Python'da çift alt çizgi ile başlayıp biten metotlar (`__str__` gibi), özel davranışları tanımlar. Python, operatörleri veya yerleşik fonksiyonları kullandığınızda bu metotları arka planda çağırır.

---

### Nesne Temsili

- `__init__(self, ...)`: Nesne ilk oluşturulduğunda çalışan yapıcı metot.
- `__str__(self)`: `print()` veya `str()` kullanıldığında çağrılır. Kullanıcı dostu bir metin döndürmelidir.
- `__repr__(self)`: Nesne doğrudan çağrıldığında veya `repr()` ile kullanıldığında çalışır. Geliştiriciye yönelik, nesneyi yeniden oluşturabilecek teknik bir metin döndürmelidir.

```python
class Kitap:
    def __init__(self, ad, yazar):
        self.ad = ad
        self.yazar = yazar

    def __str__(self):
        return f'{self.ad} - {self.yazar}'

    def __repr__(self):
        return f'Kitap(ad="{self.ad}", yazar="{self.yazar}")'

kitap = Kitap("Suç ve Ceza", "Dostoyevski")
print(kitap)  # __str__ -> Suç ve Ceza - Dostoyevski
# kitap      # __repr__ -> Kitap(ad="Suç ve Ceza", yazar="Dostoyevski")
```

---

### Aritmetik Operatörler

- `__add__(self, other)`: `+` operatörü
- `__sub__(self, other)`: `-` operatörü
- `__mul__(self, other)`: `*` operatörü

```python
class Vektor:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, diger):
        return Vektor(self.x + diger.x, self.y + diger.y)

    def __str__(self):
        return f'({self.x}, {self.y})'

v1 = Vektor(1, 2)
v2 = Vektor(3, 4)
v3 = v1 + v2
print(v3)  # (4, 6)
```

---

### Karşılaştırma Operatörleri

- `__eq__(self, other)`: `==` (eşit)
- `__ne__(self, other)`: `!=` (eşit değil)
- `__lt__(self, other)`: `<` (küçük)
- `__gt__(self, other)`: `>` (büyük)
- `__le__(self, other)`: `<=` (küçük veya eşit)
- `__ge__(self, other)`: `>=` (büyük veya eşit)

```python
class Ogrenci:
    def __init__(self, ad, notu):
        self.ad = ad
        self.notu = notu

    def __eq__(self, diger):
        return self.notu == diger.notu

ogr1 = Ogrenci("Ali", 90)
ogr2 = Ogrenci("Veli", 90)
print(ogr1 == ogr2)  # True
```

---

### Koleksiyon (Container) Metotları

- `__len__(self)`: `len()` fonksiyonu
- `__getitem__(self, key)`: `[]` ile eleman okuma (indeksleme)
- `__setitem__(self, key, value)`: `[]` ile eleman atama
- `__delitem__(self, key)`: `del` ile eleman silme

```python
class Kutuphane:
    def __init__(self):
        self.kitaplar = []

    def __len__(self):
        return len(self.kitaplar)

    def __getitem__(self, index):
        return self.kitaplar[index]

    def __setitem__(self, index, kitap):
        self.kitaplar[index] = kitap

k = Kutuphane()
k.kitaplar.append("Sefiller")
k.kitaplar.append("1984")

print(len(k))      # 2
print(k[0])        # Sefiller
k[0] = "Suç ve Ceza"
print(k[0])        # Suç ve Ceza
```
