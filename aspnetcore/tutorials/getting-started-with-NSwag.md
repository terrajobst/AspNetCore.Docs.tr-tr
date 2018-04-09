---
title: NSwag ve ASP.NET Core kullanmaya başlama
author: zuckerthoben
description: Belgeleri oluşturmak ve ASP.NET Core Web API uygulaması için sayfa yardımcı olmak için NSwag kullanmayı öğrenin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>NSwag ve ASP.NET Core kullanmaya başlama

Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](https://rsuter.com)

Kullanarak [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core ile ara yazılımı gerektirir [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet paketi. Swagger kullanıcı arabirimini (v2 ve v3), bir Swagger oluşturucuyu paketi oluşur ve [ReDoc UI](https://github.com/Rebilly/ReDoc).

Yüksek oranda NSwag, kullanıcının kullanmak için önerilen code oluşturma özellikleri. Kod oluşturma için aşağıdaki seçeneklerden birini seçin:

* Kullanım [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), C# ve TypeScript API için istemci kodu oluşturmak için bir Windows masaüstü uygulaması
* Kullanım [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) veya [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) kod oluşturmayı projenizi içinde için NuGet paketleri
* Gelen NSwag kullanmak [komut satırı](https://github.com/NSwag/NSwag/wiki/CommandLine)
* Kullanım [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet paketi

## <a name="features"></a>Özellikler

NSwag kullanmak için ana nedeni Swagger kullanıcı arabirimini ve Swagger yalnızca tanıtmak için yeteneğidir yapmak ancak Oluşturucu, esnek kod oluşturma özelliklerini kullanın. Varolan bir API gerekmeyen&mdash;Swagger birleştirme ve bir istemci uygulaması oluşturmak NSwag izin üçüncü taraf API'leri kullanabilirsiniz. Her iki durumda da geliştirme döngüsü öncelikli ve API değişiklikler daha kolayca uyarlayabilirsiniz.

## <a name="package-installation"></a>Paket yükleme

NSwag NuGet paketi ile aşağıdaki yaklaşımlardan eklenebilir:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Paket Yöneticisi Konsolu** penceresi:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Gelen **NuGet paketlerini Yönet** iletişim:
  * Projenize sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**
  * Ayarlama **paket kaynağı** "nuget.org" için
  * Arama kutusuna "NSwag.AspNetCore" girin
  * "NSwag.AspNetCore" paketten seçin **Gözat** sekmesinde **yükleyin**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* Sağ *paketleri* klasöründe **çözüm paneli** > **paketleri Ekle...**
* Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" için açılır
* Arama kutusuna NSwag.AspNetCore girin
* Sonuçlar bölmesinde NSwag.AspNetCore paketi seçin ve **Paketi Ekle**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki komutu çalıştırın **tümleşik Terminal**:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Şu komutu çalıştırın:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Ekleme ve Swagger ara yazılımını yapılandırma

Aşağıdaki ad alanlarında alma `Info` sınıfı:

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

İçinde `Startup.Configure` yöntemi, oluşturulan Swagger belirtimi ve Swagger kullanıcı arabirimini hizmet veren ara yazılımı etkinleştir:

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

Uygulamasını başlatın. Gidin `/swagger` Swagger kullanıcı arabirimini görüntülemek için. Gidin `/swagger/v1/swagger.json` Swagger belirtimi görüntülemek için.

## <a name="code-generation"></a>Kod oluşturma

### <a name="via-nswagstudio"></a>Via NSwagStudio

* Yükleme `NSwagStudio` resmi gelen [GitHub deposunu](https://github.com/RSuter/NSwag/wiki/NSwagStudio).

* Launch NSwagStudio. Konumu girin, *swagger.json* veya doğrudan kopyalayın:

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* İstenen istemci çıktı türü belirtin. Seçenekleriniz **TypeScript istemci**, **CSharp istemci**, veya **CSharp Web API denetleyicisi**. Bir Web API denetleyicisi kullanarak, aslında bir ters oluşturma. Hizmet yeniden oluşturmak için bir hizmet belirtimini kullanır.

* Tıklatın **çıkışları oluşturmak**.

* Bir tam istemci uygulaması Burada gördüğünüz *TodoApi.NSwag* C# Örnek:

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* Bir istemci projeye dosya yerleştirme (örneğin, bir [Xamarin.Forms](/xamarin/xamarin-forms/) uygulama). API tüketen başlatın:

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Bir temel URL'yi ve/veya HTTP istemcisi API istemcisini ekleyemezsiniz. En iyi uygulama her zaman olduğu [HttpClient yeniden](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

API'nizi istemci projelere kolayca uygulama şimdi başlayabilirsiniz.

### <a name="other-ways-to-generate-client-code"></a>İstemci kodu oluşturmak için diğer yolları

İş akışınıza uygun kodu daha fazla farklı yollarla oluşturabilirsiniz:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [Kod:](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [T4 şablonları](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>Özelleştirme

### <a name="xml-comments"></a>XML açıklamaları

XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilir:

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)
* ' Nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**
* Denetleyin **XML belge dosyası** altında kutusunda **çıkış** bölümünü **yapı** sekmesi:

![Proje Özellikleri'nin sekmesi oluştur](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Mac için Visual Studio](#tab/visual-studio-mac-xml/)
* Açık **proje seçenekleri** iletişim > **yapı** > **derleyici**
* Denetleme **xml belgeleri oluşturmak** altında kutusunda **Genel Seçenekler** bölümü:

![Proje seçenekleri Genel Seçenekler bölümünde](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)
El ile eklemek için aşağıdaki kod parçacığını *.csproj* dosyası:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a>Veri açıklamaları

NSwag kullanan [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve Web API eylemler için en iyi deneyim getirmektir [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Sonuç olarak, NSwag eyleminizi ne yaptığını ve ne döndürür gösterilemiyor. Aşağıdaki örnek göz önünde bulundurun:

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

Önceki eylem döndürür `IActionResult`, ancak işlem içinde ya da döndürmektir [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) veya [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Veri ek açıklamaları, istemciler bu eylem döndürme hangi HTTP yanıtı bildirmek için kullanılır. Aşağıdaki özniteliklerle eylem tasarlamanız:

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

Swagger Oluşturucu artık doğru şekilde bu eylem tanımlayabilir ve uç nokta çağrılırken ne aldıkları oluşturulan istemcileri biliyorsunuz. Bu öznitelikler ile tüm eylemler dekorasyon kullanmamanız önerilir. API eylemlerinizi döndürmelidir hangi HTTP yanıtları hakkında yönergeler için bkz: [RFC 7231 belirtimi](https://tools.ietf.org/html/rfc7231#section-4.3).
