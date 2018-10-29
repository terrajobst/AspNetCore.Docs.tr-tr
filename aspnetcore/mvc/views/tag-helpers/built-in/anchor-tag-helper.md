---
title: ASP.NET core'da yer işareti etiketi Yardımcısı
author: pkellner
description: ASP.NET Core yer işareti etiketi Yardımcısı öznitelikleri ve her bir öznitelik HTML yer işareti etiketi davranışını genişletmek oynadığı rolü keşfedin.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 01c5833210b73dafb763602d363afcf9e7bc0122
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206282"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="3631d-103">ASP.NET core'da yer işareti etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="3631d-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="3631d-104">Tarafından [Peter Kellner](http://peterkellner.net) ve [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="3631d-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="3631d-105">[Yer işareti etiketi Yardımcısı](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) standart HTML tutturucusu geliştirir (`<a ... ></a>`) yeni özellikler ekleyerek etiketi.</span><span class="sxs-lookup"><span data-stu-id="3631d-105">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="3631d-106">Kural gereği, öznitelik adları ile ön ekli `asp-`.</span><span class="sxs-lookup"><span data-stu-id="3631d-106">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="3631d-107">İşlenen bağlantı öğenin `href` öznitelik değeri, değerleri tarafından belirlenir `asp-` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="3631d-107">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="3631d-108">Etiket Yardımcıları genel bakış için bkz. <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="3631d-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="3631d-109">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3631d-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3631d-110">*SpeakerController* örnekleri bu belge boyunca kullanılır:</span><span class="sxs-lookup"><span data-stu-id="3631d-110">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="3631d-111">Bir envanterini `asp-` aşağıdaki öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="3631d-111">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="3631d-112">ASP denetleyicisi</span><span class="sxs-lookup"><span data-stu-id="3631d-112">asp-controller</span></span>

<span data-ttu-id="3631d-113">[Asp denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) öznitelik, URL'yi oluşturmak için kullanılan denetleyici atar.</span><span class="sxs-lookup"><span data-stu-id="3631d-113">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="3631d-114">Aşağıdaki biçimlendirmede tüm konuşmacılarını listeler:</span><span class="sxs-lookup"><span data-stu-id="3631d-114">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="3631d-115">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-115">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="3631d-116">Varsa `asp-controller` özniteliği belirtilirse ve `asp-action` değil, varsayılan `asp-action` yürütülmekte görünüm ile ilişkilendirilen denetleyici eylemi bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="3631d-116">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="3631d-117">Varsa `asp-action` önceki biçimlendirmeden atlanır ve yer işareti etiketi Yardımcısı kullanılır *HomeController*'s *dizin* görünümü (*/Home*), oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-117">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="3631d-118">ASP eylemi</span><span class="sxs-lookup"><span data-stu-id="3631d-118">asp-action</span></span>

<span data-ttu-id="3631d-119">[Asp eylem](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) öznitelik değeri temsil eden oluşturulmuş dahil denetleyici eylem adı `href` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="3631d-119">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="3631d-120">Aşağıdaki biçimlendirmede oluşturulan ayarlar `href` Konuşmacı değerlendirmeleri sayfasına öznitelik değeri:</span><span class="sxs-lookup"><span data-stu-id="3631d-120">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="3631d-121">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-121">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="3631d-122">Hayır ise `asp-controller` özniteliği belirtilmezse, geçerli görünüm yürütme görünümü çağırma varsayılan denetleyicisi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3631d-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="3631d-123">Varsa `asp-action` öznitelik değeri `Index`, hiçbir eylem için varsayılan çağırma giden URL, eklenecek sonra `Index` eylem.</span><span class="sxs-lookup"><span data-stu-id="3631d-123">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="3631d-124">Eylem belirtilen (veya varsayılan), başvurulan denetleyicisindeki bulunmalıdır `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="3631d-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="3631d-125">ASP - route-{value}</span><span class="sxs-lookup"><span data-stu-id="3631d-125">asp-route-{value}</span></span>

<span data-ttu-id="3631d-126">[Asp - route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) öznitelik joker karakter rota öneki sağlar.</span><span class="sxs-lookup"><span data-stu-id="3631d-126">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="3631d-127">Herhangi bir değer kaplayan `{value}` yer tutucusu, olası bir rota parametresini yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="3631d-127">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="3631d-128">Varsayılan bir yol bulunmazsa, bu rota öneki eklenir oluşturulan `href` bir istek parametresi ve değeri olarak özniteliği.</span><span class="sxs-lookup"><span data-stu-id="3631d-128">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="3631d-129">Aksi takdirde, bu rota şablonu konur.</span><span class="sxs-lookup"><span data-stu-id="3631d-129">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="3631d-130">Şu denetleyici eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="3631d-130">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="3631d-131">İçinde tanımlanan varsayılan rota şablonuyla *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="3631d-131">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="3631d-132">MVC görünümü gibi bir eylem tarafından sağlanan bir model kullanır:</span><span class="sxs-lookup"><span data-stu-id="3631d-132">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="3631d-133">Varsayılan yolun `{id?}` yer tutucu eşleşti.</span><span class="sxs-lookup"><span data-stu-id="3631d-133">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="3631d-134">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-134">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="3631d-135">Rota öneki eşleştirme yönlendirme şablonunun parçası olmadığından aşağıdaki MVC görünümü olarak kabul edin:</span><span class="sxs-lookup"><span data-stu-id="3631d-135">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="3631d-136">Aşağıdaki HTML'yi çünkü oluşturulan `speakerid` eşleşen yolda bulunamadı:</span><span class="sxs-lookup"><span data-stu-id="3631d-136">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="3631d-137">Ya da `asp-controller` veya `asp-action` belirtilmemişse, sonra aynı varsayılan işlem olduğundan ardından `asp-route` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="3631d-137">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="3631d-138">ASP yol</span><span class="sxs-lookup"><span data-stu-id="3631d-138">asp-route</span></span>

<span data-ttu-id="3631d-139">[Asp rota](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) öznitelik, URL'yi doğrudan adlandırılmış bir rotayı bağlama oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3631d-139">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="3631d-140">Kullanarak [yönlendirme öznitelikleri](xref:mvc/controllers/routing#attribute-routing), gösterildiği gibi bir yol adlandırılabilir `SpeakerController` ve kullanılan kendi `Evaluations` eylem:</span><span class="sxs-lookup"><span data-stu-id="3631d-140">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="3631d-141">Aşağıdaki biçimlendirmede `asp-route` öznitelik adlandırılmış rota başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="3631d-141">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="3631d-142">Yer işareti etiketi Yardımcısı doğrudan URL kullanılarak bu denetleyici eylemi bir yol oluşturur */Konuşmacı/değerlendirmeleri*.</span><span class="sxs-lookup"><span data-stu-id="3631d-142">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="3631d-143">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-143">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="3631d-144">Varsa `asp-controller` veya `asp-action` ek olarak belirtilen `asp-route`, oluşturulan rota beklediğiniz olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="3631d-144">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="3631d-145">Bir rota çakışmayı önlemek için `asp-route` ile kullanılmaması `asp-controller` ve `asp-action` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="3631d-145">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="3631d-146">ASP tüm rota veri</span><span class="sxs-lookup"><span data-stu-id="3631d-146">asp-all-route-data</span></span>

<span data-ttu-id="3631d-147">[Tüm rota veri asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) özniteliği bir anahtar-değer çiftlerinin dictionary'si oluşturulmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="3631d-147">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="3631d-148">Anahtarı parametre adı ve değerin parametre değeri olduğu.</span><span class="sxs-lookup"><span data-stu-id="3631d-148">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="3631d-149">Aşağıdaki örnekte, bir sözlük başlatılır ve bir Razor görünüme geçirildi.</span><span class="sxs-lookup"><span data-stu-id="3631d-149">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="3631d-150">Alternatif olarak, veri modelinizi oturum geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="3631d-150">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="3631d-151">Yukarıdaki kod, aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="3631d-151">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="3631d-152">`asp-all-route-data` Sözlük Aşırı yüklenen gereksinimlerini karşılayan bir sorgu dizesi üretmek için düzleştirilir `Evaluations` eylem:</span><span class="sxs-lookup"><span data-stu-id="3631d-152">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="3631d-153">Sözlükteki tüm anahtarları rota parametrelerinin eşleşiyorsa, bu değerleri uygun şekilde rotadaki yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3631d-153">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="3631d-154">Eşleşmeyen diğer değerleri, istek parametreleri oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3631d-154">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="3631d-155">ASP parçası</span><span class="sxs-lookup"><span data-stu-id="3631d-155">asp-fragment</span></span>

<span data-ttu-id="3631d-156">[Asp parça](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) özniteliği URL'ye için URL parçası belirtmesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="3631d-156">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="3631d-157">Yer işareti etiketi Yardımcısı karma karakteri ekler (#).</span><span class="sxs-lookup"><span data-stu-id="3631d-157">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="3631d-158">Aşağıdaki biçimlendirmede göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="3631d-158">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="3631d-159">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-159">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="3631d-160">Karma etiketleri, istemci tarafı uygulamalar oluştururken yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="3631d-160">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="3631d-161">Bunlar, kolay işaretleme ve JavaScript'te, örneğin arama için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3631d-161">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="3631d-162">ASP alanı</span><span class="sxs-lookup"><span data-stu-id="3631d-162">asp-area</span></span>

<span data-ttu-id="3631d-163">[Asp alan](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) özniteliği uygun bir yol ayarlamak için kullanılan alan adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3631d-163">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="3631d-164">Aşağıdaki örnek nasıl yeniden eşleme yollarını alan özniteliği neden gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="3631d-164">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="3631d-165">Ayarı `asp-area` "Bloglarda" dizin ön ekleri *alanlar/Blog'lar* ilişkili denetleyicileri ve görünümlerinin bu yer işareti etiketi için yollar.</span><span class="sxs-lookup"><span data-stu-id="3631d-165">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="3631d-166">**{} Proje adı**</span><span class="sxs-lookup"><span data-stu-id="3631d-166">**{Project name}**</span></span>
  * <span data-ttu-id="3631d-167">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="3631d-167">**wwwroot**</span></span>
  * <span data-ttu-id="3631d-168">**Alanlar**</span><span class="sxs-lookup"><span data-stu-id="3631d-168">**Areas**</span></span>
    * <span data-ttu-id="3631d-169">**Bloglar**</span><span class="sxs-lookup"><span data-stu-id="3631d-169">**Blogs**</span></span>
      * <span data-ttu-id="3631d-170">**Denetleyiciler**</span><span class="sxs-lookup"><span data-stu-id="3631d-170">**Controllers**</span></span>
        * <span data-ttu-id="3631d-171">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="3631d-171">*HomeController.cs*</span></span>
      * <span data-ttu-id="3631d-172">**Görünümler**</span><span class="sxs-lookup"><span data-stu-id="3631d-172">**Views**</span></span>
        * <span data-ttu-id="3631d-173">**Giriş**</span><span class="sxs-lookup"><span data-stu-id="3631d-173">**Home**</span></span>
          * <span data-ttu-id="3631d-174">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3631d-174">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="3631d-175">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3631d-175">*Index.cshtml*</span></span>
        * <span data-ttu-id="3631d-176">*\_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3631d-176">*\_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="3631d-177">**Denetleyiciler**</span><span class="sxs-lookup"><span data-stu-id="3631d-177">**Controllers**</span></span>

<span data-ttu-id="3631d-178">Yukarıdaki dizin hiyerarşisinin başvurmak için biçimlendirmeyi verilen *AboutBlog.cshtml* dosyasıdır:</span><span class="sxs-lookup"><span data-stu-id="3631d-178">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="3631d-179">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-179">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="3631d-180">Varsa bir MVC uygulamasında çalışma alanları için rota şablonu alanına bir başvuru içermelidir.</span><span class="sxs-lookup"><span data-stu-id="3631d-180">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="3631d-181">Bu şablon ikinci parametre tarafından temsil edilen `routes.MapRoute` yöntem çağrısı *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="3631d-181">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*:</span></span>
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a><span data-ttu-id="3631d-182">ASP Protokolü</span><span class="sxs-lookup"><span data-stu-id="3631d-182">asp-protocol</span></span>

<span data-ttu-id="3631d-183">[Asp Protokolü](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) özniteliği, bir protokol belirtmek için (gibi `https`), URL.</span><span class="sxs-lookup"><span data-stu-id="3631d-183">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="3631d-184">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3631d-184">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="3631d-185">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="3631d-186">Ana bilgisayar adı örnekteki localhost'tur, ancak URL oluşturulurken yer işareti etiketi Yardımcısı Web sitesinin genel etki alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="3631d-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="3631d-187">ASP konak</span><span class="sxs-lookup"><span data-stu-id="3631d-187">asp-host</span></span>

<span data-ttu-id="3631d-188">[Asp konak](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) özniteliği, bir ana bilgisayar adı, URL belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="3631d-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="3631d-189">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3631d-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="3631d-190">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="3631d-191">ASP sayfası</span><span class="sxs-lookup"><span data-stu-id="3631d-191">asp-page</span></span>

<span data-ttu-id="3631d-192">[Asp sayfasının](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) özniteliği, Razor sayfaları ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3631d-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="3631d-193">Bir yer işareti etiketin ayarlamak için kullanın `href` belirli bir sayfaya öznitelik değeri.</span><span class="sxs-lookup"><span data-stu-id="3631d-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="3631d-194">Sayfanın adını bir eğik çizgi ("/"), URL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3631d-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="3631d-195">Aşağıdaki örnek, katılımcı Razor sayfası noktaları:</span><span class="sxs-lookup"><span data-stu-id="3631d-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="3631d-196">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="3631d-197">`asp-page` Özniteliktir birbirini dışlayan `asp-route`, `asp-controller`, ve `asp-action` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="3631d-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="3631d-198">Ancak, `asp-page` kullanılabilir `asp-route-{value}` yönlendirme, aşağıdaki biçimlendirme gösterildiği gibi denetlemek için:</span><span class="sxs-lookup"><span data-stu-id="3631d-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="3631d-199">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="3631d-200">ASP sayfası işleyicisi</span><span class="sxs-lookup"><span data-stu-id="3631d-200">asp-page-handler</span></span>

<span data-ttu-id="3631d-201">[Asp sayfasını işleyici](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) özniteliği, Razor sayfaları ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3631d-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="3631d-202">Belirli bir sayfaya işleyicilerine bağlamak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3631d-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="3631d-203">Aşağıdaki sayfayı işleyici göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="3631d-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="3631d-204">Sayfa modeli biçimlendirme bağlantılar ilişkili `OnGetProfile` sayfası işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="3631d-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="3631d-205">Unutmayın `On<Verb>` sayfa işleyicisi yöntem adı ön eki atlanırsa `asp-page-handler` öznitelik değeri.</span><span class="sxs-lookup"><span data-stu-id="3631d-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="3631d-206">Bu zaman uyumsuz bir yöntem olsaydı `Async` soneki etmeyebilirsiniz çok.</span><span class="sxs-lookup"><span data-stu-id="3631d-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="3631d-207">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="3631d-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="3631d-208">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3631d-208">Additional resources</span></span>

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
