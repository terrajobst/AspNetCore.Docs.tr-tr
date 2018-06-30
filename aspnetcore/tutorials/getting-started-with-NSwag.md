---
title: NSwag ve ASP.NET Core kullanmaya başlama
author: zuckerthoben
description: Belgeleri oluşturmak ve ASP.NET Core web API'si için sayfa yardımcı olmak için NSwag kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: c0811593609b7d1e3529d5253e8b053f180281f3
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126280"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>NSwag ve ASP.NET Core kullanmaya başlama

Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](https://rsuter.com)

::: moniker range=">= aspnetcore-2.1"

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

Kullanarak [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core ile ara yazılımı gerektirir [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet paketi. Swagger kullanıcı arabirimini (v2 ve v3), bir Swagger oluşturucuyu paketi oluşur ve [ReDoc UI](https://github.com/Rebilly/ReDoc).

Yüksek oranda NSwag, kullanıcının kullanmak için önerilen code oluşturma özellikleri. Kod oluşturma için aşağıdaki seçeneklerden birini seçin:

* Kullanım [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), C# ve TypeScript API için istemci kodu oluşturmak için bir Windows masaüstü uygulaması.
* Kullanım [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) veya [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet paketlerini kod oluşturmayı projenizi içinde.
* Gelen NSwag kullanmak [komut satırı](https://github.com/NSwag/NSwag/wiki/CommandLine).
* Kullanım [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet paketi.

## <a name="features"></a>Özellikler

NSwag kullanmak için ana nedeni Swagger kullanıcı arabirimini ve Swagger yalnızca tanıtmak için yeteneğidir yapmak ancak Oluşturucu, esnek kod oluşturma özelliklerini kullanın. Varolan bir API gerekmeyen&mdash;Swagger birleştirme ve bir istemci uygulaması oluşturmak NSwag izin üçüncü taraf API'leri kullanabilirsiniz. Her iki durumda da geliştirme döngüsü öncelikli ve API değişiklikler daha kolayca uyarlayabilirsiniz.

## <a name="package-installation"></a>Paket yükleme

NSwag NuGet paketi ile aşağıdaki yaklaşımlardan eklenebilir:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Paket Yöneticisi Konsolu** penceresi:
  * Git **Görünüm** > **diğer Windows** > **Paket Yöneticisi Konsolu**
  * Dizine gidin *TodoApi.csproj* dosya var
  * Aşağıdaki komutu yürütün:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Gelen **NuGet paketlerini Yönet** iletişim:
  * ' Nde projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**
  * Ayarlama **paket kaynağı** "nuget.org" için
  * Arama kutusuna "NSwag.AspNetCore" girin
  * "NSwag.AspNetCore" paketten seçin **Gözat** sekmesinde **yükleyin**

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Sağ *paketleri* klasöründe **çözüm paneli** > **paketleri Ekle...**
* Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" için açılır
* Arama kutusuna "NSwag.AspNetCore" girin
* Sonuçlar bölmesinde "NSwag.AspNetCore" paketi seçin ve **Paketi Ekle**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki komutu çalıştırın **tümleşik Terminal**:

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

İçinde `Startup.Configure` yöntemi, oluşturulan Swagger belirtimi ve Swagger kullanıcı arabirimini hizmet veren ara yazılımı etkinleştir:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

Uygulamasını başlatın. Gidin `http://localhost:<port>/swagger` Swagger kullanıcı arabirimini görüntülemek için. Gidin `http://localhost:<port>/swagger/v1/swagger.json` Swagger belirtimi görüntülemek için.

## <a name="code-generation"></a>Kod oluşturma

### <a name="via-nswagstudio"></a>NSwagStudio

* Resmi NSwagStudio yükleme [GitHub deposunu](https://github.com/RSuter/NSwag/wiki/NSwagStudio).
* NSwagStudio başlatın. Girin *swagger.json* URL'de dosya **Swagger belirtimi URL** metin kutusuna, tıklatıp **yerel kopyasını oluştur** düğmesi.
* Seçin **CSharp istemci** istemci çıktı türü. Diğer Seçenekler **TypeScript istemci** ve **CSharp Web API denetleyicisi**. Bir Web API denetleyicisi kullanarak, aslında bir ters oluşturma. Hizmet yeniden oluşturmak için bir hizmet belirtimini kullanır.
* Tıklatın **oluşturma çıkışları** düğmesi. Bir tam C# istemci uygulaması *TodoApi.NSwag* projesi oluşturulur. Tıklatın **CSharp istemci** sekmesinde **çıkışları** bölümünde oluşturulan istemci kodu görmek için:

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
> C# istemci kodu tanımlanan ayarlara göre oluşturulan **ayarları** sekmesinde **CSharp istemci** sekmesi. Varsayılan ad alanı yeniden adlandırma ve zaman uyumlu yöntemi oluşturma gibi görevleri gerçekleştirmek için ayarları değiştirin.

* İstemci proje dosyasında oluşturulan C# kod kopyalayın (örneğin, bir [Xamarin.Forms](/xamarin/xamarin-forms/) uygulama).
* Web API kullanma başlatın:

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
> Bir temel URL'yi ve/veya HTTP istemcisi API istemcisini ekleyemezsiniz. En iyi uygulama her zaman olduğu [HttpClient yeniden](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

### <a name="other-ways-to-generate-client-code"></a>İstemci kodu oluşturmak için diğer yolları

İş akışınıza uygun istemci kodu daha fazla farklı yollarla oluşturabilirsiniz:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [Kod:](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [T4 şablonları](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a>Özelleştir

Swagger API web kullanımını kolaylaştırmak için nesne modeli belgeleme için seçenekler sağlar.

### <a name="api-info-and-description"></a>API bilgileri ve açıklaması

İçinde `Startup.Configure` yöntemi, bir yapılandırma eylemi geçirilen `UseSwagger` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:

![Swagger kullanıcı Arabirimi ile sürüm bilgileri](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>XML açıklamaları

XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilir:

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* ' Nde projeye sağ **Çözüm Gezgini** seçip **< project_name > .csproj Düzenle**.
* Vurgulanan satırlar el ile eklemeniz *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* ' Nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**
* Denetleyin **XML belge dosyası** altında kutusunda **çıkış** bölümünü **yapı** sekmesi

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Mac için Visual Studio](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* Gelen *çözüm paneli*, basın **denetim** ve proje adına tıklayın. Gidin **Araçları** > **dosyasını düzenleyin**.
* Vurgulanan satırlar el ile eklemeniz *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Açık **proje seçenekleri** iletişim > **yapı** > **derleyici**
* Denetleme **xml belgeleri oluşturmak** altında kutusunda **Genel Seçenekler** bölümü

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

Vurgulanan satırlar el ile eklemeniz *.csproj* dosyası:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a>Veri açıklamaları

::: moniker range="<= aspnetcore-2.0"
NSwag kullanan [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü web API eylemler için: [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Sonuç olarak, NSwag eyleminizi ne yaptığını ve ne döndürür gösterilemiyor. Aşağıdaki örnek göz önünde bulundurun:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Önceki eylem döndürür `IActionResult`, ancak işlem içinde ya da döndürmektir [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) veya [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Veri ek açıklamaları, istemciler bu eylemi geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanılır. Aşağıdaki özniteliklerle eylem tasarlamanız:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
NSwag kullanan [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü web API eylemler için: [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1). Sonuç olarak, NSwag tarafından tanımlanan dönüş türü yalnızca çıkarımını `T`. Diğer olası dönüş türleri eyleminde çıkarsanamıyor. Aşağıdaki örnek göz önünde bulundurun:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Önceki eylem döndürür `ActionResult<T>`, ancak işlem içinde ya da döndürmektir [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute). Denetleyici ile donatılmış beri [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) öznitelik, bir [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) olası çok olan yanıt. Bkz: [otomatik HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses) daha fazla bilgi için. Veri ek açıklamaları, istemciler bu eylemi geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanılır. Aşağıdaki özniteliklerle eylem tasarlamanız:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end

Swagger Oluşturucu artık doğru şekilde bu eylem tanımlayabilir ve uç nokta çağrılırken ne aldıkları oluşturulan istemcileri biliyorsunuz. Bu öznitelikler ile tüm eylemler dekorasyon kullanmamanız önerilir. API eylemlerinizi döndürmelidir hangi HTTP yanıtları hakkında yönergeler için bkz: [RFC 7231 belirtimi](https://tools.ietf.org/html/rfc7231#section-4.3).
