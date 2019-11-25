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
ms.openlocfilehash: d48288de90626ada83f5da1759f0057f0be46f19
ms.sourcegitcommit: f91d322f790123d41ec3271fa084ae20ed9f89a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2019
ms.locfileid: "74155151"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="69153-102">Swashbuckle ve ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="69153-102">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="69153-103">Bir Web API 'sini kullanırken, çeşitli yöntemlerini anlamak bir geliştirici için zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="69153-103">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="69153-104">[Openapı](https://www.openapis.org/)olarak da bilinen [Swagger](https://swagger.io/), Web API 'leri için faydalı belge ve yardım sayfaları oluşturma sorununu çözer.</span><span class="sxs-lookup"><span data-stu-id="69153-104">[Swagger](https://swagger.io/), also known as [OpenAPI](https://www.openapis.org/), solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="69153-105">Etkileşimli belgeler, istemci SDK 'Sı oluşturma ve API bulunabilirliği gibi avantajlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="69153-105">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="69153-106">Bu örnekte, [swashbuckle. AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) .NET uygulamasını gösteriliyor.</span><span class="sxs-lookup"><span data-stu-id="69153-106">In this sample, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) the .NET implementation is shown.</span></span>

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="69153-107">Swagger ara yazılım ekleme ve yapılandırma</span><span class="sxs-lookup"><span data-stu-id="69153-107">Add and configure Swagger middleware</span></span>

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

<span data-ttu-id="69153-108">`Startup.Configure` yönteminde, oluşturulan JSON belgesine ve Swagger Kullanıcı arabirimine hizmet veren ara yazılımı etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="69153-108">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

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

<span data-ttu-id="69153-109">Yukarıdaki `UseSwaggerUI` yöntemi çağrısı [statik dosya ara yazılımını](https://docs.microsoft.com/aspnet/core/fundamentals/static-files)sunar.</span><span class="sxs-lookup"><span data-stu-id="69153-109">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/static-files).</span></span> <span data-ttu-id="69153-110">.NET Framework veya .NET Core 1. x 'i hedefliyorsanız, projeye [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="69153-110">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="69153-111">Uygulamayı başlatın ve `http://localhost:<port>/swagger/v1/swagger.json`gidin.</span><span class="sxs-lookup"><span data-stu-id="69153-111">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="69153-112">Uç noktaları tanımlayan oluşturulan belge, [Swagger belirtiminde (Swagger. JSON)](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson)gösterildiği gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="69153-112">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="69153-113">Swagger Kullanıcı arabirimi `http://localhost:<port>/swagger`' de bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="69153-113">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="69153-114">Swagger Kullanıcı arabirimi aracılığıyla API 'YI keşfet ve diğer programlarda birleştirme.</span><span class="sxs-lookup"><span data-stu-id="69153-114">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="69153-115">Swagger Kullanıcı arabirimine uygulamanın kökünde (`http://localhost:<port>/`) hizmeti sağlamak için `RoutePrefix` özelliğini boş bir dize olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="69153-115">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> ```csharp
>app.UseSwaggerUI(c =>
>{
>    c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
>    c.RoutePrefix = string.Empty;
>});
>```

<span data-ttu-id="69153-116">IIS veya ters proxy ile dizin kullanıyorsanız, Swagger uç noktasını `./` önekini kullanarak göreli bir yol olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="69153-116">If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="69153-117">Örneğin, `./swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="69153-117">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="69153-118">`/swagger/v1/swagger.json` kullanmak, uygulamayı URL 'nin gerçek kökünde (artı kullanılıyorsa rota öneki), JSON dosyasını aramasını söyler.</span><span class="sxs-lookup"><span data-stu-id="69153-118">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="69153-119">Örneğin, `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`yerine `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` kullanın.</span><span class="sxs-lookup"><span data-stu-id="69153-119">For example, use `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

## <a name="customize-and-extend"></a><span data-ttu-id="69153-120">Özelleştirme ve genişletme</span><span class="sxs-lookup"><span data-stu-id="69153-120">Customize and extend</span></span>

<span data-ttu-id="69153-121">Swagger, nesne modelini belgeleme ve Kullanıcı arabirimini temanızla eşleşecek şekilde özelleştirme seçenekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="69153-121">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

<span data-ttu-id="69153-122">`Startup` sınıfında, aşağıdaki ad alanlarını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="69153-122">In the `Startup` class, add the following namespaces:</span></span>
```csharp
using System;
using System.Reflection;
using System.IO;
```

### <a name="api-info-and-description"></a><span data-ttu-id="69153-123">API bilgisi ve açıklaması</span><span class="sxs-lookup"><span data-stu-id="69153-123">API info and description</span></span>

<span data-ttu-id="69153-124">`AddSwaggerGen` yöntemine geçirilen yapılandırma eylemi yazar, lisans ve açıklama gibi bilgileri ekler:</span><span class="sxs-lookup"><span data-stu-id="69153-124">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

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

<span data-ttu-id="69153-125">Swagger Kullanıcı arabirimi, sürümün bilgilerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="69153-125">The Swagger UI displays the version's information:</span></span>

![Sürüm bilgileriyle Swagger Kullanıcı arabirimi: Açıklama, yazar ve daha fazla bağlantı görüntüle](sample_images/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="69153-127">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="69153-127">XML comments</span></span>

<span data-ttu-id="69153-128">XML açıklamaları aşağıdaki yaklaşımlar ile etkinleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="69153-128">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="69153-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="69153-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="69153-130">**Çözüm Gezgini** projeye sağ tıklayın ve **>. csproj Project_Name < Düzenle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="69153-130">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="69153-131">Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="69153-131">Manually add the highlighted lines to the *.csproj* file:</span></span>

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="69153-132">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="69153-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="69153-133">*Çözüm bölmesi*, **Denetim** ' e basın ve proje adına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="69153-133">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="69153-134">**Araçlar** > **dosya düzenleme**' ye gidin.</span><span class="sxs-lookup"><span data-stu-id="69153-134">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="69153-135">Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="69153-135">Manually add the highlighted lines to the *.csproj* file:</span></span>

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="69153-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="69153-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="69153-137">Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="69153-137">Manually add the highlighted lines to the *.csproj* file:</span></span>

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

---

<span data-ttu-id="69153-138">XML açıklamalarını etkinleştirmek, belgelenmemiş ortak türler ve Üyeler için hata ayıklama bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="69153-138">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="69153-139">Belgelenmemiş türler ve Üyeler uyarı iletisiyle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="69153-139">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="69153-140">Örneğin, aşağıdaki ileti 1591 uyarı kodunu ihlal eder:</span><span class="sxs-lookup"><span data-stu-id="69153-140">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="69153-141">Uyarıları proje genelinde gizlemek için, proje dosyasında yoksayılacak uyarı kodlarının noktalı virgülle ayrılmış bir listesini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="69153-141">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="69153-142">Uyarı kodlarının `$(NoWarn);` eklenmesi, [ C# varsayılan değerleri](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) de uygular.</span><span class="sxs-lookup"><span data-stu-id="69153-142">Appending the warning codes to `$(NoWarn);` applies the [C# default values](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) too.</span></span>

```xml
<NoWarn>$(NoWarn);1591</NoWarn>
```

<span data-ttu-id="69153-143">Yalnızca belirli Üyeler için uyarıları gizlemek için, kodu [#pragma uyarı](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) Önişlemci yönergeleri arasına alın.</span><span class="sxs-lookup"><span data-stu-id="69153-143">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="69153-144">Bu yaklaşım, API belgeleri aracılığıyla sunulmaması gereken kod için yararlıdır. Aşağıdaki örnekte, tüm `Program` sınıfı için uyarı kodu CS1591 yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="69153-144">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="69153-145">Uyarı kodu zorlaması sınıf tanımının kapandığına geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="69153-145">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="69153-146">Virgülle ayrılmış bir liste ile birden çok uyarı kodu belirtin.</span><span class="sxs-lookup"><span data-stu-id="69153-146">Specify multiple warning codes with a comma-delimited list.</span></span>

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

<span data-ttu-id="69153-147">Swagger 'yi yukarıdaki yönergelerle oluşturulan XML dosyasını kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="69153-147">Configure Swagger to use the XML file that's generated with the preceding instructions.</span></span> <span data-ttu-id="69153-148">Linux veya Windows dışı işletim sistemleri için dosya adları ve yolları büyük/küçük harfe duyarlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="69153-148">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="69153-149">Örneğin, *TodoApi. xml* dosyası Windows üzerinde geçerlidir ancak CentOS değildir.</span><span class="sxs-lookup"><span data-stu-id="69153-149">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

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

<span data-ttu-id="69153-150">Yukarıdaki kodda, [yansıma](/dotnet/csharp/programming-guide/concepts/reflection) , Web API projesi ile eşleşen bir XML dosya adı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="69153-150">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the web API project.</span></span> <span data-ttu-id="69153-151">[AppContext. BaseDirectory](/dotnet/api/system.appcontext.basedirectory) ÖZELLIĞI, XML dosyasının yolunu oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="69153-151">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory) property is used to construct a path to the XML file.</span></span> <span data-ttu-id="69153-152">Bazı Swagger özellikleri (örneğin, bir XML belge dosyası kullanılmadan, giriş parametrelerinin veya HTTP yöntemlerinin ve yanıt kodlarının) bir bölümü çalışır.</span><span class="sxs-lookup"><span data-stu-id="69153-152">Some Swagger features (for example, schemata of input parameters or HTTP methods and response codes from the respective attributes) work without the use of an XML documentation file.</span></span> <span data-ttu-id="69153-153">Çoğu özellik için, yöntem özetleri ve parametrelerin ve yanıt kodlarının açıklamaları, bir XML dosyası kullanımı zorunludur.</span><span class="sxs-lookup"><span data-stu-id="69153-153">For most features, namely method summaries and the descriptions of parameters and response codes, the use of an XML file is mandatory.</span></span>

<span data-ttu-id="69153-154">Bir eyleme Üçlü eğik çizgi açıklamaları eklemek, Bölüm üstbilgisine açıklama ekleyerek Swagger Kullanıcı arabirimini geliştirir.</span><span class="sxs-lookup"><span data-stu-id="69153-154">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="69153-155">`Delete` eyleminin üstüne bir [\<summary >](/dotnet/csharp/programming-guide/xmldoc/summary) öğesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="69153-155">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

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
<span data-ttu-id="69153-156">Swagger Kullanıcı arabirimi, önceki kodun `<summary>` öğesinin iç metnini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="69153-156">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![XML açıklamasını gösteren Swagger Kullanıcı arabirimi, belirli bir TodoItem siler.](sample_images/triple-slash-comments.png)

<span data-ttu-id="69153-159">Kullanıcı arabirimi, oluşturulan JSON şeması tarafından çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="69153-159">The UI is driven by the generated JSON schema:</span></span>

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
<span data-ttu-id="69153-160">`Create` Action yöntemi belgelerine bir [\<açıklamaları >](/dotnet/csharp/programming-guide/xmldoc/remarks) öğesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="69153-160">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="69153-161">`<summary>` öğesinde belirtilen bilgileri tamamlar ve daha sağlam bir Swagger Kullanıcı arabirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="69153-161">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="69153-162">`<remarks>` öğesi içeriği metin, JSON veya XML içerebilir.</span><span class="sxs-lookup"><span data-stu-id="69153-162">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

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
<span data-ttu-id="69153-163">Bu ek açıklamalarla UI geliştirmelerini göz unutmayın:</span><span class="sxs-lookup"><span data-stu-id="69153-163">Notice the UI enhancements with these additional comments:</span></span>

![Ek açıklamaların gösterildiği Swagger Kullanıcı arabirimi](sample_images/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="69153-165">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="69153-165">Data annotations</span></span>

<span data-ttu-id="69153-166">[System. ComponentModel. datanot](/dotnet/api/system.componentmodel.dataannotations) ad alanında bulunan ve bu modeli, Swagger Kullanıcı Arabirimi bileşenlerini kullanmanıza yardımcı olacak şekilde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="69153-166">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="69153-167">`TodoItem` sınıfının `Name` özelliğine `[Required]` özniteliğini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="69153-167">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

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

<span data-ttu-id="69153-168">Bu özniteliğin varlığı, Kullanıcı arabirimi davranışını değiştirir ve temel alınan JSON şemasını değiştirir:</span><span class="sxs-lookup"><span data-stu-id="69153-168">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="69153-169">API denetleyicisine `[Produces("application/json")]` özniteliğini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="69153-169">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="69153-170">Amaç, denetleyicinin eylemlerinin bir *uygulama/JSON*yanıt içerik türünü desteklediğini bildirsağlamaktır:</span><span class="sxs-lookup"><span data-stu-id="69153-170">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

```csharp
[Produces("application/json")]
[Route("api/[controller]")]
[ApiController]
public class TodoController : ControllerBase
{
    private readonly TodoContext _context;
```
<span data-ttu-id="69153-171">**Yanıt Içerik türü** açılan liste, denetleyicinin al eylemleri için varsayılan olarak bu içerik türünü seçer:</span><span class="sxs-lookup"><span data-stu-id="69153-171">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Varsayılan yanıt içerik türüyle Swagger Kullanıcı arabirimi](sample_images/json-response-content-type.png)

<span data-ttu-id="69153-173">Web API 'sindeki veri ek açıklamaların kullanımı arttıkça, UI ve API Yardım sayfaları daha açıklayıcı ve yararlı hale gelir.</span><span class="sxs-lookup"><span data-stu-id="69153-173">As the usage of data annotations in the web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="69153-174">Yanıt türlerini açıkla</span><span class="sxs-lookup"><span data-stu-id="69153-174">Describe response types</span></span>

<span data-ttu-id="69153-175">Web API kullanan geliştiriciler, özellikle yanıt türleri ve hata kodları (Standart değilse)&mdash;döndürülmesiyle ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="69153-175">Developers consuming a web API are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="69153-176">Yanıt türleri ve hata kodları XML açıklamaları ve veri ek açıklamalarında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="69153-176">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="69153-177">`Create` eylemi başarılı olduğunda bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="69153-177">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="69153-178">Postalanan istek gövdesi null olduğunda bir HTTP 400 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="69153-178">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="69153-179">Swagger Kullanıcı arabiriminde doğru belgeler olmadan, tüketici beklenen bu sonuçlar hakkında bilgi sahibi yoktur.</span><span class="sxs-lookup"><span data-stu-id="69153-179">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="69153-180">Aşağıdaki örneğe vurgulanan satırları ekleyerek bu sorunu giderebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="69153-180">Fix that problem by adding the highlighted lines in the following example:</span></span>

```csharp
/// <returns>A newly created TodoItem</returns>
/// <response code="201">Returns the newly created item</response>
/// <response code="400">If the item is null</response>            
[HttpPost]
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public ActionResult<TodoItem> Create(TodoItem item)
```

<span data-ttu-id="69153-181">Swagger Kullanıcı arabirimi artık beklenen HTTP yanıt kodlarını açıkça belgelemektedir:</span><span class="sxs-lookup"><span data-stu-id="69153-181">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Yanıt Iletileri ' ndeki durum kodu ve nedeni için, POST Response sınıfının Description ', yeni oluşturulan Todo öğesini döndürür ve ' 400-öğesi null ise '](sample_images/data-annotations-response-types.png)

<span data-ttu-id="69153-183">ASP.NET Core 2,2 veya üzeri sürümlerde, kurallar `[ProducesResponseType]`ile tek tek eylemleri açıkça dekorasyon alternatif olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="69153-183">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="69153-184">Daha fazla bilgi için bkz. [Web API kurallarını kullanma](https://docs.microsoft.com/aspnet/core/web-api/advanced/conventions).</span><span class="sxs-lookup"><span data-stu-id="69153-184">For more information, see [Use web API conventions](https://docs.microsoft.com/aspnet/core/web-api/advanced/conventions).</span></span>

<span data-ttu-id="69153-185">Kullanıcı arabirimini özelleştirme hakkında bilgi için bkz. [Kullanıcı arabirimini özelleştirme](/aspnet/core/tutorials/getting-started-with-swashbuckle?#customize-and-extend)</span><span class="sxs-lookup"><span data-stu-id="69153-185">For information on customizing the UI see: [Customize the UI](/aspnet/core/tutorials/getting-started-with-swashbuckle?#customize-and-extend)</span></span>
