---
title: "ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma"
author: rick-anderson
description: "ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma"
keywords: "ASP.NET Core, MVC, QR kodu oluşturma, kimlik doğrulayıcı, 2FA"
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a3029e68164dd91d1bc43704c5e96bd591bcae05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="dbb63-104">ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="dbb63-104">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="dbb63-105">Not: Bu konu, ASP.NET Core geçerlidir 2.x</span><span class="sxs-lookup"><span data-stu-id="dbb63-105">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="dbb63-106">ASP.NET Core tek tek kimlik doğrulaması için Doğrulayıcı uygulamalar için destek ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="dbb63-106">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="dbb63-107">Bir zaman tabanlı kerelik parola algoritması (TOTP), kullanarak iki faktörlü kimlik doğrulamasını (2FA) Doğrulayıcı uygulamalar önerilen yaklaşımı 2FA için endüstri ' dir.</span><span class="sxs-lookup"><span data-stu-id="dbb63-107">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="dbb63-108">2FA TOTP kullanarak, SMS 2FA için tercih edilen yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="dbb63-108">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="dbb63-109">Doğrulayıcı uygulama hangi kullanıcıların, kullanıcı adını ve parolayı doğruladıktan sonra girmelisiniz 8 6 rakamlı bir kod sağlar.</span><span class="sxs-lookup"><span data-stu-id="dbb63-109">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="dbb63-110">Genellikle bir doğrulayıcı uygulama üzerinde akıllı telefona yüklenir.</span><span class="sxs-lookup"><span data-stu-id="dbb63-110">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="dbb63-111">ASP.NET Core web uygulama şablonları Doğrulayıcı destekler, ancak QRCode oluşturma için destek sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="dbb63-111">The ASP.NET Core web app templates support authenticators, but do not provide support for QRCode generation.</span></span> <span data-ttu-id="dbb63-112">QRCode oluşturucuları 2FA kurulumu kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="dbb63-112">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="dbb63-113">Bu belge eklerken size yol gösterecek [QR kodu](https://wikipedia.org/wiki/QR_code) 2FA yapılandırma sayfasına oluşturma.</span><span class="sxs-lookup"><span data-stu-id="dbb63-113">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="dbb63-114">QR kodlarını 2FA yapılandırma sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="dbb63-114">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="dbb63-115">Bu yönergeleri kullanmak *qrcode.js* https://davidshimjs.github.io/qrcodejs/ depoyu gelen.</span><span class="sxs-lookup"><span data-stu-id="dbb63-115">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="dbb63-116">Karşıdan [qrcode.js javascript Kitaplığı](https://davidshimjs.github.io/qrcodejs/) için `wwwroot\lib` projenizdeki klasöre.</span><span class="sxs-lookup"><span data-stu-id="dbb63-116">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="dbb63-117">İçinde *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor sayfalarının) veya *Views\Manage\EnableAuthenticator.cshtml* (MVC) bulun `Scripts` dosyasının sonuna kısmına:</span><span class="sxs-lookup"><span data-stu-id="dbb63-117">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="dbb63-118">Güncelleştirme `Scripts` bir başvuru eklemek için bölüm `qrcodejs` eklediğiniz kitaplık ve QR kodu oluşturmak için bir çağrı.</span><span class="sxs-lookup"><span data-stu-id="dbb63-118">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="dbb63-119">Aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="dbb63-119">It should look as follows:</span></span>

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

* <span data-ttu-id="dbb63-120">Bu yönergeleri bağlayan paragraf silin.</span><span class="sxs-lookup"><span data-stu-id="dbb63-120">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="dbb63-121">Uygulamanızı çalıştırın ve QR kodunu tarayın ve Doğrulayıcı kanıtlar kod doğrulama emin olun.</span><span class="sxs-lookup"><span data-stu-id="dbb63-121">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="dbb63-122">QR kodu site adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="dbb63-122">Change the site name in the QR Code</span></span>

<span data-ttu-id="dbb63-123">Site adı QR kodu başlangıçta projenizi oluştururken seçtiğiniz proje adından alınır.</span><span class="sxs-lookup"><span data-stu-id="dbb63-123">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="dbb63-124">Bakarak değiştirme `GenerateQrCodeUri(string email, string unformattedKey)` yönteminde *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor sayfalarının) dosyası ya da *Controllers\ManageController.cs* (MVC) dosyası.</span><span class="sxs-lookup"><span data-stu-id="dbb63-124">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="dbb63-125">Şablondan varsayılan kod şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="dbb63-125">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="dbb63-126">İkinci parametre çağrısında `string.Format` çözüm adından alınan, site adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dbb63-126">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="dbb63-127">Herhangi bir değere değiştirilebilir, ancak her zaman URL kodlanmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="dbb63-127">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="dbb63-128">Farklı bir QR kodu kitaplık kullanma</span><span class="sxs-lookup"><span data-stu-id="dbb63-128">Using a different QR Code library</span></span>

<span data-ttu-id="dbb63-129">Tercih edilen kitaplıkla QR kodunu kitaplığı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dbb63-129">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="dbb63-130">HTML içeren bir `qrCode` kitaplığınızın öğesi içine yerleştirebileceğiniz bir QR kodu tarafından mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="dbb63-130">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="dbb63-131">QR kodunu doğru biçimlendirilmiş URL'sini bulunur:</span><span class="sxs-lookup"><span data-stu-id="dbb63-131">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="dbb63-132">`AuthenticatorUri`model özelliği.</span><span class="sxs-lookup"><span data-stu-id="dbb63-132">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="dbb63-133">`data-url`bir özellik `qrCodeData` öğesi.</span><span class="sxs-lookup"><span data-stu-id="dbb63-133">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="dbb63-134">Kullanım `@Html.Raw` görünümünde model özelliğine erişmek için (Aksi halde URL'deki ve işaretlerini çift kodlanmış ve QR kodunu etiket parametresinin göz ardı edilir).</span><span class="sxs-lookup"><span data-stu-id="dbb63-134">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="dbb63-135">TOTP istemci ve sunucu zaman eğme</span><span class="sxs-lookup"><span data-stu-id="dbb63-135">TOTP client and server time skew</span></span>

<span data-ttu-id="dbb63-136">TOTP kimlik doğrulaması doğru zaman sahip hem sunucu hem de kimlik doğrulayıcı aygıta bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="dbb63-136">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="dbb63-137">Belirteçleri yalnızca 30 saniye son.</span><span class="sxs-lookup"><span data-stu-id="dbb63-137">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="dbb63-138">TOTP 2FA oturumları başarısız oluyorsa, sunucu saati doğru ve doğru bir NTP hizmeti tercihen eşitlenmiş olup olmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="dbb63-138">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
