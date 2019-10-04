---
title: Swashbuckle ve ASP.NET Core kullanmaya başlayın
author: zuckerthoben
description: Swagger Kullanıcı arabirimini bütünleştirmek için ASP.NET Core Web API Projenize swashbuckle ekleme hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/21/2019
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: d3cef72de22e54f7e65ddf9f1446eb32256d0c71
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71924978"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="f62d6-103">Swashbuckle ve ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="f62d6-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="f62d6-104">, [Shayne Boyer](https://twitter.com/spboyer) ve [Scott Ade](https://twitter.com/Scott_Addie) tarafından</span><span class="sxs-lookup"><span data-stu-id="f62d6-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f62d6-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f62d6-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f62d6-106">Swashbuckle için üç ana bileşen vardır:</span><span class="sxs-lookup"><span data-stu-id="f62d6-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="f62d6-107">[Swashbuckle. aspnetcore. Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): nesneleri JSON uç noktaları olarak göstermek `SwaggerDocument` için Swagger nesne modeli ve ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="f62d6-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="f62d6-108">[Swashbuckle. aspnetcore. SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): `SwaggerDocument` nesneleri doğrudan rotalarınız, Denetleyicilerinizden ve modellerden oluşturan bir Swagger Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="f62d6-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="f62d6-109">Swagger JSON 'yi otomatik olarak göstermek için genellikle Swagger uç nokta ara yazılımı ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="f62d6-110">[Swashbuckle. AspNetCore. SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): Swagger Kullanıcı arabirimi aracının gömülü bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="f62d6-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="f62d6-111">Web API işlevlerini açıklamak için bir zengin ve özelleştirilebilir deneyim oluşturmak üzere Swagger JSON 'u yorumlar.</span><span class="sxs-lookup"><span data-stu-id="f62d6-111">It interprets Swagger JSON to build a rich, customizable experience for describing the web API functionality.</span></span> <span data-ttu-id="f62d6-112">Ortak yöntemler için yerleşik test gücünden içerir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="f62d6-113">Paket yüklemesi</span><span class="sxs-lookup"><span data-stu-id="f62d6-113">Package installation</span></span>

<span data-ttu-id="f62d6-114">Aşağıdaki yaklaşımlar ile swashbuckle eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="f62d6-114">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f62d6-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f62d6-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f62d6-116">**Paket Yöneticisi konsol** penceresinde:</span><span class="sxs-lookup"><span data-stu-id="f62d6-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="f62d6-117"> > **Diğer**Windowspaket > **Yöneticisi konsolunu** görüntüle ' ye git</span><span class="sxs-lookup"><span data-stu-id="f62d6-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="f62d6-118">*TodoApi. csproj* dosyasının bulunduğu dizine gidin</span><span class="sxs-lookup"><span data-stu-id="f62d6-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="f62d6-119">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="f62d6-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore -Version 5.0.0-rc4
    ```

* <span data-ttu-id="f62d6-120">**NuGet Paketlerini Yönet** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="f62d6-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="f62d6-121">**Çözüm Gezgini** > **NuGet Paketlerini Yönet** ' de projeye sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="f62d6-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="f62d6-122">**Paket kaynağını** "NuGet.org" olarak ayarlayın</span><span class="sxs-lookup"><span data-stu-id="f62d6-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="f62d6-123">"Ön sürümü dahil et" seçeneğinin etkinleştirildiğinden emin olun</span><span class="sxs-lookup"><span data-stu-id="f62d6-123">Ensure the "Include prerelease" option is enabled</span></span>
  * <span data-ttu-id="f62d6-124">Arama kutusuna "swashbuckle. AspNetCore" yazın</span><span class="sxs-lookup"><span data-stu-id="f62d6-124">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="f62d6-125">**Araştır** sekmesinden en son "swashbuckle. aspnetcore" paketini seçin ve sonra da **yüklensin** ' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-125">Select the latest "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f62d6-126">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f62d6-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f62d6-127">**Çözüm bölmesi** paket > **Ekle...** ' da paketler klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="f62d6-128">**Paket Ekle** penceresinin **kaynak** açılan penceresini "NuGet.org" olarak ayarlayın</span><span class="sxs-lookup"><span data-stu-id="f62d6-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="f62d6-129">"Yayın öncesi paketleri göster" seçeneğinin etkin olduğundan emin olun</span><span class="sxs-lookup"><span data-stu-id="f62d6-129">Ensure the "Show pre-release packages" option is enabled</span></span>
* <span data-ttu-id="f62d6-130">Arama kutusuna "swashbuckle. AspNetCore" yazın</span><span class="sxs-lookup"><span data-stu-id="f62d6-130">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="f62d6-131">Sonuçlar bölmesinden en son "swashbuckle. AspNetCore" paketini seçin ve **paket Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-131">Select the latest "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f62d6-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f62d6-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f62d6-133">**Tümleşik terminalden**aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f62d6-133">Run the following command from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore -v 5.0.0-rc4
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f62d6-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f62d6-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f62d6-135">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f62d6-135">Run the following command:</span></span>

```dotnetcli
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore -v 5.0.0-rc4
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="f62d6-136">Swagger ara yazılım ekleme ve yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f62d6-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="f62d6-137">Sınıfında, `OpenApiInfo` sınıfını kullanmak için aşağıdaki ad alanını içeri aktarın: `Startup`</span><span class="sxs-lookup"><span data-stu-id="f62d6-137">In the `Startup` class, import the following namespace to use the `OpenApiInfo` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="f62d6-138">`Startup.ConfigureServices` Yöntemdeki Services koleksiyonuna Swagger oluşturucuyu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f62d6-138">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

<span data-ttu-id="f62d6-139">`Startup.Configure` Yönteminde, oluşturulan JSON belgesine ve Swagger Kullanıcı arabirimine hizmet veren ara yazılımı etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="f62d6-139">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

::: moniker-end

<span data-ttu-id="f62d6-140">Önceki `UseSwaggerUI` Yöntem çağrısı [statik dosya ara yazılımını](xref:fundamentals/static-files)sunar.</span><span class="sxs-lookup"><span data-stu-id="f62d6-140">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="f62d6-141">.NET Framework veya .NET Core 1. x 'i hedefliyorsanız, projeye [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f62d6-141">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="f62d6-142">Uygulamayı başlatın ve adresine `http://localhost:<port>/swagger/v1/swagger.json`gidin.</span><span class="sxs-lookup"><span data-stu-id="f62d6-142">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="f62d6-143">Uç noktaları tanımlayan oluşturulan belge, [Swagger belirtiminde (Swagger. JSON)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson)gösterildiği gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="f62d6-143">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="f62d6-144">Swagger Kullanıcı arabirimi adresinde `http://localhost:<port>/swagger`bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-144">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="f62d6-145">Swagger Kullanıcı arabirimi aracılığıyla API 'YI keşfet ve diğer programlarda birleştirme.</span><span class="sxs-lookup"><span data-stu-id="f62d6-145">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="f62d6-146">Swagger Kullanıcı arabirimine uygulamanın kökünde (`http://localhost:<port>/`) hizmeti sağlamak için, `RoutePrefix` özelliğini boş bir dizeye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="f62d6-146">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

<span data-ttu-id="f62d6-147">IIS veya ters proxy ile dizin kullanıyorsanız, Swagger uç noktasını, `./` öneki kullanılarak göreli bir yol olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-147">If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="f62d6-148">Örneğin, `./swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="f62d6-148">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="f62d6-149">Kullanarak `/swagger/v1/swagger.json` , uygulamanın URL 'nin gerçek kökünde json dosyasını aramasını söyler (Ayrıca kullanılıyorsa rota öneki).</span><span class="sxs-lookup"><span data-stu-id="f62d6-149">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="f62d6-150">Örneğin, `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`yerine kullanın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-150">For example, use `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

## <a name="customize-and-extend"></a><span data-ttu-id="f62d6-151">Özelleştirme ve genişletme</span><span class="sxs-lookup"><span data-stu-id="f62d6-151">Customize and extend</span></span>

<span data-ttu-id="f62d6-152">Swagger, nesne modelini belgeleme ve Kullanıcı arabirimini temanızla eşleşecek şekilde özelleştirme seçenekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="f62d6-152">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

<span data-ttu-id="f62d6-153">`Startup` Sınıfında, aşağıdaki ad alanlarını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f62d6-153">In the `Startup` class, add the following namespaces:</span></span>

```csharp
using System;
using System.Reflection;
using System.IO;
```

### <a name="api-info-and-description"></a><span data-ttu-id="f62d6-154">API bilgisi ve açıklaması</span><span class="sxs-lookup"><span data-stu-id="f62d6-154">API info and description</span></span>

<span data-ttu-id="f62d6-155">`AddSwaggerGen` Yöntemine geçirilen yapılandırma eylemi yazar, lisans ve açıklama gibi bilgileri ekler:</span><span class="sxs-lookup"><span data-stu-id="f62d6-155">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="f62d6-156">Swagger Kullanıcı arabirimi, sürümün bilgilerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="f62d6-156">The Swagger UI displays the version's information:</span></span>

![Sürüm bilgileriyle Swagger Kullanıcı arabirimi: Açıklama, yazar ve daha fazla bağlantı görüntüle](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="f62d6-158">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="f62d6-158">XML comments</span></span>

<span data-ttu-id="f62d6-159">XML açıklamaları aşağıdaki yaklaşımlar ile etkinleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="f62d6-159">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f62d6-160">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f62d6-160">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="f62d6-161">**Çözüm Gezgini** projeye sağ tıklayın ve **düzenle < Project_Name >. csproj**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="f62d6-161">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="f62d6-162">Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f62d6-162">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="f62d6-163">Projeye sağ **Çözüm Gezgini** seçip **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="f62d6-163">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="f62d6-164">**Build** sekmesinin **output** bölümünün altındaki **XML belge dosyası** kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="f62d6-164">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f62d6-165">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f62d6-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="f62d6-166">*Çözüm bölmesi*, **Denetim** ' e basın ve proje adına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-166">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="f62d6-167">**Araçlar** > **dosya düzenleme**sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="f62d6-167">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="f62d6-168">Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f62d6-168">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="f62d6-169">**Derleme** derleyicisi>proje> seçenekleri iletişim kutusunu açın</span><span class="sxs-lookup"><span data-stu-id="f62d6-169">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="f62d6-170">**Genel Seçenekler** bölümünün altındaki **XML oluştur belge** kutusunu işaretleyin</span><span class="sxs-lookup"><span data-stu-id="f62d6-170">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f62d6-171">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f62d6-171">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f62d6-172">Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f62d6-172">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f62d6-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f62d6-173">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f62d6-174">Vurgulanan satırları *. csproj* dosyasına el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f62d6-174">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="f62d6-175">XML açıklamalarını etkinleştirmek, belgelenmemiş ortak türler ve Üyeler için hata ayıklama bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="f62d6-175">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="f62d6-176">Belgelenmemiş türler ve Üyeler uyarı iletisiyle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-176">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="f62d6-177">Örneğin, aşağıdaki ileti 1591 uyarı kodunu ihlal eder:</span><span class="sxs-lookup"><span data-stu-id="f62d6-177">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="f62d6-178">Uyarıları proje genelinde gizlemek için, proje dosyasında yoksayılacak uyarı kodlarının noktalı virgülle ayrılmış bir listesini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-178">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="f62d6-179">Uyarı kodlarının `$(NoWarn);` eklenmesi [ C# varsayılan değerleri](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) de uygular.</span><span class="sxs-lookup"><span data-stu-id="f62d6-179">Appending the warning codes to `$(NoWarn);` applies the [C# default values](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="f62d6-180">Yalnızca belirli Üyeler için uyarıları gizlemek için, kodu [#pragma uyarı](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) Önişlemci yönergeleri arasına alın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-180">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="f62d6-181">Bu yaklaşım, API belgeleri aracılığıyla sunulmaması gereken kod için yararlıdır. Aşağıdaki örnekte, tüm `Program` sınıf için uyarı kodu CS1591 yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="f62d6-181">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="f62d6-182">Uyarı kodu zorlaması sınıf tanımının kapandığına geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-182">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="f62d6-183">Virgülle ayrılmış bir liste ile birden çok uyarı kodu belirtin.</span><span class="sxs-lookup"><span data-stu-id="f62d6-183">Specify multiple warning codes with a comma-delimited list.</span></span>

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

<span data-ttu-id="f62d6-184">Swagger 'yi yukarıdaki yönergelerle oluşturulan XML dosyasını kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-184">Configure Swagger to use the XML file that's generated with the preceding instructions.</span></span> <span data-ttu-id="f62d6-185">Linux veya Windows dışı işletim sistemleri için dosya adları ve yolları büyük/küçük harfe duyarlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-185">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="f62d6-186">Örneğin, *TodoApi. xml* dosyası Windows üzerinde geçerlidir ancak CentOS değildir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-186">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="f62d6-187">Yukarıdaki kodda, [yansıma](/dotnet/csharp/programming-guide/concepts/reflection) , Web API projesi ile eşleşen bir XML dosya adı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f62d6-187">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the web API project.</span></span> <span data-ttu-id="f62d6-188">[AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory*) ÖZELLIĞI, XML dosyasının yolunu oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f62d6-188">The [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) property is used to construct a path to the XML file.</span></span> <span data-ttu-id="f62d6-189">Bazı Swagger özellikleri (örneğin, bir XML belge dosyası kullanılmadan, giriş parametrelerinin veya HTTP yöntemlerinin ve yanıt kodlarının) bir bölümü çalışır.</span><span class="sxs-lookup"><span data-stu-id="f62d6-189">Some Swagger features (for example, schemata of input parameters or HTTP methods and response codes from the respective attributes) work without the use of an XML documentation file.</span></span> <span data-ttu-id="f62d6-190">Çoğu özellik için, yöntem özetleri ve parametrelerin ve yanıt kodlarının açıklamaları, bir XML dosyası kullanımı zorunludur.</span><span class="sxs-lookup"><span data-stu-id="f62d6-190">For most features, namely method summaries and the descriptions of parameters and response codes, the use of an XML file is mandatory.</span></span>

<span data-ttu-id="f62d6-191">Bir eyleme Üçlü eğik çizgi açıklamaları eklemek, Bölüm üstbilgisine açıklama ekleyerek Swagger Kullanıcı arabirimini geliştirir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-191">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="f62d6-192">Eylemin`Delete` üstüne bir [ \<Summary >](/dotnet/csharp/programming-guide/xmldoc/summary) öğesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f62d6-192">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="f62d6-193">Swagger Kullanıcı arabirimi, önceki kodun `<summary>` öğesinin iç metnini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="f62d6-193">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![XML açıklamasını gösteren Swagger Kullanıcı arabirimi, belirli bir TodoItem siler.](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="f62d6-196">Kullanıcı arabirimi, oluşturulan JSON şeması tarafından çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="f62d6-196">The UI is driven by the generated JSON schema:</span></span>

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

<span data-ttu-id="f62d6-197">Eylem yöntemi belgelerine bir [ \<açıklama >](/dotnet/csharp/programming-guide/xmldoc/remarks) öğesi ekleyin. `Create`</span><span class="sxs-lookup"><span data-stu-id="f62d6-197">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="f62d6-198">`<summary>` Öğesinde belirtilen bilgileri tamamlar ve daha sağlam bir Swagger Kullanıcı arabirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="f62d6-198">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="f62d6-199">`<remarks>` Öğe içeriği metin, JSON veya XML içerebilir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-199">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="f62d6-200">Bu ek açıklamalarla UI geliştirmelerini göz unutmayın:</span><span class="sxs-lookup"><span data-stu-id="f62d6-200">Notice the UI enhancements with these additional comments:</span></span>

![Ek açıklamaların gösterildiği Swagger Kullanıcı arabirimi](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="f62d6-202">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="f62d6-202">Data annotations</span></span>

<span data-ttu-id="f62d6-203">[System. ComponentModel. datanot](/dotnet/api/system.componentmodel.dataannotations) ad alanında bulunan ve bu modeli, Swagger Kullanıcı Arabirimi bileşenlerini kullanmanıza yardımcı olacak şekilde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f62d6-203">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="f62d6-204">`[Required]` Özniteliğini sınıfının`TodoItem` özelliğine ekleyin: `Name`</span><span class="sxs-lookup"><span data-stu-id="f62d6-204">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="f62d6-205">Bu özniteliğin varlığı, Kullanıcı arabirimi davranışını değiştirir ve temel alınan JSON şemasını değiştirir:</span><span class="sxs-lookup"><span data-stu-id="f62d6-205">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="f62d6-206">`[Produces("application/json")]` Özniteliği API denetleyicisine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f62d6-206">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="f62d6-207">Amaç, denetleyicinin eylemlerinin bir *uygulama/JSON*yanıt içerik türünü desteklediğini bildirsağlamaktır:</span><span class="sxs-lookup"><span data-stu-id="f62d6-207">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="f62d6-208">**Yanıt Içerik türü** açılan liste, denetleyicinin al eylemleri için varsayılan olarak bu içerik türünü seçer:</span><span class="sxs-lookup"><span data-stu-id="f62d6-208">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Varsayılan yanıt içerik türüyle Swagger Kullanıcı arabirimi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="f62d6-210">Web API 'sindeki veri ek açıklamaların kullanımı arttıkça, UI ve API Yardım sayfaları daha açıklayıcı ve yararlı hale gelir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-210">As the usage of data annotations in the web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="f62d6-211">Yanıt türlerini açıkla</span><span class="sxs-lookup"><span data-stu-id="f62d6-211">Describe response types</span></span>

<span data-ttu-id="f62d6-212">Bir Web API 'si kullanan geliştiriciler, özellikle yanıt türleri ve hata&mdash;kodları (Standart değilse) döndürülmesiyle ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-212">Developers consuming a web API are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="f62d6-213">Yanıt türleri ve hata kodları XML açıklamaları ve veri ek açıklamalarında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-213">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="f62d6-214">`Create` Eylem başarılı olduğunda bir http 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="f62d6-214">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="f62d6-215">Postalanan istek gövdesi null olduğunda bir HTTP 400 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f62d6-215">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="f62d6-216">Swagger Kullanıcı arabiriminde doğru belgeler olmadan, tüketici beklenen bu sonuçlar hakkında bilgi sahibi yoktur.</span><span class="sxs-lookup"><span data-stu-id="f62d6-216">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="f62d6-217">Aşağıdaki örneğe vurgulanan satırları ekleyerek bu sorunu giderebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="f62d6-217">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="f62d6-218">Swagger Kullanıcı arabirimi artık beklenen HTTP yanıt kodlarını açıkça belgelemektedir:</span><span class="sxs-lookup"><span data-stu-id="f62d6-218">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Yanıt Iletileri ' ndeki durum kodu ve nedeni için, POST Response sınıfının Description ', yeni oluşturulan Todo öğesini döndürür ve ' 400-öğesi null ise '](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f62d6-220">ASP.NET Core 2,2 veya üzeri sürümlerde kurallar, ile `[ProducesResponseType]`tek tek eylemleri açıkça dekorasyon alternatifi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-220">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="f62d6-221">Daha fazla bilgi için bkz. <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="f62d6-221">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

### <a name="customize-the-ui"></a><span data-ttu-id="f62d6-222">Kullanıcı arabirimini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="f62d6-222">Customize the UI</span></span>

<span data-ttu-id="f62d6-223">Hisse senedi Kullanıcı arabirimi hem işlevsel hem de edileni.</span><span class="sxs-lookup"><span data-stu-id="f62d6-223">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="f62d6-224">Ancak, API belge sayfaları markanızı veya temanızı temsil etmelidir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-224">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="f62d6-225">Marka, swashbuckle bileşenleri, statik dosyalara ve bu dosyaları barındırmak için klasör yapısını oluşturmaya yönelik kaynakların eklenmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-225">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="f62d6-226">.NET Framework veya .NET Core 1. x 'i hedefliyorsanız, [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet paketini projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f62d6-226">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="f62d6-227">Önceki NuGet paketi, .NET Core 2. x hedefleniyorsa ve [metapackage](xref:fundamentals/metapackage)kullanılarak zaten yüklüdür.</span><span class="sxs-lookup"><span data-stu-id="f62d6-227">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="f62d6-228">Statik dosya ara yazılımını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="f62d6-228">Enable Static File Middleware:</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

::: moniker-end

<span data-ttu-id="f62d6-229">[Swagger Kullanıcı arabirimi GitHub deposundan](https://github.com/swagger-api/swagger-ui/tree/master/dist) *Dist* klasörünün içeriğini alın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-229">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="f62d6-230">Bu klasör, Swagger Kullanıcı arabirimi sayfası için gerekli varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="f62d6-230">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="f62d6-231">Bir *Wwwroot/Swagger/UI* klasörü oluşturun ve bunu *Dist* klasörünün içeriğine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-231">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="f62d6-232">Sayfa üstbilgisini özelleştirmek için aşağıdaki CSS ile *Wwwroot/Swagger/UI*içinde *özel bir. css* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f62d6-232">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="f62d6-233">Diğer CSS dosyalarından sonra, Kullanıcı arabirimi klasörünün içindeki *index. html* dosyasında *Custom. css* dosyasına başvurun:</span><span class="sxs-lookup"><span data-stu-id="f62d6-233">Reference *custom.css* in the *index.html* file inside ui folder, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="f62d6-234">Konumundaki`http://localhost:<port>/swagger/ui/index.html` *index. html* sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="f62d6-234">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="f62d6-235">Üstbilginin `https://localhost:<port>/swagger/v1/swagger.json` metin kutusuna girip **keşfet** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-235">Enter `https://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="f62d6-236">Elde edilen sayfa şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="f62d6-236">The resulting page looks as follows:</span></span>

![Özel üstbilgi başlıklı Swagger Kullanıcı arabirimi](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="f62d6-238">Sayfada yapabileceğiniz çok daha fazla şey vardır.</span><span class="sxs-lookup"><span data-stu-id="f62d6-238">There's much more you can do with the page.</span></span> <span data-ttu-id="f62d6-239">[Swagger Kullanıcı arabirimi GitHub DEPOSUNDAKI](https://github.com/swagger-api/swagger-ui)UI kaynakları için tam yeteneklere bakın.</span><span class="sxs-lookup"><span data-stu-id="f62d6-239">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
