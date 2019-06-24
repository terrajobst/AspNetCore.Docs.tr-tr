---
title: NSwag ve ASP.NET Core ile çalışmaya başlama
author: zuckerthoben
description: NSwag belgeler oluşturmak ve Yardım sayfaları için bir ASP.NET Core web API'sini kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2019
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: c5b2dc47328d6d3c271a87579fa8c300109bd734
ms.sourcegitcommit: 06a455d63ff7d6b571ca832e8117f4ac9d646baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67316550"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="45b04-103">NSwag ve ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="45b04-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="45b04-104">Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben), [Riko Suter](https://rsuter.com), ve [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="45b04-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), and [Dave Brock](https://twitter.com/daveabrock)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="45b04-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="45b04-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="45b04-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="45b04-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="45b04-107">NSwag aşağıdaki özellikleri sunar:</span><span class="sxs-lookup"><span data-stu-id="45b04-107">NSwag offers the following capabilities:</span></span>

* <span data-ttu-id="45b04-108">Swagger kullanıcı arabirimini ve Swagger kullanmaya özelliği Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="45b04-108">The ability to utilize the Swagger UI and Swagger generator.</span></span>
* <span data-ttu-id="45b04-109">Esnek kod oluşturma özellikleri.</span><span class="sxs-lookup"><span data-stu-id="45b04-109">Flexible code generation capabilities.</span></span>

<span data-ttu-id="45b04-110">NSwag kullanmaya, mevcut bir API'ye ihtiyacınız olmayan&mdash;Swagger birleştirmek ve bir istemci uygulaması oluşturacak üçüncü taraf API'leri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45b04-110">With NSwag, you don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and generate a client implementation.</span></span> <span data-ttu-id="45b04-111">NSwag geliştirme döngüsü hızlandırmak ve API değişiklikleri kolayca uyum sağlar.</span><span class="sxs-lookup"><span data-stu-id="45b04-111">NSwag allows you to expedite the development cycle and easily adapt to API changes.</span></span>

## <a name="register-the-nswag-middleware"></a><span data-ttu-id="45b04-112">NSwag ara yazılım kaydetme</span><span class="sxs-lookup"><span data-stu-id="45b04-112">Register the NSwag middleware</span></span>

<span data-ttu-id="45b04-113">NSwag Ara yazılımıyla kaydedin:</span><span class="sxs-lookup"><span data-stu-id="45b04-113">Register the NSwag middleware to:</span></span>

* <span data-ttu-id="45b04-114">Uygulanan web API'si için Swagger belirtimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="45b04-114">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="45b04-115">Swagger göz atın ve web API'si test etmek için kullanıcı Arabirimi işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="45b04-115">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="45b04-116">Kullanılacak [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core ara yazılım, yükleme [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="45b04-116">To use the [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core middleware, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="45b04-117">Bu paket oluşturmak ve Swagger belirtimi, hizmet için bir ara yazılım içeren Swagger kullanıcı arabirimini (v2 ve v3) ve [ReDoc UI](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="45b04-117">This package contains the middleware to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="45b04-118">NSwag NuGet paketini yüklemek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="45b04-118">Use one of the following approaches to install the NSwag NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="45b04-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45b04-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="45b04-120">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="45b04-120">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="45b04-121">Git **görünümü** > **diğer Windows** > **Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="45b04-121">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="45b04-122">Dizin gidin *TodoApi.csproj* dosya var</span><span class="sxs-lookup"><span data-stu-id="45b04-122">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="45b04-123">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="45b04-123">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="45b04-124">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="45b04-124">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="45b04-125">Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="45b04-125">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="45b04-126">Ayarlama **paket kaynağı** "nuget.org'da"</span><span class="sxs-lookup"><span data-stu-id="45b04-126">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="45b04-127">Arama kutusuna "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="45b04-127">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="45b04-128">"NSwag.AspNetCore" paketinden seçin **Gözat** sekmesine **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="45b04-128">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="45b04-129">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45b04-129">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="45b04-130">Sağ *paketleri* klasöründe **çözüm bölmesi** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="45b04-130">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="45b04-131">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" açılır menüsünü</span><span class="sxs-lookup"><span data-stu-id="45b04-131">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="45b04-132">Arama kutusuna "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="45b04-132">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="45b04-133">Sonuçlar bölmesinde "NSwag.AspNetCore" paketi seçin ve tıklayın **' paket Ekle**</span><span class="sxs-lookup"><span data-stu-id="45b04-133">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="45b04-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="45b04-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="45b04-135">Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:</span><span class="sxs-lookup"><span data-stu-id="45b04-135">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="45b04-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="45b04-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="45b04-137">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="45b04-137">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="45b04-138">Ekleme ve Swagger ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="45b04-138">Add and configure Swagger middleware</span></span>

<span data-ttu-id="45b04-139">Ekleyin ve aşağıdaki adımları uygulayarak Swagger kullanarak ASP.NET Core uygulamanızı yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="45b04-139">Add and configure Swagger in your ASP.NET Core app by performing the following steps:</span></span>

* <span data-ttu-id="45b04-140">İçinde `Startup.ConfigureServices` yöntemi, gerekli Swagger hizmetler kaydedin:</span><span class="sxs-lookup"><span data-stu-id="45b04-140">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* <span data-ttu-id="45b04-141">İçinde `Startup.Configure` yöntemi, oluşturulan Swagger belirtimi ve Swagger kullanıcı arabirimini sunulması için Ara yazılımlarını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="45b04-141">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* <span data-ttu-id="45b04-142">Uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="45b04-142">Launch the app.</span></span> <span data-ttu-id="45b04-143">Şuraya gidin:</span><span class="sxs-lookup"><span data-stu-id="45b04-143">Navigate to:</span></span>
  * <span data-ttu-id="45b04-144">`http://localhost:<port>/swagger` Swagger kullanıcı arabirimini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="45b04-144">`http://localhost:<port>/swagger` to view the Swagger UI.</span></span>
  * <span data-ttu-id="45b04-145">`http://localhost:<port>/swagger/v1/swagger.json` Swagger belirtimi görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="45b04-145">`http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="45b04-146">Kod oluşturma</span><span class="sxs-lookup"><span data-stu-id="45b04-146">Code generation</span></span>

<span data-ttu-id="45b04-147">Aşağıdaki seçeneklerden birini seçerek NSwag'ın kod oluşturma özelliklerinden gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="45b04-147">You can take advantage of NSwag's code generation capabilities by choosing one of the following options:</span></span>

* <span data-ttu-id="45b04-148">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; API İstemci kodu oluşturmak için bir Windows masaüstü uygulaması C# veya TypeScript.</span><span class="sxs-lookup"><span data-stu-id="45b04-148">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; a Windows desktop app for generating API client code in C# or TypeScript.</span></span>
* <span data-ttu-id="45b04-149">[NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) veya [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet paketlerini projenize içinde kod oluşturma için.</span><span class="sxs-lookup"><span data-stu-id="45b04-149">The [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages for code generation inside your project.</span></span>
* <span data-ttu-id="45b04-150">NSwag gelen [komut satırı](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="45b04-150">NSwag from the [command line](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="45b04-151">[NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/MSBuild) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="45b04-151">The [NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/MSBuild) NuGet package.</span></span>
* <span data-ttu-id="45b04-152">[Unchase Openapı (Swagger) bağlı hizmet](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; API İstemci kodu oluşturmak için bir Visual Studio bağlı hizmeti C# veya TypeScript.</span><span class="sxs-lookup"><span data-stu-id="45b04-152">The [Unchase OpenAPI (Swagger) Connected Service](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; a Visual Studio Connected Service for generating API client code in C# or TypeScript.</span></span> <span data-ttu-id="45b04-153">Ayrıca oluşturur C# NSwag Openapı hizmetleriyle denetleyicileri.</span><span class="sxs-lookup"><span data-stu-id="45b04-153">Also generates C# controllers for OpenAPI services with NSwag.</span></span>

### <a name="generate-code-with-nswagstudio"></a><span data-ttu-id="45b04-154">NSwagStudio ile kodu oluştur</span><span class="sxs-lookup"><span data-stu-id="45b04-154">Generate code with NSwagStudio</span></span>

* <span data-ttu-id="45b04-155">Yönergeleri izleyerek NSwagStudio yükleme [NSwagStudio GitHub deposu](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="45b04-155">Install NSwagStudio by following the instructions at the [NSwagStudio GitHub repository](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="45b04-156">NSwagStudio başlatın ve girin *swagger.json* URL'SİNDE dosya **Swagger belirtimi URL'si** metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="45b04-156">Launch NSwagStudio and enter the *swagger.json* file URL in the **Swagger Specification URL** text box.</span></span> <span data-ttu-id="45b04-157">Örneğin, *http://localhost:44354/swagger/v1/swagger.json* .</span><span class="sxs-lookup"><span data-stu-id="45b04-157">For example, *http://localhost:44354/swagger/v1/swagger.json*.</span></span>
* <span data-ttu-id="45b04-158">Tıklayın **yerel kopya oluşturmak** , Swagger belirtimi JSON temsilini üretmek için düğme.</span><span class="sxs-lookup"><span data-stu-id="45b04-158">Click the **Create local Copy** button to generate a JSON representation of your Swagger specification.</span></span>

  ![Swagger belirtimi yerel kopyasını oluşturma](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* <span data-ttu-id="45b04-160">İçinde **çıkışları** alanı tıklayın **CSharp istemci** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="45b04-160">In the **Outputs** area, click the **CSharp Client** check box.</span></span> <span data-ttu-id="45b04-161">Projenize bağlı olarak ayrıca seçebilirsiniz **TypeScript istemci** veya **CSharp Web API denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="45b04-161">Depending on your project, you can also choose **TypeScript Client** or **CSharp Web API Controller**.</span></span> <span data-ttu-id="45b04-162">Seçerseniz **CSharp Web API denetleyicisi**, geriye doğru bir nesil olarak hizmet veren hizmet, hizmet belirtimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="45b04-162">If you select **CSharp Web API Controller**, a service specification rebuilds the service, serving as a reverse generation.</span></span>
* <span data-ttu-id="45b04-163">Tıklayın **Oluştur çıkışları** tam üretmek için C# istemci uygulaması *TodoApi.NSwag* proje.</span><span class="sxs-lookup"><span data-stu-id="45b04-163">Click **Generate Outputs** to produce a complete C# client implementation of the *TodoApi.NSwag* project.</span></span> <span data-ttu-id="45b04-164">Oluşturulan istemci kodu görmek için tıklayın **CSharp istemci** sekmesinde:</span><span class="sxs-lookup"><span data-stu-id="45b04-164">To see the generated client code, click the **CSharp Client** tab:</span></span>

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
> <span data-ttu-id="45b04-165">C# İstemci kodu seçimleri göre oluşturulan **ayarları** sekmesi. Varsayılan ad alanı yeniden adlandırma ve zaman uyumlu yöntemi oluşturma gibi görevleri gerçekleştirmek için ayarları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="45b04-165">The C# client code is generated based on selections in the **Settings** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="45b04-166">Oluşturulan kopyalama C# API'yi kullanacak istemci projesindeki bir dosyaya kod.</span><span class="sxs-lookup"><span data-stu-id="45b04-166">Copy the generated C# code into a file in the client project that will consume the API.</span></span>
* <span data-ttu-id="45b04-167">Web API'sini kullanan başlatın:</span><span class="sxs-lookup"><span data-stu-id="45b04-167">Start consuming the web API:</span></span>

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a><span data-ttu-id="45b04-168">API belgeleri özelleştirme</span><span class="sxs-lookup"><span data-stu-id="45b04-168">Customize API documentation</span></span>

<span data-ttu-id="45b04-169">Swagger web API'si kullanımını kolaylaştırmak için nesne modeli belgelemek için seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="45b04-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="45b04-170">API bilgisi ve açıklama</span><span class="sxs-lookup"><span data-stu-id="45b04-170">API info and description</span></span>

<span data-ttu-id="45b04-171">İçinde `Startup.ConfigureServices` yapılandırma eylem yöntemi, geçilen `AddSwaggerDocument` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:</span><span class="sxs-lookup"><span data-stu-id="45b04-171">In the `Startup.ConfigureServices` method, a configuration action passed to the `AddSwaggerDocument` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

<span data-ttu-id="45b04-172">Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="45b04-172">The Swagger UI displays the version's information:</span></span>

![Swagger kullanıcı Arabirimi ile sürüm bilgileri](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="45b04-174">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="45b04-174">XML comments</span></span>

<span data-ttu-id="45b04-175">XML açıklamaları etkinleştirmek için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="45b04-175">To enable XML comments, perform the following steps:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="45b04-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45b04-176">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="45b04-177">Projeye sağ **Çözüm Gezgini** seçip **< project_name > .csproj Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="45b04-177">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="45b04-178">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="45b04-178">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="45b04-179">Projeye sağ **Çözüm Gezgini** seçip **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="45b04-179">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="45b04-180">Denetleme **XML belge dosyası** altında kutusunda **çıkış** bölümünü **derleme** sekmesi</span><span class="sxs-lookup"><span data-stu-id="45b04-180">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="45b04-181">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45b04-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="45b04-182">Gelen *çözüm bölmesi*, basın **denetim** ve proje adına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="45b04-182">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="45b04-183">Gidin **Araçları** > **dosyasını düzenleyin**.</span><span class="sxs-lookup"><span data-stu-id="45b04-183">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="45b04-184">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="45b04-184">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="45b04-185">Açık **proje seçenekleri** iletişim > **derleme** > **derleyici**</span><span class="sxs-lookup"><span data-stu-id="45b04-185">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="45b04-186">Denetleme **xml belgeleri oluştur** altında kutusunda **genel seçenekleri** bölümü</span><span class="sxs-lookup"><span data-stu-id="45b04-186">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="45b04-187">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="45b04-187">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="45b04-188">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="45b04-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="45b04-189">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="45b04-189">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="45b04-190">NSwag kullandığından [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü için web API eylemlerini [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), eyleminiz ne yaptığını ve hangi döndürür çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="45b04-190">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), it can't infer what your action is doing and what it returns.</span></span>

<span data-ttu-id="45b04-191">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="45b04-191">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="45b04-192">Önceki eylemi döndürür `IActionResult`, ancak eylemi içinde ya da döndürmektir [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) veya [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span><span class="sxs-lookup"><span data-stu-id="45b04-192">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) or [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span></span> <span data-ttu-id="45b04-193">Veri ek açıklamaları, istemciler bu eylem geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="45b04-193">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="45b04-194">Aşağıdaki özniteliklerle eylem süslemek:</span><span class="sxs-lookup"><span data-stu-id="45b04-194">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 <span data-ttu-id="45b04-195">NSwag kullandığından [yansıma](/dotnet/csharp/programming-guide/concepts/reflection), ve önerilen dönüş türü için web API eylemlerini [actionresult öğesini\<T >](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), dönüş türü tarafından tanımlanan yalnızca çıkarımını `T`.</span><span class="sxs-lookup"><span data-stu-id="45b04-195">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), it can only infer the return type defined by `T`.</span></span> <span data-ttu-id="45b04-196">Otomatik olarak diğer olası dönüş türü çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="45b04-196">You can't automatically infer other possible return types.</span></span>

<span data-ttu-id="45b04-197">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="45b04-197">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="45b04-198">Önceki eylemi döndürür `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="45b04-198">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="45b04-199">Eylem içinde döndüren [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span><span class="sxs-lookup"><span data-stu-id="45b04-199">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="45b04-200">Denetleyici ile donatılmış olduğundan [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) öznitelik, bir [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) mümkün çok olan yanıt.</span><span class="sxs-lookup"><span data-stu-id="45b04-200">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="45b04-201">Daha fazla bilgi için [otomatik HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="45b04-201">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="45b04-202">Veri ek açıklamaları, istemciler bu eylem geri dönmek için bilinen hangi HTTP durum kodları bildirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="45b04-202">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="45b04-203">Aşağıdaki özniteliklerle eylem süslemek:</span><span class="sxs-lookup"><span data-stu-id="45b04-203">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="45b04-204">ASP.NET Core 2.2 veya sonraki sürümlerde, tek tek eylemlerle açıkça dekorasyon yerine kuralları kullanabilirsiniz `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="45b04-204">In ASP.NET Core 2.2 or later, you can use conventions instead of explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="45b04-205">Daha fazla bilgi için bkz. <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="45b04-205">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="45b04-206">Swagger Oluşturucusu artık doğru bir şekilde bu eylem tanımlayabilir ve uç noktası çağrısı yapıldığında ne aldıkları oluşturulan istemciler bildirin.</span><span class="sxs-lookup"><span data-stu-id="45b04-206">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="45b04-207">Bir öneri olarak, bu özniteliklere sahip tüm eylemleri süslemek.</span><span class="sxs-lookup"><span data-stu-id="45b04-207">As a recommendation, decorate all actions with these attributes.</span></span>

<span data-ttu-id="45b04-208">API eylemlerinizi döndürmelidir HTTP yanıtları hakkında yönergeler için bkz: [RFC 7231 belirtimi](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="45b04-208">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
