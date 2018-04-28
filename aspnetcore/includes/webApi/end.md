## <a name="implement-the-other-crud-operations"></a>Diğer CRUD işlemleri uygulama

Aşağıdaki bölümlerde `Create`, `Update`, ve `Delete` yöntemleri, denetleyiciye eklenir.

### <a name="create"></a>Create

Aşağıdakileri ekleyin `Create` yöntemi:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Önceki kod belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. [[FromBody]](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Önceki kod belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. MVC HTTP isteği gövdesinden Yapılacaklar öğesi değerini alır.
::: moniker-end

`CreatedAtRoute` Yöntemi:

* 201 yanıtı döndürür. HTTP 201 yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt'dir.
* Bir konum üstbilgisi yanıta ekler. Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir. Bkz: [10.2.2 oluşturulan 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* URL oluşturmak için "adlı rota GetTodo" kullanır. "Rota adlı GetTodo" tanımlanan `GetById`:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Postman oluşturma isteği göndermek için kullanın

* Uygulamayı başlatın.
* Postman açın.

![Postman konsol](../../tutorials/first-web-api/_static/pmc.png)

* Localhost URL'sini bağlantı noktası numarasını güncelleştirin.
* HTTP yöntem kümesine *POST*.
* Tıklatın **gövde** sekmesi.
* Seçin **ham** radyo düğmesi.
* Türü kümesine *JSON (uygulama/json)*.
* Bir istek gövdesi aşağıdaki JSON benzeyen bir Yapılacaklar öğesi girin:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Tıklatın **Gönder** düğmesi.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Yanıt tıkladıktan sonra görüntülerse **Gönder**, devre dışı **SSL sertifika doğrulaması** seçeneği. Bu altında bulunan **dosya** > **ayarları**. Tıklatın **Gönder** daha sonra tekrar ayarı devre dışı bırakma düğmesi.
::: moniker-end

Tıklatın **üstbilgileri** sekmesinde **yanıt** bölmesinde ve kopyalama **konumu** üstbilgi değeri:

![Postman konsolunun üst bilgiler sekmesi](../../tutorials/first-web-api/_static/pmc2.png)

Konum üstbilgisi URI yeni öğe erişmek için kullanılabilir.

### <a name="update"></a>Güncelleştirme

Aşağıdakileri ekleyin `Update` yöntemi:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` benzer `Create`, HTTP PUT kullanır. Yanıt [204 (No içerik)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir. Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.

Postman "kat yürütmek için" Yapılacaklar öğesi'nin adı güncelleştirmek için kullanın:

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Sil

Aşağıdakileri ekleyin `Delete` yöntemi:

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` Yanıt [204 (No içerik)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Postman Yapılacaklar öğesi silmek için kullanın:

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmd.png)
