---
title: "ASP.NET Core MVC etiketi yardımcı önbelleğe alma"
author: pkellner
description: "Önbellek etiket Yardımcısı ile çalışmaya nasıl gösterir"
keywords: "ASP.NET Core, etiket Yardımcısı"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 1710a5781fb69aaa6101270d6b4fd44f92c7f06c
ms.sourcegitcommit: a33737ea24e1ea9642e461d1bc90d6701f889436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="227c4-104">ASP.NET Core MVC etiketi yardımcı önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="227c4-104">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="227c4-105">Tarafından [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="227c4-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="227c4-106">Önbellek etiket Yardımcısı iç ASP.NET Core önbelleği sağlayıcısı için içeriği önbelleğe alarak, ASP.NET Core uygulamanızın performansını önemli ölçüde artırmak için yeteneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="227c4-106">The  Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="227c4-107">Razor görüntüleme altyapısı varsayılan ayarlar `expires-after` yirmi dakika.</span><span class="sxs-lookup"><span data-stu-id="227c4-107">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="227c4-108">Aşağıdaki Razor biçimlendirme tarih/saat önbelleğe alır:</span><span class="sxs-lookup"><span data-stu-id="227c4-108">The following Razor markup caches the date/time:</span></span>

```cshtml
<Cache>@DateTime.Now<Cache>
```

<span data-ttu-id="227c4-109">İçeren sayfaya yapılan ilk istek `CacheTagHelper` geçerli tarih görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="227c4-109">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="227c4-110">Önbellek (varsayılan 20 dakika) süresi dolana veya bellek baskısı tarafından çıkarılacak kadar ek istekleri önbelleğe alınan değeri gösterir.</span><span class="sxs-lookup"><span data-stu-id="227c4-110">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="227c4-111">Aşağıdaki özniteliklerle önbellek süresini ayarlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="227c4-111">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="227c4-112">Etiket Yardımcısı özniteliklerini önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="227c4-112">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="227c4-113">Etkin</span><span class="sxs-lookup"><span data-stu-id="227c4-113">enabled</span></span>    


| <span data-ttu-id="227c4-114">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-114">Attribute Type</span></span>    | <span data-ttu-id="227c4-115">Geçerli Değerler</span><span class="sxs-lookup"><span data-stu-id="227c4-115">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="227c4-116">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="227c4-116">boolean</span></span>           | <span data-ttu-id="227c4-117">"true" (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="227c4-117">"true" (default)</span></span>  |
|                   | <span data-ttu-id="227c4-118">"false"</span><span class="sxs-lookup"><span data-stu-id="227c4-118">"false"</span></span>   |


<span data-ttu-id="227c4-119">Önbellek etiket Yardımcısı tarafından içine içeriğin önbellekte olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="227c4-119">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="227c4-120">Varsayılan, `true` değeridir.</span><span class="sxs-lookup"><span data-stu-id="227c4-120">The default is `true`.</span></span>  <span data-ttu-id="227c4-121">Varsa kümesine `false` bu önbelleği etiket Yardımcısı işlenmiş çıktıyı önbelleğe alma hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="227c4-121">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="227c4-122">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-122">Example:</span></span>

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="227c4-123">süresi dolmadan üzerinde</span><span class="sxs-lookup"><span data-stu-id="227c4-123">expires-on</span></span> 

| <span data-ttu-id="227c4-124">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-124">Attribute Type</span></span>    | <span data-ttu-id="227c4-125">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="227c4-125">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="227c4-126">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="227c4-126">DateTimeOffset</span></span>    | <span data-ttu-id="227c4-127">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="227c4-127">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="227c4-128">Bir mutlak sona erme tarihi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="227c4-128">Sets an absolute expiration date.</span></span> <span data-ttu-id="227c4-129">Aşağıdaki örnek, 17:02:00 saatleri 29 Ocak 2025 üzerinde kadar önbellek etiket Yardımcısı içeriğini önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="227c4-129">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="227c4-130">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-130">Example:</span></span>

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="227c4-131">süresi dolduktan sonra</span><span class="sxs-lookup"><span data-stu-id="227c4-131">expires-after</span></span>

| <span data-ttu-id="227c4-132">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-132">Attribute Type</span></span>    | <span data-ttu-id="227c4-133">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="227c4-133">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="227c4-134">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="227c4-134">TimeSpan</span></span>    | <span data-ttu-id="227c4-135">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="227c4-135">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="227c4-136">Süre içeriği önbelleğe almak için ilk istek saati ayarlar.</span><span class="sxs-lookup"><span data-stu-id="227c4-136">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="227c4-137">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-137">Example:</span></span>

```cshtml
<Cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="227c4-138">süresi dolmadan kayan</span><span class="sxs-lookup"><span data-stu-id="227c4-138">expires-sliding</span></span>

| <span data-ttu-id="227c4-139">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-139">Attribute Type</span></span>    | <span data-ttu-id="227c4-140">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="227c4-140">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="227c4-141">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="227c4-141">TimeSpan</span></span>    | <span data-ttu-id="227c4-142">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="227c4-142">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="227c4-143">Değil erişilen, önbellek girişi çıkarılacak süreyi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="227c4-143">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="227c4-144">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-144">Example:</span></span>

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="227c4-145">farklılık tarafından-üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="227c4-145">vary-by-header</span></span>

| <span data-ttu-id="227c4-146">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-146">Attribute Type</span></span>    | <span data-ttu-id="227c4-147">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="227c4-147">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="227c4-148">Dize</span><span class="sxs-lookup"><span data-stu-id="227c4-148">String</span></span>            | <span data-ttu-id="227c4-149">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="227c4-149">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="227c4-150">"User-Agent, içerik kodlaması"</span><span class="sxs-lookup"><span data-stu-id="227c4-150">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="227c4-151">Bir tek üstbilgi değeri veya bir önbellek yenileme bunlar değiştirdiğinizde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="227c4-151">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="227c4-152">Aşağıdaki örnek üstbilgi değeri izler `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="227c4-152">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="227c4-153">Bu örnek için içeriği önbelleğe alır her farklı `User-Agent` web sunucusuna sunulur.</span><span class="sxs-lookup"><span data-stu-id="227c4-153">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="227c4-154">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-154">Example:</span></span>

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="227c4-155">farklılık-by-sorgusu</span><span class="sxs-lookup"><span data-stu-id="227c4-155">vary-by-query</span></span>

| <span data-ttu-id="227c4-156">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-156">Attribute Type</span></span>    | <span data-ttu-id="227c4-157">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="227c4-157">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="227c4-158">Dize</span><span class="sxs-lookup"><span data-stu-id="227c4-158">String</span></span>            | <span data-ttu-id="227c4-159">"Oluştur"</span><span class="sxs-lookup"><span data-stu-id="227c4-159">"Make"</span></span>                |
|                   | <span data-ttu-id="227c4-160">"Marka, Model"</span><span class="sxs-lookup"><span data-stu-id="227c4-160">"Make,Model"</span></span> |

<span data-ttu-id="227c4-161">Bir tek üstbilgi değeri veya bir önbellek yenileme üstbilgi değeri değiştiğinde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="227c4-161">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="227c4-162">Aşağıdaki örnek değerlere görünmesi `Make` ve `Model`.</span><span class="sxs-lookup"><span data-stu-id="227c4-162">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="227c4-163">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-163">Example:</span></span>

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="227c4-164">farklılık tarafından-yönlendirme</span><span class="sxs-lookup"><span data-stu-id="227c4-164">vary-by-route</span></span>

| <span data-ttu-id="227c4-165">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-165">Attribute Type</span></span>    | <span data-ttu-id="227c4-166">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="227c4-166">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="227c4-167">Dize</span><span class="sxs-lookup"><span data-stu-id="227c4-167">String</span></span>            | <span data-ttu-id="227c4-168">"Oluştur"</span><span class="sxs-lookup"><span data-stu-id="227c4-168">"Make"</span></span>                |
|                   | <span data-ttu-id="227c4-169">"Marka, Model"</span><span class="sxs-lookup"><span data-stu-id="227c4-169">"Make,Model"</span></span> |

<span data-ttu-id="227c4-170">Bir tek üstbilgi değeri veya değişiklik rota veri parametresinin Değer yükleyen bir önbellek yenileme tetiklemek üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="227c4-170">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="227c4-171">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-171">Example:</span></span>

<span data-ttu-id="227c4-172">*Haline*</span><span class="sxs-lookup"><span data-stu-id="227c4-172">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="227c4-173">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="227c4-173">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="227c4-174">farklılık-tarafından-tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="227c4-174">vary-by-cookie</span></span>

| <span data-ttu-id="227c4-175">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-175">Attribute Type</span></span>    | <span data-ttu-id="227c4-176">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="227c4-176">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="227c4-177">Dize</span><span class="sxs-lookup"><span data-stu-id="227c4-177">String</span></span>            | <span data-ttu-id="227c4-178">". AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="227c4-178">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="227c4-179">". AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="227c4-179">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="227c4-180">Bir tek üstbilgi değeri veya bir önbellek yenileme (s) üstbilgi değerleri değiştiğinde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="227c4-180">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="227c4-181">Aşağıdaki örnek, ASP.NET kimliği ile ilişkili tanımlama bakar.</span><span class="sxs-lookup"><span data-stu-id="227c4-181">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="227c4-182">Bir kullanıcının kimliği doğrulanır zaman, bir önbellek yenileme tetikler ayarlanacak istek tanımlama bilgisi.</span><span class="sxs-lookup"><span data-stu-id="227c4-182">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="227c4-183">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-183">Example:</span></span>

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="227c4-184">farklı kullanıcı tarafından</span><span class="sxs-lookup"><span data-stu-id="227c4-184">vary-by-user</span></span>

| <span data-ttu-id="227c4-185">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-185">Attribute Type</span></span>    | <span data-ttu-id="227c4-186">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="227c4-186">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="227c4-187">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="227c4-187">Boolean</span></span>             | <span data-ttu-id="227c4-188">"true"</span><span class="sxs-lookup"><span data-stu-id="227c4-188">"true"</span></span>                  |
|                     | <span data-ttu-id="227c4-189">"false" (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="227c4-189">"false" (default)</span></span> |

<span data-ttu-id="227c4-190">Oturum açma kullanıcı (veya içerik asıl) değiştiğinde önbelleği sıfırlamalıdır olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="227c4-190">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="227c4-191">Geçerli kullanıcı olarak da bilinen istek bağlamı asıl ve Razor görünümünde başvurarak görüntülenebilir `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="227c4-191">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="227c4-192">Aşağıdaki örnek, geçerli kullanıcının oturum bakar.</span><span class="sxs-lookup"><span data-stu-id="227c4-192">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="227c4-193">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-193">Example:</span></span>

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="227c4-194">Bu öznitelik kullanarak oturum açması ve günlük genişletme döngüsü boyunca Önbellekteki içeriği tutar.</span><span class="sxs-lookup"><span data-stu-id="227c4-194">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="227c4-195">Kullanırken `vary-by-user="true"`, bir oturum açma ve oturum genişletme eylemi önbellek kimliği doğrulanmış kullanıcı için geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="227c4-195">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="227c4-196">Yeni bir benzersiz tanımlama bilgisi değerini oturum açma üretildiği için önbellek geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="227c4-196">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="227c4-197">Tanımlama bilgisi mevcut olduğunda veya süresi dolmuş önbelleği için anonim durumu korunur.</span><span class="sxs-lookup"><span data-stu-id="227c4-197">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="227c4-198">Bu, hiçbir kullanıcı oturum açtıysa, önbellek sürdürüleceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="227c4-198">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="227c4-199">farklılık tarafından</span><span class="sxs-lookup"><span data-stu-id="227c4-199">vary-by</span></span>

| <span data-ttu-id="227c4-200">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-200">Attribute Type</span></span>    | <span data-ttu-id="227c4-201">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="227c4-201">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="227c4-202">Dize</span><span class="sxs-lookup"><span data-stu-id="227c4-202">String</span></span>             | <span data-ttu-id="227c4-203">"@Model"</span><span class="sxs-lookup"><span data-stu-id="227c4-203">"@Model"</span></span>                 |


<span data-ttu-id="227c4-204">Hangi verileri önbelleğe, özelleştirme için sağlar.</span><span class="sxs-lookup"><span data-stu-id="227c4-204">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="227c4-205">Önbellek etiket Yardımcısı içeriğini özniteliğin dize değeri değişiklikleri tarafından başvurulan nesne güncelleştirildiğinde.</span><span class="sxs-lookup"><span data-stu-id="227c4-205">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="227c4-206">Genellikle dize birleştirme modeli değerlerin bu özniteliğe atanır.</span><span class="sxs-lookup"><span data-stu-id="227c4-206">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="227c4-207">Etkili bir şekilde birleştirilmiş değerleri için bir güncelleştirme önbelleği geçersiz kılar anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="227c4-207">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="227c4-208">Aşağıdaki örnekte iki rota parametrelerinin tamsayı değerini görünüm SUM'ları oluşturma denetleyici yöntemi varsayar `myParam1` ve `myParam2`ve tek model özelliği döndürür.</span><span class="sxs-lookup"><span data-stu-id="227c4-208">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="227c4-209">Bu toplamın değiştiğinde önbelleği etiket Yardımcısı içeriğini çizilir ve tekrar önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="227c4-209">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="227c4-210">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-210">Example:</span></span>

<span data-ttu-id="227c4-211">Eylem:</span><span class="sxs-lookup"><span data-stu-id="227c4-211">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="227c4-212">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="227c4-212">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="227c4-213">önceliği</span><span class="sxs-lookup"><span data-stu-id="227c4-213">priority</span></span>

| <span data-ttu-id="227c4-214">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="227c4-214">Attribute Type</span></span>    | <span data-ttu-id="227c4-215">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="227c4-215">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="227c4-216">CacheltemPriority</span><span class="sxs-lookup"><span data-stu-id="227c4-216">CacheItemPriority</span></span>  | <span data-ttu-id="227c4-217">"Yüksek"</span><span class="sxs-lookup"><span data-stu-id="227c4-217">"High"</span></span>                   |
|                    | <span data-ttu-id="227c4-218">"Düşük"</span><span class="sxs-lookup"><span data-stu-id="227c4-218">"Low"</span></span> |
|                    | <span data-ttu-id="227c4-219">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="227c4-219">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="227c4-220">"Normal"</span><span class="sxs-lookup"><span data-stu-id="227c4-220">"Normal"</span></span> |

<span data-ttu-id="227c4-221">Önbellek çıkarma kılavuzu için yerleşik önbelleği sağlayıcısı sağlar.</span><span class="sxs-lookup"><span data-stu-id="227c4-221">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="227c4-222">Web sunucusu çıkarırsınız `Low` bellek baskısı altında olduğunda girişleri ilk önbelleğe.</span><span class="sxs-lookup"><span data-stu-id="227c4-222">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="227c4-223">Örnek:</span><span class="sxs-lookup"><span data-stu-id="227c4-223">Example:</span></span>

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="227c4-224">`priority` Özniteliği önbellek bekletme belirli bir düzeyde garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="227c4-224">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="227c4-225">`CacheItemPriority`yalnızca bir öneridir.</span><span class="sxs-lookup"><span data-stu-id="227c4-225">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="227c4-226">Bu öznitelik ayarını `NeverRemove` önbelleği her zaman korunması garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="227c4-226">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="227c4-227">Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="227c4-227">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="227c4-228">Önbellek etiket Yardımcısı bağlı [bellek önbellek hizmeti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="227c4-228">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="227c4-229">Değil eklenmişse önbellek etiket Yardımcısı hizmet ekler.</span><span class="sxs-lookup"><span data-stu-id="227c4-229">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="227c4-230">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="227c4-230">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
