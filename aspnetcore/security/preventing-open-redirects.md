---
title: "Bir ASP.NET Core uygulamada açık yeniden yönlendirme saldırılarını önleme | Microsoft Docs"
author: ardalis
description: "Açık yeniden yönlendirme saldırılarına karşı bir ASP.NET Core uygulama önlemek nasıl gösterir"
keywords: "ASP.NET Core, güvenlik, açık yeniden yönlendirme saldırısı"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: 4604e563-e91a-4ecd-b7ed-00b3f1eee2b5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: 4083845a77eb19d9ba9beb389a92ceb5c14edbde
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="f4c55-104">Bir ASP.NET Core uygulamada açık yeniden yönlendirme saldırılarını önleme</span><span class="sxs-lookup"><span data-stu-id="f4c55-104">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="f4c55-105">İstek sorgu dizesi veya form verileri gibi aracılığıyla belirtilen bir URL'ye yönlendiren bir web uygulaması olası kullanıcıları bir dış, kötü amaçlı URL'sine yeniden yönlendirmek için değiştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="f4c55-105">A web app that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="f4c55-106">Bu oynama açık yeniden yönlendirme saldırısı olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="f4c55-106">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="f4c55-107">Uygulama mantığınızın bir belirtilen URL'ye yeniden yönlendirilen olduğunda yeniden yönlendirme URL'sini oynanmadığını doğrulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-107">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="f4c55-108">ASP.NET Core uygulamaları açık (açık olarak da bilinen yeniden yönlendirme) yeniden yönlendirme saldırılarına karşı korumaya yardımcı olmak için yerleşik bir işleve sahiptir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-108">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="f4c55-109">Açık yeniden yönlendirme saldırının nedir?</span><span class="sxs-lookup"><span data-stu-id="f4c55-109">What is an open redirect attack?</span></span>

<span data-ttu-id="f4c55-110">Kimlik doğrulaması gerektiren kaynaklara erişirken web uygulamaları sık kullanıcılar bir oturum açma sayfasına yönlendir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-110">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="f4c55-111">Yeniden yönlendirme typlically içeren bir `returnUrl` sorgu dizesi parametresi böylece bunlar başarıyla oturum açtıktan sonra kullanıcı ilk olarak istenen URL'ye yeniden döndürülebilir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-111">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="f4c55-112">Kullanıcı kimlik doğrulaması yaptıktan sonra cihazın ilk olarak istedikleri URL yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-112">After the user authenticates, they are redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="f4c55-113">Hedef URL isteğinin sorgu dizesinde belirtildiğinden, kötü niyetli bir kullanıcı ile querystring değiştirmesine.</span><span class="sxs-lookup"><span data-stu-id="f4c55-113">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="f4c55-114">Değiştirilen bir sorgu dizesi, kullanıcı bir dış, kötü amaçlı sitesine yönlendirmek site izin verebilir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-114">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="f4c55-115">Bu teknik açık bir yeniden yönlendirme (veya yeniden yönlendirme) saldırısı olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="f4c55-115">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="f4c55-116">Örnek saldırının</span><span class="sxs-lookup"><span data-stu-id="f4c55-116">An example attack</span></span>

<span data-ttu-id="f4c55-117">Kötü niyetli bir kullanıcı, bir kullanıcının kimlik bilgileri veya uygulamanızdan hassas bilgileri kötü amaçlı kullanıcı erişmesine izin vermek için amaçlanan bir saldırı geliştirebilir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-117">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="f4c55-118">Saldırı başlatmak için kullanıcının, sitenin oturum açma sayfasına bir bağlantı ile'ı bunlar ikna bir `returnUrl` URL'ye eklenen sorgu dizesi değeri.</span><span class="sxs-lookup"><span data-stu-id="f4c55-118">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="f4c55-119">Örneğin, [NerdDinner.com](http://nerddinner.com) örnek uygulama (ASP.NET MVC için yazılan) bu tür bir oturum açma sayfasına burada içerir: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span><span class="sxs-lookup"><span data-stu-id="f4c55-119">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="f4c55-120">Saldırı sonra şu adımları izler:</span><span class="sxs-lookup"><span data-stu-id="f4c55-120">The attack then follows these steps:</span></span>

1. <span data-ttu-id="f4c55-121">Kullanıcı, bir bağlantı ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Not, ikinci URL'dir nerddi**n**er, değil nerddi**nn**er).</span><span class="sxs-lookup"><span data-stu-id="f4c55-121">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="f4c55-122">Kullanıcı başarıyla oturum.</span><span class="sxs-lookup"><span data-stu-id="f4c55-122">The user logs in successfully.</span></span>
3. <span data-ttu-id="f4c55-123">Kullanıcı için (site tarafından) yönlendirilir ``http://nerddiner.com/Account/LogOn`` (gerçek sitesine benzeyen kötü amaçlı site).</span><span class="sxs-lookup"><span data-stu-id="f4c55-123">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="f4c55-124">Kullanıcı yeniden oturum (kimlik bilgilerini site kötü amaçlı vermiş) ve gerçek sitenin yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-124">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="f4c55-125">Kullanıcı büyük olasılıkla, ilk oturum açma girişimi başarısız oldu düşünüyorsanız ve bunların ikincisi başarılı oldu.</span><span class="sxs-lookup"><span data-stu-id="f4c55-125">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="f4c55-126">Büyük olasılıkla farkında kalmaları kimlik bilgilerini tehlikeye.</span><span class="sxs-lookup"><span data-stu-id="f4c55-126">They'll most likely remain unaware their credentials have been compromised.</span></span>

![Açık yeniden yönlendirme saldırısına işlemi](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="f4c55-128">Oturum açma sayfaları ek olarak bazı siteler yeniden yönlendirme sayfaları veya uç noktaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="f4c55-128">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="f4c55-129">Uygulamanızı sahip açık bir yeniden yönlendirme içeren bir sayfa düşünün ``/Home/Redirect``.</span><span class="sxs-lookup"><span data-stu-id="f4c55-129">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="f4c55-130">Bir saldırgan, örneğin, giden e-posta iletisinde bir bağlantı oluşturabilir ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span><span class="sxs-lookup"><span data-stu-id="f4c55-130">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="f4c55-131">Tipik bir kullanıcıyı URL'de bakın ve site adınızı ile başlayan bakın.</span><span class="sxs-lookup"><span data-stu-id="f4c55-131">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="f4c55-132">Güvenen, bunlar bağlantıya tıklayarak.</span><span class="sxs-lookup"><span data-stu-id="f4c55-132">Trusting that, they will click the link.</span></span> <span data-ttu-id="f4c55-133">Açık yeniden yönlendirme sonra kullanıcı dilediğiniz aynı görünür, kimlik avı siteye gönderir ve kullanıcı büyük olasılıkla misiniz ne düşünüyorsanız için oturum açma adıdır, sitenizi.</span><span class="sxs-lookup"><span data-stu-id="f4c55-133">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="f4c55-134">Açık yeniden yönlendirme saldırılarına karşı koruma</span><span class="sxs-lookup"><span data-stu-id="f4c55-134">Protecting against open redirect attacks</span></span>

<span data-ttu-id="f4c55-135">Web uygulamaları geliştirirken, tüm kullanıcı tarafından sağlanan verileri güvenilmez olarak kabul eder.</span><span class="sxs-lookup"><span data-stu-id="f4c55-135">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="f4c55-136">URL içeriğine göre kullanıcı yönlendiren işlevselliği uygulamanız varsa, bu tür yeniden yönlendirmeleri yalnızca yerel olarak, uygulamanızın içinde (veya bilinen bir URL, sorgu dizesinde sağlanan olmayan herhangi bir URL adresi) yapıldığını emin olun.</span><span class="sxs-lookup"><span data-stu-id="f4c55-136">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="f4c55-137">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="f4c55-137">LocalRedirect</span></span>

<span data-ttu-id="f4c55-138">Kullanım ``LocalRedirect`` temel yardımcı yöntemini `Controller` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="f4c55-138">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="f4c55-139">``LocalRedirect``yerel olmayan URL'si belirtilmişse, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4c55-139">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="f4c55-140">Aksi takdirde, olduğu gibi davranır ``Redirect`` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f4c55-140">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="f4c55-141">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="f4c55-141">IsLocalUrl</span></span>

<span data-ttu-id="f4c55-142">Kullanım [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) yöntemi yönlendirmeden önce URL'leri test etmek için:</span><span class="sxs-lookup"><span data-stu-id="f4c55-142">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="f4c55-143">Aşağıdaki örnek, bir URL yeniden yönlendirme önce yerel olup olmadığını denetlemek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-143">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="f4c55-144">`IsLocalUrl` Yöntemi, kötü amaçlı bir siteye yanlışlıkla yönlendirildi kullanıcıları korur.</span><span class="sxs-lookup"><span data-stu-id="f4c55-144">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="f4c55-145">Yerel olmayan URL yerel bir URL beklenirken bir durumda sağlandığında sağlanan URL ayrıntılarını oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-145">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="f4c55-146">Yeniden yönlendirme günlüğü URL'leri yeniden yönlendirme saldırılarına tanılamalarına yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f4c55-146">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
