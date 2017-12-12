[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Önceki kod:

* Bir boş denetleyicisi sınıfı tanımlar. Sonraki bölümlerde, API uygulamak için yöntemler eklenir.
* Oluşturucusu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`TodoContext `) denetleyicisi içine. Veritabanı bağlamı her biri kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyicisi yöntemleri.
* Yoksa Oluşturucu bir öğe bellek içi veritabanına ekler.

## <a name="getting-to-do-items"></a>Yapılacaklar öğelerini alma

Yapılacaklar öğelerini almak için aşağıdaki yöntemi ekleyin `TodoController` sınıfı.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Bu yöntemler iki GET yöntemleri uygulayın:

* `GET /api/todo`
* `GET /api/todo/{id}`

Bir örnek HTTP yanıtı için işte `GetAll` yöntemi:

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

Öğreticide daha sonra t ile HTTP yanıtı nasıl görüntülenebilir göstereceğiz [Postman](https://www.getpostman.com/) veya [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Yönlendirme ve URL yolları

`[HttpGet]` Özniteliği, bir HTTP GET yöntemi belirtir. Her bir yöntemin URL yolu şu şekilde oluşturulur:

* Denetleyicinin şablonu dizesi ele `Route` özniteliği:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* "[Denetleyici]" denetleyici sınıfı adı "Controller" soneki eksi denetleyicinin adı ile değiştirin. Bu örnek, denetleyici sınıfı adı olan **Yapılacaklar**denetleyicisi ve kök "todo" adıdır. ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük küçük harfe duyarlı değildir.
* Varsa `[HttpGet]` özniteliğine sahip bir rota şablonu (gibi `[HttpGet("/products")]`, yolunu ekleyin. Bu örnek, bir şablon kullanmaz. Bkz: [özniteliği Http [eylem] özniteliklerle yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) daha fazla bilgi için.

İçinde `GetById` yöntemi:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

`"{id}"`Kimliği için bir yer tutucu değişkendir `todo` öğesi. Zaman `GetById` olan çağrılır, "{id}" değerini yöntemin URL'yi atar `id` parametresi.

`Name = "GetTodo"`adlandırılmış bir yol oluşturur. Adlandırılmış yollar:

* Rota adını kullanarak bir HTTP bağlantısı oluşturmak için uygulama etkinleştirin.
* Daha sonra öğreticide açıklanmıştır.

### <a name="return-values"></a>Döndürülen değerler

`GetAll` Yöntemi döndürür bir `IEnumerable`. MVC otomatik olarak serileştiren nesnesine [JSON](http://www.json.org/) ve JSON yanıt iletisi gövdesine yazar. Yanıt kodu 200 bu yöntem olduğu için hiçbir işlenmeyen özel durumları varsayılarak. (İşlenmeyen özel durumlar 5xx hatalarla karşılaşırsanız çevrilir.)

Buna karşılık, `GetById` yöntemi döndürür daha genel `IActionResult` çok çeşitli dönüş türleri temsil eden tür. `GetById`iki farklı dönüş türü vardır:

* Öğe istenen kimliği eşleşirse, yöntem 404 hatası döndürür. Döndürme `NotFound` bir HTTP 404 yanıtı döndürür.

* Aksi takdirde, yöntemi bir JSON yanıt gövdesine ile 200 döndürür. Döndürme `ObjectResult` bir HTTP 200 yanıtı döndürür.
