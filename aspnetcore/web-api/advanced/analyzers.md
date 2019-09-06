---
title: Web API Çözümleyicileri kullanma
author: pranavkm
description: ASP.NET Core MVC web API Çözümleyicileri paketi hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/05/2019
uid: web-api/advanced/analyzers
ms.openlocfilehash: 1568eb0304a58758caa5f82249dc42872f5c36b9
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384861"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="1e544-103">Web API Çözümleyicileri kullanma</span><span class="sxs-lookup"><span data-stu-id="1e544-103">Use web API analyzers</span></span>

<span data-ttu-id="1e544-104">ASP.NET Core 2,2 ve üzeri, Web API projeleriyle kullanılması amaçlanan bir MVC Çözümleyicileri paketi sağlar.</span><span class="sxs-lookup"><span data-stu-id="1e544-104">ASP.NET Core 2.2 and later provides an MVC analyzers package intended for use with web API projects.</span></span> <span data-ttu-id="1e544-105">Çözümleyiciler, [Web API kuralları](xref:web-api/advanced/conventions)üzerinde oluşturma sırasında <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>ile açıklanmış olan denetleyicilerle birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="1e544-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [web API conventions](xref:web-api/advanced/conventions).</span></span>

<span data-ttu-id="1e544-106">Çözümleyiciler paketi şu şekilde bir denetleyici eylemi bildirir:</span><span class="sxs-lookup"><span data-stu-id="1e544-106">The analyzers package notifies you of any controller action that:</span></span>

* <span data-ttu-id="1e544-107">Bildirilmemiş bir durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="1e544-107">Returns an undeclared status code.</span></span>
* <span data-ttu-id="1e544-108">Bildirilmemiş başarı sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="1e544-108">Returns an undeclared success result.</span></span>
* <span data-ttu-id="1e544-109">Döndürülen bir durum kodunu belgeler.</span><span class="sxs-lookup"><span data-stu-id="1e544-109">Documents a status code that isn't returned.</span></span>
* <span data-ttu-id="1e544-110">Açık model doğrulama denetimi içerir.</span><span class="sxs-lookup"><span data-stu-id="1e544-110">Includes an explicit model validation check.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="reference-the-analyzer-package"></a><span data-ttu-id="1e544-111">Çözümleyici paketine başvur</span><span class="sxs-lookup"><span data-stu-id="1e544-111">Reference the analyzer package</span></span>

<span data-ttu-id="1e544-112">ASP.NET Core 3,0 veya sonraki sürümlerde, çözümleyiciler .NET Core SDK dahil edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1e544-112">In ASP.NET Core 3.0 or later, the analyzers are included in the .NET Core SDK.</span></span> <span data-ttu-id="1e544-113">Projenizdeki çözümleyici 'yi etkinleştirmek için, proje dosyasına `IncludeOpenAPIAnalyzers` özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1e544-113">To enable the analyzer in your project, include the `IncludeOpenAPIAnalyzers` property in the project file:</span></span>

```xml
<PropertyGroup>
 <IncludeOpenAPIAnalyzers>true</IncludeOpenAPIAnalyzers>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

## <a name="package-installation"></a><span data-ttu-id="1e544-114">Paket yüklemesi</span><span class="sxs-lookup"><span data-stu-id="1e544-114">Package installation</span></span>

<span data-ttu-id="1e544-115">Aşağıdaki yaklaşımlardan biriyle [Microsoft. AspNetCore. Mvc. api. çözümleyiciler](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet paketini yükler:</span><span class="sxs-lookup"><span data-stu-id="1e544-115">Install the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package with one of the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1e544-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e544-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1e544-117">**Paket Yöneticisi konsol** penceresinde:</span><span class="sxs-lookup"><span data-stu-id="1e544-117">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="1e544-118">> **Diğer** Windows paket> **Yöneticisi konsolunu**görüntüle ' ye gidin.</span><span class="sxs-lookup"><span data-stu-id="1e544-118">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="1e544-119">*Apiconventions. csproj* dosyasının bulunduğu dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="1e544-119">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="1e544-120">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1e544-120">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1e544-121">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e544-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1e544-122">**Çözüm bölmesi** paket> **Ekle...** ' da paketler klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1e544-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="1e544-123">**Paket Ekle** penceresinin **kaynak** açılan penceresini "NuGet.org" olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1e544-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="1e544-124">Arama kutusuna "Microsoft. AspNetCore. Mvc. api. çözümleyiciler" yazın.</span><span class="sxs-lookup"><span data-stu-id="1e544-124">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="1e544-125">Sonuçlar bölmesinden "Microsoft. AspNetCore. Mvc. api. çözümleyiciler" paketini seçin ve **paket Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1e544-125">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1e544-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e544-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1e544-127">**Tümleşik terminalden**aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1e544-127">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1e544-128">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1e544-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1e544-129">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1e544-129">Run the following command:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

::: moniker-end

## <a name="analyzers-for-web-api-conventions"></a><span data-ttu-id="1e544-130">Web API kuralları için çözümleyiciler</span><span class="sxs-lookup"><span data-stu-id="1e544-130">Analyzers for web API conventions</span></span>

<span data-ttu-id="1e544-131">Openapı belgeleri, bir eylemin döndürebildiği durum kodlarını ve yanıt türlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="1e544-131">OpenAPI documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="1e544-132">ASP.NET Core MVC 'de, <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> ve <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> gibi öznitelikler bir eylemi belgelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1e544-132">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="1e544-133"><xref:tutorials/web-api-help-pages-using-swagger>Web API 'nizi belgeleme hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="1e544-133"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your web API.</span></span>

<span data-ttu-id="1e544-134">Paketteki çözümleyicilerden biri ile <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> açıklanmış denetimleri inceler ve yanıtlarını tamamen belgemeyen eylemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1e544-134">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="1e544-135">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1e544-135">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=10)]

<span data-ttu-id="1e544-136">Yukarıdaki eylem HTTP 200 başarılı dönüş türünü belgeler, ancak HTTP 404 hata durumu kodunu belgeetmez.</span><span class="sxs-lookup"><span data-stu-id="1e544-136">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="1e544-137">Çözümleyici, HTTP 404 durum kodu için eksik belgeleri uyarı olarak bildirir.</span><span class="sxs-lookup"><span data-stu-id="1e544-137">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="1e544-138">Sorunu gidermeye yönelik bir seçenek sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1e544-138">An option to fix the problem is provided.</span></span>

![Çözümleyici bir uyarı bildiriyor](conventions/_static/Analyzer.gif)

## <a name="additional-resources"></a><span data-ttu-id="1e544-140">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1e544-140">Additional resources</span></span>

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:web-api/index>
