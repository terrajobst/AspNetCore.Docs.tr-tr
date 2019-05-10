---
title: Önbellek etiketi Yardımcısı, ASP.NET Core MVC
author: pkellner
description: Önbellek etiketi Yardımcısı'nı kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: fb69584f6e9d4756e175bbd6f3deb1f413b80fc5
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902630"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="0e25c-103">Önbellek etiketi Yardımcısı, ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="0e25c-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="0e25c-104">Tarafından [Peter Kellner](http://peterkellner.net) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0e25c-104">By [Peter Kellner](http://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span> 

<span data-ttu-id="0e25c-105">Önbellek etiketi Yardımcısı iç ASP.NET Core önbelleği sağlayıcısı için içeriği önbelleğe alarak ASP.NET Core uygulamanızı performansını olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="0e25c-105">The Cache Tag Helper provides the ability to improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="0e25c-106">Etiket Yardımcıları genel bakış için bkz. <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="0e25c-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="0e25c-107">Aşağıdaki Razor biçimlendirme, geçerli tarihi önbelleğe alır:</span><span class="sxs-lookup"><span data-stu-id="0e25c-107">The following Razor markup caches the current date:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="0e25c-108">Etiket Yardımcısını içeren sayfasında ilk isteği, geçerli tarihi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0e25c-108">The first request to the page that contains the Tag Helper displays the current date.</span></span> <span data-ttu-id="0e25c-109">(Varsayılan 20 dakika) önbelleğe süresi dolana kadar veya önbellekten önbelleğe alınan tarih veriler çıkarıldığında kadar ek isteklerin önbelleğe alınan değeri gösterilir.</span><span class="sxs-lookup"><span data-stu-id="0e25c-109">Additional requests show the cached value until the cache expires (default 20 minutes) or until the cached date is evicted from the cache.</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="0e25c-110">Önbellek etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="0e25c-110">Cache Tag Helper Attributes</span></span>

### <a name="enabled"></a><span data-ttu-id="0e25c-111">Etkin</span><span class="sxs-lookup"><span data-stu-id="0e25c-111">enabled</span></span>

| <span data-ttu-id="0e25c-112">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-112">Attribute Type</span></span>  | <span data-ttu-id="0e25c-113">Örnekler</span><span class="sxs-lookup"><span data-stu-id="0e25c-113">Examples</span></span>        | <span data-ttu-id="0e25c-114">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="0e25c-114">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="0e25c-115">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="0e25c-115">Boolean</span></span>         | <span data-ttu-id="0e25c-116">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="0e25c-116">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="0e25c-117">`enabled` Önbellek etiketi Yardımcısı tarafından alınmış içeriği önbelleğe alınmış belirler.</span><span class="sxs-lookup"><span data-stu-id="0e25c-117">`enabled` determines if the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="0e25c-118">Varsayılan, `true` değeridir.</span><span class="sxs-lookup"><span data-stu-id="0e25c-118">The default is `true`.</span></span> <span data-ttu-id="0e25c-119">Varsa kümesine `false`, işlenmiş çıktı **değil** önbelleğe alınmış.</span><span class="sxs-lookup"><span data-stu-id="0e25c-119">If set to `false`, the rendered output is **not** cached.</span></span>

<span data-ttu-id="0e25c-120">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0e25c-120">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a><span data-ttu-id="0e25c-121">süresi dolmadan açma</span><span class="sxs-lookup"><span data-stu-id="0e25c-121">expires-on</span></span>

| <span data-ttu-id="0e25c-122">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-122">Attribute Type</span></span>   | <span data-ttu-id="0e25c-123">Örnek</span><span class="sxs-lookup"><span data-stu-id="0e25c-123">Example</span></span>                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

<span data-ttu-id="0e25c-124">`expires-on` önbelleğe alınan öğe için bir mutlak sona erme tarihi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0e25c-124">`expires-on` sets an absolute expiration date for the cached item.</span></span>

<span data-ttu-id="0e25c-125">Aşağıdaki örnek, 17:02:00 29 Ocak 2025 üzerinde kadar önbellek etiketi Yardımcısı içeriğini önbelleğe alır:</span><span class="sxs-lookup"><span data-stu-id="0e25c-125">The following example caches the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a><span data-ttu-id="0e25c-126">süresi dolduktan sonra</span><span class="sxs-lookup"><span data-stu-id="0e25c-126">expires-after</span></span>

| <span data-ttu-id="0e25c-127">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-127">Attribute Type</span></span> | <span data-ttu-id="0e25c-128">Örnek</span><span class="sxs-lookup"><span data-stu-id="0e25c-128">Example</span></span>                      | <span data-ttu-id="0e25c-129">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="0e25c-129">Default</span></span>    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | <span data-ttu-id="0e25c-130">20 dakika</span><span class="sxs-lookup"><span data-stu-id="0e25c-130">20 minutes</span></span> |

<span data-ttu-id="0e25c-131">`expires-after` İçeriği önbelleğe almak için ilk isteği zamanından sürenin uzunluğunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0e25c-131">`expires-after` sets the length of time from the first request time to cache the contents.</span></span>

<span data-ttu-id="0e25c-132">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0e25c-132">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="0e25c-133">Razor görüntüleme motorunu varsayılan ayarlar `expires-after` yirmi dakika değeri.</span><span class="sxs-lookup"><span data-stu-id="0e25c-133">The Razor View Engine sets the default `expires-after` value to twenty minutes.</span></span>

### <a name="expires-sliding"></a><span data-ttu-id="0e25c-134">süresi dolmadan kayan</span><span class="sxs-lookup"><span data-stu-id="0e25c-134">expires-sliding</span></span>

| <span data-ttu-id="0e25c-135">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-135">Attribute Type</span></span> | <span data-ttu-id="0e25c-136">Örnek</span><span class="sxs-lookup"><span data-stu-id="0e25c-136">Example</span></span>                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

<span data-ttu-id="0e25c-137">Değerini erişilmeyen, önbellek girişi çıkarılacak süreyi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0e25c-137">Sets the time that a cache entry should be evicted if its value hasn't been accessed.</span></span>

<span data-ttu-id="0e25c-138">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0e25c-138">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a><span data-ttu-id="0e25c-139">Vary-tarafından-üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="0e25c-139">vary-by-header</span></span>

| <span data-ttu-id="0e25c-140">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-140">Attribute Type</span></span> | <span data-ttu-id="0e25c-141">Örnekler</span><span class="sxs-lookup"><span data-stu-id="0e25c-141">Examples</span></span>                                    |
| -------------- | ------------------------------------------- |
| <span data-ttu-id="0e25c-142">Dize</span><span class="sxs-lookup"><span data-stu-id="0e25c-142">String</span></span>         | <span data-ttu-id="0e25c-143">`User-Agent`, `User-Agent,content-encoding`</span><span class="sxs-lookup"><span data-stu-id="0e25c-143">`User-Agent`, `User-Agent,content-encoding`</span></span> |

<span data-ttu-id="0e25c-144">`vary-by-header` Bunlar değiştirdiğinizde, önbellek yenileme tetiklemek üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0e25c-144">`vary-by-header` accepts a comma-delimited list of header values that trigger a cache refresh when they change.</span></span>

<span data-ttu-id="0e25c-145">Aşağıdaki örnekte üst bilgi değeri izler `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="0e25c-145">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="0e25c-146">Bu örnek için içerikleri önbelleğe alan her farklı `User-Agent` web sunucusuna sunulur:</span><span class="sxs-lookup"><span data-stu-id="0e25c-146">The example caches the content for every different `User-Agent` presented to the web server:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a><span data-ttu-id="0e25c-147">farklı-tarafından-sorgu</span><span class="sxs-lookup"><span data-stu-id="0e25c-147">vary-by-query</span></span>

| <span data-ttu-id="0e25c-148">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-148">Attribute Type</span></span> | <span data-ttu-id="0e25c-149">Örnekler</span><span class="sxs-lookup"><span data-stu-id="0e25c-149">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="0e25c-150">Dize</span><span class="sxs-lookup"><span data-stu-id="0e25c-150">String</span></span>         | <span data-ttu-id="0e25c-151">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="0e25c-151">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="0e25c-152">`vary-by-query` bir virgülle ayrılmış listesini kabul eder <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> bir sorgu dizesinde (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>), tetikleme önbellek yenileme herhangi bir değerini listelenen anahtar değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="0e25c-152">`vary-by-query` accepts a comma-delimited list of <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> in a query string (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) that trigger a cache refresh when the value of any listed key changes.</span></span>

<span data-ttu-id="0e25c-153">Aşağıdaki örnek değerleri izler `Make` ve `Model`.</span><span class="sxs-lookup"><span data-stu-id="0e25c-153">The following example monitors the values of `Make` and `Model`.</span></span> <span data-ttu-id="0e25c-154">Bu örnek için içerikleri önbelleğe alan her farklı `Make` ve `Model` web sunucusuna sunulur:</span><span class="sxs-lookup"><span data-stu-id="0e25c-154">The example caches the content for every different `Make` and `Model` presented to the web server:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a><span data-ttu-id="0e25c-155">farklı-tarafından-route</span><span class="sxs-lookup"><span data-stu-id="0e25c-155">vary-by-route</span></span>

| <span data-ttu-id="0e25c-156">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-156">Attribute Type</span></span> | <span data-ttu-id="0e25c-157">Örnekler</span><span class="sxs-lookup"><span data-stu-id="0e25c-157">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="0e25c-158">Dize</span><span class="sxs-lookup"><span data-stu-id="0e25c-158">String</span></span>         | <span data-ttu-id="0e25c-159">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="0e25c-159">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="0e25c-160">`vary-by-route` Rota veri parametre değeri değiştiğinde bir önbellek yenileme tetikleyen rota parametre adlarının virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0e25c-160">`vary-by-route` accepts a comma-delimited list of route parameter names that trigger a cache refresh when the route data parameter value changes.</span></span>

<span data-ttu-id="0e25c-161">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0e25c-161">Example:</span></span>

<span data-ttu-id="0e25c-162">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0e25c-162">*Startup.cs*:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="0e25c-163">*Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0e25c-163">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a><span data-ttu-id="0e25c-164">farklı-tarafından-tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="0e25c-164">vary-by-cookie</span></span>

| <span data-ttu-id="0e25c-165">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-165">Attribute Type</span></span> | <span data-ttu-id="0e25c-166">Örnekler</span><span class="sxs-lookup"><span data-stu-id="0e25c-166">Examples</span></span>                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| <span data-ttu-id="0e25c-167">Dize</span><span class="sxs-lookup"><span data-stu-id="0e25c-167">String</span></span>         | <span data-ttu-id="0e25c-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span><span class="sxs-lookup"><span data-stu-id="0e25c-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span></span> |

<span data-ttu-id="0e25c-169">`vary-by-cookie` tanımlama bilgisi değerleri değiştiğinde önbelleği yenileme tetiklemek tanımlama bilgisi adlarının virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0e25c-169">`vary-by-cookie` accepts a comma-delimited list of cookie names that trigger a cache refresh when the cookie values change.</span></span>

<span data-ttu-id="0e25c-170">Aşağıdaki örnek, ASP.NET Core kimliği ile ilişkili tanımlama izler.</span><span class="sxs-lookup"><span data-stu-id="0e25c-170">The following example monitors the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="0e25c-171">Bir kullanıcının kimliği doğrulandığında, kimlik tanımlama değişikliği bir önbellek yenileme tetikleyen:</span><span class="sxs-lookup"><span data-stu-id="0e25c-171">When a user is authenticated, a change in the Identity cookie triggers a cache refresh:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a><span data-ttu-id="0e25c-172">farklı kullanıcı tarafından</span><span class="sxs-lookup"><span data-stu-id="0e25c-172">vary-by-user</span></span>

| <span data-ttu-id="0e25c-173">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-173">Attribute Type</span></span>  | <span data-ttu-id="0e25c-174">Örnekler</span><span class="sxs-lookup"><span data-stu-id="0e25c-174">Examples</span></span>        | <span data-ttu-id="0e25c-175">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="0e25c-175">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="0e25c-176">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="0e25c-176">Boolean</span></span>         | <span data-ttu-id="0e25c-177">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="0e25c-177">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="0e25c-178">`vary-by-user` oturum açmış kullanıcı (veya bağlam sorumlusu) değiştiğinde önbelleği sıfırlar olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="0e25c-178">`vary-by-user` specifies whether or not the cache resets when the signed-in user (or Context Principal) changes.</span></span> <span data-ttu-id="0e25c-179">Geçerli kullanıcı olarak da bilinen istek bağlamı sorumlusu ve bir Razor Görünümü'nde başvurarak görüntülenebilir `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="0e25c-179">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="0e25c-180">Aşağıdaki örnek, geçerli bir önbellek yenileme tetiklemek için kullanıcı oturum izler:</span><span class="sxs-lookup"><span data-stu-id="0e25c-180">The following example monitors the current logged in user to trigger a cache refresh:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="0e25c-181">Bu özniteliği kullanarak içerikleri önbellekte aracılığıyla bir oturum açma ve oturum kapatma döngüsü tutar.</span><span class="sxs-lookup"><span data-stu-id="0e25c-181">Using this attribute maintains the contents in cache through a sign-in and sign-out cycle.</span></span> <span data-ttu-id="0e25c-182">Değer ayarlandığında `true`, önbellek kimliği doğrulanmış kullanıcı için bir kimlik doğrulama döngüsü geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="0e25c-182">When the value is set to `true`, an authentication cycle invalidates the cache for the authenticated user.</span></span> <span data-ttu-id="0e25c-183">Yeni bir benzersiz tanımlama bilgisi değeri, bir kullanıcının kimliği doğrulandığında oluşturulmuş olduğu için önbellek geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="0e25c-183">The cache is invalidated because a new unique cookie value is generated when a user is authenticated.</span></span> <span data-ttu-id="0e25c-184">Önbellek anonim durumu için tanımlama bilgisi mevcut olduğunda veya tanımlama bilgisinin doldu korunur.</span><span class="sxs-lookup"><span data-stu-id="0e25c-184">Cache is maintained for the anonymous state when no cookie is present or the cookie has expired.</span></span> <span data-ttu-id="0e25c-185">Kullanıcı, **değil** kimliği doğrulanmış ve önbellek korunur.</span><span class="sxs-lookup"><span data-stu-id="0e25c-185">If the user is **not** authenticated, the cache is maintained.</span></span>

### <a name="vary-by"></a><span data-ttu-id="0e25c-186">değişiklik tarafından</span><span class="sxs-lookup"><span data-stu-id="0e25c-186">vary-by</span></span>

| <span data-ttu-id="0e25c-187">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-187">Attribute Type</span></span> | <span data-ttu-id="0e25c-188">Örnek</span><span class="sxs-lookup"><span data-stu-id="0e25c-188">Example</span></span>  |
| -------------- | -------- |
| <span data-ttu-id="0e25c-189">Dize</span><span class="sxs-lookup"><span data-stu-id="0e25c-189">String</span></span>         | `@Model` |

<span data-ttu-id="0e25c-190">`vary-by` hangi verilerin önbelleğe alınmış bir özelleştirme için sağlar.</span><span class="sxs-lookup"><span data-stu-id="0e25c-190">`vary-by` allows for customization of what data is cached.</span></span> <span data-ttu-id="0e25c-191">Önbellek etiketi Yardımcısı içeriğini özniteliğin dize değeri değiştiğinde tarafından başvurulan nesne güncelleştirildiğinde.</span><span class="sxs-lookup"><span data-stu-id="0e25c-191">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="0e25c-192">Genellikle, model değerlerinin dize birleştirme bu özniteliğe atanır.</span><span class="sxs-lookup"><span data-stu-id="0e25c-192">Often, a string-concatenation of model values are assigned to this attribute.</span></span> <span data-ttu-id="0e25c-193">Etkili bir şekilde, burada önbellek nesnelerindeki değerleri herhangi bir güncelleştirme geçersiz kılar bir senaryoda sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="0e25c-193">Effectively, this results in a scenario where an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="0e25c-194">Aşağıdaki örnek iki yol parametreleri, tamsayı değeri görünümü SUM'ları oluşturma denetleyici yöntemi varsayar `myParam1` ve `myParam2`ve tek bir model özelliği olarak toplamını döndürür.</span><span class="sxs-lookup"><span data-stu-id="0e25c-194">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns the sum as the single model property.</span></span> <span data-ttu-id="0e25c-195">Bu toplam değiştiğinde, önbellek etiketi Yardımcısı içeriğini oluşturulur ve tekrar önbelleğe alınmış.</span><span class="sxs-lookup"><span data-stu-id="0e25c-195">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="0e25c-196">Eylem:</span><span class="sxs-lookup"><span data-stu-id="0e25c-196">Action:</span></span>

```csharp
public IActionResult Index(string myParam1, string myParam2, string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="0e25c-197">*Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0e25c-197">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a><span data-ttu-id="0e25c-198">öncelik</span><span class="sxs-lookup"><span data-stu-id="0e25c-198">priority</span></span>

| <span data-ttu-id="0e25c-199">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="0e25c-199">Attribute Type</span></span>      | <span data-ttu-id="0e25c-200">Örnekler</span><span class="sxs-lookup"><span data-stu-id="0e25c-200">Examples</span></span>                               | <span data-ttu-id="0e25c-201">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="0e25c-201">Default</span></span>  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | <span data-ttu-id="0e25c-202">`High`, `Low`, `NeverRemove`, `Normal`</span><span class="sxs-lookup"><span data-stu-id="0e25c-202">`High`, `Low`, `NeverRemove`, `Normal`</span></span> | `Normal` |

<span data-ttu-id="0e25c-203">`priority` Yerleşik önbelleği sağlayıcısı için önbellek çıkarma rehberlik sağlar.</span><span class="sxs-lookup"><span data-stu-id="0e25c-203">`priority` provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="0e25c-204">Web sunucusu çıkarır `Low` bellek baskısı altında olduğunda girişleri ilk önbellek.</span><span class="sxs-lookup"><span data-stu-id="0e25c-204">The web server evicts `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="0e25c-205">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0e25c-205">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="0e25c-206">`priority` Özniteliği, belirli bir önbellek bekletme düzeyini garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="0e25c-206">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="0e25c-207">`CacheItemPriority` yalnızca bir öneridir.</span><span class="sxs-lookup"><span data-stu-id="0e25c-207">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="0e25c-208">Bu öznitelik ayarını `NeverRemove` önbelleğe alınmış öğeleri her zaman korunur garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="0e25c-208">Setting this attribute to `NeverRemove` doesn't guarantee that cached items are always retained.</span></span> <span data-ttu-id="0e25c-209">Konular, bkz: [ek kaynaklar](#additional-resources) bölümünde daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0e25c-209">See the topics in the [Additional Resources](#additional-resources) section for more information.</span></span>

<span data-ttu-id="0e25c-210">Önbellek etiketi Yardımcısı bağlıdır [bellek önbellek hizmeti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="0e25c-210">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="0e25c-211">Önbellek etiketi Yardımcısı eklenmemiş olan hizmet ekler.</span><span class="sxs-lookup"><span data-stu-id="0e25c-211">The Cache Tag Helper adds the service if it hasn't been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0e25c-212">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0e25c-212">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
