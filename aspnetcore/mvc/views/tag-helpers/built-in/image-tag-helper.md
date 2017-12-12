---
title: "Görüntü etiketi Yardımcısı | Microsoft Docs"
author: pkellner
description: "Görüntü etiketi Yardımcısı ile çalışmaya nasıl gösterir"
keywords: "ASP.NET Core, etiket Yardımcısı"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a013
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 0d55514508b963ce05031f89a20af696f5d4a670
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="imagetaghelper"></a><span data-ttu-id="99bbe-104">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="99bbe-104">ImageTagHelper</span></span>

<span data-ttu-id="99bbe-105">Tarafından [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="99bbe-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="99bbe-106">Görüntü etiketi yardımcı geliştirir `img` (`<img>`) etiketi.</span><span class="sxs-lookup"><span data-stu-id="99bbe-106">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="99bbe-107">Bunu gerektiren bir `src` etiketi yanı sıra `boolean` özniteliği `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="99bbe-107">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="99bbe-108">Varsa görüntü kaynağı (`src`) statik bir dosya mı ana web sunucusunda dize busting benzersiz bir önbellek görüntü kaynağı için sorgu parametresi olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="99bbe-108">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="99bbe-109">Bu, ana bilgisayar web sunucusunda dosyasını değişirse benzersiz istek URL'si güncelleştirilmiş istek parametresi içeren oluştuğunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="99bbe-109">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="99bbe-110">Dize busting önbellek statik görüntü dosyasının karma temsil eden benzersiz bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="99bbe-110">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="99bbe-111">Varsa görüntü kaynağı (`src`) (örneğin bir uzak URL veya dosya yoksa sunucuda) statik bir dosya değil `<img>` etiketinin `src` özniteliği önbellek sorgu dizesi parametresi busting yok oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="99bbe-111">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="99bbe-112">Görüntü etiketi yardımcı öznitelik</span><span class="sxs-lookup"><span data-stu-id="99bbe-112">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="99bbe-113">ASP append sürümü</span><span class="sxs-lookup"><span data-stu-id="99bbe-113">asp-append-version</span></span>

<span data-ttu-id="99bbe-114">İle birlikte belirtildiğinde bir `src` , resim etiketi yardımcı öznitelik çağrılır.</span><span class="sxs-lookup"><span data-stu-id="99bbe-114">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="99bbe-115">Geçerli bir örneği `img` etiketi yardımcı:</span><span class="sxs-lookup"><span data-stu-id="99bbe-115">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="99bbe-116">Statik dosya dizininde bulunuyorsa *... wwwroot/images/asplogo.PNG* oluşturulan html (karma farklı olacaktır) aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="99bbe-116">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="99bbe-117">Parametresine atanan değer `v` diskteki dosya karma değeri.</span><span class="sxs-lookup"><span data-stu-id="99bbe-117">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="99bbe-118">Web sunucusu için okuma erişimi alamadı ise statik dosya başvurulan, Hayır `v` parametreleri eklenir `src` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="99bbe-118">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="99bbe-119">src</span><span class="sxs-lookup"><span data-stu-id="99bbe-119">src</span></span>

<span data-ttu-id="99bbe-120">Görüntü etiketi yardımcı etkinleştirmek için src özniteliği gereklidir `<img>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="99bbe-120">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="99bbe-121">Görüntü etiketi yardımcı kullanan `Cache` hesaplanan depolamak için sağlayıcı yerel web sunucusunda `Sha512` , belirli bir dosya.</span><span class="sxs-lookup"><span data-stu-id="99bbe-121">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="99bbe-122">Dosyayı yeniden isteniyorsa `Sha512` hesaplanacak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="99bbe-122">If the file is requested again the `Sha512` does not need to be recalculated.</span></span> <span data-ttu-id="99bbe-123">Önbellek dosyaya iliştirilmiş bir dosya İzleyicisi tarafından geçersiz kılınan olduğunda dosyanın `Sha512` hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="99bbe-123">The Cache is invalidated by a file watcher that is attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="99bbe-124">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="99bbe-124">Additional resources</span></span>

* <xref:performance/caching/memory>
