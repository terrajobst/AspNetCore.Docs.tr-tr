---
title: "ASP.NET Core Web API Yardım Swagger kullanarak sayfaları"
author: spboyer
description: "Bu öğretici belgeleri oluşturmak ve bir Web API uygulaması için sayfa yardımcı olmak için Swagger ekleme bir kılavuz sağlar."
keywords: "ASP.NET Core, Swagger, Swashbuckle, sayfalar, Web API Yardım"
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 630378510f4182034735cb4c306dfc5a761543ab
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a>ASP.NET Core Web API Yardım Swagger kullanarak sayfaları

<a name="web-api-help-pages-using-swagger"></a>

Tarafından [Shayne Boyer](https://twitter.com/spboyer) ve [Scott Addie](https://twitter.com/Scott_Addie)

Bir API çeşitli yöntemleri anlama kullanıcı bir uygulama oluştururken geliştiriciler için bir sınama olabilir.

Web API için iyi belgeleri ve Yardım sayfaları oluşturma, kullanarak [Swagger](https://swagger.io/) .NET Core uygulamasıyla [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), birkaç NuGet paketleri ekleme ve değiştirme olarak kadar kolaydır *haline*.

* [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) ASP.NET çekirdek Web API için Swagger belgeleri oluşturmak için bir açık kaynaklı proje.

* [Swagger](https://swagger.io/) etkileşimli belgeler, istemci SDK oluşturma ve bulunabilirlik için destek sağlayan bir RESTful API'si makine tarafından okunabilir bir gösterimidir.

Bu öğretici örneğe bağlı derlemeler [yapı bilgisayarınızı ilk Web API ile ASP.NET Core MVC ve Visual Studio](xref:tutorials/first-web-api). Örnek: izlemek istiyorsanız, indirme [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).

## <a name="getting-started"></a>Başlarken

Swashbuckle için üç ana bileşeni vardır:

* `Swashbuckle.AspNetCore.Swagger`: bir Swagger nesne modeli ve kullanıma sunmak için ara yazılım `SwaggerDocument` nesneleri JSON uç noktalar olarak.

* `Swashbuckle.AspNetCore.SwaggerGen`: derlemeleri bir Swagger oluşturucuyu `SwaggerDocument` nesneleri doğrudan rotalara, denetleyicilere ve modeller. Bu, genellikle otomatik olarak Swagger JSON kullanıma sunmak için Swagger uç nokta ara yazılımı ile birleştirilir.

* `Swashbuckle.AspNetCore.SwaggerUI`: Web API işlevleri açıklamak için zengin, özelleştirilebilir bir deneyim oluşturmak için Swagger JSON yorumlar Swagger kullanıcı Arabirimi aracını katıştırılmış bir sürümü. Genel yöntemler için yerleşik test harnesses içerir.

## <a name="nuget-packages"></a>NuGet paketleri

Swashbuckle ile aşağıdaki yaklaşımlardan eklenebilir:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Paket Yöneticisi Konsolu** penceresi:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* Gelen **NuGet paketlerini Yönet** iletişim:

     * Projenize sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**
     * Ayarlama **paket kaynağı** "nuget.org" için
     * Arama kutusuna "Swashbuckle.AspNetCore" girin
     * "Swashbuckle.AspNetCore" paketten seçin **Gözat** sekmesinde **yükleyin**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Sağ *paketleri* klasöründe **çözüm paneli** > **paketleri Ekle...**
* Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" için açılır
* Arama kutusuna Swashbuckle.AspNetCore girin
* Sonuçlar bölmesinde Swashbuckle.AspNetCore paketi seçin ve **Paketi Ekle**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki komutu çalıştırın **tümleşik Terminal**:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Şu komutu çalıştırın:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a>Ekleme ve Swagger ara yazılım için yapılandırma

Hizmetleri koleksiyonunu Swagger oluşturucusunu eklemek `ConfigureServices` yöntemi *haline*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

Aşağıdaki ekleme deyimini kullanarak `Info` sınıfı:

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

İçinde `Configure` yöntemi *haline*, oluşturulan JSON belgesini ve SwaggerUI hizmet veren ara yazılımı etkinleştir:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

Uygulamayı başlatın ve gidin `http://localhost:<random_port>/swagger/v1/swagger.json`. Uç noktaları açıklayan oluşturulan belge görüntülenir.

**Not:** Microsoft Edge, Google Chrome, Firefox görünen ve JSON belgelerini yerel olarak. Chrome için daha kolay okumak için belgenin biçimlendirme uzantıları vardır. *Aşağıdaki örnek okumanızdır azalır.*

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
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
   "securityDefinitions": {}
}
```

Bu belge Swagger giderek görüntülenebilen kullanıcı Arabirimi, sürücüler `http://localhost:<random_port>/swagger`:

![Swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Her ortak eylem yönteminde `TodoController` kullanıcı Arabiriminden test edilebilir. Bir yöntem adı bölümü genişletmek için tıklatın. Tüm gerekli parametreleri ekleyin ve "deneyin!"'i tıklatın.

![Örnek Swagger alma testi](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a>Özelleştirme ve genişletilebilirlik

Swagger tema eşleştirmek için nesne modeli belgeleme ve kullanıcı arabirimini özelleştirmek için seçenekler sağlar.

### <a name="api-info-and-description"></a>API bilgileri ve açıklaması

Yapılandırma eylemi geçirilen `AddSwaggerGen` yöntemi, yazar, lisans ve açıklaması gibi bilgileri eklemek için kullanılabilir:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

Aşağıdaki resimde Swagger sürüm bilgilerini görüntüleme UI gösterilmektedir:

![Sürüm bilgileri ile swagger kullanıcı Arabirimi: açıklama, yazar ve daha fazla bağlantıya bakın](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML açıklamaları

XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilebilir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* ' Nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**
* Denetleyin **XML belge dosyası** altında kutusunda **çıkış** bölümünü **yapı** sekmesi:

![Proje Özellikleri'nin sekmesi oluştur](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Açık **proje seçenekleri** iletişim > **yapı** > **derleyici**
* Denetleme **xml belgeleri oluşturmak** altında kutusunda **Genel Seçenekler** bölümü:

![Proje seçenekleri Genel Seçenekler bölümünde](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

El ile eklemek için aşağıdaki kod parçacığını *.csproj* dosyası:

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Visual Studio koduna bakın.

---

Oluşturulan XML dosyasını kullanmak için Swagger yapılandırın. Linux veya Windows olmayan işletim sistemleri için dosya adlarını ve yollarını büyük küçük harfe duyarlı olabilir. Örneğin, bir *ToDoApi.XML* Windows ancak değil CentOS dosya'nin bulunabilir.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

Önceki kod `ApplicationBasePath` uygulamanın taban yolu alır. Taban yol XML açıklamaları dosyayı bulmak için kullanılır. *TodoApi.xml* dosya uygulama adına dayalı oluşturulan XML adını yorumları Bu örnekte, yalnızca çalışır.

Yöntem Üçlü eğik çizgi açıklamaları ekleme, Swagger kullanıcı arabirimini bölüm başlığı açıklama ekleyerek geliştirir:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Swagger kullanıcı Arabirimi 'Belirli bir Todoıtem siler.' XML açıklama gösterme DELETE yöntemi](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

Kullanıcı arabirimini de bu açıklamalar içeren oluşturulan JSON dosya tarafından yönetilir:

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

Ekleme bir [ <remarks> ](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) etiketini `Create` eylem yöntemi belgeleri. Belirtilen bilgilerini tamamlayan `<summary>` etiketi ve daha güçlü bir Swagger kullanıcı Arabirimi sağlar. `<remarks>` Metin, JSON veya XML etiket içeriği oluşabilir.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

Bu ek açıklamalar UI geliştirmeleriyle dikkat edin.

![Swagger kullanıcı Arabirimi ile gösterilen ek açıklamaları](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Veri ek açıklamaları

Modelin bulunan özniteliklerle tasarlamanız `System.ComponentModel.DataAnnotations`, Swagger kullanıcı Arabirimi bileşenlerini sürücü yardımcı olacak.

Ekleme `[Required]` özniteliğini `Name` özelliği `TodoItem` sınıfı:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

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

Ekleme `[Produces("application/json")]` özniteliği API denetleyicisi. Amacı denetleyicinin Eylemler bir içerik türü bir dönüş desteği bildirmektir *uygulama/json*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

**Yanıt içerik türü** açılan denetleyicinin GET eylemler için varsayılan olarak bu içerik türü seçer:

![Swagger kullanıcı Arabirimi ile varsayılan yanıt içerik türü](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Web API içinde veri ek açıklamaları kullanımını arttıkça, kullanıcı Arabirimi ve API daha açıklayıcı ve kullanışlı hale sayfaları yardımcı olur.

### <a name="describing-response-types"></a>Açıklayan yanıt türleri

Süren geliştiricilerin ne döndürülen ile en ilgili &mdash; özellikle yanıt türleri ve hata kodları (değil standart değilse). Bunlar XML açıklamaları ve veri ek açıklamaları işlenir.

`Create` Eylem döndürür `201 Created` başarı veya `400 Bad Request` gönderilen istek gövdesi olduğunda null. Swagger kullanıcı arabirimini uygun belgelerinde tüketici bu beklenen sonuçları bilgisi eksik. Aşağıdaki örnekte vurgulanmış satırlarını ekleyerek bu sorun düzeltilene:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

Swagger kullanıcı Arabirimi şimdi beklenen HTTP yanıt kodları açıkça belgeler:

![Swagger kullanıcı Arabirimi 'yeni oluşturulan Yapılacaklar öğesi döndürür' POST yanıt sınıf tanımına gösteren ve '400 - öğe null ise' durum kodunu ve yanıt iletileri altında nedeni](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a>Kullanıcı arabirimini özelleştirme

UI stock işlevsel ve presentable; Ancak, belge sayfalarının API'nize oluştururken, marka veya temayı temsil etmek için istediğiniz. Swashbuckle bileşenleri ile bu görevi gerçekleştirmeye statik dosyaları işleme için kaynakları ekleme ve bu dosyaları barındırmak için klasör yapısı oluşturulmasını gerektirir.

.NET Framework'ü hedefleme varsa ekleyin `Microsoft.AspNetCore.StaticFiles` projesi için NuGet paketi:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Statik dosya ara yazılımlarını etkinleştir:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

İçeriği alma *dağ* klasöründen [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui/tree/2.x/dist). Bu klasör Swagger kullanıcı Arabirimi sayfası için gerekli varlıkları içerir.

Oluşturma bir *wwwroot/swagger/UI* klasörünü ve içeriğini buraya kopyalayın *dağ* klasör.

Oluşturma bir *wwwroot/swagger/ui/css/custom.css* sayfa üstbilgisi özelleştirmek için aşağıdaki CSS dosyasıyla:

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

Başvuru *custom.css* içinde *index.html* dosyası:

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

Gözat *index.html* adresindeki sayfasında `http://localhost:<random_port>/swagger/ui/index.html`. Girin `http://localhost:<random_port>/swagger/v1/swagger.json` üst bilginin textbox ve tıklatın **Araştır** düğmesi. Sonuçta elde edilen sayfanın şu şekilde görünür:

![Özel üstbilgi başlık swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/custom-header.png)

Çok daha fazlasını sayfasıyla yapabilirsiniz yoktur. UI kaynakları tam özelliklerine bakın [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui).
