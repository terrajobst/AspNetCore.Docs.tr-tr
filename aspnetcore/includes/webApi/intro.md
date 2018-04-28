## <a name="overview"></a>Genel Bakış

Bu öğretici aşağıdaki API oluşturur:

|API | Açıklama | İstek gövdesi | Yanıt gövdesi |
|--- | ---- | ---- | ---- |
|/Api/TODO Al | Tüm yapılacaklar öğelerini alma | Yok. | Yapılacaklar öğelerini dizisi|
|/Api/todo / {id} Al | Bir öğe kimliği tarafından Al | Yok. | Yapılacaklar öğesi|
|/ Api/todo sonrası | Yeni Öğe Ekle | Yapılacaklar öğesi | Yapılacaklar öğesi |
|PUT /api/todo / {id} | Varolan öğeyi güncelleştir &nbsp; | Yapılacaklar öğesi | Yok. |
|/Api/todo / {id} Sil &nbsp; &nbsp; | Bir öğeyi silin &nbsp; &nbsp; | Yok. | Yok.|

Aşağıdaki diyagramda, uygulamanın temel tasarım gösterir.

![İstemci isteği gönderir soldaki kutu tarafından temsil edilen ve sağ tarafta çizilmiş bir kutusu uygulamasından bir yanıt alır. Uygulama kutusu içinde üç kutuları denetleyici, modeli ve veri erişim katmanı temsil eder. Uygulamanın Denetleyicisi'nde istek gelir ve denetleyici ve veri erişim katmanı arasında okuma/yazma işlemleri gerçekleşir. Model sıralanabilir ve istemciye yanıt döndürdü.](../../tutorials/first-web-api/_static/architecture.png)

* Her web API'si (mobil uygulama, tarayıcı, vb.) tüketir istemcidir. Bu öğretici, bir istemci oluşturmaz. [Postman](https://www.getpostman.com/) veya [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) istemci olarak uygulama test etmek için kullanılır.

* A *modeli* uygulama verileri temsil eden bir nesnedir. Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir. Modelleri temsil C# sınıfları olarak da olarak da bilinen **P**larak **O**ld **C**# **O**nesne (POCOs).

* A *denetleyicisi* HTTP işleyen nesneyi ister ve HTTP yanıtı oluşturur. Bu uygulamanın tek bir denetleyici yok.

* Öğretici basit tutmak için uygulamayı kalıcı bir veritabanı kullanmaz. Örnek uygulama Yapılacaklar öğelerini bir bellek içi veritabanına depolar.
