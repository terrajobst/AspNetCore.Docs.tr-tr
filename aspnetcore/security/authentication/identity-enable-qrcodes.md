---
title: "ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma"
author: rick-anderson
description: "ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma"
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: ae2d8eb938c00a26cf7ffb5f2fff0b9e0d22148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma

Not: Bu konu, ASP.NET Core geçerlidir 2.x

ASP.NET Core tek tek kimlik doğrulaması için Doğrulayıcı uygulamalar için destek ile birlikte gelir. Bir zaman tabanlı kerelik parola algoritması (TOTP), kullanarak iki faktörlü kimlik doğrulamasını (2FA) Doğrulayıcı uygulamalar önerilen yaklaşımı 2FA için endüstri ' dir. 2FA TOTP kullanarak, SMS 2FA için tercih edilen yöntemdir. Doğrulayıcı uygulama hangi kullanıcıların, kullanıcı adını ve parolayı doğruladıktan sonra girmelisiniz 8 6 rakamlı bir kod sağlar. Genellikle bir doğrulayıcı uygulama üzerinde akıllı telefona yüklenir.

ASP.NET Core web uygulama şablonları Doğrulayıcı destekler, ancak QRCode oluşturma için desteği yoktur. QRCode oluşturucuları 2FA kurulumu kolaylaştırır. Bu belge eklerken size yol gösterecek [QR kodu](https://wikipedia.org/wiki/QR_code) 2FA yapılandırma sayfasına oluşturma.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>QR kodlarını 2FA yapılandırma sayfasına ekleme

Bu yönergeleri kullanmak *qrcode.js* https://davidshimjs.github.io/qrcodejs/ depoyu gelen.

* Karşıdan [qrcode.js javascript Kitaplığı](https://davidshimjs.github.io/qrcodejs/) için `wwwroot\lib` projenizdeki klasöre.

* İçinde *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor sayfalarının) veya *Views\Manage\EnableAuthenticator.cshtml* (MVC) bulun `Scripts` dosyasının sonuna kısmına:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Güncelleştirme `Scripts` bir başvuru eklemek için bölüm `qrcodejs` eklediğiniz kitaplık ve QR kodu oluşturmak için bir çağrı. Aşağıdaki gibi görünmelidir:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Bu yönergeleri bağlayan paragraf silin.

Uygulamanızı çalıştırın ve QR kodunu tarayın ve Doğrulayıcı kanıtlar kod doğrulama emin olun.

## <a name="change-the-site-name-in-the-qr-code"></a>QR kodu site adını değiştirin

Site adı QR kodu başlangıçta projenizi oluştururken seçtiğiniz proje adından alınır. Bakarak değiştirme `GenerateQrCodeUri(string email, string unformattedKey)` yönteminde *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor sayfalarının) dosyası ya da *Controllers\ManageController.cs* (MVC) dosyası. 

Şablondan varsayılan kod şu şekilde görünür:

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

İkinci parametre çağrısında `string.Format` çözüm adından alınan, site adı kullanılır. Herhangi bir değere değiştirilebilir, ancak her zaman URL kodlanmış olmalıdır.

## <a name="using-a-different-qr-code-library"></a>Farklı bir QR kodu kitaplık kullanma

Tercih edilen kitaplıkla QR kodunu kitaplığı değiştirebilirsiniz. HTML içeren bir `qrCode` kitaplığınızın öğesi içine yerleştirebileceğiniz bir QR kodu tarafından mekanizma sağlar.

QR kodunu doğru biçimlendirilmiş URL'sini bulunur:

* `AuthenticatorUri`model özelliği.
* `data-url`bir özellik `qrCodeData` öğesi. 

Kullanım `@Html.Raw` görünümünde model özelliğine erişmek için (Aksi halde URL'deki ve işaretlerini çift kodlanmış ve QR kodunu etiket parametresinin göz ardı edilir).

## <a name="totp-client-and-server-time-skew"></a>TOTP istemci ve sunucu zaman eğme

TOTP kimlik doğrulaması doğru zaman sahip hem sunucu hem de kimlik doğrulayıcı aygıta bağlıdır. Belirteçleri yalnızca 30 saniye son. TOTP 2FA oturumları başarısız oluyorsa, sunucu saati doğru ve doğru bir NTP hizmeti tercihen eşitlenmiş olup olmadığını denetleyin.
