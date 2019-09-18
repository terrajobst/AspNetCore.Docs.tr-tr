---
title: NSwag ve ASP.NET Core kullanmaya başlayın
author: zuckerthoben
description: ASP.NET Core Web API 'SI için belge ve yardım sayfaları oluşturmak üzere NSwag 'yi nasıl kullanacağınızı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2019
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: cdaa5aff3007030c70bc959bbf07903bdf39e736
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082429"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="761c2-103">NSwag ve ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="761c2-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="761c2-104">, [Christoph Nienaber](https://twitter.com/zuckerthoben), [Riko Suter](https://rsuter.com)ve [bave Brock](https://twitter.com/daveabrock) tarafından</span><span class="sxs-lookup"><span data-stu-id="761c2-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), and [Dave Brock](https://twitter.com/daveabrock)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="761c2-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="761c2-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="761c2-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="761c2-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="761c2-107">NSwag aşağıdaki özellikleri sunar:</span><span class="sxs-lookup"><span data-stu-id="761c2-107">NSwag offers the following capabilities:</span></span>

* <span data-ttu-id="761c2-108">Swagger Kullanıcı arabirimi ve Swagger oluşturucuyu kullanma özelliği.</span><span class="sxs-lookup"><span data-stu-id="761c2-108">The ability to utilize the Swagger UI and Swagger generator.</span></span>
* <span data-ttu-id="761c2-109">Esnek kod oluşturma özellikleri.</span><span class="sxs-lookup"><span data-stu-id="761c2-109">Flexible code generation capabilities.</span></span>

<span data-ttu-id="761c2-110">Nswag ile, var olan bir API&mdash;'ye gerek kalmaz Swagger içeren üçüncü taraf API 'leri kullanabilir ve bir istemci uygulamasını oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="761c2-110">With NSwag, you don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and generate a client implementation.</span></span> <span data-ttu-id="761c2-111">NSwag, geliştirme döngüsünü hızlandırın ve API değişikliklerine kolayca uyum sağlar.</span><span class="sxs-lookup"><span data-stu-id="761c2-111">NSwag allows you to expedite the development cycle and easily adapt to API changes.</span></span>

## <a name="register-the-nswag-middleware"></a><span data-ttu-id="761c2-112">NSwag ara yazılımını kaydetme</span><span class="sxs-lookup"><span data-stu-id="761c2-112">Register the NSwag middleware</span></span>

<span data-ttu-id="761c2-113">NSwag ara yazılımını şu şekilde kaydedin:</span><span class="sxs-lookup"><span data-stu-id="761c2-113">Register the NSwag middleware to:</span></span>

* <span data-ttu-id="761c2-114">Uygulanan Web API 'SI için Swagger belirtimini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="761c2-114">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="761c2-115">Web API 'sini taramak ve test etmek için Swagger Kullanıcı arabirimini sunar.</span><span class="sxs-lookup"><span data-stu-id="761c2-115">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="761c2-116">[Nswag](https://github.com/RicoSuter/NSwag) ASP.NET Core ara yazılımını kullanmak Için [nswag. aspnetcore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet paketini yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="761c2-116">To use the [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core middleware, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="761c2-117">Bu paket Swagger belirtimini oluşturma ve sunma ara yazılımını, Swagger Kullanıcı arabirimini (v2 ve v3) ve [Redoc Kullanıcı arabirimini](https://github.com/Rebilly/ReDoc)içerir.</span><span class="sxs-lookup"><span data-stu-id="761c2-117">This package contains the middleware to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="761c2-118">NSwag NuGet paketini yüklemek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="761c2-118">Use one of the following approaches to install the NSwag NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="761c2-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="761c2-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="761c2-120">**Paket Yöneticisi konsol** penceresinde:</span><span class="sxs-lookup"><span data-stu-id="761c2-120">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="761c2-121"> > **Diğer**Windowspaket > **Yöneticisi konsolunu** görüntüle ' ye git</span><span class="sxs-lookup"><span data-stu-id="761c2-121">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="761c2-122">*TodoApi. csproj* dosyasının bulunduğu dizine gidin</span><span class="sxs-lookup"><span data-stu-id="761c2-122">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="761c2-123">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="761c2-123">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="761c2-124">**NuGet Paketlerini Yönet** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="761c2-124">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="761c2-125">**Çözüm Gezgini** > **NuGet Paketlerini Yönet** ' de projeye sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="761c2-125">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="761c2-126">**Paket kaynağını** "NuGet.org" olarak ayarlayın</span><span class="sxs-lookup"><span data-stu-id="761c2-126">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="761c2-127">Arama kutusuna "NSwag. AspNetCore" yazın</span><span class="sxs-lookup"><span data-stu-id="761c2-127">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="761c2-128">**Araştır** sekmesinden "NSwag. aspnetcore" paketini seçin ve sonra da **yüklensin** ' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="761c2-128">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="761c2-129">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="761c2-129">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="761c2-130">**Çözüm bölmesi** paket > **Ekle...** ' da paketler klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="761c2-130">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="761c2-131">**Paket Ekle** penceresinin **kaynak** açılan penceresini "NuGet.org" olarak ayarlayın</span><span class="sxs-lookup"><span data-stu-id="761c2-131">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="761c2-132">Arama kutusuna "NSwag. AspNetCore" yazın</span><span class="sxs-lookup"><span data-stu-id="761c2-132">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="761c2-133">Sonuçlar bölmesinden "NSwag. AspNetCore" paketini seçin ve **paket Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="761c2-133">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="761c2-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="761c2-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="761c2-135">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="761c2-135">Run the following command:</span></span>

```dotnetcli
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="761c2-136">Swagger ara yazılım ekleme ve yapılandırma</span><span class="sxs-lookup"><span data-stu-id="761c2-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="761c2-137">Aşağıdaki adımları gerçekleştirerek ASP.NET Core uygulamanızda Swagger ekleyin ve yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="761c2-137">Add and configure Swagger in your ASP.NET Core app by performing the following steps:</span></span>

* <span data-ttu-id="761c2-138">`Startup.ConfigureServices` Yönteminde, gerekli Swagger hizmetlerini kaydedin:</span><span class="sxs-lookup"><span data-stu-id="761c2-138">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* <span data-ttu-id="761c2-139">`Startup.Configure` Yönteminde, oluşturulan Swagger belirtimini ve Swagger Kullanıcı arabirimini sunan ara yazılımı etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="761c2-139">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* <span data-ttu-id="761c2-140">Uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="761c2-140">Launch the app.</span></span> <span data-ttu-id="761c2-141">Şuraya gidin:</span><span class="sxs-lookup"><span data-stu-id="761c2-141">Navigate to:</span></span>
  * <span data-ttu-id="761c2-142">`http://localhost:<port>/swagger`Swagger Kullanıcı arabirimini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="761c2-142">`http://localhost:<port>/swagger` to view the Swagger UI.</span></span>
  * <span data-ttu-id="761c2-143">`http://localhost:<port>/swagger/v1/swagger.json`Swagger belirtimini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="761c2-143">`http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="761c2-144">Kod oluşturma</span><span class="sxs-lookup"><span data-stu-id="761c2-144">Code generation</span></span>

<span data-ttu-id="761c2-145">Aşağıdaki seçeneklerden birini seçerek NSwag 'nin kod oluşturma özelliğinden yararlanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="761c2-145">You can take advantage of NSwag's code generation capabilities by choosing one of the following options:</span></span>

* <span data-ttu-id="761c2-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) veya TypeScript içinde C# API istemci kodu oluşturmaya yönelik bir Windows masaüstü uygulaması. &ndash;</span><span class="sxs-lookup"><span data-stu-id="761c2-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; a Windows desktop app for generating API client code in C# or TypeScript.</span></span>
* <span data-ttu-id="761c2-147">Projenizin içindeki kod üretimi için [Nswag. CodeGeneration. CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) veya [Nswag. CodeGeneration. TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet paketleri.</span><span class="sxs-lookup"><span data-stu-id="761c2-147">The [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages for code generation inside your project.</span></span>
* <span data-ttu-id="761c2-148">[Komut satırından](https://github.com/RicoSuter/NSwag/wiki/CommandLine)NSwag.</span><span class="sxs-lookup"><span data-stu-id="761c2-148">NSwag from the [command line](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="761c2-149">[Nswag. MSBuild](https://github.com/RicoSuter/NSwag/wiki/MSBuild) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="761c2-149">The [NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/MSBuild) NuGet package.</span></span>
* <span data-ttu-id="761c2-150">[Unchase openapı (Swagger) bağlı hizmeti](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; , veya TypeScript içinde C# API istemci kodu oluşturmak için bir Visual Studio bağlı hizmeti.</span><span class="sxs-lookup"><span data-stu-id="761c2-150">The [Unchase OpenAPI (Swagger) Connected Service](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; a Visual Studio Connected Service for generating API client code in C# or TypeScript.</span></span> <span data-ttu-id="761c2-151">Ayrıca, C# NSwag Ile openapı Hizmetleri için denetleyiciler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="761c2-151">Also generates C# controllers for OpenAPI services with NSwag.</span></span>

### <a name="generate-code-with-nswagstudio"></a><span data-ttu-id="761c2-152">NSwagStudio ile kod oluşturma</span><span class="sxs-lookup"><span data-stu-id="761c2-152">Generate code with NSwagStudio</span></span>

* <span data-ttu-id="761c2-153">[NSwagStudio GitHub deposundaki](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio)yönergeleri izleyerek NSwagStudio 'i yükler.</span><span class="sxs-lookup"><span data-stu-id="761c2-153">Install NSwagStudio by following the instructions at the [NSwagStudio GitHub repository](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="761c2-154">NSwagStudio başlatın ve **Swagger BELIRTIM URL** 'si metin kutusuna *Swagger. JSON* dosya URL 'sini girin.</span><span class="sxs-lookup"><span data-stu-id="761c2-154">Launch NSwagStudio and enter the *swagger.json* file URL in the **Swagger Specification URL** text box.</span></span> <span data-ttu-id="761c2-155">Örneğin, *http://localhost:44354/swagger/v1/swagger.json* .</span><span class="sxs-lookup"><span data-stu-id="761c2-155">For example, *http://localhost:44354/swagger/v1/swagger.json*.</span></span>
* <span data-ttu-id="761c2-156">Swagger belirtimin bir JSON gösterimini oluşturmak için **Yerel kopya oluştur** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="761c2-156">Click the **Create local Copy** button to generate a JSON representation of your Swagger specification.</span></span>

  ![Swagger belirtiminin yerel kopyasını oluştur](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* <span data-ttu-id="761c2-158">**Çıktılar** alanında, **CSharp Client** onay kutusuna tıklayın.</span><span class="sxs-lookup"><span data-stu-id="761c2-158">In the **Outputs** area, click the **CSharp Client** check box.</span></span> <span data-ttu-id="761c2-159">Projenize bağlı olarak, **TypeScript Client** veya **CSharp Web API Controller**' ı da seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="761c2-159">Depending on your project, you can also choose **TypeScript Client** or **CSharp Web API Controller**.</span></span> <span data-ttu-id="761c2-160">**CSharp Web API denetleyicisi**' ni seçerseniz, bir hizmet belirtimi hizmeti yeniden oluşturur ve bu da ters bir oluşturma görevi görür.</span><span class="sxs-lookup"><span data-stu-id="761c2-160">If you select **CSharp Web API Controller**, a service specification rebuilds the service, serving as a reverse generation.</span></span>
* <span data-ttu-id="761c2-161">*TodoApi. NSwag* projesinin tüm C# Istemci uygulamasını oluşturmak için **çıkış oluştur** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="761c2-161">Click **Generate Outputs** to produce a complete C# client implementation of the *TodoApi.NSwag* project.</span></span> <span data-ttu-id="761c2-162">Oluşturulan istemci kodunu görmek için **CSharp istemci** sekmesine tıklayın:</span><span class="sxs-lookup"><span data-stu-id="761c2-162">To see the generated client code, click the **CSharp Client** tab:</span></span>

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
> <span data-ttu-id="761c2-163">C# İstemci kodu, **Ayarlar** sekmesindeki seçimlere göre oluşturulur. Varsayılan ad alanı yeniden adlandırma ve zaman uyumlu yöntem oluşturma gibi görevleri gerçekleştirmek için ayarları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="761c2-163">The C# client code is generated based on selections in the **Settings** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="761c2-164">Oluşturulan C# kodu, istemci projesindeki bir dosyaya, API 'yi kullanacak şekilde kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="761c2-164">Copy the generated C# code into a file in the client project that will consume the API.</span></span>
* <span data-ttu-id="761c2-165">Web API 'sini kullanmayı Başlat:</span><span class="sxs-lookup"><span data-stu-id="761c2-165">Start consuming the web API:</span></span>

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a><span data-ttu-id="761c2-166">API belgelerini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="761c2-166">Customize API documentation</span></span>

<span data-ttu-id="761c2-167">Swagger, Web API 'sinin kullanımını kolaylaştırmak için nesne modelini belgeleme seçeneklerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="761c2-167">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="761c2-168">API bilgisi ve açıklaması</span><span class="sxs-lookup"><span data-stu-id="761c2-168">API info and description</span></span>

<span data-ttu-id="761c2-169">Yönteminde, `AddSwaggerDocument` yöntemine geçirilen bir yapılandırma eylemi yazar, lisans ve açıklama gibi bilgileri ekler: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="761c2-169">In the `Startup.ConfigureServices` method, a configuration action passed to the `AddSwaggerDocument` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

<span data-ttu-id="761c2-170">Swagger Kullanıcı arabirimi, sürümün bilgilerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="761c2-170">The Swagger UI displays the version's information:</span></span>

![Sürüm bilgileriyle Swagger Kullanıcı arabirimi](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="761c2-172">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="761c2-172">XML comments</span></span>

<span data-ttu-id="761c2-173">XML açıklamalarını etkinleştirmek için aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="761c2-173">To enable XML comments, perform the following steps:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="761c2-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="761c2-174">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="761c2-175">**Çözüm Gezgini** projeye sağ tıklayın ve **düzenle < Project_Name >. csproj**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="761c2-175">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="761c2-176">Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="761c2-176">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="761c2-177">**Çözüm Gezgini** ' de projeye sağ tıklayın ve **Özellikler** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="761c2-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="761c2-178">**Build** sekmesinin **output** bölümünün altındaki **XML belge dosyası** kutusunu işaretleyin</span><span class="sxs-lookup"><span data-stu-id="761c2-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="761c2-179">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="761c2-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="761c2-180">*Çözüm bölmesi*, **Denetim** ' e basın ve proje adına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="761c2-180">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="761c2-181">**Araçlar** > **dosya düzenleme**sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="761c2-181">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="761c2-182">Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="761c2-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="761c2-183">**Derleme** derleyicisi>proje> seçenekleri iletişim kutusunu açın</span><span class="sxs-lookup"><span data-stu-id="761c2-183">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="761c2-184">**Genel Seçenekler** bölümünün altındaki **XML oluştur belge** kutusunu işaretleyin</span><span class="sxs-lookup"><span data-stu-id="761c2-184">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="761c2-185">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="761c2-185">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="761c2-186">Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="761c2-186">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="761c2-187">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="761c2-187">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="761c2-188">NSwag [yansıma](/dotnet/csharp/programming-guide/concepts/reflection)kullandığından ve Web API 'si eylemleri için önerilen dönüş türü [ıactionresult](xref:Microsoft.AspNetCore.Mvc.IActionResult)olduğundan, eyleminiz ne yaptığını ve döndürdüğü şeyi çıkarmaz.</span><span class="sxs-lookup"><span data-stu-id="761c2-188">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), it can't infer what your action is doing and what it returns.</span></span>

<span data-ttu-id="761c2-189">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="761c2-189">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="761c2-190">Önceki eylem döndürülür `IActionResult`, ancak eylemin içinde [createdatroute](xref:System.Web.Http.ApiController.CreatedAtRoute*) veya [BadRequest](xref:System.Web.Http.ApiController.BadRequest*)döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="761c2-190">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) or [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span></span> <span data-ttu-id="761c2-191">İstemcilere bu eylemin hangi HTTP durum kodlarına geri dönebileceği bilinmektedir olduğunu bildirmek için veri açıklamalarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="761c2-191">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="761c2-192">Aşağıdaki özniteliklerle eylemi süslemek:</span><span class="sxs-lookup"><span data-stu-id="761c2-192">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 <span data-ttu-id="761c2-193">Nswag [yansıma](/dotnet/csharp/programming-guide/concepts/reflection)kullandığından ve Web API eylemleri için önerilen dönüş türü [\<ActionResult T >](xref:Microsoft.AspNetCore.Mvc.ActionResult%601)olduğundan, yalnızca tarafından `T`tanımlanan dönüş türünü çıkarabilir.</span><span class="sxs-lookup"><span data-stu-id="761c2-193">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), it can only infer the return type defined by `T`.</span></span> <span data-ttu-id="761c2-194">Diğer olası dönüş türlerini otomatik olarak çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="761c2-194">You can't automatically infer other possible return types.</span></span>

<span data-ttu-id="761c2-195">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="761c2-195">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="761c2-196">Önceki eylem döndürülür `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="761c2-196">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="761c2-197">Eylemin içinde, [Createdatroute](xref:System.Web.Http.ApiController.CreatedAtRoute*)döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="761c2-197">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="761c2-198">Denetleyici [[Apicontroller]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliğiyle donatılmış olduğundan, bir [rozet istek](xref:System.Web.Http.ApiController.BadRequest*) yanıtı da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="761c2-198">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="761c2-199">Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="761c2-199">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="761c2-200">İstemcilere bu eylemin hangi HTTP durum kodlarına geri dönebileceği bilinmektedir olduğunu bildirmek için veri açıklamalarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="761c2-200">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="761c2-201">Aşağıdaki özniteliklerle eylemi süslemek:</span><span class="sxs-lookup"><span data-stu-id="761c2-201">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="761c2-202">ASP.NET Core 2,2 veya üzeri sürümlerde, tek tek eylemleri ile `[ProducesResponseType]`açıkça dekorasyon yerine kuralları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="761c2-202">In ASP.NET Core 2.2 or later, you can use conventions instead of explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="761c2-203">Daha fazla bilgi için bkz. <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="761c2-203">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="761c2-204">Swagger Oluşturucusu artık bu eylemi doğru bir şekilde tanımlayabilir ve üretilen istemciler, uç noktayı çağırırken ne elde ettikleri hakkında bilgi alabilir.</span><span class="sxs-lookup"><span data-stu-id="761c2-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="761c2-205">Öneri olarak, tüm eylemleri bu özniteliklerle süsle.</span><span class="sxs-lookup"><span data-stu-id="761c2-205">As a recommendation, decorate all actions with these attributes.</span></span>

<span data-ttu-id="761c2-206">API eylemlerinizin dönmesi gereken HTTP yanıtlarının hakkında yönergeler için bkz. [RFC 7231 belirtimi](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="761c2-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
