---
title: ASP.NET core'da açık yeniden yönlendirme saldırılarını önleme
author: ardalis
description: ASP.NET Core uygulaması karşı açık yeniden yönlendirme saldırılarını önlemek nasıl gösterir
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 9d8cac8708fe9aeadba5af1287362a20df7f6bfe
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815493"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a><span data-ttu-id="8670a-103">ASP.NET core'da açık yeniden yönlendirme saldırılarını önleme</span><span class="sxs-lookup"><span data-stu-id="8670a-103">Prevent open redirect attacks in ASP.NET Core</span></span>

<span data-ttu-id="8670a-104">Sorgu dizesi veya form verileri gibi isteği aracılığıyla belirtilen bir URL'ye yönlendiren bir web uygulaması büyük olasılıkla kullanıcıların dış, kötü amaçlı bir URL'ye yönlendirilmesini bozuabilir.</span><span class="sxs-lookup"><span data-stu-id="8670a-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="8670a-105">Bu izinsiz bir açık yeniden yönlendirme saldırısı olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="8670a-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="8670a-106">Uygulama mantığınızın bir belirtilen URL'ye yeniden yönlendirir olduğunda, yeniden yönlendirme URL'sini kurcalanmadığı doğrulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8670a-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="8670a-107">ASP.NET Core uygulamaları (yeniden yönlendirme açık olarak da bilinir) açık yeniden yönlendirme saldırılarına karşı korumaya yardımcı olmak için yerleşik bir işleve sahiptir.</span><span class="sxs-lookup"><span data-stu-id="8670a-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="8670a-108">Açık yeniden yönlendirme saldırının nedir?</span><span class="sxs-lookup"><span data-stu-id="8670a-108">What is an open redirect attack?</span></span>

<span data-ttu-id="8670a-109">Kimlik doğrulaması gerektiren kaynaklara erişirken web uygulamaları kullanıcıların sık bir oturum açma sayfasına yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="8670a-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="8670a-110">Yeniden yönlendirme genellikle içeren bir `returnUrl` querystring parametresi böylece bunlar başarıyla oturum açtıktan sonra başta istenen URL'ye kullanıcı döndürülebilir.</span><span class="sxs-lookup"><span data-stu-id="8670a-110">The redirection typically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="8670a-111">Kullanıcı kimlik doğrulaması yaptıktan sonra bunlar ilk olarak istedikleri URL'ye yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8670a-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="8670a-112">Hedef URL isteğinin sorgu dizesinde belirtildiğinden, kötü niyetli bir kullanıcı sorgu dizesini ile neden.</span><span class="sxs-lookup"><span data-stu-id="8670a-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="8670a-113">Değiştirilen bir sorgu dizesi, kullanıcı bir harici, kötü amaçlı sitesine yönlendirmek site izin verebilir.</span><span class="sxs-lookup"><span data-stu-id="8670a-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="8670a-114">Bu teknik, bir açık yeniden yönlendirme (veya yeniden yönlendirme) saldırısı olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="8670a-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="8670a-115">Bir örnek saldırı</span><span class="sxs-lookup"><span data-stu-id="8670a-115">An example attack</span></span>

<span data-ttu-id="8670a-116">Kötü niyetli bir kullanıcı, bir kullanıcının kimlik bilgilerine veya hassas bilgileri kötü niyetli bir kullanıcı erişime izin vermeye yönelik bir saldırı geliştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8670a-116">A malicious user can develop an attack intended to allow the malicious user access to a user's credentials or sensitive information.</span></span> <span data-ttu-id="8670a-117">Kötü amaçlı kullanıcı saldırı başlatmak için sitenizin oturum açma sayfasına bir bağlantıya tıklayabilecekleri kullanıcı convinces bir `returnUrl` URL'ye eklenen sorgu dizesi değeri.</span><span class="sxs-lookup"><span data-stu-id="8670a-117">To begin the attack, the malicious user convinces the user to click a link to your site's login page with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="8670a-118">Örneğin, bir uygulamanın düşünün `contoso.com` içeren bir oturum açma sayfasına `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="8670a-118">For example, consider an app at `contoso.com` that includes a login page at `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span></span> <span data-ttu-id="8670a-119">Saldırı şu adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="8670a-119">The attack follows these steps:</span></span>

1. <span data-ttu-id="8670a-120">Kullanıcı kötü amaçlı bir bağlantıyı tıkladığında `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (ikinci URL "contoso**1**.com", yok "contoso.com").</span><span class="sxs-lookup"><span data-stu-id="8670a-120">The user clicks a malicious link to `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (the second URL is "contoso**1**.com", not "contoso.com").</span></span>
2. <span data-ttu-id="8670a-121">Kullanıcı başarıyla oturum.</span><span class="sxs-lookup"><span data-stu-id="8670a-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="8670a-122">(Site tarafından) yönlendireceği kullanıcı `http://contoso1.com/Account/LogOn` (tam olarak gerçek site gibi görünen bir kötü niyetli site).</span><span class="sxs-lookup"><span data-stu-id="8670a-122">The user is redirected (by the site) to `http://contoso1.com/Account/LogOn` (a malicious site that looks exactly like real site).</span></span>
4. <span data-ttu-id="8670a-123">Kullanıcının yeniden oturum açması (kimlik bilgilerini site kötü amaçlı vererek) ve gerçek sitede yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8670a-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="8670a-124">Kullanıcının büyük olasılıkla, ilk oturum açma girişimi başarısız olduğunu ve bunların ikinci denemesi başarılı olduğunu düşündüğü.</span><span class="sxs-lookup"><span data-stu-id="8670a-124">The user likely believes that their first attempt to log in failed and that their second attempt is successful.</span></span> <span data-ttu-id="8670a-125">Kullanıcı, büyük olasılıkla farkında kimlik bilgilerini tehlikede olduğunu kalır.</span><span class="sxs-lookup"><span data-stu-id="8670a-125">The user most likely remains unaware that their credentials are compromised.</span></span>

![Açık yeniden yönlendirme saldırısına işlemi](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="8670a-127">Oturum açma sayfalarına ek olarak bazı siteler yeniden yönlendirme sayfaları veya uç noktaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="8670a-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="8670a-128">Uygulamanız bir açık yeniden yönlendirme içeren bir sayfa sahip Imagine `/Home/Redirect`.</span><span class="sxs-lookup"><span data-stu-id="8670a-128">Imagine your app has a page with an open redirect, `/Home/Redirect`.</span></span> <span data-ttu-id="8670a-129">Bir saldırgan, örneğin, bir bağlantı, giden e-posta oluşturabilir `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span><span class="sxs-lookup"><span data-stu-id="8670a-129">An attacker could create, for example, a link in an email that goes to `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span></span> <span data-ttu-id="8670a-130">Tipik bir kullanıcı URL'SİNDE görüneceğini ve site adınızı ile başlayan bakın.</span><span class="sxs-lookup"><span data-stu-id="8670a-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="8670a-131">Güvenen, bunlar bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8670a-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="8670a-132">Açık yeniden yönlendirme kullanıcı sizin için aynı görünür, kimlik avı siteye gönderir ve kullanıcı büyük olasılıkla misiniz ne düşünüyorsanız için oturum açma bilgileri ise sitenizi.</span><span class="sxs-lookup"><span data-stu-id="8670a-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="8670a-133">Açık yeniden yönlendirme saldırılarına karşı koruma</span><span class="sxs-lookup"><span data-stu-id="8670a-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="8670a-134">Web uygulamaları geliştirirken, kullanıcı tarafından sağlanan tüm verileri güvenilmez olarak kabul eder.</span><span class="sxs-lookup"><span data-stu-id="8670a-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="8670a-135">Uygulamanızın URL'sini içeriğine göre kullanıcı yönlendiren işlevi varsa, bu tür yeniden yönlendirmeler yalnızca yerel olarak uygulamanızın içinde (veya bilinen bir URL, sorgu dizesinde sağlanan değil herhangi bir URL) yapıldığını emin olun.</span><span class="sxs-lookup"><span data-stu-id="8670a-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="8670a-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="8670a-136">LocalRedirect</span></span>

<span data-ttu-id="8670a-137">Kullanım `LocalRedirect` yardımcı yöntem tabanından `Controller` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="8670a-137">Use the `LocalRedirect` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="8670a-138">`LocalRedirect` yerel olmayan URL'si belirtilmişse bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8670a-138">`LocalRedirect` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="8670a-139">Aksi takdirde, olduğu gibi davranır `Redirect` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8670a-139">Otherwise, it behaves just like the `Redirect` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="8670a-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="8670a-140">IsLocalUrl</span></span>

<span data-ttu-id="8670a-141">Kullanım [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) URL yeniden yönlendirme önce test etme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8670a-141">Use the [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="8670a-142">Aşağıdaki örnek, bir URL yeniden yönlendirme önce yerel olup olmadığını denetlemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8670a-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

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

<span data-ttu-id="8670a-143">`IsLocalUrl` Yöntemi, kötü amaçlı bir siteye yanlışlıkla yönlendirilen kullanıcıları korur.</span><span class="sxs-lookup"><span data-stu-id="8670a-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="8670a-144">Yerel olmayan bir URL yerel bir URL beklenirken bir durumda sağlandığında sağlanan URL ayrıntılarını oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8670a-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="8670a-145">Günlüğü yeniden yönlendirme URL'lerini yeniden yönlendirme saldırılarını tanılamaya yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="8670a-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
