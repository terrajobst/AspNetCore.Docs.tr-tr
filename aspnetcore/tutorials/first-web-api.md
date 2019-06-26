---
title: "Öğretici: ASP.NET Core web API'si oluşturma"
author: rick-anderson
description: Bir web API ASP.NET Core ile oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 06/23/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 0f069a75868bcbb988ade3f80d1f64c2cef4e972
ms.sourcegitcommit: 763af2cbdab0da62d1f1cfef4bcf787f251dfb5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394770"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>Öğretici: ASP.NET Core web API'si oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)

Bu öğretici, bir web API ASP.NET Core ile oluşturmaya ilişkin temel bilgileri öğretir.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Web API projesi oluşturun.
> * Bir model sınıfı ekleyin.
> * Veritabanı bağlamı oluşturun.
> * Veritabanı bağlamı kaydedin.
> * Bir denetleyici ekleyeceksiniz.
> * CRUD yöntemleri ekleyin.
> * Yönlendirmeyi Yapılandırma ve URL yolu.
> * Dönüş değerleri belirtin.
> * Web API'si Postman ile çağırın.
> * JQuery ile web API'sini çağırın.

Sonunda, web API'si "Yapılacaklar" öğelerini ilişkisel bir veritabanında depolanan yönetebileceği sahip.

## <a name="overview"></a>Genel Bakış

Bu öğretici yandaki API oluşturur:

|API | Açıklama | İstek gövdesi | Yanıt gövdesi |
|--- | ---- | ---- | ---- |
|/Api/TODO Al | Tüm yapılacak iş öğeleri al | Yok. | Yapılacaklar öğelerinin bir dizisi|
|Alma/API'si/todo / {id} | Bir öğeyi Kimliğine göre Al | Yok. | Yapılacak iş öğesi|
|Todo/api/gönderin | Yeni Öğe Ekle | Yapılacak iş öğesi | Yapılacak iş öğesi |
|PUT/API'si/todo / {id} | Mevcut öğeyi güncelleştirin &nbsp; | Yapılacak iş öğesi | Hiçbiri |
|/ API'si/todo / {id} Sil &nbsp; &nbsp; | Öğeyi Sil &nbsp; &nbsp; | Hiçbiri | Hiçbiri|

Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.

![İstemci, sol taraftaki kutuyu temsil edilir. Bu, bir istek gönderir ve sağ tarafta çizilmiş bir kutu uygulamasından bir yanıt alır. Uygulama kutusu içinde üç kutular, denetleyici, modeli ve veri erişim katmanı temsil eder. İstek uygulamanın denetleyicisi ile gelir ve okuma/yazma işlemleri, denetleyici ve veri erişim katmanı arasında oluşur. Model serileştirilmiş ve istemciye yanıt döndürdü.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a>Bir web projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **dosya** menüsünde **yeni** > **proje**.
* Seçin **ASP.NET Core Web uygulaması** şablonu ve tıklatın **sonraki**.
* Projeyi adlandırın *TodoApi* tıklatıp **Oluştur**.
* İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 2.2** seçilir. Seçin **API** şablonu ve tıklatın **Oluştur**. **Yoksa** seçin **Docker desteğini etkinleştir**.

![VS yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`) proje klasörünü içeren klasör.
* Aşağıdaki komutları çalıştırın:

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  Bu komutlar, yeni bir web API projesi oluşturun ve yeni proje klasöründe Visual Studio Code yeni bir örneğini açın.

* Bir iletişim kutusu gerekli varlıklar projeye eklemek isteyip istemediğinizi seçin sorduğunda **Evet**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Seçin **dosya** > **yeni çözüm**.

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* Seçin **.NET Core uygulaması** > **ASP.NET Core Web API'sini** > **sonraki**.

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

Seçin **çalıştırma** > **ile Start Debugging** uygulamayı başlatın. Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır. HTTP 404 (bulunamadı) hatası döndürülür. Append `/api/values` URL'sine (URL'yi `https://localhost:<port>/api/values`).

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

* Sağ *modelleri* klasörü ve select **Ekle** > **yeni dosya** > **genel**  >  **Boş sınıf**.

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

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

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
* Seçin **ekleme** > **yeni öğe**.
* İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyici sınıfı** şablonu.
* Sınıf adı *TodoController*seçip **Ekle**.

  ![Yeni öğe iletişim denetleyicisiyle seçilen arama kutusu ve web API denetleyicisi Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

* İçinde *denetleyicileri* klasör adında bir sınıf oluşturma `TodoController`.

---

* Şablon kodunu aşağıdaki kodla değiştirin:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Yukarıdaki kod:

* Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.
* Sınıf ile düzenler [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliği. Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir. Özniteliği sağlayan belirli davranışları hakkında daha fazla bilgi için bkz: <xref:web-api/index>.
* Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici. Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.
* Adlı bir öğe ekler `Item1` veritabanı boşsa veritabanı. Her çalıştığında bu kod oluşturucusunun içinde yeni bir HTTP isteği olduğundan. Tüm öğeleri silerseniz, oluşturucu oluşturur `Item1` API yöntemi çağrıldığında tekrar başlattığınızda. Bu nedenle, gerçekten işe yaradı silme işlemi işe yaramadı gibi görünebilir.

## <a name="add-get-methods"></a>Get yöntemleri ekleyin

Yapılacak iş öğeleri alır bir API sağlamak için aşağıdaki yöntemi ekleyin. `TodoController` sınıfı:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

İki GET uç noktası bu yöntemleri uygulayın:

* `GET /api/todo`
* `GET /api/todo/{id}`

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
* Varsa `[HttpGet]` özniteliğine sahip bir rota şablonu (örneğin, `[HttpGet("products")]`), yolunu ekleyin. Bu örnek, bir şablon kullanmaz. Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

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
  
  * Gelen **Dosya > Ayarlar** (**genel* sekmesinde), devre dışı **SSL sertifika doğrulama**.
    > [!WARNING]
    > Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.

* Yeni bir istek oluşturun.
  * HTTP yöntemi kümesine **alma**.
  * İstek URL'si kümesine `https://localhost:<port>/api/todo`. Örneğin: `https://localhost:5001/api/todo`
* Ayarlama **iki bölme görünümü** postman'deki.
* **Gönder**’i seçin.

![Get isteğiyle postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a>Create yöntemi ekleme

Aşağıdaki `PostTodoItem` yöntemi:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.

`CreatedAtAction` Yöntemi:

* Başarılı olursa, bir HTTP 201 durum kodunu döndürür. HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.
* Ekler bir `Location` yanıt üst bilgisi. `Location` Üst bilgisi, yeni oluşturulan yapılacak iş öğesi URI'sini belirtir. Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Başvuruları `GetTodoItem` oluşturmak için eylem `Location` başlığının URI. C# `nameof` Eylem adı, sabit kodlama önlemek için kullanılan anahtar sözcüğü `CreatedAtAction` çağırın.

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

  405 bir yönteme izin verilmiyor hata alırsanız, büyük olasılıkla projeye ekledikten sonra derleme değil sonucudur `PostTodoItem` yöntemi.

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

Hata arama alırsanız `PutTodoItem`, çağrı `GET` veritabanında bir öğe var. olmak.

### <a name="test-the-puttodoitem-method"></a>Test PutTodoItem yöntemi

Bu örnek, uygulama her başlatıldığında initialed bir bellek içi veritabanı kullanır. Olmalıdır bir öğe veritabanında bir PUT çağrısı yapmadan önce. Bir öğe var. veritabanında bir PUT çağrısı yapmadan önce sağlamak üzere GET çağırın.

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

Örnek uygulamayı tüm öğeleri silmenize olanak sağlar. Bununla birlikte, son öğe silindiğinde, yeni bir API bir sonraki zamana model sınıfı Oluşturucu tarafından oluşturulur.

## <a name="call-the-api-with-jquery"></a>JQuery ile API çağırma

Bu bölümde, Web'i çağırmaya jQuery kullanan bir HTML sayfasına eklenen API. jQuery isteği başlatır ve API'nin yanıt Ayrıntıları sayfası güncelleştirir.

İçin uygulamayı yapılandırma [statik dosyaları işleme](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [varsayılan dosya eşlemesini etkinleştir](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) güncelleştirerek *Startup.cs* aşağıdaki vurgulanmış kodu:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
Oluşturma bir *wwwroot* proje dizininde klasör.
::: moniker-end

Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin. Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin. Dosyanın içeriğini aşağıdaki kodla değiştirin:

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:

* Açık *Properties\launchSettings.json*.
* Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.

JQuery almak için birkaç yolu vardır. Yukarıdaki kod parçacığında, bir CDN kitaplığı yüklenir.

Bu örnek, tüm API CRUD yöntemleri çağırır. API çağrıları açıklamaları aşağıda verilmiştir.

### <a name="get-a-list-of-to-do-items"></a>Yapılacaklar öğelerinin bir listesini alın

JQuery [ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `GET` Yapılacaklar öğelerini dizisini temsil eden JSON döndüren API isteği. `success` İstek başarılı olursa geri çağırma işlevi çağrılır. Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Yapılacak İş Öğesi Ekle

[Ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `POST` istek ile istek gövdesindeki yapılacak iş öğesi. `accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` gönderilen ve alınan medya türü belirtmek için. Yapılacak iş öğesi kullanarak JSON'a dönüştürülür [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). API'yi bir başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Yapılacak iş öğesini güncelleştirme

Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir. `url` Öğenin benzersiz tanıtıcısı eklemek için değişiklikleri ve `type` olduğu `PUT`.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Yapılacak iş öğesi silme

Yapılacak iş öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı hedefi üzerinde `DELETE` ve öğenin benzersiz tanıtıcısı URL'yi belirterek.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a>Ek kaynaklar

[Görüntülemek veya Bu öğretici için örnek kodu indirdikten](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Bkz: [nasıl indirileceğini](xref:index#how-to-download-a-sample).

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [Bu öğreticide YouTube sürümü](https://www.youtube.com/watch?v=TTkhEyGBfAk)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API projesi oluşturun.
> * Bir model sınıfı ekleyin.
> * Veritabanı bağlamı oluşturun.
> * Veritabanı bağlamı kaydedin.
> * Bir denetleyici ekleyeceksiniz.
> * CRUD yöntemleri ekleyin.
> * Yönlendirmeyi Yapılandırma ve URL yolu.
> * Dönüş değerleri belirtin.
> * Web API'si Postman ile çağırın.
> * Web arama API'si jQuery ile.

API Yardım sayfaları oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
