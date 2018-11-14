---
title: Swashbuckle'ı ve ASP.NET Core ile çalışmaya başlama
author: zuckerthoben
description: Swagger kullanıcı arabirimini tümleştirmek için ASP.NET Core web API projesi için Swashbuckle eklemeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/14/2018
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 9832e1ea2b59085b6680820469b16d549f4b0582
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635348"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a>Swashbuckle'ı ve ASP.NET Core ile çalışmaya başlama

Tarafından [Shayne boyer'ın](https://twitter.com/spboyer) ve [Scott Addie](https://twitter.com/Scott_Addie)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Swashbuckle'ı için üç ana bileşeni vardır:

* [Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger nesne modeli ve kullanıma sunmak için bir ara yazılım `SwaggerDocument` JSON uç noktalar olarak nesneleri.

* [Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): oluşturan bir Swagger Oluşturucusu `SwaggerDocument` nesneleri doğrudan, rotalara, denetleyicilere ve modeller. Bu genellikle otomatik olarak Swagger JSON kullanıma sunmak için Swagger uç nokta ara yazılımı ile birleştirilir.

* [Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): katıştırılmış bir Swagger kullanıcı arabirimini aracı sürümü. Bu Web API'SİNİN işlevselliğini tanımlamak için zengin, özelleştirilebilir bir deneyim oluşturmak için Swagger JSON yorumlar. Genel metotlar için yerleşik test harnesses içerir.

## <a name="package-installation"></a>Paket yüklemesi

Swashbuckle'ı ile aşağıdaki yaklaşımlardan eklenebilir:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Paket Yöneticisi Konsolu** penceresi:
  * Git **görünümü** > **diğer Windows** > **Paket Yöneticisi Konsolu**
  * Dizin gidin *TodoApi.csproj* dosya var
  * Aşağıdaki komutu yürütün:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* Gelen **NuGet paketlerini Yönet** iletişim:
  * Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**
  * Ayarlama **paket kaynağı** "nuget.org'da"
  * Arama kutusuna "Swashbuckle.AspNetCore"
  * "Swashbuckle.AspNetCore" paketinden seçin **Gözat** sekmesine **yükleyin**

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Sağ *paketleri* klasöründe **çözüm bölmesi** > **paketleri Ekle...**
* Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" açılır menüsünü
* Arama kutusuna "Swashbuckle.AspNetCore"
* Sonuçlar bölmesinde "Swashbuckle.AspNetCore" paketi seçin ve tıklayın **' paket Ekle**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Şu komutu çalıştırın:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Ekleme ve Swagger ara yazılımını yapılandırma

Swagger oluşturucusunu Hizmetleri koleksiyona eklemek `Startup.ConfigureServices` yöntemi:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

Kullanmak için aşağıdaki ad alanı içe `Info` sınıfı:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

İçinde `Startup.Configure` yöntemi, oluşturulan JSON belgesini ve Swagger kullanıcı arabirimini sunulması için Ara yazılımlarını etkinleştir:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

Önceki `UseSwaggerUI` yöntem çağrısının sağlayan [statik dosya ara yazılımlarını](xref:fundamentals/static-files). .NET Framework veya .NET targeting, Core 1.x sürümüne, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet paketini projeye.

Uygulamayı başlatın ve gidin `http://localhost:<port>/swagger/v1/swagger.json`. Uç noktaları açıklayan oluşturulan belgenin gösterildiği görünür [Swagger belirtimi (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).

Swagger kullanıcı arabirimini şu yolda bulunabilir: `http://localhost:<port>/swagger`. Swagger kullanıcı Arabirimi API'yi keşfedin ve diğer programlarda dahil edilip derecelendirilir.

> [!TIP]
> Swagger kullanıcı arabirimini uygulamanın kök dizininde hizmet (`http://localhost:<port>/`) ayarlayın `RoutePrefix` boş bir dize özelliğini:
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

Sanal dizinler (IIS veya örneğin bir ters proxy ile) kullanarak ayarlarsanız Swagger uç nokta kullanarak bir göreli yol `./` önek. Örneğin: `./swagger/v1/swagger.json` Kullanarak `/swagger/v1/swagger.json` URL (artı kullandıysanız rota öneki) true kökünde JSON dosyasını bulmak için uygulama bildirir. Örneğin, `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` yerine `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.

## <a name="customize-and-extend"></a>Özelleştirme ve genişletme

Swagger temanızı eşleştirmek için nesne modeli belgelemekten ve kullanıcı arabirimini özelleştirme seçenekleri sağlar.

### <a name="api-info-and-description"></a>API bilgisi ve açıklama

Yapılandırma eylemi geçirilen `AddSwaggerGen` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:

![Swagger kullanıcı Arabirimi ile sürüm bilgisi: açıklama, yazar ve daha fazla bağlantıya göz atın](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML açıklamaları

XML açıklamaları aşağıdaki yaklaşımlardan ile etkin hale getirilebilir:

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* Projeye sağ **Çözüm Gezgini** seçip **< project_name > .csproj Düzenle**.
* El ile vurgulanan satırları ekleyin *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Projeye sağ **Çözüm Gezgini** seçip **özellikleri**.
* Denetleme **XML belge dosyası** altında kutusunda **çıkış** bölümünü **derleme** sekmesi.

::: moniker-end

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* Gelen *çözüm bölmesi*, basın **denetim** ve proje adına tıklayın. Gidin **Araçları** > **dosyasını düzenleyin**.
* El ile vurgulanan satırları ekleyin *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Açık **proje seçenekleri** iletişim > **derleme** > **derleyici**
* Denetleme **xml belgeleri oluştur** altında kutusunda **genel seçenekleri** bölümü

::: moniker-end

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

El ile vurgulanan satırları ekleyin *.csproj* dosyası:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

El ile vurgulanan satırları ekleyin *.csproj* dosyası:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

XML açıklamaları'nı etkinleştirme, belgelenmemiş genel türler ve üyeler için hata ayıklama bilgileri sağlar. Belgelenmemiş türleri ve üyeleri tarafından uyarı iletisinde gösterilir. Örneğin, aşağıdaki ileti uyarı kodu 1591 ihlalini gösterir:

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

Proje genelinde uyarıları bastırmak için proje dosyasında yok saymak için uyarı kodlarının noktalı virgülle ayrılmış bir listesini tanımlar. Ekleme için uyarı kodlarının `$(NoWarn);` C# varsayılan değerleri çok uygular.

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

Yalnızca belirli üyeleri için uyarıları bastırmak için kod içine [#pragma Uyarısı](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) önişlemci yönergeleri. Bu yaklaşım, API belgeleri kullanıma sunulan olmamalıdır kod için kullanışlıdır. Aşağıdaki örnekte, tüm uyarı kodu CS1591 göz ardı edilir `Program` sınıfı. Zorlama uyarı kodun sınıf tanımının kapanışında geri yüklenir. Birden çok uyarı kodlarının virgülle ayrılmış bir liste belirtin.

```csharp
namespace TodoApi
{
#pragma warning disable CS1591
    public class Program
    {
        public static void Main(string[] args) =>
            BuildWebHost(args).Run();

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
#pragma warning restore CS1591
}
```

Oluşturulan XML dosyasını kullanmak için Swagger'ı yapılandırın. Linux veya Windows olmayan işletim sistemleri için dosya adlarını ve yollarını büyük küçük harfe duyarlı olabilir. Örneğin, bir *TodoApi.XML* dosya, Windows ancak değil CentOS geçerlidir.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

Önceki kodda, [yansıma](/dotnet/csharp/programming-guide/concepts/reflection) Web API projesi, eşleşen bir XML dosya adı oluşturmak için kullanılır. [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) özelliği, bir XML dosyasının yolu oluşturmak için kullanılır.

Swagger kullanıcı arabirimini üç eğik çizgi açıklama eklemek için bir eylem için bölüm başlığı açıklama ekleyerek geliştirir. Ekleme bir [ \<Özet >](/dotnet/csharp/programming-guide/xmldoc/summary) öğesi yukarıdaki `Delete` eylem:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

Önceki kodun iç metni Swagger kullanıcı arabirimini görüntüler `<summary>` öğesi:

![Swagger 'Belirli bir Todoıtem siler.' XML açıklama gösteren bir kullanıcı Arabirimi DELETE yöntemi için](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

Kullanıcı Arabirimi tarafından oluşturulan JSON şema yönetilir:

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

Ekleme bir [ \<remarks >](/dotnet/csharp/programming-guide/xmldoc/remarks) öğesine `Create` eylem yöntemi belgeleri. Belirtilen bilgilerini tamamlayan `<summary>` öğesi ve daha güçlü bir Swagger kullanıcı Arabirimi sağlar. `<remarks>` Metin, JSON veya XML öğesi içeriği oluşabilir.

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

Bu ek açıklamaları olan kullanıcı Arabirimi geliştirmeleri dikkat edin:

![Swagger kullanıcı Arabirimi ile gösterilen ek açıklamaları](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Veri açıklamaları

Öznitelikler bulundu, modeliyle süslemek [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) Swagger kullanıcı Arabirimi bileşenleri sürücü yardımcı olması için ad alanı.

Ekleme `[Required]` özniteliğini `Name` özelliği `TodoItem` sınıfı:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

Bu öznitelik varlığını UI davranışını değiştirir ve temel alınan JSON şemasını değiştirir:

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

Ekleme `[Produces("application/json")]` özniteliği için API denetleyicisi. Denetleyicinin eylemleri bir yanıt içerik türü desteği bildirmek için amacı olan *application/json*:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

**Yanıt içerik türü** açılan denetleyicinin GET eylemler için varsayılan olarak bu içerik türünü seçer:

![Swagger kullanıcı Arabirimi ile varsayılan yanıt içerik türü](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Web API'sindeki veri ek açıklamaları kullanımı arttıkça, UI ve API tarafından Yardım sayfaları daha açıklayıcı ve yararlı olur.

### <a name="describe-response-types"></a>Yanıt türleri açıklanmaktadır

Kullanan geliştiriciler hangi iade ile en ilgili&mdash;özellikle yanıt türleri ve hata kodları (standart varsa). Hata kodları ve yanıt türleri XML açıklamaları ve verileri ek açıklamalar içinde belirtilir.

`Create` Eylem başarılı olduğunda bir HTTP 201 durum kodunu döndürür. Gönderilen istek gövdesi null olduğunda, bir HTTP 400 durum kodu döndürülür. Swagger kullanıcı arabirimini uygun belgelerinde tüketici bu beklenen sonuçları bilgisine sahip değil. Aşağıdaki örnekte vurgulanan satırları ekleyerek bu sorunu düzeltin:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

Swagger kullanıcı arabirimini açıkça beklenen HTTP yanıt kodları artık belgeler:

![Swagger kullanıcı Arabirimi POST yanıt sınıf açıklaması 'yeni oluşturulan bir Todo öğesini döndürür' gösteren ve '400 - öğe null ise' durum kodu ve yanıt iletilerinin altında nedeni](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a>Kullanıcı arabirimini özelleştirme

Hem işlevsel hem de edileni hisse senedi kullanıcı Arabirimi. Ancak, API belgeleri sayfaları markanız veya tema temsil etmelidir. Swashbuckle bileşenleri marka statik dosyaları sunmak için kaynak ekleme ve bu dosyaları barındırmak için klasör yapısını oluşturma gerektirir.

.NET Framework veya .NET targeting, Core 1.x sürümüne, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet paketini projeye:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Önceki NuGet paketini, .NET Core'u hedefleyen zaten yüklü 2.x ve kullanarak [metapackage](xref:fundamentals/metapackage).

Statik dosya ara yazılımlarını etkinleştir:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

İçeriğini alma *dist* klasöründen [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui/tree/master/dist). Bu klasör, Swagger kullanıcı Arabirimi sayfası için gerekli varlıkları içerir.

Oluşturma bir *wwwroot/swagger/UI* klasörü ve içeriği dosyaya kopyalayın *dist* klasör.

Oluşturma bir *custom.css* dosyasındaki *wwwroot/swagger/UI*, sayfa üstbilgisindeki özelleştirmek için aşağıdaki CSS ile:

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

Başvuru *custom.css* içinde *index.html* dosya, herhangi bir CSS dosyaları sonra:

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

Gözat *index.html* , sayfa `http://localhost:<port>/swagger/ui/index.html`. Girin `http://localhost:<port>/swagger/v1/swagger.json` başlığının metin seçeneğine tıklayıp **Araştır** düğmesi. Sonuçta elde edilen sayfanın şu şekilde görünür:

![Swagger kullanıcı Arabirimi ile özel üst bilgi başlığı](web-api-help-pages-using-swagger/_static/custom-header.png)

Sayfa ile yapabileceğiniz çok daha fazla yoktur. UI kaynakları, tam özelliklerine bakın [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui).
