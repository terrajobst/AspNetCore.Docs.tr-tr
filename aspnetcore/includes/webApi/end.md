## <a name="implement-the-other-crud-operations"></a>Bir CRUD işlemleri uygulama

Aşağıdaki bölümlerde `Create`, `Update`, ve `Delete` yöntemleri, denetleyiciye eklenir.

### <a name="create"></a>Create

Aşağıdaki `Create` yöntemi:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği HTTP isteği gövdesinden yapılacak iş öğesi değeri almak için MVC söyler.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. MVC, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.

::: moniker-end

`CreatedAtRoute` Yöntemi:

* 201 yanıtı döndürür. HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.
* Bir konum üst bilgisi yanıta ekler. Location üst bilgisini, yeni oluşturulan yapılacak iş öğesi URI'sini belirtir. Bkz: [10.2.2 oluşturulan 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* URL oluşturmak için "adlı rota GetTodo" kullanır. "Adlı rota GetTodo" içinde tanımlanan `GetById`:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Bir oluşturma isteği göndermek için Postman'ı kullanma

* Uygulamayı başlatın.
* Postman'i açın.

![Postman Konsolu](../../tutorials/first-web-api/_static/pmc.png)

* Localhost URL'sini kullanılan bağlantı noktası numarasını güncelleştirin.
* HTTP yöntemi kümesine *POST*.
* Tıklayın **gövdesi** sekmesi.
* Seçin **ham** radyo düğmesi.
* Tür kümesine *JSON (application/json)*.
* İstek gövdesi aşağıdaki JSON benzeyen bir yapılacak iş öğesi ile girin:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Tıklayın **Gönder** düğmesi.

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> Yanıt tıklandıktan sonra görüntülerse **Gönder**, devre dışı **SSL sertifika doğrulama** seçeneği. Bunun altında bulunan **dosya** > **ayarları**. Tıklayın **Gönder** ayarı devre dışı bırakma sonra yeniden düğmesi.

::: moniker-end

Tıklayın **üstbilgileri** sekmesinde **yanıt** bölmesi ve kopyalama **konumu** üst bilgi değeri:

![Postman konsolunun üst bilgiler sekmesi](../../tutorials/first-web-api/_static/pmc2.png)

Konum üst bilgisi URI, yeni öğeye erişmek için kullanılabilir.

### <a name="update"></a>Güncelleştirme

Aşağıdaki `Update` yöntemi:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

`Update` benzer `Create`, HTTP PUT kullanır. Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca deltaları göndermek istemci gerektirir. Kısmi güncelleştirmeleri desteklemek için HTTP PATCH kullanın.

"Cat yürütmek için" Yapılacaklar öğenin adını güncelleştirmek için Postman'ı kullanın:

![204 (içerik yok) yanıtı gösteren postman konsol](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Sil

Aşağıdaki `Delete` yöntemi:

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Postman yapılacak iş öğesini silmek için kullanın:

![204 (içerik yok) yanıtı gösteren postman konsol](../../tutorials/first-web-api/_static/pmd.png)
