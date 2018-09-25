---
title: Önbellek etiketi Yardımcısı, ASP.NET Core MVC
author: pkellner
description: Önbellek etiketi Yardımcısı'nı kullanmayı öğrenin.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 11754d2858d8f02c7eb9baac8feda9b50ddb3d79
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028161"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="041b1-103">Önbellek etiketi Yardımcısı, ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="041b1-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="041b1-104">Tarafından [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="041b1-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="041b1-105">Önbellek etiketi Yardımcısı iç ASP.NET Core önbelleği sağlayıcısı için içeriği önbelleğe alarak, ASP.NET Core uygulamanızı performansını önemli ölçüde artırmak olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="041b1-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="041b1-106">Razor görüntüleme motorunu varsayılan ayarlar `expires-after` yirmi dakika.</span><span class="sxs-lookup"><span data-stu-id="041b1-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="041b1-107">Aşağıdaki Razor işaretlemesi tarih/saat önbelleğe alır:</span><span class="sxs-lookup"><span data-stu-id="041b1-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="041b1-108">İlk istek içeren sayfasına `CacheTagHelper` geçerli tarih/saat görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="041b1-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="041b1-109">Önbellek süresi (varsayılan 20 dakika) ya da veriler çıkarıldığında tarafından bellek baskısı kadar ek istekler önbelleğe alınan değeri gösterilir.</span><span class="sxs-lookup"><span data-stu-id="041b1-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="041b1-110">Aşağıdaki özniteliklerle önbellek süresi ayarlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="041b1-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="041b1-111">Önbellek etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="041b1-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="041b1-112">Etkin</span><span class="sxs-lookup"><span data-stu-id="041b1-112">enabled</span></span>    


| <span data-ttu-id="041b1-113">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-113">Attribute Type</span></span>    | <span data-ttu-id="041b1-114">Geçerli Değerler</span><span class="sxs-lookup"><span data-stu-id="041b1-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="041b1-115">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="041b1-115">boolean</span></span>           | <span data-ttu-id="041b1-116">"true" (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="041b1-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="041b1-117">"false"</span><span class="sxs-lookup"><span data-stu-id="041b1-117">"false"</span></span>   |


<span data-ttu-id="041b1-118">Önbellek etiketi Yardımcısı tarafından alınmış içeriği önbelleğe alınmış olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="041b1-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="041b1-119">Varsayılan, `true` değeridir.</span><span class="sxs-lookup"><span data-stu-id="041b1-119">The default is `true`.</span></span>  <span data-ttu-id="041b1-120">Varsa kümesine `false` Bu önbellek etiketi Yardımcısı üzerinde işlenen çıkışı önbelleğe alma hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="041b1-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="041b1-121">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="041b1-122">süresi dolmadan açma</span><span class="sxs-lookup"><span data-stu-id="041b1-122">expires-on</span></span> 

| <span data-ttu-id="041b1-123">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-123">Attribute Type</span></span> |           <span data-ttu-id="041b1-124">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="041b1-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="041b1-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="041b1-125">DateTimeOffset</span></span> | <span data-ttu-id="041b1-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="041b1-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="041b1-127">Mutlak sona erme tarihini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="041b1-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="041b1-128">Aşağıdaki örnek, 17:02:00 29 Ocak 2025 üzerinde kadar önbellek etiketi Yardımcısı içeriğini önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="041b1-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="041b1-129">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="041b1-130">süresi dolduktan sonra</span><span class="sxs-lookup"><span data-stu-id="041b1-130">expires-after</span></span>

| <span data-ttu-id="041b1-131">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-131">Attribute Type</span></span> |        <span data-ttu-id="041b1-132">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="041b1-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="041b1-133">Zaman aralığı</span><span class="sxs-lookup"><span data-stu-id="041b1-133">TimeSpan</span></span>    | <span data-ttu-id="041b1-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="041b1-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="041b1-135">İçeriği önbelleğe almak için ilk isteği zamanından sürenin uzunluğunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="041b1-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="041b1-136">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="041b1-137">süresi dolmadan kayan</span><span class="sxs-lookup"><span data-stu-id="041b1-137">expires-sliding</span></span>

| <span data-ttu-id="041b1-138">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-138">Attribute Type</span></span> |        <span data-ttu-id="041b1-139">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="041b1-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="041b1-140">Zaman aralığı</span><span class="sxs-lookup"><span data-stu-id="041b1-140">TimeSpan</span></span>    | <span data-ttu-id="041b1-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="041b1-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="041b1-142">Değil erişilen, önbellek girişi çıkarılacak süreyi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="041b1-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="041b1-143">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="041b1-144">Vary-tarafından-üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="041b1-144">vary-by-header</span></span>

| <span data-ttu-id="041b1-145">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-145">Attribute Type</span></span>    | <span data-ttu-id="041b1-146">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="041b1-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b1-147">Dize</span><span class="sxs-lookup"><span data-stu-id="041b1-147">String</span></span>            | <span data-ttu-id="041b1-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="041b1-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="041b1-149">"User-Agent, content-encoding"</span><span class="sxs-lookup"><span data-stu-id="041b1-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="041b1-150">Tek bir üst bilgi değeri ya da bunlar değiştirdiğinizde, önbellek yenileme tetiklemek üstbilgi değerlerini virgülle ayrılmış bir listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="041b1-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="041b1-151">Aşağıdaki örnekte üst bilgi değeri izler `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="041b1-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="041b1-152">Örnek içeriğini önbelleğe alır her farklı `User-Agent` web sunucusuna sunulur.</span><span class="sxs-lookup"><span data-stu-id="041b1-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="041b1-153">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="041b1-154">farklı-tarafından-sorgu</span><span class="sxs-lookup"><span data-stu-id="041b1-154">vary-by-query</span></span>

| <span data-ttu-id="041b1-155">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-155">Attribute Type</span></span>    | <span data-ttu-id="041b1-156">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="041b1-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b1-157">Dize</span><span class="sxs-lookup"><span data-stu-id="041b1-157">String</span></span>            | <span data-ttu-id="041b1-158">"Oluştur"</span><span class="sxs-lookup"><span data-stu-id="041b1-158">"Make"</span></span>                |
|                   | <span data-ttu-id="041b1-159">"Marka, Model"</span><span class="sxs-lookup"><span data-stu-id="041b1-159">"Make,Model"</span></span> |

<span data-ttu-id="041b1-160">Tek bir üst bilgi değeri veya üst bilgi değeri değiştiğinde bir önbellek yenileme tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="041b1-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="041b1-161">Aşağıdaki örnekte değerlerinde görünür `Make` ve `Model`.</span><span class="sxs-lookup"><span data-stu-id="041b1-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="041b1-162">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="041b1-163">farklı-tarafından-route</span><span class="sxs-lookup"><span data-stu-id="041b1-163">vary-by-route</span></span>

| <span data-ttu-id="041b1-164">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-164">Attribute Type</span></span>    | <span data-ttu-id="041b1-165">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="041b1-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b1-166">Dize</span><span class="sxs-lookup"><span data-stu-id="041b1-166">String</span></span>            | <span data-ttu-id="041b1-167">"Oluştur"</span><span class="sxs-lookup"><span data-stu-id="041b1-167">"Make"</span></span>                |
|                   | <span data-ttu-id="041b1-168">"Marka, Model"</span><span class="sxs-lookup"><span data-stu-id="041b1-168">"Make,Model"</span></span> |

<span data-ttu-id="041b1-169">Rota veri parametresinin Değer değişiklik olduğunda, önbellek yenileme tetiklemek üstbilgi değerlerini virgülle ayrılmış bir listesi veya bir tek bir üstbilgi değerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="041b1-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="041b1-170">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-170">Example:</span></span>

<span data-ttu-id="041b1-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="041b1-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="041b1-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="041b1-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="041b1-173">farklı-tarafından-tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="041b1-173">vary-by-cookie</span></span>

| <span data-ttu-id="041b1-174">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-174">Attribute Type</span></span>    | <span data-ttu-id="041b1-175">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="041b1-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b1-176">Dize</span><span class="sxs-lookup"><span data-stu-id="041b1-176">String</span></span>            | <span data-ttu-id="041b1-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="041b1-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="041b1-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="041b1-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="041b1-179">Bir tek bir üstbilgi değerini veya bir önbellek yenileme (s) başlık değerleri değiştiğinde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="041b1-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="041b1-180">Aşağıdaki örnek, ASP.NET Core kimliği ile ilişkili tanımlama bakar.</span><span class="sxs-lookup"><span data-stu-id="041b1-180">The following example looks at the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="041b1-181">Ne zaman bir kullanıcının kimliği doğrulanır ve bu önbellek yenileme tetikler ayarlamak için istek tanımlama bilgisi.</span><span class="sxs-lookup"><span data-stu-id="041b1-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="041b1-182">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="041b1-183">farklı kullanıcı tarafından</span><span class="sxs-lookup"><span data-stu-id="041b1-183">vary-by-user</span></span>

| <span data-ttu-id="041b1-184">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-184">Attribute Type</span></span>    | <span data-ttu-id="041b1-185">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="041b1-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b1-186">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="041b1-186">Boolean</span></span>             | <span data-ttu-id="041b1-187">"true"</span><span class="sxs-lookup"><span data-stu-id="041b1-187">"true"</span></span>                  |
|                     | <span data-ttu-id="041b1-188">"false" (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="041b1-188">"false" (default)</span></span> |

<span data-ttu-id="041b1-189">Oturum açmış kullanıcı (veya bağlam sorumlusu) değiştiğinde önbelleği sıfırlamalısınız olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="041b1-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="041b1-190">Geçerli kullanıcı olarak da bilinen istek bağlamı sorumlusu ve bir Razor Görünümü'nde başvurarak görüntülenebilir `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="041b1-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="041b1-191">Aşağıdaki örnek, geçerli kullanıcı oturum bakar.</span><span class="sxs-lookup"><span data-stu-id="041b1-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="041b1-192">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="041b1-193">Bu özniteliği kullanarak içerikleri bir oturum açma ve günlük genişletme döngüsü boyunca önbellekte tutar.</span><span class="sxs-lookup"><span data-stu-id="041b1-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="041b1-194">Kullanırken `vary-by-user="true"`, önbellek kimliği doğrulanmış kullanıcı için bir oturum açma ve günlük genişletme eylemi geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="041b1-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="041b1-195">Oturum açma için yeni bir benzersiz tanımlama bilgisi değeri güvenle önbellek geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="041b1-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="041b1-196">Tanımlama bilgisi varsa ya da süresi dolmuş önbellek için anonim durumu korunur.</span><span class="sxs-lookup"><span data-stu-id="041b1-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="041b1-197">Bu, hiçbir kullanıcı oturum açtıysa, önbellekte tutulan anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="041b1-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="041b1-198">değişiklik tarafından</span><span class="sxs-lookup"><span data-stu-id="041b1-198">vary-by</span></span>

| <span data-ttu-id="041b1-199">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-199">Attribute Type</span></span> | <span data-ttu-id="041b1-200">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="041b1-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="041b1-201">Dize</span><span class="sxs-lookup"><span data-stu-id="041b1-201">String</span></span>     |    <span data-ttu-id="041b1-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="041b1-202">"@Model"</span></span>    |

<span data-ttu-id="041b1-203">Hangi veriler önbelleğe, özelleştirme için sağlar.</span><span class="sxs-lookup"><span data-stu-id="041b1-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="041b1-204">Önbellek etiketi Yardımcısı içeriğini özniteliğin dize değeri değiştiğinde tarafından başvurulan nesne güncelleştirildiğinde.</span><span class="sxs-lookup"><span data-stu-id="041b1-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="041b1-205">Genellikle model değerlerinin dize birleştirme bu özniteliğe atanır.</span><span class="sxs-lookup"><span data-stu-id="041b1-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="041b1-206">Etkili bir şekilde önbelleğe nesnelerindeki değerleri herhangi bir güncelleştirme geçersiz kılar anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="041b1-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="041b1-207">Aşağıdaki örnek iki yol parametreleri, tamsayı değeri görünümü SUM'ları oluşturma denetleyici yöntemi varsayar `myParam1` ve `myParam2`ve tek bir model özelliği döndürür.</span><span class="sxs-lookup"><span data-stu-id="041b1-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="041b1-208">Bu toplam değiştiğinde, önbellek etiketi Yardımcısı içeriğini oluşturulur ve tekrar önbelleğe alınmış.</span><span class="sxs-lookup"><span data-stu-id="041b1-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="041b1-209">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-209">Example:</span></span>

<span data-ttu-id="041b1-210">Eylem:</span><span class="sxs-lookup"><span data-stu-id="041b1-210">Action:</span></span>

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

<span data-ttu-id="041b1-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="041b1-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="041b1-212">önceliği</span><span class="sxs-lookup"><span data-stu-id="041b1-212">priority</span></span>

| <span data-ttu-id="041b1-213">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="041b1-213">Attribute Type</span></span>    | <span data-ttu-id="041b1-214">Örnek değerler</span><span class="sxs-lookup"><span data-stu-id="041b1-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="041b1-215">CacheltemPriority</span><span class="sxs-lookup"><span data-stu-id="041b1-215">CacheItemPriority</span></span>  | <span data-ttu-id="041b1-216">"Yüksek"</span><span class="sxs-lookup"><span data-stu-id="041b1-216">"High"</span></span>                   |
|                    | <span data-ttu-id="041b1-217">"Düşük"</span><span class="sxs-lookup"><span data-stu-id="041b1-217">"Low"</span></span> |
|                    | <span data-ttu-id="041b1-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="041b1-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="041b1-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="041b1-219">"Normal"</span></span> |

<span data-ttu-id="041b1-220">Yerleşik önbelleği sağlayıcısı için önbellek çıkarma rehberlik sağlar.</span><span class="sxs-lookup"><span data-stu-id="041b1-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="041b1-221">Web sunucusu çıkarırsınız `Low` bellek baskısı altında olduğunda girişleri ilk önbellek.</span><span class="sxs-lookup"><span data-stu-id="041b1-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="041b1-222">Örnek:</span><span class="sxs-lookup"><span data-stu-id="041b1-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="041b1-223">`priority` Özniteliği, belirli bir önbellek bekletme düzeyini garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="041b1-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="041b1-224">`CacheItemPriority` yalnızca bir öneridir.</span><span class="sxs-lookup"><span data-stu-id="041b1-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="041b1-225">Bu öznitelik ayarını `NeverRemove` önbelleği her zaman korunması garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="041b1-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="041b1-226">Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="041b1-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="041b1-227">Önbellek etiketi Yardımcısı bağlıdır [bellek önbellek hizmeti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="041b1-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="041b1-228">Önbellek etiketi Yardımcısı henüz eklenmemiş bir hizmet ekler.</span><span class="sxs-lookup"><span data-stu-id="041b1-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="041b1-229">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="041b1-229">Additional resources</span></span>

* [<span data-ttu-id="041b1-230">Belleğe yüklenmiş önbellek</span><span class="sxs-lookup"><span data-stu-id="041b1-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="041b1-231">Kimliğe giriş</span><span class="sxs-lookup"><span data-stu-id="041b1-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
