---
title: ASP.NET Core görüntü etiketi yok
author: pkellner
description: Görüntü etiketi Yardımcısı ile çalışmaya nasıl gösterir
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 6aa9175f873c4ea62e0319c812e5312cd3331141
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="370cf-103">ASP.NET Core görüntü etiketi yok</span><span class="sxs-lookup"><span data-stu-id="370cf-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="370cf-104">Tarafından [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="370cf-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="370cf-105">Görüntü etiketi yardımcı geliştirir `img` (`<img>`) etiketi.</span><span class="sxs-lookup"><span data-stu-id="370cf-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="370cf-106">Bunu gerektiren bir `src` etiketi yanı sıra `boolean` özniteliği `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="370cf-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="370cf-107">Varsa görüntü kaynağı (`src`) statik bir dosya mı ana web sunucusunda dize busting benzersiz bir önbellek görüntü kaynağı için sorgu parametresi olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="370cf-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="370cf-108">Bu, ana bilgisayar web sunucusunda dosyasını değişirse benzersiz istek URL'si güncelleştirilmiş istek parametresi içeren oluştuğunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="370cf-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="370cf-109">Dize busting önbellek statik görüntü dosyasının karma temsil eden benzersiz bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="370cf-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="370cf-110">Varsa görüntü kaynağı (`src`) (örneğin bir uzak URL veya dosya yoksa sunucuda) statik bir dosya değil `<img>` etiketinin `src` özniteliği önbellek sorgu dizesi parametresi busting yok oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="370cf-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="370cf-111">Görüntü etiketi yardımcı öznitelik</span><span class="sxs-lookup"><span data-stu-id="370cf-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="370cf-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="370cf-112">asp-append-version</span></span>

<span data-ttu-id="370cf-113">İle birlikte belirtildiğinde bir `src` , resim etiketi yardımcı öznitelik çağrılır.</span><span class="sxs-lookup"><span data-stu-id="370cf-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="370cf-114">Geçerli bir örneği `img` etiketi yardımcı:</span><span class="sxs-lookup"><span data-stu-id="370cf-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="370cf-115">Statik dosya dizininde bulunuyorsa *... wwwroot/images/asplogo.PNG* oluşturulan html (karma farklı olacaktır) aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="370cf-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="370cf-116">Parametresine atanan değer `v` diskteki dosya karma değeri.</span><span class="sxs-lookup"><span data-stu-id="370cf-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="370cf-117">Web sunucusu için okuma erişimi alamadı ise statik dosya başvurulan, Hayır `v` parametreleri eklenir `src` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="370cf-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="370cf-118">src</span><span class="sxs-lookup"><span data-stu-id="370cf-118">src</span></span>

<span data-ttu-id="370cf-119">Görüntü etiketi yardımcı etkinleştirmek için src özniteliği gereklidir `<img>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="370cf-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="370cf-120">Görüntü etiketi yardımcı kullanan `Cache` hesaplanan depolamak için sağlayıcı yerel web sunucusunda `Sha512` , belirli bir dosya.</span><span class="sxs-lookup"><span data-stu-id="370cf-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="370cf-121">Dosyayı yeniden isteniyorsa `Sha512` hesaplanacak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="370cf-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="370cf-122">Önbellek dosyaya iliştirilmiş bir dosya İzleyicisi tarafından geçersiz kılınan olduğunda dosyanın `Sha512` hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="370cf-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="370cf-123">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="370cf-123">Additional resources</span></span>

* <xref:performance/caching/memory>
