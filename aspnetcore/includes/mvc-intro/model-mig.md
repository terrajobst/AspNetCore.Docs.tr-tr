Database Update komutu aşağıdaki uyarıyı üretir: 

   "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi. Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur. ' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "

Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.

Veritabanı şeması `MvcMovieContext` sınıfında belirtilen modeli temel alır ( *Data/mvcmoviecontext. cs* dosyasında). `InitialCreate` Bağımsız değişkeni geçiş adıdır. Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad seçilidir.

`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.