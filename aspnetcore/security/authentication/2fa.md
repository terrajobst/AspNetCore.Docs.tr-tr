---
title: "SMS ile iki öğeli kimlik doğrulama"
author: rick-anderson
description: "ASP.NET Core ile iki öğeli kimlik doğrulamasını (2FA) ayarlamak nasıl gösterir"
keywords: "ASP.NET Core, SMS, kimlik doğrulama, 2FA, iki öğeli kimlik doğrulama, iki faktörlü kimlik doğrulaması"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 15620d89c4db2e74dbcec4707bb2ebc6df916e03
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="2ed1b-104">SMS ile iki öğeli kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="2ed1b-104">Two-factor authentication with SMS</span></span>

<span data-ttu-id="2ed1b-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [İsviçre Devs](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="2ed1b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="2ed1b-106">Bu öğretici için ASP.NET Core geçerlidir yalnızca 1.x.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-106">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="2ed1b-107">Bkz: [ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma](xref:security/authentication/identity-enable-qrcodes) ASP.NET Core 2.0 ve daha sonra.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-107">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="2ed1b-108">Bu öğretici, SMS kullanarak iki faktörlü kimlik doğrulamasını (2FA) ayarlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="2ed1b-109">Yönergeler için verilen [twilio](https://www.twilio.com/) ve [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ancak herhangi bir SMS sağlayıcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="2ed1b-110">Tamamlamanız önerilir [hesap ve parola kurtarma](accconfirm.md) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-110">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="2ed1b-111">Görünüm [tamamlanan örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="2ed1b-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="2ed1b-112">[Karşıdan yükleme](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="2ed1b-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="2ed1b-113">Yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="2ed1b-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="2ed1b-114">Adlı yeni bir ASP.NET Core web uygulaması oluşturma `Web2FA` bireysel kullanıcı hesapları ile.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="2ed1b-115">' Ndaki yönergeleri izleyin [zorlamayı SSL bir ASP.NET Core uygulamasında](xref:security/enforcing-ssl) ayarlamak ve SSL gerektirmek için.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-115">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="2ed1b-116">Bir SMS hesabı oluşturma</span><span class="sxs-lookup"><span data-stu-id="2ed1b-116">Create an SMS account</span></span>

<span data-ttu-id="2ed1b-117">Örneğin, bir SMS hesabı oluşturmak [twilio](https://www.twilio.com/) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="2ed1b-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="2ed1b-118">Kimlik doğrulama bilgilerini kaydetmek (twilio için: accountSid ve ASPSMS için authToken: Userkey ve parola).</span><span class="sxs-lookup"><span data-stu-id="2ed1b-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="2ed1b-119">SMS Sağlayıcısı kimlik bilgilerini açık</span><span class="sxs-lookup"><span data-stu-id="2ed1b-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="2ed1b-120">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="2ed1b-120">**Twilio:**</span></span>  
<span data-ttu-id="2ed1b-121">Twilio hesabınızı Pano sekmesinden kopyalama **hesabının SID** ve **kimlik doğrulama belirteci**.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="2ed1b-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="2ed1b-122">**ASPSMS:**</span></span>  
<span data-ttu-id="2ed1b-123">Hesap ayarlarınızı, gitmek **Userkey** ve ile birlikte kopyalayın, **parola**.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="2ed1b-124">Biz daha sonra bu değerler anahtarları içinde gizli Yöneticisi aracını oturum depolayacaktır `SMSAccountIdentification` ve `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="2ed1b-125">Senderıd belirtme / gönderen</span><span class="sxs-lookup"><span data-stu-id="2ed1b-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="2ed1b-126">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="2ed1b-126">**Twilio:**</span></span>  
<span data-ttu-id="2ed1b-127">Sayı sekmesinden, Twilio kopyalama **telefon numarası**.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-127">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="2ed1b-128">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="2ed1b-128">**ASPSMS:**</span></span>  
<span data-ttu-id="2ed1b-129">Kilidini başlatıcılarını menüsünde içinde bir veya daha fazla başlatıcılarını kilidini veya bir alfasayısal başlatanın (tüm ağları tarafından desteklenmez) seçin.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-129">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="2ed1b-130">Bu değer anahtar içindeki gizli Yöneticisi aracıyla depolarız daha sonra `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-130">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="2ed1b-131">SMS hizmet için kimlik bilgilerini sağlayın</span><span class="sxs-lookup"><span data-stu-id="2ed1b-131">Provide credentials for the SMS service</span></span>

<span data-ttu-id="2ed1b-132">Kullanacağız [seçenekleri düzeni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-132">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="2ed1b-133">Güvenli SMS anahtar getirmek için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-133">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="2ed1b-134">Bu örnek için `SMSoptions` sınıfı oluşturulur *Services/SMSoptions.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-134">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="2ed1b-135">Ayarlama `SMSAccountIdentification`, `SMSAccountPassword` ve `SMSAccountFrom` ile [gizli Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2ed1b-135">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="2ed1b-136">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2ed1b-136">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="2ed1b-137">NuGet paketi için SMS Sağlayıcısı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-137">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="2ed1b-138">Paket Yöneticisi Konsolu (çalıştırmak PMC gelen):</span><span class="sxs-lookup"><span data-stu-id="2ed1b-138">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="2ed1b-139">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="2ed1b-139">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="2ed1b-140">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="2ed1b-140">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="2ed1b-141">Kod ekleme *Services/MessageServices.cs* SMS etkinleştirmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-141">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="2ed1b-142">Twilio veya ASPSMS bölüm kullanın:</span><span class="sxs-lookup"><span data-stu-id="2ed1b-142">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="2ed1b-143">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="2ed1b-143">**Twilio:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="2ed1b-144">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="2ed1b-144">**ASPSMS:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="2ed1b-145">Başlangıç kullanmak için yapılandırma`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="2ed1b-145">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="2ed1b-146">Ekleme `SMSoptions` hizmet kapsayıcısında için `ConfigureServices` yönteminde *haline*:</span><span class="sxs-lookup"><span data-stu-id="2ed1b-146">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="2ed1b-147">İki faktörlü kimlik doğrulamasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="2ed1b-147">Enable two-factor authentication</span></span>

<span data-ttu-id="2ed1b-148">Açık *Views/Manage/Index.cshtml* Razor görünüm dosyası ve yorum karakterleri (out commnted biçimlendirme yok olacak şekilde) kaldırma.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-148">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="2ed1b-149">İki faktörlü kimlik bilgilerinizle oturum açın</span><span class="sxs-lookup"><span data-stu-id="2ed1b-149">Log in with two-factor authentication</span></span>

* <span data-ttu-id="2ed1b-150">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="2ed1b-150">Run the app and register a new user</span></span>

![Web uygulama kayıt görünümü Microsoft Edge'de Aç](2fa/_static/login2fa1.png)

* <span data-ttu-id="2ed1b-152">Etkinleştirir, kullanıcı adına dokunun `Index` Yönet denetleyicideki eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-152">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="2ed1b-153">Telefon numarası dokunun **Ekle** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-153">Then tap the phone number **Add** link.</span></span>

![Görünüm yönetme](2fa/_static/login2fa2.png)

* <span data-ttu-id="2ed1b-155">Doğrulama kodunu alma ve dokunun telefon numarası ekleme **Gönder doğrulama kodu**.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-155">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Telefon numarası Sayfası Ekle](2fa/_static/login2fa3.png)

* <span data-ttu-id="2ed1b-157">Doğrulama kodunu içeren bir kısa mesaj alırsınız.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-157">You will get a text message with the verification code.</span></span> <span data-ttu-id="2ed1b-158">Girin ve dokunun **Gönder**</span><span class="sxs-lookup"><span data-stu-id="2ed1b-158">Enter it and tap **Submit**</span></span>

![Telefon numarası sayfasında doğrulayın](2fa/_static/login2fa4.png)

<span data-ttu-id="2ed1b-160">SMS mesajı alamazsanız twilio günlük sayfasına bakın.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-160">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="2ed1b-161">Telefon numaranız başarıyla eklendi yönet görünümü gösterir.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-161">The Manage view shows your phone number was added successfully.</span></span>

![Görünüm yönetme](2fa/_static/login2fa5.png)

* <span data-ttu-id="2ed1b-163">Dokunun **etkinleştirmek** iki faktörlü kimlik doğrulamasını etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-163">Tap **Enable** to enable two-factor authentication.</span></span>

![Görünüm yönetme](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="2ed1b-165">İki faktörlü kimlik doğrulamasını Sına</span><span class="sxs-lookup"><span data-stu-id="2ed1b-165">Test two-factor authentication</span></span>

* <span data-ttu-id="2ed1b-166">Oturumunuzu kapatın.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-166">Log off.</span></span>

* <span data-ttu-id="2ed1b-167">Oturum aç.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-167">Log in.</span></span>

* <span data-ttu-id="2ed1b-168">Böylece ikinci faktörlü kimlik doğrulaması sağlamak zorunda iki faktörlü kimlik doğrulaması, kullanıcı hesabı etkinleştirdi.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-168">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="2ed1b-169">Bu öğreticide, telefon doğrulama etkinleştirmiş olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-169">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="2ed1b-170">Yerleşik şablonlar, e-posta ikinci öğe olarak ayarlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-170">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="2ed1b-171">QR kodlarını gibi kimlik doğrulaması için ek ikinci Etkenler ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-171">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="2ed1b-172">Dokunun **gönderme**.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-172">Tap **Submit**.</span></span>

![Doğrulama kodu görünüm Gönder](2fa/_static/login2fa7.png)

* <span data-ttu-id="2ed1b-174">KISA mesajla aldığınız kodu girin.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-174">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="2ed1b-175">Tıklayarak **bu tarayıcı unutmayın** onay kutusunu muaf, aynı aygıt ve tarayıcı kullanarak oturum açmak için 2FA kullanmaya gerek.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-175">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="2ed1b-176">Etkinleştirme 2FA ve tıklayarak **bu tarayıcı unutmayın** aygıtınıza erişimi olmayan sürece, hesabınıza erişmeniz çalışılırken kötü niyetli kullanıcıların güçlü 2FA korumasıyla sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-176">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="2ed1b-177">Düzenli olarak kullanmak istediğiniz özel cihazda bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-177">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="2ed1b-178">Ayarlayarak **bu tarayıcı unutmayın**, düzenli olarak kullanmama aygıtlardan 2FA ek güvenlik alın ve 2FA kendi cihazlarda gitmek zorunda değil üzerinde kolaylık alın.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-178">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Görünüm doğrulayın](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="2ed1b-180">Deneme yanılma saldırılarına karşı korumak için hesap kilitleme</span><span class="sxs-lookup"><span data-stu-id="2ed1b-180">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="2ed1b-181">Hesap kilitleme 2FA ile kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-181">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="2ed1b-182">Bir kullanıcı (yerel hesap veya sosyal hesap aracılığıyla), oturum sonra her başarısız girişimden 2FA konumunda depolanır ve (varsayılan olarak 5) en fazla deneme ulaşıldığında, kullanıcı beş dakika kilitlenmeden (süresiyle kilitleme ayarlayabilirsiniz `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="2ed1b-182">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="2ed1b-183">10 başarısız girişimden sonra 10 dakika kilitlenmesi için hesap yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="2ed1b-183">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
