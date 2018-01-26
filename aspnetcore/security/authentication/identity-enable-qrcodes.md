---
title: "ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma"
author: rick-anderson
description: "ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma"
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: d3710e72f3f4f2a5ecc4cfa53f721cca5239aa70
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="5af35-103">ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="5af35-103">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="5af35-104">Not: Bu konu, ASP.NET Core geçerlidir 2.x</span><span class="sxs-lookup"><span data-stu-id="5af35-104">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="5af35-105">ASP.NET Core tek tek kimlik doğrulaması için Doğrulayıcı uygulamalar için destek ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="5af35-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="5af35-106">Bir zaman tabanlı kerelik parola algoritması (TOTP), kullanarak iki faktörlü kimlik doğrulamasını (2FA) Doğrulayıcı uygulamalar önerilen yaklaşımı 2FA için endüstri ' dir.</span><span class="sxs-lookup"><span data-stu-id="5af35-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="5af35-107">2FA TOTP kullanarak, SMS 2FA için tercih edilen yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="5af35-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="5af35-108">Doğrulayıcı uygulama hangi kullanıcıların, kullanıcı adını ve parolayı doğruladıktan sonra girmelisiniz 8 6 rakamlı bir kod sağlar.</span><span class="sxs-lookup"><span data-stu-id="5af35-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="5af35-109">Genellikle bir doğrulayıcı uygulama üzerinde akıllı telefona yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5af35-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="5af35-110">ASP.NET Core web uygulama şablonları Doğrulayıcı destekler, ancak QRCode oluşturma için desteği yoktur.</span><span class="sxs-lookup"><span data-stu-id="5af35-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="5af35-111">QRCode oluşturucuları 2FA kurulumu kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="5af35-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="5af35-112">Bu belge eklerken size yol gösterecek [QR kodu](https://wikipedia.org/wiki/QR_code) 2FA yapılandırma sayfasına oluşturma.</span><span class="sxs-lookup"><span data-stu-id="5af35-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="5af35-113">QR kodlarını 2FA yapılandırma sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="5af35-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="5af35-114">Bu yönergeleri kullanmak *qrcode.js* https://davidshimjs.github.io/qrcodejs/ depoyu gelen.</span><span class="sxs-lookup"><span data-stu-id="5af35-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="5af35-115">Karşıdan [qrcode.js javascript Kitaplığı](https://davidshimjs.github.io/qrcodejs/) için `wwwroot\lib` projenizdeki klasöre.</span><span class="sxs-lookup"><span data-stu-id="5af35-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="5af35-116">İçinde *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor sayfalarının) veya *Views\Manage\EnableAuthenticator.cshtml* (MVC) bulun `Scripts` dosyasının sonuna kısmına:</span><span class="sxs-lookup"><span data-stu-id="5af35-116">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="5af35-117">Güncelleştirme `Scripts` bir başvuru eklemek için bölüm `qrcodejs` eklediğiniz kitaplık ve QR kodu oluşturmak için bir çağrı.</span><span class="sxs-lookup"><span data-stu-id="5af35-117">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="5af35-118">Aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="5af35-118">It should look as follows:</span></span>

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

* <span data-ttu-id="5af35-119">Bu yönergeleri bağlayan paragraf silin.</span><span class="sxs-lookup"><span data-stu-id="5af35-119">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="5af35-120">Uygulamanızı çalıştırın ve QR kodunu tarayın ve Doğrulayıcı kanıtlar kod doğrulama emin olun.</span><span class="sxs-lookup"><span data-stu-id="5af35-120">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="5af35-121">QR kodu site adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="5af35-121">Change the site name in the QR Code</span></span>

<span data-ttu-id="5af35-122">Site adı QR kodu başlangıçta projenizi oluştururken seçtiğiniz proje adından alınır.</span><span class="sxs-lookup"><span data-stu-id="5af35-122">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="5af35-123">Bakarak değiştirme `GenerateQrCodeUri(string email, string unformattedKey)` yönteminde *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor sayfalarının) dosyası ya da *Controllers\ManageController.cs* (MVC) dosyası.</span><span class="sxs-lookup"><span data-stu-id="5af35-123">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="5af35-124">Şablondan varsayılan kod şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="5af35-124">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="5af35-125">İkinci parametre çağrısında `string.Format` çözüm adından alınan, site adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5af35-125">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="5af35-126">Herhangi bir değere değiştirilebilir, ancak her zaman URL kodlanmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5af35-126">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="5af35-127">Farklı bir QR kodu kitaplık kullanma</span><span class="sxs-lookup"><span data-stu-id="5af35-127">Using a different QR Code library</span></span>

<span data-ttu-id="5af35-128">Tercih edilen kitaplıkla QR kodunu kitaplığı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5af35-128">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="5af35-129">HTML içeren bir `qrCode` kitaplığınızın öğesi içine yerleştirebileceğiniz bir QR kodu tarafından mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="5af35-129">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="5af35-130">QR kodunu doğru biçimlendirilmiş URL'sini bulunur:</span><span class="sxs-lookup"><span data-stu-id="5af35-130">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="5af35-131">`AuthenticatorUri`model özelliği.</span><span class="sxs-lookup"><span data-stu-id="5af35-131">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="5af35-132">`data-url`bir özellik `qrCodeData` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5af35-132">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="5af35-133">Kullanım `@Html.Raw` görünümünde model özelliğine erişmek için (Aksi halde URL'deki ve işaretlerini çift kodlanmış ve QR kodunu etiket parametresinin göz ardı edilir).</span><span class="sxs-lookup"><span data-stu-id="5af35-133">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="5af35-134">TOTP istemci ve sunucu zaman eğme</span><span class="sxs-lookup"><span data-stu-id="5af35-134">TOTP client and server time skew</span></span>

<span data-ttu-id="5af35-135">TOTP kimlik doğrulaması doğru zaman sahip hem sunucu hem de kimlik doğrulayıcı aygıta bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="5af35-135">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="5af35-136">Belirteçleri yalnızca 30 saniye son.</span><span class="sxs-lookup"><span data-stu-id="5af35-136">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="5af35-137">TOTP 2FA oturumları başarısız oluyorsa, sunucu saati doğru ve doğru bir NTP hizmeti tercihen eşitlenmiş olup olmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5af35-137">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
