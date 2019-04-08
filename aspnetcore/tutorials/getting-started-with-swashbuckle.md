---
title: Swashbuckle'ı ve ASP.NET Core ile çalışmaya başlama
author: zuckerthoben
description: Swagger kullanıcı arabirimini tümleştirmek için ASP.NET Core web API projesi için Swashbuckle eklemeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/04/2019
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 31d45eaa684118ab78d1b3ecac594e95712f631f
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068355"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="ebcf8-103">Swashbuckle'ı ve ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="ebcf8-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="ebcf8-104">Tarafından [Shayne boyer'ın](https://twitter.com/spboyer) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="ebcf8-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="ebcf8-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ebcf8-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ebcf8-106">Swashbuckle'ı için üç ana bileşeni vardır:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="ebcf8-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger nesne modeli ve kullanıma sunmak için bir ara yazılım `SwaggerDocument` JSON uç noktalar olarak nesneleri.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="ebcf8-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): oluşturan bir Swagger Oluşturucusu `SwaggerDocument` nesneleri doğrudan, rotalara, denetleyicilere ve modeller.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="ebcf8-109">Bu genellikle otomatik olarak Swagger JSON kullanıma sunmak için Swagger uç nokta ara yazılımı ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="ebcf8-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): katıştırılmış bir Swagger kullanıcı arabirimini aracı sürümü.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="ebcf8-111">Bu, web API işlevleri tanımlamak için zengin, özelleştirilebilir bir deneyim oluşturmak için Swagger JSON yorumlar.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-111">It interprets Swagger JSON to build a rich, customizable experience for describing the web API functionality.</span></span> <span data-ttu-id="ebcf8-112">Genel metotlar için yerleşik test harnesses içerir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="ebcf8-113">Paket yüklemesi</span><span class="sxs-lookup"><span data-stu-id="ebcf8-113">Package installation</span></span>

<span data-ttu-id="ebcf8-114">Swashbuckle'ı ile aşağıdaki yaklaşımlardan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-114">Swashbuckle can be added with the following approaches:</span></span>

### [<a name="visual-studio"></a><span data-ttu-id="ebcf8-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebcf8-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ebcf8-116">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="ebcf8-117">Git **görünümü** > **diğer Windows** > **Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="ebcf8-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="ebcf8-118">Dizin gidin *TodoApi.csproj* dosya var</span><span class="sxs-lookup"><span data-stu-id="ebcf8-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="ebcf8-119">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="ebcf8-120">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="ebcf8-121">Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="ebcf8-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="ebcf8-122">Ayarlama **paket kaynağı** "nuget.org'da"</span><span class="sxs-lookup"><span data-stu-id="ebcf8-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="ebcf8-123">Arama kutusuna "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="ebcf8-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="ebcf8-124">"Swashbuckle.AspNetCore" paketinden seçin **Gözat** sekmesine **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="ebcf8-124">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### [<a name="visual-studio-for-mac"></a><span data-ttu-id="ebcf8-125">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebcf8-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ebcf8-126">Sağ *paketleri* klasöründe **çözüm bölmesi** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="ebcf8-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="ebcf8-127">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" açılır menüsünü</span><span class="sxs-lookup"><span data-stu-id="ebcf8-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="ebcf8-128">Arama kutusuna "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="ebcf8-128">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="ebcf8-129">Sonuçlar bölmesinde "Swashbuckle.AspNetCore" paketi seçin ve tıklayın **' paket Ekle**</span><span class="sxs-lookup"><span data-stu-id="ebcf8-129">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### [<a name="visual-studio-code"></a><span data-ttu-id="ebcf8-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ebcf8-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ebcf8-131">Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-131">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### [<a name="net-core-cli"></a><span data-ttu-id="ebcf8-132">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="ebcf8-132">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ebcf8-133">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-133">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="ebcf8-134">Ekleme ve Swagger ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ebcf8-134">Add and configure Swagger middleware</span></span>

<span data-ttu-id="ebcf8-135">Swagger oluşturucusunu Hizmetleri koleksiyona eklemek `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-135">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

<span data-ttu-id="ebcf8-136">Kullanmak için aşağıdaki ad alanı içe `Info` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-136">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="ebcf8-137">İçinde `Startup.Configure` yöntemi, oluşturulan JSON belgesini ve Swagger kullanıcı arabirimini sunulması için Ara yazılımlarını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-137">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="ebcf8-138">Önceki `UseSwaggerUI` yöntem çağrısının sağlayan [statik dosya ara yazılımlarını](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="ebcf8-138">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="ebcf8-139">.NET Framework veya .NET targeting, Core 1.x sürümüne, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet paketini projeye.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-139">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="ebcf8-140">Uygulamayı başlatın ve gidin `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-140">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="ebcf8-141">Uç noktaları açıklayan oluşturulan belgenin gösterildiği görünür [Swagger belirtimi (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="ebcf8-141">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="ebcf8-142">Swagger kullanıcı arabirimini şu yolda bulunabilir: `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-142">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="ebcf8-143">Swagger kullanıcı Arabirimi API'yi keşfedin ve diğer programlarda dahil edilip derecelendirilir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-143">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="ebcf8-144">Swagger kullanıcı arabirimini uygulamanın kök dizininde hizmet (`http://localhost:<port>/`) ayarlayın `RoutePrefix` boş bir dize özelliğini:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-144">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

<span data-ttu-id="ebcf8-145">IIS veya ters bir proxy ile #using Directories ayarlarsanız Swagger uç nokta kullanarak bir göreli yol `./` önek.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-145">If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="ebcf8-146">Örneğin: `./swagger/v1/swagger.json`</span><span class="sxs-lookup"><span data-stu-id="ebcf8-146">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="ebcf8-147">Kullanarak `/swagger/v1/swagger.json` URL (artı kullandıysanız rota öneki) true kökünde JSON dosyasını bulmak için uygulama bildirir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-147">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="ebcf8-148">Örneğin, `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` yerine `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-148">For example, use `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

## <a name="customize-and-extend"></a><span data-ttu-id="ebcf8-149">Özelleştirme ve genişletme</span><span class="sxs-lookup"><span data-stu-id="ebcf8-149">Customize and extend</span></span>

<span data-ttu-id="ebcf8-150">Swagger temanızı eşleştirmek için nesne modeli belgelemekten ve kullanıcı arabirimini özelleştirme seçenekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-150">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="ebcf8-151">API bilgisi ve açıklama</span><span class="sxs-lookup"><span data-stu-id="ebcf8-151">API info and description</span></span>

<span data-ttu-id="ebcf8-152">Yapılandırma eylemi geçirilen `AddSwaggerGen` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-152">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="ebcf8-153">Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-153">The Swagger UI displays the version's information:</span></span>

![Swagger kullanıcı Arabirimi ile sürüm bilgisi: açıklama, yazar ve daha fazla bağlantıya göz atın](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="ebcf8-155">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ebcf8-155">XML comments</span></span>

<span data-ttu-id="ebcf8-156">XML açıklamaları aşağıdaki yaklaşımlardan ile etkin hale getirilebilir:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-156">XML comments can be enabled with the following approaches:</span></span>

#### [<a name="visual-studio"></a><span data-ttu-id="ebcf8-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebcf8-157">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="ebcf8-158">Projeye sağ **Çözüm Gezgini** seçip **< project_name > .csproj Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-158">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="ebcf8-159">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-159">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="ebcf8-160">Projeye sağ **Çözüm Gezgini** seçip **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-160">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="ebcf8-161">Denetleme **XML belge dosyası** altında kutusunda **çıkış** bölümünü **derleme** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

#### [<a name="visual-studio-for-mac"></a><span data-ttu-id="ebcf8-162">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebcf8-162">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="ebcf8-163">Gelen *çözüm bölmesi*, basın **denetim** ve proje adına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-163">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="ebcf8-164">Gidin **Araçları** > **dosyasını düzenleyin**.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-164">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="ebcf8-165">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-165">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="ebcf8-166">Açık **proje seçenekleri** iletişim > **derleme** > **derleyici**</span><span class="sxs-lookup"><span data-stu-id="ebcf8-166">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="ebcf8-167">Denetleme **xml belgeleri oluştur** altında kutusunda **genel seçenekleri** bölümü</span><span class="sxs-lookup"><span data-stu-id="ebcf8-167">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

#### [<a name="visual-studio-code"></a><span data-ttu-id="ebcf8-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ebcf8-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ebcf8-169">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-169">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### [<a name="net-core-cli"></a><span data-ttu-id="ebcf8-170">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="ebcf8-170">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ebcf8-171">El ile vurgulanan satırları ekleyin *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-171">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="ebcf8-172">XML açıklamaları'nı etkinleştirme, belgelenmemiş genel türler ve üyeler için hata ayıklama bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-172">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="ebcf8-173">Belgelenmemiş türleri ve üyeleri tarafından uyarı iletisinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-173">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="ebcf8-174">Örneğin, aşağıdaki ileti uyarı kodu 1591 ihlalini gösterir:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-174">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="ebcf8-175">Proje genelinde uyarıları bastırmak için proje dosyasında yok saymak için uyarı kodlarının noktalı virgülle ayrılmış bir listesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-175">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="ebcf8-176">Ekleme için uyarı kodlarının `$(NoWarn);` geçerlidir [ C# varsayılan değerler](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) çok.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-176">Appending the warning codes to `$(NoWarn);` applies the [C# default values](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="ebcf8-177">Yalnızca belirli üyeleri için uyarıları bastırmak için kod içine [#pragma Uyarısı](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) önişlemci yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-177">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="ebcf8-178">Bu yaklaşım, API belgeleri kullanıma sunulan olmamalıdır kod için kullanışlıdır. Aşağıdaki örnekte, tüm uyarı kodu CS1591 göz ardı edilir `Program` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-178">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="ebcf8-179">Zorlama uyarı kodun sınıf tanımının kapanışında geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-179">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="ebcf8-180">Birden çok uyarı kodlarının virgülle ayrılmış bir liste belirtin.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-180">Specify multiple warning codes with a comma-delimited list.</span></span>

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

<span data-ttu-id="ebcf8-181">Oluşturulan XML dosyası yukarıdaki yönergeleri ile kullanmak için Swagger'ı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-181">Configure Swagger to use the XML file that's generated with the preceding instructions.</span></span> <span data-ttu-id="ebcf8-182">Linux veya Windows olmayan işletim sistemleri için dosya adlarını ve yollarını büyük küçük harfe duyarlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-182">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="ebcf8-183">Örneğin, bir *TodoApi.XML* dosya, Windows ancak değil CentOS geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-183">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="ebcf8-184">Önceki kodda, [yansıma](/dotnet/csharp/programming-guide/concepts/reflection) web API projesi, eşleşen bir XML dosya adı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-184">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the web API project.</span></span> <span data-ttu-id="ebcf8-185">[AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) özelliği, bir XML dosyasının yolu oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-185">The [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) property is used to construct a path to the XML file.</span></span> <span data-ttu-id="ebcf8-186">XML belge dosyası kullanmadan bazı Swagger özellikler (örneğin, şemaların giriş parametreleri veya HTTP yöntemleri ve ilgili özniteliklerden yanıt kodları) çalışır.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-186">Some Swagger features (for example, schemata of input parameters or HTTP methods and response codes from the respective attributes) work without the use of an XML documentation file.</span></span> <span data-ttu-id="ebcf8-187">Çoğu özelliği için yöntemi özetler ve açıklamalar parametrelerinin ve yanıt kodları, bir XML dosyasının zorunludur.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-187">For most features, namely method summaries and the descriptions of parameters and response codes, the use of an XML file is mandatory.</span></span>

<span data-ttu-id="ebcf8-188">Swagger kullanıcı arabirimini üç eğik çizgi açıklama eklemek için bir eylem için bölüm başlığı açıklama ekleyerek geliştirir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-188">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="ebcf8-189">Ekleme bir [ \<Özet >](/dotnet/csharp/programming-guide/xmldoc/summary) öğesi yukarıdaki `Delete` eylem:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-189">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="ebcf8-190">Önceki kodun iç metni Swagger kullanıcı arabirimini görüntüler `<summary>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-190">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Swagger 'Belirli bir Todoıtem siler.' XML açıklama gösteren bir kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="ebcf8-193">Kullanıcı Arabirimi tarafından oluşturulan JSON şema yönetilir:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-193">The UI is driven by the generated JSON schema:</span></span>

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

<span data-ttu-id="ebcf8-194">Ekleme bir [ \<remarks >](/dotnet/csharp/programming-guide/xmldoc/remarks) öğesine `Create` eylem yöntemi belgeleri.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-194">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="ebcf8-195">Belirtilen bilgilerini tamamlayan `<summary>` öğesi ve daha güçlü bir Swagger kullanıcı Arabirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-195">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="ebcf8-196">`<remarks>` Metin, JSON veya XML öğesi içeriği oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-196">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="ebcf8-197">Bu ek açıklamaları olan kullanıcı Arabirimi geliştirmeleri dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-197">Notice the UI enhancements with these additional comments:</span></span>

![Swagger kullanıcı Arabirimi ile gösterilen ek açıklamaları](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="ebcf8-199">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ebcf8-199">Data annotations</span></span>

<span data-ttu-id="ebcf8-200">Öznitelikler bulundu, modeliyle süslemek [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) Swagger kullanıcı Arabirimi bileşenleri sürücü yardımcı olması için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-200">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="ebcf8-201">Ekleme `[Required]` özniteliğini `Name` özelliği `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-201">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="ebcf8-202">Bu öznitelik varlığını UI davranışını değiştirir ve temel alınan JSON şemasını değiştirir:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-202">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="ebcf8-203">Ekleme `[Produces("application/json")]` özniteliği için API denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-203">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="ebcf8-204">Denetleyicinin eylemleri bir yanıt içerik türü desteği bildirmek için amacı olan *application/json*:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-204">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="ebcf8-205">**Yanıt içerik türü** açılan denetleyicinin GET eylemler için varsayılan olarak bu içerik türünü seçer:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-205">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Swagger kullanıcı Arabirimi ile varsayılan yanıt içerik türü](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="ebcf8-207">Web API'SİNDEKİ veri ek açıklamaları kullanımı arttıkça, UI ve API tarafından Yardım sayfaları daha açıklayıcı ve yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-207">As the usage of data annotations in the web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="ebcf8-208">Yanıt türleri açıklanmaktadır</span><span class="sxs-lookup"><span data-stu-id="ebcf8-208">Describe response types</span></span>

<span data-ttu-id="ebcf8-209">Bir web API'sini kullanan geliştiriciler hangi iade ile en ilgili&mdash;özellikle yanıt türleri ve hata kodları (standart varsa).</span><span class="sxs-lookup"><span data-stu-id="ebcf8-209">Developers consuming a web API are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="ebcf8-210">Hata kodları ve yanıt türleri XML açıklamaları ve verileri ek açıklamalar içinde belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-210">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="ebcf8-211">`Create` Eylem başarılı olduğunda bir HTTP 201 durum kodunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-211">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="ebcf8-212">Gönderilen istek gövdesi null olduğunda, bir HTTP 400 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-212">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="ebcf8-213">Swagger kullanıcı arabirimini uygun belgelerinde tüketici bu beklenen sonuçları bilgisine sahip değil.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-213">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="ebcf8-214">Aşağıdaki örnekte vurgulanan satırları ekleyerek bu sorunu düzeltin:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-214">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="ebcf8-215">Swagger kullanıcı arabirimini açıkça beklenen HTTP yanıt kodları artık belgeler:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-215">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger kullanıcı Arabirimi POST yanıt sınıf açıklaması 'yeni oluşturulan bir Todo öğesini döndürür' gösteren ve '400 - öğe null ise' durum kodu ve yanıt iletilerinin altında nedeni](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ebcf8-217">Açıkça bireysel eylemleri dekore etmeye alternatif olarak ASP.NET Core 2.2 veya sonraki sürümlerde, kuralları kullanılabilir `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-217">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="ebcf8-218">Daha fazla bilgi için bkz. <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-218">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

### <a name="customize-the-ui"></a><span data-ttu-id="ebcf8-219">Kullanıcı arabirimini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="ebcf8-219">Customize the UI</span></span>

<span data-ttu-id="ebcf8-220">Hem işlevsel hem de edileni hisse senedi kullanıcı Arabirimi.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-220">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="ebcf8-221">Ancak, API belgeleri sayfaları markanız veya tema temsil etmelidir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-221">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="ebcf8-222">Swashbuckle bileşenleri marka statik dosyaları sunmak için kaynak ekleme ve bu dosyaları barındırmak için klasör yapısını oluşturma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-222">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="ebcf8-223">.NET Framework veya .NET targeting, Core 1.x sürümüne, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet paketini projeye:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-223">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="ebcf8-224">Önceki NuGet paketini, .NET Core'u hedefleyen zaten yüklü 2.x ve kullanarak [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="ebcf8-224">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="ebcf8-225">Statik dosya ara yazılımlarını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-225">Enable Static File Middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="ebcf8-226">İçeriğini alma *dist* klasöründen [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="ebcf8-226">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="ebcf8-227">Bu klasör, Swagger kullanıcı Arabirimi sayfası için gerekli varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-227">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="ebcf8-228">Oluşturma bir *wwwroot/swagger/UI* klasörü ve içeriği dosyaya kopyalayın *dist* klasör.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-228">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="ebcf8-229">Oluşturma bir *custom.css* dosyasındaki *wwwroot/swagger/UI*, sayfa üstbilgisindeki özelleştirmek için aşağıdaki CSS ile:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-229">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="ebcf8-230">Başvuru *custom.css* içinde *index.html* dosya, herhangi bir CSS dosyaları sonra:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-230">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="ebcf8-231">Gözat *index.html* , sayfa `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-231">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="ebcf8-232">Girin `http://localhost:<port>/swagger/v1/swagger.json` başlığının metin seçeneğine tıklayıp **Araştır** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-232">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="ebcf8-233">Sonuçta elde edilen sayfanın şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="ebcf8-233">The resulting page looks as follows:</span></span>

![Swagger kullanıcı Arabirimi ile özel üst bilgi başlığı](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="ebcf8-235">Sayfa ile yapabileceğiniz çok daha fazla yoktur.</span><span class="sxs-lookup"><span data-stu-id="ebcf8-235">There's much more you can do with the page.</span></span> <span data-ttu-id="ebcf8-236">UI kaynakları, tam özelliklerine bakın [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="ebcf8-236">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
