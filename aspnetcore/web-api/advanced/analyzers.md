---
title: Web API Çözümleyicileri kullanma
author: pranavkm
description: ASP.NET Core MVC web API Çözümleyicileri paketi hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/05/2019
uid: web-api/advanced/analyzers
ms.openlocfilehash: 7b6a7328deb8718a2a1c67c104cec359a4f13497
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661919"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="44c22-103">Web API Çözümleyicileri kullanma</span><span class="sxs-lookup"><span data-stu-id="44c22-103">Use web API analyzers</span></span>

<span data-ttu-id="44c22-104">ASP.NET Core 2,2 ve üzeri, Web API projeleriyle kullanılması amaçlanan bir MVC Çözümleyicileri paketi sağlar.</span><span class="sxs-lookup"><span data-stu-id="44c22-104">ASP.NET Core 2.2 and later provides an MVC analyzers package intended for use with web API projects.</span></span> <span data-ttu-id="44c22-105">Çözümleyiciler, [Web API kuralları](xref:web-api/advanced/conventions)üzerinde derlenirken <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>ile açıklanmış olan denetleyicilerle çalışır.</span><span class="sxs-lookup"><span data-stu-id="44c22-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [web API conventions](xref:web-api/advanced/conventions).</span></span>

<span data-ttu-id="44c22-106">Çözümleyiciler paketi şu şekilde bir denetleyici eylemi bildirir:</span><span class="sxs-lookup"><span data-stu-id="44c22-106">The analyzers package notifies you of any controller action that:</span></span>

* <span data-ttu-id="44c22-107">Bildirilmemiş bir durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="44c22-107">Returns an undeclared status code.</span></span>
* <span data-ttu-id="44c22-108">Bildirilmemiş başarı sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="44c22-108">Returns an undeclared success result.</span></span>
* <span data-ttu-id="44c22-109">Döndürülen bir durum kodunu belgeler.</span><span class="sxs-lookup"><span data-stu-id="44c22-109">Documents a status code that isn't returned.</span></span>
* <span data-ttu-id="44c22-110">Açık model doğrulama denetimi içerir.</span><span class="sxs-lookup"><span data-stu-id="44c22-110">Includes an explicit model validation check.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="reference-the-analyzer-package"></a><span data-ttu-id="44c22-111">Çözümleyici paketine başvur</span><span class="sxs-lookup"><span data-stu-id="44c22-111">Reference the analyzer package</span></span>

<span data-ttu-id="44c22-112">ASP.NET Core 3,0 veya sonraki sürümlerde, çözümleyiciler .NET Core SDK dahil edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="44c22-112">In ASP.NET Core 3.0 or later, the analyzers are included in the .NET Core SDK.</span></span> <span data-ttu-id="44c22-113">Projenizde çözümleyici 'yi etkinleştirmek için, proje dosyasına `IncludeOpenAPIAnalyzers` özelliğini dahil edin:</span><span class="sxs-lookup"><span data-stu-id="44c22-113">To enable the analyzer in your project, include the `IncludeOpenAPIAnalyzers` property in the project file:</span></span>

```xml
<PropertyGroup>
 <IncludeOpenAPIAnalyzers>true</IncludeOpenAPIAnalyzers>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

## <a name="package-installation"></a><span data-ttu-id="44c22-114">Paket yüklemesi</span><span class="sxs-lookup"><span data-stu-id="44c22-114">Package installation</span></span>

<span data-ttu-id="44c22-115">Aşağıdaki yaklaşımlardan biriyle [Microsoft. AspNetCore. Mvc. api. çözümleyiciler](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet paketini yükler:</span><span class="sxs-lookup"><span data-stu-id="44c22-115">Install the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package with one of the following approaches:</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="44c22-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44c22-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="44c22-117">**Paket Yöneticisi konsol** penceresinde:</span><span class="sxs-lookup"><span data-stu-id="44c22-117">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="44c22-118">**Diğer Windows** > **paket Yöneticisi konsolu**> **görüntüle** ' ye gidin.</span><span class="sxs-lookup"><span data-stu-id="44c22-118">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="44c22-119">*Apiconventions. csproj* dosyasının bulunduğu dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="44c22-119">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="44c22-120">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="44c22-120">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

### <a name="visual-studio-for-mac"></a>[<span data-ttu-id="44c22-121">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44c22-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="44c22-122">Paket Ekle ' **Çözüm Bölmesi** > *paketler* klasörüne sağ tıklayın **...** .</span><span class="sxs-lookup"><span data-stu-id="44c22-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="44c22-123">**Paket Ekle** penceresinin **kaynak** açılan penceresini "NuGet.org" olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="44c22-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="44c22-124">Arama kutusuna "Microsoft. AspNetCore. Mvc. api. çözümleyiciler" yazın.</span><span class="sxs-lookup"><span data-stu-id="44c22-124">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="44c22-125">Sonuçlar bölmesinden "Microsoft. AspNetCore. Mvc. api. çözümleyiciler" paketini seçin ve **paket Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44c22-125">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-code"></a>[<span data-ttu-id="44c22-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="44c22-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="44c22-127">**Tümleşik terminalden**aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="44c22-127">Run the following command from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-cli"></a>[<span data-ttu-id="44c22-128">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="44c22-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="44c22-129">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="44c22-129">Run the following command:</span></span>

```dotnetcli
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

::: moniker-end

## <a name="analyzers-for-web-api-conventions"></a><span data-ttu-id="44c22-130">Web API kuralları için çözümleyiciler</span><span class="sxs-lookup"><span data-stu-id="44c22-130">Analyzers for web API conventions</span></span>

<span data-ttu-id="44c22-131">Openapı belgeleri, bir eylemin döndürebildiği durum kodlarını ve yanıt türlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="44c22-131">OpenAPI documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="44c22-132">ASP.NET Core MVC 'de <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> ve <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> gibi öznitelikler bir eylemi belgelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="44c22-132">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="44c22-133"><xref:tutorials/web-api-help-pages-using-swagger>, Web API 'nizi belgeleme hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="44c22-133"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your web API.</span></span>

<span data-ttu-id="44c22-134">Paketteki çözümleyiciler, <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> ile açıklanmış denetimleri inceler ve yanıtlarını tamamen belgemeyen eylemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="44c22-134">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="44c22-135">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="44c22-135">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=10)]

<span data-ttu-id="44c22-136">Yukarıdaki eylem HTTP 200 başarılı dönüş türünü belgeler, ancak HTTP 404 hata durumu kodunu belgeetmez.</span><span class="sxs-lookup"><span data-stu-id="44c22-136">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="44c22-137">Çözümleyici, HTTP 404 durum kodu için eksik belgeleri uyarı olarak bildirir.</span><span class="sxs-lookup"><span data-stu-id="44c22-137">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="44c22-138">Sorunu gidermeye yönelik bir seçenek sağlanır.</span><span class="sxs-lookup"><span data-stu-id="44c22-138">An option to fix the problem is provided.</span></span>

![Çözümleyici bir uyarı bildiriyor](conventions/_static/Analyzer.gif)

## <a name="additional-resources"></a><span data-ttu-id="44c22-140">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="44c22-140">Additional resources</span></span>

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:web-api/index>
