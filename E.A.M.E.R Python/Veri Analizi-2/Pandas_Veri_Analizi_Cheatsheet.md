# Pandas ile Veri Analizi Temelleri Cheatsheet'i

Bu döküman, `pandas` kütüphanesini kullanarak veri setlerini anlamak ve manipüle etmek için sıkça kullanılan fonksiyonları ve yöntemleri açıklamaktadır. Örnekler, `yillara_gore_il_nufuslari(2014-2024).xls` dosyası üzerinden uyarlanabilir.

## 1. Veri Setini Yükleme

Öncelikle Excel dosyamızı `pandas` ile bir DataFrame'e yükleyelim:

```python
import pandas as pd

# Excel dosyasını yükleme
# Kendi dosya yolunuzu ve sayfa adınızı (varsa) güncellemeyi unutmayın.
file_path = '../../Veri Analizi-1/yillara gore il nufuslari(2014-2024).xls'
df = pd.read_excel(file_path)

print("DataFrame'in ilk 5 satırı:")
print(df.head())
```

## 2. Veri Setine Genel Bakış

### `df.info()`

`info()` metodu, DataFrame hakkında özet bilgi sağlar. Bu bilgiler arasında DataFrame'deki toplam giriş sayısı, her sütunun veri tipi, boş olmayan değer sayısı ve bellek kullanımı bulunur. Veri setindeki eksik değerleri ve sütun tiplerini hızlıca anlamak için çok kullanışlıdır.

**Parametreler:**
- `verbose` (bool, varsayılan `None`): `True` ise tüm sütunlar için özet yazdırır. `False` ise yalnızca önemli bilgileri yazdırır. `None` ise sütun sayısına göre otomatik karar verir.
- `buf` (writable buffer, varsayılan `sys.stdout`): Çıktının yazılacağı tampon. Genellikle değiştirilmez.
- `max_cols` (int, varsayılan `None`): Yazdırılacak maksimum sütun sayısı. `None` ise tüm sütunları yazdırır.
- `memory_usage` (bool, str, varsayılan `None`): Bellek kullanımını gösterip göstermeyeceğini belirler. `True` ise toplam bellek kullanımını gösterir. `'deep'` ise her bir öğenin gerçek bellek kullanımını hesaplar (daha yavaş olabilir).
- `show_counts` (bool, varsayılan `None`): Boş olmayan değer sayılarını gösterip göstermeyeceğini belirler. `None` ise otomatik karar verir.

```python
print("\nDataFrame Bilgisi (df.info()):")
df.info()
```

### `df.describe()`

`describe()` metodu, DataFrame'deki sayısal sütunlar için istatistiksel özetler (sayı, ortalama, standart sapma, minimum, maksimum ve çeyreklikler) sağlar. Kategorik sütunlar için farklı bir özet sunar.

**Parametreler:**
- `percentiles` (list-like of floats, varsayılan `[.25, .5, .75]`): Hesaplamak istediğiniz çeyreklikler. 0 ile 1 arasında olmalıdır.
- `include` (str, list-like of str, varsayılan `None`): Hangi veri tiplerinin dahil edileceğini belirler. `'all'` tüm sütunları (sayısal ve kategorik) dahil eder. `'object'` veya `'category'` gibi belirli tipleri belirtebilirsiniz. Sayısal tipler için `'number'` kullanabilirsiniz.
- `exclude` (str, list-like of str, varsayılan `None`): Hangi veri tiplerinin hariç tutulacağını belirler. `include` ile aynı değerleri alır.

```python
print("\nDataFrame İstatistiksel Özeti (df.describe()):")
print(df.describe())

# Tüm sütunları dahil ederek (sayısal ve kategorik) özet alma
print("\nDataFrame İstatistiksel Özeti (Tüm Sütunlar - df.describe(include='all')):")
print(df.describe(include='all'))
```

### `df.dtypes`

`dtypes` özelliği, DataFrame'deki her sütunun veri tipini gösterir. Bu, veri temizliği ve dönüşümü için önemlidir.

```python
print("\nDataFrame Sütun Veri Tipleri (df.dtypes):")
print(df.dtypes)
```

## 3. Veri Tiplerini Değiştirme

Bazen sütunların veri tiplerini analiz veya modelleme için değiştirmek gerekebilir. Örneğin, sayısal olması gereken bir sütun `object` (string) olarak okunmuş olabilir.

### `astype()` Metodu

`astype()` metodu, bir sütunun veya birden fazla sütunun veri tipini değiştirmek için kullanılır.

```python
# Örnek bir DataFrame oluşturalım
df_example = pd.DataFrame({
    'Sayısal_Sütun': ['1', '2', '3', '4'],
    'Tarih_Sütun': ['2023-01-01', '2023-01-02', '2023-01-03', '2023-01-04'],
    'Kategorik_Sütun': ['A', 'B', 'A', 'C']
})

print("\nVeri tipi değiştirme öncesi (df_example.dtypes):")
print(df_example.dtypes)

# 'Sayısal_Sütun'u integer tipine dönüştürme
df_example['Sayısal_Sütun'] = df_example['Sayısal_Sütun'].astype(int)

# 'Tarih_Sütun'u datetime tipine dönüştürme
df_example['Tarih_Sütun'] = pd.to_datetime(df_example['Tarih_Sütun'])

# 'Kategorik_Sütun'u category tipine dönüştürme (bellek optimizasyonu için)
df_example['Kategorik_Sütun'] = df_example['Kategorik_Sütun'].astype('category')

print("\nVeri tipi değiştirme sonrası (df_example.dtypes):")
print(df_example.dtypes)
```

### `pd.to_numeric()`, `pd.to_datetime()`, `pd.to_timedelta()`

Bu fonksiyonlar, özellikle dönüşüm sırasında hatalarla karşılaşma olasılığı olan durumlarda daha esnek kontrol sağlar.

```python
# Hatalı değer içeren bir sütun örneği
df_error = pd.DataFrame({'Sayı': ['1', '2', 'hatalı', '4']})

# Hataları NaN olarak dönüştürme (errors='coerce')
df_error['Sayı'] = pd.to_numeric(df_error['Sayı'], errors='coerce')

print("\nHatalı değerleri dönüştürme sonrası (df_error.dtypes):")
print(df_error)
print(df_error.dtypes)
```

## 4. Sütun Adlarını Değiştirme

Sütun adlarını daha anlamlı hale getirmek veya standartlaştırmak için değiştirmek önemlidir.

### `df.rename()` Metodu

`rename()` metodu, sütun veya indeks adlarını değiştirmek için kullanılır. Orijinal DataFrame'i değiştirmek için `inplace=True` parametresi kullanılabilir veya yeni bir DataFrame döndürülebilir.

**Parametreler:**
- `mapper` (dict-like or function): Eski adı yeni adla eşleştiren bir sözlük veya bir fonksiyon.
- `axis` (int or str, varsayılan `0`): `0` veya `'index'` ise indeks adlarını, `1` veya `'columns'` ise sütun adlarını değiştirir.
- `inplace` (bool, varsayılan `False`): `True` ise değişiklikleri orijinal DataFrame üzerinde yapar ve `None` döndürür. `False` ise yeni bir DataFrame döndürür.

```python
# Örnek bir DataFrame oluşturalım
df_cols = pd.DataFrame({
    'eski_sutun_adi_1': [1, 2, 3],
    'eski_sutun_adi_2': [4, 5, 6]
})

print("\nSütun adı değiştirme öncesi (df_cols.columns):")
print(df_cols.columns)

# Tek bir sütun adını değiştirme
df_cols_renamed = df_cols.rename(columns={'eski_sutun_adi_1': 'yeni_sutun_adi_1'})
print("\nTek sütun adı değiştirme sonrası (df_cols_renamed.columns):")
print(df_cols_renamed.columns)

# Birden fazla sütun adını değiştirme (inplace)
df_cols.rename(columns={
    'eski_sutun_adi_1': 'Sutun_A',
    'eski_sutun_adi_2': 'Sutun_B'
}, inplace=True)

print("\nBirden fazla sütun adı değiştirme sonrası (df_cols.columns):")
print(df_cols.columns)
```

### Sütunları Doğrudan Atama

DataFrame'in `columns` özelliğini doğrudan yeni bir liste ile atayarak tüm sütun adlarını değiştirebilirsiniz. Ancak bu yöntem, sütunların sırasının korunmasını gerektirir.

```python
# Örnek bir DataFrame oluşturalım
df_cols_direct = pd.DataFrame({
    'col_1': [1, 2, 3],
    'col_2': [4, 5, 6],
    'col_3': [7, 8, 9]
})

print("\nDoğrudan atama öncesi (df_cols_direct.columns):")
print(df_cols_direct.columns)

# Tüm sütun adlarını doğrudan atama
df_cols_direct.columns = ['Yeni_Kolon_1', 'Yeni_Kolon_2', 'Yeni_Kolon_3']

print("\nDoğrudan atama sonrası (df_cols_direct.columns):")
print(df_cols_direct.columns)
```

