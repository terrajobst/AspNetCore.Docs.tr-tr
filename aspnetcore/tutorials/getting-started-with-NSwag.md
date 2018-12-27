---
title: NSwag ve ASP.NET Core ile çalışmaya başlama
author: zuckerthoben
description: NSwag belgeler oluşturmak ve Yardım sayfaları için bir ASP.NET Core web API'sini kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/18/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 8af5bed1e042c4f6d83043b05084c51b3064a548
ms.sourcegitcommit: ea215df889e89db44037a6ac2f01baede0450da9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53595366"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="8e1cd-103">NSwag ve ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="8e1cd-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="8e1cd-104">Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="8e1cd-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8e1cd-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e1cd-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="8e1cd-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e1cd-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="8e1cd-107">NSwag middlewares için kaydedin:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-107">Register the NSwag middlewares to:</span></span>

* <span data-ttu-id="8e1cd-108">Uygulanan web API'si için Swagger belirtimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-108">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="8e1cd-109">Swagger göz atın ve web API'si test etmek için kullanıcı Arabirimi işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-109">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="8e1cd-110">Kullanılacak [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middlewares yükleme [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-110">To use the [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middlewares, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="8e1cd-111">Bu pakette oluşturup Swagger belirtimi hizmet middlewares Swagger kullanıcı arabirimini (v2 ve v3) ve [ReDoc UI](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-111">This package contains the middlewares to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="8e1cd-112">Ayrıca, NSwag ın yararlanması için tavsiye kod oluşturma özellikleri.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-112">Additionally, it's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="8e1cd-113">Kod oluşturma özelliklerini kullanmak için aşağıdaki seçeneklerden birini seçin:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-113">Choose one of the following options to use the code generation capabilities:</span></span>

* <span data-ttu-id="8e1cd-114">Kullanım [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), API'niz için C# ve TypeScript istemci kodu oluşturmak için bir Windows masaüstü uygulaması.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-114">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="8e1cd-115">Kullanım [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) veya [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) kod projeniz içindeki oluşturma için NuGet paketlerini.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-115">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="8e1cd-116">NSwag gelen kullanın [komut satırı](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-116">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="8e1cd-117">Kullanım [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-117">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="8e1cd-118">Özellikler</span><span class="sxs-lookup"><span data-stu-id="8e1cd-118">Features</span></span>

<span data-ttu-id="8e1cd-119">NSwag kullanmaya temel nedeni, yalnızca Swagger oluşturucusu ve Swagger kullanıcı arabirimini sunar, ancak ayrıca hale getirmek üzere esnek kod oluşturma özelliklerini kullanmak yeteneğidir.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-119">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to also make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="8e1cd-120">Mevcut bir API'ye ihtiyacınız olmayan&mdash;Swagger birleştirmek ve bir istemci uygulaması oluşturmak NSwag izin üçüncü taraf API'leri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-120">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="8e1cd-121">Her iki durumda da, geliştirme döngüsü öncelikli ve API değişiklikleri daha kolayca uyarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-121">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="8e1cd-122">Paket yüklemesi</span><span class="sxs-lookup"><span data-stu-id="8e1cd-122">Package installation</span></span>

<span data-ttu-id="8e1cd-123">NSwag NuGet paketi ile aşağıdaki yaklaşımlardan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-123">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e1cd-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e1cd-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8e1cd-125">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-125">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="8e1cd-126">Git **görünümü** > **diğer Windows** > **Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="8e1cd-126">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="8e1cd-127">Dizin gidin *TodoApi.csproj* dosya var</span><span class="sxs-lookup"><span data-stu-id="8e1cd-127">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="8e1cd-128">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-128">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="8e1cd-129">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-129">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="8e1cd-130">Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="8e1cd-130">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="8e1cd-131">Ayarlama **paket kaynağı** "nuget.org'da"</span><span class="sxs-lookup"><span data-stu-id="8e1cd-131">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="8e1cd-132">Arama kutusuna "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="8e1cd-132">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="8e1cd-133">"NSwag.AspNetCore" paketinden seçin **Gözat** sekmesine **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="8e1cd-133">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e1cd-134">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e1cd-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8e1cd-135">Sağ *paketleri* klasöründe **çözüm bölmesi** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="8e1cd-135">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="8e1cd-136">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" açılır menüsünü</span><span class="sxs-lookup"><span data-stu-id="8e1cd-136">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="8e1cd-137">Arama kutusuna "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="8e1cd-137">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="8e1cd-138">Sonuçlar bölmesinde "NSwag.AspNetCore" paketi seçin ve tıklayın **' paket Ekle**</span><span class="sxs-lookup"><span data-stu-id="8e1cd-138">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e1cd-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e1cd-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8e1cd-140">Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-140">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8e1cd-141">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8e1cd-141">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8e1cd-142">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-142">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="8e1cd-143">Ekleme ve Swagger ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8e1cd-143">Add and configure Swagger middleware</span></span>

<span data-ttu-id="8e1cd-144">Aşağıdaki ad alanlarında alma `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-144">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="8e1cd-145">İçinde `Startup.ConfigureServices` yöntemi, gerekli Swagger hizmetler kaydedin:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-145">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span> 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

<span data-ttu-id="8e1cd-146">İçinde `Startup.Configure` yöntemi, oluşturulan Swagger belirtimi ve Swagger kullanıcı arabirimini v3 sunulması için Ara yazılımlarını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-146">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI v3:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="8e1cd-147">Uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-147">Launch the app.</span></span> <span data-ttu-id="8e1cd-148">Gidin `http://localhost:<port>/swagger` Swagger kullanıcı arabirimini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-148">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="8e1cd-149">Gidin `http://localhost:<port>/swagger/v1/swagger.json` Swagger belirtimi görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-149">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="8e1cd-150">Kod oluşturma</span><span class="sxs-lookup"><span data-stu-id="8e1cd-150">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="8e1cd-151">NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="8e1cd-151">Via NSwagStudio</span></span>

* <span data-ttu-id="8e1cd-152">Resmi NSwagStudio yükleme [GitHub deposu](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-152">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="8e1cd-153">NSwagStudio başlatın.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-153">Launch NSwagStudio.</span></span> <span data-ttu-id="8e1cd-154">Girin *swagger.json* URL'SİNDE dosya **Swagger belirtimi URL'si** metin tıklatıp **yerel kopya oluşturmak** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-154">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="8e1cd-155">Seçin **CSharp istemci** istemci çıkış türü.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-155">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="8e1cd-156">Diğer Seçenekler **TypeScript istemci** ve **CSharp Web API denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-156">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="8e1cd-157">Bir Web API denetleyicisi kullanmaktır temelde ters oluşturma.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-157">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="8e1cd-158">Hizmet yeniden derlemek için bir hizmet belirtimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-158">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="8e1cd-159">Tıklayın **Oluştur çıkışları** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-159">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="8e1cd-160">Tam bir C# istemci uygulama, *TodoApi.NSwag* Proje oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-160">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="8e1cd-161">Tıklayın **CSharp istemci** sekmesinde **çıkışları** bölümünde oluşturulan istemci kodu görmek için:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-161">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

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
> <span data-ttu-id="8e1cd-162">C# istemci kodu içinde tanımlanan ayarları temel alınarak oluşturulur **ayarları** sekmesinde **CSharp istemci** sekmesi. Varsayılan ad alanı yeniden adlandırma ve zaman uyumlu yöntemi oluşturma gibi görevleri gerçekleştirmek için ayarları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-162">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="8e1cd-163">Bir istemci projesi dosyasında oluşturulan C# kodu kopyalayın (örneğin, bir [Xamarin.Forms](/xamarin/xamarin-forms/) uygulama).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-163">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="8e1cd-164">Web API'sini kullanan başlatın:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-164">Start consuming the web API:</span></span>

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
> <span data-ttu-id="8e1cd-165">Temel URL ve/veya HTTP istemci API istemcisi ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-165">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="8e1cd-166">Her zaman en iyi yöntem olacaktır [HttpClient yeniden](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-166">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="8e1cd-167">İstemci kodu oluşturmak için kullanabileceğiniz diğer yöntemler</span><span class="sxs-lookup"><span data-stu-id="8e1cd-167">Other ways to generate client code</span></span>

<span data-ttu-id="8e1cd-168">Akışınız için uygun istemci kodu daha fazla farklı yollarla oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-168">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="8e1cd-169">MSBuild</span><span class="sxs-lookup"><span data-stu-id="8e1cd-169">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="8e1cd-170">Kodda</span><span class="sxs-lookup"><span data-stu-id="8e1cd-170">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="8e1cd-171">T4 şablonları</span><span class="sxs-lookup"><span data-stu-id="8e1cd-171">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="8e1cd-172">Özelleştir</span><span class="sxs-lookup"><span data-stu-id="8e1cd-172">Customize</span></span>

<span data-ttu-id="8e1cd-173">Swagger web API'si kullanımını kolaylaştırmak için nesne modeli belgelemek için seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-173">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="8e1cd-174">API bilgisi ve açıklama</span><span class="sxs-lookup"><span data-stu-id="8e1cd-174">API info and description</span></span>

<span data-ttu-id="8e1cd-175">İçinde `Startup.Configure` yapılandırma eylem yöntemi, geçilen `UseSwagger` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-175">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="8e1cd-176">Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-176">The Swagger UI displays the version's information:</span></span>

![Swagger kullanıcı Arabirimi ile sürüm bilgileri](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="8e1cd-178">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="8e1cd-178">XML comments</span></span>

<span data-ttu-id="8e1cd-179">XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-179">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="8e1cd-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e1cd-180">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="8e1cd-181">Projeye sağ **Çözüm Gezgini** seçip **< project_name > .csproj Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-181">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="8e1cd-182">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="8e1cd-183">Projeye sağ **Çözüm Gezgini** seçip **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="8e1cd-183">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="8e1cd-184">Denetleme **XML belge dosyası** altında kutusunda **çıkış** bölümünü **derleme** sekmesi</span><span class="sxs-lookup"><span data-stu-id="8e1cd-184">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="8e1cd-185">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e1cd-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="8e1cd-186">Gelen *çözüm bölmesi*, basın **denetim** ve proje adına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-186">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="8e1cd-187">Gidin **Araçları** > **dosyasını düzenleyin**.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-187">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="8e1cd-188">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="8e1cd-189">Açık **proje seçenekleri** iletişim > **derleme** > **derleyici**</span><span class="sxs-lookup"><span data-stu-id="8e1cd-189">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="8e1cd-190">Denetleme **xml belgeleri oluştur** altında kutusunda **genel seçenekleri** bölümü</span><span class="sxs-lookup"><span data-stu-id="8e1cd-190">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="8e1cd-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e1cd-191">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="8e1cd-192">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-192">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="8e1cd-193">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="8e1cd-193">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="8e1cd-194">NSwag kullanan [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü için web API eylemlerini [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-194">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="8e1cd-195">Sonuç olarak, NSwag eyleminizi ne yaptığını ve hangi döndürür çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-195">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="8e1cd-196">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-196">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="8e1cd-197">Önceki eylemi döndürür `IActionResult`, ancak eylemi içinde ya da döndürmektir [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) veya [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-197">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="8e1cd-198">Veri ek açıklamaları, istemciler bu eylem geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-198">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="8e1cd-199">Aşağıdaki özniteliklerle eylem süslemek:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-199">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8e1cd-200">NSwag kullanan [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü için web API eylemlerini [actionresult öğesini\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-200">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="8e1cd-201">Sonuç olarak, NSwag tarafından tanımlanan bir dönüş türü yalnızca çıkarımını `T`.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-201">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="8e1cd-202">Diğer olası dönüş türleri uygulamada çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-202">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="8e1cd-203">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-203">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="8e1cd-204">Önceki eylemi döndürür `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-204">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="8e1cd-205">Eylem içinde döndüren [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-205">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="8e1cd-206">Denetleyici ile donatılmış olduğundan [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) öznitelik, bir [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) mümkün çok olan yanıt.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-206">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="8e1cd-207">Daha fazla bilgi için [otomatik HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-207">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="8e1cd-208">Veri ek açıklamaları, istemciler bu eylem geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-208">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="8e1cd-209">Aşağıdaki özniteliklerle eylem süslemek:</span><span class="sxs-lookup"><span data-stu-id="8e1cd-209">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="8e1cd-210">Açıkça bireysel eylemleri dekore etmeye alternatif olarak ASP.NET Core 2.2 veya sonraki sürümlerde, kuralları kullanılabilir `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-210">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="8e1cd-211">Daha fazla bilgi için bkz. <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-211">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="8e1cd-212">Swagger Oluşturucusu artık doğru bir şekilde bu eylem tanımlayabilir ve uç noktası çağrısı yapıldığında ne aldıkları oluşturulan istemciler bildirin.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-212">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="8e1cd-213">Bu öznitelikleri olan tüm eylemleri dekorasyon kesinlikle önerilir.</span><span class="sxs-lookup"><span data-stu-id="8e1cd-213">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="8e1cd-214">API eylemlerinizi döndürmelidir HTTP yanıtları hakkında yönergeler için bkz: [RFC 7231 belirtimi](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="8e1cd-214">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
