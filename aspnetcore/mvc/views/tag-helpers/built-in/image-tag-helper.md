---
title: ASP.NET Core resim etiketi Yardımcısı
author: pkellner
description: Resim etiketi Yardımcısı ile çalışmayı gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 964072ad276f7e3e411ee41cb03a2efb9d05c585
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663998"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="43f3b-103">ASP.NET Core resim etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="43f3b-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="43f3b-104">By [Peter Kellner](https://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="43f3b-104">By [Peter Kellner](https://peterkellner.net)</span></span>

<span data-ttu-id="43f3b-105">Resim etiketi Yardımcısı, statik görüntü dosyaları için önbellek-busting davranışı sağlamak üzere `<img>` etiketini geliştirir.</span><span class="sxs-lookup"><span data-stu-id="43f3b-105">The Image Tag Helper enhances the `<img>` tag to provide cache-busting behavior for static image files.</span></span>

<span data-ttu-id="43f3b-106">Önbellek-busting dizesi, varlığın URL 'sine eklenen statik görüntü dosyasının karmasını temsil eden benzersiz bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="43f3b-106">A cache-busting string is a unique value representing the hash of the static image file appended to the asset's URL.</span></span> <span data-ttu-id="43f3b-107">Benzersiz dize, istemcilerin (ve bazı proxy 'lerde), görüntüyü istemci önbelleğinden değil, ana bilgisayar Web sunucusundan yeniden yüklemesi için istemde bulunur.</span><span class="sxs-lookup"><span data-stu-id="43f3b-107">The unique string prompts clients (and some proxies) to reload the image from the host web server and not from the client's cache.</span></span>

<span data-ttu-id="43f3b-108">Görüntü kaynağı (`src`), ana bilgisayar Web sunucusunda bir statik dosya ise:</span><span class="sxs-lookup"><span data-stu-id="43f3b-108">If the image source (`src`) is a static file on the host web server:</span></span>

* <span data-ttu-id="43f3b-109">Benzersiz bir önbellek-busting dizesi, resim kaynağına bir sorgu parametresi olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="43f3b-109">A unique cache-busting string is appended as a query parameter to the image source.</span></span>
* <span data-ttu-id="43f3b-110">Ana bilgisayar Web sunucusundaki dosya değişirse, güncelleştirilmiş istek parametresini içeren benzersiz bir istek URL 'SI oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="43f3b-110">If the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span>

<span data-ttu-id="43f3b-111">Etiket Yardımcıları hakkında genel bilgi için bkz. <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="43f3b-111">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="43f3b-112">Resim etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="43f3b-112">Image Tag Helper Attributes</span></span>

### <a name="src"></a><span data-ttu-id="43f3b-113">YN</span><span class="sxs-lookup"><span data-stu-id="43f3b-113">src</span></span>

<span data-ttu-id="43f3b-114">Resim etiketi yardımcısını etkinleştirmek için, `<img>` öğesinde `src` özniteliği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="43f3b-114">To activate the Image Tag Helper, the `src` attribute is required on the `<img>` element.</span></span>

<span data-ttu-id="43f3b-115">Görüntü kaynağı (`src`), sunucudaki bir fiziksel statik dosyayı göstermelidir.</span><span class="sxs-lookup"><span data-stu-id="43f3b-115">The image source (`src`) must point to a physical static file on the server.</span></span> <span data-ttu-id="43f3b-116">`src` uzak bir URI ise, önbellek-busting sorgu dizesi parametresi oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="43f3b-116">If the `src` is a remote URI, the cache-busting query string parameter isn't generated.</span></span>

### <a name="asp-append-version"></a><span data-ttu-id="43f3b-117">ASP-Append-sürüm</span><span class="sxs-lookup"><span data-stu-id="43f3b-117">asp-append-version</span></span>

<span data-ttu-id="43f3b-118">`asp-append-version`, bir `src` özniteliğiyle birlikte bir `true` değeriyle belirtildiğinde, resim etiketi Yardımcısı çağrılır.</span><span class="sxs-lookup"><span data-stu-id="43f3b-118">When `asp-append-version` is specified with a `true` value along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="43f3b-119">Aşağıdaki örnek bir resim etiketi yardımcısını kullanır:</span><span class="sxs-lookup"><span data-stu-id="43f3b-119">The following example uses an Image Tag Helper:</span></span>

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true">
```

<span data-ttu-id="43f3b-120">Statik dosya */Wwwroot/images/* dizininde varsa, oluşturulan HTML şuna benzerdir (karma farklı olur):</span><span class="sxs-lookup"><span data-stu-id="43f3b-120">If the static file exists in the directory */wwwroot/images/*, the generated HTML is similar to the following (the hash will be different):</span></span>

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM">
```

<span data-ttu-id="43f3b-121">`v` parametreye atanan değer, diskteki *asplogo. png* dosyasının karma değeridir.</span><span class="sxs-lookup"><span data-stu-id="43f3b-121">The value assigned to the parameter `v` is the hash value of the *asplogo.png* file on disk.</span></span> <span data-ttu-id="43f3b-122">Web sunucusu statik dosyaya okuma erişimi alamadığında, işlenmiş İşaretlemede `src` özniteliğine `v` parametresi eklenmez.</span><span class="sxs-lookup"><span data-stu-id="43f3b-122">If the web server is unable to obtain read access to the static file, no `v` parameter is added to the `src` attribute in the rendered markup.</span></span>

## <a name="hash-caching-behavior"></a><span data-ttu-id="43f3b-123">Karma önbelleğe alma davranışı</span><span class="sxs-lookup"><span data-stu-id="43f3b-123">Hash caching behavior</span></span>

<span data-ttu-id="43f3b-124">Resim etiketi Yardımcısı, belirli bir dosyanın hesaplanmış `Sha512` karmasını depolamak için yerel Web sunucusundaki önbellek sağlayıcısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="43f3b-124">The Image Tag Helper uses the cache provider on the local web server to store the calculated `Sha512` hash of a given file.</span></span> <span data-ttu-id="43f3b-125">Dosya birden çok kez isteniyorsa, karma yeniden hesaplanmadı.</span><span class="sxs-lookup"><span data-stu-id="43f3b-125">If the file is requested multiple times, the hash isn't recalculated.</span></span> <span data-ttu-id="43f3b-126">Önbellek, dosyanın `Sha512` karması hesaplandığında dosyaya iliştirilmiş bir dosya İzleyicisi tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="43f3b-126">The cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` hash is calculated.</span></span> <span data-ttu-id="43f3b-127">Dosya diskte değiştiğinde yeni bir karma hesaplanır ve önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="43f3b-127">When the file changes on disk, a new hash is calculated and cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43f3b-128">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="43f3b-128">Additional resources</span></span>

* <xref:performance/caching/memory>
