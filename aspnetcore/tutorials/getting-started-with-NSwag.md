---
title: "NSwag ile çalışmaya başlama"
author: zuckerthoben
description: "Bu öğretici belgeleri oluşturmak ve bir Web API uygulaması için sayfa yardımcı olmak için NSwag ekleme bir kılavuz sağlar."
keywords: "ASP.NET Core, Swagger, NSwag, sayfalar, Web API Yardım"
ms.author: scaddie
manager: wpickett
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 70923210ef8f5b23ddd3cc74efceb05cbd90a179
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="get-started-with-nswag"></a><span data-ttu-id="b0f34-104">NSwag ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="b0f34-104">Get started with NSwag</span></span>

<span data-ttu-id="b0f34-105">Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="b0f34-105">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="b0f34-106">Kullanarak [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core ile ara yazılımı gerektirir [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="b0f34-106">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="b0f34-107">Swagger kullanıcı arabirimini (v2 ve v3), bir Swagger oluşturucuyu paketi oluşur ve [ReDoc UI](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="b0f34-107">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="b0f34-108">Yüksek oranda NSwag, kullanıcının kullanmak için önerilen code oluşturma özellikleri.</span><span class="sxs-lookup"><span data-stu-id="b0f34-108">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="b0f34-109">Kod oluşturma için aşağıdaki seçeneklerden birini seçin:</span><span class="sxs-lookup"><span data-stu-id="b0f34-109">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="b0f34-110">Kullanım [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), C# ve TypeScript API için istemci kodu oluşturmak için bir Windows masaüstü uygulaması</span><span class="sxs-lookup"><span data-stu-id="b0f34-110">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="b0f34-111">Kullanım [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) veya [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) kod oluşturmayı projenizi içinde için NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="b0f34-111">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="b0f34-112">Gelen NSwag kullanmak [komut satırı](https://github.com/NSwag/NSwag/wiki/CommandLine)</span><span class="sxs-lookup"><span data-stu-id="b0f34-112">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="b0f34-113">Kullanım [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="b0f34-113">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="b0f34-114">Özellikler</span><span class="sxs-lookup"><span data-stu-id="b0f34-114">Features</span></span>

<span data-ttu-id="b0f34-115">NSwag kullanmak için ana nedeni Swagger kullanıcı arabirimini ve Swagger yalnızca tanıtmak için yeteneğidir yapmak ancak Oluşturucu, esnek kod oluşturma özelliklerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0f34-115">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="b0f34-116">Varolan bir API gerekmeyen&mdash;Swagger birleştirme ve bir istemci uygulaması oluşturmak NSwag izin üçüncü taraf API'leri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0f34-116">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="b0f34-117">Her iki durumda da geliştirme döngüsü öncelikli ve API değişiklikler daha kolayca uyarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0f34-117">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="b0f34-118">Paket yükleme</span><span class="sxs-lookup"><span data-stu-id="b0f34-118">Package installation</span></span>

<span data-ttu-id="b0f34-119">NSwag NuGet paketi ile aşağıdaki yaklaşımlardan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="b0f34-119">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b0f34-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b0f34-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b0f34-121">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="b0f34-121">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="b0f34-122">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="b0f34-122">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="b0f34-123">Projenize sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="b0f34-123">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="b0f34-124">Ayarlama **paket kaynağı** "nuget.org" için</span><span class="sxs-lookup"><span data-stu-id="b0f34-124">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="b0f34-125">Arama kutusuna "NSwag.AspNetCore" girin</span><span class="sxs-lookup"><span data-stu-id="b0f34-125">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="b0f34-126">"NSwag.AspNetCore" paketten seçin **Gözat** sekmesinde **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="b0f34-126">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b0f34-127">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b0f34-127">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b0f34-128">Sağ *paketleri* klasöründe **çözüm paneli** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="b0f34-128">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="b0f34-129">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" için açılır</span><span class="sxs-lookup"><span data-stu-id="b0f34-129">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="b0f34-130">Arama kutusuna NSwag.AspNetCore girin</span><span class="sxs-lookup"><span data-stu-id="b0f34-130">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="b0f34-131">Sonuçlar bölmesinde NSwag.AspNetCore paketi seçin ve **Paketi Ekle**</span><span class="sxs-lookup"><span data-stu-id="b0f34-131">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b0f34-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b0f34-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b0f34-133">Aşağıdaki komutu çalıştırın **tümleşik Terminal**:</span><span class="sxs-lookup"><span data-stu-id="b0f34-133">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b0f34-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b0f34-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b0f34-135">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b0f34-135">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

# <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="b0f34-136">Ekleme ve Swagger ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0f34-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="b0f34-137">Aşağıdaki ad alanlarında alma `Info` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="b0f34-137">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="b0f34-138">İçinde `Startup.Configure` yöntemi, oluşturulan Swagger belirtimi ve Swagger kullanıcı arabirimini hizmet veren ara yazılımı etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="b0f34-138">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="b0f34-139">Uygulamasını başlatın.</span><span class="sxs-lookup"><span data-stu-id="b0f34-139">Launch the app.</span></span> <span data-ttu-id="b0f34-140">Gidin `/swagger` Swagger kullanıcı arabirimini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="b0f34-140">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="b0f34-141">Gidin `/swagger/v1/swagger.json` Swagger belirtimi görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="b0f34-141">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="b0f34-142">Kod oluşturma</span><span class="sxs-lookup"><span data-stu-id="b0f34-142">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="b0f34-143">Via NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="b0f34-143">Via NSwagStudio</span></span>

* <span data-ttu-id="b0f34-144">Yükleme `NSwagStudio` resmi gelen [GitHub deposunu](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="b0f34-144">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="b0f34-145">Launch NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="b0f34-145">Launch NSwagStudio.</span></span> <span data-ttu-id="b0f34-146">Konumu girin, *swagger.json* veya doğrudan kopyalayın:</span><span class="sxs-lookup"><span data-stu-id="b0f34-146">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="b0f34-148">İstenen istemci çıktı türü belirtin.</span><span class="sxs-lookup"><span data-stu-id="b0f34-148">Indicate the desired client output type.</span></span> <span data-ttu-id="b0f34-149">Seçenekleriniz **TypeScript istemci**, **CSharp istemci**, veya **CSharp Web API denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="b0f34-149">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="b0f34-150">Bir Web API denetleyicisi kullanarak, aslında bir ters oluşturma.</span><span class="sxs-lookup"><span data-stu-id="b0f34-150">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="b0f34-151">Hizmet yeniden oluşturmak için bir hizmet belirtimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0f34-151">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="b0f34-152">Tıklatın **çıkışları oluşturmak**.</span><span class="sxs-lookup"><span data-stu-id="b0f34-152">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="b0f34-153">Bir tam istemci uygulaması Burada gördüğünüz *TodoApi.NSwag* C# Örnek:</span><span class="sxs-lookup"><span data-stu-id="b0f34-153">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="b0f34-155">Bir istemci projeye dosya yerleştirme (örneğin, bir [Xamarin.Forms](https://developer.xamarin.com/guides/xamarin-forms/) uygulama).</span><span class="sxs-lookup"><span data-stu-id="b0f34-155">Put the file into a client project (for example, a [Xamarin.Forms](https://developer.xamarin.com/guides/xamarin-forms/) app).</span></span> <span data-ttu-id="b0f34-156">API tüketen başlatın:</span><span class="sxs-lookup"><span data-stu-id="b0f34-156">Start consuming the API:</span></span>

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
> <span data-ttu-id="b0f34-157">Bir temel URL'yi ve/veya HTTP istemcisi API istemcisini ekleyemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0f34-157">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="b0f34-158">En iyi uygulama her zaman olduğu [HttpClient yeniden](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="b0f34-158">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="b0f34-159">API'nizi istemci projelere kolayca uygulama şimdi başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0f34-159">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="b0f34-160">İstemci kodu oluşturmak için diğer yolları</span><span class="sxs-lookup"><span data-stu-id="b0f34-160">Other ways to generate client code</span></span>

<span data-ttu-id="b0f34-161">İş akışınıza uygun kodu daha fazla farklı yollarla oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b0f34-161">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="b0f34-162">MSBuild</span><span class="sxs-lookup"><span data-stu-id="b0f34-162">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="b0f34-163">Kod:</span><span class="sxs-lookup"><span data-stu-id="b0f34-163">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="b0f34-164">T4 şablonları</span><span class="sxs-lookup"><span data-stu-id="b0f34-164">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="b0f34-165">Özelleştirme</span><span class="sxs-lookup"><span data-stu-id="b0f34-165">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="b0f34-166">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="b0f34-166">XML comments</span></span>

<span data-ttu-id="b0f34-167">XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="b0f34-167">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="b0f34-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b0f34-168">Visual Studio</span></span>](#tab/visual-studio-xml)

* <span data-ttu-id="b0f34-169">' Nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="b0f34-169">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="b0f34-170">Denetleyin **XML belge dosyası** altında kutusunda **çıkış** bölümünü **yapı** sekmesi:</span><span class="sxs-lookup"><span data-stu-id="b0f34-170">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Proje Özellikleri'nin sekmesi oluştur](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="b0f34-172">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b0f34-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml)

* <span data-ttu-id="b0f34-173">Açık **proje seçenekleri** iletişim > **yapı** > **derleyici**</span><span class="sxs-lookup"><span data-stu-id="b0f34-173">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="b0f34-174">Denetleme **xml belgeleri oluşturmak** altında kutusunda **Genel Seçenekler** bölümü:</span><span class="sxs-lookup"><span data-stu-id="b0f34-174">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Proje seçenekleri Genel Seçenekler bölümünde](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="b0f34-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b0f34-176">Visual Studio Code</span></span>](#tab/visual-studio-code-xml)

<span data-ttu-id="b0f34-177">El ile eklemek için aşağıdaki kod parçacığını *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b0f34-177">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

---

## <a name="data-annotations"></a><span data-ttu-id="b0f34-178">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="b0f34-178">Data annotations</span></span>

<span data-ttu-id="b0f34-179">NSwag kullanan [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve Web API eylemler için en iyi deneyim getirmektir [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="b0f34-179">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="b0f34-180">Sonuç olarak, NSwag eyleminizi ne yaptığını ve ne döndürür gösterilemiyor.</span><span class="sxs-lookup"><span data-stu-id="b0f34-180">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="b0f34-181">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="b0f34-181">Consider the following example:</span></span>

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

<span data-ttu-id="b0f34-182">Önceki eylem döndürür `IActionResult`, ancak işlem içinde ya da döndürmektir [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) veya [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="b0f34-182">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="b0f34-183">Veri ek açıklamaları, istemciler bu eylem döndürme hangi HTTP yanıtı bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b0f34-183">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="b0f34-184">Aşağıdaki özniteliklerle eylem tasarlamanız:</span><span class="sxs-lookup"><span data-stu-id="b0f34-184">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="b0f34-185">Swagger Oluşturucu artık doğru şekilde bu eylem tanımlayabilir ve uç nokta çağrılırken ne aldıkları oluşturulan istemcileri biliyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="b0f34-185">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="b0f34-186">Bu öznitelikler ile tüm eylemler dekorasyon kullanmamanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="b0f34-186">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="b0f34-187">API eylemlerinizi döndürmelidir hangi HTTP yanıtları hakkında yönergeler için bkz: [RFC 7231 belirtimi](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="b0f34-187">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
