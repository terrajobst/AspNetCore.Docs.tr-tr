---
title: Web API Çözümleyicileri kullanma
author: pranavkm
description: Web API Çözümleyicileri Microsoft.AspNetCore.Mvc.Api.Analyzers içinde hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 12/14/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 7558552586d3056c43d8bfd9ef74cbcb3396726f
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425100"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="943e9-103">Web API Çözümleyicileri kullanma</span><span class="sxs-lookup"><span data-stu-id="943e9-103">Use web API analyzers</span></span>

<span data-ttu-id="943e9-104">ASP.NET Core 2.2 ve sonraki sürümleri içeren [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) web API'leri için çözümleyicilerini içeren bir NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="943e9-104">ASP.NET Core 2.2 and later includes the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package containing analyzers for web APIs.</span></span> <span data-ttu-id="943e9-105">Denetleyicileri ile açıklanan Çözümleyicileri çalışmak <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, oluşturmaya çalışırken [API kuralları](xref:web-api/advanced/conventions).</span><span class="sxs-lookup"><span data-stu-id="943e9-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [API conventions](xref:web-api/advanced/conventions).</span></span>

## <a name="package-installation"></a><span data-ttu-id="943e9-106">Paket yüklemesi</span><span class="sxs-lookup"><span data-stu-id="943e9-106">Package installation</span></span>

<span data-ttu-id="943e9-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` aşağıdaki yaklaşımlardan birini eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="943e9-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` can be added with one of the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="943e9-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="943e9-108">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="943e9-109">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="943e9-109">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="943e9-110">Git **görünümü** > **diğer Windows** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="943e9-110">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="943e9-111">Dizin gidin *ApiConventions.csproj* dosya yok.</span><span class="sxs-lookup"><span data-stu-id="943e9-111">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="943e9-112">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="943e9-112">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* <span data-ttu-id="943e9-113">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="943e9-113">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="943e9-114">Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="943e9-114">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**.</span></span>
  * <span data-ttu-id="943e9-115">Ayarlama **paket kaynağı** "nuget.org'da".</span><span class="sxs-lookup"><span data-stu-id="943e9-115">Set the **Package source** to "nuget.org".</span></span>
  * <span data-ttu-id="943e9-116">Arama kutusuna "Microsoft.AspNetCore.Mvc.Api.Analyzers" girin.</span><span class="sxs-lookup"><span data-stu-id="943e9-116">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
  * <span data-ttu-id="943e9-117">"Microsoft.AspNetCore.Mvc.Api.Analyzers" paketinden seçin **Gözat** sekmesine **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="943e9-117">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the **Browse** tab and click **Install**.</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="943e9-118">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="943e9-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="943e9-119">Sağ *paketleri* klasöründe **çözüm bölmesi** > **paketleri Ekle...** .</span><span class="sxs-lookup"><span data-stu-id="943e9-119">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="943e9-120">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org'da" açılır.</span><span class="sxs-lookup"><span data-stu-id="943e9-120">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="943e9-121">Arama kutusuna "Microsoft.AspNetCore.Mvc.Api.Analyzers" girin.</span><span class="sxs-lookup"><span data-stu-id="943e9-121">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="943e9-122">Sonuçlar bölmesinde "Microsoft.AspNetCore.Mvc.Api.Analyzers" paketi seçin ve tıklayın **Paketi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="943e9-122">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="943e9-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="943e9-123">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="943e9-124">Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:</span><span class="sxs-lookup"><span data-stu-id="943e9-124">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="943e9-125">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="943e9-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="943e9-126">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="943e9-126">Run the following command:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a><span data-ttu-id="943e9-127">API kuralları için Çözümleyicileri</span><span class="sxs-lookup"><span data-stu-id="943e9-127">Analyzers for API conventions</span></span>

<span data-ttu-id="943e9-128">Openapı belgeleri, durum kodları ve eylem döndürebilir ve yanıt türleri içerir.</span><span class="sxs-lookup"><span data-stu-id="943e9-128">OpenAPI documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="943e9-129">ASP.NET Core MVC gibi öznitelikleri <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> ve <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> eylem belgelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="943e9-129">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="943e9-130"><xref:tutorials/web-api-help-pages-using-swagger> API'nizi belgeleme üzerinde daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="943e9-130"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your API.</span></span>

<span data-ttu-id="943e9-131">Paketteki Çözümleyicileri birini inceler denetleyicileri ile açıklanan <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> ve yanıtlarını tamamen belge yok eylemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="943e9-131">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="943e9-132">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="943e9-132">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

<span data-ttu-id="943e9-133">Önceki eylemi HTTP başarı dönüş türü, ancak belge olmayan HTTP 404 hatası durum kodu 200 belgeler.</span><span class="sxs-lookup"><span data-stu-id="943e9-133">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="943e9-134">Çözümleyici eksik HTTP 404 durum kodu belgelerine bir uyarı olarak bildirir.</span><span class="sxs-lookup"><span data-stu-id="943e9-134">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="943e9-135">Sorunu düzeltmek için bir seçenek sağlanır.</span><span class="sxs-lookup"><span data-stu-id="943e9-135">An option to fix the problem is provided.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="943e9-136">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="943e9-136">Additional resources</span></span>

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* [<span data-ttu-id="943e9-137">Ek açıklama ApiController özniteliğine sahip</span><span class="sxs-lookup"><span data-stu-id="943e9-137">Annotation with ApiController attribute</span></span>](xref:web-api/index#annotation-with-apicontroller-attribute)
