---
title: NSwag ve ASP.NET Core kullanmaya başlama
author: zuckerthoben
description: Belgeleri oluşturmak ve ASP.NET Core web API'si için sayfa yardımcı olmak için NSwag kullanmayı öğrenin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: ab5bd13bb6ea3f38a81ecdfabe152864f60e6a16
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="5f0bf-103">NSwag ve ASP.NET Core kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="5f0bf-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="5f0bf-104">Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="5f0bf-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="5f0bf-105">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f0bf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="5f0bf-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f0bf-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="5f0bf-107">Kullanarak [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core ile ara yazılımı gerektirir [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="5f0bf-108">Swagger kullanıcı arabirimini (v2 ve v3), bir Swagger oluşturucuyu paketi oluşur ve [ReDoc UI](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="5f0bf-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="5f0bf-109">Yüksek oranda NSwag, kullanıcının kullanmak için önerilen code oluşturma özellikleri.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="5f0bf-110">Kod oluşturma için aşağıdaki seçeneklerden birini seçin:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="5f0bf-111">Kullanım [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), C# ve TypeScript API için istemci kodu oluşturmak için bir Windows masaüstü uygulaması.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="5f0bf-112">Kullanım [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) veya [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet paketlerini kod oluşturmayı projenizi içinde.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="5f0bf-113">Gelen NSwag kullanmak [komut satırı](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="5f0bf-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="5f0bf-114">Kullanım [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="5f0bf-115">Özellikler</span><span class="sxs-lookup"><span data-stu-id="5f0bf-115">Features</span></span>

<span data-ttu-id="5f0bf-116">NSwag kullanmak için ana nedeni Swagger kullanıcı arabirimini ve Swagger yalnızca tanıtmak için yeteneğidir yapmak ancak Oluşturucu, esnek kod oluşturma özelliklerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="5f0bf-117">Varolan bir API gerekmeyen&mdash;Swagger birleştirme ve bir istemci uygulaması oluşturmak NSwag izin üçüncü taraf API'leri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="5f0bf-118">Her iki durumda da geliştirme döngüsü öncelikli ve API değişiklikler daha kolayca uyarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="5f0bf-119">Paket yükleme</span><span class="sxs-lookup"><span data-stu-id="5f0bf-119">Package installation</span></span>

<span data-ttu-id="5f0bf-120">NSwag NuGet paketi ile aşağıdaki yaklaşımlardan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f0bf-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f0bf-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5f0bf-122">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="5f0bf-123">Git **Görünüm** > **diğer Windows** > **Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="5f0bf-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="5f0bf-124">Dizine gidin *TodoApi.csproj* dosya var</span><span class="sxs-lookup"><span data-stu-id="5f0bf-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="5f0bf-125">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="5f0bf-126">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="5f0bf-127">' Nde projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="5f0bf-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="5f0bf-128">Ayarlama **paket kaynağı** "nuget.org" için</span><span class="sxs-lookup"><span data-stu-id="5f0bf-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="5f0bf-129">Arama kutusuna "NSwag.AspNetCore" girin</span><span class="sxs-lookup"><span data-stu-id="5f0bf-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="5f0bf-130">"NSwag.AspNetCore" paketten seçin **Gözat** sekmesinde **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="5f0bf-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5f0bf-131">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f0bf-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5f0bf-132">Sağ *paketleri* klasöründe **çözüm paneli** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="5f0bf-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="5f0bf-133">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" için açılır</span><span class="sxs-lookup"><span data-stu-id="5f0bf-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="5f0bf-134">Arama kutusuna "NSwag.AspNetCore" girin</span><span class="sxs-lookup"><span data-stu-id="5f0bf-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="5f0bf-135">Sonuçlar bölmesinde "NSwag.AspNetCore" paketi seçin ve **Paketi Ekle**</span><span class="sxs-lookup"><span data-stu-id="5f0bf-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5f0bf-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5f0bf-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5f0bf-137">Aşağıdaki komutu çalıştırın **tümleşik Terminal**:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5f0bf-138">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5f0bf-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5f0bf-139">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="5f0bf-140">Ekleme ve Swagger ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5f0bf-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="5f0bf-141">Aşağıdaki ad alanlarında alma `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="5f0bf-142">İçinde `Startup.Configure` yöntemi, oluşturulan Swagger belirtimi ve Swagger kullanıcı arabirimini hizmet veren ara yazılımı etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="5f0bf-143">Uygulamasını başlatın.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-143">Launch the app.</span></span> <span data-ttu-id="5f0bf-144">Gidin `http://localhost:<port>/swagger` Swagger kullanıcı arabirimini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="5f0bf-145">Gidin `http://localhost:<port>/swagger/v1/swagger.json` Swagger belirtimi görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="5f0bf-146">Kod oluşturma</span><span class="sxs-lookup"><span data-stu-id="5f0bf-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="5f0bf-147">NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="5f0bf-147">Via NSwagStudio</span></span>

* <span data-ttu-id="5f0bf-148">Resmi NSwagStudio yükleme [GitHub deposunu](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="5f0bf-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="5f0bf-149">NSwagStudio başlatın.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-149">Launch NSwagStudio.</span></span> <span data-ttu-id="5f0bf-150">Girin *swagger.json* URL'de dosya **Swagger belirtimi URL** metin kutusuna, tıklatıp **yerel kopyasını oluştur** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="5f0bf-151">Seçin **CSharp istemci** istemci çıktı türü.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="5f0bf-152">Diğer Seçenekler **TypeScript istemci** ve **CSharp Web API denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="5f0bf-153">Bir Web API denetleyicisi kullanarak, aslında bir ters oluşturma.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="5f0bf-154">Hizmet yeniden oluşturmak için bir hizmet belirtimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="5f0bf-155">Tıklatın **oluşturma çıkışları** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="5f0bf-156">Bir tam C# istemci uygulaması *TodoApi.NSwag* projesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="5f0bf-157">Tıklatın **CSharp istemci** sekmesinde **çıkışları** bölümünde oluşturulan istemci kodu görmek için:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

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
> <span data-ttu-id="5f0bf-158">C# istemci kodu tanımlanan ayarlara göre oluşturulan **ayarları** sekmesinde **CSharp istemci** sekmesi. Varsayılan ad alanı yeniden adlandırma ve zaman uyumlu yöntemi oluşturma gibi görevleri gerçekleştirmek için ayarları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="5f0bf-159">İstemci proje dosyasında oluşturulan C# kod kopyalayın (örneğin, bir [Xamarin.Forms](/xamarin/xamarin-forms/) uygulama).</span><span class="sxs-lookup"><span data-stu-id="5f0bf-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="5f0bf-160">Web API kullanma başlatın:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-160">Start consuming the web API:</span></span>

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
> <span data-ttu-id="5f0bf-161">Bir temel URL'yi ve/veya HTTP istemcisi API istemcisini ekleyemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="5f0bf-162">En iyi uygulama her zaman olduğu [HttpClient yeniden](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="5f0bf-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="5f0bf-163">İstemci kodu oluşturmak için diğer yolları</span><span class="sxs-lookup"><span data-stu-id="5f0bf-163">Other ways to generate client code</span></span>

<span data-ttu-id="5f0bf-164">İş akışınıza uygun istemci kodu daha fazla farklı yollarla oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="5f0bf-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="5f0bf-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="5f0bf-166">Kod:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="5f0bf-167">T4 şablonları</span><span class="sxs-lookup"><span data-stu-id="5f0bf-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="5f0bf-168">Özelleştir</span><span class="sxs-lookup"><span data-stu-id="5f0bf-168">Customize</span></span>

<span data-ttu-id="5f0bf-169">Swagger API web kullanımını kolaylaştırmak için nesne modeli belgeleme için seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="5f0bf-170">API bilgileri ve açıklaması</span><span class="sxs-lookup"><span data-stu-id="5f0bf-170">API info and description</span></span>

<span data-ttu-id="5f0bf-171">İçinde `Startup.Configure` yöntemi, bir yapılandırma eylemi geçirilen `UseSwagger` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="5f0bf-172">Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-172">The Swagger UI displays the version's information:</span></span>

![Swagger kullanıcı Arabirimi ile sürüm bilgileri](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="5f0bf-174">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="5f0bf-174">XML comments</span></span>

<span data-ttu-id="5f0bf-175">XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-175">XML comments are enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="5f0bf-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f0bf-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="5f0bf-177">' Nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="5f0bf-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="5f0bf-178">Denetleyin **XML belge dosyası** altında kutusunda **çıkış** bölümünü **yapı** sekmesi</span><span class="sxs-lookup"><span data-stu-id="5f0bf-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="5f0bf-179">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f0bf-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="5f0bf-180">Açık **proje seçenekleri** iletişim > **yapı** > **derleyici**</span><span class="sxs-lookup"><span data-stu-id="5f0bf-180">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="5f0bf-181">Denetleme **xml belgeleri oluşturmak** altında kutusunda **Genel Seçenekler** bölümü</span><span class="sxs-lookup"><span data-stu-id="5f0bf-181">Check the **Generate xml documentation** box under the **General Options** section</span></span>

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="5f0bf-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5f0bf-182">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="5f0bf-183">El ile eklemek için aşağıdaki kod parçacığını *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-183">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a><span data-ttu-id="5f0bf-184">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="5f0bf-184">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="5f0bf-185">NSwag kullanan [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü web API eylemler için: [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="5f0bf-185">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="5f0bf-186">Sonuç olarak, NSwag eyleminizi ne yaptığını ve ne döndürür gösterilemiyor.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-186">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="5f0bf-187">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-187">Consider the following example:</span></span>

<span data-ttu-id="5f0bf-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="5f0bf-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="5f0bf-189">Önceki eylem döndürür `IActionResult`, ancak işlem içinde ya da döndürmektir [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) veya [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="5f0bf-189">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="5f0bf-190">Veri ek açıklamaları, istemciler bu eylemi geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-190">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="5f0bf-191">Aşağıdaki özniteliklerle eylem tasarlamanız:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-191">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="5f0bf-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="5f0bf-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="5f0bf-193">NSwag kullanan [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü web API eylemler için: [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="5f0bf-193">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="5f0bf-194">Sonuç olarak, NSwag tarafından tanımlanan dönüş türü yalnızca çıkarımını `T`.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-194">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="5f0bf-195">Diğer olası dönüş türleri eyleminde çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-195">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="5f0bf-196">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-196">Consider the following example:</span></span>

<span data-ttu-id="5f0bf-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="5f0bf-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="5f0bf-198">Önceki eylem döndürür `ActionResult<T>`, ancak işlem içinde ya da döndürmektir [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="5f0bf-198">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="5f0bf-199">Denetleyici ile donatılmış beri [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) öznitelik, bir [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) olası çok olan yanıt.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-199">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="5f0bf-200">Bkz: [otomatik HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-200">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="5f0bf-201">Veri ek açıklamaları, istemciler bu eylemi geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-201">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="5f0bf-202">Aşağıdaki özniteliklerle eylem tasarlamanız:</span><span class="sxs-lookup"><span data-stu-id="5f0bf-202">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="5f0bf-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="5f0bf-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end

<span data-ttu-id="5f0bf-204">Swagger Oluşturucu artık doğru şekilde bu eylem tanımlayabilir ve uç nokta çağrılırken ne aldıkları oluşturulan istemcileri biliyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="5f0bf-205">Bu öznitelikler ile tüm eylemler dekorasyon kullanmamanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="5f0bf-205">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="5f0bf-206">API eylemlerinizi döndürmelidir hangi HTTP yanıtları hakkında yönergeler için bkz: [RFC 7231 belirtimi](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="5f0bf-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
