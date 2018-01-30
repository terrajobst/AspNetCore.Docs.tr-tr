---
title: "ASP.NET Core yer işareti etiketi yok"
author: pkellner
description: "Yer işareti etiketi Yardımcısı ile çalışmaya nasıl gösterir"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="e0c2c-103">Yer işareti etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="e0c2c-103">Anchor Tag Helper</span></span>

<span data-ttu-id="e0c2c-104">Tarafından [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="e0c2c-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="e0c2c-105">Yer işareti etiketi yardımcı HTML bağlantı geliştirir (`<a ... ></a>`) yeni öznitelikler ekleyerek etiketi.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="e0c2c-106">Oluşturulan bağlantı (üzerinde `href` etiketi) yeni öznitelikler kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="e0c2c-107">Bu URL, https gibi isteğe bağlı bir protokolü içerebilir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="e0c2c-108">Aşağıdaki Konuşmacı denetleyicisi örnekleri bu belgedeki kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="e0c2c-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="e0c2c-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="e0c2c-110">Yer işareti etiketi yardımcı öznitelik</span><span class="sxs-lookup"><span data-stu-id="e0c2c-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="e0c2c-111">ASP denetleyicisi</span><span class="sxs-lookup"><span data-stu-id="e0c2c-111">asp-controller</span></span>

<span data-ttu-id="e0c2c-112">`asp-controller`hangi denetleyicisi URL'yi oluşturmak için kullanılan ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="e0c2c-113">Belirtilen denetleyicileri geçerli projede mevcut olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="e0c2c-114">Aşağıdaki kod tüm konuşmacılar listeler:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="e0c2c-115">Oluşturulan biçimlendirme olacaktır:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="e0c2c-116">Varsa `asp-controller` belirtilir ve `asp-action` değil, varsayılan `asp-action` şu anda yürütülen görünümünün varsayılan denetleyici yöntemi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="e0c2c-117">Olduğunu, yukarıdaki örnekte ise `asp-action` çıkışı, sol ve bu bağlantı etiketi yardımcı oluşturulur *HomeController*'s `Index` Görünüm (**/ev**), oluşturulan biçimlendirme olacaktır:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="e0c2c-118">ASP eylemi</span><span class="sxs-lookup"><span data-stu-id="e0c2c-118">asp-action</span></span>

<span data-ttu-id="e0c2c-119">`asp-action`Eklenecek denetleyicideki eylem yöntemi adını oluşturulan içinde `href`.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="e0c2c-120">Örneğin, aşağıdaki kodu oluşturulan ayarlayın `href` Konuşmacı Ayrıntı Sayfası'na işaret etmek için:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="e0c2c-121">Oluşturulan biçimlendirme olacaktır:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="e0c2c-122">Öyle değilse `asp-controller` özniteliği belirtilmezse, geçerli görünümü yürütme görünümü çağırma varsayılan denetleyicisi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="e0c2c-123">Öznitelik `asp-action` olan `Index`, hiçbir eylem varsayılan önde gelen URL, eklenecek sonra `Index` çağrılan yöntem.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="e0c2c-124">Eylem belirtilen (veya varsayılan), başvurulan denetleyicisi bulunmalıdır `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="e0c2c-125">ASP sayfasının</span><span class="sxs-lookup"><span data-stu-id="e0c2c-125">asp-page</span></span>

<span data-ttu-id="e0c2c-126">Kullanım `asp-page` belirli bir sayfaya işaret edecek şekilde URL'sini ayarlamak için bir yer işareti etiketi özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="e0c2c-127">Sayfa adı eğik çizgiyle önek "/" URL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="e0c2c-128">Aşağıdaki örnek URL'de geçerli dizin "Konuşmacı" sayfasında işaret eder.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="e0c2c-129">`asp-page` Öznitelik önceki kod örneğinde aşağıdaki kod parçacığını benzer görünümünde olan HTML çıktısı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="e0c2c-130">`asp-page` Özniteliği ile birbirini dışlayan `asp-route`, `asp-controller`, ve `asp-action` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="e0c2c-131">Ancak, `asp-page` ile kullanılan `asp-route-id` yönlendirme, aşağıdaki kod örneği gösterilmektedir olarak denetlemek için:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="e0c2c-132">`asp-route-id` Şu çıkışı üretir:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="e0c2c-133">Kullanılacak `asp-page` Razor sayfalarında, URL'ler özniteliği bir göreli yol olmalıdır, örneğin `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="e0c2c-134">Göreli yolda `asp-page` özniteliği MVC görünümlerde kullanılabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="e0c2c-135">"/" Sözdizimi için MVC görünümleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="e0c2c-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="e0c2c-136">asp-route-{value}</span></span>

<span data-ttu-id="e0c2c-137">`asp-route-`joker karakter rota öneki ' dir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="e0c2c-138">Sonda Tire olası bir rota parametresi olarak yorumlanacak sonra yerleştirdiğiniz herhangi bir değer.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="e0c2c-139">Varsayılan bir yol bulunmazsa, bu rota öneki oluşturulan href istek parametresi ve değeri olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="e0c2c-140">Aksi takdirde rota şablonunda değiştirilecektir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="e0c2c-141">Varsayılmıştır denetleyici yöntemi gibi tanımladınız:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="e0c2c-142">Ve içinde tanımlanan varsayılan rota şablonuna sahip, *haline* gibi:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="e0c2c-143">**Cshtml** kullanmak için gerekli bağlantı etiket Yardımcısı içeren dosya **Konuşmacı** denetleyicisinden görünüme iletilen model parametresi aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="e0c2c-144">Oluşturulan HTML sonra şu şekilde olacaktır, çünkü **kimliği** varsayılan rotada bulundu.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="e0c2c-145">Rota öneki bulunan yönlendirme şablonunun parçası değilse, olduğu aşağıdaki durumuyla **cshtml** dosyası:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="e0c2c-146">Oluşturulan HTML sonra şu şekilde olacaktır, çünkü **speakerid** eşleşen yol bulunamadı:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="e0c2c-147">Her iki `asp-controller` veya `asp-action` de olduğu gibi aynı varsayılan işleme ardından sonra belirtilmeyen `asp-route` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="e0c2c-148">ASP yol</span><span class="sxs-lookup"><span data-stu-id="e0c2c-148">asp-route</span></span>

<span data-ttu-id="e0c2c-149">`asp-route`adlandırılmış bir rotayı bağlanan doğrudan bir URL oluşturmak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="e0c2c-150">Yönlendirme özniteliklerini kullanarak, bir rota gösterildiği şekilde adlandırılabilir `SpeakerController` ve kullanılan kendi `Evaluations` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="e0c2c-151">`Name = "speakerevals"`bir rota URL'yi kullanarak doğrudan bu yönteme denetleyicisi oluşturmak için yer işareti etiketi yardımcı söyler `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="e0c2c-152">Varsa `asp-controller` veya `asp-action` ek olarak belirtilen `asp-route`, oluşturulan rota beklediğiniz olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="e0c2c-153">`asp-route`öznitelikleri birini kullanarak kullanılmaması `asp-controller` veya `asp-action` rota çakışmayı önlemek için.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="e0c2c-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="e0c2c-154">asp-all-route-data</span></span>

<span data-ttu-id="e0c2c-155">`asp-all-route-data`Burada anahtar parametre adı ve değeri bu anahtarla ilişkili değeri anahtar değer çifti sözlüğü oluşturma sağlar.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="e0c2c-156">Örnekteki aşağıda gösterildiği gibi bir satır içi sözlük oluşturulur ve veriler razor görünüme iletilir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="e0c2c-157">Alternatif olarak, veri modeliniz oturum gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="e0c2c-158">Yukarıdaki kod aşağıdaki URL'yi oluşturur: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="e0c2c-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="e0c2c-159">Ne zaman bağlantısı tıklatıldığında, denetleyici yönteminin `EvaluationsCurrent` olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="e0c2c-160">Bu denetleyici ne gelen oluşturuldu eşleşen iki dize parametresi olduğundan adlı `asp-all-route-data` sözlük.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="e0c2c-161">Herhangi bir anahtarı sözlük eşleşme parametreleri yol varsa, bu değerleri uygun şekilde rotadaki değiştirilecektir ve diğer eşleşmeyen değerleri İstek parametreleri oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="e0c2c-162">ASP parçası</span><span class="sxs-lookup"><span data-stu-id="e0c2c-162">asp-fragment</span></span>

<span data-ttu-id="e0c2c-163">`asp-fragment`URL'ye eklemek için URL parçası tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="e0c2c-164">Yer işareti etiketi yardımcı karma karakteri ekler (#).</span><span class="sxs-lookup"><span data-stu-id="e0c2c-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="e0c2c-165">Bir etiket oluşturmak istiyorsanız:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="e0c2c-166">Oluşturulan URL olacaktır: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="e0c2c-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="e0c2c-167">Karma etiketleri, istemci tarafı uygulamaları oluştururken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="e0c2c-168">Bunlar, kolay işaretleme ve JavaScript'te, örneğin arama için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="e0c2c-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="e0c2c-169">asp-area</span></span>

<span data-ttu-id="e0c2c-170">`asp-area`uygun yolu için ASP.NET Core kullanır alan adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="e0c2c-171">Yeniden eşleme yolların alanı özniteliği nasıl neden örnekleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="e0c2c-172">Ayarı `asp-area` Bloglara dizin önekleri `Areas/Blogs` ilişkili denetleyicilerinin ve görünümlerin bu yer işareti etiketi için yollar.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="e0c2c-173">Proje adı</span><span class="sxs-lookup"><span data-stu-id="e0c2c-173">Project name</span></span>
  * <span data-ttu-id="e0c2c-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="e0c2c-174">wwwroot</span></span>
  * <span data-ttu-id="e0c2c-175">Alanları</span><span class="sxs-lookup"><span data-stu-id="e0c2c-175">Areas</span></span>
    * <span data-ttu-id="e0c2c-176">Bloglar</span><span class="sxs-lookup"><span data-stu-id="e0c2c-176">Blogs</span></span>
      * <span data-ttu-id="e0c2c-177">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="e0c2c-177">Controllers</span></span>
        * <span data-ttu-id="e0c2c-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="e0c2c-178">HomeController.cs</span></span>
      * <span data-ttu-id="e0c2c-179">Görünümler</span><span class="sxs-lookup"><span data-stu-id="e0c2c-179">Views</span></span>
        * <span data-ttu-id="e0c2c-180">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="e0c2c-180">Home</span></span>
          * <span data-ttu-id="e0c2c-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="e0c2c-181">Index.cshtml</span></span>
          * <span data-ttu-id="e0c2c-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="e0c2c-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="e0c2c-183">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="e0c2c-183">Controllers</span></span>

<span data-ttu-id="e0c2c-184">Gibi geçerli bir alan etiketi belirtme ```area="Blogs"``` başvururken ```AboutBlog.cshtml``` dosya, aşağıdaki gibi görünür yer işareti etiketi Yardımcısını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="e0c2c-185">Oluşturulan HTML alanları segmenti içerir ve şu şekilde olacaktır:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="e0c2c-186">Varsa MVC alanları bir web uygulamasında çalışmak rota şablonu alanı için bir başvuru içermelidir.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="e0c2c-187">İkinci parametre Bu şablon, `routes.MapRoute` yöntem çağrısı olarak görünür:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="e0c2c-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="e0c2c-188">ASP Protokolü</span><span class="sxs-lookup"><span data-stu-id="e0c2c-188">asp-protocol</span></span>

<span data-ttu-id="e0c2c-189">`asp-protocol` Bir protokolü belirtmek için (gibi `https`) URL'nizde.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="e0c2c-190">Bir örnek protokolünü içeren bir yer işareti etiketi yardımcı şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="e0c2c-191">ve HTML gibi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="e0c2c-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="e0c2c-192">Örnekteki etki alanı, localhost olmakla birlikte bağlantı etiket Yardımcısı Web sitesinin ortak etki alanı için URL oluşturulurken kullanır.</span><span class="sxs-lookup"><span data-stu-id="e0c2c-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e0c2c-193">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e0c2c-193">Additional resources</span></span>

* [<span data-ttu-id="e0c2c-194">Alanlar</span><span class="sxs-lookup"><span data-stu-id="e0c2c-194">Areas</span></span>](xref:mvc/controllers/areas)
