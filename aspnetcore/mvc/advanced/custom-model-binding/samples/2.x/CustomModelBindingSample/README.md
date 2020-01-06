# <a name="custom-model-binding-demo"></a>Özel model bağlama tanıtımı

Uygulamayı çalıştırıp `ImageController` uç noktasına (`/api/image/`) Base64 olarak kodlanmış bir dize göndererek test `ByteArrayModelBinder`. İstek gövdesinde dosya ve dosya adı özelliklerini form verileri olarak ( [Postman](https://www.getpostman.com/) veya benzer bir araç kullanarak) belirtin. [Bu örnek dizeyi](Base64String.txt)kullanabilirsiniz. Sonuç, belirtilen dosya adı ile *Wwwroot/Images/upload* klasörüne kaydedilir.

Özel bağlama örneğini test etmek için aşağıdaki uç noktaları deneyin:

* /api/Authors/1
* /api/Authors/2 (bulunamadı)
* /api/boundaduthors/1
* /api/boundaduthors/2 (bulunamadı)
* /api/boundaduthors/Get/1
* /api/boundaduthors/Get/2 (IÇERIK yok) &ndash; bu eylem null değerini denetlemez ve bir *404*döndürmez.
