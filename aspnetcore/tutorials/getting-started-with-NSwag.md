---
title: NSwag ve ASP.NET Core ile çalışmaya başlama
author: zuckerthoben
description: NSwag belgeler oluşturmak ve Yardım sayfaları için bir ASP.NET Core web API'sini kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/18/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 8af5bed1e042c4f6d83043b05084c51b3064a548
ms.sourcegitcommit: ea215df889e89db44037a6ac2f01baede0450da9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53595366"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>NSwag ve ASP.NET Core ile çalışmaya başlama

Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](https://rsuter.com)

::: moniker range=">= aspnetcore-2.1"

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

::: moniker-end

NSwag middlewares için kaydedin:

* Uygulanan web API'si için Swagger belirtimi oluşturur.
* Swagger göz atın ve web API'si test etmek için kullanıcı Arabirimi işlevi görür.

Kullanılacak [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middlewares yükleme [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet paketi. Bu pakette oluşturup Swagger belirtimi hizmet middlewares Swagger kullanıcı arabirimini (v2 ve v3) ve [ReDoc UI](https://github.com/Rebilly/ReDoc).

Ayrıca, NSwag ın yararlanması için tavsiye kod oluşturma özellikleri. Kod oluşturma özelliklerini kullanmak için aşağıdaki seçeneklerden birini seçin:

* Kullanım [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), API'niz için C# ve TypeScript istemci kodu oluşturmak için bir Windows masaüstü uygulaması.
* Kullanım [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) veya [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) kod projeniz içindeki oluşturma için NuGet paketlerini.
* NSwag gelen kullanın [komut satırı](https://github.com/NSwag/NSwag/wiki/CommandLine).
* Kullanım [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet paketi.

## <a name="features"></a>Özellikler

NSwag kullanmaya temel nedeni, yalnızca Swagger oluşturucusu ve Swagger kullanıcı arabirimini sunar, ancak ayrıca hale getirmek üzere esnek kod oluşturma özelliklerini kullanmak yeteneğidir. Mevcut bir API'ye ihtiyacınız olmayan&mdash;Swagger birleştirmek ve bir istemci uygulaması oluşturmak NSwag izin üçüncü taraf API'leri kullanabilirsiniz. Her iki durumda da, geliştirme döngüsü öncelikli ve API değişiklikleri daha kolayca uyarlayabilirsiniz.

## <a name="package-installation"></a>Paket yüklemesi

NSwag NuGet paketi ile aşağıdaki yaklaşımlardan eklenebilir:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

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

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Sağ *paketleri* klasöründe **çözüm bölmesi** > **paketleri Ekle...**
* Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" açılır menüsünü
* Arama kutusuna "NSwag.AspNetCore"
* Sonuçlar bölmesinde "NSwag.AspNetCore" paketi seçin ve tıklayın **' paket Ekle**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Şu komutu çalıştırın:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Ekleme ve Swagger ara yazılımını yapılandırma

Aşağıdaki ad alanlarında alma `Startup` sınıfı:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

İçinde `Startup.ConfigureServices` yöntemi, gerekli Swagger hizmetler kaydedin: 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

İçinde `Startup.Configure` yöntemi, oluşturulan Swagger belirtimi ve Swagger kullanıcı arabirimini v3 sunulması için Ara yazılımlarını etkinleştir:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

Uygulamayı başlatın. Gidin `http://localhost:<port>/swagger` Swagger kullanıcı arabirimini görüntülemek için. Gidin `http://localhost:<port>/swagger/v1/swagger.json` Swagger belirtimi görüntülemek için.

## <a name="code-generation"></a>Kod oluşturma

### <a name="via-nswagstudio"></a>NSwagStudio

* Resmi NSwagStudio yükleme [GitHub deposu](https://github.com/RSuter/NSwag/wiki/NSwagStudio).
* NSwagStudio başlatın. Girin *swagger.json* URL'SİNDE dosya **Swagger belirtimi URL'si** metin tıklatıp **yerel kopya oluşturmak** düğmesi.
* Seçin **CSharp istemci** istemci çıkış türü. Diğer Seçenekler **TypeScript istemci** ve **CSharp Web API denetleyicisi**. Bir Web API denetleyicisi kullanmaktır temelde ters oluşturma. Hizmet yeniden derlemek için bir hizmet belirtimini kullanır.
* Tıklayın **Oluştur çıkışları** düğmesi. Tam bir C# istemci uygulama, *TodoApi.NSwag* Proje oluşturulur. Tıklayın **CSharp istemci** sekmesinde **çıkışları** bölümünde oluşturulan istemci kodu görmek için:

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
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
> C# istemci kodu içinde tanımlanan ayarları temel alınarak oluşturulur **ayarları** sekmesinde **CSharp istemci** sekmesi. Varsayılan ad alanı yeniden adlandırma ve zaman uyumlu yöntemi oluşturma gibi görevleri gerçekleştirmek için ayarları değiştirin.

* Bir istemci projesi dosyasında oluşturulan C# kodu kopyalayın (örneğin, bir [Xamarin.Forms](/xamarin/xamarin-forms/) uygulama).
* Web API'sini kullanan başlatın:

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Temel URL ve/veya HTTP istemci API istemcisi ekleyebilir. Her zaman en iyi yöntem olacaktır [HttpClient yeniden](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

### <a name="other-ways-to-generate-client-code"></a>İstemci kodu oluşturmak için kullanabileceğiniz diğer yöntemler

Akışınız için uygun istemci kodu daha fazla farklı yollarla oluşturabilirsiniz:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [Kodda](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [T4 şablonları](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a>Özelleştir

Swagger web API'si kullanımını kolaylaştırmak için nesne modeli belgelemek için seçenekler sağlar.

### <a name="api-info-and-description"></a>API bilgisi ve açıklama

İçinde `Startup.Configure` yapılandırma eylem yöntemi, geçilen `UseSwagger` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:

![Swagger kullanıcı Arabirimi ile sürüm bilgileri](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>XML açıklamaları

XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilir:

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* Projeye sağ **Çözüm Gezgini** seçip **< project_name > .csproj Düzenle**.
* El ile vurgulanan satırları ekleyin *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Projeye sağ **Çözüm Gezgini** seçip **özellikleri**
* Denetleme **XML belge dosyası** altında kutusunda **çıkış** bölümünü **derleme** sekmesi

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Mac için Visual Studio](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* Gelen *çözüm bölmesi*, basın **denetim** ve proje adına tıklayın. Gidin **Araçları** > **dosyasını düzenleyin**.
* El ile vurgulanan satırları ekleyin *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Açık **proje seçenekleri** iletişim > **derleme** > **derleyici**
* Denetleme **xml belgeleri oluştur** altında kutusunda **genel seçenekleri** bölümü

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

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

NSwag kullanan [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü için web API eylemlerini [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Sonuç olarak, NSwag eyleminizi ne yaptığını ve hangi döndürür çıkarsanamıyor. Aşağıdaki örnek göz önünde bulundurun:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Önceki eylemi döndürür `IActionResult`, ancak eylemi içinde ya da döndürmektir [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) veya [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Veri ek açıklamaları, istemciler bu eylem geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanılır. Aşağıdaki özniteliklerle eylem süslemek:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

NSwag kullanan [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü için web API eylemlerini [actionresult öğesini\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1). Sonuç olarak, NSwag tarafından tanımlanan bir dönüş türü yalnızca çıkarımını `T`. Diğer olası dönüş türleri uygulamada çıkarsanamıyor. Aşağıdaki örnek göz önünde bulundurun:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Önceki eylemi döndürür `ActionResult<T>`. Eylem içinde döndüren [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*). Denetleyici ile donatılmış olduğundan [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) öznitelik, bir [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) mümkün çok olan yanıt. Daha fazla bilgi için [otomatik HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses). Veri ek açıklamaları, istemciler bu eylem geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanılır. Aşağıdaki özniteliklerle eylem süslemek:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

Açıkça bireysel eylemleri dekore etmeye alternatif olarak ASP.NET Core 2.2 veya sonraki sürümlerde, kuralları kullanılabilir `[ProducesResponseType]`. Daha fazla bilgi için bkz. <xref:web-api/advanced/conventions>.

::: moniker-end

Swagger Oluşturucusu artık doğru bir şekilde bu eylem tanımlayabilir ve uç noktası çağrısı yapıldığında ne aldıkları oluşturulan istemciler bildirin. Bu öznitelikleri olan tüm eylemleri dekorasyon kesinlikle önerilir. API eylemlerinizi döndürmelidir HTTP yanıtları hakkında yönergeler için bkz: [RFC 7231 belirtimi](https://tools.ietf.org/html/rfc7231#section-4.3).
