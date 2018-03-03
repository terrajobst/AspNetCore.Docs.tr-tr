---
title: "SMS ile iki öğeli kimlik doğrulama"
author: rick-anderson
description: "ASP.NET Core ile iki öğeli kimlik doğrulamasını (2FA) ayarlamak nasıl gösterir"
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 721c4c20234c7232b509a0cff444538c2cfeb166
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="6ecbd-103">SMS ile iki öğeli kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="6ecbd-103">Two-factor authentication with SMS</span></span>

<span data-ttu-id="6ecbd-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [İsviçre Devs](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="6ecbd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="6ecbd-105">Bu öğretici için ASP.NET Core geçerlidir yalnızca 1.x.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-105">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="6ecbd-106">Bkz: [ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma](xref:security/authentication/identity-enable-qrcodes) ASP.NET Core 2.0 ve daha sonra.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-106">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="6ecbd-107">Bu öğretici, SMS kullanarak iki faktörlü kimlik doğrulamasını (2FA) ayarlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-107">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="6ecbd-108">Yönergeler için verilen [twilio](https://www.twilio.com/) ve [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ancak herhangi bir SMS sağlayıcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-108">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="6ecbd-109">Tamamlamanız önerilir [hesap ve parola kurtarma](accconfirm.md) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-109">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="6ecbd-110">Görünüm [tamamlanan örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="6ecbd-110">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="6ecbd-111">[Karşıdan yükleme](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="6ecbd-111">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="6ecbd-112">Yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="6ecbd-112">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="6ecbd-113">Adlı yeni bir ASP.NET Core web uygulaması oluşturma `Web2FA` bireysel kullanıcı hesapları ile.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-113">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="6ecbd-114">' Ndaki yönergeleri izleyin [zorlamayı SSL bir ASP.NET Core uygulamasında](xref:security/enforcing-ssl) ayarlamak ve SSL gerektirmek için.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-114">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="6ecbd-115">Bir SMS hesabı oluşturma</span><span class="sxs-lookup"><span data-stu-id="6ecbd-115">Create an SMS account</span></span>

<span data-ttu-id="6ecbd-116">Örneğin, bir SMS hesabı oluşturmak [twilio](https://www.twilio.com/) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="6ecbd-116">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="6ecbd-117">Kimlik doğrulama bilgilerini kaydetmek (twilio için: accountSid ve ASPSMS için authToken: Userkey ve parola).</span><span class="sxs-lookup"><span data-stu-id="6ecbd-117">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="6ecbd-118">SMS Sağlayıcısı kimlik bilgilerini açık</span><span class="sxs-lookup"><span data-stu-id="6ecbd-118">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="6ecbd-119">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="6ecbd-119">**Twilio:**</span></span>  
<span data-ttu-id="6ecbd-120">Twilio hesabınızı Pano sekmesinden kopyalama **hesabının SID** ve **kimlik doğrulama belirteci**.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-120">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="6ecbd-121">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="6ecbd-121">**ASPSMS:**</span></span>  
<span data-ttu-id="6ecbd-122">Hesap ayarlarınızı, gitmek **Userkey** ve ile birlikte kopyalayın, **parola**.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-122">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="6ecbd-123">Biz daha sonra bu değerler anahtarları içinde gizli Yöneticisi aracını oturum depolayacaktır `SMSAccountIdentification` ve `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-123">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="6ecbd-124">Senderıd belirtme / gönderen</span><span class="sxs-lookup"><span data-stu-id="6ecbd-124">Specifying SenderID / Originator</span></span>

<span data-ttu-id="6ecbd-125">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="6ecbd-125">**Twilio:**</span></span>  
<span data-ttu-id="6ecbd-126">Sayı sekmesinden, Twilio kopyalama **telefon numarası**.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-126">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="6ecbd-127">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="6ecbd-127">**ASPSMS:**</span></span>  
<span data-ttu-id="6ecbd-128">Kilidini başlatıcılarını menüsünde içinde bir veya daha fazla başlatıcılarını kilidini veya bir alfasayısal başlatanın (tüm ağları tarafından desteklenmez) seçin.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-128">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="6ecbd-129">Bu değer anahtar içindeki gizli Yöneticisi aracıyla depolarız daha sonra `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-129">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="6ecbd-130">SMS hizmet için kimlik bilgilerini sağlayın</span><span class="sxs-lookup"><span data-stu-id="6ecbd-130">Provide credentials for the SMS service</span></span>

<span data-ttu-id="6ecbd-131">Kullanacağız [seçenekleri düzeni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-131">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="6ecbd-132">Güvenli SMS anahtar getirmek için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-132">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="6ecbd-133">Bu örnek için `SMSoptions` sınıfı oluşturulur *Services/SMSoptions.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-133">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="6ecbd-134">Ayarlama `SMSAccountIdentification`, `SMSAccountPassword` ve `SMSAccountFrom` ile [gizli Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="6ecbd-134">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="6ecbd-135">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6ecbd-135">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="6ecbd-136">NuGet paketi için SMS Sağlayıcısı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-136">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="6ecbd-137">Paket Yöneticisi Konsolu (çalıştırmak PMC gelen):</span><span class="sxs-lookup"><span data-stu-id="6ecbd-137">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="6ecbd-138">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="6ecbd-138">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="6ecbd-139">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="6ecbd-139">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="6ecbd-140">Kod ekleme *Services/MessageServices.cs* SMS etkinleştirmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-140">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="6ecbd-141">Twilio veya ASPSMS bölüm kullanın:</span><span class="sxs-lookup"><span data-stu-id="6ecbd-141">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="6ecbd-142">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="6ecbd-142">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="6ecbd-143">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="6ecbd-143">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="6ecbd-144">Başlangıç kullanmak için yapılandırma `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="6ecbd-144">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="6ecbd-145">Ekleme `SMSoptions` hizmet kapsayıcısında için `ConfigureServices` yönteminde *haline*:</span><span class="sxs-lookup"><span data-stu-id="6ecbd-145">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="6ecbd-146">İki faktörlü kimlik doğrulamasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="6ecbd-146">Enable two-factor authentication</span></span>

<span data-ttu-id="6ecbd-147">Açık *Views/Manage/Index.cshtml* Razor görünüm dosyası ve yorum karakterleri (out commnted biçimlendirme yok olacak şekilde) kaldırma.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-147">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="6ecbd-148">İki faktörlü kimlik bilgilerinizle oturum açın</span><span class="sxs-lookup"><span data-stu-id="6ecbd-148">Log in with two-factor authentication</span></span>

* <span data-ttu-id="6ecbd-149">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="6ecbd-149">Run the app and register a new user</span></span>

![Web uygulama kayıt görünümü Microsoft Edge'de Aç](2fa/_static/login2fa1.png)

* <span data-ttu-id="6ecbd-151">Etkinleştirir, kullanıcı adına dokunun `Index` Yönet denetleyicideki eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-151">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="6ecbd-152">Telefon numarası dokunun **Ekle** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-152">Then tap the phone number **Add** link.</span></span>

![Görünüm yönetme](2fa/_static/login2fa2.png)

* <span data-ttu-id="6ecbd-154">Doğrulama kodunu alma ve dokunun telefon numarası ekleme **Gönder doğrulama kodu**.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-154">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Telefon numarası Sayfası Ekle](2fa/_static/login2fa3.png)

* <span data-ttu-id="6ecbd-156">Doğrulama kodunu içeren bir kısa mesaj alırsınız.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-156">You will get a text message with the verification code.</span></span> <span data-ttu-id="6ecbd-157">Girin ve dokunun **Gönder**</span><span class="sxs-lookup"><span data-stu-id="6ecbd-157">Enter it and tap **Submit**</span></span>

![Telefon numarası sayfasında doğrulayın](2fa/_static/login2fa4.png)

<span data-ttu-id="6ecbd-159">SMS mesajı alamazsanız twilio günlük sayfasına bakın.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-159">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="6ecbd-160">Telefon numaranız başarıyla eklendi yönet görünümü gösterir.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-160">The Manage view shows your phone number was added successfully.</span></span>

![Görünüm yönetme](2fa/_static/login2fa5.png)

* <span data-ttu-id="6ecbd-162">Dokunun **etkinleştirmek** iki faktörlü kimlik doğrulamasını etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-162">Tap **Enable** to enable two-factor authentication.</span></span>

![Görünüm yönetme](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="6ecbd-164">İki faktörlü kimlik doğrulamasını Sına</span><span class="sxs-lookup"><span data-stu-id="6ecbd-164">Test two-factor authentication</span></span>

* <span data-ttu-id="6ecbd-165">Oturumunuzu kapatın.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-165">Log off.</span></span>

* <span data-ttu-id="6ecbd-166">Oturum aç.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-166">Log in.</span></span>

* <span data-ttu-id="6ecbd-167">Böylece ikinci faktörlü kimlik doğrulaması sağlamak zorunda iki faktörlü kimlik doğrulaması, kullanıcı hesabı etkinleştirdi.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-167">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="6ecbd-168">Bu öğreticide, telefon doğrulama etkinleştirmiş olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-168">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="6ecbd-169">Yerleşik şablonlar, e-posta ikinci öğe olarak ayarlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-169">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="6ecbd-170">QR kodlarını gibi kimlik doğrulaması için ek ikinci Etkenler ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-170">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="6ecbd-171">Dokunun **gönderme**.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-171">Tap **Submit**.</span></span>

![Doğrulama kodu görünüm Gönder](2fa/_static/login2fa7.png)

* <span data-ttu-id="6ecbd-173">KISA mesajla aldığınız kodu girin.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-173">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="6ecbd-174">Tıklayarak **bu tarayıcı unutmayın** onay kutusunu muaf, aynı aygıt ve tarayıcı kullanarak oturum açmak için 2FA kullanmaya gerek.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-174">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="6ecbd-175">Etkinleştirme 2FA ve tıklayarak **bu tarayıcı unutmayın** aygıtınıza erişimi olmayan sürece, hesabınıza erişmeniz çalışılırken kötü niyetli kullanıcıların güçlü 2FA korumasıyla sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-175">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="6ecbd-176">Düzenli olarak kullanmak istediğiniz özel cihazda bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-176">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="6ecbd-177">Ayarlayarak **bu tarayıcı unutmayın**, düzenli olarak kullanmama aygıtlardan 2FA ek güvenlik alın ve 2FA kendi cihazlarda gitmek zorunda değil üzerinde kolaylık alın.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-177">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Görünüm doğrulayın](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="6ecbd-179">Deneme yanılma saldırılarına karşı korumak için hesap kilitleme</span><span class="sxs-lookup"><span data-stu-id="6ecbd-179">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="6ecbd-180">Hesap kilitleme 2FA ile kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-180">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="6ecbd-181">Bir kullanıcı (yerel hesap veya sosyal hesap aracılığıyla), oturum sonra her başarısız girişimden 2FA konumunda depolanır ve (varsayılan olarak 5) en fazla deneme ulaşıldığında, kullanıcı beş dakika kilitlenmeden (süresiyle kilitleme ayarlayabilirsiniz `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="6ecbd-181">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="6ecbd-182">10 başarısız girişimden sonra 10 dakika kilitlenmesi için hesap yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6ecbd-182">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
