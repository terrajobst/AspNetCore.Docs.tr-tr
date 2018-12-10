---
title: ASP.NET Core SMS ile iki öğeli kimlik doğrulama
author: rick-anderson
description: İki öğeli kimlik doğrulamayı (2FA) ile ASP.NET Core uygulaması ayarlama konusunda bilgi edinin.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 6f20928b0dec9b235fa17c1b44c81a48d031e9e0
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121667"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="5a20f-103">ASP.NET Core SMS ile iki öğeli kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="5a20f-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="5a20f-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [İsviçre geliştiriciler](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="5a20f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="5a20f-105">Kullanarak bir zamana bağlı kerelik parola algoritması (TOTP), iki öğeli kimlik doğrulamayı (2FA) kimlik doğrulayıcısı uygulamalarını önerilen yaklaşımı 2FA için sektöre var.</span><span class="sxs-lookup"><span data-stu-id="5a20f-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="5a20f-106">2fa'yı kullanarak TOTP SMS 2FA için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="5a20f-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="5a20f-107">Daha fazla bilgi için [ASP.NET core'da TOTP authenticator uygulamaları için etkinleştirme QR kodu oluşturmayı](xref:security/authentication/identity-enable-qrcodes) ASP.NET Core 2.0 ve üzeri.</span><span class="sxs-lookup"><span data-stu-id="5a20f-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="5a20f-108">Bu öğreticide, SMS kullanarak iki öğeli kimlik doğrulamasını (2FA) ayarlama işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="5a20f-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="5a20f-109">Yönergeler için verilir [twilio](https://www.twilio.com/) ve [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ancak herhangi bir SMS Sağlayıcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a20f-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="5a20f-110">Tamamlamanız önerilir [hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="5a20f-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="5a20f-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="5a20f-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="5a20f-112">[Karşıdan yükleme](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="5a20f-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="5a20f-113">Yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="5a20f-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="5a20f-114">Adlı yeni bir ASP.NET Core web uygulaması oluşturma `Web2FA` bireysel kullanıcı hesapları ile.</span><span class="sxs-lookup"><span data-stu-id="5a20f-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="5a20f-115">Bölümündeki yönergeleri [SSL'yi zorunlu bir ASP.NET Core uygulaması](xref:security/enforcing-ssl) ayarlamak ve SSL gerektirmek için.</span><span class="sxs-lookup"><span data-stu-id="5a20f-115">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="5a20f-116">SMS hesap oluşturma</span><span class="sxs-lookup"><span data-stu-id="5a20f-116">Create an SMS account</span></span>

<span data-ttu-id="5a20f-117">Örneğin, bir SMS hesap oluşturma [twilio](https://www.twilio.com/) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="5a20f-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="5a20f-118">Kimlik doğrulama bilgileri kaydetmek (twilio'dan: accountSid ve ASPSMS için bir authToken: Userkey ve parola).</span><span class="sxs-lookup"><span data-stu-id="5a20f-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="5a20f-119">SMS Sağlayıcısı kimlik bilgilerini başarınızda</span><span class="sxs-lookup"><span data-stu-id="5a20f-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="5a20f-120">**Twilio:** Twilio hesabınızın Pano sekmesinden kopyalama **hesap SID'si** ve **kimlik doğrulama belirteci**.</span><span class="sxs-lookup"><span data-stu-id="5a20f-120">**Twilio:** From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="5a20f-121">**ASPSMS:** , hesap ayarlarınıza gidin **Userkey** ve birlikte kopyalayın, **parola**.</span><span class="sxs-lookup"><span data-stu-id="5a20f-121">**ASPSMS:** From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="5a20f-122">Daha sonra bu değerleri gizli dizi Yöneticisi Aracı anahtarları içinde oturum depolarız `SMSAccountIdentification` ve `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="5a20f-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="5a20f-123">Senderıd belirtme / Düzenleyicisi</span><span class="sxs-lookup"><span data-stu-id="5a20f-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="5a20f-124">**Twilio:** numaraları sekmesinden, Twilio kopyalama **telefon numarası**.</span><span class="sxs-lookup"><span data-stu-id="5a20f-124">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="5a20f-125">**ASPSMS:** kilidini düzenleyicileri menüsünde içinde bir veya daha fazla düzenleyicileri kilidini veya bir alfasayısal gönderen (tüm ağlar tarafından desteklenmez) seçin.</span><span class="sxs-lookup"><span data-stu-id="5a20f-125">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="5a20f-126">Gizli dizi Yöneticisi Aracı anahtarı içinde bu değer daha sonra depolarız `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="5a20f-126">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="5a20f-127">SMS hizmet için kimlik bilgilerini sağlayın</span><span class="sxs-lookup"><span data-stu-id="5a20f-127">Provide credentials for the SMS service</span></span>

<span data-ttu-id="5a20f-128">Kullanacağız [seçenekleri deseni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için.</span><span class="sxs-lookup"><span data-stu-id="5a20f-128">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

   * <span data-ttu-id="5a20f-129">Güvenli SMS anahtarını getirmek için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5a20f-129">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="5a20f-130">Bu örnek için `SMSoptions` sınıf oluşturulduğu *Services/SMSoptions.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="5a20f-130">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="5a20f-131">Ayarlama `SMSAccountIdentification`, `SMSAccountPassword` ve `SMSAccountFrom` ile [gizli dizi Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="5a20f-131">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="5a20f-132">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5a20f-132">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="5a20f-133">SMS Sağlayıcısı için NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5a20f-133">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="5a20f-134">Paket Yöneticisi Konsolu (çalıştırma PMC'yi gelen):</span><span class="sxs-lookup"><span data-stu-id="5a20f-134">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="5a20f-135">**Twilio:**
`Install-Package Twilio`</span><span class="sxs-lookup"><span data-stu-id="5a20f-135">**Twilio:**
`Install-Package Twilio`</span></span>

<span data-ttu-id="5a20f-136">**ASPSMS:**
`Install-Package ASPSMS`</span><span class="sxs-lookup"><span data-stu-id="5a20f-136">**ASPSMS:**
`Install-Package ASPSMS`</span></span>


* <span data-ttu-id="5a20f-137">Kodda *Services/MessageServices.cs* SMS etkinleştirmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="5a20f-137">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="5a20f-138">Twilio veya ASPSMS bölüm kullanın:</span><span class="sxs-lookup"><span data-stu-id="5a20f-138">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="5a20f-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="5a20f-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="5a20f-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="5a20f-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="5a20f-141">Başlangıç kullanmak için yapılandırma `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="5a20f-141">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="5a20f-142">Ekleme `SMSoptions` hizmet kapsayıcısını `ConfigureServices` yönteminde *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5a20f-142">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="5a20f-143">İki öğeli kimlik doğrulamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="5a20f-143">Enable two-factor authentication</span></span>

<span data-ttu-id="5a20f-144">Açık *Views/Manage/Index.cshtml* Razor görünüm dosyası ve yorum karakterleri (hiçbir işaretleme bırakmayın olduğundan) kaldırın.</span><span class="sxs-lookup"><span data-stu-id="5a20f-144">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="5a20f-145">İki öğeli kimlik bilgilerinizle oturum açın</span><span class="sxs-lookup"><span data-stu-id="5a20f-145">Log in with two-factor authentication</span></span>

* <span data-ttu-id="5a20f-146">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="5a20f-146">Run the app and register a new user</span></span>

![Web uygulama kayıt görünümü Microsoft Edge'de Aç](2fa/_static/login2fa1.png)

* <span data-ttu-id="5a20f-148">Etkinleştirir, kullanıcı adına dokunun `Index` Yönet denetleyicideki eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5a20f-148">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="5a20f-149">Telefon numarası'e dokunun **Ekle** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="5a20f-149">Then tap the phone number **Add** link.</span></span>

![Görünüm yönetme - "Ekle" bağlantısına dokunun](2fa/_static/login2fa2.png)

* <span data-ttu-id="5a20f-151">Doğrulama kodu almak ve dokunun telefon numarası ekleme **doğrulama kodu Gönder**.</span><span class="sxs-lookup"><span data-stu-id="5a20f-151">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Telefon numarası Sayfası Ekle](2fa/_static/login2fa3.png)

* <span data-ttu-id="5a20f-153">Doğrulama kodunu içeren bir kısa mesaj alırsınız.</span><span class="sxs-lookup"><span data-stu-id="5a20f-153">You will get a text message with the verification code.</span></span> <span data-ttu-id="5a20f-154">Girin ve dokunun **Gönder**</span><span class="sxs-lookup"><span data-stu-id="5a20f-154">Enter it and tap **Submit**</span></span>

![Telefon numarası sayfanın doğrulayın](2fa/_static/login2fa4.png)

<span data-ttu-id="5a20f-156">Kısa mesaj alamazsanız, twilio günlüğü sayfasında bakın.</span><span class="sxs-lookup"><span data-stu-id="5a20f-156">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="5a20f-157">Telefon numaranızı başarıyla eklendi yönet görünümü gösterir.</span><span class="sxs-lookup"><span data-stu-id="5a20f-157">The Manage view shows your phone number was added successfully.</span></span>

![Telefon numarası başarıyla eklendi görünümü - yönetme](2fa/_static/login2fa5.png)

* <span data-ttu-id="5a20f-159">Dokunun **etkinleştirme** iki öğeli kimlik doğrulamasını etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="5a20f-159">Tap **Enable** to enable two-factor authentication.</span></span>

![Görünüm yönetme - iki öğeli kimlik doğrulamayı etkinleştirme](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="5a20f-161">İki öğeli kimlik doğrulamasını Sına</span><span class="sxs-lookup"><span data-stu-id="5a20f-161">Test two-factor authentication</span></span>

* <span data-ttu-id="5a20f-162">Oturumunuzu kapatın.</span><span class="sxs-lookup"><span data-stu-id="5a20f-162">Log off.</span></span>

* <span data-ttu-id="5a20f-163">Oturum aç.</span><span class="sxs-lookup"><span data-stu-id="5a20f-163">Log in.</span></span>

* <span data-ttu-id="5a20f-164">Kullanıcı hesabı, iki öğeli kimlik doğrulama, ikinci faktör kimlik doğrulaması sağlamak zorunda etkinleştirdi.</span><span class="sxs-lookup"><span data-stu-id="5a20f-164">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="5a20f-165">Bu öğreticide, telefon doğrulama etkinleştirmiş olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5a20f-165">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="5a20f-166">Yerleşik şablonlar, e-posta ikinci öğe olarak ayarlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="5a20f-166">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="5a20f-167">QR kodları gibi kimlik doğrulaması için ek ikinci faktör ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a20f-167">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="5a20f-168">Dokunun **gönderme**.</span><span class="sxs-lookup"><span data-stu-id="5a20f-168">Tap **Submit**.</span></span>

![Doğrulama kodu görünümü Gönder](2fa/_static/login2fa7.png)

* <span data-ttu-id="5a20f-170">Mesajla aldığınız kodu girin.</span><span class="sxs-lookup"><span data-stu-id="5a20f-170">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="5a20f-171">Tıklayarak **bu tarayıcı Hatırlansın** onay kutusunu muaf, 2FA aynı cihaz ve tarayıcı kullanarak oturum açmak için kullandığınız gerek.</span><span class="sxs-lookup"><span data-stu-id="5a20f-171">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="5a20f-172">2fa'yı etkinleştirme ve tıklayarak **bu tarayıcı Hatırlansın** cihazınıza erişiminiz yoksa sürece, hesabınıza erişmeye çalışırken kötü niyetli kullanıcıların güçlü 2FA koruma sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="5a20f-172">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="5a20f-173">Düzenli olarak kullandığınız özel cihaz üzerinde bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a20f-173">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="5a20f-174">Ayarlayarak **bu tarayıcı Hatırlansın**, düzenli olarak kullanmadığınız cihazlardan 2FA eklenen güvenliğini size ve kendi cihazlarında 2FA gitmek zorunda değil üzerinde kolaylık alın.</span><span class="sxs-lookup"><span data-stu-id="5a20f-174">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Görünüm doğrulayın](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="5a20f-176">Deneme yanılma saldırılarına karşı korumak için hesap kilitleme</span><span class="sxs-lookup"><span data-stu-id="5a20f-176">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="5a20f-177">Hesap kilitleme 2FA ile önerilir.</span><span class="sxs-lookup"><span data-stu-id="5a20f-177">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="5a20f-178">Bir kullanıcı bir yerel hesap veya sosyal hesap ile oturum açtıktan sonra başarısız girişimleri 2FA'konumunda depolanır.</span><span class="sxs-lookup"><span data-stu-id="5a20f-178">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="5a20f-179">En çok başarısız erişim denemesi ulaşıldığında, kullanıcının kilitli (varsayılan: 5 erişim denemesi başarısız olduktan sonra 5 dakika kilitleme).</span><span class="sxs-lookup"><span data-stu-id="5a20f-179">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="5a20f-180">Başarılı bir kimlik doğrulaması başarısız erişim denemesi sayısını sıfırlar ve saatini sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="5a20f-180">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="5a20f-181">En fazla erişim denemesi başarısız oldu ve kilitleme süresi ile ayarlanabilir [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) ve [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="5a20f-181">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="5a20f-182">Aşağıdaki hesap kilitleme 10 erişim denemesi başarısız olduktan sonra 10 dakika için yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="5a20f-182">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="5a20f-183">Onaylayın [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ayarlar `lockoutOnFailure` için `true`:</span><span class="sxs-lookup"><span data-stu-id="5a20f-183">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
