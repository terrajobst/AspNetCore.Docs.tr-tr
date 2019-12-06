---
page_type: sample
description: Swagger Kullanıcı arabirimini bütünleştirmek için ASP.NET Core Web API Projenize swashbuckle ekleme hakkında bilgi edinin.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
- vs-code
- vs-mac
urlFragment: getstarted-swashbuckle-aspnetcore
ms.openlocfilehash: e02247325f430b0ce23dbb3f5bc344a60a1a164a
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879726"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a>Swashbuckle ve ASP.NET Core kullanmaya başlayın

Bir Web API'si kullanılırken, çeşitli metotlarını anlama bir geliştirici için zor olabilir. [Swagger](https://swagger.io/)olarak da bilinen [Openapı](https://www.openapis.org/), Web API'leri için kullanışlı belgeler ve Yardım sayfaları oluşturma sorununu çözer. Bu, etkileşimli belgeleri, istemci SDK oluşturma ve API keşfedilebilirliğini gibi avantajlar sağlar.

Bu örnekte, [swashbuckle. AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) .NET uygulamasını gösteriliyor.

## <a name="add-and-configure-swagger-middleware"></a>Swagger ara yazılım ekleme ve yapılandırma

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<TodoContext>(opt =>
        opt.UseInMemoryDatabase("TodoList"));
    services.AddControllers();

    // Register the Swagger generator, defining 1 or more Swagger documents
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo { Title = "My API", Version = "v1" });
    });
}
```

`Startup.Configure` yönteminde, oluşturulan JSON belgesine ve Swagger Kullanıcı arabirimine hizmet veren ara yazılımı etkinleştirin:

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable middleware to serve generated Swagger as a JSON endpoint.
    app.UseSwagger();

    // Enable middleware to serve swagger-ui (HTML, JS, CSS, etc.),
    // specifying the Swagger JSON endpoint.
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1"); 
    });

    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

Yukarıdaki `UseSwaggerUI` yöntemi çağrısı [statik dosya ara yazılımını](https://docs.microsoft.com/aspnet/core/fundamentals/static-files)sunar. .NET Framework veya .NET Core 1. x 'i hedefliyorsanız, projeye [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet paketini ekleyin.

Uygulamayı başlatın ve `http://localhost:<port>/swagger/v1/swagger.json`gidin. Uç noktaları tanımlayan oluşturulan belge, [Swagger belirtiminde (Swagger. JSON)](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson)gösterildiği gibi görünür.

Swagger Kullanıcı arabirimi `http://localhost:<port>/swagger`' de bulunabilir. Swagger Kullanıcı arabirimi aracılığıyla API 'YI keşfet ve diğer programlarda birleştirme.

> [!TIP]
> Swagger Kullanıcı arabirimine uygulamanın kökünde (`http://localhost:<port>/`) hizmeti sağlamak için `RoutePrefix` özelliğini boş bir dize olarak ayarlayın:
>
> ```csharp
>app.UseSwaggerUI(c =>
>{
>    c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
>    c.RoutePrefix = string.Empty;
>});
>```

IIS veya ters proxy ile dizin kullanıyorsanız, Swagger uç noktasını `./` önekini kullanarak göreli bir yol olarak ayarlayın. Örneğin: `./swagger/v1/swagger.json`. `/swagger/v1/swagger.json` kullanmak, uygulamayı URL 'nin gerçek kökünde (artı kullanılıyorsa rota öneki), JSON dosyasını aramasını söyler. Örneğin, `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`yerine `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` kullanın.

## <a name="customize-and-extend"></a>Özelleştirme ve genişletme

Swagger, nesne modelini belgeleme ve Kullanıcı arabirimini temanızla eşleşecek şekilde özelleştirme seçenekleri sağlar.

`Startup` sınıfında, aşağıdaki ad alanlarını ekleyin:
```csharp
using System;
using System.Reflection;
using System.IO;
```

### <a name="api-info-and-description"></a>API bilgisi ve açıklaması

`AddSwaggerGen` yöntemine geçirilen yapılandırma eylemi yazar, lisans ve açıklama gibi bilgileri ekler:

```csharp
// Register the Swagger generator, defining 1 or more Swagger documents
services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo
    {
        Version = "v1",
        Title = "ToDo API",
        Description = "A simple example ASP.NET Core Web API",
        TermsOfService = new Uri("https://example.com/terms"),
        Contact = new OpenApiContact
        {
            Name = "Shayne Boyer",
            Email = string.Empty,
            Url = new Uri("https://twitter.com/spboyer"),
        },
        License = new OpenApiLicense
        {
            Name = "Use under LICX",
            Url = new Uri("https://example.com/license"),
        }
    });
});
```

Swagger Kullanıcı arabirimi, sürümün bilgilerini görüntüler:

![Sürüm bilgileriyle Swagger Kullanıcı arabirimi: Açıklama, yazar ve daha fazla bağlantı görüntüle](sample_images/custom-info.png)

### <a name="xml-comments"></a>XML açıklamaları

XML açıklamaları aşağıdaki yaklaşımlar ile etkinleştirilebilir:

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini** projeye sağ tıklayın ve **>. csproj Project_Name < Düzenle**' yi seçin.
* Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* *Çözüm bölmesi*, **Denetim** ' e basın ve proje adına tıklayın. **Araçlar** > **dosya düzenleme**' ye gidin.
* Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

---

XML açıklamalarını etkinleştirmek, belgelenmemiş ortak türler ve Üyeler için hata ayıklama bilgileri sağlar. Belgelenmemiş türler ve Üyeler uyarı iletisiyle belirtilir. Örneğin, aşağıdaki ileti 1591 uyarı kodunu ihlal eder:

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

Uyarıları proje genelinde gizlemek için, proje dosyasında yoksayılacak uyarı kodlarının noktalı virgülle ayrılmış bir listesini tanımlayın. Uyarı kodlarının `$(NoWarn);` eklenmesi, [ C# varsayılan değerleri](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) de uygular.

```xml
<NoWarn>$(NoWarn);1591</NoWarn>
```

Yalnızca belirli Üyeler için uyarıları gizlemek için, kodu [#pragma uyarı](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) Önişlemci yönergeleri arasına alın. Bu yaklaşım, API belgeleri aracılığıyla sunulmaması gereken kod için yararlıdır. Aşağıdaki örnekte, tüm `Program` sınıfı için uyarı kodu CS1591 yok sayılır. Uyarı kodu zorlaması sınıf tanımının kapandığına geri yüklenir. Virgülle ayrılmış bir liste ile birden çok uyarı kodu belirtin.

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

Swagger 'yi yukarıdaki yönergelerle oluşturulan XML dosyasını kullanacak şekilde yapılandırın. Linux veya Windows dışı işletim sistemleri için dosya adları ve yolları büyük/küçük harfe duyarlı olabilir. Örneğin, *TodoApi. xml* dosyası Windows üzerinde geçerlidir ancak CentOS değildir.

```csharp
/// NOTE LAST 3 LINES IN THIS SNIPPET
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<TodoContext>(opt =>
        opt.UseInMemoryDatabase("TodoList"));
    services.AddControllers();

    // Register the Swagger generator, defining 1 or more Swagger documents
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo
        {
            Version = "v1",
            Title = "ToDo API",
            Description = "A simple example ASP.NET Core Web API",
            TermsOfService = new Uri("https://example.com/terms"),
            Contact = new OpenApiContact
            {
                Name = "Shayne Boyer",
                Email = string.Empty,
                Url = new Uri("https://twitter.com/spboyer"),
            },
            License = new OpenApiLicense
            {
                Name = "Use under LICX",
                Url = new Uri("https://example.com/license"),
            }
        });

        // Set the comments path for the Swagger JSON and UI.
        var xmlFile = $"{Assembly.GetExecutingAssembly().GetName().Name}.xml";
        var xmlPath = Path.Combine(AppContext.BaseDirectory, xmlFile);
        c.IncludeXmlComments(xmlPath);
    });
}
```

Yukarıdaki kodda, [yansıma](/dotnet/csharp/programming-guide/concepts/reflection) , Web API projesi ile eşleşen bir XML dosya adı oluşturmak için kullanılır. [AppContext. BaseDirectory](/dotnet/api/system.appcontext.basedirectory) ÖZELLIĞI, XML dosyasının yolunu oluşturmak için kullanılır. Bazı Swagger özellikleri (örneğin, bir XML belge dosyası kullanılmadan, giriş parametrelerinin veya HTTP yöntemlerinin ve yanıt kodlarının) bir bölümü çalışır. Çoğu özellik için, yöntem özetleri ve parametrelerin ve yanıt kodlarının açıklamaları, bir XML dosyası kullanımı zorunludur.

Bir eyleme üç eğik çizgiyle açıklama eklediğinizde bölüm üst bilgisine açıklama eklenir ve Swagger UI geliştirilir. `Delete` eyleminin üstüne bir [\<summary >](/dotnet/csharp/programming-guide/xmldoc/summary) öğesi ekleyin:

```csharp
/// <summary>
/// Deletes a specific TodoItem.
/// </summary>
/// <param name="id"></param>        
[HttpDelete("{id}")]
public IActionResult Delete(long id)
{
    var todo = _context.TodoItems.Find(id);

    if (todo == null)
    {
        return NotFound();
    }

    _context.TodoItems.Remove(todo);
    _context.SaveChanges();

    return NoContent();
}
```
Swagger Kullanıcı arabirimi, önceki kodun `<summary>` öğesinin iç metnini görüntüler:

![XML açıklamasını gösteren Swagger Kullanıcı arabirimi, belirli bir TodoItem siler. DELETE yöntemi için](sample_images/triple-slash-comments.png)

Kullanıcı arabirimi, oluşturulan JSON şeması tarafından çalıştırılır:

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
`Create` Action yöntemi belgelerine bir [\<açıklamaları >](/dotnet/csharp/programming-guide/xmldoc/remarks) öğesi ekleyin. `<summary>` öğesinde belirtilen bilgileri tamamlar ve daha sağlam bir Swagger Kullanıcı arabirimi sağlar. `<remarks>` öğesi içeriği metin, JSON veya XML içerebilir.

```csharp
/// <summary>
/// Creates a TodoItem.
/// </summary>
/// <remarks>
/// Sample request:
///
///     POST /Todo
///     {
///        "id": 1,
///        "name": "Item1",
///        "isComplete": true
///     }
///
/// </remarks>
/// <param name="item"></param>
/// <returns>A newly created TodoItem</returns>
/// <response code="201">Returns the newly created item</response>
/// <response code="400">If the item is null</response>            
[HttpPost]
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public ActionResult<TodoItem> Create(TodoItem item)
{
    _context.TodoItems.Add(item);
    _context.SaveChanges();

    return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```
Bu ek açıklamalarla UI geliştirmelerini göz unutmayın:

![Ek açıklamaların gösterildiği Swagger Kullanıcı arabirimi](sample_images/xml-comments-extended.png)

### <a name="data-annotations"></a>Veri açıklamaları

Swagger Kullanıcı Arabirimi bileşenlerini sağlamaya yardımcı olmak için [System. ComponentModel. Dataaçıklamalarda](/dotnet/api/system.componentmodel.dataannotations) ad alanında bulunan öznitelikleri olan modeli işaretleyin.

`TodoItem` sınıfının `Name` özelliğine `[Required]` özniteliğini ekleyin:

```csharp
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;

namespace TodoApi.Models
{
    public class TodoItem
    {
        public long Id { get; set; }

        [Required]
        public string Name { get; set; }

        [DefaultValue(false)]
        public bool IsComplete { get; set; }
    }
}
```

Bu özniteliğin varlığı, Kullanıcı arabirimi davranışını değiştirir ve temel alınan JSON şemasını değiştirir:

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

API denetleyicisine `[Produces("application/json")]` özniteliğini ekleyin. Amaç, denetleyicinin eylemlerinin bir *uygulama/JSON*yanıt içerik türünü desteklediğini bildirsağlamaktır:

```csharp
[Produces("application/json")]
[Route("api/[controller]")]
[ApiController]
public class TodoController : ControllerBase
{
    private readonly TodoContext _context;
```
**Yanıt Içerik türü** açılan liste, denetleyicinin al eylemleri için varsayılan olarak bu içerik türünü seçer:

![Varsayılan yanıt içerik türüyle Swagger Kullanıcı arabirimi](sample_images/json-response-content-type.png)

Web API 'sindeki veri ek açıklamaların kullanımı arttıkça, UI ve API Yardım sayfaları daha açıklayıcı ve yararlı hale gelir.

### <a name="describe-response-types"></a>Yanıt türlerini açıkla

Web API kullanan geliştiriciler, özellikle yanıt türleri ve hata kodları (Standart değilse)&mdash;döndürülmesiyle ilgilidir. Yanıt türleri ve hata kodları XML açıklamaları ve veri ek açıklamalarında gösterilir.

`Create` eylemi başarılı olduğunda bir HTTP 201 durum kodu döndürür. Postalanan istek gövdesi null olduğunda bir HTTP 400 durum kodu döndürülür. Swagger Kullanıcı arabiriminde doğru belgeler olmadan, tüketici beklenen bu sonuçlar hakkında bilgi sahibi yoktur. Aşağıdaki örneğe vurgulanan satırları ekleyerek bu sorunu giderebilirsiniz:

```csharp
/// <returns>A newly created TodoItem</returns>
/// <response code="201">Returns the newly created item</response>
/// <response code="400">If the item is null</response>            
[HttpPost]
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public ActionResult<TodoItem> Create(TodoItem item)
```

Swagger Kullanıcı arabirimi artık beklenen HTTP yanıt kodlarını açıkça belgelemektedir:

![Yanıt Iletileri ' ndeki durum kodu ve nedeni için, POST Response sınıfının Description ', yeni oluşturulan Todo öğesini döndürür ve ' 400-öğesi null ise '](sample_images/data-annotations-response-types.png)

ASP.NET Core 2,2 veya üzeri sürümlerde, kurallar `[ProducesResponseType]`ile tek tek eylemleri açıkça dekorasyon alternatif olarak kullanılabilir. Daha fazla bilgi için bkz. [Web API kurallarını kullanma](https://docs.microsoft.com/aspnet/core/web-api/advanced/conventions).

Kullanıcı arabirimini özelleştirme hakkında bilgi için bkz. [Kullanıcı arabirimini özelleştirme](/aspnet/core/tutorials/getting-started-with-swashbuckle?#customize-and-extend)
