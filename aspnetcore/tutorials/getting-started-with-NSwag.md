---
title: NSwag ve ASP.NET Core kullanmaya başlayın
author: zuckerthoben
description: ASP.NET Core Web API 'SI için belge ve yardım sayfaları oluşturmak üzere NSwag 'yi nasıl kullanacağınızı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: bd68e134fb71fd396a30ec9c674111bc8536860d
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944180"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>NSwag ve ASP.NET Core kullanmaya başlayın

, [Christoph Nienaber](https://twitter.com/zuckerthoben), [Riko Suter](https://rsuter.com)ve [bave Brock](https://twitter.com/daveabrock) tarafından

::: moniker range=">= aspnetcore-2.1"

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

::: moniker-end

NSwag aşağıdaki özellikleri sunar:

* Swagger Kullanıcı arabirimi ve Swagger oluşturucuyu kullanma özelliği.
* Esnek kod oluşturma özellikleri.

NSwag ile, var olan bir API 'ye ihtiyacınız yoktur&mdash;Swagger içeren üçüncü taraf API 'Leri kullanabilir ve bir istemci uygulamasını oluşturabilirsiniz. NSwag, geliştirme döngüsünü hızlandırın ve API değişikliklerine kolayca uyum sağlar.

## <a name="register-the-nswag-middleware"></a>NSwag ara yazılımını kaydetme

NSwag ara yazılımını şu şekilde kaydedin:

* Uygulanan Web API 'SI için Swagger belirtimini oluşturun.
* Web API 'sini taramak ve test etmek için Swagger Kullanıcı arabirimini sunar.

[Nswag](https://github.com/RicoSuter/NSwag) ASP.NET Core ara yazılımını kullanmak Için [nswag. aspnetcore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet paketini yüklemelisiniz. Bu paket Swagger belirtimini oluşturma ve sunma ara yazılımını, Swagger Kullanıcı arabirimini (v2 ve v3) ve [Redoc Kullanıcı arabirimini](https://github.com/Rebilly/ReDoc)içerir.

NSwag NuGet paketini yüklemek için aşağıdaki yaklaşımlardan birini kullanın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Paket Yöneticisi konsol** penceresinde:
  * **Diğer Windows** > **paket Yöneticisi konsolu** > **görüntüle** ' ye gidin
  * *TodoApi. csproj* dosyasının bulunduğu dizine gidin
  * Aşağıdaki komutu yürütün:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* **NuGet Paketlerini Yönet** iletişim kutusunda:
  * **NuGet paketlerini yönetmek** > **Çözüm Gezgini** ' de projeye sağ tıklayın
  * **Paket kaynağını** "NuGet.org" olarak ayarlayın
  * Arama kutusuna "NSwag. AspNetCore" yazın
  * **Araştır** sekmesinden "NSwag. aspnetcore" paketini seçin ve sonra da **yüklensin** ' e tıklayın.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Paket Ekle ' **Çözüm Bölmesi** > *paketler* klasörüne sağ tıklayın **...**
* **Paket Ekle** penceresinin **kaynak** açılan penceresini "NuGet.org" olarak ayarlayın
* Arama kutusuna "NSwag. AspNetCore" yazın
* Sonuçlar bölmesinden "NSwag. AspNetCore" paketini seçin ve **paket Ekle** ' ye tıklayın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Şu komutu çalıştırın:

```dotnetcli
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Swagger ara yazılım ekleme ve yapılandırma

Aşağıdaki adımları gerçekleştirerek ASP.NET Core uygulamanızda Swagger ekleyin ve yapılandırın:

* `Startup.ConfigureServices` yönteminde, gerekli Swagger hizmetlerini kaydedin:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* `Startup.Configure` yönteminde, oluşturulan Swagger belirtimini ve Swagger Kullanıcı arabirimini sunan ara yazılımı etkinleştirin:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* Uygulamayı başlatma. Şuraya gidin:
  * Swagger Kullanıcı arabirimini görüntülemek için `http://localhost:<port>/swagger`.
  * Swagger belirtimini görüntülemek için `http://localhost:<port>/swagger/v1/swagger.json`.

## <a name="code-generation"></a>Kod oluşturma

Aşağıdaki seçeneklerden birini seçerek NSwag 'nin kod oluşturma özelliğinden yararlanabilirsiniz:

* [NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) , veya TYPESCRIPT içinde C# API istemci kodu oluşturmak için bir Windows masaüstü uygulaması &ndash;.
* Projenizin içindeki kod üretimi için [Nswag. CodeGeneration. CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) veya [Nswag. CodeGeneration. TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet paketleri.
* [Komut satırından](https://github.com/RicoSuter/NSwag/wiki/CommandLine)NSwag.
* [Nswag. MSBuild](https://github.com/RicoSuter/NSwag/wiki/NSwag.MSBuild) NuGet paketi.
* [Unchase Openapı (Swagger) bağlı hizmeti](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) , veya TYPESCRIPT içinde C# API istemci kodu oluşturmak Için bir Visual Studio bağlı hizmeti &ndash;. Ayrıca, C# NSwag Ile openapı Hizmetleri için denetleyiciler oluşturur.

### <a name="generate-code-with-nswagstudio"></a>NSwagStudio ile kod oluşturma

* [NSwagStudio GitHub deposundaki](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio)yönergeleri izleyerek NSwagStudio 'i yükler. NSwag Release sayfasında, yükleme ve yönetici ayrıcalıkları olmadan başlatılabilen bir Xcopy sürümü indirebilirsiniz.
* NSwagStudio başlatın ve **Swagger BELIRTIM URL** 'si metin kutusuna *Swagger. JSON* dosya URL 'sini girin. Örneğin, *http://localhost:44354/swagger/v1/swagger.json* .
* Swagger belirtimin bir JSON gösterimini oluşturmak için **Yerel kopya oluştur** düğmesine tıklayın.

  ![Swagger belirtiminin yerel kopyasını oluştur](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* **Çıktılar** alanında, **CSharp Client** onay kutusuna tıklayın. Projenize bağlı olarak, **TypeScript Client** veya **CSharp Web API Controller**' ı da seçebilirsiniz. **CSharp Web API denetleyicisi**' ni seçerseniz, bir hizmet belirtimi hizmeti yeniden oluşturur ve bu da ters bir oluşturma görevi görür.
* *TodoApi. NSwag* projesinin tüm C# Istemci uygulamasını oluşturmak için **çıkış oluştur** ' a tıklayın. Oluşturulan istemci kodunu görmek için **CSharp istemci** sekmesine tıklayın:

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
> C# İstemci kodu, **Ayarlar** sekmesindeki seçimlere göre oluşturulur. varsayılan ad alanı yeniden adlandırma ve zaman uyumlu yöntem oluşturma gibi görevleri gerçekleştirmek için ayarları değiştirin.

* Oluşturulan C# kodu, istemci projesindeki bir dosyaya, API 'yi kullanacak şekilde kopyalayın.
* Web API 'sini kullanmayı Başlat:

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a>API belgelerini özelleştirme

Swagger, Web API 'sinin kullanımını kolaylaştırmak için nesne modelini belgeleme seçeneklerini sağlar.

### <a name="api-info-and-description"></a>API bilgisi ve açıklaması

`Startup.ConfigureServices` yönteminde, `AddSwaggerDocument` yöntemine geçirilen bir yapılandırma eylemi yazar, lisans ve açıklama gibi bilgileri ekler:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

Swagger Kullanıcı arabirimi, sürümün bilgilerini görüntüler:

![Sürüm bilgileriyle Swagger Kullanıcı arabirimi](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>XML açıklamaları

XML açıklamalarını etkinleştirmek için aşağıdaki adımları uygulayın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* **Çözüm Gezgini** projeye sağ tıklayın ve **>. csproj Project_Name < Düzenle**' yi seçin.
* Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* **Çözüm Gezgini** ' de projeye sağ tıklayın ve **Özellikler** ' i seçin
* **Build** sekmesinin **output** bölümünün altındaki **XML belge dosyası** kutusunu işaretleyin

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* *Çözüm bölmesi*, **Denetim** ' e basın ve proje adına tıklayın. **Araçlar** > **dosya düzenleme**' ye gidin.
* Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* **Derleme** > **derleyicisi** > **Proje seçenekleri** iletişim kutusunu açın
* **Genel Seçenekler** bölümünün altındaki **XML oluştur belge** kutusunu işaretleyin

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a>Veri açıklamaları

::: moniker range="<= aspnetcore-2.0"

NSwag [yansıma](/dotnet/csharp/programming-guide/concepts/reflection)kullandığından ve Web API 'si eylemleri için önerilen dönüş türü [ıactionresult](xref:Microsoft.AspNetCore.Mvc.IActionResult)olduğundan, eyleminiz ne yaptığını ve döndürdüğü şeyi çıkarmaz.

Aşağıdaki örnek göz önünde bulundurun:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Önceki eylem `IActionResult`döndürüyor, ancak eylemin içinde [Createdatroute](xref:System.Web.Http.ApiController.CreatedAtRoute*) veya [BadRequest](xref:System.Web.Http.ApiController.BadRequest*)döndürüyor. İstemcilere bu eylemin hangi HTTP durum kodlarına geri dönebileceği bilinmektedir olduğunu bildirmek için veri açıklamalarını kullanın. Eylemi aşağıdaki özniteliklerle işaretleyin:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

NSwag [yansıma](/dotnet/csharp/programming-guide/concepts/reflection)kullandığından ve Web API eylemleri için önerilen dönüş türü [ActionResult\<t >](xref:Microsoft.AspNetCore.Mvc.ActionResult%601)olduğundan, yalnızca `T`tarafından tanımlanan dönüş türünü çıkarabilir. Diğer olası dönüş türlerini otomatik olarak çıkarsanamıyor.

Aşağıdaki örnek göz önünde bulundurun:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Önceki eylem `ActionResult<T>`döndürür. Eylemin içinde, [Createdatroute](xref:System.Web.Http.ApiController.CreatedAtRoute*)döndürüyor. Denetleyicinin [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği olduğundan, bir [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) yanıtı da mümkündür. Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses). İstemcilere bu eylemin hangi HTTP durum kodlarına geri dönebileceği bilinmektedir olduğunu bildirmek için veri açıklamalarını kullanın. Eylemi aşağıdaki özniteliklerle işaretleyin:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

ASP.NET Core 2,2 veya üzeri sürümlerde, tek tek eylemleri `[ProducesResponseType]`birlikte açıkça dekorasyon yerine kuralları kullanabilirsiniz. Daha fazla bilgi için bkz. <xref:web-api/advanced/conventions>.

::: moniker-end

Swagger Oluşturucusu artık bu eylemi doğru bir şekilde tanımlayabilir ve üretilen istemciler, uç noktayı çağırırken ne elde ettikleri hakkında bilgi alabilir. Öneri olarak tüm eylemleri bu özniteliklerle işaretleyin.

API eylemlerinizin dönmesi gereken HTTP yanıtlarının hakkında yönergeler için bkz. [RFC 7231 belirtimi](https://tools.ietf.org/html/rfc7231#section-4.3).
