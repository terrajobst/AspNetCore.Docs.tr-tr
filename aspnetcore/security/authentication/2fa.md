---
title: ASP.NET Core SMS ile iki öğeli kimlik doğrulaması
author: rick-anderson
description: Bir ASP.NET Core uygulama ile iki öğeli kimlik doğrulamasını (2FA) ayarlamak öğrenin.
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 1c4acc4e4be593051d30793b7f73ad90ce727283
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="332e0-103">ASP.NET Core SMS ile iki öğeli kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="332e0-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="332e0-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [İsviçre Devs](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="332e0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="332e0-105">Bu öğretici için ASP.NET Core geçerlidir yalnızca 1.x.</span><span class="sxs-lookup"><span data-stu-id="332e0-105">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="332e0-106">Bkz: [ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma](xref:security/authentication/identity-enable-qrcodes) ASP.NET Core 2.0 ve daha sonra.</span><span class="sxs-lookup"><span data-stu-id="332e0-106">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="332e0-107">Bu öğretici, SMS kullanarak iki faktörlü kimlik doğrulamasını (2FA) ayarlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="332e0-107">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="332e0-108">Yönergeler için verilen [twilio](https://www.twilio.com/) ve [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ancak herhangi bir SMS sağlayıcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="332e0-108">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="332e0-109">Tamamlamanız önerilir [hesap ve parola kurtarma](xref:security/authentication/accconfirm) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="332e0-109">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="332e0-110">Görünüm [tamamlanan örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="332e0-110">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="332e0-111">[Karşıdan yükleme](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="332e0-111">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="332e0-112">Yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="332e0-112">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="332e0-113">Adlı yeni bir ASP.NET Core web uygulaması oluşturma `Web2FA` bireysel kullanıcı hesapları ile.</span><span class="sxs-lookup"><span data-stu-id="332e0-113">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="332e0-114">' Ndaki yönergeleri izleyin [Zorla SSL bir ASP.NET Core uygulamasında](xref:security/enforcing-ssl) ayarlamak ve SSL gerektirmek için.</span><span class="sxs-lookup"><span data-stu-id="332e0-114">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="332e0-115">Bir SMS hesabı oluşturma</span><span class="sxs-lookup"><span data-stu-id="332e0-115">Create an SMS account</span></span>

<span data-ttu-id="332e0-116">Örneğin, bir SMS hesabı oluşturmak [twilio](https://www.twilio.com/) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="332e0-116">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="332e0-117">Kimlik doğrulama bilgilerini kaydetmek (twilio için: accountSid ve ASPSMS için authToken: Userkey ve parola).</span><span class="sxs-lookup"><span data-stu-id="332e0-117">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="332e0-118">SMS Sağlayıcısı kimlik bilgilerini açık</span><span class="sxs-lookup"><span data-stu-id="332e0-118">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="332e0-119">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="332e0-119">**Twilio:**</span></span>  
<span data-ttu-id="332e0-120">Twilio hesabınızı Pano sekmesinden kopyalama **hesabının SID** ve **kimlik doğrulama belirteci**.</span><span class="sxs-lookup"><span data-stu-id="332e0-120">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="332e0-121">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="332e0-121">**ASPSMS:**</span></span>  
<span data-ttu-id="332e0-122">Hesap ayarlarınızı, gitmek **Userkey** ve ile birlikte kopyalayın, **parola**.</span><span class="sxs-lookup"><span data-stu-id="332e0-122">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="332e0-123">Biz daha sonra bu değerler anahtarları içinde gizli Yöneticisi aracını oturum depolayacaktır `SMSAccountIdentification` ve `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="332e0-123">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="332e0-124">Senderıd belirtme / gönderen</span><span class="sxs-lookup"><span data-stu-id="332e0-124">Specifying SenderID / Originator</span></span>

<span data-ttu-id="332e0-125">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="332e0-125">**Twilio:**</span></span>  
<span data-ttu-id="332e0-126">Sayı sekmesinden, Twilio kopyalama **telefon numarası**.</span><span class="sxs-lookup"><span data-stu-id="332e0-126">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="332e0-127">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="332e0-127">**ASPSMS:**</span></span>  
<span data-ttu-id="332e0-128">Kilidini başlatıcılarını menüsünde içinde bir veya daha fazla başlatıcılarını kilidini veya bir alfasayısal başlatanın (tüm ağları tarafından desteklenmez) seçin.</span><span class="sxs-lookup"><span data-stu-id="332e0-128">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="332e0-129">Bu değer anahtar içindeki gizli Yöneticisi aracıyla depolarız daha sonra `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="332e0-129">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="332e0-130">SMS hizmet için kimlik bilgilerini sağlayın</span><span class="sxs-lookup"><span data-stu-id="332e0-130">Provide credentials for the SMS service</span></span>

<span data-ttu-id="332e0-131">Kullanacağız [seçenekleri düzeni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için.</span><span class="sxs-lookup"><span data-stu-id="332e0-131">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="332e0-132">Güvenli SMS anahtar getirmek için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="332e0-132">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="332e0-133">Bu örnek için `SMSoptions` sınıfı oluşturulur *Services/SMSoptions.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="332e0-133">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="332e0-134">Ayarlama `SMSAccountIdentification`, `SMSAccountPassword` ve `SMSAccountFrom` ile [gizli Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="332e0-134">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="332e0-135">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="332e0-135">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="332e0-136">NuGet paketi için SMS Sağlayıcısı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="332e0-136">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="332e0-137">Paket Yöneticisi Konsolu (çalıştırmak PMC gelen):</span><span class="sxs-lookup"><span data-stu-id="332e0-137">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="332e0-138">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="332e0-138">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="332e0-139">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="332e0-139">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="332e0-140">Kod ekleme *Services/MessageServices.cs* SMS etkinleştirmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="332e0-140">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="332e0-141">Twilio veya ASPSMS bölüm kullanın:</span><span class="sxs-lookup"><span data-stu-id="332e0-141">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="332e0-142">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="332e0-142">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="332e0-143">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="332e0-143">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="332e0-144">Başlangıç kullanmak için yapılandırma `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="332e0-144">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="332e0-145">Ekleme `SMSoptions` hizmet kapsayıcısında için `ConfigureServices` yönteminde *haline*:</span><span class="sxs-lookup"><span data-stu-id="332e0-145">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="332e0-146">İki faktörlü kimlik doğrulamasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="332e0-146">Enable two-factor authentication</span></span>

<span data-ttu-id="332e0-147">Açık *Views/Manage/Index.cshtml* Razor görünüm dosyası ve yorum karakterleri (out commnted biçimlendirme yok olacak şekilde) kaldırma.</span><span class="sxs-lookup"><span data-stu-id="332e0-147">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="332e0-148">İki faktörlü kimlik bilgilerinizle oturum açın</span><span class="sxs-lookup"><span data-stu-id="332e0-148">Log in with two-factor authentication</span></span>

* <span data-ttu-id="332e0-149">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="332e0-149">Run the app and register a new user</span></span>

![Web uygulama kayıt görünümü Microsoft Edge'de Aç](2fa/_static/login2fa1.png)

* <span data-ttu-id="332e0-151">Etkinleştirir, kullanıcı adına dokunun `Index` Yönet denetleyicideki eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="332e0-151">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="332e0-152">Telefon numarası dokunun **Ekle** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="332e0-152">Then tap the phone number **Add** link.</span></span>

![Görünüm yönetme](2fa/_static/login2fa2.png)

* <span data-ttu-id="332e0-154">Doğrulama kodunu alma ve dokunun telefon numarası ekleme **Gönder doğrulama kodu**.</span><span class="sxs-lookup"><span data-stu-id="332e0-154">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Telefon numarası Sayfası Ekle](2fa/_static/login2fa3.png)

* <span data-ttu-id="332e0-156">Doğrulama kodunu içeren bir kısa mesaj alırsınız.</span><span class="sxs-lookup"><span data-stu-id="332e0-156">You will get a text message with the verification code.</span></span> <span data-ttu-id="332e0-157">Girin ve dokunun **Gönder**</span><span class="sxs-lookup"><span data-stu-id="332e0-157">Enter it and tap **Submit**</span></span>

![Telefon numarası sayfasında doğrulayın](2fa/_static/login2fa4.png)

<span data-ttu-id="332e0-159">SMS mesajı alamazsanız twilio günlük sayfasına bakın.</span><span class="sxs-lookup"><span data-stu-id="332e0-159">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="332e0-160">Telefon numaranız başarıyla eklendi yönet görünümü gösterir.</span><span class="sxs-lookup"><span data-stu-id="332e0-160">The Manage view shows your phone number was added successfully.</span></span>

![Görünüm yönetme](2fa/_static/login2fa5.png)

* <span data-ttu-id="332e0-162">Dokunun **etkinleştirmek** iki faktörlü kimlik doğrulamasını etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="332e0-162">Tap **Enable** to enable two-factor authentication.</span></span>

![Görünüm yönetme](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="332e0-164">İki faktörlü kimlik doğrulamasını Sına</span><span class="sxs-lookup"><span data-stu-id="332e0-164">Test two-factor authentication</span></span>

* <span data-ttu-id="332e0-165">Oturumunuzu kapatın.</span><span class="sxs-lookup"><span data-stu-id="332e0-165">Log off.</span></span>

* <span data-ttu-id="332e0-166">Oturum aç.</span><span class="sxs-lookup"><span data-stu-id="332e0-166">Log in.</span></span>

* <span data-ttu-id="332e0-167">Böylece ikinci faktörlü kimlik doğrulaması sağlamak zorunda iki faktörlü kimlik doğrulaması, kullanıcı hesabı etkinleştirdi.</span><span class="sxs-lookup"><span data-stu-id="332e0-167">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="332e0-168">Bu öğreticide, telefon doğrulama etkinleştirmiş olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="332e0-168">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="332e0-169">Yerleşik şablonlar, e-posta ikinci öğe olarak ayarlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="332e0-169">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="332e0-170">QR kodlarını gibi kimlik doğrulaması için ek ikinci Etkenler ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="332e0-170">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="332e0-171">Dokunun **gönderme**.</span><span class="sxs-lookup"><span data-stu-id="332e0-171">Tap **Submit**.</span></span>

![Doğrulama kodu görünüm Gönder](2fa/_static/login2fa7.png)

* <span data-ttu-id="332e0-173">KISA mesajla aldığınız kodu girin.</span><span class="sxs-lookup"><span data-stu-id="332e0-173">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="332e0-174">Tıklayarak **bu tarayıcı unutmayın** onay kutusunu muaf, aynı aygıt ve tarayıcı kullanarak oturum açmak için 2FA kullanmaya gerek.</span><span class="sxs-lookup"><span data-stu-id="332e0-174">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="332e0-175">Etkinleştirme 2FA ve tıklayarak **bu tarayıcı unutmayın** aygıtınıza erişimi olmayan sürece, hesabınıza erişmeniz çalışılırken kötü niyetli kullanıcıların güçlü 2FA korumasıyla sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="332e0-175">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="332e0-176">Düzenli olarak kullanmak istediğiniz özel cihazda bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="332e0-176">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="332e0-177">Ayarlayarak **bu tarayıcı unutmayın**, düzenli olarak kullanmama aygıtlardan 2FA ek güvenlik alın ve 2FA kendi cihazlarda gitmek zorunda değil üzerinde kolaylık alın.</span><span class="sxs-lookup"><span data-stu-id="332e0-177">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Görünüm doğrulayın](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="332e0-179">Deneme yanılma saldırılarına karşı korumak için hesap kilitleme</span><span class="sxs-lookup"><span data-stu-id="332e0-179">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="332e0-180">Hesap kilitleme ile 2FA önerilir.</span><span class="sxs-lookup"><span data-stu-id="332e0-180">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="332e0-181">Bir kullanıcı bir yerel hesabı veya sosyal hesabı ile oturum açtıktan sonra her başarısız girişimden 2FA konumunda depolanır.</span><span class="sxs-lookup"><span data-stu-id="332e0-181">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="332e0-182">En fazla başarısız erişim denemesi ulaşıldığında, kullanıcının kilitli (varsayılan: 5 erişim denemesi başarısız olduktan sonra 5 dakika kilitleme).</span><span class="sxs-lookup"><span data-stu-id="332e0-182">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="332e0-183">Başarılı bir kimlik doğrulaması başarısız erişim denemesi sayısını sıfırlar ve saatini sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="332e0-183">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="332e0-184">En fazla başarısız erişim denemesi ve kilitleme süresini ayarlayabilirsiniz ile [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) ve [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="332e0-184">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="332e0-185">Aşağıdaki hesap kilitleme 10 erişim denemesi başarısız olduktan sonra 10 dakika için yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="332e0-185">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="332e0-186">Onaylayın [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ayarlar `lockoutOnFailure` için `true`:</span><span class="sxs-lookup"><span data-stu-id="332e0-186">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
