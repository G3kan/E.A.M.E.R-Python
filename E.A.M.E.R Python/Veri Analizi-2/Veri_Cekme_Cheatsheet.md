# Pandas ile Farklı Veri Formatlarını Okuma Cheatsheet'i

Bu döküman, `pandas` kütüphanesi kullanarak çeşitli formatlardaki verileri nasıl okuyabileceğinizi gösteren bir cheatsheet içerir.

## 1. CSV Dosyası Okuma

En yaygın kullanılan formatlardan biridir.

```python
import pandas as pd

# Temel okuma
df_csv = pd.read_csv('dosya_adi.csv')

# Ayırıcı (separator) belirtme (örneğin, noktalı virgül ile ayrılmışsa)
df_csv_semicolon = pd.read_csv('dosya_adi.csv', sep=';')

# Başlık satırı olmadığını belirtme
df_csv_no_header = pd.read_csv('dosya_adi.csv', header=None)

# Belirli sütunları okuma
df_csv_cols = pd.read_csv('dosya_adi.csv', usecols=['sutun1', 'sutun2'])

print(df_csv.head())
```

## 2. Excel Dosyası Okuma

Excel dosyalarını okumak için `openpyxl` veya `xlrd` paketinin kurulu olması gerekebilir (`pip install openpyxl`).

```python
import pandas as pd

# Belirli bir sayfayı okuma (sayfa adıyla)
df_excel_sheet_name = pd.read_excel('dosya_adi.xlsx', sheet_name='Sayfa1')

# Belirli bir sayfayı okuma (sayfa indeksiyle)
df_excel_sheet_index = pd.read_excel('dosya_adi.xlsx', sheet_name=0)

# Başlık satırının hangi satırda olduğunu belirtme
df_excel_header = pd.read_excel('dosya_adi.xlsx', header=2)

print(df_excel_sheet_name.head())
```

## 3. JSON Dosyası Okuma

JSON (JavaScript Object Notation) formatındaki verileri okumak için kullanılır.

```python
import pandas as pd

# Standart JSON formatı
df_json = pd.read_json('dosya_adi.json')

# Farklı orient (yönelim) seçenekleri
# 'records', 'columns', 'index', 'split', 'values' gibi
df_json_records = pd.read_json('dosya_adi.json', orient='records')

print(df_json.head())
```

## 4. SQL Veritabanından Okuma

Bir veritabanından veri çekmek için `SQLAlchemy` gibi bir kütüphaneye ihtiyaç duyulabilir (`pip install sqlalchemy`).

```python
import pandas as pd
from sqlalchemy import create_engine

# Veritabanı bağlantı motoru oluşturma (örnek: SQLite)
engine = create_engine('sqlite:///veritabani.db')

# Bir tablonun tamamını okuma
df_sql_table = pd.read_sql_table('tablo_adi', engine)

# SQL sorgusu ile veri çekme
query = "SELECT sutun1, sutun2 FROM tablo_adi WHERE sutun1 > 5"
df_sql_query = pd.read_sql_query(query, engine)

print(df_sql_query.head())
```

## 5. HTML Tablosu Okuma

Web sayfalarındaki HTML tablolarını çekmek için kullanılır. `lxml`, `html5lib` veya `beautifulsoup4` paketlerinden birinin kurulu olması gerekir (`pip install lxml beautifulsoup4 html5lib`).

```python
import pandas as pd

# URL'deki tüm tabloları bir liste olarak çeker
url = 'https://tr.wikipedia.org/wiki/T%C3%BCrkiye%27deki_illerin_n%C3%BCfus_yo%C4%9Funlu%C4%9Fu'
tables = pd.read_html(url)

# Genellikle ilk tablo istenir
df_html = tables[0]

print(df_html.head())
```

## 6. SAS Dosyası Okuma

SAS (Statistical Analysis System) veri dosyalarını (`.sas7bdat`) okumak için `sas7bdat` veya `pyreadstat` gibi kütüphaneler kullanılabilir. Pandas'ın kendi `read_sas` fonksiyonu da mevcuttur.

```python
import pandas as pd

# SAS dosyası okuma
# Bu fonksiyon, SAS XPORT veya SAS7BDAT formatlarını destekler.
# 'format' parametresi ile 'xport' veya 'sas7bdat' belirtebilirsiniz.
# Varsayılan olarak dosya uzantısına göre formatı tahmin etmeye çalışır.
df_sas = pd.read_sas('dosya_adi.sas7bdat')

# Encoding belirtme (gerekliyse)
df_sas_encoded = pd.read_sas('dosya_adi.sas7bdat', encoding='latin1')

print(df_sas.head())
```