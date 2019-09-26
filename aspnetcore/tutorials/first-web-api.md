---
title: "Öğretici: ASP.NET Core ile Web API 'SI oluşturma"
author: rick-anderson
description: ASP.NET Core ile Web API 'SI oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 5e5215f246c6c7a805a4c99f485d51a2fb3c712d
ms.sourcegitcommit: cf9ffcce4fe0b69fe795aae9ae06e99fdb18bdfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71306671"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>Öğretici: ASP.NET Core ile Web API 'SI oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)

Bu öğretici, bir web API ASP.NET Core ile oluşturmaya ilişkin temel bilgileri öğretir.

::: moniker range=">= aspnetcore-3.0"

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Web API projesi oluşturun.
> * Bir model sınıfı ve bir veritabanı bağlamı ekleyin.
> * CRUD yöntemleriyle bir denetleyiciyi dolandırın.
> * Yönlendirmeyi, URL yollarını ve dönüş değerlerini yapılandırın.
> * Web API'si Postman ile çağırın.

Sonunda, bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.

## <a name="overview"></a>Genel Bakış

Bu öğretici yandaki API oluşturur:

|API | Açıklama | İstek gövdesi | Yanıt gövdesi |
|--- | ---- | ---- | ---- |
|/Api/TodoItems al | Tüm yapılacak iş öğeleri al | Yok. | Yapılacaklar öğelerinin bir dizisi|
|/Api/TodoItems/{id} al | Bir öğeyi Kimliğine göre Al | Yok. | Yapılacak iş öğesi|
|POST/api/TodoItems | Yeni Öğe Ekle | Yapılacak iş öğesi | Yapılacak iş öğesi |
|/Api/TodoItems/{id} koy | Mevcut öğeyi güncelleştirin &nbsp; | Yapılacak iş öğesi | Yok. |
|/Api/TodoItems/{id} &nbsp; Sil&nbsp; | Öğeyi Sil &nbsp; &nbsp; | Hiçbiri | Hiçbiri|

Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.

![İstemci, sol taraftaki bir kutu ile temsil edilir. Bir istek gönderir ve sağ tarafta çizilmiş bir kutu olan uygulamadan bir yanıt alır. Uygulama kutusu içinde üç kutular, denetleyici, modeli ve veri erişim katmanı temsil eder. İstek uygulamanın denetleyicisi ile gelir ve okuma/yazma işlemleri, denetleyici ve veri erişim katmanı arasında oluşur. Model serileştirilmiş ve istemciye yanıt döndürdü.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a>Bir web projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.
* **ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.
* Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.
* **Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın. **API** şablonunu seçin ve **Oluştur**' a tıklayın.

![VS yeni proje iletişim kutusu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.
* Aşağıdaki komutları çalıştırın:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.

  Yukarıdaki komutlar:

  * Yeni bir Web API projesi oluşturur ve Visual Studio Code açar.
  * Sonraki bölümde gerekli olan NuGet paketlerini ekler.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* **Dosya** > **yeni çözüm**' ü seçin.

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* **.NET Core** > **uygulama** **API 'si** ileri ' **yi**seçin. > >

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* **Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **hedef Framework** * *.NET Core 3,0*' i seçin.

* Girin *TodoApi* için **proje adı** seçip **Oluştur**.

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

Proje klasöründe bir komut terminali açın ve aşağıdaki komutları çalıştırın:

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a>API'yi test etme

Proje şablonu oluşturur bir `WeatherForecast` API. Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/WeatherForecast`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.

IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**. İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Uygulamayı başlatmak için**hata ayıklamayı Başlat** ' **ı seçin.**  >  Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır. HTTP 404 (bulunamadı) hatası döndürülür. Append `/WeatherForecast` URL'sine (URL'yi `https://localhost:<port>/WeatherForecast`).

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

## <a name="add-a-model-class"></a>Bir model sınıfı ekleme

A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir. Tek bir modeldir bu uygulama için `TodoItem` sınıfı.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* İçinde **Çözüm Gezgini**, projeye sağ tıklayın. Seçin **ekleme** > **yeni klasör**. Klasör adı *modelleri*.

* Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**. Sınıf adı *Todoıtem* seçip **Ekle**.

* Şablon kodunu aşağıdaki kodla değiştirin:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Adlı bir klasör ekleme *modelleri*.

* Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Projeye sağ tıklayın. Seçin **ekleme** > **yeni klasör**. Klasör adı *modelleri*.

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* *Modeller* klasörüne sağ tıklayın ve **yeni dosya** > **Ekle** > **genel** > **boş sınıfı**' nı seçin.

* Sınıf adı *Todoıtem*ve ardından **yeni**.

* Şablon kodunu aşağıdaki kodla değiştirin:

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.

Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.

## <a name="add-a-database-context"></a>Veritabanı bağlamı Ekle

*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır. Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a>Microsoft. EntityFrameworkCore. SqlServer ekleyin

* **Araçlar** menüsünde **nuget Paket Yöneticisi > çözüm Için NuGet Paketlerini Yönet**' i seçin.
* **Araştır** sekmesini seçin ve arama kutusuna **Microsoft. Entityframeworkcore. SqlServer** yazın.
* Sol bölmedeki **Microsoft. EntityFrameworkCore. SqlServer** öğesini seçin.
* Sağ bölmedeki **Proje** onay kutusunu seçin ve ardından **Install**' ı seçin.
* Önceki yönergeleri kullanarak `Microsoft.EntityFrameworkCore.InMemory` NuGet paketini ekleyin.

![NuGet Paket Yöneticisi](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a>TodoContext veritabanı bağlamını ekleme

* Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**. Sınıf adı *TodoContext* tıklatıp **Ekle**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* Ekleme bir `TodoContext` sınıfının *modelleri* klasör.

---

* Aşağıdaki kodu girin:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Veritabanı bağlamı Kaydet

ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı. Kapsayıcı hizmeti denetleyicilerine sağlar.

Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

Yukarıdaki kod:

* Kullanılmayan kaldırır `using` bildirimleri.
* Veritabanı bağlamı DI kapsayıcıya ekler.
* Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.

## <a name="scaffold-a-controller"></a>Denetleyiciyi bir denetleyiciye katlama

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Sağ *denetleyicileri* klasör.
* **Yeni yapı iskelesi öğesi** **Ekle** > öğesini seçin.
* **Entity Framework kullanarak ve eylemler Içeren API denetleyicisi**' ni seçin ve ardından **Ekle**' yi seçin.
* **API denetleyiciyi eylemler Ile Ekle ' de Entity Framework** iletişim kutusunu kullanarak:

  * **Model sınıfında** **TodoItem (TodoAPI. modeller)** öğesini seçin.
  * **Veri bağlamı sınıfında** **TodoContext (TodoAPI. modeller)** öğesini seçin.
  * **Ekle** 'yi seçin

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

Aşağıdaki komutları çalıştırın:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

Yukarıdaki komutlar:

* Yapı iskelesi için gereken NuGet paketlerini ekleyin.
* Scafkatlama altyapısını (`dotnet-aspnet-codegenerator`) kurar.
* Yapı iskelesi `TodoItemsController`.

---

Oluşturulan kod:

* Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.
* Sınıfı [[Apicontroller]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle süsler. Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir. Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz <xref:web-api/index>.
* Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici. Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.

## <a name="examine-the-posttodoitem-create-method"></a>PostTodoItem Create metodunu inceleyin

İçindeki `PostTodoItem` return ifadesini, [NameOf](/dotnet/csharp/language-reference/operators/nameof) işlecini kullanmak için değiştirin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.

<xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Yöntemi:

* Başarılı olursa bir HTTP 201 durum kodu döndürür. HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.
* Yanıta bir [konum](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) üst bilgisi ekler. Üst bilgi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir. [](https://developer.mozilla.org/docs/Glossary/URI) `Location` Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* `Location` Üstbilginin URI 'sini oluşturma eylemine başvurur. `GetTodoItem` Anahtar sözcüğü, `CreatedAtAction` çağrıda eylem adının sabit kodlanmasını önlemek için kullanılır. C# `nameof`

### <a name="install-postman"></a>Postman yükleme

Bu öğreticide Postman web API'si test etmek için kullanılır.

* Yükleme [Postman](https://www.getpostman.com/downloads/)
* Web uygulaması başlatın.
* Postman'i başlatın.
* Devre dışı **SSL sertifika doğrulama**
* Gelen **Dosya > Ayarlar** (**genel* sekmesinde), devre dışı **SSL sertifika doğrulama**.
    > [!WARNING]
    > Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a>Postman ile test PostTodoItem

* Yeni bir istek oluşturun.
* HTTP yöntemini olarak `POST`ayarlayın.
* Seçin **gövdesi** sekmesi.
* Seçin **ham** radyo düğmesi.
* Tür kümesine **JSON (application/json)** .
* İstek gövdesinde bir yapılacak iş öğesi için JSON girin:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* **Gönder**’i seçin.

  ![Postman ile isteği oluştur](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a>Konum üst bilgisi URI test

* Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.
* Kopyalama **konumu** üst bilgi değeri:

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/3/create.png)

* Yöntemini GET öğesine Ayarla.
* URİ'sini yapıştırın (örneğin, `https://localhost:5001/api/TodoItems/1`)
* **Gönder**’i seçin.

## <a name="examine-the-get-methods"></a>GET yöntemlerini inceleyin

İki GET uç noktası bu yöntemleri uygulayın:

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

Tarayıcıdan veya Postman 'dan iki uç noktayı çağırarak uygulamayı test edin. Örneğin:

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

Şuna benzer bir yanıt, şu çağrı `GetTodoItems`tarafından üretilir:

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
* HTTP yöntemi kümesine **alma**.
* İstek URL'si kümesine `https://localhost:<port>/api/TodoItems`. Örneğin: `https://localhost:5001/api/TodoItems`
* Ayarlama **iki bölme görünümü** postman'deki.
* **Gönder**’i seçin.

Bu uygulama, bellek içi bir veritabanını kullanır. Uygulama durdurulup başlatılırsa, önceki GET isteği herhangi bir veri döndürmez. Hiçbir veri döndürülmezse, verileri uygulamaya [gönderin](#post) .

## <a name="routing-and-url-paths"></a>URL Yönlendirme ve yolları

[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir. Her yöntem için URL yolu şu şekilde oluşturulur:

* Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı. Bu örnek için denetleyici sınıfı adı **todoıtems**denetleyicisidir, bu nedenle denetleyicinin adı "todoıtems" olur. ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.
* Özniteliğin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin. `[HttpGet]` Bu örnek, bir şablon kullanmaz. Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir. Zaman `GetTodoItem` çağrılır, değerini `"{id}"` yöntemine URL'de sağlanan kendi`id` parametresi.

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Döndürülen değerler

Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar. Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır. İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.

`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir. Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:

* Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.
* Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür. Döndüren `item` sonuçları bir HTTP 200 yanıtı.

## <a name="the-puttodoitem-method"></a>PutTodoItem yöntemi

`PutTodoItem` Yöntemi inceleyin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır. Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir. Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Çağrılırken `PutTodoItem`bir hata alırsanız, veritabanında bir öğe `GET` olduğundan emin olmak için çağırın.

### <a name="test-the-puttodoitem-method"></a>Test PutTodoItem yöntemi

Bu örnek, uygulama her başlatıldığında yeniden başlatılması gereken bellek içi veritabanını kullanır. Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır. PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.

ID = 1 olan Yapılacaklar öğesini güncelleştirin ve adını "Feed balık" olarak ayarlayın:

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a>DeleteTodoItem yöntemi

`DeleteTodoItem` Yöntemi inceleyin:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

`DeleteTodoItem` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Test DeleteTodoItem yöntemi

Postman bir yapılacak iş öğesini silmek için kullanın:

* Yöntem kümesine `DELETE`.
* Silmek, örneğin nesnenin URI ayarlayın `https://localhost:5001/api/TodoItems/1`
* Seçin **Gönder**

## <a name="call-the-web-api-with-javascript"></a>JavaScript ile Web API 'sini çağırma

Öğreticiye bakın [: JavaScript](xref:tutorials/web-api-javascript)ile ASP.NET Core Web API 'si çağırın.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Web API projesi oluşturun.
> * Bir model sınıfı ve bir veritabanı bağlamı ekleyin.
> * Bir denetleyici ekleyeceksiniz.
> * CRUD yöntemleri ekleyin.
> * Yönlendirmeyi Yapılandırma ve URL yolu.
> * Dönüş değerleri belirtin.
> * Web API'si Postman ile çağırın.
> * JavaScript ile Web API 'sini çağırın.

Sonunda, web API'si "Yapılacaklar" öğelerini ilişkisel bir veritabanında depolanan yönetebileceği sahip.

## <a name="overview"></a>Genel Bakış

Bu öğretici yandaki API oluşturur:

|API | Açıklama | İstek gövdesi | Yanıt gövdesi |
|--- | ---- | ---- | ---- |
|/Api/TodoItems al | Tüm yapılacak iş öğeleri al | Yok. | Yapılacaklar öğelerinin bir dizisi|
|/Api/TodoItems/{id} al | Bir öğeyi Kimliğine göre Al | Yok. | Yapılacak iş öğesi|
|POST/api/TodoItems | Yeni Öğe Ekle | Yapılacak iş öğesi | Yapılacak iş öğesi |
|/Api/TodoItems/{id} koy | Mevcut öğeyi güncelleştirin &nbsp; | Yapılacak iş öğesi | Yok. |
|/Api/TodoItems/{id} &nbsp; Sil&nbsp; | Öğeyi Sil &nbsp; &nbsp; | Hiçbiri | Hiçbiri|

Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.

![İstemci, sol taraftaki bir kutu ile temsil edilir. Bir istek gönderir ve sağ tarafta çizilmiş bir kutu olan uygulamadan bir yanıt alır. Uygulama kutusu içinde üç kutular, denetleyici, modeli ve veri erişim katmanı temsil eder. İstek uygulamanın denetleyicisi ile gelir ve okuma/yazma işlemleri, denetleyici ve veri erişim katmanı arasında oluşur. Model serileştirilmiş ve istemciye yanıt döndürdü.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a>Bir web projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.
* **ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.
* Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.
* **Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 2,2** ' un seçili olduğunu doğrulayın. **API** şablonunu seçin ve **Oluştur**' a tıklayın. **Docker desteğini etkinleştir** **' i seçmeyin** .

![VS yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.
* Aşağıdaki komutları çalıştırın:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  Bu komutlar yeni bir Web API projesi oluşturur ve yeni proje klasöründe Visual Studio Code yeni bir örneğini açar.

* Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* **Dosya** > **yeni çözüm**' ü seçin.

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* **.NET Core** > **uygulama** **API 'si** ileri ' **yi**seçin. > >

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* İçinde **, yeni ASP.NET Core Web API'sini yapılandırma** iletişim kutusunda varsayılan değerleri kabul **hedef Framework'ü** , * *.NET Core 2.2*.

* Girin *TodoApi* için **proje adı** seçip **Oluştur**.

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a>API'yi test etme

Proje şablonu oluşturur bir `values` API. Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/api/values`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.

IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**. İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Uygulamayı başlatmak için**hata ayıklamayı Başlat** ' **ı seçin.**  >  Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır. HTTP 404 (bulunamadı) hatası döndürülür. Append `/api/values` URL'sine (URL'yi `https://localhost:<port>/api/values`).

---

Aşağıdaki JSON döndürülür:

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a>Bir model sınıfı ekleme

A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir. Tek bir modeldir bu uygulama için `TodoItem` sınıfı.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* İçinde **Çözüm Gezgini**, projeye sağ tıklayın. Seçin **ekleme** > **yeni klasör**. Klasör adı *modelleri*.

* Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**. Sınıf adı *Todoıtem* seçip **Ekle**.

* Şablon kodunu aşağıdaki kodla değiştirin:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Adlı bir klasör ekleme *modelleri*.

* Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Projeye sağ tıklayın. Seçin **ekleme** > **yeni klasör**. Klasör adı *modelleri*.

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* *Modeller* klasörüne sağ tıklayın ve **yeni dosya** > **Ekle** > **genel** > **boş sınıfı**' nı seçin.

* Sınıf adı *Todoıtem*ve ardından **yeni**.

* Şablon kodunu aşağıdaki kodla değiştirin:

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.

Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.

## <a name="add-a-database-context"></a>Veritabanı bağlamı Ekle

*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır. Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**. Sınıf adı *TodoContext* tıklatıp **Ekle**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* Ekleme bir `TodoContext` sınıfının *modelleri* klasör.

---

* Şablon kodunu aşağıdaki kodla değiştirin:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Veritabanı bağlamı Kaydet

ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı. Kapsayıcı hizmeti denetleyicilerine sağlar.

Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

Yukarıdaki kod:

* Kullanılmayan kaldırır `using` bildirimleri.
* Veritabanı bağlamı DI kapsayıcıya ekler.
* Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.

## <a name="add-a-controller"></a>Denetleyici ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Sağ *denetleyicileri* klasör.
* **Yeni öğe** **Ekle** > ' yi seçin.
* İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyici sınıfı** şablonu.
* Sınıf adı *TodoController*seçip **Ekle**.

  ![Yeni öğe iletişim denetleyicisiyle seçilen arama kutusu ve web API denetleyicisi Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* İçinde *denetleyicileri* klasör adında bir sınıf oluşturma `TodoController`.

---

* Şablon kodunu aşağıdaki kodla değiştirin:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Yukarıdaki kod:

* Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.
* Sınıfı [[Apicontroller]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle süsler. Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir. Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz <xref:web-api/index>.
* Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici. Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.
* Adlı bir öğe ekler `Item1` veritabanı boşsa veritabanı. Her çalıştığında bu kod oluşturucusunun içinde yeni bir HTTP isteği olduğundan. Tüm öğeleri silerseniz, oluşturucu oluşturur `Item1` API yöntemi çağrıldığında tekrar başlattığınızda. Bu nedenle, gerçekten işe yaradı silme işlemi işe yaramadı gibi görünebilir.

## <a name="add-get-methods"></a>Get yöntemleri ekleyin

Yapılacak iş öğeleri alır bir API sağlamak için aşağıdaki yöntemi ekleyin. `TodoController` sınıfı:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

İki GET uç noktası bu yöntemleri uygulayın:

* `GET /api/todo`
* `GET /api/todo/{id}`

Hala çalışıyorsa uygulamayı durdurun. Ardından, en son değişiklikleri dahil etmek için yeniden çalıştırın.

Bir tarayıcıdan iki uç nokta çağırarak uygulamayı test edin. Örneğin:

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

Şu HTTP yanıtı çağrısı tarafından üretilen `GetTodoItems`:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a>URL Yönlendirme ve yolları

[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir. Her yöntem için URL yolu şu şekilde oluşturulur:

* Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı. Bu örnek, denetleyici sınıfı adı olan **Todo**Denetleyici adı "todo" Bu nedenle denetleyicisi. ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.
* Özniteliğin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin. `[HttpGet]` Bu örnek, bir şablon kullanmaz. Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir. Zaman `GetTodoItem` çağrılır, değerini `"{id}"` yöntemine URL'de sağlanan kendi`id` parametresi.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Döndürülen değerler

Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar. Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır. İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.

`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir. Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:

* Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.
* Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür. Döndüren `item` sonuçları bir HTTP 200 yanıtı.

## <a name="test-the-gettodoitems-method"></a>Test GetTodoItems yöntemi

Bu öğreticide Postman web API'si test etmek için kullanılır.

* Yükleme [Postman](https://www.getpostman.com/downloads/)
* Web uygulaması başlatın.
* Postman'i başlatın.
* Devre dışı **SSL sertifika doğrulama**

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Dosya** ayarlarından (Genel sekmesinden) SSL sertifikası doğrulamasını devre dışı bırakın. >

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

* **Postman** > **tercihleri** ' nden (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın. Alternatif olarak, wranı seçin ve **Ayarlar**' ı seçip SSL sertifikası doğrulamasını devre dışı bırakın.

---
  
> [!WARNING]
> Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.

* Yeni bir istek oluşturun.
  * HTTP yöntemi kümesine **alma**.
  * İstek URL'si kümesine `https://localhost:<port>/api/todo`. Örneğin: `https://localhost:5001/api/todo`
* Ayarlama **iki bölme görünümü** postman'deki.
* **Gönder**’i seçin.

![Get isteğiyle postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a>Create yöntemi ekleme

`PostTodoItem` *Controllers/TodoController. cs*içindeki şu yöntemi ekleyin: 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.

`CreatedAtAction` Yöntemi:

* Başarılı olursa bir HTTP 201 durum kodu döndürür. HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.
* Yanıta bir `Location` üst bilgi ekler. `Location` Üst bilgi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir. Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* `Location` Üstbilginin URI 'sini oluşturma eylemine başvurur. `GetTodoItem` Anahtar sözcüğü, `CreatedAtAction` çağrıda eylem adının sabit kodlanmasını önlemek için kullanılır. C# `nameof`

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a>Test PostTodoItem yöntemi

* Projeyi oluşturun.
* Postman HTTP yöntemi kümesine `POST`.
* Seçin **gövdesi** sekmesi.
* Seçin **ham** radyo düğmesi.
* Tür kümesine **JSON (application/json)** .
* İstek gövdesinde bir yapılacak iş öğesi için JSON girin:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* **Gönder**’i seçin.

  ![Postman ile isteği oluştur](first-web-api/_static/create.png)

  Bir 405 yöntemine izin verilmiyor hatası alırsanız, `PostTodoItem` Yöntem eklendikten sonra projeyi derlememe sonucu büyük olasılıkla oluşur.

### <a name="test-the-location-header-uri"></a>Konum üst bilgisi URI test

* Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.
* Kopyalama **konumu** üst bilgi değeri:

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

* Yöntemini GET öğesine Ayarla.
* URİ'sini yapıştırın (örneğin, `https://localhost:5001/api/Todo/2`)
* **Gönder**’i seçin.

## <a name="add-a-puttodoitem-method"></a>PutTodoItem yöntemi ekleme

Aşağıdaki `PutTodoItem` yöntemi:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır. Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir. Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Çağrılırken `PutTodoItem`bir hata alırsanız, veritabanında bir öğe `GET` olduğundan emin olmak için çağırın.

### <a name="test-the-puttodoitem-method"></a>Test PutTodoItem yöntemi

Bu örnek, uygulama her başlatıldığında yeniden başlatılması gereken bellek içi veritabanını kullanır. Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır. PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.

Kimliğine sahip bir yapılacak iş öğesi güncelleştirme = 1 ve "balık akış için" adını girin:

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a>DeleteTodoItem yöntemi ekleme

Aşağıdaki `DeleteTodoItem` yöntemi:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`DeleteTodoItem` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Test DeleteTodoItem yöntemi

Postman bir yapılacak iş öğesini silmek için kullanın:

* Yöntem kümesine `DELETE`.
* Silmek, örneğin nesnenin URI ayarlayın `https://localhost:5001/api/todo/1`
* Seçin **Gönder**

Örnek uygulama, tüm öğeleri silmenizi sağlar. Ancak, son öğe silindiğinde, API 'nin bir sonraki çağrılışında model sınıfı Oluşturucu tarafından yeni bir tane oluşturulur.

## <a name="call-the-web-api-with-javascript"></a>JavaScript ile Web API 'sini çağırma

Bu bölümde, Web API 'sini çağırmak için JavaScript kullanan bir HTML sayfası eklenir. Fetch API 'SI isteği başlatır. JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.

Uygulamayı [statik dosyalara sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) şekilde yapılandırın ve aşağıdaki vurgulanmış kodla *Startup.cs* güncelleştirerek [varsayılan dosya eşlemesini etkinleştirin](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

Oluşturma bir *wwwroot* proje dizininde klasör.

Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin. Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin. Dosyanın içeriğini aşağıdaki kodla değiştirin:

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:

* Açık *Properties\launchSettings.json*.
* Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.

Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır. API çağrıları açıklamaları aşağıda verilmiştir.

### <a name="get-a-list-of-to-do-items"></a>Yapılacaklar öğelerinin bir listesini alın

Fetch, Web API 'sine bir HTTP GET isteği gönderir ve bu, Yapılacaklar öğeleri dizisini temsil eden JSON döndürür. `success` İstek başarılı olursa geri çağırma işlevi çağrılır. Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Yapılacak İş Öğesi Ekle

Fetch, istek gövdesinde Yapılacaklar öğesiyle bir HTTP POST isteği gönderir. `accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` gönderilen ve alınan medya türü belirtmek için. Yapılacak iş öğesi kullanarak JSON'a dönüştürülür [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). API'yi bir başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Yapılacak iş öğesini güncelleştirme

Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir. `url` Öğenin benzersiz tanıtıcısı eklemek için değişiklikleri ve `type` olduğu `PUT`.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Yapılacak iş öğesi silme

Yapılacak iş öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı hedefi üzerinde `DELETE` ve öğenin benzersiz tanıtıcısı URL'yi belirterek.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a>Web API 'sine kimlik doğrulama desteği ekleme

Bkz. [ıdentityserver4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) öğreticisi.

## <a name="additional-resources"></a>Ek kaynaklar

[Görüntülemek veya Bu öğretici için örnek kodu indirdikten](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Bkz: [nasıl indirileceğini](xref:index#how-to-download-a-sample).

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [Bu öğreticinin YouTube sürümü](https://www.youtube.com/watch?v=TTkhEyGBfAk)
