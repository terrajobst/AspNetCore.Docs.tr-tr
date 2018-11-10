---
title: ASP.NET core'da TOTP authenticator uygulamaları için QR kodu oluşturmayı etkinleştirme
author: rick-anderson
description: ASP.NET Core iki öğeli kimlik doğrulama ile çalışmasını TOTP authenticator uygulamaları için QR kodu oluşturmayı etkinleştirme keşfedin.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 437f354f71128a98bae9abdced291e04efc9f48e
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225388"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>ASP.NET core'da TOTP authenticator uygulamaları için QR kodu oluşturmayı etkinleştirme

::: moniker range="<= aspnetcore-2.0"

QR kodları, ASP.NET Core 2.0 veya sonraki sürümünü gerektirir.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core, bireysel kimlik doğrulaması için Doğrulayıcı uygulamalar için destek ile birlikte gelir. Kullanarak bir zamana bağlı kerelik parola algoritması (TOTP), iki öğeli kimlik doğrulamayı (2FA) kimlik doğrulayıcısı uygulamalarını önerilen yaklaşımı 2FA için sektöre var. 2fa'yı kullanarak TOTP SMS 2FA için tercih edilir. Authenticator uygulaması, kullanıcı adı ve parola onayladıktan sonra hangi kullanıcıların girmelisiniz 8 6 rakamlı bir kod sağlar. Genellikle bir kimlik doğrulayıcı uygulaması, bir akıllı telefonda yüklenir.

ASP.NET Core web uygulaması şablonları kimlik doğrulayıcılar destekler, ancak QRCode oluşturulması için destek sağlaması gerekmez. QRCode oluşturucuları 2FA kurulumu kolaylaştırır. Bu belgede eklerken size yol gösterecek [QR kodunu](https://wikipedia.org/wiki/QR_code) 2fa'yı yapılandırma sayfasına oluşturma.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>QR kodları 2fa'yı yapılandırma sayfasına ekleme

Bu yönergeleri kullanın *qrcode.js* gelen https://davidshimjs.github.io/qrcodejs/ depo.

* İndirme [qrcode.js javascript Kitaplığı](https://davidshimjs.github.io/qrcodejs/) için `wwwroot\lib` projenizdeki klasör.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Bölümündeki yönergeleri [İskele kimlik](xref:security/authentication/scaffold-identity) oluşturulacak */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.
* İçinde */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, bulun `Scripts` dosyanın sonunda bölüm:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* İçinde *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor sayfaları) veya *Views/Manage/EnableAuthenticator.cshtml* (MVC) bulun `Scripts` dosyanın sonunda bölüm:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Güncelleştirme `Scripts` bir başvuru eklemek için bölüm `qrcodejs` kitaplığı eklediğiniz ve QR kodunu oluşturmak için bir çağrı. Şu şekilde görünmelidir:

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

* Bu yönergeleri için bağlantıları paragraf silin.

Uygulamanızı çalıştırın ve QR kodunu tarayın ve Doğrulayıcı kanıtlar doğrulanması emin olun.

## <a name="change-the-site-name-in-the-qr-code"></a>Site adı QR kodunu değiştirin

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

QR kodunu site adı, başlangıçta, projeyi oluştururken seçtiğiniz proje adından alınır. Bakarak değiştirebilirsiniz `GenerateQrCodeUri(string email, string unformattedKey)` yönteminde */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

QR kodunu site adı, başlangıçta, projeyi oluştururken seçtiğiniz proje adından alınır. Bakarak değiştirebilirsiniz `GenerateQrCodeUri(string email, string unformattedKey)` yönteminde *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor sayfaları) dosya veya *Controllers/ManageController.cs* (MVC) dosyası.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Şablondan varsayılan kod şu şekilde görünür:

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

İkinci parametre çağrısında `string.Format` , çözüm adı geçen, site adı kullanılır. Herhangi bir değere değiştirilebilir, ancak her zaman URL olarak kodlanmış olmalıdır.

## <a name="using-a-different-qr-code-library"></a>Farklı bir QR kodu kitaplığı kullanma

QR kodu kitaplığı ile tercih edilen kitaplığınızı değiştirebilirsiniz. HTML'yi içeren bir `qrCode` kitaplığınızı öğesi içine yerleştirebileceğiniz bir QR kodu tarafından ne olursa olsun mekanizması sağlar.

Doğru biçimlendirilmiş bir URL için QR kodunu kullanılabilir:

* `AuthenticatorUri` model özelliği.
* `data-url` bir özellik `qrCodeData` öğesi.

## <a name="totp-client-and-server-time-skew"></a>TOTP istemci ve sunucu zaman eğimi

TOTP (zamana bağlı kerelik parola) kimlik doğrulaması doğru bir zaman hem sunucu hem de authenticator cihazda bağlıdır. Belirteçleri, 30 saniye için yalnızca en son. TOTP 2FA oturum açma başarısız oluyorsa, sunucu saatinin doğru ve doğru bir NTP hizmetine tercihen eşitlenmiş olduğunu denetleyin.

::: moniker-end
