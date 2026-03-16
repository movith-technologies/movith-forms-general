Elbette, BI Rapor Oluşturucu'nun (Report Builder) çalışma mantığını günlük hayattan bir örnekle açıklayayım.

Bir "Satış Formu" doldurduğunuzu hayal edelim. Bu formda şu alanlar olsun:

Şehir (İstanbul, Ankara, İzmir...)
Ürün (Telefon, Laptop, Tablet...)
Fiyat (10.000, 20.000...)
Tarih
Rapor oluştururken bu verileri şu 4 alana yerleştirerek grafiklerinizi tasarlarsınız:

1. Sütunlar (X-Ekseni) — "Neye Göre Gruplayalım?"
Grafiğin alt kısmında (yatay eksende) ne yazacağını belirler. Genellikle metin bazlı alanlardır.

Örnek: "Şehir" alanını buraya koyarsanız, grafiğin altında İstanbul, Ankara gibi şehir isimleri görünür.
2. Değerler (Y-Ekseni) — "Neyi Hesaplayalım?"
Grafiğin sütun uzunluğunu veya çizginin yüksekliğini belirleyen rakamlardır.

Örnek: "Fiyat" alanını buraya koyup "Toplam (Sum)" derseniz, şehirlerin toplam satış tutarını görürsünüz.
Örnek: Eğer buraya "Kayıt Sayısı" (Count) koyarsanız, hangi şehirden kaç adet satış yapıldığını görürsünüz.
3. Satırlar (Seriler) — "Detayı Nasıl Bölelim?"
Veriyi bir katman daha detaylandırmak içindir.

Örnek: X-Ekseninde "Şehir" varken, Seri kısmına "Ürün" derseniz; İstanbul sütunu kendi içinde Telefon, Laptop diye alt parçalara bölünür. Böylece hangi şehirde hangi üründen ne kadar satıldığını aynı grafikte görürsünüz.
4. Filtreler — "Hangi Verileri Atlayalım?"
Raporun kapsamını daraltır.

Örnek: "Fiyat > 5000" filtresi eklerseniz, sadece 5000 TL üzerindeki satışlar rapora dahil edilir.
Grafik Tiplerine Göre Örnekler:
Grafik Tipi	X-Ekseni (Sütun)	Y-Ekseni (Değer)	Sonuç
Sütun (Bar)	Şehir	Fiyat (Toplam)	Hangi şehirde ne kadarlık satış yapıldı? (En yüksek sütun en çok satış olan şehirdir)
Çizgi (Line)	Tarih	Kayıt Sayısı	Günlere göre satış trafiği nasıl değişti? (Artış/azalış trendi)
Pasta (Pie)	Ürün	Kayıt Sayısı	Toplam satışların yüzde kaçı Telefon, yüzde kaçı Laptop?
Tablo (Table)	Kullanıcı	Fiyat (Ortalama)	Hangi kullanıcı ortalama ne kadarlık satış yapıyor? (Liste şeklinde)

Pivot Tablo Nedir ve Nasıl Çalışacak?

Satırlar (Yatay): "Satırlar" kutusuna sürüklediğiniz alanlar tablonun sol tarafında dikey başlıkları oluşturacak.
Sütunlar (Dikey): "Sütunlar" kutusuna sürüklediğiniz alanlar tablonun en üstünde yatay başlıkları oluşturacak.
Değerler (Kesişim): Bu iki değerin kesiştiği hücrelerde ise seçtiğiniz metrikler (Toplam, Adet vb.) hesaplanacak.

Aralıklı Filtrelendirme: (Örneğin tarih aralığı vererek filtreleme)
Filtreler bölgesine aynı alanı (örneğin Sipariş Tarihi veya Oluşturulma Tarihi) birden fazla kez sürüklediğinizde, sistem arka planda bu koşulları "VE" (AND) mantığıyla birbirine bağlar.

Sizin oluşturduğunuz senaryoda:

Sipariş Tarihi > 02/28/2026 (VE)
Sipariş Tarihi < 04/01/2026
Bu iki kural aynı anda çalışacağı için sistem sadece Mart ayına (01 Mart - 31 Mart) ait olan kayıtları bulacak ve raporlayacaktır. Tam olarak "Between (Arasında)" mantığını bu şekilde kurabilirsiniz.

Diğer veri tipleri (Sayısal ve Metin) için de durum şöyle:

Sayısal Değerler (Numeric): Evet, tamamen aynı mantık geçerlidir. Örneğin elinizde bir "Sipariş Tutarı" alanı var. Filtrelere iki kez sürükleyip birine Büyüktür (>) 1000, diğerine Küçüktür (<) 5000 yazarsanız, sadece 1000₺ ile 5000₺ arasındaki siparişlerin raporunu alırsınız. Sistem arka planda otomatik olarak değerin bir sayı olduğunu algılayıp matematiksel büyüklük/küçüklük hesabı yapıyor.
Metin Değerler (String): Metin alanları için "Büyüktür/Küçüktür" mantığı çalışmaz. Örneğin "Müşteri Adı" > "Ahmet" gibi alfabetik bir aralık araması şu anki tasarımda desteklenmiyor. Metinler için filtrelerde genelde "İçerir" (Contains) veya "Eşittir" (Equals) operatörlerini kullanmanız en doğrusu olacaktır.
Özetle; Tarih (DateTime) ve Sayısal (Numeric) tipindeki her alan için bu pratik "iki kere sürükleyip aralık (range) belirleme" yöntemini güvenle kullanabilirsiniz. Çok doğru ve profesyonel bir yaklaşım olmuş!
