::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Yukarıdaki kod, bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar. Sonraki bölümde, API'yi uygulamak için yöntemleri eklenir.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Yukarıdaki kod:

* Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.
* Yeni bir Todo oluşturur öğe `TodoItems` boştur. Tüm Oluşturucu bir yeni bir IF oluşturduğundan Todo öğelerini silmek mümkün olmayacaktır `TodoItems` boştur.

Sonraki bölümde, API'yi uygulamak için yöntemleri eklenir. Sınıf ile açıklanıyor bir `[ApiController]` kullanışlı bazı özellikleri etkinleştirmek için özniteliği. Özniteliği tarafından etkinleştirilen özellikler hakkında daha fazla bilgi için bkz: [ApiController özniteliğiyle ek açıklama](xref:web-api/index#annotation-with-apicontroller-attribute).

::: moniker-end

Denetleyicinin Oluşturucu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`TodoContext`) içine denetleyici. Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri. Yoksa Oluşturucu bir öğe bellek içi veritabanına ekler.

## <a name="get-to-do-items"></a>Yapılacaklar öğelerini alma

Yapılacaklar öğelerini almak için aşağıdaki yöntemi ekleyin. `TodoController` sınıfı:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

Bu yöntemler, iki GET yöntemleri uygulayın:

* `GET /api/todo`
* `GET /api/todo/{id}`

Örnek HTTP yanıtı için `GetAll` yöntemi:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

Öğreticinin sonraki bölümlerinde miyim ile HTTP yanıtının nasıl görüntülenebilir göstereceğiz [Postman](https://www.getpostman.com/) veya [curl](https://curl.haxx.se/docs/manpage.html).

### <a name="routing-and-url-paths"></a>URL Yönlendirme ve yolları

`[HttpGet]` Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir. Her yöntem için URL yolu şu şekilde oluşturulur:

* Denetleyicinin şablonu dizesi ele `Route` özniteliği:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

* Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı. Bu örnek, denetleyici sınıfı adı olan **Todo**denetleyicisi ve kök "todo" adıdır. ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.
* Varsa `[HttpGet]` özniteliğine sahip bir rota şablonu (gibi `[HttpGet("/products")]`, yolunu ekleyin. Bu örnek, bir şablon kullanmaz. Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Aşağıdaki `GetById` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir. Zaman `GetById` olan çağrılır, değeri atar `"{id}"` yöntemin URL'yi `id` parametresi.

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

`Name = "GetTodo"` adlandırılmış bir yol oluşturur. Adlandırılmış rotalar:

* Rota adını kullanarak bir HTTP bağlantısı oluşturmak uygulamayı etkinleştirin.
* Öğreticinin ilerleyen bölümlerinde açıklanmıştır.

### <a name="return-values"></a>Döndürülen değerler

`GetAll` Yöntem koleksiyonu döndürür `TodoItem` nesneleri. MVC nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar. Yanıt kodu 200 olarak bu yöntem için işlenmeyen özel durumlar vardır varsayılıyor. İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.

::: moniker range="<= aspnetcore-2.0"

Buna karşılık, `GetById` yöntemi döndürür daha genel [IActionResult türü](xref:web-api/action-return-types#iactionresult-type), dönüş türleri çeşitli temsil eder. `GetById` iki farklı dönüş türlerine sahiptir:

* Öğe istenen kimliği eşleşirse, yöntem, 404 hatası döndürür. Döndüren [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) bir HTTP 404 yanıtı döndürür.
* Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür. Döndüren [Tamam](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) sonuçları bir HTTP 200 yanıtı.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Buna karşılık, `GetById` yöntemi döndürür [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type), dönüş türleri çeşitli temsil eder. `GetById` iki farklı dönüş türlerine sahiptir:

* Öğe istenen kimliği eşleşirse, yöntem, 404 hatası döndürür. Döndüren [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) bir HTTP 404 yanıtı döndürür.
* Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür. Döndüren `item` sonuçları bir HTTP 200 yanıtı.

::: moniker-end
