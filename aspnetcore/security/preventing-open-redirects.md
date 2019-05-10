---
title: ASP.NET core'da açık yeniden yönlendirme saldırılarını önleme
author: ardalis
description: ASP.NET Core uygulaması karşı açık yeniden yönlendirme saldırılarını önlemek nasıl gösterir
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 0896189d2caaccb19647eb7c6d57f29dfc0290dd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898301"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>ASP.NET core'da açık yeniden yönlendirme saldırılarını önleme

Sorgu dizesi veya form verileri gibi isteği aracılığıyla belirtilen bir URL'ye yönlendiren bir web uygulaması büyük olasılıkla kullanıcıların dış, kötü amaçlı bir URL'ye yönlendirilmesini bozuabilir. Bu izinsiz bir açık yeniden yönlendirme saldırısı olarak adlandırılır.

Uygulama mantığınızın bir belirtilen URL'ye yeniden yönlendirir olduğunda, yeniden yönlendirme URL'sini kurcalanmadığı doğrulamanız gerekir. ASP.NET Core uygulamaları (yeniden yönlendirme açık olarak da bilinir) açık yeniden yönlendirme saldırılarına karşı korumaya yardımcı olmak için yerleşik bir işleve sahiptir.

## <a name="what-is-an-open-redirect-attack"></a>Açık yeniden yönlendirme saldırının nedir?

Kimlik doğrulaması gerektiren kaynaklara erişirken web uygulamaları kullanıcıların sık bir oturum açma sayfasına yönlendirir. Yeniden yönlendirme genellikle içeren bir `returnUrl` querystring parametresi böylece bunlar başarıyla oturum açtıktan sonra başta istenen URL'ye kullanıcı döndürülebilir. Kullanıcı kimlik doğrulaması yaptıktan sonra bunlar ilk olarak istedikleri URL'ye yönlendirilirsiniz.

Hedef URL isteğinin sorgu dizesinde belirtildiğinden, kötü niyetli bir kullanıcı sorgu dizesini ile neden. Değiştirilen bir sorgu dizesi, kullanıcı bir harici, kötü amaçlı sitesine yönlendirmek site izin verebilir. Bu teknik, bir açık yeniden yönlendirme (veya yeniden yönlendirme) saldırısı olarak adlandırılır.

### <a name="an-example-attack"></a>Bir örnek saldırı

Kötü niyetli bir kullanıcı, bir kullanıcının kimlik bilgilerine veya hassas bilgileri kötü niyetli bir kullanıcı erişime izin vermeye yönelik bir saldırı geliştirebilirsiniz. Kötü amaçlı kullanıcı saldırı başlatmak için sitenizin oturum açma sayfasına bir bağlantıya tıklayabilecekleri kullanıcı convinces bir `returnUrl` URL'ye eklenen sorgu dizesi değeri. Örneğin, bir uygulamanın düşünün `contoso.com` içeren bir oturum açma sayfasına `http://contoso.com/Account/LogOn?returnUrl=/Home/About`. Saldırı şu adımlardan oluşur:

1. Kullanıcı kötü amaçlı bir bağlantıyı tıkladığında `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (ikinci URL "contoso**1**.com", yok "contoso.com").
2. Kullanıcı başarıyla oturum.
3. (Site tarafından) yönlendireceği kullanıcı `http://contoso1.com/Account/LogOn` (tam olarak gerçek site gibi görünen bir kötü niyetli site).
4. Kullanıcının yeniden oturum açması (kimlik bilgilerini site kötü amaçlı vererek) ve gerçek sitede yeniden yönlendirilir.

Kullanıcının büyük olasılıkla, ilk oturum açma girişimi başarısız olduğunu ve bunların ikinci denemesi başarılı olduğunu düşündüğü. Kullanıcı, büyük olasılıkla farkında kimlik bilgilerini tehlikede olduğunu kalır.

![Açık yeniden yönlendirme saldırısına işlemi](preventing-open-redirects/_static/open-redirection-attack-process.png)

Oturum açma sayfalarına ek olarak bazı siteler yeniden yönlendirme sayfaları veya uç noktaları sağlar. Uygulamanız bir açık yeniden yönlendirme içeren bir sayfa sahip Imagine `/Home/Redirect`. Bir saldırgan, örneğin, bir bağlantı, giden e-posta oluşturabilir `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`. Tipik bir kullanıcı URL'SİNDE görüneceğini ve site adınızı ile başlayan bakın. Güvenen, bunlar bağlantıya tıklayın. Açık yeniden yönlendirme kullanıcı sizin için aynı görünür, kimlik avı siteye gönderir ve kullanıcı büyük olasılıkla misiniz ne düşünüyorsanız için oturum açma bilgileri ise sitenizi.

## <a name="protecting-against-open-redirect-attacks"></a>Açık yeniden yönlendirme saldırılarına karşı koruma

Web uygulamaları geliştirirken, kullanıcı tarafından sağlanan tüm verileri güvenilmez olarak kabul eder. Uygulamanızın URL'sini içeriğine göre kullanıcı yönlendiren işlevi varsa, bu tür yeniden yönlendirmeler yalnızca yerel olarak uygulamanızın içinde (veya bilinen bir URL, sorgu dizesinde sağlanan değil herhangi bir URL) yapıldığını emin olun.

### <a name="localredirect"></a>LocalRedirect

Kullanım `LocalRedirect` yardımcı yöntem tabanından `Controller` sınıfı:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect` yerel olmayan URL'si belirtilmişse bir özel durum oluşturur. Aksi takdirde, olduğu gibi davranır `Redirect` yöntemi.

### <a name="islocalurl"></a>IsLocalUrl

Kullanım [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) URL yeniden yönlendirme önce test etme yöntemi:

Aşağıdaki örnek, bir URL yeniden yönlendirme önce yerel olup olmadığını denetlemek gösterilmektedir.

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

`IsLocalUrl` Yöntemi, kötü amaçlı bir siteye yanlışlıkla yönlendirilen kullanıcıları korur. Yerel olmayan bir URL yerel bir URL beklenirken bir durumda sağlandığında sağlanan URL ayrıntılarını oturum açabilirsiniz. Günlüğü yeniden yönlendirme URL'lerini yeniden yönlendirme saldırılarını tanılamaya yardımcı olabilir.
