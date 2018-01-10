---
title: "ASP.NET Core içinde siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını önleme"
author: steve-smith
ms.author: riande
description: "ASP.NET Core içinde siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını önleme"
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/anti-request-forgery
ms.openlocfilehash: d7df8f91e88290509c8751a4b69804b60138846e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="a410d-103">ASP.NET Core içinde siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını önleme</span><span class="sxs-lookup"><span data-stu-id="a410d-103">Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core</span></span>

<span data-ttu-id="a410d-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a410d-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="a410d-105">Hangi saldırı sahteciliğe karşı koruma engellemez?</span><span class="sxs-lookup"><span data-stu-id="a410d-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="a410d-106">Siteler arası istek sahtekarlığı (olarak da bilinen XSRF veya CSRF, belirgin *bakın surf*) web barındırılan uygulamalar yapabildiği kötü amaçlı bir web sitesi etkilemek istemci tarayıcısına ve güvendiği bir web sitesi arasındaki etkileşim karşı bir saldırı Bu tarayıcı.</span><span class="sxs-lookup"><span data-stu-id="a410d-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="a410d-107">Web tarayıcıları bir web sitesi için kimlik doğrulama belirteçleri bazı türleri her istek ile otomatik olarak göndermek için bu saldırıların olası yapılır.</span><span class="sxs-lookup"><span data-stu-id="a410d-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="a410d-108">Bu yararlanma olarak da bilinen biçimidir bir *tek tıklatmayla saldırı* veya as *arabası oturum*, saldırı yararlanır kullanıcı daha önce oturum kimliği doğrulanmış.</span><span class="sxs-lookup"><span data-stu-id="a410d-108">This form of exploit is also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="a410d-109">CSRF saldırı örneği:</span><span class="sxs-lookup"><span data-stu-id="a410d-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="a410d-110">İçine bir kullanıcı oturum `www.example.com`, forms kimlik doğrulaması kullanma.</span><span class="sxs-lookup"><span data-stu-id="a410d-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="a410d-111">Sunucu kullanıcının kimliğini doğrular ve bir kimlik doğrulama tanımlama bilgisini içeren bir yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="a410d-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="a410d-112">Kullanıcı kötü amaçlı bir siteyi ziyaret eder.</span><span class="sxs-lookup"><span data-stu-id="a410d-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="a410d-113">Kötü amaçlı site aşağıdakine benzer bir HTML formuna içerir:</span><span class="sxs-lookup"><span data-stu-id="a410d-113">The malicious site contains an HTML form similar to the following:</span></span>

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

<span data-ttu-id="a410d-114">Form eylemi kötü amaçlı siteye değil savunmasız siteye yazılarını dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a410d-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="a410d-115">CSRF "siteler arası" parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="a410d-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="a410d-116">Kullanıcı gönder düğmesine tıklar.</span><span class="sxs-lookup"><span data-stu-id="a410d-116">The user clicks the submit button.</span></span> <span data-ttu-id="a410d-117">Tarayıcı, kimlik doğrulama tanımlama bilgisi istekle istenen etki alanı (Bu durumda savunmasız site) için otomatik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="a410d-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="a410d-118">İstek, kullanıcının kimlik doğrulaması bağlamı ile sunucuda çalışır ve kimliği doğrulanmış bir kullanıcı yapmak için izin verilen herhangi bir şey yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a410d-118">The request runs on the server with the user’s authentication context and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="a410d-119">Bu örnekte form düğmesini kullanıcıya gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a410d-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="a410d-120">Kötü amaçlı sayfası olabilir:</span><span class="sxs-lookup"><span data-stu-id="a410d-120">The malicious page could:</span></span>

* <span data-ttu-id="a410d-121">Otomatik olarak form gönderen bir komut dosyasını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a410d-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="a410d-122">Form gönderme AJAX isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="a410d-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="a410d-123">Gizli bir form CSS ile kullanın.</span><span class="sxs-lookup"><span data-stu-id="a410d-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="a410d-124">SSL kullanarak engellemez CSRF saldırı, zararlı site gönderebilirsiniz bir `https://` isteği.</span><span class="sxs-lookup"><span data-stu-id="a410d-124">Using SSL does not prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="a410d-125">Bazı saldırılar yanıt site uç noktaları hedef `GET` istekleri, hangi durumda resim etiketi (Bu saldırı biçimidir ortak görüntüleri izin ancak JavaScript engelleme Forumu sitelerinde) eylemi gerçekleştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a410d-125">Some attacks  target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="a410d-126">İle durum değişikliği uygulamaları `GET` istekleri kötü amaçlı saldırılara karşı savunmasız.</span><span class="sxs-lookup"><span data-stu-id="a410d-126">Applications that change state with `GET` requests are  vulnerable from malicious attacks.</span></span>

<span data-ttu-id="a410d-127">Tarayıcıların tüm ilgili tanımlama bilgilerini hedef web sitesine gönderdiği çünkü CSRF saldırılarına karşı kimlik doğrulaması için tanımlama bilgileri kullanan web siteleri mümkündür.</span><span class="sxs-lookup"><span data-stu-id="a410d-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="a410d-128">Ancak, CSRF saldırıları tanımlama bilgisinden faydalanmakta için sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="a410d-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="a410d-129">Örneğin, temel ve Özet kimlik doğrulaması da savunmasız.</span><span class="sxs-lookup"><span data-stu-id="a410d-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="a410d-130">Bir kullanıcının temel veya Özet kimlik doğrulaması ile oturum açtığı sonra oturumu sona kadar tarayıcı otomatik olarak kimlik bilgilerini gönderir.</span><span class="sxs-lookup"><span data-stu-id="a410d-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="a410d-131">Not: Bu bağlamda *oturum* sırasında kullanıcının kimliğinin istemci-tarafı oturumuna başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="a410d-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="a410d-132">Sunucu tarafı oturumlarını ilgisi yoktur veya [oturum Ara](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="a410d-132">It is unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="a410d-133">Kullanıcılar tarafından CSRF güvenlik açıklarına karşı önleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a410d-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="a410d-134">Bunları kullanmayı bitirdikten sonra web sitelerinde oturum.</span><span class="sxs-lookup"><span data-stu-id="a410d-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="a410d-135">Kendi tarayıcının tanımlama bilgilerini düzenli olarak temizleme.</span><span class="sxs-lookup"><span data-stu-id="a410d-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="a410d-136">Ancak, CSRF güvenlik açıkları temelde web uygulaması, son kullanıcı ile ilgili bir sorun değildir.</span><span class="sxs-lookup"><span data-stu-id="a410d-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="a410d-137">ASP.NET Core MVC CSRF nasıl ele?</span><span class="sxs-lookup"><span data-stu-id="a410d-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="a410d-138">ASP.NET Core uygulayan anti-request-sahte kullanarak [ASP.NET Core veri koruma yığını](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="a410d-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="a410d-139">ASP.NET Core veri koruma, bir sunucu grubunda çalışmak için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a410d-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="a410d-140">Bkz: [veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="a410d-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="a410d-141">ASP.NET Core anti-request-sahte varsayılan veri koruma yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a410d-141">ASP.NET Core anti-request-forgery  default data protection configuration</span></span> 

<span data-ttu-id="a410d-142">ASP.NET Core MVC 2.0 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) sahteciliğe karşı koruma belirteçleri için HTML form öğelerini yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="a410d-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="a410d-143">Örneğin, bir Razor dosyasının aşağıdaki biçimlendirmede sahteciliğe karşı koruma belirteçlerini otomatik olarak oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a410d-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="a410d-144">Otomatik olarak oluşturulmasını sahteciliğe karşı koruma belirteçleri için HTML form öğelerini olur zaman:</span><span class="sxs-lookup"><span data-stu-id="a410d-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="a410d-145">`form` Etiketinde `method="post"` özniteliği ve</span><span class="sxs-lookup"><span data-stu-id="a410d-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="a410d-146">Boş eylem özniteliğidir.</span><span class="sxs-lookup"><span data-stu-id="a410d-146">The action attribute is empty.</span></span> <span data-ttu-id="a410d-147">( `action=""`) VEYA</span><span class="sxs-lookup"><span data-stu-id="a410d-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="a410d-148">Action özniteliği belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="a410d-148">The action attribute is not supplied.</span></span> <span data-ttu-id="a410d-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="a410d-149">(`<form method="post">`)</span></span>

<span data-ttu-id="a410d-150">Sahteciliğe karşı koruma belirteçlerini otomatik olarak oluşturulmasını HTML form öğelerini tarafından için devre dışı bırakabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a410d-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="a410d-151">Açıkça devre dışı bırakma `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="a410d-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="a410d-152">Örneğin</span><span class="sxs-lookup"><span data-stu-id="a410d-152">For example</span></span>

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="a410d-153">Etiket Yardımcısını kullanarak form öğesi etiket Yardımcıları dışında opt [! çevirin simgesi](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="a410d-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

 ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="a410d-154">Kaldırma `FormTagHelper` görünümünden.</span><span class="sxs-lookup"><span data-stu-id="a410d-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="a410d-155">Kaldırabileceğiniz `FormTagHelper` Razor görünümüne aşağıdaki yönergesi ekleyerek görünümden:</span><span class="sxs-lookup"><span data-stu-id="a410d-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="a410d-156">[Razor sayfalarının](xref:mvc/razor-pages/index) XSRF/CSRF otomatik olarak korunur.</span><span class="sxs-lookup"><span data-stu-id="a410d-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="a410d-157">Herhangi bir ek kod yazmak zorunda değilsiniz.</span><span class="sxs-lookup"><span data-stu-id="a410d-157">You don't have to write any additional code.</span></span> <span data-ttu-id="a410d-158">Bkz: [XSRF/CSRF ve Razor sayfalarının](xref:mvc/razor-pages/index#xsrf) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="a410d-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="a410d-159">CSRF saldırılarına karşı savunma en yaygın Eşitleyici belirteci deseni (STP) yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="a410d-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="a410d-160">STP kullanıcı form verilerini içeren bir sayfa istediğinde kullanılan bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="a410d-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="a410d-161">Sunucu, istemci için geçerli kullanıcının kimliği ile ilişkili bir belirteç gönderir.</span><span class="sxs-lookup"><span data-stu-id="a410d-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="a410d-162">İstemci belirteci doğrulama için sunucuya geri gönderir.</span><span class="sxs-lookup"><span data-stu-id="a410d-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="a410d-163">Sunucunun kimliği doğrulanmış kullanıcının kimliğini eşleşmeyen bir belirteç alırsa, isteği reddedilir.</span><span class="sxs-lookup"><span data-stu-id="a410d-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="a410d-164">Belirteç, benzersiz ve tahmin edilemez.</span><span class="sxs-lookup"><span data-stu-id="a410d-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="a410d-165">Belirteç, bir dizi (sayfa 1 sağlama sayfası 3 önündeki sayfa 2 önündeki) istekleri uygun sıralama emin olmak için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a410d-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="a410d-166">ASP.NET Core MVC şablonları tüm formlarında antiforgery belirteçleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a410d-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="a410d-167">Aşağıdaki iki örnek görünümü mantığı antiforgery belirteçleri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a410d-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="a410d-168">Bir antiforgery belirteci açıkça ekleyebileceğiniz bir ``<form>`` etiket Yardımcıları ile HTML Yardımcısı kullanmadan öğesi ``@Html.AntiForgeryToken``:</span><span class="sxs-lookup"><span data-stu-id="a410d-168">You can explicitly add an antiforgery token to a ``<form>`` element without using tag helpers with the HTML helper ``@Html.AntiForgeryToken``:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="a410d-169">Her önceki durumlarda, ASP.NET Core aşağıdakine benzer bir gizli bir form alanı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a410d-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

<span data-ttu-id="a410d-170">ASP.NET Core içeren üç [filtreleri](xref:mvc/controllers/filters) antiforgery belirteçleri ile çalışmak için: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, ve ``IgnoreAntiforgeryToken``.</span><span class="sxs-lookup"><span data-stu-id="a410d-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, and ``IgnoreAntiforgeryToken``.</span></span>

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="a410d-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="a410d-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="a410d-172">``ValidateAntiForgeryToken`` Tek tek bir eylem, bir denetleyici uygulanabilir bir eylem filtresi veya genel olarak.</span><span class="sxs-lookup"><span data-stu-id="a410d-172">The ``ValidateAntiForgeryToken`` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="a410d-173">İsteğin geçerli bir antiforgery belirteci içermedikçe Bu filtre uygulanmış olan eylemler için yapılan istekleri engellenir.</span><span class="sxs-lookup"><span data-stu-id="a410d-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="a410d-174">``ValidateAntiForgeryToken`` Özniteliği eylem yöntemlerine onu süsler, dahil olmak üzere istekleri için bir belirteç gerektirir `HTTP GET` istekleri.</span><span class="sxs-lookup"><span data-stu-id="a410d-174">The ``ValidateAntiForgeryToken`` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="a410d-175">Geniş çapta uygularsanız, kendisiyle kılabilirsiniz ``IgnoreAntiforgeryToken`` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a410d-175">If you apply it broadly, you can override it with the ``IgnoreAntiforgeryToken`` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="a410d-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="a410d-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="a410d-177">ASP.NET Core uygulamaları antiforgery belirteçleri HTTP güvenli yöntemleri (GET, HEAD, seçenekleri ve izleme) için genellikle oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="a410d-177">ASP.NET Core apps generally do not generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="a410d-178">Kapsamlı uygulama yerine ``ValidateAntiForgeryToken`` özniteliği ve ile geçersiz kılma ``IgnoreAntiforgeryToken`` kullanabileceğiniz öznitelikleri ``AutoValidateAntiforgeryToken`` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a410d-178">Instead of broadly applying the ``ValidateAntiForgeryToken`` attribute and then overriding it with ``IgnoreAntiforgeryToken`` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="a410d-179">Bu öznitelik için aynı şekilde çalışır ``ValidateAntiForgeryToken`` için aşağıdaki HTTP yöntemleri kullanılarak yapılan istekleri belirteçleri gerektirmeyen dışında öznitelik:</span><span class="sxs-lookup"><span data-stu-id="a410d-179">This attribute works identically to the ``ValidateAntiForgeryToken`` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="a410d-180">AL</span><span class="sxs-lookup"><span data-stu-id="a410d-180">GET</span></span>
* <span data-ttu-id="a410d-181">HEAD</span><span class="sxs-lookup"><span data-stu-id="a410d-181">HEAD</span></span>
* <span data-ttu-id="a410d-182">SEÇENEKLER</span><span class="sxs-lookup"><span data-stu-id="a410d-182">OPTIONS</span></span>
* <span data-ttu-id="a410d-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="a410d-183">TRACE</span></span>

<span data-ttu-id="a410d-184">Şunu kullanmanızı öneririz ``AutoValidateAntiforgeryToken`` API olmayan senaryolar için kapsamlı.</span><span class="sxs-lookup"><span data-stu-id="a410d-184">We recommend you use ``AutoValidateAntiforgeryToken`` broadly for non-API scenarios.</span></span> <span data-ttu-id="a410d-185">Bu işlem sonrası eylemler varsayılan olarak korunan sağlar.</span><span class="sxs-lookup"><span data-stu-id="a410d-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="a410d-186">Varsayılan olarak, antiforgery belirteçleri yoksaymayı sürece alternatiftir ``ValidateAntiForgeryToken`` tek tek eylem yöntemine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a410d-186">The alternative is to ignore antiforgery tokens by default, unless ``ValidateAntiForgeryToken`` is applied to the individual action method.</span></span> <span data-ttu-id="a410d-187">Bu senaryoda olmasını POST eylem yöntemi için büyük olasılıkla korumasız, sol uygulamanızı CSRF saldırılara karşı savunmasız bırakır.</span><span class="sxs-lookup"><span data-stu-id="a410d-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="a410d-188">Hatta anonim GÖNDERİLERİ antiforgery belirteci göndermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a410d-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="a410d-189">Not: API'leri tanımlama bilgisi olmayan belirtecinin bir parçası göndermek için bir otomatik mekanizması gerekmez; Uygulamanız, büyük olasılıkla, istemci kodu uygulamanızı bağlı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a410d-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="a410d-190">Aşağıda bazı örnekler gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a410d-190">Some examples are shown below.</span></span>


<span data-ttu-id="a410d-191">Örnek (sınıf düzeyinde):</span><span class="sxs-lookup"><span data-stu-id="a410d-191">Example (class level):</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="a410d-192">Örnek (Genel):</span><span class="sxs-lookup"><span data-stu-id="a410d-192">Example (global):</span></span>

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="a410d-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="a410d-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="a410d-194">``IgnoreAntiforgeryToken`` Filtre, belirli bir eylem (veya denetleyicisi) olması için bir antiforgery belirteci gereksinimini ortadan kaldırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a410d-194">The ``IgnoreAntiforgeryToken`` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="a410d-195">Uygulandığında, bu filtre geçersiz kılar ``ValidateAntiForgeryToken`` ve/veya ``AutoValidateAntiforgeryToken`` daha yüksek bir düzeyde (genel olarak veya bir denetleyicisinde) belirtilen filtreler.</span><span class="sxs-lookup"><span data-stu-id="a410d-195">When applied, this filter will override ``ValidateAntiForgeryToken`` and/or ``AutoValidateAntiforgeryToken`` filters specified at a higher level (globally or on a controller).</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
  [HttpPost]
  [IgnoreAntiforgeryToken]
  public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
  {
    // no antiforgery token required
  }
}
```

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="a410d-196">JavaScript, AJAX ve SPAs</span><span class="sxs-lookup"><span data-stu-id="a410d-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="a410d-197">Geleneksel HTML tabanlı uygulamalarda antiforgery belirteçleri gizli form alanlarını kullanarak sunucuya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a410d-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="a410d-198">Modern JavaScript tabanlı uygulamalar ve tek sayfa uygulamaları (SPAs), birçok istek program aracılığıyla yapılır.</span><span class="sxs-lookup"><span data-stu-id="a410d-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="a410d-199">AJAX istekleri belirteç göndermek için başka teknikler (örneğin, istek üstbilgileri veya tanımlama bilgileri) kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="a410d-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="a410d-200">Tanımlama bilgileri kimlik doğrulama belirteçleri depolamak ve API isteklerinin sunucusunda kimlik doğrulaması için kullanılıyorsa, CSRF olası bir sorun olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a410d-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="a410d-201">Belirteç depolamak için yerel depolama alanı kullandıysanız, yerel depolama değerlerinden her yeni isteği sunucusuyla otomatik olarak gönderilmez beri ancak CSRF güvenlik açığı, azaltılması gereken.</span><span class="sxs-lookup"><span data-stu-id="a410d-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="a410d-202">Bu nedenle, istemci ve bir istek üstbilgisini önerilen yaklaşımdır olarak belirtecin gönderme antiforgery belirteci depolamak için yerel depolama kullanma.</span><span class="sxs-lookup"><span data-stu-id="a410d-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="a410d-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="a410d-203">AngularJS</span></span>

<span data-ttu-id="a410d-204">AngularJS CSRF adresine bir kuralı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a410d-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="a410d-205">Sunucu tanımlama bilgisi adı ile gönderirse ``XSRF-TOKEN``, Angular ``$http`` hizmet ekleyecek değeri bu tanımlama bilgisinden bir üst bilginin bu sunucu için bir istek gönderdiğinde.</span><span class="sxs-lookup"><span data-stu-id="a410d-205">If the server sends a cookie with the name ``XSRF-TOKEN``, the Angular ``$http`` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="a410d-206">Bu işlemi otomatiktir; Üstbilgi açıkça ayarlamanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a410d-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="a410d-207">Üstbilgi adı ``X-XSRF-TOKEN``.</span><span class="sxs-lookup"><span data-stu-id="a410d-207">The header name is ``X-XSRF-TOKEN``.</span></span> <span data-ttu-id="a410d-208">Sunucu, bu başlığı algılamak ve içeriğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a410d-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="a410d-209">ASP.NET Core API çalışmak için bu kural:</span><span class="sxs-lookup"><span data-stu-id="a410d-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="a410d-210">Adlı bir tanımlama bilgisine bir belirteç sağlamak için uygulamanızı yapılandırma``XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="a410d-210">Configure your app to provide a token in a cookie called ``XSRF-TOKEN``</span></span>
* <span data-ttu-id="a410d-211">Adında bir başlık aramak için antiforgery hizmetini yapılandırma``X-XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="a410d-211">Configure the antiforgery service to look for a header named ``X-XSRF-TOKEN``</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="a410d-212">[Görünüm örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="a410d-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="a410d-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a410d-213">JavaScript</span></span>

<span data-ttu-id="a410d-214">JavaScript görünümlerle kullanarak, görünümü içinde hizmetinden kullanarak belirteci oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a410d-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="a410d-215">Bunu yapmak için ekleme `Microsoft.AspNetCore.Antiforgery.IAntiforgery` görüntüleyebileceği ve çağırabileceği içine hizmet `GetAndStoreTokens`gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="a410d-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

<span data-ttu-id="a410d-216">Bu yaklaşım doğrudan sunucudan tanımlama bilgilerini ayarlama veya istemciden okuma uğraşmanız gereğini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="a410d-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="a410d-217">JavaScript ayrıca tanımlama bilgilerini sağlanan belirteçleri erişmek ve ardından tanımlama bilgisinin içeriği üstbilgi belirtecin değeri ile oluşturmak için aşağıda gösterildiği gibi kullanın.</span><span class="sxs-lookup"><span data-stu-id="a410d-217">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="a410d-218">Ardından, belirteci olarak adlandırılan bir üstbilgisinde göndermek için komut dosyanızı oluşturmaya varsayılarak istekleri ``X-CSRF-TOKEN``, aranacak antiforgery hizmetini yapılandırma ``X-CSRF-TOKEN`` üstbilgisi:</span><span class="sxs-lookup"><span data-stu-id="a410d-218">Then, assuming you construct your script requests to send the token in a header called ``X-CSRF-TOKEN``, configure the antiforgery service to look for the ``X-CSRF-TOKEN`` header:</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="a410d-219">Aşağıdaki örnek, uygun üstbilgiyle AJAX isteği yapmak için jQuery kullanır:</span><span class="sxs-lookup"><span data-stu-id="a410d-219">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a><span data-ttu-id="a410d-220">Antiforgery yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a410d-220">Configuring Antiforgery</span></span>

<span data-ttu-id="a410d-221">`IAntiforgery`antiforgery sistemi yapılandırmak için API sağlar.</span><span class="sxs-lookup"><span data-stu-id="a410d-221">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="a410d-222">İçinde istenebilir `Configure` yöntemi `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a410d-222">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="a410d-223">Aşağıdaki örnek, antiforgery bir belirteç oluşturmak ve yanıtta (yukarıda açıklanan varsayılan Açısal adlandırma kuralını kullanarak) bir tanımlama bilgisi olarak göndermek için uygulamanın giriş sayfasından ara yazılımını kullanır:</span><span class="sxs-lookup"><span data-stu-id="a410d-223">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a><span data-ttu-id="a410d-224">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="a410d-224">Options</span></span>

<span data-ttu-id="a410d-225">Özelleştirebileceğiniz [antiforgery seçenekleri](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) içinde `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a410d-225">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|<span data-ttu-id="a410d-226">Seçenek</span><span class="sxs-lookup"><span data-stu-id="a410d-226">Option</span></span>        | <span data-ttu-id="a410d-227">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a410d-227">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="a410d-228">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="a410d-228">CookieDomain</span></span>  | <span data-ttu-id="a410d-229">Tanımlama bilgisinin etki alanı.</span><span class="sxs-lookup"><span data-stu-id="a410d-229">The domain of the cookie.</span></span> <span data-ttu-id="a410d-230">Varsayılan olarak `null`.</span><span class="sxs-lookup"><span data-stu-id="a410d-230">Defaults to `null`.</span></span> |
|<span data-ttu-id="a410d-231">Tanımlamabilgisiadı</span><span class="sxs-lookup"><span data-stu-id="a410d-231">CookieName</span></span>    | <span data-ttu-id="a410d-232">Tanımlama bilgisinin adı.</span><span class="sxs-lookup"><span data-stu-id="a410d-232">The name of the cookie.</span></span> <span data-ttu-id="a410d-233">Ayarlanmadı, sistem ile başlayan bir benzersiz bir ad oluşturur `DefaultCookiePrefix` (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="a410d-233">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="a410d-234">CookiePath</span><span class="sxs-lookup"><span data-stu-id="a410d-234">CookiePath</span></span>    | <span data-ttu-id="a410d-235">Yolu tanımlama bilgisinde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a410d-235">The path set on the cookie.</span></span> |
|<span data-ttu-id="a410d-236">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="a410d-236">FormFieldName</span></span> | <span data-ttu-id="a410d-237">Görünümlerde antiforgery belirteçleri oluşturmak için antiforgery sistem tarafından kullanılan gizli form alanının adı.</span><span class="sxs-lookup"><span data-stu-id="a410d-237">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="a410d-238">HeaderName</span><span class="sxs-lookup"><span data-stu-id="a410d-238">HeaderName</span></span>    | <span data-ttu-id="a410d-239">Antiforgery sistem tarafından kullanılan üstbilginin adı.</span><span class="sxs-lookup"><span data-stu-id="a410d-239">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="a410d-240">Varsa `null`, sistem yalnızca form verilerini değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="a410d-240">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="a410d-241">requireSsl</span><span class="sxs-lookup"><span data-stu-id="a410d-241">RequireSsl</span></span>    | <span data-ttu-id="a410d-242">SSL antiforgery sistem tarafından gerekip gerekmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a410d-242">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="a410d-243">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="a410d-243">Defaults to `false`.</span></span> <span data-ttu-id="a410d-244">Varsa `true`, SSL olmayan istekleri başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="a410d-244">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="a410d-245">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="a410d-245">SuppressXFrameOptionsHeader</span></span>  | <span data-ttu-id="a410d-246">Nesil engellenip engellenmeyeceğini belirtir `X-Frame-Options` üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="a410d-246">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="a410d-247">Varsayılan olarak, "SAMEORIGIN" değerine sahip üstbilgi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a410d-247">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="a410d-248">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="a410d-248">Defaults to `false`.</span></span> |

<span data-ttu-id="a410d-249">https://docs.microsoft.com/ASPNET/Core/api/Microsoft.aspnetcore.Builder.cookieauthenticationoptions daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="a410d-249">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="a410d-250">Antiforgery genişletme</span><span class="sxs-lookup"><span data-stu-id="a410d-250">Extending Antiforgery</span></span>

<span data-ttu-id="a410d-251">[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) türü her belirteci ek veriler gidiş tarafından anti-XSRF sistem davranışını genişletmek geliştiricilere sağlar.</span><span class="sxs-lookup"><span data-stu-id="a410d-251">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="a410d-252">[GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) yöntemi her çağrıldığında bir alan belirteci oluşturulur ve dönüş değeri içinde oluşturulan belirteç katıştırılır.</span><span class="sxs-lookup"><span data-stu-id="a410d-252">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="a410d-253">Bir uygulayan bir zaman damgası, nonce veya başka bir değer döndürür ve ardından arama [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) belirteç doğrulandığında bu verileri doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="a410d-253">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="a410d-254">Bu yüzden bu bilgiyi içer gerek yoktur istemcinin kullanıcı adı zaten oluşturulan belirteçlere katıştırılır.</span><span class="sxs-lookup"><span data-stu-id="a410d-254">The client's username is already embedded in the generated tokens, so there is no need to include this information.</span></span> <span data-ttu-id="a410d-255">Bir belirteci ek veriler ancak hiçbir içeriyorsa `IAntiForgeryAdditionalDataProvider` bırakıldı yapılandırıldıysa, ek veriler doğrulanmazsa.</span><span class="sxs-lookup"><span data-stu-id="a410d-255">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data is not validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="a410d-256">Temelleri</span><span class="sxs-lookup"><span data-stu-id="a410d-256">Fundamentals</span></span>

<span data-ttu-id="a410d-257">Bu etki alanına yapılan her isteği bir etki alanı ile ilişkili tanımlama bilgileri gönderme varsayılan tarayıcı davranışını CSRF saldırıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="a410d-257">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="a410d-258">Bu tanımlama bilgileri tarayıcı içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="a410d-258">These cookies are stored within the browser.</span></span> <span data-ttu-id="a410d-259">Kimliği doğrulanmış kullanıcılar için oturum tanımlama bilgileri sık içerirler.</span><span class="sxs-lookup"><span data-stu-id="a410d-259">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="a410d-260">Tanımlama bilgisi tabanlı kimlik doğrulaması, kimlik doğrulama popüler şeklidir.</span><span class="sxs-lookup"><span data-stu-id="a410d-260">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="a410d-261">Belirteç tabanlı kimlik doğrulama sistemleriyle popülerliği özellikle SPAs ve diğer "akıllı istemci" senaryoları için büyüyen.</span><span class="sxs-lookup"><span data-stu-id="a410d-261">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="a410d-262">Tanımlama bilgisi tabanlı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="a410d-262">Cookie-based authentication</span></span>

<span data-ttu-id="a410d-263">Bir kullanıcı, kullanıcı adı ve parolasını kullanarak kimliğini doğrulamasından sonra bunları belirlemek ve bunlar doğrulanan olduğunu doğrulamak için kullanılan bir belirteç alacakları.</span><span class="sxs-lookup"><span data-stu-id="a410d-263">Once a user has authenticated using their username and password, they are issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="a410d-264">Her istek istemci eşlik bir tanımlama bilgisi getirir belirteç depolanır.</span><span class="sxs-lookup"><span data-stu-id="a410d-264">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="a410d-265">Oluşturma ve bu tanımlama bilgisi doğrulama tanımlama bilgisi kimlik doğrulaması ara yazılım tarafından gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a410d-265">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="a410d-266">ASP.NET Core sağlar tanımlama bilgisi [ara yazılımı](../fundamentals/middleware.md) , kullanıcı asıl şifrelenmiş bir tanımlama bilgisine serileştirir ve daha sonra sonraki isteklerde tanımlama bilgisini doğrular asıl yeniden oluşturur ve atar `User` özelliği `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="a410d-266">ASP.NET Core provides cookie [middleware](../fundamentals/middleware.md) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="a410d-267">Bir tanımlama bilgisi kullanıldığında, kimlik doğrulama tanımlama bilgisini bir form kimlik doğrulaması bileti için kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="a410d-267">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="a410d-268">Raporu her istek ile form kimlik doğrulaması tanımlama bilgisinin değeri olarak geçirilir ve sunucuda, form kimlik doğrulaması tarafından kimliği doğrulanmış bir kullanıcıyı tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a410d-268">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="a410d-269">Bir kullanıcı bir sistemde oturum açtığında, kullanıcı oturumunu sunucu tarafında oluşturulur ve bir veritabanı veya başka bir kalıcı deposunda saklanır.</span><span class="sxs-lookup"><span data-stu-id="a410d-269">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="a410d-270">Sistem, veri deposunda gerçek oturumuna işaret eden bir oturum anahtarı oluşturur ve istemci tarafı tanımlama bilgisi olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a410d-270">The system generates a session key that points to the actual session in the data store and it is sent as a client side cookie.</span></span> <span data-ttu-id="a410d-271">Web sunucusu bu oturum anahtarı bir kullanıcı yetkilendirme gerektiren kaynak istekleri her zaman kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="a410d-271">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="a410d-272">Sistem, ilişkili kullanıcı oturumunu istenen kaynağa erişme ayrıcalığına sahip olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="a410d-272">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="a410d-273">Bu durumda, istek devam eder.</span><span class="sxs-lookup"><span data-stu-id="a410d-273">If so, the request continues.</span></span> <span data-ttu-id="a410d-274">Aksi takdirde, istek yetkili değil olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="a410d-274">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="a410d-275">Bu yaklaşım durum bilgisi olarak görünen uygulama yapmak için kullanılan tanımlama bilgileri, "unutmayın mümkün" olduğundan kullanıcı daha önce sunucuyla doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="a410d-275">In this approach, cookies are used to make the application appear to be stateful, since it is able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="a410d-276">Kullanıcı belirteçleri</span><span class="sxs-lookup"><span data-stu-id="a410d-276">User tokens</span></span>

<span data-ttu-id="a410d-277">Belirteç tabanlı kimlik doğrulaması oturum sunucuda depolamak değil.</span><span class="sxs-lookup"><span data-stu-id="a410d-277">Token-based authentication doesn’t store session on the server.</span></span> <span data-ttu-id="a410d-278">Bunun yerine, bir kullanıcı oturum açtığında bir belirteç (antiforgery bir belirteç değil) verilir.</span><span class="sxs-lookup"><span data-stu-id="a410d-278">Instead, when a user is logged in they are issued a token (not an antiforgery token).</span></span> <span data-ttu-id="a410d-279">Bu belirteç belirteci doğrulamak için gerekli tüm verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="a410d-279">This token holds all the data that is required to validate the token.</span></span> <span data-ttu-id="a410d-280">Ayrıca biçiminde kullanıcı bilgilerini içeren [talep](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="a410d-280">It also contains user information, in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="a410d-281">Bir kullanıcı kimlik doğrulaması gerektiren bir sunucu kaynağa erişmek istediğinde, belirteç taşıyıcı {belirteci} biçiminde bir ek authorization üstbilgisi sunucusuyla gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a410d-281">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="a410d-282">Sonraki her istek için sunucu tarafı doğrulama istekte belirteç geçirilen beri bu uygulamayı durum bilgisiz hale getirir.</span><span class="sxs-lookup"><span data-stu-id="a410d-282">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="a410d-283">Bu belirteç değil *şifrelenmiş*; bunun yerine olan *kodlanmış*.</span><span class="sxs-lookup"><span data-stu-id="a410d-283">This token is not *encrypted*; rather it is *encoded*.</span></span> <span data-ttu-id="a410d-284">Sunucu tarafında belirteç, belirtecin içinde ham bilgilerine erişmek için çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="a410d-284">On the server-side the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="a410d-285">Belirteç içinde sonraki istekleri göndermek için ya da, tarayıcının yerel depolama biriminde veya bir tanımlama bilgisinde saklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a410d-285">To send the token in subsequent requests, you can either store it in browser’s local storage or in a cookie.</span></span> <span data-ttu-id="a410d-286">Belirtecinizi yerel depolama alanına depolanır, ancak belirteç bir tanımlama bilgisinde depolanıyorsa, bir sorun olduğundan XSRF güvenlik açığı hakkında endişelenmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a410d-286">You don’t have to worry about XSRF vulnerability if your token is stored in the local storage, but it is an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="a410d-287">Bir etki alanında barındırılan birden çok uygulamalarını</span><span class="sxs-lookup"><span data-stu-id="a410d-287">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="a410d-288">Olsa bile `example1.cloudapp.net` ve `example2.cloudapp.net` farklı ana altındaki tüm konaklarda arasında örtük güven ilişkisi yoktur `*.cloudapp.net` etki alanı.</span><span class="sxs-lookup"><span data-stu-id="a410d-288">Even though `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there is an implicit trust relationship between all hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="a410d-289">Bu örtük güven ilişkisi, büyük olasılıkla güvenilmeyen ana (AJAX istekleri yöneten kaynak aynı ilkeleri mutlaka HTTP tanımlama bilgileri için geçerli değildir) birbirlerinin tanımlama bilgilerini etkileyen olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a410d-289">This implicit trust relationship allows potentially untrusted hosts to affect each other’s cookies (the same-origin policies that govern AJAX requests do not necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="a410d-290">Kötü amaçlı bir alt etki alanı oturum belirteci üzerine mümkün olsa bile, kullanıcı için geçerli bir alan belirteci oluşturamıyor gerçekleşmesi için kullanıcı adı alanı belirteci katıştırılır, ASP.NET çekirdeği çalışma zamanı bazı azaltma sağlar.</span><span class="sxs-lookup"><span data-stu-id="a410d-290">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token, so even if a malicious subdomain is able to overwrite a session token it will be unable to generate a valid field token for the user.</span></span> <span data-ttu-id="a410d-291">Ancak, bu tür bir ortamda barındırıldığında yerleşik anti-XSRF yordamlar hala oturumu ele geçirme veya oturum açma CSRF karşı saldırıları korumaya olamaz.</span><span class="sxs-lookup"><span data-stu-id="a410d-291">However, when hosted in such an environment the built-in anti-XSRF routines still cannot defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="a410d-292">Paylaşılan barındırma ortamları oturumu ele geçirme, oturum açma CSRF ve diğer saldırılara vunerable ' dir.</span><span class="sxs-lookup"><span data-stu-id="a410d-292">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="a410d-293">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a410d-293">Additional Resources</span></span>

* <span data-ttu-id="a410d-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) üzerinde [Web uygulaması güvenlik projeyi açın](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="a410d-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
