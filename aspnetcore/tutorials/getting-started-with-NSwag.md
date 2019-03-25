---
title: NSwag ve ASP.NET Core ile çalışmaya başlama
author: zuckerthoben
description: NSwag belgeler oluşturmak ve Yardım sayfaları için bir ASP.NET Core web API'sini kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/30/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: af07ad771c582cfad80f297748c3c1049ff4a7d6
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419413"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>NSwag ve ASP.NET Core ile çalışmaya başlama

Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben), [Riko Suter](https://rsuter.com), ve [Dave Brock](https://twitter.com/daveabrock)

::: moniker range=">= aspnetcore-2.1"

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

::: moniker-end

NSwag aşağıdaki özellikleri sunar:

* Swagger kullanıcı arabirimini ve Swagger kullanmaya özelliği Oluşturucu.
* Esnek kod oluşturma özellikleri.

NSwag kullanmaya, mevcut bir API'ye ihtiyacınız olmayan&mdash;Swagger birleştirmek ve bir istemci uygulaması oluşturacak üçüncü taraf API'leri kullanabilirsiniz. NSwag geliştirme döngüsü hızlandırmak ve API değişiklikleri kolayca uyum sağlar.

## <a name="register-the-nswag-middleware"></a>NSwag ara yazılım kaydetme

NSwag Ara yazılımıyla kaydedin:

* Uygulanan web API'si için Swagger belirtimi oluşturur.
* Swagger göz atın ve web API'si test etmek için kullanıcı Arabirimi işlevi görür.

Kullanılacak [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core ara yazılım, yükleme [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet paketi. Bu paket oluşturmak ve Swagger belirtimi, hizmet için bir ara yazılım içeren Swagger kullanıcı arabirimini (v2 ve v3) ve [ReDoc UI](https://github.com/Rebilly/ReDoc).

NSwag NuGet paketini yüklemek için aşağıdaki yaklaşımlardan birini kullanın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Paket Yöneticisi Konsolu** penceresi:
  * Git **görünümü** > **diğer Windows** > **Paket Yöneticisi Konsolu**
  * Dizin gidin *TodoApi.csproj* dosya var
  * Aşağıdaki komutu yürütün:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Gelen **NuGet paketlerini Yönet** iletişim:
  * Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**
  * Ayarlama **paket kaynağı** "nuget.org'da"
  * Arama kutusuna "NSwag.AspNetCore"
  * "NSwag.AspNetCore" paketinden seçin **Gözat** sekmesine **yükleyin**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Sağ *paketleri* klasöründe **çözüm bölmesi** > **paketleri Ekle...**
* Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" açılır menüsünü
* Arama kutusuna "NSwag.AspNetCore"
* Sonuçlar bölmesinde "NSwag.AspNetCore" paketi seçin ve tıklayın **' paket Ekle**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Şu komutu çalıştırın:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Ekleme ve Swagger ara yazılımını yapılandırma

 Ekleme ve aşağıdaki adımları gerçekleştirerek Swagger kullanarak ASP.NET Core uygulamanızı yapılandırma `Startup` sınıfı:

* Aşağıdaki ad alanlarını içeri aktarın:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

* İçinde `ConfigureServices` yöntemi, gerekli Swagger hizmetler kaydedin:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* İçinde `Configure` yöntemi, oluşturulan Swagger belirtimi ve Swagger kullanıcı arabirimini sunulması için Ara yazılımlarını etkinleştir:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* Uygulamayı başlatın. Şuraya gidin:
  * `http://localhost:<port>/swagger` Swagger kullanıcı arabirimini görüntülemek için.
  * `http://localhost:<port>/swagger/v1/swagger.json` Swagger belirtimi görüntülemek için.

## <a name="code-generation"></a>Kod oluşturma

Aşağıdaki seçeneklerden birini seçerek NSwag'ın kod oluşturma özelliklerinden gerçekleştirebilirsiniz:

* [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) &ndash; API İstemci kodu oluşturmak için bir Windows masaüstü uygulaması C# veya TypeScript.
* [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) veya [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet paketlerini projenize içinde kod oluşturma için.
* NSwag gelen [komut satırı](https://github.com/NSwag/NSwag/wiki/CommandLine).
* [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet paketi.

### <a name="generate-code-with-nswagstudio"></a>NSwagStudio ile kodu oluştur

* Yönergeleri izleyerek NSwagStudio yükleme [NSwagStudio GitHub deposu](https://github.com/RSuter/NSwag/wiki/NSwagStudio).
* NSwagStudio başlatın ve girin *swagger.json* URL'SİNDE dosya **Swagger belirtimi URL'si** metin kutusu. Örneğin, *http://localhost:44354/swagger/v1/swagger.json*.
* Tıklayın **yerel kopya oluşturmak** , Swagger belirtimi JSON temsilini üretmek için düğme.

  ![Swagger belirtimi yerel kopyasını oluşturma](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* İçinde **çıkışları** alanı tıklayın **CSharp istemci** onay kutusu. Projenize bağlı olarak ayrıca seçebilirsiniz **TypeScript istemci** veya **CSharp Web API denetleyicisi**. Seçerseniz **CSharp Web API denetleyicisi**, geriye doğru bir nesil olarak hizmet veren hizmet, hizmet belirtimi oluşturur.
* Tıklayın **Oluştur çıkışları** tam üretmek için C# istemci uygulaması *TodoApi.NSwag* proje. Oluşturulan istemci kodu görmek için tıklayın **CSharp istemci** sekmesinde:

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable

    [System.CodeDom.Compiler.GeneratedCode("NSwag", "12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "https://localhost:44354";
        private System.Net.Http.HttpClient _httpClient;
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient(System.Net.Http.HttpClient httpClient)
        {
            _httpClient = httpClient;
            _settings = new System.Lazy<Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> C# İstemci kodu seçimleri göre oluşturulan **ayarları** sekmesi. Varsayılan ad alanı yeniden adlandırma ve zaman uyumlu yöntemi oluşturma gibi görevleri gerçekleştirmek için ayarları değiştirin.

* Oluşturulan kopyalama C# API'yi kullanacak istemci projesindeki bir dosyaya kod.
* Web API'sini kullanan başlatın:

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a>API belgeleri özelleştirme

Swagger web API'si kullanımını kolaylaştırmak için nesne modeli belgelemek için seçenekler sağlar.

### <a name="api-info-and-description"></a>API bilgisi ve açıklama

İçinde `Startup.ConfigureServices` yapılandırma eylem yöntemi, geçilen `AddSwaggerDocument` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:

![Swagger kullanıcı Arabirimi ile sürüm bilgileri](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>XML açıklamaları

XML açıklamaları etkinleştirmek için aşağıdaki adımları gerçekleştirin:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* Projeye sağ **Çözüm Gezgini** seçip **< project_name > .csproj Düzenle**.
* El ile vurgulanan satırları ekleyin *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Projeye sağ **Çözüm Gezgini** seçip **özellikleri**
* Denetleme **XML belge dosyası** altında kutusunda **çıkış** bölümünü **derleme** sekmesi

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* Gelen *çözüm bölmesi*, basın **denetim** ve proje adına tıklayın. Gidin **Araçları** > **dosyasını düzenleyin**.
* El ile vurgulanan satırları ekleyin *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Açık **proje seçenekleri** iletişim > **derleme** > **derleyici**
* Denetleme **xml belgeleri oluştur** altında kutusunda **genel seçenekleri** bölümü

::: moniker-end

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

El ile vurgulanan satırları ekleyin *.csproj* dosyası:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a>Veri açıklamaları

::: moniker range="<= aspnetcore-2.0"

NSwag kullandığından [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü için web API eylemlerini [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), eyleminiz ne yaptığını ve hangi döndürür çıkarsanamıyor.

Aşağıdaki örnek göz önünde bulundurun:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Önceki eylemi döndürür `IActionResult`, ancak eylemi içinde ya da döndürmektir [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) veya [BadRequest](xref:System.Web.Http.ApiController.BadRequest*). Veri ek açıklamaları, istemciler bu eylem geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanın. Aşağıdaki özniteliklerle eylem süslemek:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 NSwag kullandığından [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü için web API eylemlerini [actionresult öğesini\<T >](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), dönüş türü tarafından tanımlanan yalnızca çıkarımını `T`. Otomatik olarak diğer olası dönüş türü çıkarsanamıyor. 

Aşağıdaki örnek göz önünde bulundurun:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Önceki eylemi döndürür `ActionResult<T>`. Eylem içinde döndüren [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*). Denetleyici ile donatılmış olduğundan [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) öznitelik, bir [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) mümkün çok olan yanıt. Daha fazla bilgi için [otomatik HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses). Veri ek açıklamaları, istemciler bu eylem geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanın. Aşağıdaki özniteliklerle eylem süslemek:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

ASP.NET Core 2.2 veya sonraki sürümlerde, tek tek eylemlerle açıkça dekorasyon yerine kuralları kullanabilirsiniz `[ProducesResponseType]`. Daha fazla bilgi için bkz. <xref:web-api/advanced/conventions>.

::: moniker-end

Swagger Oluşturucusu artık doğru bir şekilde bu eylem tanımlayabilir ve uç noktası çağrısı yapıldığında ne aldıkları oluşturulan istemciler bildirin. Bir öneri olarak, bu özniteliklere sahip tüm eylemleri süslemek.

API eylemlerinizi döndürmelidir HTTP yanıtları hakkında yönergeler için bkz: [RFC 7231 belirtimi](https://tools.ietf.org/html/rfc7231#section-4.3).
