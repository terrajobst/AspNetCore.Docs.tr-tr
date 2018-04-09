---
title: ASP.NET Core açık yeniden yönlendirme saldırılarına engelle
author: ardalis
description: Açık yeniden yönlendirme saldırılarına karşı bir ASP.NET Core uygulama önlemek nasıl gösterir
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/preventing-open-redirects
ms.openlocfilehash: 4a210b8bb8091e7c036d4bc98306e3b3f90d7d46
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>ASP.NET Core açık yeniden yönlendirme saldırılarına engelle

İstek sorgu dizesi veya form verileri gibi aracılığıyla belirtilen bir URL'ye yönlendiren bir web uygulaması olası kullanıcıları bir dış, kötü amaçlı URL'sine yeniden yönlendirmek için değiştirilmiş. Bu oynama açık yeniden yönlendirme saldırısı olarak adlandırılır.

Uygulama mantığınızın bir belirtilen URL'ye yeniden yönlendirilen olduğunda yeniden yönlendirme URL'sini oynanmadığını doğrulamanız gerekir. ASP.NET Core uygulamaları açık (açık olarak da bilinen yeniden yönlendirme) yeniden yönlendirme saldırılarına karşı korumaya yardımcı olmak için yerleşik bir işleve sahiptir.

## <a name="what-is-an-open-redirect-attack"></a>Açık yeniden yönlendirme saldırının nedir?

Kimlik doğrulaması gerektiren kaynaklara erişirken web uygulamaları sık kullanıcılar bir oturum açma sayfasına yönlendir. Yeniden yönlendirme typlically içeren bir `returnUrl` sorgu dizesi parametresi böylece bunlar başarıyla oturum açtıktan sonra kullanıcı ilk olarak istenen URL'ye yeniden döndürülebilir. Kullanıcı kimlik doğrulaması yaptıktan sonra bunlar ilk olarak istedikleri URL yönlendirilirsiniz.

Hedef URL isteğinin sorgu dizesinde belirtildiğinden, kötü niyetli bir kullanıcı ile querystring değiştirmesine. Değiştirilen bir sorgu dizesi, kullanıcı bir dış, kötü amaçlı sitesine yönlendirmek site izin verebilir. Bu teknik açık bir yeniden yönlendirme (veya yeniden yönlendirme) saldırısı olarak adlandırılır.

### <a name="an-example-attack"></a>Örnek saldırının

Kötü niyetli bir kullanıcı, bir kullanıcının kimlik bilgileri veya uygulamanızdan hassas bilgileri kötü amaçlı kullanıcı erişmesine izin vermek için amaçlanan bir saldırı geliştirebilir. Saldırı başlatmak için kullanıcının, sitenin oturum açma sayfasına bir bağlantı ile'ı bunlar ikna bir `returnUrl` URL'ye eklenen sorgu dizesi değeri. Örneğin, [NerdDinner.com](http://nerddinner.com) örnek uygulama (ASP.NET MVC için yazılan) bu tür bir oturum açma sayfasına burada içerir: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``. Saldırı sonra şu adımları izler:

1. Kullanıcı, bir bağlantı ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Not, ikinci URL'dir nerddi**n**er, değil nerddi**nn**er).
2. Kullanıcı başarıyla oturum.
3. Kullanıcı için (site tarafından) yönlendirilir ``http://nerddiner.com/Account/LogOn`` (gerçek sitesine benzeyen kötü amaçlı site).
4. Kullanıcı yeniden oturum (kimlik bilgilerini site kötü amaçlı vermiş) ve gerçek sitenin yönlendirilir.

Kullanıcı büyük olasılıkla, ilk oturum açma girişimi başarısız oldu düşünüyorsanız ve bunların ikincisi başarılı oldu. Bunlar büyük olasılıkla farkında kalacak kimlik bilgilerini tehlikeye.

![Açık yeniden yönlendirme saldırısına işlemi](preventing-open-redirects/_static/open-redirection-attack-process.png)

Oturum açma sayfaları ek olarak bazı siteler yeniden yönlendirme sayfaları veya uç noktaları sağlar. Uygulamanızı sahip açık bir yeniden yönlendirme içeren bir sayfa düşünün ``/Home/Redirect``. Bir saldırgan, örneğin, giden e-posta iletisinde bir bağlantı oluşturabilir ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``. Tipik bir kullanıcıyı URL'de bakın ve site adınızı ile başlayan bakın. Güvenen, bunlar bağlantıya tıklayarak. Açık yeniden yönlendirme sonra kullanıcı dilediğiniz aynı görünür, kimlik avı siteye gönderir ve kullanıcı büyük olasılıkla misiniz ne düşünüyorsanız için oturum açma adıdır, sitenizi.

## <a name="protecting-against-open-redirect-attacks"></a>Açık yeniden yönlendirme saldırılarına karşı koruma

Web uygulamaları geliştirirken, tüm kullanıcı tarafından sağlanan verileri güvenilmez olarak kabul eder. URL içeriğine göre kullanıcı yönlendiren işlevselliği uygulamanız varsa, bu tür yeniden yönlendirmeleri yalnızca yerel olarak, uygulamanızın içinde (veya bilinen bir URL, sorgu dizesinde sağlanan olmayan herhangi bir URL adresi) yapıldığını emin olun.

### <a name="localredirect"></a>LocalRedirect

Kullanım ``LocalRedirect`` temel yardımcı yöntemini `Controller` sınıfı:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

``LocalRedirect`` yerel olmayan URL'si belirtilmişse, bir özel durum oluşturur. Aksi takdirde, olduğu gibi davranır ``Redirect`` yöntemi.

### <a name="islocalurl"></a>IsLocalUrl

Kullanım [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) yöntemi yönlendirmeden önce URL'leri test etmek için:

Aşağıdaki örnek, bir URL yeniden yönlendirme önce yerel olup olmadığını denetlemek gösterilmiştir.

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

`IsLocalUrl` Yöntemi, kötü amaçlı bir siteye yanlışlıkla yönlendirildi kullanıcıları korur. Yerel olmayan URL yerel bir URL beklenirken bir durumda sağlandığında sağlanan URL ayrıntılarını oturum açabilir. Yeniden yönlendirme günlüğü URL'leri yeniden yönlendirme saldırılarına tanılamalarına yardımcı olabilir.
