---
title: ASP.NET core'da görüntü etiketi Yardımcısı
author: pkellner
description: Görüntü etiketi Yardımcısı ile çalışma işlemi gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 916a68c187cbf516a59d3c5d7578cdb6ada01b86
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901001"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="da5ff-103">ASP.NET core'da görüntü etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="da5ff-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="da5ff-104">Tarafından [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="da5ff-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="da5ff-105">Görüntü etiketi Yardımcısı geliştirir `<img>` statik resim dosyaları için önbellek busting davranışı sağlamak için etiket.</span><span class="sxs-lookup"><span data-stu-id="da5ff-105">The Image Tag Helper enhances the `<img>` tag to provide cache-busting behavior for static image files.</span></span>

<span data-ttu-id="da5ff-106">Bir önbellek busting dize varlığın URL'ye eklenen statik görüntü dosyasının bir karmasını temsil eden benzersiz bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="da5ff-106">A cache-busting string is a unique value representing the hash of the static image file appended to the asset's URL.</span></span> <span data-ttu-id="da5ff-107">Benzersiz bir dize istemciler (ve bazı Ara sunucular) konak web sunucusu ve istemci önbelleğinden görüntü yeniden yüklemek ister.</span><span class="sxs-lookup"><span data-stu-id="da5ff-107">The unique string prompts clients (and some proxies) to reload the image from the host web server and not from the client's cache.</span></span>

<span data-ttu-id="da5ff-108">Varsa resim kaynağını (`src`) konak web sunucusundaki bir statik dosya:</span><span class="sxs-lookup"><span data-stu-id="da5ff-108">If the image source (`src`) is a static file on the host web server:</span></span>

* <span data-ttu-id="da5ff-109">Önbellek busting benzersiz bir dize, görüntü kaynağı için bir sorgu parametresi olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="da5ff-109">A unique cache-busting string is appended as a query parameter to the image source.</span></span>
* <span data-ttu-id="da5ff-110">Konak web sunucusunda dosyayı değişirse, güncelleştirilmiş istek parametresi içeren bir benzersiz istek URL'si oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="da5ff-110">If the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span>

<span data-ttu-id="da5ff-111">Etiket Yardımcıları genel bakış için bkz. <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="da5ff-111">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="da5ff-112">Görüntü etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="da5ff-112">Image Tag Helper Attributes</span></span>

### <a name="src"></a><span data-ttu-id="da5ff-113">src</span><span class="sxs-lookup"><span data-stu-id="da5ff-113">src</span></span>

<span data-ttu-id="da5ff-114">Görüntü etiketi Yardımcısı, etkinleştirme `src` öznitelik gerekli `<img>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="da5ff-114">To activate the Image Tag Helper, the `src` attribute is required on the `<img>` element.</span></span>

<span data-ttu-id="da5ff-115">Resim kaynağını (`src`) statik fiziksel bir dosya sunucusuna işaret etmelidir.</span><span class="sxs-lookup"><span data-stu-id="da5ff-115">The image source (`src`) must point to a physical static file on the server.</span></span> <span data-ttu-id="da5ff-116">Varsa `src` uzak bir URI önbelleği busting sorgu dizesi parametresi oluşturulmadığından.</span><span class="sxs-lookup"><span data-stu-id="da5ff-116">If the `src` is a remote URI, the cache-busting query string parameter isn't generated.</span></span>

### <a name="asp-append-version"></a><span data-ttu-id="da5ff-117">ASP ekleme sürümü</span><span class="sxs-lookup"><span data-stu-id="da5ff-117">asp-append-version</span></span>

<span data-ttu-id="da5ff-118">Zaman `asp-append-version` ile belirtilen bir `true` değer ile birlikte bir `src` özniteliği, görüntü etiketi Yardımcısı çağrılır.</span><span class="sxs-lookup"><span data-stu-id="da5ff-118">When `asp-append-version` is specified with a `true` value along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="da5ff-119">Aşağıdaki örnek, bir görüntü etiketi Yardımcısı kullanır:</span><span class="sxs-lookup"><span data-stu-id="da5ff-119">The following example uses an Image Tag Helper:</span></span>

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true">
```

<span data-ttu-id="da5ff-120">Statik dosya dizininde bulunuyorsa */wwwroot/resimler/*, oluşturulan HTML (karma farklı olacaktır) aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="da5ff-120">If the static file exists in the directory */wwwroot/images/*, the generated HTML is similar to the following (the hash will be different):</span></span>

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM">
```

<span data-ttu-id="da5ff-121">Parametresine atanan değer `v` karma değeri *asplogo.png* diskteki dosya.</span><span class="sxs-lookup"><span data-stu-id="da5ff-121">The value assigned to the parameter `v` is the hash value of the *asplogo.png* file on disk.</span></span> <span data-ttu-id="da5ff-122">Web sunucusu statik dosya için okuma erişimi alamadı ise hiçbir `v` parametresi eklenir `src` biçimlendirmenin özniteliği.</span><span class="sxs-lookup"><span data-stu-id="da5ff-122">If the web server is unable to obtain read access to the static file, no `v` parameter is added to the `src` attribute in the rendered markup.</span></span>

## <a name="hash-caching-behavior"></a><span data-ttu-id="da5ff-123">Karma önbelleğe alma davranışı</span><span class="sxs-lookup"><span data-stu-id="da5ff-123">Hash caching behavior</span></span>

<span data-ttu-id="da5ff-124">Görüntü etiketi Yardımcısı önbelleği sağlayıcısı hesaplanan depolamak için yerel web sunucusu üzerinde kullanır `Sha512` belirli bir dosya karması.</span><span class="sxs-lookup"><span data-stu-id="da5ff-124">The Image Tag Helper uses the cache provider on the local web server to store the calculated `Sha512` hash of a given file.</span></span> <span data-ttu-id="da5ff-125">Dosyanın birden çok kez istenirse, karma yeniden hesaplanması değil.</span><span class="sxs-lookup"><span data-stu-id="da5ff-125">If the file is requested multiple times, the hash isn't recalculated.</span></span> <span data-ttu-id="da5ff-126">Önbellek dosyaya eklenmiş bir dosya İzleyicisi tarafından geçersiz olduğunda dosyanın `Sha512` karma hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="da5ff-126">The cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` hash is calculated.</span></span> <span data-ttu-id="da5ff-127">Dosyanın diskte değiştiğinde yeni bir karma hesaplanır ve önbelleğe alınmış.</span><span class="sxs-lookup"><span data-stu-id="da5ff-127">When the file changes on disk, a new hash is calculated and cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="da5ff-128">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="da5ff-128">Additional resources</span></span>

* <xref:performance/caching/memory>
