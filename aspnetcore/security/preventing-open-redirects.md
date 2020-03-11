---
title: ASP.NET Core 'da açık yeniden yönlendirme saldırılarını önleme
author: ardalis
description: ASP.NET Core uygulamasında açık yeniden yönlendirme saldırılarının nasıl önleneceği gösterilmektedir
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 9d8cac8708fe9aeadba5af1287362a20df7f6bfe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660526"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>ASP.NET Core 'da açık yeniden yönlendirme saldırılarını önleme

QueryString veya form verileri gibi istek aracılığıyla belirtilen bir URL 'ye yeniden yönlendiren bir Web uygulaması, kullanıcıları harici, kötü amaçlı bir URL 'ye yönlendirmek için ile oynanmış olabilir. Bu değişikliklere açık yeniden yönlendirme saldırısı denir.

Uygulamanızın mantığı belirtilen bir URL 'ye yeniden yönlendirdiğinde, yeniden yönlendirme URL 'sinin değiştirilmediğini doğrulamanız gerekir. ASP.NET Core, uygulamaların yeniden yönlendirme (açık yeniden yönlendirme olarak da bilinir) saldırıları korumasına yardımcı olacak yerleşik işlevlere sahiptir.

## <a name="what-is-an-open-redirect-attack"></a>Açık yeniden yönlendirme saldırısı nedir?

Web uygulamaları, kimlik doğrulaması gerektiren kaynaklara erişirken kullanıcıları sıklıkla bir oturum açma sayfasına yönlendirir. Yeniden yönlendirme genellikle, kullanıcının başarıyla oturum açtıktan sonra ilk olarak istenen URL 'ye dönebilmesi için bir `returnUrl` QueryString parametresi içerir. Kullanıcı kimlik doğrulamasından geçtikten sonra, ilk olarak istediği URL 'ye yeniden yönlendirilir.

Hedef URL isteğin QueryString öğesinde belirtildiğinden, kötü niyetli bir Kullanıcı QueryString ile oynayabilir. Değiştirilen bir QueryString, sitenin kullanıcıyı harici, kötü amaçlı bir siteye yönlendirmesine izin verebilir. Bu teknik bir açık yeniden yönlendirme (veya yeniden yönlendirme) saldırısı olarak adlandırılır.

### <a name="an-example-attack"></a>Örnek saldırı

Kötü niyetli bir Kullanıcı, kötü niyetli kullanıcıların bir kullanıcının kimlik bilgilerine veya gizli bilgilere erişmesine izin veren bir saldırı geliştirebilir. Kötü amaçlı Kullanıcı, saldırının başlaması için kullanıcıyı, URL 'ye eklenen bir `returnUrl` QueryString değeri ile sitenizin oturum açma sayfasının bağlantısını tıklamayı ikna eder. Örneğin, `http://contoso.com/Account/LogOn?returnUrl=/Home/About`bir oturum açma sayfası içeren `contoso.com` bir uygulamayı düşünün. Saldırı aşağıdaki adımları izler:

1. Kullanıcı `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` için kötü amaçlı bir bağlantıyı tıklatır (ikinci URL "contoso**1**. com", "contoso.com" değil).
2. Kullanıcı başarıyla oturum açar.
3. Kullanıcı (tam olarak gerçek site gibi görünen kötü amaçlı bir site) `http://contoso1.com/Account/LogOn` için yeniden yönlendirilir (site tarafından).
4. Kullanıcı yeniden oturum açar (kötü amaçlı site kimlik bilgileri verir) ve gerçek siteye yeniden yönlendirilir.

Kullanıcı, ilk oturum açma girişiminin başarısız olduğunu ve ikinci denemesinin başarılı olduğunu düşündüğü bir durum olabilir. Kullanıcı kimlik bilgilerinin güvenliğinin aşıldığına ilişkin büyük olasılıkla Kullanıcı farkında değildir.

![Yeniden yönlendirme saldırı Işlemini aç](preventing-open-redirects/_static/open-redirection-attack-process.png)

Bazı siteler, oturum açma sayfalarının yanı sıra yeniden yönlendirme sayfaları veya uç noktaları sağlar. Uygulamanızı açık yeniden yönlendirme içeren bir sayfa olduğunu düşünün `/Home/Redirect`. Saldırgan, `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`giden bir e-postada bağlantı oluşturabilir. Tipik bir Kullanıcı URL 'ye bakar ve site adınızla başlayıp başlamadığını görür. Bunun için bağlantıya tıklamaları gerekir. Açık yeniden yönlendirme daha sonra kullanıcıyı sizinkilerle aynı olan kimlik avı sitesine gönderir ve Kullanıcı büyük olasılıkla sitenizin ne olduğuna inandıkları anlamına gelir.

## <a name="protecting-against-open-redirect-attacks"></a>Açık yeniden yönlendirme saldırılarına karşı koruma

Web uygulamaları geliştirirken, Kullanıcı tarafından sağlanmış tüm verileri güvenilir değil olarak değerlendirin. Uygulamanızın, URL 'nin içeriğine göre kullanıcıyı yönlendiren işlevselliği varsa, bu yeniden yönlerinizin yalnızca uygulamanızda (veya QueryString 'de sağlanabilecek URL 'ler değil, bilinen bir URL 'ye değil) yerel olarak yapıldığından emin olun.

### <a name="localredirect"></a>LocalRedirect

Temel `Controller` sınıfındaki `LocalRedirect` yardımcı yöntemini kullanın:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect`, yerel olmayan bir URL belirtildiğinde bir özel durum oluşturur. Aksi takdirde, tıpkı `Redirect` yöntemi gibi davranır.

### <a name="islocalurl"></a>IsLocalUrl 'Si

Yeniden yönlendirmeden önce URL 'Leri test etmek için [ıslocalurl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metodunu kullanın:

Aşağıdaki örnek, yeniden yönlendirmeden önce bir URL 'nin yerel olup olmadığını denetlemeyi gösterir.

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

`IsLocalUrl` yöntemi, kullanıcıların farkında olmadan kötü amaçlı bir siteye yönlendirilmesini önler. Yerel bir URL 'yi beklediğiniz bir durumda, yerel olmayan bir URL sağlandığında sağlanan URL 'nin ayrıntılarını günlüğe kaydedebilirsiniz. Günlüğe kaydetme yeniden yönlendirme URL 'Leri, yeniden yönlendirme saldırılarını tanılamaya yardımcı olabilir.
