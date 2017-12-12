# <a name="custom-model-binding-demo"></a>Özel Model bağlama Tanıtımı

Test edebilirsiniz `ByteArrayModelBinder` uygulamayı çalıştırarak ve base64 ile kodlanmış bir dize ImageController uç nakil (/ api/görüntü /). İstek gövdesini form verileri olarak dosya ve dosya adı proparties belirtmeniz gerekir (Postman veya benzer bir aracı kullanarak). Kullanabileceğiniz [Bu örnek dize](Base64String.txt). Belirttiğiniz dosya adı ile görüntüleri/wwwroot/yükleme klasöründeki sonucu kaydedilir.

Özel bağlama örneği test etmek için şu uç noktalar deneyin: /api/authors/1 /api/authors/2 (bulunamadı) /api/boundauthors/1 /api/boundauthors/2 (bulunamadı) /api/boundauthors/get/1 /api/boundauthors/get/2 (içerik) NO - bu eylemi değil olup olmadığını denetleyin NULL ve Not Found döndürür
