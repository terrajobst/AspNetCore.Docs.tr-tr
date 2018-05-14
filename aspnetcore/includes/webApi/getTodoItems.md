::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Önceki kod yöntemleri olmadan bir API denetleyicisi sınıfı tanımlar. Sonraki bölümlerde, API uygulamak için yöntemler eklenir.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Önceki kod yöntemleri olmadan bir API denetleyicisi sınıfı tanımlar. Sonraki bölümlerde, API uygulamak için yöntemler eklenir. Sınıf ile Açıklama bir `[ApiController]` uygun bazı özellikleri etkinleştirmek için öznitelik. Özniteliği tarafından etkin özellikler hakkında daha fazla bilgi için bkz: [ApiControllerAttribute sınıfıyla açıklama](xref:web-api/index#annotate-class-with-apicontrollerattribute).
::: moniker-end

Denetleyicinin Oluşturucusu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`TodoContext`) denetleyicisi içine. Veritabanı bağlamı her biri kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyicisi yöntemleri. Yoksa Oluşturucu bir öğe bellek içi veritabanına ekler.

## <a name="get-to-do-items"></a>Yapılacaklar öğelerini alma

Yapılacaklar öğelerini almak için aşağıdaki yöntemi ekleyin `TodoController` sınıfı:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

Bu yöntemler iki GET yöntemleri uygulayın:

* `GET /api/todo`
* `GET /api/todo/{id}`

Bir örnek HTTP yanıtı için işte `GetAll` yöntemi:

```json
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

`[HttpGet]` Öznitelik bir HTTP GET isteğine yanıt bir yöntemi gösterir. Her bir yöntemin URL yolu şu şekilde oluşturulur:

* Denetleyicinin şablonu dizesi ele `Route` özniteliği:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* Değiştir `[controller]` denetleyicinin adı ile olduğu "Controller" soneki eksi denetleyici sınıfı adı. Bu örnek, denetleyici sınıfı adı olan **Yapılacaklar**denetleyicisi ve kök "todo" adıdır. ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.
* Varsa `[HttpGet]` özniteliğine sahip bir rota şablonu (gibi `[HttpGet("/products")]`, yolunu ekleyin. Bu örnek, bir şablon kullanmaz. Daha fazla bilgi için bkz: [özniteliği Http [eylem] özniteliklerle yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Aşağıdaki `GetById` yöntemi, `"{id}"` yapılacak iş öğesinin benzersiz tanımlayıcısı için yer tutucu değişkenidir. Zaman `GetById` olan çağrılan değeri atar `"{id}"` yöntemin URL'yi `id` parametresi.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

`Name = "GetTodo"` adlandırılmış bir yol oluşturur. Adlandırılmış yollar:

* Rota adını kullanarak bir HTTP bağlantısı oluşturmak için uygulama etkinleştirin.
* Daha sonra öğreticide açıklanmıştır.

### <a name="return-values"></a>Döndürülen değerler

`GetAll` Yöntem koleksiyonu döndürür `TodoItem` nesneleri. MVC otomatik olarak serileştiren nesnesine [JSON](https://www.json.org/) ve JSON yanıt iletisi gövdesine yazar. Yanıt kodu 200 bu yöntem olduğu için hiçbir işlenmeyen özel durumları varsayılarak. İşlenmeyen özel durumlar 5xx hatalarla karşılaşırsanız çevrilir.

::: moniker range="<= aspnetcore-2.0"
Buna karşılık, `GetById` yöntemi döndürür daha genel [IActionResult türü](xref:web-api/action-return-types#iactionresult-type), çok çeşitli dönüş türleri temsil eder. `GetById` iki farklı dönüş türü vardır:

* Öğe istenen kimliği eşleşirse, yöntem 404 hatası döndürür. Döndürme [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) bir HTTP 404 yanıtı döndürür.
* Aksi takdirde, yöntemi bir JSON yanıt gövdesine ile 200 döndürür. Döndürme [Tamam](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) sonuçları bir HTTP 200 yanıtı.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Buna karşılık, `GetById` yöntemi döndürür [ActionResult\<T > türü](xref:web-api/action-return-types#actionresultt-type), çok çeşitli dönüş türleri temsil eder. `GetById` iki farklı dönüş türü vardır:

* Öğe istenen kimliği eşleşirse, yöntem 404 hatası döndürür. Döndürme [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) bir HTTP 404 yanıtı döndürür.
* Aksi takdirde, yöntemi bir JSON yanıt gövdesine ile 200 döndürür. Döndürme `item` sonuçları bir HTTP 200 yanıtı.
::: moniker-end