---
title: ASP.NET Core MVC etiketi yardımcı önbelleğe alma
author: pkellner
description: Önbellek etiket Yardımcısı ile çalışmaya nasıl gösterir
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 969716e21211513053f52049368a0a7190ffba47
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276558"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="4561a-103">ASP.NET Core MVC etiketi yardımcı önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="4561a-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="4561a-104">Tarafından [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="4561a-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="4561a-105">Önbellek etiket Yardımcısı iç ASP.NET Core önbelleği sağlayıcısı için içeriği önbelleğe alarak, ASP.NET Core uygulamanızın performansını önemli ölçüde artırmak için yeteneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="4561a-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="4561a-106">Razor görüntüleme altyapısı varsayılan ayarlar `expires-after` yirmi dakika.</span><span class="sxs-lookup"><span data-stu-id="4561a-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="4561a-107">Aşağıdaki Razor biçimlendirme tarih/saat önbelleğe alır:</span><span class="sxs-lookup"><span data-stu-id="4561a-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="4561a-108">İçeren sayfaya yapılan ilk istek `CacheTagHelper` geçerli tarih görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4561a-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="4561a-109">Önbellek (varsayılan 20 dakika) süresi dolana veya bellek baskısı tarafından çıkarılacak kadar ek istekleri önbelleğe alınan değeri gösterir.</span><span class="sxs-lookup"><span data-stu-id="4561a-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="4561a-110">Aşağıdaki özniteliklerle önbellek süresini ayarlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4561a-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="4561a-111">Etiket Yardımcısı özniteliklerini önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="4561a-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="4561a-112">Etkin</span><span class="sxs-lookup"><span data-stu-id="4561a-112">enabled</span></span>    


| <span data-ttu-id="4561a-113">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-113">Attribute Type</span></span>    | <span data-ttu-id="4561a-114">Geçerli Değerler</span><span class="sxs-lookup"><span data-stu-id="4561a-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="4561a-115">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="4561a-115">boolean</span></span>           | <span data-ttu-id="4561a-116">"true" (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="4561a-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="4561a-117">"false"</span><span class="sxs-lookup"><span data-stu-id="4561a-117">"false"</span></span>   |


<span data-ttu-id="4561a-118">Önbellek etiket Yardımcısı tarafından içine içeriğin önbellekte olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="4561a-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="4561a-119">Varsayılan, `true` değeridir.</span><span class="sxs-lookup"><span data-stu-id="4561a-119">The default is `true`.</span></span>  <span data-ttu-id="4561a-120">Varsa kümesine `false` bu önbelleği etiket Yardımcısı işlenmiş çıktıyı önbelleğe alma hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="4561a-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="4561a-121">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="4561a-122">süresi dolmadan üzerinde</span><span class="sxs-lookup"><span data-stu-id="4561a-122">expires-on</span></span> 

| <span data-ttu-id="4561a-123">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-123">Attribute Type</span></span> |           <span data-ttu-id="4561a-124">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="4561a-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="4561a-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="4561a-125">DateTimeOffset</span></span> | <span data-ttu-id="4561a-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="4561a-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="4561a-127">Bir mutlak sona erme tarihi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4561a-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="4561a-128">Aşağıdaki örnek, 17:02:00 saatleri 29 Ocak 2025 üzerinde kadar önbellek etiket Yardımcısı içeriğini önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="4561a-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="4561a-129">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="4561a-130">süresi dolduktan sonra</span><span class="sxs-lookup"><span data-stu-id="4561a-130">expires-after</span></span>

| <span data-ttu-id="4561a-131">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-131">Attribute Type</span></span> |        <span data-ttu-id="4561a-132">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="4561a-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="4561a-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="4561a-133">TimeSpan</span></span>    | <span data-ttu-id="4561a-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="4561a-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="4561a-135">Süre içeriği önbelleğe almak için ilk istek saati ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4561a-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="4561a-136">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="4561a-137">süresi dolmadan kayan</span><span class="sxs-lookup"><span data-stu-id="4561a-137">expires-sliding</span></span>

| <span data-ttu-id="4561a-138">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-138">Attribute Type</span></span> |        <span data-ttu-id="4561a-139">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="4561a-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="4561a-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="4561a-140">TimeSpan</span></span>    | <span data-ttu-id="4561a-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="4561a-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="4561a-142">Değil erişilen, önbellek girişi çıkarılacak süreyi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4561a-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="4561a-143">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="4561a-144">farklılık tarafından-üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="4561a-144">vary-by-header</span></span>

| <span data-ttu-id="4561a-145">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-145">Attribute Type</span></span>    | <span data-ttu-id="4561a-146">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="4561a-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4561a-147">Dize</span><span class="sxs-lookup"><span data-stu-id="4561a-147">String</span></span>            | <span data-ttu-id="4561a-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="4561a-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="4561a-149">"User-Agent, içerik kodlaması"</span><span class="sxs-lookup"><span data-stu-id="4561a-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="4561a-150">Bir tek üstbilgi değeri veya bir önbellek yenileme bunlar değiştirdiğinizde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="4561a-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="4561a-151">Aşağıdaki örnek üstbilgi değeri izler `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="4561a-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="4561a-152">Bu örnek için içeriği önbelleğe alır her farklı `User-Agent` web sunucusuna sunulur.</span><span class="sxs-lookup"><span data-stu-id="4561a-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="4561a-153">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="4561a-154">farklılık-by-sorgusu</span><span class="sxs-lookup"><span data-stu-id="4561a-154">vary-by-query</span></span>

| <span data-ttu-id="4561a-155">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-155">Attribute Type</span></span>    | <span data-ttu-id="4561a-156">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="4561a-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4561a-157">Dize</span><span class="sxs-lookup"><span data-stu-id="4561a-157">String</span></span>            | <span data-ttu-id="4561a-158">"Oluştur"</span><span class="sxs-lookup"><span data-stu-id="4561a-158">"Make"</span></span>                |
|                   | <span data-ttu-id="4561a-159">"Marka, Model"</span><span class="sxs-lookup"><span data-stu-id="4561a-159">"Make,Model"</span></span> |

<span data-ttu-id="4561a-160">Bir tek üstbilgi değeri veya bir önbellek yenileme üstbilgi değeri değiştiğinde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="4561a-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="4561a-161">Aşağıdaki örnek değerlere görünmesi `Make` ve `Model`.</span><span class="sxs-lookup"><span data-stu-id="4561a-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="4561a-162">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="4561a-163">farklılık tarafından-yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4561a-163">vary-by-route</span></span>

| <span data-ttu-id="4561a-164">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-164">Attribute Type</span></span>    | <span data-ttu-id="4561a-165">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="4561a-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4561a-166">Dize</span><span class="sxs-lookup"><span data-stu-id="4561a-166">String</span></span>            | <span data-ttu-id="4561a-167">"Oluştur"</span><span class="sxs-lookup"><span data-stu-id="4561a-167">"Make"</span></span>                |
|                   | <span data-ttu-id="4561a-168">"Marka, Model"</span><span class="sxs-lookup"><span data-stu-id="4561a-168">"Make,Model"</span></span> |

<span data-ttu-id="4561a-169">Bir tek üstbilgi değeri veya değişiklik rota veri parametresinin Değer yükleyen bir önbellek yenileme tetiklemek üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="4561a-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="4561a-170">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-170">Example:</span></span>

<span data-ttu-id="4561a-171">*Haline*</span><span class="sxs-lookup"><span data-stu-id="4561a-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="4561a-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4561a-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="4561a-173">farklılık-tarafından-tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="4561a-173">vary-by-cookie</span></span>

| <span data-ttu-id="4561a-174">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-174">Attribute Type</span></span>    | <span data-ttu-id="4561a-175">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="4561a-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4561a-176">Dize</span><span class="sxs-lookup"><span data-stu-id="4561a-176">String</span></span>            | <span data-ttu-id="4561a-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="4561a-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="4561a-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="4561a-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="4561a-179">Bir tek üstbilgi değeri veya bir önbellek yenileme (s) üstbilgi değerleri değiştiğinde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="4561a-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="4561a-180">Aşağıdaki örnek, ASP.NET kimliği ile ilişkili tanımlama bakar.</span><span class="sxs-lookup"><span data-stu-id="4561a-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="4561a-181">Bir kullanıcının kimliği doğrulanır zaman, bir önbellek yenileme tetikler ayarlanacak istek tanımlama bilgisi.</span><span class="sxs-lookup"><span data-stu-id="4561a-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="4561a-182">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="4561a-183">farklı kullanıcı tarafından</span><span class="sxs-lookup"><span data-stu-id="4561a-183">vary-by-user</span></span>

| <span data-ttu-id="4561a-184">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-184">Attribute Type</span></span>    | <span data-ttu-id="4561a-185">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="4561a-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4561a-186">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="4561a-186">Boolean</span></span>             | <span data-ttu-id="4561a-187">"true"</span><span class="sxs-lookup"><span data-stu-id="4561a-187">"true"</span></span>                  |
|                     | <span data-ttu-id="4561a-188">"false" (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="4561a-188">"false" (default)</span></span> |

<span data-ttu-id="4561a-189">Oturum açma kullanıcı (veya içerik asıl) değiştiğinde önbelleği sıfırlamalıdır olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="4561a-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="4561a-190">Geçerli kullanıcı olarak da bilinen istek bağlamı asıl ve Razor görünümünde başvurarak görüntülenebilir `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="4561a-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="4561a-191">Aşağıdaki örnek, geçerli kullanıcının oturum bakar.</span><span class="sxs-lookup"><span data-stu-id="4561a-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="4561a-192">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="4561a-193">Bu öznitelik kullanarak oturum açması ve günlük genişletme döngüsü boyunca Önbellekteki içeriği tutar.</span><span class="sxs-lookup"><span data-stu-id="4561a-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="4561a-194">Kullanırken `vary-by-user="true"`, bir oturum açma ve oturum genişletme eylemi önbellek kimliği doğrulanmış kullanıcı için geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4561a-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="4561a-195">Yeni bir benzersiz tanımlama bilgisi değerini oturum açma üretildiği için önbellek geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="4561a-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="4561a-196">Tanımlama bilgisi mevcut olduğunda veya süresi dolmuş önbelleği için anonim durumu korunur.</span><span class="sxs-lookup"><span data-stu-id="4561a-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="4561a-197">Bu, hiçbir kullanıcı oturum açtıysa, önbellek sürdürüleceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4561a-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="4561a-198">farklılık tarafından</span><span class="sxs-lookup"><span data-stu-id="4561a-198">vary-by</span></span>

| <span data-ttu-id="4561a-199">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-199">Attribute Type</span></span> | <span data-ttu-id="4561a-200">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="4561a-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="4561a-201">Dize</span><span class="sxs-lookup"><span data-stu-id="4561a-201">String</span></span>     |    <span data-ttu-id="4561a-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="4561a-202">"@Model"</span></span>    |

<span data-ttu-id="4561a-203">Hangi verileri önbelleğe, özelleştirme için sağlar.</span><span class="sxs-lookup"><span data-stu-id="4561a-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="4561a-204">Önbellek etiket Yardımcısı içeriğini özniteliğin dize değeri değişiklikleri tarafından başvurulan nesne güncelleştirildiğinde.</span><span class="sxs-lookup"><span data-stu-id="4561a-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="4561a-205">Genellikle dize birleştirme modeli değerlerin bu özniteliğe atanır.</span><span class="sxs-lookup"><span data-stu-id="4561a-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="4561a-206">Etkili bir şekilde birleştirilmiş değerleri için bir güncelleştirme önbelleği geçersiz kılar anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4561a-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="4561a-207">Aşağıdaki örnekte iki rota parametrelerinin tamsayı değerini görünüm SUM'ları oluşturma denetleyici yöntemi varsayar `myParam1` ve `myParam2`ve tek model özelliği döndürür.</span><span class="sxs-lookup"><span data-stu-id="4561a-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="4561a-208">Bu toplamın değiştiğinde önbelleği etiket Yardımcısı içeriğini çizilir ve tekrar önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="4561a-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="4561a-209">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-209">Example:</span></span>

<span data-ttu-id="4561a-210">Eylem:</span><span class="sxs-lookup"><span data-stu-id="4561a-210">Action:</span></span>

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

<span data-ttu-id="4561a-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4561a-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="4561a-212">önceliği</span><span class="sxs-lookup"><span data-stu-id="4561a-212">priority</span></span>

| <span data-ttu-id="4561a-213">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="4561a-213">Attribute Type</span></span>    | <span data-ttu-id="4561a-214">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="4561a-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4561a-215">CacheltemPriority</span><span class="sxs-lookup"><span data-stu-id="4561a-215">CacheItemPriority</span></span>  | <span data-ttu-id="4561a-216">"Yüksek"</span><span class="sxs-lookup"><span data-stu-id="4561a-216">"High"</span></span>                   |
|                    | <span data-ttu-id="4561a-217">"Düşük"</span><span class="sxs-lookup"><span data-stu-id="4561a-217">"Low"</span></span> |
|                    | <span data-ttu-id="4561a-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="4561a-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="4561a-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="4561a-219">"Normal"</span></span> |

<span data-ttu-id="4561a-220">Önbellek çıkarma kılavuzu için yerleşik önbelleği sağlayıcısı sağlar.</span><span class="sxs-lookup"><span data-stu-id="4561a-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="4561a-221">Web sunucusu çıkarırsınız `Low` bellek baskısı altında olduğunda girişleri ilk önbelleğe.</span><span class="sxs-lookup"><span data-stu-id="4561a-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="4561a-222">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4561a-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="4561a-223">`priority` Özniteliği önbellek bekletme belirli bir düzeyde garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="4561a-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="4561a-224">`CacheItemPriority` yalnızca bir öneridir.</span><span class="sxs-lookup"><span data-stu-id="4561a-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="4561a-225">Bu öznitelik ayarını `NeverRemove` önbelleği her zaman korunması garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="4561a-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="4561a-226">Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4561a-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="4561a-227">Önbellek etiket Yardımcısı bağlı [bellek önbellek hizmeti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="4561a-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="4561a-228">Değil eklenmişse önbellek etiket Yardımcısı hizmet ekler.</span><span class="sxs-lookup"><span data-stu-id="4561a-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4561a-229">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4561a-229">Additional resources</span></span>

* [<span data-ttu-id="4561a-230">Belleğe yüklenmiş önbellek</span><span class="sxs-lookup"><span data-stu-id="4561a-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="4561a-231">Kimliğe giriş</span><span class="sxs-lookup"><span data-stu-id="4561a-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
