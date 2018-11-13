---
title: Swashbuckle'ı ve ASP.NET Core ile çalışmaya başlama
author: zuckerthoben
description: Swagger kullanıcı arabirimini tümleştirmek için ASP.NET Core web API projesi için Swashbuckle eklemeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/05/2018
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: b932a0a494366bd13549c1c6c7f7003133d120d4
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570145"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="c5179-103">Swashbuckle'ı ve ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="c5179-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="c5179-104">Tarafından [Shayne boyer'ın](https://twitter.com/spboyer) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="c5179-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="c5179-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c5179-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c5179-106">Swashbuckle'ı için üç ana bileşeni vardır:</span><span class="sxs-lookup"><span data-stu-id="c5179-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="c5179-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger nesne modeli ve kullanıma sunmak için bir ara yazılım `SwaggerDocument` JSON uç noktalar olarak nesneleri.</span><span class="sxs-lookup"><span data-stu-id="c5179-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="c5179-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): oluşturan bir Swagger Oluşturucusu `SwaggerDocument` nesneleri doğrudan, rotalara, denetleyicilere ve modeller.</span><span class="sxs-lookup"><span data-stu-id="c5179-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="c5179-109">Bu genellikle otomatik olarak Swagger JSON kullanıma sunmak için Swagger uç nokta ara yazılımı ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c5179-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="c5179-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): katıştırılmış bir Swagger kullanıcı arabirimini aracı sürümü.</span><span class="sxs-lookup"><span data-stu-id="c5179-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="c5179-111">Bu Web API'SİNİN işlevselliğini tanımlamak için zengin, özelleştirilebilir bir deneyim oluşturmak için Swagger JSON yorumlar.</span><span class="sxs-lookup"><span data-stu-id="c5179-111">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="c5179-112">Genel metotlar için yerleşik test harnesses içerir.</span><span class="sxs-lookup"><span data-stu-id="c5179-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="c5179-113">Paket yüklemesi</span><span class="sxs-lookup"><span data-stu-id="c5179-113">Package installation</span></span>

<span data-ttu-id="c5179-114">Swashbuckle'ı ile aşağıdaki yaklaşımlardan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="c5179-114">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c5179-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5179-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c5179-116">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="c5179-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="c5179-117">Git **görünümü** > **diğer Windows** > **Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="c5179-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="c5179-118">Dizin gidin *TodoApi.csproj* dosya var</span><span class="sxs-lookup"><span data-stu-id="c5179-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="c5179-119">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="c5179-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="c5179-120">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="c5179-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="c5179-121">Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="c5179-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="c5179-122">Ayarlama **paket kaynağı** "nuget.org'da"</span><span class="sxs-lookup"><span data-stu-id="c5179-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="c5179-123">Arama kutusuna "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="c5179-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="c5179-124">"Swashbuckle.AspNetCore" paketinden seçin **Gözat** sekmesine **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="c5179-124">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c5179-125">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5179-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c5179-126">Sağ *paketleri* klasöründe **çözüm bölmesi** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="c5179-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="c5179-127">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" açılır menüsünü</span><span class="sxs-lookup"><span data-stu-id="c5179-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="c5179-128">Arama kutusuna "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="c5179-128">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="c5179-129">Sonuçlar bölmesinde "Swashbuckle.AspNetCore" paketi seçin ve tıklayın **' paket Ekle**</span><span class="sxs-lookup"><span data-stu-id="c5179-129">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c5179-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c5179-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c5179-131">Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:</span><span class="sxs-lookup"><span data-stu-id="c5179-131">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c5179-132">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c5179-132">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c5179-133">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c5179-133">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="c5179-134">Ekleme ve Swagger ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c5179-134">Add and configure Swagger middleware</span></span>

<span data-ttu-id="c5179-135">Swagger oluşturucusunu Hizmetleri koleksiyona eklemek `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c5179-135">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

<span data-ttu-id="c5179-136">Kullanmak için aşağıdaki ad alanı içe `Info` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="c5179-136">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="c5179-137">İçinde `Startup.Configure` yöntemi, oluşturulan JSON belgesini ve Swagger kullanıcı arabirimini sunulması için Ara yazılımlarını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="c5179-137">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="c5179-138">Önceki `UseSwaggerUI` yöntem çağrısının sağlayan [statik dosya ara yazılımlarını](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="c5179-138">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="c5179-139">.NET Framework veya .NET targeting, Core 1.x sürümüne, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet paketini projeye.</span><span class="sxs-lookup"><span data-stu-id="c5179-139">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="c5179-140">Uygulamayı başlatın ve gidin `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="c5179-140">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="c5179-141">Uç noktaları açıklayan oluşturulan belgenin gösterildiği görünür [Swagger belirtimi (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="c5179-141">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="c5179-142">Swagger kullanıcı arabirimini şu yolda bulunabilir: `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="c5179-142">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="c5179-143">Swagger kullanıcı Arabirimi API'yi keşfedin ve diğer programlarda dahil edilip derecelendirilir.</span><span class="sxs-lookup"><span data-stu-id="c5179-143">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="c5179-144">Swagger kullanıcı arabirimini uygulamanın kök dizininde hizmet (`http://localhost:<port>/`) ayarlayın `RoutePrefix` boş bir dize özelliğini:</span><span class="sxs-lookup"><span data-stu-id="c5179-144">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize-and-extend"></a><span data-ttu-id="c5179-145">Özelleştirme ve genişletme</span><span class="sxs-lookup"><span data-stu-id="c5179-145">Customize and extend</span></span>

<span data-ttu-id="c5179-146">Swagger temanızı eşleştirmek için nesne modeli belgelemekten ve kullanıcı arabirimini özelleştirme seçenekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="c5179-146">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="c5179-147">API bilgisi ve açıklama</span><span class="sxs-lookup"><span data-stu-id="c5179-147">API info and description</span></span>

<span data-ttu-id="c5179-148">Yapılandırma eylemi geçirilen `AddSwaggerGen` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:</span><span class="sxs-lookup"><span data-stu-id="c5179-148">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="c5179-149">Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="c5179-149">The Swagger UI displays the version's information:</span></span>

![Swagger kullanıcı Arabirimi ile sürüm bilgisi: açıklama, yazar ve daha fazla bağlantıya göz atın](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="c5179-151">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="c5179-151">XML comments</span></span>

<span data-ttu-id="c5179-152">XML açıklamaları aşağıdaki yaklaşımlardan ile etkin hale getirilebilir:</span><span class="sxs-lookup"><span data-stu-id="c5179-152">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c5179-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5179-153">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="c5179-154">Projeye sağ **Çözüm Gezgini** seçip **< project_name > .csproj Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="c5179-154">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="c5179-155">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c5179-155">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="c5179-156">Projeye sağ **Çözüm Gezgini** seçip **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="c5179-156">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="c5179-157">Denetleme **XML belge dosyası** altında kutusunda **çıkış** bölümünü **derleme** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="c5179-157">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c5179-158">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5179-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="c5179-159">Gelen *çözüm bölmesi*, basın **denetim** ve proje adına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c5179-159">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="c5179-160">Gidin **Araçları** > **dosyasını düzenleyin**.</span><span class="sxs-lookup"><span data-stu-id="c5179-160">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="c5179-161">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c5179-161">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="c5179-162">Açık **proje seçenekleri** iletişim > **derleme** > **derleyici**</span><span class="sxs-lookup"><span data-stu-id="c5179-162">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="c5179-163">Denetleme **xml belgeleri oluştur** altında kutusunda **genel seçenekleri** bölümü</span><span class="sxs-lookup"><span data-stu-id="c5179-163">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c5179-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c5179-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c5179-165">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c5179-165">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c5179-166">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c5179-166">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c5179-167">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c5179-167">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="c5179-168">XML açıklamaları'nı etkinleştirme, belgelenmemiş genel türler ve üyeler için hata ayıklama bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="c5179-168">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="c5179-169">Belgelenmemiş türleri ve üyeleri tarafından uyarı iletisinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="c5179-169">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="c5179-170">Örneğin, aşağıdaki ileti uyarı kodu 1591 ihlalini gösterir:</span><span class="sxs-lookup"><span data-stu-id="c5179-170">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="c5179-171">Proje genelinde uyarıları bastırmak için proje dosyasında yok saymak için uyarı kodlarının noktalı virgülle ayrılmış bir listesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c5179-171">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="c5179-172">Ekleme için uyarı kodlarının `$(NoWarn);` C# varsayılan değerleri çok uygular.</span><span class="sxs-lookup"><span data-stu-id="c5179-172">Appending the warning codes to `$(NoWarn);` applies the C# default values too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="c5179-173">Yalnızca belirli üyeleri için uyarıları bastırmak için kod içine [#pragma Uyarısı](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) önişlemci yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="c5179-173">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="c5179-174">Bu yaklaşım, API belgeleri kullanıma sunulan olmamalıdır kod için kullanışlıdır. Aşağıdaki örnekte, tüm uyarı kodu CS1591 göz ardı edilir `Program` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c5179-174">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="c5179-175">Zorlama uyarı kodun sınıf tanımının kapanışında geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="c5179-175">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="c5179-176">Birden çok uyarı kodlarının virgülle ayrılmış bir liste belirtin.</span><span class="sxs-lookup"><span data-stu-id="c5179-176">Specify multiple warning codes with a comma-delimited list.</span></span>

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

<span data-ttu-id="c5179-177">Oluşturulan XML dosyasını kullanmak için Swagger'ı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="c5179-177">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="c5179-178">Linux veya Windows olmayan işletim sistemleri için dosya adlarını ve yollarını büyük küçük harfe duyarlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c5179-178">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="c5179-179">Örneğin, bir *TodoApi.XML* dosya, Windows ancak değil CentOS geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c5179-179">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="c5179-180">Önceki kodda, [yansıma](/dotnet/csharp/programming-guide/concepts/reflection) Web API projesi, eşleşen bir XML dosya adı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c5179-180">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="c5179-181">[AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) özelliği, bir XML dosyasının yolu oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c5179-181">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="c5179-182">Swagger kullanıcı arabirimini üç eğik çizgi açıklama eklemek için bir eylem için bölüm başlığı açıklama ekleyerek geliştirir.</span><span class="sxs-lookup"><span data-stu-id="c5179-182">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="c5179-183">Ekleme bir [ \<Özet >](/dotnet/csharp/programming-guide/xmldoc/summary) öğesi yukarıdaki `Delete` eylem:</span><span class="sxs-lookup"><span data-stu-id="c5179-183">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="c5179-184">Önceki kodun iç metni Swagger kullanıcı arabirimini görüntüler `<summary>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="c5179-184">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Swagger 'Belirli bir Todoıtem siler.' XML açıklama gösteren bir kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="c5179-187">Kullanıcı Arabirimi tarafından oluşturulan JSON şema yönetilir:</span><span class="sxs-lookup"><span data-stu-id="c5179-187">The UI is driven by the generated JSON schema:</span></span>

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

<span data-ttu-id="c5179-188">Ekleme bir [ \<remarks >](/dotnet/csharp/programming-guide/xmldoc/remarks) öğesine `Create` eylem yöntemi belgeleri.</span><span class="sxs-lookup"><span data-stu-id="c5179-188">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="c5179-189">Belirtilen bilgilerini tamamlayan `<summary>` öğesi ve daha güçlü bir Swagger kullanıcı Arabirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c5179-189">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="c5179-190">`<remarks>` Metin, JSON veya XML öğesi içeriği oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="c5179-190">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="c5179-191">Bu ek açıklamaları olan kullanıcı Arabirimi geliştirmeleri dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="c5179-191">Notice the UI enhancements with these additional comments:</span></span>

![Swagger kullanıcı Arabirimi ile gösterilen ek açıklamaları](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="c5179-193">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="c5179-193">Data annotations</span></span>

<span data-ttu-id="c5179-194">Öznitelikler bulundu, modeliyle süslemek [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) Swagger kullanıcı Arabirimi bileşenleri sürücü yardımcı olması için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="c5179-194">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="c5179-195">Ekleme `[Required]` özniteliğini `Name` özelliği `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="c5179-195">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="c5179-196">Bu öznitelik varlığını UI davranışını değiştirir ve temel alınan JSON şemasını değiştirir:</span><span class="sxs-lookup"><span data-stu-id="c5179-196">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="c5179-197">Ekleme `[Produces("application/json")]` özniteliği için API denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="c5179-197">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="c5179-198">Denetleyicinin eylemleri bir yanıt içerik türü desteği bildirmek için amacı olan *application/json*:</span><span class="sxs-lookup"><span data-stu-id="c5179-198">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="c5179-199">**Yanıt içerik türü** açılan denetleyicinin GET eylemler için varsayılan olarak bu içerik türünü seçer:</span><span class="sxs-lookup"><span data-stu-id="c5179-199">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Swagger kullanıcı Arabirimi ile varsayılan yanıt içerik türü](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="c5179-201">Web API'sindeki veri ek açıklamaları kullanımı arttıkça, UI ve API tarafından Yardım sayfaları daha açıklayıcı ve yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="c5179-201">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="c5179-202">Yanıt türleri açıklanmaktadır</span><span class="sxs-lookup"><span data-stu-id="c5179-202">Describe response types</span></span>

<span data-ttu-id="c5179-203">Kullanan geliştiriciler hangi iade ile en ilgili&mdash;özellikle yanıt türleri ve hata kodları (standart varsa).</span><span class="sxs-lookup"><span data-stu-id="c5179-203">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="c5179-204">Hata kodları ve yanıt türleri XML açıklamaları ve verileri ek açıklamalar içinde belirtilir.</span><span class="sxs-lookup"><span data-stu-id="c5179-204">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="c5179-205">`Create` Eylem başarılı olduğunda bir HTTP 201 durum kodunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="c5179-205">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="c5179-206">Gönderilen istek gövdesi null olduğunda, bir HTTP 400 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="c5179-206">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="c5179-207">Swagger kullanıcı arabirimini uygun belgelerinde tüketici bu beklenen sonuçları bilgisine sahip değil.</span><span class="sxs-lookup"><span data-stu-id="c5179-207">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="c5179-208">Aşağıdaki örnekte vurgulanan satırları ekleyerek bu sorunu düzeltin:</span><span class="sxs-lookup"><span data-stu-id="c5179-208">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="c5179-209">Swagger kullanıcı arabirimini açıkça beklenen HTTP yanıt kodları artık belgeler:</span><span class="sxs-lookup"><span data-stu-id="c5179-209">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger kullanıcı Arabirimi POST yanıt sınıf açıklaması 'yeni oluşturulan bir Todo öğesini döndürür' gösteren ve '400 - öğe null ise' durum kodu ve yanıt iletilerinin altında nedeni](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="c5179-211">Kullanıcı arabirimini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="c5179-211">Customize the UI</span></span>

<span data-ttu-id="c5179-212">Hem işlevsel hem de edileni hisse senedi kullanıcı Arabirimi.</span><span class="sxs-lookup"><span data-stu-id="c5179-212">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="c5179-213">Ancak, API belgeleri sayfaları markanız veya tema temsil etmelidir.</span><span class="sxs-lookup"><span data-stu-id="c5179-213">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="c5179-214">Swashbuckle bileşenleri marka statik dosyaları sunmak için kaynak ekleme ve bu dosyaları barındırmak için klasör yapısını oluşturma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c5179-214">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="c5179-215">.NET Framework veya .NET targeting, Core 1.x sürümüne, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet paketini projeye:</span><span class="sxs-lookup"><span data-stu-id="c5179-215">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="c5179-216">Önceki NuGet paketini, .NET Core'u hedefleyen zaten yüklü 2.x ve kullanarak [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="c5179-216">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="c5179-217">Statik dosya ara yazılımlarını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="c5179-217">Enable Static File Middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="c5179-218">İçeriğini alma *dist* klasöründen [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="c5179-218">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="c5179-219">Bu klasör, Swagger kullanıcı Arabirimi sayfası için gerekli varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="c5179-219">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="c5179-220">Oluşturma bir *wwwroot/swagger/UI* klasörü ve içeriği dosyaya kopyalayın *dist* klasör.</span><span class="sxs-lookup"><span data-stu-id="c5179-220">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="c5179-221">Oluşturma bir *custom.css* dosyasındaki *wwwroot/swagger/UI*, sayfa üstbilgisindeki özelleştirmek için aşağıdaki CSS ile:</span><span class="sxs-lookup"><span data-stu-id="c5179-221">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="c5179-222">Başvuru *custom.css* içinde *index.html* dosya, herhangi bir CSS dosyaları sonra:</span><span class="sxs-lookup"><span data-stu-id="c5179-222">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="c5179-223">Gözat *index.html* , sayfa `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="c5179-223">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="c5179-224">Girin `http://localhost:<port>/swagger/v1/swagger.json` başlığının metin seçeneğine tıklayıp **Araştır** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c5179-224">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="c5179-225">Sonuçta elde edilen sayfanın şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="c5179-225">The resulting page looks as follows:</span></span>

![Swagger kullanıcı Arabirimi ile özel üst bilgi başlığı](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="c5179-227">Sayfa ile yapabileceğiniz çok daha fazla yoktur.</span><span class="sxs-lookup"><span data-stu-id="c5179-227">There's much more you can do with the page.</span></span> <span data-ttu-id="c5179-228">UI kaynakları, tam özelliklerine bakın [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="c5179-228">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
