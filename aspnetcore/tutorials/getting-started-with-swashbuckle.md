---
title: Swashbuckle ve ASP.NET Core kullanmaya başlama
author: zuckerthoben
description: Swagger kullanıcı arabirimini tümleştirmek için ASP.NET Core web API projesi için Swashbuckle eklemeyi öğrenin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: eaeb27903c462ef002edbb0b84cd5a751db2bb9d
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729736"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a>Swashbuckle ve ASP.NET Core kullanmaya başlama

Tarafından [Shayne Boyer](https://twitter.com/spboyer) ve [Scott Addie](https://twitter.com/Scott_Addie)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Swashbuckle için üç ana bileşeni vardır:

* [Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger nesne modeli ve kullanıma sunmak için ara yazılım `SwaggerDocument` nesneleri JSON uç noktalar olarak.

* [Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): derlemeleri bir Swagger oluşturucuyu `SwaggerDocument` nesneleri doğrudan rotalara, denetleyicilere ve modeller. Bu, genellikle otomatik olarak Swagger JSON kullanıma sunmak için Swagger uç nokta ara yazılımı ile birleştirilir.

* [Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): Swagger kullanıcı Arabirimi aracını katıştırılmış bir sürümü. Web API işlevleri açıklamak için zengin, özelleştirilebilir bir deneyim oluşturmak için Swagger JSON yorumlar. Genel yöntemler için yerleşik test harnesses içerir.

## <a name="package-installation"></a>Paket yükleme

Swashbuckle ile aşağıdaki yaklaşımlardan eklenebilir:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Paket Yöneticisi Konsolu** penceresi:
  * Git **Görünüm** > **diğer Windows** > **Paket Yöneticisi Konsolu**
  * Dizine gidin *TodoApi.csproj* dosya var
  * Aşağıdaki komutu yürütün:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* Gelen **NuGet paketlerini Yönet** iletişim:
  * ' Nde projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**
  * Ayarlama **paket kaynağı** "nuget.org" için
  * Arama kutusuna "Swashbuckle.AspNetCore" girin
  * "Swashbuckle.AspNetCore" paketten seçin **Gözat** sekmesinde **yükleyin**

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Sağ *paketleri* klasöründe **çözüm paneli** > **paketleri Ekle...**
* Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" için açılır
* Arama kutusuna "Swashbuckle.AspNetCore" girin
* Sonuçlar bölmesinde "Swashbuckle.AspNetCore" paketi seçin ve **Paketi Ekle**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki komutu çalıştırın **tümleşik Terminal**:

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

Hizmetleri koleksiyonunu Swagger oluşturucusunu eklemek `Startup.ConfigureServices` yöntemi:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

Kullanmak için aşağıdaki ad alanı içe `Info` sınıfı:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

İçinde `Startup.Configure` yöntemi, oluşturulan JSON belgesini ve Swagger kullanıcı arabirimini hizmet veren ara yazılımı etkinleştir:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

Uygulamayı başlatın ve gidin `http://localhost:<port>/swagger/v1/swagger.json`. Uç noktaları açıklayan oluşturulan belge gösterildiği gibi görünüyor [Swagger belirtimi (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).

Swagger kullanıcı arabirimini bulunabilir `http://localhost:<port>/swagger`. Swagger kullanıcı Arabirimi API inceleyin ve diğer programları içerecek.

> [!TIP]
> Swagger kullanıcı arabirimini uygulamanızın kök dizininde hizmet (`http://localhost:<port>/`) ayarlayın `RoutePrefix` boş bir dize özelliği:
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize-and-extend"></a>Özelleştirme ve genişletme

Swagger tema eşleştirmek için nesne modeli belgeleme ve kullanıcı arabirimini özelleştirmek için seçenekler sağlar.

### <a name="api-info-and-description"></a>API bilgileri ve açıklaması

Yapılandırma eylemi geçirilen `AddSwaggerGen` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:

![Sürüm bilgileri ile swagger kullanıcı Arabirimi: açıklama, yazar ve daha fazla bağlantıya bakın](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML açıklamaları

XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilebilir.

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

* ' Nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**
* Denetleyin **XML belge dosyası** altında kutusunda **çıkış** bölümünü **yapı** sekmesi

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Mac için Visual Studio](#tab/visual-studio-mac-xml/)

* Açık **proje seçenekleri** iletişim > **yapı** > **derleyici**
* Denetleme **xml belgeleri oluşturmak** altında kutusunda **Genel Seçenekler** bölümü

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

El ile eklemek için aşağıdaki kod parçacığını *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

---

XML açıklamaları etkinleştirme belgelenmemiş genel türleri ve üyeleri için hata ayıklama bilgileri sağlar. Belgelenmemiş türleri ve üyeleri tarafından uyarı iletisi gösterilir. Örneğin, aşağıdaki ileti uyarı kod 1591 ihlal gösterir:

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

Noktalı virgülle ayrılmış olarak yoksaymak için uyarı kodlarının listesini tanımlayarak uyarıları bastırma *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

Oluşturulan XML dosyasını kullanmak için Swagger yapılandırın. Linux veya Windows olmayan işletim sistemleri için dosya adlarını ve yollarını büyük küçük harfe duyarlı olabilir. Örneğin, bir *TodoApi.XML* Windows ancak değil CentOS dosyanın geçerli olduğu.

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

Önceki kod [yansıma](/dotnet/csharp/programming-guide/concepts/reflection) , Web API projesi eşleşen bir XML dosya adı oluşturmak için kullanılır. Bu yaklaşım, oluşturulan XML dosya adı proje adının eşleşmesini sağlar. [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) özelliği XML dosyasının yolunu oluşturmak için kullanılır.

Bir eylem Üçlü eğik çizgi açıklama ekleme, Swagger kullanıcı arabirimini bölüm başlığı açıklama ekleyerek geliştirir. Ekleme bir [ \<Özet >](/dotnet/csharp/programming-guide/xmldoc/summary) öğesi yukarıdaki `Delete` eylem:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

Önceki kodun iç metni Swagger kullanıcı arabirimini görüntüler `<summary>` öğe:

![Swagger kullanıcı Arabirimi 'Belirli bir Todoıtem siler.' XML açıklama gösterme DELETE yöntemi](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

Kullanıcı Arabirimi tarafından oluşturulan JSON şeması yönetilir:

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

Ekleme bir [ \<açıklamalar >](/dotnet/csharp/programming-guide/xmldoc/remarks) öğesine `Create` eylem yöntemi belgeleri. Belirtilen bilgilerini tamamlayan `<summary>` öğesi ve daha güçlü bir Swagger kullanıcı Arabirimi sağlar. `<remarks>` Metin, JSON veya XML öğesi içeriği oluşabilir.

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

Bu ek açıklamalar UI geliştirmeleriyle dikkat edin:

![Swagger kullanıcı Arabirimi ile gösterilen ek açıklamaları](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Veri açıklamaları

Modelin bulunan özniteliklerle tasarlamanız [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) Swagger kullanıcı Arabirimi bileşenlerini sürücü yardımcı olması için ad alanı.

Ekleme `[Required]` özniteliğini `Name` özelliği `TodoItem` sınıfı:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

Bu öznitelik varlığını UI davranışını değiştiren ve temel JSON şeması değiştirir:

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

Ekleme `[Produces("application/json")]` özniteliği API denetleyicisi. Amacı denetleyicinin Eylemler bir yanıt içerik türü desteği bildirmektir *uygulama/json*:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

**Yanıt içerik türü** açılan denetleyicinin GET eylemler için varsayılan olarak bu içerik türü seçer:

![Swagger kullanıcı Arabirimi ile varsayılan yanıt içerik türü](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Web API içinde veri ek açıklamaları kullanımını arttıkça, kullanıcı Arabirimi ve API daha açıklayıcı ve kullanışlı hale sayfaları yardımcı olur.

### <a name="describe-response-types"></a>Yanıt türleri açıklanmaktadır

Süren geliştiricilerin ne döndürülen ile en ilgili&mdash;özellikle yanıt türleri ve hata kodları (değil standart değilse). XML açıklamaları ve veri ek açıklamaları içinde yanıt türleri ve hata kodları gösterilir.

`Create` Eylem başarı bir HTTP 201 durum kodunu döndürür. Gönderilen istek gövdesi null olduğunda bir HTTP 400 durum kodu döndürülür. Swagger kullanıcı arabirimini uygun belgelerinde tüketici bu beklenen sonuçları bilgisi eksik. Aşağıdaki örnekte vurgulanmış satırlarını ekleyerek bu sorunu düzeltin:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

Swagger kullanıcı Arabirimi şimdi beklenen HTTP yanıt kodları açıkça belgeler:

![Swagger kullanıcı Arabirimi 'yeni oluşturulan Yapılacaklar öğesi döndürür' POST yanıt sınıf tanımına gösteren ve '400 - öğe null ise' durum kodunu ve yanıt iletileri altında nedeni](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a>Kullanıcı arabirimini özelleştirme

UI stock işlevsel ve presentable ' dir. Ancak, API belgelerine sayfaları marka veya temayı temsil etmelidir. Swashbuckle bileşenleri markalama statik dosyaları işleme için kaynakları ekleme ve bu dosyaları barındırmak için klasör yapısı oluşturulmasını gerektirir.

.NET Framework veya .NET hedefleme varsa 1.x çekirdek, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet paketini projeye:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Önceki NuGet paketi zaten .NET Core hedefleme yüklü olsa 2.x ve kullanarak [metapackage](xref:fundamentals/metapackage).

Statik dosya ara yazılımlarını etkinleştir:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

İçeriği alma *dağ* klasöründen [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui/tree/master/dist). Bu klasör Swagger kullanıcı Arabirimi sayfası için gerekli varlıkları içerir.

Oluşturma bir *wwwroot/swagger/UI* klasörünü ve içeriğini buraya kopyalayın *dağ* klasör.

Oluşturma bir *custom.css* dosyasında *wwwroot/swagger/UI*, sayfa üstbilgisi özelleştirmek için aşağıdaki CSS ile:

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

Başvuru *custom.css* içinde *index.html* dosya, diğer bir CSS dosyaları sonra:

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

Gözat *index.html* adresindeki sayfasında `http://localhost:<port>/swagger/ui/index.html`. Girin `http://localhost:<port>/swagger/v1/swagger.json` üst bilginin textbox ve tıklatın **Araştır** düğmesi. Sonuçta elde edilen sayfanın şu şekilde görünür:

![Özel üstbilgi başlık swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/custom-header.png)

Çok daha fazlasını sayfasıyla yapabilirsiniz yoktur. UI kaynakları tam özelliklerine bakın [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui).
