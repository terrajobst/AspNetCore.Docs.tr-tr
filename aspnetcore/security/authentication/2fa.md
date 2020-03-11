---
title: ASP.NET Core SMS ile iki öğeli kimlik doğrulama
author: rick-anderson
description: İki öğeli kimlik doğrulamayı (2FA) ile ASP.NET Core uygulaması ayarlama konusunda bilgi edinin.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 424d21e446de02b39daa7685205a00931361b326
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661457"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="7d8a0-103">ASP.NET Core SMS ile iki öğeli kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="7d8a0-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="7d8a0-104">[Rick Anderson](https://twitter.com/RickAndMSFT) ve [Isviçre-Devs](https://github.com/Swiss-Devs) tarafından</span><span class="sxs-lookup"><span data-stu-id="7d8a0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="7d8a0-105">Kullanarak bir zamana bağlı kerelik parola algoritması (TOTP), iki öğeli kimlik doğrulamayı (2FA) kimlik doğrulayıcısı uygulamalarını önerilen yaklaşımı 2FA için sektöre var.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="7d8a0-106">2fa'yı kullanarak TOTP SMS 2FA için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="7d8a0-107">Daha fazla bilgi için bkz. ASP.NET Core 2,0 ve üzeri için [ASP.NET Core ' de TOTP Authenticator uygulamaları IÇIN QR kodu oluşturmayı etkinleştirme](xref:security/authentication/identity-enable-qrcodes) .</span><span class="sxs-lookup"><span data-stu-id="7d8a0-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="7d8a0-108">Bu öğreticide, SMS kullanarak iki öğeli kimlik doğrulamasını (2FA) ayarlama işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="7d8a0-109">Yönergeler [Twilio](https://www.twilio.com/) ve [aspsms](https://www.aspsms.com/asp.net/identity/core/testcredits/)için verilmiştir, ancak başka herhangi bir SMS sağlayıcısını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="7d8a0-110">Bu Öğreticiyi başlatmadan önce [Hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm) işleminin tamamlanmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="7d8a0-111">[Örnek kodu görüntüleyin veya indirin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="7d8a0-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="7d8a0-112">[Nasıl indirileceği](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="7d8a0-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="7d8a0-113">Yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="7d8a0-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="7d8a0-114">Bireysel kullanıcı hesaplarıyla `Web2FA` adlı yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="7d8a0-115">HTTPS 'yi ayarlamak ve istemek için <xref:security/enforcing-ssl> içindeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-115">Follow the instructions in <xref:security/enforcing-ssl> to set up and require HTTPS.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="7d8a0-116">SMS hesap oluşturma</span><span class="sxs-lookup"><span data-stu-id="7d8a0-116">Create an SMS account</span></span>

<span data-ttu-id="7d8a0-117">Örneğin, [Twilio](https://www.twilio.com/) veya [aspsms](https://www.aspsms.com/asp.net/identity/core/testcredits/)adresinden bir SMS hesabı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="7d8a0-118">Kimlik doğrulama bilgileri kaydetmek (twilio'dan: accountSid ve ASPSMS için bir authToken: Userkey ve parola).</span><span class="sxs-lookup"><span data-stu-id="7d8a0-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="7d8a0-119">SMS Sağlayıcısı kimlik bilgilerini başarınızda</span><span class="sxs-lookup"><span data-stu-id="7d8a0-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="7d8a0-120">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="7d8a0-120">**Twilio:**</span></span>

<span data-ttu-id="7d8a0-121">Twilio hesabınızın Pano sekmesinden **Hesap SID** 'Sini ve **kimlik doğrulama belirtecini**kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="7d8a0-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7d8a0-122">**ASPSMS:**</span></span>

<span data-ttu-id="7d8a0-123">Hesap ayarlarınızda **userKey** ' e gidin ve **parolanızla**birlikte kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="7d8a0-124">Bu değerleri daha sonra anahtarlar `SMSAccountIdentification` ve `SMSAccountPassword`içindeki gizli-Manager aracıyla depolayacağız.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="7d8a0-125">Senderıd belirtme / Düzenleyicisi</span><span class="sxs-lookup"><span data-stu-id="7d8a0-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="7d8a0-126">**Twilio:** Sayılar sekmesinden Twilio **telefon numaranızı**kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-126">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="7d8a0-127">**Aspsms:** Kilit açma/kaldırma menüsünde, bir veya daha fazla kaynaktan yararlanın veya alfasayısal bir kaynağı (tüm ağlar tarafından desteklenmez) seçin.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-127">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="7d8a0-128">Bu değeri daha sonra anahtar `SMSAccountFrom`içinde gizli-Manager aracı ile depolayacağız.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>

### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="7d8a0-129">SMS hizmet için kimlik bilgilerini sağlayın</span><span class="sxs-lookup"><span data-stu-id="7d8a0-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="7d8a0-130">Kullanıcı hesabına ve anahtar ayarlarına erişmek için [Seçenekler modelini](xref:fundamentals/configuration/options) kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

* <span data-ttu-id="7d8a0-131">Güvenli SMS anahtarını getirmek için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="7d8a0-132">Bu örnekte, `SMSoptions` sınıfı *Services/SMSoptions. cs* dosyasında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="7d8a0-133">`SMSAccountIdentification`, `SMSAccountPassword` ve `SMSAccountFrom` [gizli-Manager aracı](xref:security/app-secrets)ile ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7d8a0-134">Örnek:</span><span class="sxs-lookup"><span data-stu-id="7d8a0-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* <span data-ttu-id="7d8a0-135">SMS Sağlayıcısı için NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="7d8a0-136">Paket Yöneticisi Konsolu (çalıştırma PMC'yi gelen):</span><span class="sxs-lookup"><span data-stu-id="7d8a0-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="7d8a0-137">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="7d8a0-137">**Twilio:**</span></span>

`Install-Package Twilio`

<span data-ttu-id="7d8a0-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7d8a0-138">**ASPSMS:**</span></span>

`Install-Package ASPSMS`

* <span data-ttu-id="7d8a0-139">SMS 'yi etkinleştirmek için *Services/MessageServices. cs* dosyasına kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="7d8a0-140">Twilio veya ASPSMS bölüm kullanın:</span><span class="sxs-lookup"><span data-stu-id="7d8a0-140">Use either the Twilio or the ASPSMS section:</span></span>

<span data-ttu-id="7d8a0-141">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="7d8a0-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="7d8a0-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7d8a0-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="7d8a0-143">Başlangıç `SMSoptions` kullanılacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7d8a0-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="7d8a0-144">*Startup.cs*yönteminde hizmet `ConfigureServices` kapsayıcısına `SMSoptions` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7d8a0-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="7d8a0-145">İki öğeli kimlik doğrulamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="7d8a0-145">Enable two-factor authentication</span></span>

<span data-ttu-id="7d8a0-146">*Views/Manage/Index. cshtml* Razor görünüm dosyasını açın ve açıklama karakterlerini kaldırın (Bu nedenle biçimlendirme yok).</span><span class="sxs-lookup"><span data-stu-id="7d8a0-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commented out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="7d8a0-147">İki öğeli kimlik bilgilerinizle oturum açın</span><span class="sxs-lookup"><span data-stu-id="7d8a0-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="7d8a0-148">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="7d8a0-148">Run the app and register a new user</span></span>

![Web uygulama kayıt görünümü Microsoft Edge'de Aç](2fa/_static/login2fa1.png)

* <span data-ttu-id="7d8a0-150">Kullanıcı adınızla, yönetim denetleyicisindeki `Index` eylem yöntemini etkinleştiren ' a dokunun.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="7d8a0-151">Ardından telefon numarası **Ekle** bağlantısına dokunun.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-151">Then tap the phone number **Add** link.</span></span>

![Görünüm yönetme - "Ekle" bağlantısına dokunun](2fa/_static/login2fa2.png)

* <span data-ttu-id="7d8a0-153">Doğrulama kodunu alacak bir telefon numarası ekleyin ve **doğrulama kodu gönder**' e dokunun.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Telefon numarası Sayfası Ekle](2fa/_static/login2fa3.png)

* <span data-ttu-id="7d8a0-155">Doğrulama kodunu içeren bir kısa mesaj alırsınız.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="7d8a0-156">Girin ve **Gönder** 'e dokunun</span><span class="sxs-lookup"><span data-stu-id="7d8a0-156">Enter it and tap **Submit**</span></span>

![Telefon numarası sayfanın doğrulayın](2fa/_static/login2fa4.png)

<span data-ttu-id="7d8a0-158">Kısa mesaj alamazsanız, twilio günlüğü sayfasında bakın.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="7d8a0-159">Telefon numaranızı başarıyla eklendi yönet görünümü gösterir.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-159">The Manage view shows your phone number was added successfully.</span></span>

![Telefon numarası başarıyla eklendi görünümü - yönetme](2fa/_static/login2fa5.png)

* <span data-ttu-id="7d8a0-161">İki öğeli kimlik doğrulamayı etkinleştirmek için **Etkinleştir** ' e dokunun.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Görünüm yönetme - iki öğeli kimlik doğrulamayı etkinleştirme](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="7d8a0-163">İki öğeli kimlik doğrulamasını Sına</span><span class="sxs-lookup"><span data-stu-id="7d8a0-163">Test two-factor authentication</span></span>

* <span data-ttu-id="7d8a0-164">Oturumunuzu kapatın.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-164">Log off.</span></span>

* <span data-ttu-id="7d8a0-165">Oturum aç.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-165">Log in.</span></span>

* <span data-ttu-id="7d8a0-166">Kullanıcı hesabı, iki öğeli kimlik doğrulama, ikinci faktör kimlik doğrulaması sağlamak zorunda etkinleştirdi.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="7d8a0-167">Bu öğreticide, telefon doğrulama etkinleştirmiş olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="7d8a0-168">Yerleşik şablonlar, e-posta ikinci öğe olarak ayarlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="7d8a0-169">QR kodları gibi kimlik doğrulaması için ek ikinci faktör ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="7d8a0-170">**Gönder**' e dokunun.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-170">Tap **Submit**.</span></span>

![Doğrulama kodu görünümü Gönder](2fa/_static/login2fa7.png)

* <span data-ttu-id="7d8a0-172">Mesajla aldığınız kodu girin.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="7d8a0-173">**Bu tarayıcıya** göz atma onay kutusunu tıklatmak, aynı cihaz ve tarayıcıyı kullanırken oturum açmak için 2FA kullanmaya gerek duymasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="7d8a0-174">2FA 'yı etkinleştirmek ve **Bu tarayıcıyı unutmamak** , cihazınıza erişimi olmadığı sürece hesabınıza erişmeye çalışan kötü amaçlı kullanıcılardan güçlü 2FA koruması sağlar.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="7d8a0-175">Düzenli olarak kullandığınız özel cihaz üzerinde bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="7d8a0-176">**Bu tarayıcıyı aklınızda**bulundurarak, düzenli olarak kullanmayan CIHAZLARDAN 2FA 'nın ek güvenliğine sahip olursunuz ve kendi cihazlarınızda 2FA 'yı kullanmaya gerek kalmadan rahatlığını elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Görünüm doğrulayın](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="7d8a0-178">Deneme yanılma saldırılarına karşı korumak için hesap kilitleme</span><span class="sxs-lookup"><span data-stu-id="7d8a0-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="7d8a0-179">Hesap kilitleme 2FA ile önerilir.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="7d8a0-180">Bir kullanıcı bir yerel hesap veya sosyal hesap ile oturum açtıktan sonra başarısız girişimleri 2FA'konumunda depolanır.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="7d8a0-181">En çok başarısız erişim denemesi ulaşıldığında, kullanıcının kilitli (varsayılan: 5 erişim denemesi başarısız olduktan sonra 5 dakika kilitleme).</span><span class="sxs-lookup"><span data-stu-id="7d8a0-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="7d8a0-182">Başarılı bir kimlik doğrulaması başarısız erişim denemesi sayısını sıfırlar ve saatini sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="7d8a0-183">Başarısız olan en fazla erişim denemesi sayısı ve kilitleme süresi [Maxfailedaccessattempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) ve [Defaultlockouttimespan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan)ile ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="7d8a0-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="7d8a0-184">Aşağıdaki hesap kilitleme 10 erişim denemesi başarısız olduktan sonra 10 dakika için yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="7d8a0-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="7d8a0-185">[Passwordsignınasync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) kümesinin `true``lockoutOnFailure` olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="7d8a0-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
