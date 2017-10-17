## <a name="implement-the-other-crud-operations"></a>Diğer CRUD işlemleri uygulama

Ekleyeceğiz `Create`, `Update`, ve `Delete` denetleyiciye yöntemleri. I yalnızca kodu gösterir ve temel farklar vurgulayın bunlar bir tema çeşidi olduğundan. Ekleme veya kod değiştirdikten sonra projeyi oluşturun.

### <a name="create"></a>Create

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Bu belirttiği bir HTTP POST yöntemi olup [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. [ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.

`CreatedAtRoute` Yöntemi yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt 201 bir yanıt döndürür. `CreatedAtRoute`Ayrıca bir konum üstbilgisi yanıta ekler. Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir. Bkz: [10.2.2 oluşturulan 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Postman oluşturma isteği göndermek için kullanın

![Postman konsol](../../tutorials/first-web-api/_static/pmc.png)

* HTTP yöntemini ayarlayın`POST`
* Seçin **gövde** radyo düğmesi
* Seçin **ham** radyo düğmesi
* Türü için JSON ayarlayın
* Anahtar-değer Düzenleyicisi'nde bir Yapılacaklar öğesi gibi girin 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Seçin **Gönder**

* Alt bölme ve kopyalama Üstbilgileri sekmesini seçin **konumu** üstbilgisi:

![Postman konsolunun üst bilgiler sekmesi](../../tutorials/first-web-api/_static/pmget.png)

Yeni oluşturduğunuz kaynağa erişmek için konum üstbilgisi URI kullanabilirsiniz. Geri çağırma `GetById` oluşturulan yöntemi `"GetTodo"` rota adlı:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a>Güncelleştirme

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update`benzer `Create`, ancak HTTP PUT kullanır. Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir. Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Sil

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmd.png)
