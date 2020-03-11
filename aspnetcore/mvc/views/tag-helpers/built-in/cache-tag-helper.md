---
title: ASP.NET Core MVC 'de önbellek etiketi Yardımcısı
author: pkellner
description: Önbellek etiketi yardımcısını kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: db9e1a968588410f11e5f137dfdd4542df505ebc
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662738"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="75b89-103">ASP.NET Core MVC 'de önbellek etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="75b89-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="75b89-104">By [Peter Kellner](https://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="75b89-104">By [Peter Kellner](https://peterkellner.net)</span></span>

<span data-ttu-id="75b89-105">Önbellek etiketi Yardımcısı, içeriğini dahili ASP.NET Core önbellek sağlayıcısına önbelleğe alarak ASP.NET Core uygulamanızın performansını iyileştirebilme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="75b89-105">The Cache Tag Helper provides the ability to improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="75b89-106">Etiket Yardımcıları hakkında genel bilgi için bkz. <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="75b89-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="75b89-107">Şu Razor işaretlemesi geçerli tarihi önbelleğe alır:</span><span class="sxs-lookup"><span data-stu-id="75b89-107">The following Razor markup caches the current date:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="75b89-108">Etiket Yardımcısını içeren sayfanın ilk isteği geçerli tarihi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="75b89-108">The first request to the page that contains the Tag Helper displays the current date.</span></span> <span data-ttu-id="75b89-109">Önbellek süresi dolana kadar veya önbelleğe alınmış tarih önbellekten çıkarılana kadar ek istekler önbelleğe alınmış değeri gösterir.</span><span class="sxs-lookup"><span data-stu-id="75b89-109">Additional requests show the cached value until the cache expires (default 20 minutes) or until the cached date is evicted from the cache.</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="75b89-110">Önbellek etiketi yardımcı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="75b89-110">Cache Tag Helper Attributes</span></span>

### <a name="enabled"></a><span data-ttu-id="75b89-111">enabled</span><span class="sxs-lookup"><span data-stu-id="75b89-111">enabled</span></span>

| <span data-ttu-id="75b89-112">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-112">Attribute Type</span></span>  | <span data-ttu-id="75b89-113">Örnekler</span><span class="sxs-lookup"><span data-stu-id="75b89-113">Examples</span></span>        | <span data-ttu-id="75b89-114">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="75b89-114">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="75b89-115">Boole</span><span class="sxs-lookup"><span data-stu-id="75b89-115">Boolean</span></span>         | <span data-ttu-id="75b89-116">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="75b89-116">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="75b89-117">`enabled`, önbellek etiketi Yardımcısı tarafından eklenen içeriğin önbelleğe alınıp alınmayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="75b89-117">`enabled` determines if the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="75b89-118">Varsayılan değer: `true`.</span><span class="sxs-lookup"><span data-stu-id="75b89-118">The default is `true`.</span></span> <span data-ttu-id="75b89-119">`false`olarak ayarlanırsa, işlenen çıktı önbelleğe **alınmaz** .</span><span class="sxs-lookup"><span data-stu-id="75b89-119">If set to `false`, the rendered output is **not** cached.</span></span>

<span data-ttu-id="75b89-120">Örnek:</span><span class="sxs-lookup"><span data-stu-id="75b89-120">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a><span data-ttu-id="75b89-121">süre sonu-açık</span><span class="sxs-lookup"><span data-stu-id="75b89-121">expires-on</span></span>

| <span data-ttu-id="75b89-122">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-122">Attribute Type</span></span>   | <span data-ttu-id="75b89-123">Örnek</span><span class="sxs-lookup"><span data-stu-id="75b89-123">Example</span></span>                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

<span data-ttu-id="75b89-124">`expires-on`, önbelleğe alınmış öğe için mutlak bir sona erme tarihi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="75b89-124">`expires-on` sets an absolute expiration date for the cached item.</span></span>

<span data-ttu-id="75b89-125">Aşağıdaki örnek, 29 Ocak 2025 ' de 5:02 PM 'e kadar önbellek etiketi Yardımcısı 'nın içeriğini önbelleğe alır:</span><span class="sxs-lookup"><span data-stu-id="75b89-125">The following example caches the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a><span data-ttu-id="75b89-126">süre sonu-sonra</span><span class="sxs-lookup"><span data-stu-id="75b89-126">expires-after</span></span>

| <span data-ttu-id="75b89-127">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-127">Attribute Type</span></span> | <span data-ttu-id="75b89-128">Örnek</span><span class="sxs-lookup"><span data-stu-id="75b89-128">Example</span></span>                      | <span data-ttu-id="75b89-129">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="75b89-129">Default</span></span>    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | <span data-ttu-id="75b89-130">20 dakika</span><span class="sxs-lookup"><span data-stu-id="75b89-130">20 minutes</span></span> |

<span data-ttu-id="75b89-131">`expires-after`, içeriği önbelleğe almak için ilk istek zamanından itibaren geçen süreyi belirler.</span><span class="sxs-lookup"><span data-stu-id="75b89-131">`expires-after` sets the length of time from the first request time to cache the contents.</span></span>

<span data-ttu-id="75b89-132">Örnek:</span><span class="sxs-lookup"><span data-stu-id="75b89-132">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="75b89-133">Razor Görünüm altyapısı varsayılan `expires-after` değerini yirmi dakika olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="75b89-133">The Razor View Engine sets the default `expires-after` value to twenty minutes.</span></span>

### <a name="expires-sliding"></a><span data-ttu-id="75b89-134">süre sonu-kayan</span><span class="sxs-lookup"><span data-stu-id="75b89-134">expires-sliding</span></span>

| <span data-ttu-id="75b89-135">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-135">Attribute Type</span></span> | <span data-ttu-id="75b89-136">Örnek</span><span class="sxs-lookup"><span data-stu-id="75b89-136">Example</span></span>                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

<span data-ttu-id="75b89-137">Değerine erişilmediyse önbellek girişinin çıkarılme süresini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="75b89-137">Sets the time that a cache entry should be evicted if its value hasn't been accessed.</span></span>

<span data-ttu-id="75b89-138">Örnek:</span><span class="sxs-lookup"><span data-stu-id="75b89-138">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a><span data-ttu-id="75b89-139">üst bilgiye göre değişiklik</span><span class="sxs-lookup"><span data-stu-id="75b89-139">vary-by-header</span></span>

| <span data-ttu-id="75b89-140">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-140">Attribute Type</span></span> | <span data-ttu-id="75b89-141">Örnekler</span><span class="sxs-lookup"><span data-stu-id="75b89-141">Examples</span></span>                                    |
| -------------- | ------------------------------------------- |
| <span data-ttu-id="75b89-142">String</span><span class="sxs-lookup"><span data-stu-id="75b89-142">String</span></span>         | <span data-ttu-id="75b89-143">`User-Agent`, `User-Agent,content-encoding`</span><span class="sxs-lookup"><span data-stu-id="75b89-143">`User-Agent`, `User-Agent,content-encoding`</span></span> |

<span data-ttu-id="75b89-144">`vary-by-header`, değiştiğinde önbellek yenilemeyi tetikleyen üst bilgi değerlerinin virgülle ayrılmış bir listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="75b89-144">`vary-by-header` accepts a comma-delimited list of header values that trigger a cache refresh when they change.</span></span>

<span data-ttu-id="75b89-145">Aşağıdaki örnek `User-Agent`üst bilgi değerini izler.</span><span class="sxs-lookup"><span data-stu-id="75b89-145">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="75b89-146">Örnek, Web sunucusuna sunulan her farklı `User-Agent` için içeriği önbelleğe alır:</span><span class="sxs-lookup"><span data-stu-id="75b89-146">The example caches the content for every different `User-Agent` presented to the web server:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a><span data-ttu-id="75b89-147">sorguya göre değişiklik</span><span class="sxs-lookup"><span data-stu-id="75b89-147">vary-by-query</span></span>

| <span data-ttu-id="75b89-148">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-148">Attribute Type</span></span> | <span data-ttu-id="75b89-149">Örnekler</span><span class="sxs-lookup"><span data-stu-id="75b89-149">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="75b89-150">String</span><span class="sxs-lookup"><span data-stu-id="75b89-150">String</span></span>         | <span data-ttu-id="75b89-151">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="75b89-151">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="75b89-152">`vary-by-query`, listelenen herhangi bir anahtarın değeri değiştiğinde önbellek yenilemeyi tetikleyen bir sorgu dizesindeki (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> virgülle ayrılmış bir listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="75b89-152">`vary-by-query` accepts a comma-delimited list of <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> in a query string (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) that trigger a cache refresh when the value of any listed key changes.</span></span>

<span data-ttu-id="75b89-153">Aşağıdaki örnek, `Make` ve `Model`değerlerini izler.</span><span class="sxs-lookup"><span data-stu-id="75b89-153">The following example monitors the values of `Make` and `Model`.</span></span> <span data-ttu-id="75b89-154">Örnek, Web sunucusuna sunulan her farklı `Make` ve `Model` için içeriği önbelleğe alır:</span><span class="sxs-lookup"><span data-stu-id="75b89-154">The example caches the content for every different `Make` and `Model` presented to the web server:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a><span data-ttu-id="75b89-155">Yönlendirme ölçütü</span><span class="sxs-lookup"><span data-stu-id="75b89-155">vary-by-route</span></span>

| <span data-ttu-id="75b89-156">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-156">Attribute Type</span></span> | <span data-ttu-id="75b89-157">Örnekler</span><span class="sxs-lookup"><span data-stu-id="75b89-157">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="75b89-158">String</span><span class="sxs-lookup"><span data-stu-id="75b89-158">String</span></span>         | <span data-ttu-id="75b89-159">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="75b89-159">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="75b89-160">`vary-by-route`, yol verileri parametre değeri değiştiğinde önbellek yenilemeyi tetikleyen yol parametresi adlarının virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="75b89-160">`vary-by-route` accepts a comma-delimited list of route parameter names that trigger a cache refresh when the route data parameter value changes.</span></span>

<span data-ttu-id="75b89-161">Örnek:</span><span class="sxs-lookup"><span data-stu-id="75b89-161">Example:</span></span>

<span data-ttu-id="75b89-162">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="75b89-162">*Startup.cs*:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="75b89-163">*Index. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="75b89-163">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a><span data-ttu-id="75b89-164">tanımlama bilgisine göre farklılık</span><span class="sxs-lookup"><span data-stu-id="75b89-164">vary-by-cookie</span></span>

| <span data-ttu-id="75b89-165">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-165">Attribute Type</span></span> | <span data-ttu-id="75b89-166">Örnekler</span><span class="sxs-lookup"><span data-stu-id="75b89-166">Examples</span></span>                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| <span data-ttu-id="75b89-167">String</span><span class="sxs-lookup"><span data-stu-id="75b89-167">String</span></span>         | <span data-ttu-id="75b89-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span><span class="sxs-lookup"><span data-stu-id="75b89-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span></span> |

<span data-ttu-id="75b89-169">`vary-by-cookie`, tanımlama bilgisi değerleri değiştiğinde önbellek yenilemeyi tetikleyen tanımlama bilgisi adlarının virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="75b89-169">`vary-by-cookie` accepts a comma-delimited list of cookie names that trigger a cache refresh when the cookie values change.</span></span>

<span data-ttu-id="75b89-170">Aşağıdaki örnek ASP.NET Core kimlikle ilişkili tanımlama bilgisini izler.</span><span class="sxs-lookup"><span data-stu-id="75b89-170">The following example monitors the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="75b89-171">Bir kullanıcının kimliği doğrulandığında, kimlik tanımlama bilgisindeki bir değişiklik önbellek yenilemeyi tetikler:</span><span class="sxs-lookup"><span data-stu-id="75b89-171">When a user is authenticated, a change in the Identity cookie triggers a cache refresh:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a><span data-ttu-id="75b89-172">kullanıcıya göre değişiklik</span><span class="sxs-lookup"><span data-stu-id="75b89-172">vary-by-user</span></span>

| <span data-ttu-id="75b89-173">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-173">Attribute Type</span></span>  | <span data-ttu-id="75b89-174">Örnekler</span><span class="sxs-lookup"><span data-stu-id="75b89-174">Examples</span></span>        | <span data-ttu-id="75b89-175">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="75b89-175">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="75b89-176">Boole</span><span class="sxs-lookup"><span data-stu-id="75b89-176">Boolean</span></span>         | <span data-ttu-id="75b89-177">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="75b89-177">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="75b89-178">`vary-by-user`, oturum açan kullanıcının (veya bağlamı asıl) değiştiği zaman önbelleğin sıfırlamayacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="75b89-178">`vary-by-user` specifies whether or not the cache resets when the signed-in user (or Context Principal) changes.</span></span> <span data-ttu-id="75b89-179">Geçerli Kullanıcı, Istek bağlamı sorumlusu olarak da bilinir ve `@User.Identity.Name`başvurarak Razor görünümünde görüntülenebilir.</span><span class="sxs-lookup"><span data-stu-id="75b89-179">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="75b89-180">Aşağıdaki örnek, bir önbellek yenilemeyi tetiklemek için geçerli oturum açan kullanıcıyı izler:</span><span class="sxs-lookup"><span data-stu-id="75b89-180">The following example monitors the current logged in user to trigger a cache refresh:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="75b89-181">Bu özniteliğin kullanılması, oturum açma ve oturum kapatma döngüsüyle önbellekteki içerikleri saklar.</span><span class="sxs-lookup"><span data-stu-id="75b89-181">Using this attribute maintains the contents in cache through a sign-in and sign-out cycle.</span></span> <span data-ttu-id="75b89-182">Değer `true`olarak ayarlandığında, bir kimlik doğrulama çevrimi kimliği doğrulanmış kullanıcı için önbelleği geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="75b89-182">When the value is set to `true`, an authentication cycle invalidates the cache for the authenticated user.</span></span> <span data-ttu-id="75b89-183">Bir kullanıcının kimliği doğrulandığında yeni bir benzersiz tanımlama bilgisi değeri oluşturulduğundan önbellek geçersiz kılındı.</span><span class="sxs-lookup"><span data-stu-id="75b89-183">The cache is invalidated because a new unique cookie value is generated when a user is authenticated.</span></span> <span data-ttu-id="75b89-184">Bir tanımlama bilgisi yoksa veya tanımlama bilgisinin süresi dolduğunda önbellek, anonim durum için korunur.</span><span class="sxs-lookup"><span data-stu-id="75b89-184">Cache is maintained for the anonymous state when no cookie is present or the cookie has expired.</span></span> <span data-ttu-id="75b89-185">Kullanıcının kimliği **doğrulanmıyorsa** , önbellek korunur.</span><span class="sxs-lookup"><span data-stu-id="75b89-185">If the user is **not** authenticated, the cache is maintained.</span></span>

### <a name="vary-by"></a><span data-ttu-id="75b89-186">değişiklik ölçütü-</span><span class="sxs-lookup"><span data-stu-id="75b89-186">vary-by</span></span>

| <span data-ttu-id="75b89-187">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-187">Attribute Type</span></span> | <span data-ttu-id="75b89-188">Örnek</span><span class="sxs-lookup"><span data-stu-id="75b89-188">Example</span></span>  |
| -------------- | -------- |
| <span data-ttu-id="75b89-189">String</span><span class="sxs-lookup"><span data-stu-id="75b89-189">String</span></span>         | `@Model` |

<span data-ttu-id="75b89-190">`vary-by` hangi verilerin önbelleğe alınacağını özelleştirmek için izin verir.</span><span class="sxs-lookup"><span data-stu-id="75b89-190">`vary-by` allows for customization of what data is cached.</span></span> <span data-ttu-id="75b89-191">Özniteliğin dize değeri tarafından başvurulan nesne değiştiğinde, önbellek etiketi Yardımcısı 'nın içeriği güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="75b89-191">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="75b89-192">Genellikle, model değerlerinin dize birleştirmesi bu özniteliğe atanır.</span><span class="sxs-lookup"><span data-stu-id="75b89-192">Often, a string-concatenation of model values are assigned to this attribute.</span></span> <span data-ttu-id="75b89-193">Etkin olarak, bu, herhangi bir birleştirilmiş değerden bir güncelleştirmenin önbelleği geçersiz hale getirildiği bir senaryoya neden olur.</span><span class="sxs-lookup"><span data-stu-id="75b89-193">Effectively, this results in a scenario where an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="75b89-194">Aşağıdaki örnek, görünümü işleyen denetleyici yönteminin, `myParam1` ve `myParam2`iki yol parametresi tamsayı değerini toplamasını ve toplamı tek model özelliği olarak geri döndürdüğünü varsayar.</span><span class="sxs-lookup"><span data-stu-id="75b89-194">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns the sum as the single model property.</span></span> <span data-ttu-id="75b89-195">Bu toplam değiştiğinde, önbellek etiketi Yardımcısı 'nın içeriği işlenir ve yeniden önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="75b89-195">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="75b89-196">Eylem:</span><span class="sxs-lookup"><span data-stu-id="75b89-196">Action:</span></span>

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

<span data-ttu-id="75b89-197">*Index. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="75b89-197">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a><span data-ttu-id="75b89-198">öncelik</span><span class="sxs-lookup"><span data-stu-id="75b89-198">priority</span></span>

| <span data-ttu-id="75b89-199">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="75b89-199">Attribute Type</span></span>      | <span data-ttu-id="75b89-200">Örnekler</span><span class="sxs-lookup"><span data-stu-id="75b89-200">Examples</span></span>                               | <span data-ttu-id="75b89-201">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="75b89-201">Default</span></span>  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | <span data-ttu-id="75b89-202">`High`, `Low`, `NeverRemove`, `Normal`</span><span class="sxs-lookup"><span data-stu-id="75b89-202">`High`, `Low`, `NeverRemove`, `Normal`</span></span> | `Normal` |

<span data-ttu-id="75b89-203">`priority` yerleşik önbellek sağlayıcısına önbellek çıkarma kılavuzu sağlar.</span><span class="sxs-lookup"><span data-stu-id="75b89-203">`priority` provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="75b89-204">Web sunucusu, bellek baskısı altında olduğunda önce önbellek girdilerini `Low` çıkarşır.</span><span class="sxs-lookup"><span data-stu-id="75b89-204">The web server evicts `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="75b89-205">Örnek:</span><span class="sxs-lookup"><span data-stu-id="75b89-205">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="75b89-206">`priority` özniteliği belirli bir önbellek saklama düzeyini garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="75b89-206">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="75b89-207">`CacheItemPriority` yalnızca bir öneridir.</span><span class="sxs-lookup"><span data-stu-id="75b89-207">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="75b89-208">Bu özniteliği `NeverRemove` olarak ayarlamak, önbelleğe alınmış öğelerin her zaman korunduğunu garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="75b89-208">Setting this attribute to `NeverRemove` doesn't guarantee that cached items are always retained.</span></span> <span data-ttu-id="75b89-209">Daha fazla bilgi için [ek kaynaklar](#additional-resources) bölümündeki konulara bakın.</span><span class="sxs-lookup"><span data-stu-id="75b89-209">See the topics in the [Additional Resources](#additional-resources) section for more information.</span></span>

<span data-ttu-id="75b89-210">Önbellek etiketi Yardımcısı, [bellek önbelleği hizmetine](xref:performance/caching/memory)bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="75b89-210">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="75b89-211">Önbellek etiketi Yardımcısı eklenmemişse hizmeti ekler.</span><span class="sxs-lookup"><span data-stu-id="75b89-211">The Cache Tag Helper adds the service if it hasn't been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75b89-212">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="75b89-212">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
