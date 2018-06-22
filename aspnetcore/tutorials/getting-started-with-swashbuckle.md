---
title: Swashbuckle ve ASP.NET Core kullanmaya başlama
author: zuckerthoben
description: Swagger kullanıcı arabirimini tümleştirmek için ASP.NET Core web API projesi için Swashbuckle eklemeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/31/2018
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 7a1fdad874211134308ea3feac3110ea38095d49
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274462"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="5b22e-103">Swashbuckle ve ASP.NET Core kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="5b22e-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="5b22e-104">Tarafından [Shayne Boyer](https://twitter.com/spboyer) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="5b22e-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="5b22e-105">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b22e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5b22e-106">Swashbuckle için üç ana bileşeni vardır:</span><span class="sxs-lookup"><span data-stu-id="5b22e-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="5b22e-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger nesne modeli ve kullanıma sunmak için ara yazılım `SwaggerDocument` nesneleri JSON uç noktalar olarak.</span><span class="sxs-lookup"><span data-stu-id="5b22e-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="5b22e-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): derlemeleri bir Swagger oluşturucuyu `SwaggerDocument` nesneleri doğrudan rotalara, denetleyicilere ve modeller.</span><span class="sxs-lookup"><span data-stu-id="5b22e-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="5b22e-109">Bu, genellikle otomatik olarak Swagger JSON kullanıma sunmak için Swagger uç nokta ara yazılımı ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="5b22e-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): Swagger kullanıcı Arabirimi aracını katıştırılmış bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="5b22e-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="5b22e-111">Web API işlevleri açıklamak için zengin, özelleştirilebilir bir deneyim oluşturmak için Swagger JSON yorumlar.</span><span class="sxs-lookup"><span data-stu-id="5b22e-111">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="5b22e-112">Genel yöntemler için yerleşik test harnesses içerir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="5b22e-113">Paket yükleme</span><span class="sxs-lookup"><span data-stu-id="5b22e-113">Package installation</span></span>

<span data-ttu-id="5b22e-114">Swashbuckle ile aşağıdaki yaklaşımlardan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="5b22e-114">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5b22e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b22e-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5b22e-116">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="5b22e-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="5b22e-117">Git **Görünüm** > **diğer Windows** > **Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="5b22e-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="5b22e-118">Dizine gidin *TodoApi.csproj* dosya var</span><span class="sxs-lookup"><span data-stu-id="5b22e-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="5b22e-119">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="5b22e-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="5b22e-120">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="5b22e-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="5b22e-121">' Nde projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="5b22e-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="5b22e-122">Ayarlama **paket kaynağı** "nuget.org" için</span><span class="sxs-lookup"><span data-stu-id="5b22e-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="5b22e-123">Arama kutusuna "Swashbuckle.AspNetCore" girin</span><span class="sxs-lookup"><span data-stu-id="5b22e-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="5b22e-124">"Swashbuckle.AspNetCore" paketten seçin **Gözat** sekmesinde **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="5b22e-124">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5b22e-125">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b22e-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5b22e-126">Sağ *paketleri* klasöründe **çözüm paneli** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="5b22e-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="5b22e-127">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" için açılır</span><span class="sxs-lookup"><span data-stu-id="5b22e-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="5b22e-128">Arama kutusuna "Swashbuckle.AspNetCore" girin</span><span class="sxs-lookup"><span data-stu-id="5b22e-128">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="5b22e-129">Sonuçlar bölmesinde "Swashbuckle.AspNetCore" paketi seçin ve **Paketi Ekle**</span><span class="sxs-lookup"><span data-stu-id="5b22e-129">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5b22e-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5b22e-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5b22e-131">Aşağıdaki komutu çalıştırın **tümleşik Terminal**:</span><span class="sxs-lookup"><span data-stu-id="5b22e-131">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5b22e-132">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5b22e-132">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5b22e-133">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="5b22e-133">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="5b22e-134">Ekleme ve Swagger ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5b22e-134">Add and configure Swagger middleware</span></span>

<span data-ttu-id="5b22e-135">Hizmetleri koleksiyonunu Swagger oluşturucusunu eklemek `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5b22e-135">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="5b22e-136">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-136">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5b22e-137">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-137">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]</span></span>

::: moniker-end

<span data-ttu-id="5b22e-138">Kullanmak için aşağıdaki ad alanı içe `Info` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="5b22e-138">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="5b22e-139">İçinde `Startup.Configure` yöntemi, oluşturulan JSON belgesini ve Swagger kullanıcı arabirimini hizmet veren ara yazılımı etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="5b22e-139">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="5b22e-140">Uygulamayı başlatın ve gidin `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="5b22e-140">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="5b22e-141">Uç noktaları açıklayan oluşturulan belge gösterildiği gibi görünüyor [Swagger belirtimi (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="5b22e-141">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="5b22e-142">Swagger kullanıcı arabirimini bulunabilir `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="5b22e-142">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="5b22e-143">Swagger kullanıcı Arabirimi API inceleyin ve diğer programları içerecek.</span><span class="sxs-lookup"><span data-stu-id="5b22e-143">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="5b22e-144">Swagger kullanıcı arabirimini uygulamanızın kök dizininde hizmet (`http://localhost:<port>/`) ayarlayın `RoutePrefix` boş bir dize özelliği:</span><span class="sxs-lookup"><span data-stu-id="5b22e-144">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize-and-extend"></a><span data-ttu-id="5b22e-145">Özelleştirme ve genişletme</span><span class="sxs-lookup"><span data-stu-id="5b22e-145">Customize and extend</span></span>

<span data-ttu-id="5b22e-146">Swagger tema eşleştirmek için nesne modeli belgeleme ve kullanıcı arabirimini özelleştirmek için seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="5b22e-146">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="5b22e-147">API bilgileri ve açıklaması</span><span class="sxs-lookup"><span data-stu-id="5b22e-147">API info and description</span></span>

<span data-ttu-id="5b22e-148">Yapılandırma eylemi geçirilen `AddSwaggerGen` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:</span><span class="sxs-lookup"><span data-stu-id="5b22e-148">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="5b22e-149">Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="5b22e-149">The Swagger UI displays the version's information:</span></span>

![Sürüm bilgileri ile swagger kullanıcı Arabirimi: açıklama, yazar ve daha fazla bağlantıya bakın](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="5b22e-151">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="5b22e-151">XML comments</span></span>

<span data-ttu-id="5b22e-152">XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-152">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="5b22e-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b22e-153">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="5b22e-154">' Nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="5b22e-154">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="5b22e-155">Denetleyin **XML belge dosyası** altında kutusunda **çıkış** bölümünü **yapı** sekmesi</span><span class="sxs-lookup"><span data-stu-id="5b22e-155">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="5b22e-156">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b22e-156">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="5b22e-157">Açık **proje seçenekleri** iletişim > **yapı** > **derleyici**</span><span class="sxs-lookup"><span data-stu-id="5b22e-157">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="5b22e-158">Denetleme **xml belgeleri oluşturmak** altında kutusunda **Genel Seçenekler** bölümü</span><span class="sxs-lookup"><span data-stu-id="5b22e-158">Check the **Generate xml documentation** box under the **General Options** section</span></span>

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="5b22e-159">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5b22e-159">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="5b22e-160">El ile eklemek için aşağıdaki kod parçacığını *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5b22e-160">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

---

<span data-ttu-id="5b22e-161">XML açıklamaları etkinleştirme belgelenmemiş genel türleri ve üyeleri için hata ayıklama bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="5b22e-161">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="5b22e-162">Belgelenmemiş türleri ve üyeleri tarafından uyarı iletisi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-162">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="5b22e-163">Örneğin, aşağıdaki ileti uyarı kod 1591 ihlal gösterir:</span><span class="sxs-lookup"><span data-stu-id="5b22e-163">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="5b22e-164">Noktalı virgülle ayrılmış olarak yoksaymak için uyarı kodlarının listesini tanımlayarak uyarıları bastırma *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5b22e-164">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="5b22e-165">Oluşturulan XML dosyasını kullanmak için Swagger yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="5b22e-165">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="5b22e-166">Linux veya Windows olmayan işletim sistemleri için dosya adlarını ve yollarını büyük küçük harfe duyarlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-166">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="5b22e-167">Örneğin, bir *TodoApi.XML* Windows ancak değil CentOS dosyanın geçerli olduğu.</span><span class="sxs-lookup"><span data-stu-id="5b22e-167">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="5b22e-168">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-168">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5b22e-169">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-169">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5b22e-170">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-170">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]</span></span>

::: moniker-end

<span data-ttu-id="5b22e-171">Önceki kod [yansıma](/dotnet/csharp/programming-guide/concepts/reflection) , Web API projesi eşleşen bir XML dosya adı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5b22e-171">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="5b22e-172">Bu yaklaşım, oluşturulan XML dosya adı proje adının eşleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="5b22e-172">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="5b22e-173">[AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) özelliği XML dosyasının yolunu oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5b22e-173">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="5b22e-174">Bir eylem Üçlü eğik çizgi açıklama ekleme, Swagger kullanıcı arabirimini bölüm başlığı açıklama ekleyerek geliştirir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-174">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="5b22e-175">Ekleme bir [ \<Özet >](/dotnet/csharp/programming-guide/xmldoc/summary) öğesi yukarıdaki `Delete` eylem:</span><span class="sxs-lookup"><span data-stu-id="5b22e-175">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="5b22e-176">Önceki kodun iç metni Swagger kullanıcı arabirimini görüntüler `<summary>` öğe:</span><span class="sxs-lookup"><span data-stu-id="5b22e-176">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Swagger kullanıcı Arabirimi 'Belirli bir Todoıtem siler.' XML açıklama gösterme](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="5b22e-179">Kullanıcı Arabirimi tarafından oluşturulan JSON şeması yönetilir:</span><span class="sxs-lookup"><span data-stu-id="5b22e-179">The UI is driven by the generated JSON schema:</span></span>

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

<span data-ttu-id="5b22e-180">Ekleme bir [ \<açıklamalar >](/dotnet/csharp/programming-guide/xmldoc/remarks) öğesine `Create` eylem yöntemi belgeleri.</span><span class="sxs-lookup"><span data-stu-id="5b22e-180">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="5b22e-181">Belirtilen bilgilerini tamamlayan `<summary>` öğesi ve daha güçlü bir Swagger kullanıcı Arabirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="5b22e-181">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="5b22e-182">`<remarks>` Metin, JSON veya XML öğesi içeriği oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-182">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="5b22e-183">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-183">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5b22e-184">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-184">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span></span>

::: moniker-end

<span data-ttu-id="5b22e-185">Bu ek açıklamalar UI geliştirmeleriyle dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="5b22e-185">Notice the UI enhancements with these additional comments:</span></span>

![Swagger kullanıcı Arabirimi ile gösterilen ek açıklamaları](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="5b22e-187">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="5b22e-187">Data annotations</span></span>

<span data-ttu-id="5b22e-188">Modelin bulunan özniteliklerle tasarlamanız [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) Swagger kullanıcı Arabirimi bileşenlerini sürücü yardımcı olması için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="5b22e-188">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="5b22e-189">Ekleme `[Required]` özniteliğini `Name` özelliği `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="5b22e-189">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="5b22e-190">Bu öznitelik varlığını UI davranışını değiştiren ve temel JSON şeması değiştirir:</span><span class="sxs-lookup"><span data-stu-id="5b22e-190">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="5b22e-191">Ekleme `[Produces("application/json")]` özniteliği API denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="5b22e-191">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="5b22e-192">Amacı denetleyicinin Eylemler bir yanıt içerik türü desteği bildirmektir *uygulama/json*:</span><span class="sxs-lookup"><span data-stu-id="5b22e-192">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="5b22e-193">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-193">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5b22e-194">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-194">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span></span>

::: moniker-end

<span data-ttu-id="5b22e-195">**Yanıt içerik türü** açılan denetleyicinin GET eylemler için varsayılan olarak bu içerik türü seçer:</span><span class="sxs-lookup"><span data-stu-id="5b22e-195">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Swagger kullanıcı Arabirimi ile varsayılan yanıt içerik türü](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="5b22e-197">Web API içinde veri ek açıklamaları kullanımını arttıkça, kullanıcı Arabirimi ve API daha açıklayıcı ve kullanışlı hale sayfaları yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="5b22e-197">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="5b22e-198">Yanıt türleri açıklanmaktadır</span><span class="sxs-lookup"><span data-stu-id="5b22e-198">Describe response types</span></span>

<span data-ttu-id="5b22e-199">Süren geliştiricilerin ne döndürülen ile en ilgili&mdash;özellikle yanıt türleri ve hata kodları (değil standart değilse).</span><span class="sxs-lookup"><span data-stu-id="5b22e-199">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="5b22e-200">XML açıklamaları ve veri ek açıklamaları içinde yanıt türleri ve hata kodları gösterilir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-200">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="5b22e-201">`Create` Eylem başarı bir HTTP 201 durum kodunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="5b22e-201">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="5b22e-202">Gönderilen istek gövdesi null olduğunda bir HTTP 400 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5b22e-202">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="5b22e-203">Swagger kullanıcı arabirimini uygun belgelerinde tüketici bu beklenen sonuçları bilgisi eksik.</span><span class="sxs-lookup"><span data-stu-id="5b22e-203">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="5b22e-204">Aşağıdaki örnekte vurgulanmış satırlarını ekleyerek bu sorunu düzeltin:</span><span class="sxs-lookup"><span data-stu-id="5b22e-204">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="5b22e-205">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-205">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5b22e-206">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span><span class="sxs-lookup"><span data-stu-id="5b22e-206">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span></span>

::: moniker-end

<span data-ttu-id="5b22e-207">Swagger kullanıcı Arabirimi şimdi beklenen HTTP yanıt kodları açıkça belgeler:</span><span class="sxs-lookup"><span data-stu-id="5b22e-207">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger kullanıcı Arabirimi 'yeni oluşturulan Yapılacaklar öğesi döndürür' POST yanıt sınıf tanımına gösteren ve '400 - öğe null ise' durum kodunu ve yanıt iletileri altında nedeni](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="5b22e-209">Kullanıcı arabirimini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="5b22e-209">Customize the UI</span></span>

<span data-ttu-id="5b22e-210">UI stock işlevsel ve presentable ' dir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-210">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="5b22e-211">Ancak, API belgelerine sayfaları marka veya temayı temsil etmelidir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-211">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="5b22e-212">Swashbuckle bileşenleri markalama statik dosyaları işleme için kaynakları ekleme ve bu dosyaları barındırmak için klasör yapısı oluşturulmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-212">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="5b22e-213">.NET Framework veya .NET hedefleme varsa 1.x çekirdek, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet paketini projeye:</span><span class="sxs-lookup"><span data-stu-id="5b22e-213">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="5b22e-214">Önceki NuGet paketi zaten .NET Core hedefleme yüklü olsa 2.x ve kullanarak [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="5b22e-214">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="5b22e-215">Statik dosya ara yazılımlarını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="5b22e-215">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="5b22e-216">İçeriği alma *dağ* klasöründen [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="5b22e-216">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="5b22e-217">Bu klasör Swagger kullanıcı Arabirimi sayfası için gerekli varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="5b22e-217">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="5b22e-218">Oluşturma bir *wwwroot/swagger/UI* klasörünü ve içeriğini buraya kopyalayın *dağ* klasör.</span><span class="sxs-lookup"><span data-stu-id="5b22e-218">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="5b22e-219">Oluşturma bir *custom.css* dosyasında *wwwroot/swagger/UI*, sayfa üstbilgisi özelleştirmek için aşağıdaki CSS ile:</span><span class="sxs-lookup"><span data-stu-id="5b22e-219">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="5b22e-220">Başvuru *custom.css* içinde *index.html* dosya, diğer bir CSS dosyaları sonra:</span><span class="sxs-lookup"><span data-stu-id="5b22e-220">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="5b22e-221">Gözat *index.html* adresindeki sayfasında `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="5b22e-221">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="5b22e-222">Girin `http://localhost:<port>/swagger/v1/swagger.json` üst bilginin textbox ve tıklatın **Araştır** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="5b22e-222">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="5b22e-223">Sonuçta elde edilen sayfanın şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="5b22e-223">The resulting page looks as follows:</span></span>

![Özel üstbilgi başlık swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="5b22e-225">Çok daha fazlasını sayfasıyla yapabilirsiniz yoktur.</span><span class="sxs-lookup"><span data-stu-id="5b22e-225">There's much more you can do with the page.</span></span> <span data-ttu-id="5b22e-226">UI kaynakları tam özelliklerine bakın [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="5b22e-226">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
