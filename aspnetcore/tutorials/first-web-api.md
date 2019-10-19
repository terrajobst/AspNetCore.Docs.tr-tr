---
title: "Öğretici: ASP.NET Core bir Web API 'SI oluşturma"
author: rick-anderson
description: ASP.NET Core ile Web API 'SI oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 09/29/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 6f2d62600da828261ecfc3a1df688ce914eccf33
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72590023"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>Öğretici: ASP.NET Core bir Web API 'SI oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike te son](https://github.com/mikewasson)

Bu öğreticide, ASP.NET Core ile Web API 'SI oluşturmanın temelleri öğretilir.

::: moniker range=">= aspnetcore-3.0"

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Web API projesi oluşturun.
> * Bir model sınıfı ve bir veritabanı bağlamı ekleyin.
> * CRUD yöntemleriyle bir denetleyiciyi dolandırın.
> * Yönlendirmeyi, URL yollarını ve dönüş değerlerini yapılandırın.
> * Postman ile Web API 'sini çağırın.

Sonunda, bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.

## <a name="overview"></a>Genel bakış

Bu öğretici aşağıdaki API 'YI oluşturur:

|API | Açıklama | İstek gövdesi | Yanıt gövdesi |
|--- | ---- | ---- | ---- |
|/Api/TodoItems al | Tüm yapılacaklar öğelerini Al | Yok. | Yapılacaklar öğeleri dizisi|
|/Api/TodoItems/{id} al | KIMLIĞE göre öğe al | Yok. | Yapılacaklar öğesi|
|POST/api/TodoItems | Yeni öğe Ekle | Yapılacaklar öğesi | Yapılacaklar öğesi |
|/Api/TodoItems/{id} koy | Mevcut bir öğeyi güncelleştirin &nbsp; | Yapılacaklar öğesi | Yok. |
|/Api/TodoItems/{id} &nbsp; SIL &nbsp; | Öğe &nbsp; &nbsp; silme | Yok. | Yok.|

Aşağıdaki diyagramda uygulamanın tasarımı gösterilmektedir.

![İstemci, sol taraftaki bir kutu ile temsil edilir. Bir istek gönderir ve sağ tarafta çizilmiş bir kutu olan uygulamadan bir yanıt alır. Uygulama kutusu içinde, üç kutu denetleyiciyi, modeli ve veri erişim katmanını temsil eder. İstek uygulamanın denetleyicisine gelir ve denetleyici ile veri erişim katmanı arasında okuma/yazma işlemleri gerçekleştirilir. Model serileştirilir ve yanıtta istemciye döndürülür.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Prerequisites

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a>Web projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.
* **ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.
* Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.
* **Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın. **API** şablonunu seçin ve **Oluştur**' a tıklayın.

![VS Yeni proje iletişim kutusu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.
* Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.
* Aşağıdaki komutları çalıştırın:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.

  Önceki komutlar:

  * Yeni bir Web API projesi oluşturur ve Visual Studio Code açar.
  * Sonraki bölümde gerekli olan NuGet paketlerini ekler.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* **Dosya** > **yeni çözüm**seçin.

  ![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

* **.NET Core** > **App** > **API** > ' **yi**seçin.

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* **Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **hedef Framework** * *.NET Core 3,0*' i seçin.

* **Proje adı** için *TodoApi* girin ve ardından **Oluştur**' u seçin.

  ![yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

Proje klasöründe bir komut terminali açın ve aşağıdaki komutları çalıştırın:

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a>API 'YI test etme

Proje şablonu bir `WeatherForecast` API 'SI oluşturur. Uygulamayı test etmek için bir tarayıcıdan `Get` yöntemini çağırın.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>/WeatherForecast` gider.

IIS Express sertifikaya güvenip güvenmemeyi soran bir iletişim kutusu alırsanız **Evet**' i seçin. Sonraki görüntülenen **güvenlik uyarısı** Iletişim kutusunda **Evet**' i seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Bir tarayıcıda aşağıdaki URL 'ye gidin: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Uygulamayı başlatmak için**hata ayıklamayı başlat**  >  **Çalıştır** ' ı seçin. Mac için Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>` ' a gider. HTTP 404 (bulunamadı) hatası döndürüldü. URL 'ye `/WeatherForecast` ekleyin (URL 'yi `https://localhost:<port>/WeatherForecast` olarak değiştirin).

---

Aşağıdakine benzer bir JSON döndürülür:

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a>Model sınıfı ekleme

*Model* , uygulamanın yönettiği verileri temsil eden bir sınıf kümesidir. Bu uygulamanın modeli, tek bir `TodoItem` sınıfıdır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, projeye sağ tıklayın. **Yeni  >  klasör** **Ekle** ' yi seçin. Klasör *modellerini*adlandırın.

* *Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin. Sınıfı *TodoItem* olarak adlandırın ve **Ekle**' yi seçin.

* Şablon kodunu şu kodla değiştirin:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* *Modeller*adlı bir klasör ekleyin.

* *Modeller* klasörüne aşağıdaki kodla bir `TodoItem` sınıfı ekleyin:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Projeye sağ tıklayın. **Yeni  >  klasör** **Ekle** ' yi seçin. Klasör *modellerini*adlandırın.

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* *Modeller* klasörüne sağ tıklayın ve > **yeni dosya** **ekle** ' yi > **genel** > **boş sınıf**' ı seçin.

* Sınıfı *TodoItem*olarak adlandırın ve ardından **Yeni**' ye tıklayın.

* Şablon kodunu şu kodla değiştirin:

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

@No__t_0 özelliği, ilişkisel bir veritabanındaki benzersiz anahtar olarak çalışır.

Model sınıfları projede herhangi bir yere gidebilir, ancak *modeller* klasörü kural tarafından kullanılır.

## <a name="add-a-database-context"></a>Veritabanı bağlamı ekleme

*Veritabanı bağlamı* , bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır. Bu sınıf, `Microsoft.EntityFrameworkCore.DbContext` sınıfından türeterek oluşturulur.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a>Microsoft. EntityFrameworkCore. SqlServer ekleyin

* **Araçlar** menüsünde **nuget Paket Yöneticisi > çözüm Için NuGet Paketlerini Yönet**' i seçin.
* **Araştır** sekmesini seçin ve arama kutusuna **Microsoft. Entityframeworkcore. SqlServer** yazın.
* Sol bölmedeki **Microsoft. EntityFrameworkCore. SqlServer** öğesini seçin.
* Sağ bölmedeki **Proje** onay kutusunu seçin ve ardından **Install**' ı seçin.
* @No__t_0 NuGet paketini eklemek için yukarıdaki yönergeleri kullanın.

![NuGet Paket Yöneticisi](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a>TodoContext veritabanı bağlamını ekleme

* *Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin. Sınıfı *TodoContext* olarak adlandırın ve **Ekle**' ye tıklayın.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* *Modeller* klasörüne bir `TodoContext` sınıfı ekleyin.

---

* Aşağıdaki kodu girin:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Veritabanı bağlamını kaydetme

ASP.NET Core, VERITABANı bağlamı gibi hizmetlerin [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısına kayıtlı olması gerekir. Kapsayıcı hizmeti denetleyicilere sağlar.

Aşağıdaki Vurgulanan kodla *Startup.cs* güncelleştirin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

Önceki kod:

* Kullanılmayan `using` bildirimlerini kaldırır.
* Veritabanı bağlamını dı kapsayıcısına ekler.
* Veritabanı bağlamının bellek içi bir veritabanını kullanacağı belirtir.

## <a name="scaffold-a-controller"></a>Denetleyiciyi bir denetleyiciye katlama

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* *Denetleyiciler* klasörüne sağ tıklayın.
* @No__t_1 **yeni yapı Iskelesi öğesi** **Ekle** ' yi seçin.
* **Entity Framework kullanarak ve eylemler Içeren API denetleyicisi**' ni seçin ve ardından **Ekle**' yi seçin.
* **API denetleyiciyi eylemler Ile Ekle ' de Entity Framework** iletişim kutusunu kullanarak:

  * **Model sınıfında** **TodoItem (TodoApi. modeller)** öğesini seçin.
  * **Veri bağlamı sınıfında** **TodoContext (TodoApi. modeller)** öğesini seçin.
  * **Ekle**' yi seçin.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

Aşağıdaki komutları çalıştırın:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

Önceki komutlar:

* Yapı iskelesi için gereken NuGet paketlerini ekleyin.
* Scafkatlama altyapısını (`dotnet-aspnet-codegenerator`) kurar.
* @No__t_0 yapı iskelesi.

---

Oluşturulan kod:

* Yöntemler olmadan bir API denetleyici sınıfı tanımlar.
* Sınıfı [[Apicontroller]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle süsler. Bu öznitelik, denetleyicinin Web API isteklerine yanıt verdiğini belirtir. Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.
* Veritabanı bağlamını (`TodoContext`) denetleyiciye eklemek için DI kullanır. Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.

## <a name="examine-the-posttodoitem-create-method"></a>PostTodoItem Create metodunu inceleyin

@No__t_0 ' deki return ifadesini, [NameOf](/dotnet/csharp/language-reference/operators/nameof) işlecini kullanacak şekilde değiştirin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

Yukarıdaki kod, [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir. Yöntemi, HTTP isteğinin gövdesinden Yapılacaklar öğesinin değerini alır.

@No__t_0 yöntemi:

* Başarılı olursa bir HTTP 201 durum kodu döndürür. HTTP 201, sunucuda yeni bir kaynak oluşturan HTTP POST yöntemi için standart yanıttır.
* Yanıta bir [konum](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) üst bilgisi ekler. @No__t_0 üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin [URI](https://developer.mozilla.org/docs/Glossary/URI) 'sini belirtir. Daha fazla bilgi için bkz. [10.2.2 201 oluşturma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* @No__t_1 üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur. C# @No__t_1 anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.

### <a name="install-postman"></a>Postman yükleme

Bu öğretici, Web API 'sini test etmek için Postman kullanır.

* [Postman](https://www.getpostman.com/downloads/) yükleme
* Web uygulamasını başlatın.
* Postman 'ı başlatın.
* **SSL sertifikası doğrulamasını** devre dışı bırak
* **Dosya** > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.
    > [!WARNING]
    > Denetleyiciyi test ettikten sonra SSL sertifikası doğrulamasını yeniden etkinleştirin.

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a>Postman ile test PostTodoItem

* Yeni bir istek oluşturun.
* HTTP yöntemini `POST` olarak ayarlayın.
* **Gövde** sekmesini seçin.
* **Ham** radyo düğmesini seçin.
* Türü **JSON (Application/JSON)** olarak ayarlayın.
* İstek gövdesinde, bir yapılacaklar öğesi için JSON girin:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* **Gönder**' i seçin.

  ![Oluşturma isteğiyle Postman](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a>Konum üst bilgisi URI 'sini test etme

* **Yanıt** bölmesinde **üstbilgiler** sekmesini seçin.
* **Konum** üst bilgisi değerini kopyalayın:

  ![Postman konsolunun üstbilgiler sekmesi](first-web-api/_static/3/create.png)

* ALıNACAK yöntemi ayarlayın.
* URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/TodoItems/1`).
* **Gönder**' i seçin.

## <a name="examine-the-get-methods"></a>GET yöntemlerini inceleyin

Bu yöntemler iki al uç noktası uygular:

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

Tarayıcıdan veya Postman 'dan iki uç noktayı çağırarak uygulamayı test edin. Örneğin:

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

Aşağıdakine benzer bir yanıt, `GetTodoItems` çağrısı tarafından üretilir:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a>Postman ile test al

* Yeni bir istek oluşturun.
* **Almak**için http yöntemini ayarlayın.
* İstek URL 'sini `https://localhost:<port>/api/TodoItems` olarak ayarlayın. Örneğin, `https://localhost:5001/api/TodoItems`.
* Postman 'da **iki bölme görünümü** ayarlayın.
* **Gönder**' i seçin.

Bu uygulama, bellek içi bir veritabanını kullanır. Uygulama durdurulup başlatılırsa, önceki GET isteği herhangi bir veri döndürmez. Hiçbir veri döndürülmezse, verileri uygulamaya [gönderin](#post) .

## <a name="routing-and-url-paths"></a>Yönlendirme ve URL yolları

[@No__t_1](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) ÖZNITELIĞI BIR HTTP GET isteğine yanıt veren bir yöntemi gösterir. Her yöntemin URL yolu şu şekilde oluşturulur:

* Denetleyicinin `Route` özniteliğinde şablon dizesiyle başlayın:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* @No__t_0, denetleyicinin adıyla değiştirin, bu kural, denetleyici sınıf adı "denetleyici" sonekidir. Bu örnek için denetleyici sınıfı adı **todoıtems**denetleyicisidir, bu nedenle denetleyicinin adı "todoıtems" olur. ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.
* @No__t_0 özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin. Bu örnek, bir şablon kullanmaz. Daha fazla bilgi için bkz. [http [fiil] öznitelikleriyle öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Aşağıdaki `GetTodoItem` yönteminde, `"{id}"` Yapılacaklar öğesinin benzersiz tanımlayıcısı için bir yer tutucu değişkenidir. @No__t_0 çağrıldığında, URL 'deki `"{id}"` değeri `id` parametresindeki yöntemine sağlanır.

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Döndürülen değerler

@No__t_0 ve `GetTodoItem` yöntemlerinin dönüş türü [ActionResult \<T > türüdür](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core nesneyi [JSON](https://www.json.org/) 'a otomatik olarak serileştirir ve yanıt ILETISININ gövdesine JSON yazar. Bu dönüş türü için yanıt kodu, işlenmemiş özel durum olmadığı varsayılarak 200 ' dir. İşlenmemiş özel durumlar 5 xx hataya çevrilir.

`ActionResult` dönüş türleri, çok çeşitli HTTP durum kodlarını temsil edebilir. Örneğin, `GetTodoItem` iki farklı durum değeri döndürebilir:

* İstenen KIMLIKLE eşleşen hiçbir öğe yoksa, yöntem 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu döndürür.
* Aksi takdirde, yöntemi bir JSON yanıt gövdesi ile 200 döndürür. @No__t_0 sonuçları bir HTTP 200 yanıtına döndürülüyor.

## <a name="the-puttodoitem-method"></a>PutTodoItem yöntemi

@No__t_0 yöntemini inceleyin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

`PutTodoItem`, HTTP PUT kullanması dışında `PostTodoItem` benzerdir. Yanıt 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP belirtimine göre bir PUT isteği, istemcinin yalnızca değişiklikleri değil, tüm güncelleştirilmiş varlığı göndermesini gerektirir. Kısmi güncelleştirmeleri desteklemek için [http Patch](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)kullanın.

@No__t_0 çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.

### <a name="test-the-puttodoitem-method"></a>PutTodoItem yöntemini test etme

Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır. Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır. PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.

ID = 1 olan Yapılacaklar öğesini güncelleştirin ve adını "Feed balık" olarak ayarlayın:

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

Aşağıdaki görüntüde Postman güncelleştirmesi gösterilmektedir:

![204 (Içerik yok) yanıtı gösteren Postman konsolu](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a>DeleteTodoItem yöntemi

@No__t_0 yöntemini inceleyin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

@No__t_0 yanıtı 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>DeleteTodoItem yöntemini test etme

Bir yapılacaklar öğesini silmek için Postman kullanın:

* Yöntemini `DELETE` olarak ayarlayın.
* Silinecek nesnenin URI 'sini ayarlayın (örneğin `https://localhost:5001/api/TodoItems/1`).
* **Gönder**' i seçin.

## <a name="call-the-web-api-with-javascript"></a>JavaScript ile Web API 'sini çağırma

Bkz. [öğretici: JavaScript ile ASP.NET Core Web API 'Si çağırma](xref:tutorials/web-api-javascript).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Web API projesi oluşturun.
> * Bir model sınıfı ve bir veritabanı bağlamı ekleyin.
> * Denetleyici ekleyin.
> * CRUD yöntemleri ekleyin.
> * Yönlendirme ve URL yollarını yapılandırın.
> * Dönüş değerlerini belirtin.
> * Postman ile Web API 'sini çağırın.
> * JavaScript ile Web API 'sini çağırın.

Sonunda, ilişkisel bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.

## <a name="overview"></a>Genel bakış

Bu öğretici aşağıdaki API 'YI oluşturur:

|API | Açıklama | İstek gövdesi | Yanıt gövdesi |
|--- | ---- | ---- | ---- |
|/Api/TodoItems al | Tüm yapılacaklar öğelerini Al | Yok. | Yapılacaklar öğeleri dizisi|
|/Api/TodoItems/{id} al | KIMLIĞE göre öğe al | Yok. | Yapılacaklar öğesi|
|POST/api/TodoItems | Yeni öğe Ekle | Yapılacaklar öğesi | Yapılacaklar öğesi |
|/Api/TodoItems/{id} koy | Mevcut bir öğeyi güncelleştirin &nbsp; | Yapılacaklar öğesi | Yok. |
|/Api/TodoItems/{id} &nbsp; SIL &nbsp; | Öğe &nbsp; &nbsp; silme | Yok. | Yok.|

Aşağıdaki diyagramda uygulamanın tasarımı gösterilmektedir.

![İstemci, sol taraftaki bir kutu ile temsil edilir. Bir istek gönderir ve sağ tarafta çizilmiş bir kutu olan uygulamadan bir yanıt alır. Uygulama kutusu içinde, üç kutu denetleyiciyi, modeli ve veri erişim katmanını temsil eder. İstek uygulamanın denetleyicisine gelir ve denetleyici ile veri erişim katmanı arasında okuma/yazma işlemleri gerçekleştirilir. Model serileştirilir ve yanıtta istemciye döndürülür.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Prerequisites

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a>Web projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.
* **ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.
* Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.
* **Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 2,2** ' un seçili olduğunu doğrulayın. **API** şablonunu seçin ve **Oluştur**' a tıklayın. **Docker desteğini etkinleştir** **' i seçmeyin** .

![VS Yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.
* Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.
* Aşağıdaki komutları çalıştırın:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  Bu komutlar yeni bir Web API projesi oluşturur ve yeni proje klasöründe Visual Studio Code yeni bir örneğini açar.

* Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* **Dosya** > **yeni çözüm**seçin.

  ![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

* **.NET Core** > **App** > **API** > ' **yi**seçin.

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* **Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, * *.NET Core 2,2*' un varsayılan **hedef çerçevesini** kabul edin.

* **Proje adı** için *TodoApi* girin ve ardından **Oluştur**' u seçin.

  ![yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a>API 'YI test etme

Proje şablonu bir `values` API 'SI oluşturur. Uygulamayı test etmek için bir tarayıcıdan `Get` yöntemini çağırın.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>/api/values` gider.

IIS Express sertifikaya güvenip güvenmemeyi soran bir iletişim kutusu alırsanız **Evet**' i seçin. Sonraki görüntülenen **güvenlik uyarısı** Iletişim kutusunda **Evet**' i seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Bir tarayıcıda aşağıdaki URL 'ye gidin: [https://localhost:5001/api/values](https://localhost:5001/api/values).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Uygulamayı başlatmak için**hata ayıklamayı başlat**  >  **Çalıştır** ' ı seçin. Mac için Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>` ' a gider. HTTP 404 (bulunamadı) hatası döndürüldü. URL 'ye `/api/values` ekleyin (URL 'yi `https://localhost:<port>/api/values` olarak değiştirin).

---

Aşağıdaki JSON döndürülür:

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a>Model sınıfı ekleme

*Model* , uygulamanın yönettiği verileri temsil eden bir sınıf kümesidir. Bu uygulamanın modeli, tek bir `TodoItem` sınıfıdır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, projeye sağ tıklayın. **Yeni  >  klasör** **Ekle** ' yi seçin. Klasör *modellerini*adlandırın.

* *Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin. Sınıfı *TodoItem* olarak adlandırın ve **Ekle**' yi seçin.

* Şablon kodunu şu kodla değiştirin:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* *Modeller*adlı bir klasör ekleyin.

* *Modeller* klasörüne aşağıdaki kodla bir `TodoItem` sınıfı ekleyin:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Projeye sağ tıklayın. **Yeni  >  klasör** **Ekle** ' yi seçin. Klasör *modellerini*adlandırın.

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* *Modeller* klasörüne sağ tıklayın ve > **yeni dosya** **ekle** ' yi > **genel** > **boş sınıf**' ı seçin.

* Sınıfı *TodoItem*olarak adlandırın ve ardından **Yeni**' ye tıklayın.

* Şablon kodunu şu kodla değiştirin:

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

@No__t_0 özelliği, ilişkisel bir veritabanındaki benzersiz anahtar olarak çalışır.

Model sınıfları projede herhangi bir yere gidebilir, ancak *modeller* klasörü kural tarafından kullanılır.

## <a name="add-a-database-context"></a>Veritabanı bağlamı ekleme

*Veritabanı bağlamı* , bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır. Bu sınıf, `Microsoft.EntityFrameworkCore.DbContext` sınıfından türeterek oluşturulur.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* *Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin. Sınıfı *TodoContext* olarak adlandırın ve **Ekle**' ye tıklayın.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* *Modeller* klasörüne bir `TodoContext` sınıfı ekleyin.

---

* Şablon kodunu şu kodla değiştirin:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Veritabanı bağlamını kaydetme

ASP.NET Core, VERITABANı bağlamı gibi hizmetlerin [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısına kayıtlı olması gerekir. Kapsayıcı hizmeti denetleyicilere sağlar.

Aşağıdaki Vurgulanan kodla *Startup.cs* güncelleştirin:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

Önceki kod:

* Kullanılmayan `using` bildirimlerini kaldırır.
* Veritabanı bağlamını dı kapsayıcısına ekler.
* Veritabanı bağlamının bellek içi bir veritabanını kullanacağı belirtir.

## <a name="add-a-controller"></a>Denetleyici ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* *Denetleyiciler* klasörüne sağ tıklayın.
* @No__t_1 **Yeni öğe** **Ekle** ' yi seçin.
* **Yeni öğe Ekle** iletişim kutusunda, **API denetleyici sınıfı** şablonunu seçin.
* Sınıfı *TodoController*olarak adlandırın ve **Ekle**' yi seçin.

  ![Arama kutusu ve Web API denetleyicisi seçiliyken denetleyiciyi içeren yeni öğe iletişim kutusu Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* *Denetleyiciler* klasöründe `TodoController` adlı bir sınıf oluşturun.

---

* Şablon kodunu şu kodla değiştirin:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Önceki kod:

* Yöntemler olmadan bir API denetleyici sınıfı tanımlar.
* Sınıfı [[Apicontroller]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle süsler. Bu öznitelik, denetleyicinin Web API isteklerine yanıt verdiğini belirtir. Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.
* Veritabanı bağlamını (`TodoContext`) denetleyiciye eklemek için DI kullanır. Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.
* Veritabanı boşsa, veritabanına `Item1` adlı bir öğe ekler. Bu kod oluşturucuda yer aldığı için, her yeni HTTP isteği olduğunda çalışır. Tüm öğeleri silerseniz, bir API yönteminin bir sonraki çağrılışında Oluşturucu `Item1` yeniden oluşturur. Bu nedenle, aslında çalışırken silme işe yaramadı gibi görünebilir.

## <a name="add-get-methods"></a>Get yöntemleri ekleme

Yapılacaklar öğelerini alan bir API sağlamak için, `TodoController` sınıfına aşağıdaki yöntemleri ekleyin:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Bu yöntemler iki al uç noktası uygular:

* `GET /api/todo`
* `GET /api/todo/{id}`

Hala çalışıyorsa uygulamayı durdurun. Ardından, en son değişiklikleri dahil etmek için yeniden çalıştırın.

Bir tarayıcıdan iki uç noktayı çağırarak uygulamayı test edin. Örneğin:

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

Aşağıdaki HTTP yanıtı `GetTodoItems` çağrısı tarafından üretilir:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a>Yönlendirme ve URL yolları

[@No__t_1](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) ÖZNITELIĞI BIR HTTP GET isteğine yanıt veren bir yöntemi gösterir. Her yöntemin URL yolu şu şekilde oluşturulur:

* Denetleyicinin `Route` özniteliğinde şablon dizesiyle başlayın:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* @No__t_0, denetleyicinin adıyla değiştirin, bu kural, denetleyici sınıf adı "denetleyici" sonekidir. Bu örnek için denetleyici sınıf adı **Todo**Controller olduğundan, denetleyici adı "Todo" olur. ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.
* @No__t_0 özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin. Bu örnek, bir şablon kullanmaz. Daha fazla bilgi için bkz. [http [fiil] öznitelikleriyle öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Aşağıdaki `GetTodoItem` yönteminde, `"{id}"` Yapılacaklar öğesinin benzersiz tanımlayıcısı için bir yer tutucu değişkenidir. @No__t_0 çağrıldığında, URL 'deki `"{id}"` değeri `id` parametresindeki yöntemine sağlanır.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Döndürülen değerler

@No__t_0 ve `GetTodoItem` yöntemlerinin dönüş türü [ActionResult \<T > türüdür](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core nesneyi [JSON](https://www.json.org/) 'a otomatik olarak serileştirir ve yanıt ILETISININ gövdesine JSON yazar. Bu dönüş türü için yanıt kodu, işlenmemiş özel durum olmadığı varsayılarak 200 ' dir. İşlenmemiş özel durumlar 5 xx hataya çevrilir.

`ActionResult` dönüş türleri, çok çeşitli HTTP durum kodlarını temsil edebilir. Örneğin, `GetTodoItem` iki farklı durum değeri döndürebilir:

* İstenen KIMLIKLE eşleşen hiçbir öğe yoksa, yöntem 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu döndürür.
* Aksi takdirde, yöntemi bir JSON yanıt gövdesi ile 200 döndürür. @No__t_0 sonuçları bir HTTP 200 yanıtına döndürülüyor.

## <a name="test-the-gettodoitems-method"></a>GetTodoItems yöntemini test etme

Bu öğretici, Web API 'sini test etmek için Postman kullanır.

* [Postman](https://www.getpostman.com/downloads/)'yi yükleme.
* Web uygulamasını başlatın.
* Postman 'ı başlatın.
* **SSL sertifikası doğrulamasını**devre dışı bırakın.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Dosya** > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* **Postman**  > **tercihleri** ' nden (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın. Alternatif olarak, wranı seçin ve **Ayarlar**' ı seçip SSL sertifikası doğrulamasını devre dışı bırakın.

---
  
> [!WARNING]
> Denetleyiciyi test ettikten sonra SSL sertifikası doğrulamasını yeniden etkinleştirin.

* Yeni bir istek oluşturun.
  * **Almak**için http yöntemini ayarlayın.
  * İstek URL 'sini `https://localhost:<port>/api/todo` olarak ayarlayın. Örneğin, `https://localhost:5001/api/todo`.
* Postman 'da **iki bölme görünümü** ayarlayın.
* **Gönder**' i seçin.

![Get isteği ile Postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a>Create yöntemi ekle

Aşağıdaki `PostTodoItem` yöntemini *Controllers/TodoController. cs*içine ekleyin: 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Yukarıdaki kod, [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir. Yöntemi, HTTP isteğinin gövdesinden Yapılacaklar öğesinin değerini alır.

@No__t_0 yöntemi:

* Başarılı olursa bir HTTP 201 durum kodu döndürür. HTTP 201, sunucuda yeni bir kaynak oluşturan HTTP POST yöntemi için standart yanıttır.
* Yanıta bir `Location` üst bilgisi ekler. @No__t_0 üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir. Daha fazla bilgi için bkz. [10.2.2 201 oluşturma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* @No__t_1 üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur. C# @No__t_1 anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a>PostTodoItem yöntemini test etme

* Projeyi oluşturun.
* Postman 'da HTTP yöntemini `POST` olarak ayarlayın.
* **Gövde** sekmesini seçin.
* **Ham** radyo düğmesini seçin.
* Türü **JSON (Application/JSON)** olarak ayarlayın.
* İstek gövdesinde, bir yapılacaklar öğesi için JSON girin:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* **Gönder**' i seçin.

  ![Oluşturma isteğiyle Postman](first-web-api/_static/create.png)

  Bir 405 yöntemine Izin verilmiyor hatası alırsanız, `PostTodoItem` yöntemi eklendikten sonra projenin derlenmesinin sonucu büyük olasılıkla oluşur.

### <a name="test-the-location-header-uri"></a>Konum üst bilgisi URI 'sini test etme

* **Yanıt** bölmesinde **üstbilgiler** sekmesini seçin.
* **Konum** üst bilgisi değerini kopyalayın:

  ![Postman konsolunun üstbilgiler sekmesi](first-web-api/_static/pmc2.png)

* ALıNACAK yöntemi ayarlayın.
* URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/Todo/2`).
* **Gönder**' i seçin.

## <a name="add-a-puttodoitem-method"></a>PutTodoItem yöntemi ekleme

Aşağıdaki `PutTodoItem` yöntemi ekleyin:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`PutTodoItem`, HTTP PUT kullanması dışında `PostTodoItem` benzerdir. Yanıt 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP belirtimine göre bir PUT isteği, istemcinin yalnızca değişiklikleri değil, tüm güncelleştirilmiş varlığı göndermesini gerektirir. Kısmi güncelleştirmeleri desteklemek için [http Patch](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)kullanın.

@No__t_0 çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.

### <a name="test-the-puttodoitem-method"></a>PutTodoItem yöntemini test etme

Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır. Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır. PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.

ID = 1 olan Yapılacaklar öğesini güncelleştirin ve adını "Feed balık" olarak ayarlayın:

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

Aşağıdaki görüntüde Postman güncelleştirmesi gösterilmektedir:

![204 (Içerik yok) yanıtı gösteren Postman konsolu](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a>DeleteTodoItem yöntemi ekleme

Aşağıdaki `DeleteTodoItem` yöntemi ekleyin:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

@No__t_0 yanıtı 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>DeleteTodoItem yöntemini test etme

Bir yapılacaklar öğesini silmek için Postman kullanın:

* Yöntemini `DELETE` olarak ayarlayın.
* Silinecek nesnenin URI 'sini ayarlayın (örneğin `https://localhost:5001/api/todo/1`).
* **Gönder**' i seçin.

Örnek uygulama, tüm öğeleri silmenizi sağlar. Ancak, son öğe silindiğinde, API 'nin bir sonraki çağrılışında model sınıfı Oluşturucu tarafından yeni bir tane oluşturulur.

## <a name="call-the-web-api-with-javascript"></a>JavaScript ile Web API 'sini çağırma

Bu bölümde, Web API 'sini çağırmak için JavaScript kullanan bir HTML sayfası eklenir. jQuery isteği başlatır. JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.

Uygulamayı [statik dosyalara sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) şekilde yapılandırın ve aşağıdaki vurgulanmış kodla *Startup.cs* güncelleştirerek [varsayılan dosya eşlemesini etkinleştirin](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

Proje dizininde bir *Wwwroot* klasörü oluşturun.

*Wwwroot* dizinine *index. HTML* adlı bir HTML dosyası ekleyin. İçeriğini aşağıdaki biçimlendirmeyle değiştirin:

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

*Wwwroot* dizinine *site. js* adlı bir JavaScript dosyası ekleyin. İçeriğini şu kodla değiştirin:

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

HTML sayfasını yerel olarak test etmek için ASP.NET Core projesinin başlatma ayarlarındaki bir değişikliğin yapılması gerekebilir:

* *Properties\launchSettings.JSON*'i açın.
* Uygulamanın *index. html* &mdash;the projenin varsayılan dosyasında açılmasını zorlamak için `launchUrl` özelliğini kaldırın.

Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır. API çağrılarının açıklamaları aşağıda verilmiştir.

### <a name="get-a-list-of-to-do-items"></a>Yapılacaklar öğelerinin bir listesini alın

jQuery, bir to-do öğesi dizisini temsil eden JSON döndüren Web API 'sine bir HTTP GET isteği gönderir. @No__t_0 geri çağırma işlevi, istek başarılı olursa çağrılır. Geri aramada, DOM, yapılacaklar bilgileriyle güncelleştirilir.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Yapılacaklar öğesi ekleme

jQuery, istek gövdesinde Yapılacaklar öğesiyle bir HTTP POST isteği gönderir. @No__t_0 ve `contentType` seçenekleri, alınan ve gönderilen medya türünü belirtmek için `application/json` olarak ayarlanır. Yapılacaklar öğesi [JSON. stringbelirt](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)kullanılarak JSON 'a dönüştürülür. API başarılı bir durum kodu döndürdüğünde, `getData` işlevi HTML tablosunu güncelleştirmek için çağrılır.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Yapılacaklar öğesini güncelleştirme

Bir yapılacaklar öğesinin güncelleştirilmesi, bir tane eklemeye benzer. @No__t_0 öğenin benzersiz tanımlayıcısını eklemek için değişir ve `type` `PUT`.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Bir yapılacaklar öğesini silme

Bir yapılacaklar öğesinin silinmesi, AJAX çağrısının `DELETE` `type` ayarlanarak ve öğenin URL 'sindeki benzersiz tanımlayıcısını belirterek gerçekleştirilir.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a>Web API 'sine kimlik doğrulama desteği ekleme

Bkz. [ıdentityserver4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) öğreticisi.

## <a name="additional-resources"></a>Ek kaynaklar

[Bu öğretici için örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Bkz. [indirme](xref:index#how-to-download-a-sample).

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [Bu öğreticinin YouTube sürümü](https://www.youtube.com/watch?v=TTkhEyGBfAk)
