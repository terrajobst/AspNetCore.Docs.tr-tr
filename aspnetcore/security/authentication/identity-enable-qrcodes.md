---
title: ASP.NET core'da TOTP authenticator uygulamaları için QR kodu oluşturmayı etkinleştirme
author: rick-anderson
description: ASP.NET Core iki öğeli kimlik doğrulama ile çalışmasını TOTP authenticator uygulamaları için QR kodu oluşturmayı etkinleştirme keşfedin.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a7fdc86b3fe94e714e5147c89a32fce13757d1c1
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902714"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="995f0-103">ASP.NET core'da TOTP authenticator uygulamaları için QR kodu oluşturmayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="995f0-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="995f0-104">QR kodları, ASP.NET Core 2.0 veya sonraki sürümünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="995f0-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="995f0-105">ASP.NET Core, bireysel kimlik doğrulaması için Doğrulayıcı uygulamalar için destek ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="995f0-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="995f0-106">Kullanarak bir zamana bağlı kerelik parola algoritması (TOTP), iki öğeli kimlik doğrulamayı (2FA) kimlik doğrulayıcısı uygulamalarını önerilen yaklaşımı 2FA için sektöre var.</span><span class="sxs-lookup"><span data-stu-id="995f0-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="995f0-107">2fa'yı kullanarak TOTP SMS 2FA için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="995f0-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="995f0-108">Authenticator uygulaması, kullanıcı adı ve parola onayladıktan sonra hangi kullanıcıların girmelisiniz 8 6 rakamlı bir kod sağlar.</span><span class="sxs-lookup"><span data-stu-id="995f0-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="995f0-109">Genellikle bir kimlik doğrulayıcı uygulaması, bir akıllı telefonda yüklenir.</span><span class="sxs-lookup"><span data-stu-id="995f0-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="995f0-110">ASP.NET Core web uygulaması şablonları kimlik doğrulayıcılar destekler, ancak QRCode oluşturulması için destek sağlaması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="995f0-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="995f0-111">QRCode oluşturucuları 2FA kurulumu kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="995f0-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="995f0-112">Bu belgede eklerken size yol gösterecek [QR kodunu](https://wikipedia.org/wiki/QR_code) 2fa'yı yapılandırma sayfasına oluşturma.</span><span class="sxs-lookup"><span data-stu-id="995f0-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

<span data-ttu-id="995f0-113">İki Etmenli kimlik doğrulamasının olmayacak gibi bir dış kimlik doğrulama sağlayıcısı kullanarak [Google](xref:security/authentication/google-logins) veya [Facebook](xref:security/authentication/facebook-logins).</span><span class="sxs-lookup"><span data-stu-id="995f0-113">Two factor authentication does not happen using an external authentication provider, such as [Google](xref:security/authentication/google-logins) or [Facebook](xref:security/authentication/facebook-logins).</span></span> <span data-ttu-id="995f0-114">Harici oturum açmaları korunmasını dış oturum açma sağlayıcısı tarafından herhangi bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="995f0-114">External logins are protected by whatever mechanism the external login provider provides.</span></span> <span data-ttu-id="995f0-115">Örneğin, düşünün [Microsoft](xref:security/authentication/microsoft-logins) kimlik doğrulama sağlayıcısı, donanım anahtarı veya başka bir 2FA yaklaşım gerektirir.</span><span class="sxs-lookup"><span data-stu-id="995f0-115">Consider, for example, the [Microsoft](xref:security/authentication/microsoft-logins) authentication provider requires a hardware key or another 2FA approach.</span></span> <span data-ttu-id="995f0-116">"Yerel" 2fa'yı varsayılan şablonlar zorunlu, kullanıcıların yaygın olarak kullanılan bir senaryo değildir iki 2FA yaklaşım karşılamak için duyacaktır.</span><span class="sxs-lookup"><span data-stu-id="995f0-116">If the default templates enforced "local" 2FA then users would be required to satisfy two 2FA approaches, which is not a commonly used scenario.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="995f0-117">QR kodları 2fa'yı yapılandırma sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="995f0-117">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="995f0-118">Bu yönergeleri kullanın *qrcode.js* gelen https://davidshimjs.github.io/qrcodejs/ depo.</span><span class="sxs-lookup"><span data-stu-id="995f0-118">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="995f0-119">İndirme [qrcode.js javascript Kitaplığı](https://davidshimjs.github.io/qrcodejs/) için `wwwroot\lib` projenizdeki klasör.</span><span class="sxs-lookup"><span data-stu-id="995f0-119">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="995f0-120">Bölümündeki yönergeleri [İskele kimlik](xref:security/authentication/scaffold-identity) oluşturulacak */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="995f0-120">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="995f0-121">İçinde */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, bulun `Scripts` dosyanın sonunda bölüm:</span><span class="sxs-lookup"><span data-stu-id="995f0-121">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="995f0-122">İçinde *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor sayfaları) veya *Views/Manage/EnableAuthenticator.cshtml* (MVC) bulun `Scripts` dosyanın sonunda bölüm:</span><span class="sxs-lookup"><span data-stu-id="995f0-122">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="995f0-123">Güncelleştirme `Scripts` bir başvuru eklemek için bölüm `qrcodejs` kitaplığı eklediğiniz ve QR kodunu oluşturmak için bir çağrı.</span><span class="sxs-lookup"><span data-stu-id="995f0-123">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="995f0-124">Şu şekilde görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="995f0-124">It should look as follows:</span></span>

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

* <span data-ttu-id="995f0-125">Bu yönergeleri için bağlantıları paragraf silin.</span><span class="sxs-lookup"><span data-stu-id="995f0-125">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="995f0-126">Uygulamanızı çalıştırın ve QR kodunu tarayın ve Doğrulayıcı kanıtlar doğrulanması emin olun.</span><span class="sxs-lookup"><span data-stu-id="995f0-126">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="995f0-127">Site adı QR kodunu değiştirin</span><span class="sxs-lookup"><span data-stu-id="995f0-127">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="995f0-128">QR kodunu site adı, başlangıçta, projeyi oluştururken seçtiğiniz proje adından alınır.</span><span class="sxs-lookup"><span data-stu-id="995f0-128">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="995f0-129">Bakarak değiştirebilirsiniz `GenerateQrCodeUri(string email, string unformattedKey)` yönteminde */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="995f0-129">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="995f0-130">QR kodunu site adı, başlangıçta, projeyi oluştururken seçtiğiniz proje adından alınır.</span><span class="sxs-lookup"><span data-stu-id="995f0-130">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="995f0-131">Bakarak değiştirebilirsiniz `GenerateQrCodeUri(string email, string unformattedKey)` yönteminde *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor sayfaları) dosya veya *Controllers/ManageController.cs* (MVC) dosyası.</span><span class="sxs-lookup"><span data-stu-id="995f0-131">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="995f0-132">Şablondan varsayılan kod şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="995f0-132">The default code from the template looks as follows:</span></span>

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenticatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

<span data-ttu-id="995f0-133">İkinci parametre çağrısında `string.Format` , çözüm adı geçen, site adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="995f0-133">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="995f0-134">Herhangi bir değere değiştirilebilir, ancak her zaman URL olarak kodlanmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="995f0-134">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="995f0-135">Farklı bir QR kodu kitaplığı kullanma</span><span class="sxs-lookup"><span data-stu-id="995f0-135">Using a different QR Code library</span></span>

<span data-ttu-id="995f0-136">QR kodu kitaplığı ile tercih edilen kitaplığınızı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="995f0-136">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="995f0-137">HTML'yi içeren bir `qrCode` kitaplığınızı öğesi içine yerleştirebileceğiniz bir QR kodu tarafından ne olursa olsun mekanizması sağlar.</span><span class="sxs-lookup"><span data-stu-id="995f0-137">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="995f0-138">Doğru biçimlendirilmiş bir URL için QR kodunu kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="995f0-138">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="995f0-139">`AuthenticatorUri` model özelliği.</span><span class="sxs-lookup"><span data-stu-id="995f0-139">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="995f0-140">`data-url` bir özellik `qrCodeData` öğesi.</span><span class="sxs-lookup"><span data-stu-id="995f0-140">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="995f0-141">TOTP istemci ve sunucu zaman eğimi</span><span class="sxs-lookup"><span data-stu-id="995f0-141">TOTP client and server time skew</span></span>

<span data-ttu-id="995f0-142">TOTP (zamana bağlı kerelik parola) kimlik doğrulaması doğru bir zaman hem sunucu hem de authenticator cihazda bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="995f0-142">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="995f0-143">Belirteçleri, 30 saniye için yalnızca en son.</span><span class="sxs-lookup"><span data-stu-id="995f0-143">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="995f0-144">TOTP 2FA oturum açma başarısız oluyorsa, sunucu saatinin doğru ve doğru bir NTP hizmetine tercihen eşitlenmiş olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="995f0-144">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
