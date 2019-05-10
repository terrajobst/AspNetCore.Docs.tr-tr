# <a name="custom-model-binding-demo"></a>Özel Model bağlama Tanıtımı

Test `ByteArrayModelBinder` uygulamayı çalıştırarak ve base64 ile kodlanmış dizeye yayınlayarak `ImageController` uç noktası (`/api/image/`). İstek gövdesini form verileri olarak dosya ve dosya özelliklerini belirtin (kullanarak [Postman](https://www.getpostman.com/) veya benzer bir araç). Kullanabileceğiniz [Bu örnek dizedir](Base64String.txt). Sonuç kaydedilir *wwwroot/resimler/karşıya yüklediğiniz* klasör belirtilen dosya adına sahip.

Özel bağlama örnek test etmek için aşağıdaki uç noktaların deneyin:

* /api/Authors/1
* /api/Authors/2 (bulunamadı)
* /api/boundauthors/1
* /api/boundauthors/2 (bulunamadı)
* /api/boundauthors/Get/1
* (içerik yok) /api/boundauthors/Get/2 &ndash; Bu eylem için null denetlemez ve döndüren bir *404 Bulunamadı*.
