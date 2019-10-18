---
title: "Öğretici: ASP.NET Core bir Web API 'SI oluşturma"
author: rick-anderson
description: ASP.NET Core ile Web API 'SI oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-web-api
ms.openlocfilehash: b4c88f5dc7853396448a2a6122f3652f92079e68
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72541806"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>Öğretici: ASP.NET Core bir Web API 'SI oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike te son](https://github.com/mikewasson)

Bu öğreticide, ASP.NET Core ile Web API 'SI oluşturmanın temelleri öğretilir.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Web API projesi oluşturun.
> * Bir model sınıfı ve bir veritabanı bağlamı ekleyin.
> * CRUD yöntemleriyle bir denetleyiciyi dolandırın.
> * Yönlendirmeyi, URL yollarını ve dönüş değerlerini yapılandırın.
> * Postman ile Web API 'sini çağırın.

Sonunda, bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.

## <a name="overview"></a>Genel bakış

Bu öğretici aşağıdaki uç noktaları içeren bir Web API 'SI oluşturur:

|Uç Noktası                  |Açıklama            |İstek gövdesi|Yanıt gövdesi       |
|--------------------------|-----------------------|------------|--------------------|
|/Api/TodoItems al        |Tüm yapılacaklar öğelerini Al    |Yok.        |Yapılacaklar öğeleri dizisi|
|/Api/TodoItems/{id} al   |KIMLIĞE göre öğe al      |Yok.        |Yapılacaklar öğesi          |
|POST/api/TodoItems       |Yeni öğe Ekle         |Yapılacaklar öğesi  |Yapılacaklar öğesi          |
|/Api/TodoItems/{id} koy   |Mevcut bir öğeyi güncelleştir|Yapılacaklar öğesi  |Yok.                |
|/Api/TodoItems/{id} SIL|Öğe silme         |Yok.        |Yok.                |

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

1. **Dosya** menüsünden **Yeni**  > **Proje**' yi seçin.
1. **ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.
1. Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.
1. **Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın. **API** şablonunu seçin ve **Oluştur**' a tıklayın.

![VS Yeni proje iletişim kutusu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. [Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.
1. Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.
1. Aşağıdaki komutları çalıştırın:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

1. Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.

  Önceki komutlar:

  * Yeni bir Web API projesi oluşturun ve Visual Studio Code açın.
  * Sonraki bölümde gerekli olan NuGet paketlerini ekleyin.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

1. **Yeni çözüm** >  **Dosya** ' yı seçin.

    ![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

1. **.NET Core**  > **App**  > **API**  >  '**yi**seçin.

    ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
1. **Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **hedef Framework** * *.NET Core 3,0*' i seçin.
1. **Proje adı** için *TodoApi* girin ve ardından **Oluştur**' u seçin.

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

Uygulamayı çalıştırmak için <kbd>CTRL + F5</kbd> tuşlarına basın. Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>/WeatherForecast` gider.

IIS Express sertifikaya güvenip güvenmemeyi soran bir iletişim kutusu alırsanız **Evet**' i seçin. Sonraki görüntülenen **güvenlik uyarısı** Iletişim kutusunda **Evet**' i seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uygulamayı çalıştırmak için <kbd>CTRL + F5</kbd> tuşlarına basın. Bir tarayıcıda aşağıdaki URL 'ye gidin: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).

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

1. **Çözüm Gezgini**, projeye sağ tıklayın. **Yeni  >  klasör** **Ekle** ' yi seçin. Klasör *modellerini*adlandırın.
1. *Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin. Sınıfı *TodoItem* olarak adlandırın ve **Ekle**' yi seçin.
1. Şablon kodunu şu kodla değiştirin:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. *Modeller*adlı bir klasör ekleyin.
1. *Modeller* klasörüne aşağıdaki kodla bir `TodoItem` sınıfı ekleyin:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

1. Projeye sağ tıklayın. **Yeni  >  klasör** **Ekle** ' yi seçin. Klasör *modellerini*adlandırın.

    ![Yeni klasör](first-web-api-mac/_static/folder.png)

1. *Modeller* klasörüne sağ tıklayın ve  > **yeni dosya** **ekle** ' yi  > **genel**  > **boş sınıf**' ı seçin.
1. Sınıfı *TodoItem*olarak adlandırın ve ardından **Yeni**' ye tıklayın.
1. Şablon kodunu şu kodla değiştirin:

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

@No__t_0 özelliği, ilişkisel bir veritabanındaki benzersiz anahtar olarak çalışır.

Model sınıfları projede herhangi bir yere gidebilir, ancak *modeller* klasörü kural tarafından kullanılır.

## <a name="add-a-database-context"></a>Veritabanı bağlamı ekleme

*Veritabanı bağlamı* , bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır. Bu sınıf, `Microsoft.EntityFrameworkCore.DbContext` sınıfından türeterek oluşturulur.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a>Microsoft. EntityFrameworkCore. SqlServer ekleyin

1. **Araçlar** menüsünde **nuget Paket Yöneticisi > çözüm Için NuGet Paketlerini Yönet**' i seçin.
1. **Araştır** sekmesini seçin ve arama kutusuna **Microsoft. Entityframeworkcore. SqlServer** yazın.
1. Sol bölmedeki **Microsoft. EntityFrameworkCore. SqlServer** öğesini seçin.
1. Sağ bölmedeki **Proje** onay kutusunu seçin ve ardından **Install**' ı seçin.
1. @No__t_0 NuGet paketini eklemek için yukarıdaki yönergeleri kullanın.

![NuGet Paket Yöneticisi](first-web-api/_static/vs3nuget.png)

## <a name="add-the-todocontext-database-context"></a>TodoContext veritabanı bağlamını ekleme

* *Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin. Sınıfı *TodoContext* olarak adlandırın ve **Ekle**' ye tıklayın.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* *Modeller* klasörüne bir `TodoContext` sınıfı ekleyin.

---

* Aşağıdaki kodu girin:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Veritabanı bağlamını kaydetme

ASP.NET Core, veritabanı bağlamı gibi hizmetlerin [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısına kayıtlı olması gerekir. Kapsayıcı hizmeti denetleyicilere sağlar.

Aşağıdaki Vurgulanan kodla *Startup.cs* güncelleştirin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

Önceki kod:

* Kullanılmayan `using` bildirimlerini kaldırır.
* Veritabanı bağlamını dı kapsayıcısına ekler.
* Veritabanı bağlamının bellek içi bir veritabanını kullanacağı belirtir.

## <a name="scaffold-a-controller"></a>Denetleyiciyi bir denetleyiciye katlama

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. *Denetleyiciler* klasörüne sağ tıklayın.
1. @No__t_1**yeni yapı Iskelesi öğesi** **Ekle** ' yi seçin.
1. **Entity Framework kullanarak ve eylemler Içeren API denetleyicisi**' ni seçin ve ardından **Ekle**' yi seçin.
1. **API denetleyiciyi eylemler Ile Ekle ' de Entity Framework** iletişim kutusunu kullanarak:
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
* Sınıfı [[Apicontroller]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliğiyle süsler. Bu öznitelik, denetleyicinin Web API isteklerine yanıt verdiğini belirtir. Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.
* Veritabanı bağlamını (`TodoContext`) denetleyiciye eklemek için DI kullanır. Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.

## <a name="examine-the-posttodoitem-create-method"></a>PostTodoItem Create metodunu inceleyin

@No__t_0 ' deki return ifadesini, [NameOf](/dotnet/csharp/language-reference/operators/nameof) işlecini kullanacak şekilde değiştirin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

Yukarıdaki kod, [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir. Yöntemi, HTTP isteğinin gövdesinden Yapılacaklar öğesinin değerini alır.

@No__t_0 yöntemi:

* Başarılı olursa bir HTTP 201 durum kodu döndürür. HTTP 201, sunucuda yeni bir kaynak oluşturan HTTP POST yöntemi için standart yanıttır.
* Yanıta bir [konum](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) üst bilgisi ekler. @No__t_0 üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin [URI](https://developer.mozilla.org/docs/Glossary/URI) 'sini belirtir. Daha fazla bilgi için bkz. [10.2.2 201 oluşturma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* @No__t_1 üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur. C# @No__t_1 anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.

### <a name="install-postman"></a>Postman yükleme

Bu öğretici, Web API 'sini test etmek için Postman kullanır.

1. [Postman](https://www.getpostman.com/downloads/) yükleme
1. Web uygulamasını başlatın.
1. Postman 'ı başlatın.
1. **SSL sertifikası doğrulamasını** devre dışı bırak
1. **Dosya**  > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.

    > [!WARNING]
    > Denetleyiciyi test ettikten sonra SSL sertifikası doğrulamasını yeniden etkinleştirin.

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a>Postman ile test PostTodoItem

1. Yeni bir istek oluşturun.
1. HTTP yöntemini `POST` olarak ayarlayın.
1. **Gövde** sekmesini seçin.
1. **Ham** radyo düğmesini seçin.
1. Türü **JSON (Application/JSON)** olarak ayarlayın.
1. İstek gövdesinde, bir yapılacaklar öğesi için JSON girin:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

1. **Gönder**' i seçin.

  ![Oluşturma isteğiyle Postman](first-web-api/_static/create.png)

### <a name="test-the-location-header-uri"></a>Konum üst bilgisi URI 'sini test etme

1. **Yanıt** bölmesinde **üstbilgiler** sekmesini seçin.
1. **Konum** üst bilgisi değerini kopyalayın:

    ![Postman konsolunun üstbilgiler sekmesi](first-web-api/_static/create.png)

1. ALıNACAK yöntemi ayarlayın.
1. URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/TodoItems/1`).
1. **Gönder**' i seçin.

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

1. Yeni bir istek oluşturun.
1. **Almak**için http yöntemini ayarlayın.
1. İstek URL 'sini `https://localhost:<port>/api/TodoItems` olarak ayarlayın. Örneğin, `https://localhost:5001/api/TodoItems`.
1. Postman 'da **iki bölme görünümü** ayarlayın.
1. **Gönder**' i seçin.

Bu uygulama, bellek içi bir veritabanını kullanır. Uygulama durdurulup başlatılırsa, önceki GET isteği herhangi bir veri döndürmez. Hiçbir veri döndürülmezse, verileri uygulamaya [gönderin](#post) .

## <a name="routing-and-url-paths"></a>Yönlendirme ve URL yolları

[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) ÖZNITELIĞI BIR HTTP GET isteğine yanıt veren bir yöntemi gösterir. Her yöntemin URL yolu şu şekilde oluşturulur:

* Denetleyicinin `Route` özniteliğinde şablon dizesiyle başlayın:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* @No__t_0, denetleyicinin adıyla değiştirin, bu kural, denetleyici sınıf adı "denetleyici" sonekidir. Bu örnek için denetleyici sınıfı adı **todoıtems**denetleyicisidir, bu nedenle denetleyicinin adı "todoıtems" olur. ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.
* @No__t_0 özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin. Bu örnek, bir şablon kullanmaz. Daha fazla bilgi için bkz. [http [fiil] öznitelikleriyle öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Aşağıdaki `GetTodoItem` yönteminde, `"{id}"` Yapılacaklar öğesinin benzersiz tanımlayıcısı için bir yer tutucu değişkenidir. @No__t_0 çağrıldığında, URL 'deki `"{id}"` değeri `id` parametresindeki yöntemine sağlanır.

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Döndürülen değerler

@No__t_0 ve `GetTodoItem` yöntemlerinin dönüş türü [ActionResult \<T > türüdür](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core nesneyi [JSON](https://www.json.org/) 'a otomatik olarak serileştirir ve yanıt ILETISININ gövdesine JSON yazar. Bu dönüş türü için yanıt kodu, işlenmemiş özel durum olmadığı varsayılarak 200 ' dir. İşlenmemiş özel durumlar 5 xx hataya çevrilir.

`ActionResult` dönüş türleri, çok çeşitli HTTP durum kodlarını temsil edebilir. Örneğin, `GetTodoItem` iki farklı durum değeri döndürebilir:

* İstenen KIMLIKLE eşleşen hiçbir öğe yoksa, yöntem 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound> hata kodu döndürür.
* Aksi takdirde, yöntemi bir JSON yanıt gövdesi ile 200 döndürür. @No__t_0 sonuçları bir HTTP 200 yanıtına döndürülüyor.

## <a name="the-puttodoitem-method"></a>PutTodoItem yöntemi

@No__t_0 yöntemini inceleyin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

`PutTodoItem`, HTTP PUT kullanması dışında `PostTodoItem` benzerdir. Yanıt 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP belirtimine göre bir PUT isteği, istemcinin yalnızca değişiklikleri değil, tüm güncelleştirilmiş varlığı göndermesini gerektirir. Kısmi güncelleştirmeleri desteklemek için [http Patch](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)kullanın.

@No__t_0 çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.

### <a name="test-the-puttodoitem-method"></a>PutTodoItem yöntemini test etme

Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır. Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır. PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.

KIMLIĞI 1 olan Yapılacaklar öğesini güncelleştirin. Adını "Feed balık" olarak ayarlayın:

```json
{
  "ID":1,
  "name":"feed fish",
  "isComplete":true
}
```

Aşağıdaki görüntüde Postman güncelleştirmesi gösterilmektedir:

![204 (Içerik yok) yanıtı gösteren Postman konsolu](first-web-api/_static/pmcput.png)

## <a name="the-deletetodoitem-method"></a>DeleteTodoItem yöntemi

@No__t_0 yöntemini inceleyin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

@No__t_0 yanıtı 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>DeleteTodoItem yöntemini test etme

Bir yapılacaklar öğesini silmek için Postman kullanın:

1. Yöntemini `DELETE` olarak ayarlayın.
1. Silinecek nesnenin URI 'sini ayarlayın (örneğin, `https://localhost:5001/api/TodoItems/1`).
1. **Gönder**' i seçin.

## <a name="call-the-web-api-with-javascript"></a>JavaScript ile Web API 'sini çağırma

Bkz. [öğretici: JavaScript ile ASP.NET Core Web API 'Si çağırma](xref:tutorials/web-api-javascript).

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
