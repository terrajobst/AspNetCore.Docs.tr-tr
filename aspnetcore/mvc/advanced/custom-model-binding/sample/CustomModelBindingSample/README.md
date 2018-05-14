# <a name="custom-model-binding-demo"></a>Özel Model bağlama Tanıtımı

Test `ByteArrayModelBinder` uygulamayı çalıştırarak ve Base64 ile kodlanmış bir dize olarak nakil `ImageController` uç noktası (`/api/image/`). İstek gövdesini form verileri olarak dosya ve dosya adı proparties belirtin (kullanarak [Postman](https://www.getpostman.com/) veya benzer bir aracı). Kullanabileceğiniz [Bu örnek dize](Base64String.txt). Sonuç kaydedilir *görüntüleri/wwwroot/karşıya* klasörü belirtilen dosya adı.

Özel bağlama örneği test etmek için şu uç noktalar deneyin:

* /api/Authors/1
* /api/Authors/2 (bulunamadı)
* /api/boundauthors/1
* /api/boundauthors/2 (bulunamadı)
* /api/boundauthors/Get/1
* /api/boundauthors/Get/2 (NO içerik) &ndash; Bu eylem için null denetlemez ve döndüren bir *404 Bulunamadı*.
