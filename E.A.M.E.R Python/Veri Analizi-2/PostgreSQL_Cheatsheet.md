# Python ile PostgreSQL Veritabanı İşlemleri Cheatsheet

Bu döküman, Python kullanarak PostgreSQL veritabanına nasıl bağlanılacağını, sorgu çalıştırılacağını ve veri manipülasyonu yapılacağını adım adım açıklamaktadır. `psycopg2` kütüphanesi ve `pandas` ile entegrasyon ele alınacaktır.

## 1. Gerekli Kütüphanelerin Kurulumu

PostgreSQL ile iletişim kurmak için `psycopg2` ve `pandas` ile veritabanı işlemlerini kolaylaştırmak için `SQLAlchemy` kütüphanelerine ihtiyacımız var.

```bash
pip install psycopg2-binary sqlalchemy pandas
```
*Not: `psycopg2-binary` paketi, geliştirme ortamları için hızlı bir başlangıç sunar. Production ortamları için kaynak koddan derlenen `psycopg2` paketini kullanmak daha iyi bir pratik olabilir.*

---

## 2. Veritabanına Bağlanma

İlk adım, veritabanı bağlantı bilgilerini kullanarak bir bağlantı nesnesi (`connection`) oluşturmaktır.

```python
import psycopg2

try:
    # Bağlantı nesnesi oluşturma
    conn = psycopg2.connect(
        host="localhost",      # Veritabanı sunucu adresi (veya IP)
        database="veritabani_adi", # Bağlanılacak veritabanının adı
        user="kullanici_adi",  # PostgreSQL kullanıcı adı
        password="sifreniz"    # Kullanıcı şifresi
        # port="5432"          # Varsayılan port 5432'dir, farklıysa belirtin
    )
    print("PostgreSQL veritabanına başarıyla bağlanıldı.")
    
    # Bağlantıyı daha sonra kullanmak üzere açık bırakın veya kapatın
    conn.close()

except psycopg2.OperationalError as e:
    print(f"Bağlantı hatası: {e}")
```

---

## 3. Sorgu Çalıştırma: `Cursor` Kullanımı

SQL komutlarını çalıştırmak için bir `cursor` (imleç) nesnesine ihtiyacımız vardır. `cursor`, veritabanı oturumu üzerinde kontrol sahibi olmamızı sağlar.

### a. `SELECT` Sorgusu ve Veri Çekme

```python
import psycopg2

# ... (yukarıdaki gibi bağlantı kurun) ...

# Bir cursor oluştur
cur = conn.cursor()

# SQL sorgusunu çalıştır
cur.execute("SELECT * FROM calisanlar LIMIT 5;")

# Sonuçları çekme yöntemleri

# 1. fetchone(): Sorgu sonucundan sıradaki ilk satırı alır.
print("--- fetchone() ---")
ilk_satir = cur.fetchone()
print(ilk_satir)

# 2. fetchmany(size): Belirtilen sayıda satırı bir liste olarak alır.
print("
--- fetchmany(2) ---")
sonraki_iki_satir = cur.fetchmany(2)
for row in sonraki_iki_satir:
    print(row)

# 3. fetchall(): Sorgu sonucunda kalan tüm satırları bir liste olarak alır.
print("
--- fetchall() ---")
kalan_satirlar = cur.fetchall()
for row in kalan_satirlar:
    print(row)

# Cursor ve bağlantıyı kapatmayı unutmayın!
cur.close()
conn.close()
```

### b. Güvenli Sorgular: SQL Injection'a Karşı Parametre Kullanımı

Kullanıcıdan veya dış kaynaklardan alınan verileri doğrudan SQL sorgularına eklemek (`string formatting` ile) ciddi bir güvenlik açığı olan **SQL Injection**'a neden olur. Bunu önlemek için sorgu parametreleri kullanılmalıdır.

```python
# YANLIŞ YÖNTEM (Güvenlik Açığı Var!)
# department_id = "some_user_input"
# cur.execute(f"SELECT * FROM calisanlar WHERE departman_id = {department_id}")

# DOĞRU YÖNTEM (Güvenli)
department_id = 101
cur.execute("SELECT * FROM calisanlar WHERE departman_id = %s", (department_id,))
results = cur.fetchall()
print(results)
```

---

## 4. Veri Manipülasyonu: INSERT, UPDATE, DELETE

Veri ekleme, güncelleme veya silme gibi işlemlerden sonra değişikliklerin kalıcı olması için `connection.commit()` metodu çağrılmalıdır. Hata durumunda veya değişiklikleri geri almak için `connection.rollback()` kullanılabilir.

```python
# ... (bağlantı ve cursor oluşturuldu) ...

try:
    # INSERT: Yeni bir kayıt ekleme
    insert_query = "INSERT INTO calisanlar (isim, maas, departman_id) VALUES (%s, %s, %s)"
    yeni_calisan = ('Ahmet Yılmaz', 5000, 102)
    cur.execute(insert_query, yeni_calisan)
    print("Yeni çalışan eklendi.")

    # UPDATE: Mevcut bir kaydı güncelleme
    update_query = "UPDATE calisanlar SET maas = %s WHERE isim = %s"
    cur.execute(update_query, (5500, 'Ahmet Yılmaz'))
    print("Çalışan maaşı güncellendi.")

    # Değişiklikleri veritabanına işle
    conn.commit()
    print("Değişiklikler başarıyla kaydedildi.")

except (Exception, psycopg2.DatabaseError) as error:
    print(f"Hata: {error}")
    # Hata durumunda değişiklikleri geri al
    conn.rollback()
    print("Değişiklikler geri alındı.")

finally:
    # Cursor ve bağlantıyı kapat
    if conn is not None:
        cur.close()
        conn.close()
```

---

## 5. Pandas ve SQLAlchemy ile Entegrasyon

`pandas`, `SQLAlchemy` motoru aracılığıyla veritabanı işlemlerini çok daha kolay hale getirir.

### a. SQL Sorgusundan DataFrame Oluşturma

```python
import pandas as pd
from sqlalchemy import create_engine

# SQLAlchemy motoru oluşturma
# Format: postgresql://kullanici:sifre@host:port/veritabani
db_url = 'postgresql://kullanici_adi:sifreniz@localhost:5432/veritabani_adi'
engine = create_engine(db_url)

# SQL sorgusunu doğrudan bir DataFrame'e okuma
query = "SELECT isim, maas, departman_adi FROM calisanlar c JOIN departmanlar d ON c.departman_id = d.id;"
df = pd.read_sql_query(query, con=engine)

print(df.head())
```

### b. DataFrame'i SQL Tablosuna Yazma

```python
# ... (yukarıdaki gibi engine oluşturuldu) ...

# Örnek bir DataFrame
data = {'sutun1': ['deger1', 'deger2'], 'sutun2': [10, 20]}
yeni_df = pd.DataFrame(data)

# DataFrame'i 'yeni_tablo' adıyla veritabanına yazma
# if_exists='fail' -> Tablo varsa hata ver (varsayılan)
# if_exists='replace' -> Tablo varsa sil, yeniden oluştur
# if_exists='append' -> Tablo varsa sonuna ekle
yeni_df.to_sql(
    'yeni_tablo', 
    con=engine, 
    if_exists='replace', 
    index=False # DataFrame indeksini ayrı bir sütun olarak yazma
)

print("DataFrame başarıyla veritabanına yazıldı.")
```
