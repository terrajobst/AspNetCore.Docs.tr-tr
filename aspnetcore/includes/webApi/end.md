## <a name="implement-the-other-crud-operations"></a>Diğer CRUD işlemleri uygulama

Aşağıdaki bölümlerde `Create`, `Update`, ve `Delete` yöntemleri, denetleyiciye eklenir.

### <a name="create"></a>Create

Aşağıdakileri ekleyin `Create` yöntemi.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Önceki kod tarafından gösterilen bir HTTP POST yöntemi olup [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. [ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.

`CreatedAtRoute` Yöntemi:

* 201 yanıtı döndürür. HTTP 201 yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt'dir.
* Bir konum üstbilgisi yanıta ekler. Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir. Bkz: [10.2.2 oluşturulan 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* URL oluşturmak için "adlı rota GetTodo" kullanır. "Rota adlı GetTodo" tanımlanan `GetById`:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

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

Konum üstbilgisi URI yeni öğe erişmek için kullanılabilir.

### <a name="update"></a>Güncelleştirme

Aşağıdakileri ekleyin `Update` yöntemi:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update`benzer `Create`, ancak HTTP PUT kullanır. Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir. Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Sil

Aşağıdakileri ekleyin `Delete` yöntemi:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Test `Delete`: 

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmd.png)
