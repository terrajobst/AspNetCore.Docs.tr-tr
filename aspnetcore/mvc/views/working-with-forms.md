---
title: ASP.NET Core formlarda etiket Yardımcıları
author: rick-anderson
description: Formlarda etiket yardımcılarını kullanılan yerleşik açıklar.
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: mvc/views/working-with-forms
ms.openlocfilehash: 2d5168ed4b1e14e507262361de9fa959924b82f6
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209563"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="1277d-103">ASP.NET Core formlarda etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="1277d-103">Tag Helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="1277d-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [n'e Taylor Mullen](https://github.com/NTaylorMullen), [Dave Paquette](https://twitter.com/Dave_Paquette), ve [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="1277d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [N. Taylor Mullen](https://github.com/NTaylorMullen), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="1277d-105">Bu belge, form ve Form üzerinde kullanılan HTML öğeleri ile çalışma gösterir.</span><span class="sxs-lookup"><span data-stu-id="1277d-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="1277d-106">HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) öğesi, sunucunun geri veri göndermek için birincil mekanizma web apps kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="1277d-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="1277d-107">Bu belge çoğunu açıklar [etiket Yardımcıları](tag-helpers/intro.md) ve bunların nasıl verimli sağlam HTML formları oluşturmanıza yardımcı.</span><span class="sxs-lookup"><span data-stu-id="1277d-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="1277d-108">Okumanızı öneririz [etiket Yardımcıları giriş](tag-helpers/intro.md) önce bu belgeyi okuyun.</span><span class="sxs-lookup"><span data-stu-id="1277d-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="1277d-109">Çoğu durumda, HTML Yardımcıları, belirli bir etiketi Yardımcısı için alternatif bir yaklaşım sağlar, ancak etiket Yardımcıları HTML Yardımcıları değiştirin yoktur ve her HTML Yardımcısı için bir etiket Yardımcısı olmadığını bilmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="1277d-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="1277d-110">HTML Yardımcısı alternatif mevcut olduğunda, belirtilmiyor.</span><span class="sxs-lookup"><span data-stu-id="1277d-110">When an HTML Helper alternative exists, it's mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="1277d-111">Form etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="1277d-111">The Form Tag Helper</span></span>

<span data-ttu-id="1277d-112">[Form](https://www.w3.org/TR/html401/interact/forms.html) etiket Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="1277d-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="1277d-113">HTML oluşturan [ \<FORM >](https://www.w3.org/TR/html401/interact/forms.html) `action` MVC denetleyicisi eylemi veya adlandırılmış bir rota için öznitelik değeri</span><span class="sxs-lookup"><span data-stu-id="1277d-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="1277d-114">Gizli oluşturur [istek doğrulama belirteci](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) siteler arası istek sahteciliğini önlemek için (ile kullanıldığında `[ValidateAntiForgeryToken]` HTTP Post eylem yönteminde öznitelik)</span><span class="sxs-lookup"><span data-stu-id="1277d-114">Generates a hidden [Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="1277d-115">Sağlar `asp-route-<Parameter Name>` özniteliği burada `<Parameter Name>` rota değerleri için eklenir.</span><span class="sxs-lookup"><span data-stu-id="1277d-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="1277d-116">`routeValues` Parametreleri `Html.BeginForm` ve `Html.BeginRouteForm` benzer bir işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="1277d-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="1277d-117">HTML Yardımcısı alternatif olan `Html.BeginForm` ve `Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="1277d-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="1277d-118">Örnek:</span><span class="sxs-lookup"><span data-stu-id="1277d-118">Sample:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="1277d-119">Yukarıdaki Form etiketi Yardımcısı aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1277d-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

<span data-ttu-id="1277d-120">MVC çalışma zamanının oluşturduğu `action` öznitelik değeri Form etiketi Yardımcısı öznitelikleri `asp-controller` ve `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="1277d-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="1277d-121">Form etiketi Yardımcısı aynı zamanda bir gizli oluşturur [istek doğrulama belirteci](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) siteler arası istek sahteciliğini önlemek için (ile kullanıldığında `[ValidateAntiForgeryToken]` HTTP Post eylem yönteminin özniteliği).</span><span class="sxs-lookup"><span data-stu-id="1277d-121">The Form Tag Helper also generates a hidden [Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="1277d-122">Saf bir HTML Form siteler arası istek sahteciliğini koruma zordur, bu hizmet, Form etiketi Yardımcısı sağlar.</span><span class="sxs-lookup"><span data-stu-id="1277d-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="1277d-123">Adlandırılmış bir rotayı kullanma</span><span class="sxs-lookup"><span data-stu-id="1277d-123">Using a named route</span></span>

<span data-ttu-id="1277d-124">`asp-route` Etiketi yardımcı öznitelik için HTML biçimlendirme da üretebilir `action` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1277d-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="1277d-125">Bir uygulama ile bir [rota](../../fundamentals/routing.md) adlı `register` aşağıdaki biçimlendirme için kayıt sayfasına kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1277d-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="1277d-126">Görünümlerde çoğu *görünümler/hesap* klasörü (ile yeni bir web uygulaması oluşturduğunuzda oluşturulan *bireysel kullanıcı hesapları*) içeren [asp rota returnurl](xref:mvc/views/working-with-forms) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="1277d-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](xref:mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="1277d-127">Yerleşik şablonlarla `returnUrl` , yetkili bir kaynağa erişmeyi deneyin ancak kimlik doğrulaması veya yetkili yaptığınızda yalnızca otomatik olarak doldurulur.</span><span class="sxs-lookup"><span data-stu-id="1277d-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="1277d-128">Yetkisiz erişim denediğinizde, güvenlik ara yazılım, oturum açma sayfasına yönlendirir `returnUrl` ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1277d-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-form-action-tag-helper"></a><span data-ttu-id="1277d-129">Form eylem etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="1277d-129">The Form Action Tag Helper</span></span>

<span data-ttu-id="1277d-130">Form eylemi etiket Yardımcısı oluşturur `formaction` özniteliği oluşturulmuş `<button ...>` veya `<input type="image" ...>` etiketi.</span><span class="sxs-lookup"><span data-stu-id="1277d-130">The Form Action Tag Helper generates the `formaction` attribute on the generated `<button ...>` or `<input type="image" ...>` tag.</span></span> <span data-ttu-id="1277d-131">`formaction` Özniteliği denetimleri nerede form verilerini gönderir.</span><span class="sxs-lookup"><span data-stu-id="1277d-131">The `formaction` attribute controls where a form submits its data.</span></span> <span data-ttu-id="1277d-132">İçin bağlanan [ \<Giriş >](https://www.w3.org/wiki/HTML/Elements/input) türünde öğeler `image` ve [ \<düğmesi >](https://www.w3.org/wiki/HTML/Elements/button) öğeleri.</span><span class="sxs-lookup"><span data-stu-id="1277d-132">It binds to [\<input>](https://www.w3.org/wiki/HTML/Elements/input) elements of type `image` and [\<button>](https://www.w3.org/wiki/HTML/Elements/button) elements.</span></span> <span data-ttu-id="1277d-133">Form eylemi etiket Yardımcısı birkaç kullanımını etkinleştirir [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `asp-` ne denetlemek için öznitelikleri `formaction` bağlantı karşılık gelen öğe için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1277d-133">The Form Action Tag Helper enables the usage of several [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `asp-` attributes to control what `formaction` link is generated for the corresponding element.</span></span>

<span data-ttu-id="1277d-134">Desteklenen [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) değerini denetlemek için öznitelikleri `formaction`:</span><span class="sxs-lookup"><span data-stu-id="1277d-134">Supported [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) attributes to control the value of `formaction`:</span></span>

|<span data-ttu-id="1277d-135">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="1277d-135">Attribute</span></span>|<span data-ttu-id="1277d-136">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1277d-136">Description</span></span>|
|---|---|
|[<span data-ttu-id="1277d-137">ASP denetleyicisi</span><span class="sxs-lookup"><span data-stu-id="1277d-137">asp-controller</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-controller)|<span data-ttu-id="1277d-138">Denetleyicinin adı.</span><span class="sxs-lookup"><span data-stu-id="1277d-138">The name of the controller.</span></span>|
|[<span data-ttu-id="1277d-139">ASP eylemi</span><span class="sxs-lookup"><span data-stu-id="1277d-139">asp-action</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-action)|<span data-ttu-id="1277d-140">Eylem yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="1277d-140">The name of the action method.</span></span>|
|[<span data-ttu-id="1277d-141">ASP alanı</span><span class="sxs-lookup"><span data-stu-id="1277d-141">asp-area</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-area)|<span data-ttu-id="1277d-142">Alanın adı.</span><span class="sxs-lookup"><span data-stu-id="1277d-142">The name of the area.</span></span>|
|[<span data-ttu-id="1277d-143">ASP sayfası</span><span class="sxs-lookup"><span data-stu-id="1277d-143">asp-page</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page)|<span data-ttu-id="1277d-144">Razor sayfası adı.</span><span class="sxs-lookup"><span data-stu-id="1277d-144">The name of the Razor page.</span></span>|
|[<span data-ttu-id="1277d-145">ASP sayfası işleyicisi</span><span class="sxs-lookup"><span data-stu-id="1277d-145">asp-page-handler</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page-handler)|<span data-ttu-id="1277d-146">Razor sayfası işleyici adı.</span><span class="sxs-lookup"><span data-stu-id="1277d-146">The name of the Razor page handler.</span></span>|
|[<span data-ttu-id="1277d-147">ASP yol</span><span class="sxs-lookup"><span data-stu-id="1277d-147">asp-route</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route)|<span data-ttu-id="1277d-148">Rotanın adı.</span><span class="sxs-lookup"><span data-stu-id="1277d-148">The name of the route.</span></span>|
|[<span data-ttu-id="1277d-149">ASP - route-{value}</span><span class="sxs-lookup"><span data-stu-id="1277d-149">asp-route-{value}</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route-value)|<span data-ttu-id="1277d-150">Tek bir URL rota değeri.</span><span class="sxs-lookup"><span data-stu-id="1277d-150">A single URL route value.</span></span> <span data-ttu-id="1277d-151">Örneğin: `asp-route-id="1234"`</span><span class="sxs-lookup"><span data-stu-id="1277d-151">For example, `asp-route-id="1234"`.</span></span>|
|[<span data-ttu-id="1277d-152">ASP tüm rota veri</span><span class="sxs-lookup"><span data-stu-id="1277d-152">asp-all-route-data</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-all-route-data)|<span data-ttu-id="1277d-153">Tüm rota değerleri.</span><span class="sxs-lookup"><span data-stu-id="1277d-153">All route values.</span></span>|
|[<span data-ttu-id="1277d-154">ASP parçası</span><span class="sxs-lookup"><span data-stu-id="1277d-154">asp-fragment</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-fragment)|<span data-ttu-id="1277d-155">URL parçası belirtmesini.</span><span class="sxs-lookup"><span data-stu-id="1277d-155">The URL fragment.</span></span>|

### <a name="submit-to-controller-example"></a><span data-ttu-id="1277d-156">Denetleyici örneği gönderme</span><span class="sxs-lookup"><span data-stu-id="1277d-156">Submit to controller example</span></span>

<span data-ttu-id="1277d-157">Aşağıdaki biçimlendirme için formu gönderdiği `Index` eylemi `HomeController` düğmesini veya girdi seçili olduğunda:</span><span class="sxs-lookup"><span data-stu-id="1277d-157">The following markup submits the form to the `Index` action of `HomeController` when the input or button are selected:</span></span>

```cshtml
<form method="post">
    <button asp-controller="Home" asp-action="Index">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-controller="Home" 
                                asp-action="Index" />
</form>
```

<span data-ttu-id="1277d-158">Aşağıdaki HTML önceki biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1277d-158">The previous markup generates following HTML:</span></span>

```html
<form method="post">
    <button formaction="/Home">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home" />
</form>
```

### <a name="submit-to-page-example"></a><span data-ttu-id="1277d-159">Gönderme sayfası örneği</span><span class="sxs-lookup"><span data-stu-id="1277d-159">Submit to page example</span></span>

<span data-ttu-id="1277d-160">Aşağıdaki biçimlendirme için formu gönderdiği `About` Razor sayfası:</span><span class="sxs-lookup"><span data-stu-id="1277d-160">The following markup submits the form to the `About` Razor Page:</span></span>

```cshtml
<form method="post">
    <button asp-page="About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-page="About" />
</form>
```

<span data-ttu-id="1277d-161">Aşağıdaki HTML önceki biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1277d-161">The previous markup generates following HTML:</span></span>

```html
<form method="post">
    <button formaction="/About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/About" />
</form>
```

### <a name="submit-to-route-example"></a><span data-ttu-id="1277d-162">Rota örneği gönderme</span><span class="sxs-lookup"><span data-stu-id="1277d-162">Submit to route example</span></span>

<span data-ttu-id="1277d-163">Göz önünde bulundurun `/Home/Test` uç noktası:</span><span class="sxs-lookup"><span data-stu-id="1277d-163">Consider the `/Home/Test` endpoint:</span></span>

```csharp
public class HomeController : Controller
{
    [Route("/Home/Test", Name = "Custom")]
    public string Test()
    {
        return "This is the test page";
    }
}
```

<span data-ttu-id="1277d-164">Aşağıdaki biçimlendirme için formu gönderdiği `/Home/Test` uç noktası.</span><span class="sxs-lookup"><span data-stu-id="1277d-164">The following markup submits the form to the `/Home/Test` endpoint.</span></span>

```cshtml
<form method="post">
    <button asp-route="Custom">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-route="Custom" />
</form>
```

<span data-ttu-id="1277d-165">Aşağıdaki HTML önceki biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1277d-165">The previous markup generates following HTML:</span></span>

```html
<form method="post">
    <button formaction="/Home/Test">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home/Test" />
</form>
```

## <a name="the-input-tag-helper"></a><span data-ttu-id="1277d-166">Giriş etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="1277d-166">The Input Tag Helper</span></span>

<span data-ttu-id="1277d-167">Bir HTML giriş etiketi Yardımcısı bağlar [ \<Giriş >](https://www.w3.org/wiki/HTML/Elements/input) , razor görünüm modeli ifadesinde öğesi.</span><span class="sxs-lookup"><span data-stu-id="1277d-167">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="1277d-168">Sözdizimi:</span><span class="sxs-lookup"><span data-stu-id="1277d-168">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="1277d-169">Giriş etiketi Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="1277d-169">The Input Tag Helper:</span></span>

* <span data-ttu-id="1277d-170">Oluşturur `id` ve `name` belirtilen ifade adı için HTML özniteliklerini `asp-for` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1277d-170">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="1277d-171">`asp-for="Property1.Property2"` eşdeğerdir `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="1277d-171">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="1277d-172">İçin kullanılan ifade adıdır `asp-for` öznitelik değeri.</span><span class="sxs-lookup"><span data-stu-id="1277d-172">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="1277d-173">Bkz: [ifade adlarının](#expression-names) ek bilgi bölümü.</span><span class="sxs-lookup"><span data-stu-id="1277d-173">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="1277d-174">HTML ayarlar `type` öznitelik değeri model türü temel alarak ve [veri ek açıklama](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) model özelliğine uygulanan öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="1277d-174">Sets the HTML `type` attribute value based on the model type and  [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="1277d-175">HTML üzerine yazılmayacak `type` biri belirtildiğinde öznitelik değeri</span><span class="sxs-lookup"><span data-stu-id="1277d-175">Won't overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="1277d-176">Oluşturur [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) doğrulama öznitelikleri gelen [veri ek açıklama](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) modeli özelliklerine uygulanan öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="1277d-176">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="1277d-177">Bir HTML Yardımcısı özelliği ile çakışan `Html.TextBoxFor` ve `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="1277d-177">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="1277d-178">Bkz: **giriş etiketi Yardımcısı HTML Yardımcısı alternatifleri** ayrıntıları bölümü.</span><span class="sxs-lookup"><span data-stu-id="1277d-178">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="1277d-179">Güçlü yazmayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="1277d-179">Provides strong typing.</span></span> <span data-ttu-id="1277d-180">Özellik değişiklikleri ve adını etiketi Yardımcısı güncelleştirmezseniz aşağıdakine benzer bir hata alırsınız:</span><span class="sxs-lookup"><span data-stu-id="1277d-180">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="1277d-181">`Input` HTML etiketi Yardımcısı ayarlar `type` öznitelik tabanlı üzerinde .NET türü.</span><span class="sxs-lookup"><span data-stu-id="1277d-181">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="1277d-182">Aşağıdaki tabloda, bazı yaygın .NET türleri ve oluşturulan HTML türü (her .NET türü listelenir) listeler.</span><span class="sxs-lookup"><span data-stu-id="1277d-182">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="1277d-183">.NET türü</span><span class="sxs-lookup"><span data-stu-id="1277d-183">.NET type</span></span>|<span data-ttu-id="1277d-184">Giriş türü</span><span class="sxs-lookup"><span data-stu-id="1277d-184">Input Type</span></span>|
|---|---|
|<span data-ttu-id="1277d-185">Bool</span><span class="sxs-lookup"><span data-stu-id="1277d-185">Bool</span></span>|<span data-ttu-id="1277d-186">type="checkbox"</span><span class="sxs-lookup"><span data-stu-id="1277d-186">type="checkbox"</span></span>|
|<span data-ttu-id="1277d-187">Dize</span><span class="sxs-lookup"><span data-stu-id="1277d-187">String</span></span>|<span data-ttu-id="1277d-188">type="text"</span><span class="sxs-lookup"><span data-stu-id="1277d-188">type="text"</span></span>|
|<span data-ttu-id="1277d-189">DateTime</span><span class="sxs-lookup"><span data-stu-id="1277d-189">DateTime</span></span>|<span data-ttu-id="1277d-190">tür =["yerel datetime"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)</span><span class="sxs-lookup"><span data-stu-id="1277d-190">type=["datetime-local"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)</span></span>|
|<span data-ttu-id="1277d-191">Bayt</span><span class="sxs-lookup"><span data-stu-id="1277d-191">Byte</span></span>|<span data-ttu-id="1277d-192">type="number"</span><span class="sxs-lookup"><span data-stu-id="1277d-192">type="number"</span></span>|
|<span data-ttu-id="1277d-193">int</span><span class="sxs-lookup"><span data-stu-id="1277d-193">Int</span></span>|<span data-ttu-id="1277d-194">type="number"</span><span class="sxs-lookup"><span data-stu-id="1277d-194">type="number"</span></span>|
|<span data-ttu-id="1277d-195">Tek, Double</span><span class="sxs-lookup"><span data-stu-id="1277d-195">Single, Double</span></span>|<span data-ttu-id="1277d-196">type="number"</span><span class="sxs-lookup"><span data-stu-id="1277d-196">type="number"</span></span>|

<span data-ttu-id="1277d-197">Aşağıdaki tablo bazı yaygın gösterir [veri ek açıklamaları](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) giriş etiketi Yardımcısı (her doğrulama özniteliği listelenir) belirli giriş türleri için eşler öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="1277d-197">The following table shows some common [data annotations](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>

|<span data-ttu-id="1277d-198">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="1277d-198">Attribute</span></span>|<span data-ttu-id="1277d-199">Giriş türü</span><span class="sxs-lookup"><span data-stu-id="1277d-199">Input Type</span></span>|
|---|---|
|<span data-ttu-id="1277d-200">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="1277d-200">[EmailAddress]</span></span>|<span data-ttu-id="1277d-201">tür = "email"</span><span class="sxs-lookup"><span data-stu-id="1277d-201">type="email"</span></span>|
|<span data-ttu-id="1277d-202">[Url]</span><span class="sxs-lookup"><span data-stu-id="1277d-202">[Url]</span></span>|<span data-ttu-id="1277d-203">type="url"</span><span class="sxs-lookup"><span data-stu-id="1277d-203">type="url"</span></span>|
|<span data-ttu-id="1277d-204">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="1277d-204">[HiddenInput]</span></span>|<span data-ttu-id="1277d-205">type="hidden"</span><span class="sxs-lookup"><span data-stu-id="1277d-205">type="hidden"</span></span>|
|<span data-ttu-id="1277d-206">[Phone]</span><span class="sxs-lookup"><span data-stu-id="1277d-206">[Phone]</span></span>|<span data-ttu-id="1277d-207">type="tel"</span><span class="sxs-lookup"><span data-stu-id="1277d-207">type="tel"</span></span>|
|<span data-ttu-id="1277d-208">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="1277d-208">[DataType(DataType.Password)]</span></span>|<span data-ttu-id="1277d-209">type="password"</span><span class="sxs-lookup"><span data-stu-id="1277d-209">type="password"</span></span>|
|<span data-ttu-id="1277d-210">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="1277d-210">[DataType(DataType.Date)]</span></span>|<span data-ttu-id="1277d-211">type="date"</span><span class="sxs-lookup"><span data-stu-id="1277d-211">type="date"</span></span>|
|<span data-ttu-id="1277d-212">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="1277d-212">[DataType(DataType.Time)]</span></span>|<span data-ttu-id="1277d-213">type="time"</span><span class="sxs-lookup"><span data-stu-id="1277d-213">type="time"</span></span>|

<span data-ttu-id="1277d-214">Örnek:</span><span class="sxs-lookup"><span data-stu-id="1277d-214">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="1277d-215">Yukarıdaki kod, aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1277d-215">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="1277d-216">Uygulanan veri ek açıklamaları `Email` ve `Password` özellikleri model meta verilerini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1277d-216">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="1277d-217">Giriş etiketi Yardımcısı model meta verilerini kullanır ve üretir [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` öznitelikleri (bkz [Model doğrulama](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="1277d-217">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="1277d-218">Bu öznitelikler için giriş alanlarını eklemek için doğrulayıcıları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1277d-218">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="1277d-219">Bu örtük HTML5 sağlar ve [jQuery](https://jquery.com/) doğrulama.</span><span class="sxs-lookup"><span data-stu-id="1277d-219">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="1277d-220">Örtük özniteliklerinin biçimine sahip `data-val-rule="Error Message"`kuralı adı doğrulama kuralının olduğu (gibi `data-val-required`, `data-val-email`, `data-val-maxlength`vb..) Öznitelik, bir hata iletisi sağlanırsa, değeri olarak görüntülenir `data-val-rule` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1277d-220">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it's displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="1277d-221">Form öznitelikler de vardır `data-val-ruleName-argumentName="argumentValue"` gibi kural hakkındaki ek ayrıntıları sağlayın `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="1277d-221">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="1277d-222">HTML Yardımcısı alternatifleri giriş etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="1277d-222">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="1277d-223">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` ve `Html.EditorFor` giriş etiketi Yardımcısı özelliklerle örtüşüyor.</span><span class="sxs-lookup"><span data-stu-id="1277d-223">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="1277d-224">Giriş etiketi Yardımcısı otomatik olarak ayarlayacak `type` özniteliği; `Html.TextBox` ve `Html.TextBoxFor` olmaz.</span><span class="sxs-lookup"><span data-stu-id="1277d-224">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` won't.</span></span> <span data-ttu-id="1277d-225">`Html.Editor` ve `Html.EditorFor` işlemek koleksiyonlar ve karmaşık nesneler şablonları; girişi etiketi Yardımcısı değil.</span><span class="sxs-lookup"><span data-stu-id="1277d-225">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper doesn't.</span></span> <span data-ttu-id="1277d-226">Giriş etiketi Yardımcısı `Html.EditorFor` ve `Html.TextBoxFor` türü kesin belirlenmiş (bunlar, lambda ifadeleri kullanma); `Html.TextBox` ve `Html.Editor` değildir (bunlar ifade adlarının kullanın).</span><span class="sxs-lookup"><span data-stu-id="1277d-226">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="1277d-227">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="1277d-227">HtmlAttributes</span></span>

<span data-ttu-id="1277d-228">`@Html.Editor()` ve `@Html.EditorFor()` özel bir kullanın `ViewDataDictionary` adlı giriş `htmlAttributes` kendi varsayılan şablonları yürütülürken.</span><span class="sxs-lookup"><span data-stu-id="1277d-228">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="1277d-229">Bu davranış kullanarak isteğe bağlı olarak Genişletilmiş `additionalViewData` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="1277d-229">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="1277d-230">' % S'anahtarı "htmlAttributes" büyük/küçük harf duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="1277d-230">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="1277d-231">' % S'anahtarı "htmlAttributes" benzer şekilde işlendiğini `htmlAttributes` nesnesi geçirildi yardımcıları gibi giriş `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="1277d-231">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="1277d-232">İfade adları</span><span class="sxs-lookup"><span data-stu-id="1277d-232">Expression names</span></span>

<span data-ttu-id="1277d-233">`asp-for` Öznitelik değeri bir `ModelExpression` ve bir lambda ifadesinin sağ tarafı.</span><span class="sxs-lookup"><span data-stu-id="1277d-233">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="1277d-234">Bu nedenle, `asp-for="Property1"` olur `m => m.Property1` olmasının nedeni oluşturulan kodda ile önek gerekmez `Model`.</span><span class="sxs-lookup"><span data-stu-id="1277d-234">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="1277d-235">Kullanabileceğiniz "\@" satır içi ifadesi başlatmak ve önce taşımak için karakter `m.`:</span><span class="sxs-lookup"><span data-stu-id="1277d-235">You can use the "\@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="1277d-236">Aşağıdakileri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1277d-236">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="1277d-237">Koleksiyon Özellikleri ile `asp-for="CollectionProperty[23].Member"` aynı adı taşıyan oluşturur `asp-for="CollectionProperty[i].Member"` olduğunda `i` değerine sahip `23`.</span><span class="sxs-lookup"><span data-stu-id="1277d-237">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

<span data-ttu-id="1277d-238">Zaman değerini hesaplar ASP.NET Core MVC `ModelExpression`, dahil, çeşitli kaynaklardan inceler `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="1277d-238">When ASP.NET Core MVC calculates the value of `ModelExpression`, it inspects several sources, including `ModelState`.</span></span> <span data-ttu-id="1277d-239">Göz önünde bulundurun `<input type="text" asp-for="@Name" />`.</span><span class="sxs-lookup"><span data-stu-id="1277d-239">Consider `<input type="text" asp-for="@Name" />`.</span></span> <span data-ttu-id="1277d-240">Hesaplanan `value` özniteliktir boş olmayan ilk değeri:</span><span class="sxs-lookup"><span data-stu-id="1277d-240">The calculated `value` attribute is the first non-null value from:</span></span>

* <span data-ttu-id="1277d-241">`ModelState` "Name" anahtarla girişi.</span><span class="sxs-lookup"><span data-stu-id="1277d-241">`ModelState` entry with key "Name".</span></span>
* <span data-ttu-id="1277d-242">İfadenin sonucu `Model.Name`.</span><span class="sxs-lookup"><span data-stu-id="1277d-242">Result of the expression `Model.Name`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="1277d-243">Alt özellikleri gezinme</span><span class="sxs-lookup"><span data-stu-id="1277d-243">Navigating child properties</span></span>

<span data-ttu-id="1277d-244">Görünüm modeli, özellik yolu kullanarak alt özellikleri için de gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1277d-244">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="1277d-245">Bir alt içeren daha karmaşık bir model sınıfı göz önünde bulundurun `Address` özelliği.</span><span class="sxs-lookup"><span data-stu-id="1277d-245">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="1277d-246">Görünümü'nde, biz bağlamak `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="1277d-246">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="1277d-247">Aşağıdaki HTML'yi için oluşturulan `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="1277d-247">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="1277d-248">İfade adlarının ve koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="1277d-248">Expression names and Collections</span></span>

<span data-ttu-id="1277d-249">Örnek, bir dizi içeren bir model `Colors`:</span><span class="sxs-lookup"><span data-stu-id="1277d-249">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="1277d-250">Eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1277d-250">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="1277d-251">Aşağıdaki Razor, belirli bir erişim nasıl gösterir `Color` öğesi:</span><span class="sxs-lookup"><span data-stu-id="1277d-251">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="1277d-252">*Views/Shared/EditorTemplates/String.cshtml* şablonu:</span><span class="sxs-lookup"><span data-stu-id="1277d-252">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="1277d-253">Kullanarak örnek `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="1277d-253">Sample using `List<T>`:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="1277d-254">Aşağıdaki Razor, bir koleksiyon üzerinde yinelemek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="1277d-254">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="1277d-255">*Views/Shared/EditorTemplates/ToDoItem.cshtml* şablonu:</span><span class="sxs-lookup"><span data-stu-id="1277d-255">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]

<span data-ttu-id="1277d-256">`foreach` mümkün olduğunda kullanılacak değer geçerken kullanılmalıdır bir `asp-for` veya `Html.DisplayFor` eşdeğer bağlamı.</span><span class="sxs-lookup"><span data-stu-id="1277d-256">`foreach` should be used if possible when the value is going to be used in an `asp-for` or `Html.DisplayFor` equivalent context.</span></span> <span data-ttu-id="1277d-257">Genel olarak, `for` göre daha iyidir `foreach` (senaryo izin verirse) bir numaralandırıcı; ayırmak gerekmez çünkü ancak bir oluşturucuda bir LINQ ifadesini değerlendirme pahalı olabilir ve küçültülmesine.</span><span class="sxs-lookup"><span data-stu-id="1277d-257">In general, `for` is better than `foreach` (if the scenario allows it) because it doesn't need to allocate an enumerator; however, evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="1277d-258">Yukarıdaki açıklamalı örnek kod nasıl lambda ifadesiyle değiştirin gösterir `@` her erişim işleci `ToDoItem` listesinde.</span><span class="sxs-lookup"><span data-stu-id="1277d-258">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="1277d-259">Textarea etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="1277d-259">The Textarea Tag Helper</span></span>

<span data-ttu-id="1277d-260">`Textarea Tag Helper` Etiketi Yardımcısı için giriş etiketi Yardımcısı benzerdir.</span><span class="sxs-lookup"><span data-stu-id="1277d-260">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="1277d-261">Oluşturur `id` ve `name` öznitelikleri ve model için doğrulama öznitelikleri veri bir [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) öğesi.</span><span class="sxs-lookup"><span data-stu-id="1277d-261">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="1277d-262">Güçlü yazmayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="1277d-262">Provides strong typing.</span></span>

* <span data-ttu-id="1277d-263">HTML Yardımcısı alternatif: `Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="1277d-263">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="1277d-264">Örnek:</span><span class="sxs-lookup"><span data-stu-id="1277d-264">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="1277d-265">Aşağıdaki HTML'yi oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="1277d-265">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="1277d-266">Etiket etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="1277d-266">The Label Tag Helper</span></span>

* <span data-ttu-id="1277d-267">Etiket başlığını oluşturur ve `for` özniteliği bir [ \<etiket >](https://www.w3.org/wiki/HTML/Elements/label) öğesi için bir ifade adı</span><span class="sxs-lookup"><span data-stu-id="1277d-267">Generates the label caption and `for` attribute on a [\<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="1277d-268">HTML Yardımcısı alternatif: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="1277d-268">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="1277d-269">`Label Tag Helper` Saf bir HTML label öğesini aşağıdaki avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="1277d-269">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="1277d-270">Açıklayıcı bir etiket değerini otomatik olarak Al `Display` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1277d-270">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="1277d-271">Hedeflenen görünen adı, saati ve birleşimi değişebilir `Display` özniteliği ve etiket etiketi Yardımcısı geçerli `Display` her yerde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1277d-271">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="1277d-272">Kaynak kodunda daha az biçimlendirme</span><span class="sxs-lookup"><span data-stu-id="1277d-272">Less markup in source code</span></span>

* <span data-ttu-id="1277d-273">İle model özelliğine yazarak güçlü.</span><span class="sxs-lookup"><span data-stu-id="1277d-273">Strong typing with the model property.</span></span>

<span data-ttu-id="1277d-274">Örnek:</span><span class="sxs-lookup"><span data-stu-id="1277d-274">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="1277d-275">Aşağıdaki HTML'yi için oluşturulan `<label>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="1277d-275">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="1277d-276">Oluşturulan etiket etiketi Yardımcısı `for` kimliği "Email" özniteliğinin değerini ilişkili `<input>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="1277d-276">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="1277d-277">Etiket Yardımcıları tutarlı oluşturmak `id` ve `for` öğelerinin doğru bir şekilde ilişkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="1277d-277">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="1277d-278">Bu örnekte açıklamalı alt yazı geldiği `Display` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1277d-278">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="1277d-279">Modele sahip değilse, bir `Display` özniteliği, açıklamalı alt yazı ifade özellik adı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1277d-279">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="1277d-280">Doğrulama etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="1277d-280">The Validation Tag Helpers</span></span>

<span data-ttu-id="1277d-281">İki doğrulama etiket Yardımcıları vardır.</span><span class="sxs-lookup"><span data-stu-id="1277d-281">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="1277d-282">`Validation Message Tag Helper` (Görüntüleyen tek bir özellik için bir doğrulama iletisini modelinize göre) ve `Validation Summary Tag Helper` (doğrulama hatalarının özetini görüntüleyen).</span><span class="sxs-lookup"><span data-stu-id="1277d-282">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="1277d-283">`Input Tag Helper` HTML5 istemci tarafı doğrulama özniteliklerinin öğeleri model sınıflarınızı ek açıklama özniteliklerinde verileri temel alarak giriş ekler.</span><span class="sxs-lookup"><span data-stu-id="1277d-283">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="1277d-284">Doğrulama Ayrıca sunucu üzerinde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1277d-284">Validation is also performed on the server.</span></span> <span data-ttu-id="1277d-285">Doğrulama etiketi Yardımcısı, bir doğrulama hatası oluştuğunda bu hata iletilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1277d-285">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="1277d-286">Doğrulama iletisi etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="1277d-286">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="1277d-287">Ekler [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` özniteliğini [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) öğesinin giriş alanını belirtilen model özelliğinin bir doğrulama hata iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="1277d-287">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="1277d-288">İstemci tarafı doğrulama hatası oluştuğunda [jQuery](https://jquery.com/) hata iletisi görüntüler `<span>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="1277d-288">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="1277d-289">Doğrulama sunucusundaki de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="1277d-289">Validation also takes place on the server.</span></span> <span data-ttu-id="1277d-290">JavaScript devre dışı istemciniz ve bazı doğrulama yalnızca sunucu tarafında gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="1277d-290">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="1277d-291">HTML Yardımcısı alternatif: `Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="1277d-291">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="1277d-292">`Validation Message Tag Helper` İle kullanılan `asp-validation-for` bir HTML özniteliğinin [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) öğesi.</span><span class="sxs-lookup"><span data-stu-id="1277d-292">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="1277d-293">Doğrulama iletisi etiketi Yardımcısı aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1277d-293">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="1277d-294">Genel olarak kullandığınız `Validation Message Tag Helper` sonra bir `Input` aynı özellik için etiket Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="1277d-294">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="1277d-295">Bunun yapılması, hataya neden giriş neredeyse herhangi bir doğrulama hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1277d-295">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="1277d-296">Doğru JavaScript olan bir görünümü olmalıdır ve [jQuery](https://jquery.com/) başvuruları yerde istemci tarafı doğrulama komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="1277d-296">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="1277d-297">Bkz: [Model doğrulama](../models/validation.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1277d-297">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="1277d-298">MVC (örneğin, sahip olduğunuz özel sunucu tarafı doğrulama veya istemci tarafı doğrulama devre dışı) bir sunucu tarafı doğrulama hatası oluştuğunda, hata iletisi gövdesi olarak yerleştirir. `<span>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="1277d-298">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="1277d-299">Doğrulama Özeti etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="1277d-299">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="1277d-300">Hedefleri `<div>` öğelerle `asp-validation-summary` özniteliği</span><span class="sxs-lookup"><span data-stu-id="1277d-300">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="1277d-301">HTML Yardımcısı alternatif: `@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="1277d-301">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="1277d-302">`Validation Summary Tag Helper` Doğrulama iletilerinin bir özetini görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1277d-302">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="1277d-303">`asp-validation-summary` Öznitelik değeri aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="1277d-303">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="1277d-304">ASP doğrulama özeti</span><span class="sxs-lookup"><span data-stu-id="1277d-304">asp-validation-summary</span></span>|<span data-ttu-id="1277d-305">Görüntülenen doğrulama iletileri</span><span class="sxs-lookup"><span data-stu-id="1277d-305">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="1277d-306">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="1277d-306">ValidationSummary.All</span></span>|<span data-ttu-id="1277d-307">Özellik ve model düzeyi</span><span class="sxs-lookup"><span data-stu-id="1277d-307">Property and model level</span></span>|
|<span data-ttu-id="1277d-308">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="1277d-308">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="1277d-309">Model</span><span class="sxs-lookup"><span data-stu-id="1277d-309">Model</span></span>|
|<span data-ttu-id="1277d-310">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="1277d-310">ValidationSummary.None</span></span>|<span data-ttu-id="1277d-311">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="1277d-311">None</span></span>|

### <a name="sample"></a><span data-ttu-id="1277d-312">Örnek</span><span class="sxs-lookup"><span data-stu-id="1277d-312">Sample</span></span>

<span data-ttu-id="1277d-313">Aşağıdaki örnekte, veri modeli ile donatılmış `DataAnnotation` üzerinde doğrulama hatası iletilerinin oluşturan öznitelikleri `<input>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="1277d-313">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="1277d-314">Doğrulama etiketi Yardımcısı, bir doğrulama hatası oluştuğunda, hata iletisi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="1277d-314">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="1277d-315">Oluşturulan HTML (model geçerli olduğunda):</span><span class="sxs-lookup"><span data-stu-id="1277d-315">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="1277d-316">Seçim etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="1277d-316">The Select Tag Helper</span></span>

* <span data-ttu-id="1277d-317">Oluşturur [seçin](https://www.w3.org/wiki/HTML/Elements/select) ve ilişkili [seçeneği](https://www.w3.org/wiki/HTML/Elements/option) modelinizin özellikleri için öğeleri.</span><span class="sxs-lookup"><span data-stu-id="1277d-317">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="1277d-318">HTML Yardımcısı alternatif olan `Html.DropDownListFor` ve `Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="1277d-318">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="1277d-319">`Select Tag Helper` `asp-for` Model özellik adı belirtir [seçin](https://www.w3.org/wiki/HTML/Elements/select) öğesi ve `asp-items` belirtir [seçeneği](https://www.w3.org/wiki/HTML/Elements/option) öğeleri.</span><span class="sxs-lookup"><span data-stu-id="1277d-319">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="1277d-320">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1277d-320">For example:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="1277d-321">Örnek:</span><span class="sxs-lookup"><span data-stu-id="1277d-321">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="1277d-322">`Index` Yöntemi başlatır `CountryViewModel`, seçilen ülke ayarlar ve buna ileten `Index` görünümü.</span><span class="sxs-lookup"><span data-stu-id="1277d-322">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=8-13)]

<span data-ttu-id="1277d-323">HTTP POST `Index` yöntemi seçimi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="1277d-323">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="1277d-324">`Index` Görüntüle:</span><span class="sxs-lookup"><span data-stu-id="1277d-324">The `Index` view:</span></span>

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="1277d-325">Hangi ("CA" Seçili ile) aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1277d-325">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="1277d-326">Kullanımı önerilmemektedir `ViewBag` veya `ViewData` seçin etiketi Yardımcısı ile.</span><span class="sxs-lookup"><span data-stu-id="1277d-326">We don't recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="1277d-327">MVC meta verileri sağlayarak, daha güçlü ve genellikle daha az sorunlu bir görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="1277d-327">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="1277d-328">`asp-for` Öznitelik değeri özel bir durumdur ve gerektirmeyen bir `Model` önek, bir etiket Yardımcısı öznitelikleri yapın (gibi `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="1277d-328">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="1277d-329">Enum bağlama</span><span class="sxs-lookup"><span data-stu-id="1277d-329">Enum binding</span></span>

<span data-ttu-id="1277d-330">Genellikle kullanmak uygun olan `<select>` ile bir `enum` özelliği ve `SelectListItem` öğelerden `enum` değerleri.</span><span class="sxs-lookup"><span data-stu-id="1277d-330">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="1277d-331">Örnek:</span><span class="sxs-lookup"><span data-stu-id="1277d-331">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="1277d-332">`GetEnumSelectList` Yöntemi oluşturur bir `SelectList` nesne için bir sabit listesi.</span><span class="sxs-lookup"><span data-stu-id="1277d-332">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="1277d-333">Numaralandırıcı listenizi donatmak `Display` daha zengin bir kullanıcı Arabirimi almak için öznitelik:</span><span class="sxs-lookup"><span data-stu-id="1277d-333">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="1277d-334">Aşağıdaki HTML'yi oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="1277d-334">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="1277d-335">Seçenek grubu</span><span class="sxs-lookup"><span data-stu-id="1277d-335">Option Group</span></span>

<span data-ttu-id="1277d-336">HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) öğe bir veya daha fazla görünüm modeli içerdiğinde, oluşturulan `SelectListGroup` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="1277d-336">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="1277d-337">`CountryViewModelGroup` Grupları `SelectListItem` "Kuzey Amerika" ve "Avrupa" gruplara öğeleri:</span><span class="sxs-lookup"><span data-stu-id="1277d-337">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="1277d-338">İki grup aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="1277d-338">The two groups are shown below:</span></span>

![seçenek grubu örneği](working-with-forms/_static/grp.png)

<span data-ttu-id="1277d-340">Oluşturulan HTML:</span><span class="sxs-lookup"><span data-stu-id="1277d-340">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="1277d-341">Çoklu seçim</span><span class="sxs-lookup"><span data-stu-id="1277d-341">Multiple select</span></span>

<span data-ttu-id="1277d-342">Seçin etiketi Yardımcısı otomatik olarak oluşturacak [birden çok "birden çok" =](http://w3c.github.io/html-reference/select.html) özelliği belirtilen özniteliği `asp-for` özniteliği bir `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="1277d-342">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="1277d-343">Örneğin, şu model verilen:</span><span class="sxs-lookup"><span data-stu-id="1277d-343">For example, given the following model:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="1277d-344">Aşağıdaki görünüm ile:</span><span class="sxs-lookup"><span data-stu-id="1277d-344">With the following view:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="1277d-345">Aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1277d-345">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="1277d-346">Seçim yok</span><span class="sxs-lookup"><span data-stu-id="1277d-346">No selection</span></span>

<span data-ttu-id="1277d-347">Birden çok sayfada "belirtilmemiş" seçeneğini kullanarak kendiniz bulursanız, HTML yinelenen ortadan kaldırmak için bir şablon oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1277d-347">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="1277d-348">*Views/Shared/EditorTemplates/CountryViewModel.cshtml* şablonu:</span><span class="sxs-lookup"><span data-stu-id="1277d-348">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="1277d-349">HTML ekleme [ \<seçeneği >](https://www.w3.org/wiki/HTML/Elements/option) öğeleri sınırlı değildir *seçim* çalışması.</span><span class="sxs-lookup"><span data-stu-id="1277d-349">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements isn't limited to the *No selection* case.</span></span> <span data-ttu-id="1277d-350">Örneğin, aşağıdaki görünüm ve eylem yöntemi HTML yukarıdaki koda benzer oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1277d-350">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="1277d-351">Doğru `<option>` öğe seçilir (içeren `selected="selected"` özniteliği) geçerli bağlı olarak `Country` değeri.</span><span class="sxs-lookup"><span data-stu-id="1277d-351">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="1277d-352">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1277d-352">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* [<span data-ttu-id="1277d-353">HTML Form öğesi</span><span class="sxs-lookup"><span data-stu-id="1277d-353">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)
* [<span data-ttu-id="1277d-354">İstek doğrulama belirteci</span><span class="sxs-lookup"><span data-stu-id="1277d-354">Request Verification Token</span></span>](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* <xref:mvc/models/model-binding>
* <xref:mvc/models/validation>
* [<span data-ttu-id="1277d-355">IAttributeAdapter arabirimi</span><span class="sxs-lookup"><span data-stu-id="1277d-355">IAttributeAdapter Interface</span></span>](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [<span data-ttu-id="1277d-356">Bu belge için kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="1277d-356">Code snippets for this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
