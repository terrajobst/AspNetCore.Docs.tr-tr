---
title: Hesap onaylama ve parola kurtarma ASP.NET Core
author: rick-anderson
description: E-posta onayı ve parola sıfırlama ile ASP.NET Core uygulaması oluşturmayı öğrenin.
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 802ba446af04df6a35ac73187ad693b8ec80c654
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814845"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="7f059-103">Hesap onaylama ve parola kurtarma ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f059-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="7f059-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="7f059-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="7f059-105">Bu öğretici, e-posta onayı ve parola sıfırlama ile ASP.NET Core uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7f059-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="7f059-106">Bu öğretici **değil** başına konu.</span><span class="sxs-lookup"><span data-stu-id="7f059-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="7f059-107">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="7f059-107">You should be familiar with:</span></span>

* [<span data-ttu-id="7f059-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f059-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="7f059-109">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="7f059-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="7f059-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7f059-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="7f059-111">Bkz: [bu PDF dosyası](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core 1.1 sürümü için.</span><span class="sxs-lookup"><span data-stu-id="7f059-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="7f059-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7f059-112">Prerequisites</span></span>

[<span data-ttu-id="7f059-113">.NET core 3.0 SDK veya üzeri</span><span class="sxs-lookup"><span data-stu-id="7f059-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="7f059-114">Oluşturma ve kimlik doğrulaması ile bir web uygulamasını test etme</span><span class="sxs-lookup"><span data-stu-id="7f059-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="7f059-115">Kimlik doğrulaması ile bir web uygulaması oluşturmak için aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7f059-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="7f059-116">Uygulamayı çalıştırın, seçin **kaydetme** bağlamak ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="7f059-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="7f059-117">Kaydedildikten sonra yönlendirilirsiniz için `/Identity/Account/RegisterConfirmation` e-posta onayı benzetimini yapmak için bir bağlantı içeren bir sayfa:</span><span class="sxs-lookup"><span data-stu-id="7f059-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="7f059-118">Seçin `Click here to confirm your account` bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7f059-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="7f059-119">Seçin **oturum açma** bağlamak ve oturum açma ile aynı kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="7f059-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="7f059-120">Seçin `Hello YourEmail@provider.com!` için yeniden yönlendiren bir bağlantı `/Identity/Account/Manage/PersonalData` sayfası.</span><span class="sxs-lookup"><span data-stu-id="7f059-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="7f059-121">Seçin **kişisel verileri** sekmesinde solda ve ardından **Sil**.</span><span class="sxs-lookup"><span data-stu-id="7f059-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="7f059-122">Bir e-posta sağlayıcısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7f059-122">Configure an email provider</span></span>

<span data-ttu-id="7f059-123">Bu öğreticide [SendGrid](https://sendgrid.com) e-posta göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7f059-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="7f059-124">SendGrid hesabı ve e-posta göndermek için anahtar ihtiyacınız var.</span><span class="sxs-lookup"><span data-stu-id="7f059-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="7f059-125">Diğer e-posta sağlayıcılarının kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-125">You can use other email providers.</span></span> <span data-ttu-id="7f059-126">E-posta göndermek için SendGrid veya başka bir e-posta hizmeti kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="7f059-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="7f059-127">SMTP güvenli ve doğru bir şekilde ayarlamak zordur.</span><span class="sxs-lookup"><span data-stu-id="7f059-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="7f059-128">Güvenli e-posta anahtarı almak için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7f059-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="7f059-129">Bu örnek için oluşturma *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="7f059-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="7f059-130">SendGrid kullanıcı parolaları yapılandırın</span><span class="sxs-lookup"><span data-stu-id="7f059-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="7f059-131">Ayarlama `SendGridUser` ve `SendGridKey` ile [gizli dizi Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7f059-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7f059-132">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7f059-132">For example:</span></span>

```console
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="7f059-133">Windows üzerinde gizli dizi Yöneticisi'ni anahtar/değer çiftleri olarak depolar. bir *secrets.json* dosyası `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` dizin.</span><span class="sxs-lookup"><span data-stu-id="7f059-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="7f059-134">İçeriğini *secrets.json* olmayan dosya şifrelenmiş.</span><span class="sxs-lookup"><span data-stu-id="7f059-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="7f059-135">Aşağıdaki biçimlendirme gösterildiği *secrets.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="7f059-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="7f059-136">`SendGridKey` Değer kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="7f059-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="7f059-137">Daha fazla bilgi için [seçenekleri deseni](xref:fundamentals/configuration/options) ve [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7f059-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="7f059-138">SendGrid yükleyin</span><span class="sxs-lookup"><span data-stu-id="7f059-138">Install SendGrid</span></span>

<span data-ttu-id="7f059-139">Bu öğretici, e-posta bildirimleri aracılığıyla ekleneceği gösterilmiştir [SendGrid](https://sendgrid.com/), ancak e-posta SMTP veya başka mekanizmalar kullanılarak gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="7f059-140">Yükleme `SendGrid` NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="7f059-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f059-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f059-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7f059-142">Paket Yöneticisi konsolundan aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="7f059-142">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7f059-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7f059-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7f059-144">Konsolundan aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="7f059-144">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="7f059-145">Bkz: [SendGrid ile ücretsiz olarak kullanmaya başlayın](https://sendgrid.com/free/) ücretsiz SendGrid hesabı kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="7f059-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="7f059-146">IEmailSender uygulayın</span><span class="sxs-lookup"><span data-stu-id="7f059-146">Implement IEmailSender</span></span>

<span data-ttu-id="7f059-147">Uygulama için `IEmailSender`, oluşturma *Services/EmailSender.cs* kodu aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="7f059-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="7f059-148">Başlangıç e-posta destekleyecek şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7f059-148">Configure startup to support email</span></span>

<span data-ttu-id="7f059-149">Aşağıdaki kodu ekleyin `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7f059-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="7f059-150">Ekleme `EmailSender` geçici bir hizmet olarak.</span><span class="sxs-lookup"><span data-stu-id="7f059-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="7f059-151">Kayıt `AuthMessageSenderOptions` yapılandırma örneği.</span><span class="sxs-lookup"><span data-stu-id="7f059-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="7f059-152">Kaydetme, e-posta onayı ve parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="7f059-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="7f059-153">Web uygulamasını çalıştırın ve hesap onaylama ve parola kurtarma akışı test edin.</span><span class="sxs-lookup"><span data-stu-id="7f059-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="7f059-154">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="7f059-154">Run the app and register a new user</span></span>
* <span data-ttu-id="7f059-155">Hesap onay bağlantısı için e-postanızı kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="7f059-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="7f059-156">Bkz: [hata ayıklama, e-posta](#debug) e-posta alırsanız yok.</span><span class="sxs-lookup"><span data-stu-id="7f059-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="7f059-157">E-postanızı doğrulamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7f059-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="7f059-158">E-postanıza ve parola ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="7f059-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="7f059-159">Oturumu kapatın.</span><span class="sxs-lookup"><span data-stu-id="7f059-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="7f059-160">Test parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="7f059-160">Test password reset</span></span>

* <span data-ttu-id="7f059-161">Oturum açmadıysanız, seçin **oturum kapatma**.</span><span class="sxs-lookup"><span data-stu-id="7f059-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="7f059-162">Seçin **oturum** seçin ve bağlama **parolanızı mı unuttunuz?** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7f059-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="7f059-163">Hesap kaydolmak için kullandığınız e-posta girin.</span><span class="sxs-lookup"><span data-stu-id="7f059-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="7f059-164">Parolanızı sıfırlamak için bir bağlantı içeren bir e-posta gönderilir.</span><span class="sxs-lookup"><span data-stu-id="7f059-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="7f059-165">E-postanızı kontrol edin ve parolanızı sıfırlamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7f059-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="7f059-166">Parolanızı başarıyla sıfırladıktan sonra e-posta ve yeni bir parola ile oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="7f059-167">E-posta ve etkinlik zaman aşımı değiştirme</span><span class="sxs-lookup"><span data-stu-id="7f059-167">Change email and activity timeout</span></span>

<span data-ttu-id="7f059-168">Varsayılan boş durma zaman aşımı 14 gündür.</span><span class="sxs-lookup"><span data-stu-id="7f059-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="7f059-169">Aşağıdaki kod 5 gün etkin olmama zaman aşımı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="7f059-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="7f059-170">Tüm veri koruma simge lifespans değiştirme</span><span class="sxs-lookup"><span data-stu-id="7f059-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="7f059-171">Aşağıdaki kod tüm veri koruma belirteçleri zaman aşımı süresi 3 saat olarak değiştirir:</span><span class="sxs-lookup"><span data-stu-id="7f059-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="7f059-172">Yerleşik kimlik kullanıcı belirteçlerinde (bkz [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) sahip bir [günlük bir zaman aşımı](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7f059-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="7f059-173">E-posta belirteci ömrü değiştirme</span><span class="sxs-lookup"><span data-stu-id="7f059-173">Change the email token lifespan</span></span>

<span data-ttu-id="7f059-174">Varsayılan belirteç ömrü [kimlik kullanıcı belirteçleri](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) olduğu [bir gün](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7f059-174">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="7f059-175">Bu bölümde, e-posta belirteci ömrü değiştirme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7f059-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="7f059-176">Özel bir ekleme [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) ve <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="7f059-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="7f059-177">Özel sağlayıcı hizmet kapsayıcıya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7f059-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="7f059-178">Onay e-postası gönder</span><span class="sxs-lookup"><span data-stu-id="7f059-178">Resend email confirmation</span></span>

<span data-ttu-id="7f059-179">Bkz: [bu GitHub sorunu](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="7f059-179">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="7f059-180">E-posta hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="7f059-180">Debug email</span></span>

<span data-ttu-id="7f059-181">E-posta çalışma erişemiyorsanız:</span><span class="sxs-lookup"><span data-stu-id="7f059-181">If you can't get email working:</span></span>

* <span data-ttu-id="7f059-182">Bir kesim noktası `EmailSender.Execute` doğrulamak için `SendGridClient.SendEmailAsync` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="7f059-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="7f059-183">Oluşturma bir [e-posta göndermek için konsol uygulaması](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) benzer bir kod kullanarak `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="7f059-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="7f059-184">Gözden geçirme [e-posta etkinlik](https://sendgrid.com/docs/User_Guide/email_activity.html) sayfası.</span><span class="sxs-lookup"><span data-stu-id="7f059-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="7f059-185">İstenmeyen posta klasörünüzü kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="7f059-185">Check your spam folder.</span></span>
* <span data-ttu-id="7f059-186">Farklı bir e-posta sağlayıcısı (Microsoft, Yahoo, Gmail, vb.) başka bir e-posta diğer adı deneyin</span><span class="sxs-lookup"><span data-stu-id="7f059-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="7f059-187">Farklı bir e-posta hesaplarına göndermeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="7f059-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="7f059-188">**En iyi güvenlik uygulaması** için **değil** test ve geliştirme üretim gizli dizileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="7f059-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="7f059-189">Uygulamayı Azure'a yayımlama, SendGrid gizli dizileri Azure Web uygulaması Portalı'ndaki uygulama ayarları olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7f059-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="7f059-190">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="7f059-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="7f059-191">Sosyal ve yerel oturum açma hesabını birleştirin</span><span class="sxs-lookup"><span data-stu-id="7f059-191">Combine social and local login accounts</span></span>

<span data-ttu-id="7f059-192">Bu bölümde tamamlamak için önce bir dış kimlik doğrulama sağlayıcısı etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="7f059-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="7f059-193">Bkz: [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7f059-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="7f059-194">Yerel hem de sosyal hesaplar, e-posta bağlantısına tıklayarak birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="7f059-195">Aşağıdaki sırayla "RickAndMSFT@gmail.com" ilk olarak bir yerel oturum açın; oluşturulur ancak, bir sosyal oturum açma ilk hesap oluşturun, sonra bir yerel oturum açma ekleme.</span><span class="sxs-lookup"><span data-stu-id="7f059-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web uygulaması: RickAndMSFT@gmail.com kimliği doğrulanmış kullanıcı](accconfirm/_static/rick.png)

<span data-ttu-id="7f059-197">Tıklayarak **Yönet** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7f059-197">Click on the **Manage** link.</span></span> <span data-ttu-id="7f059-198">Bu hesapla ilişkili 0 dış (sosyal oturum açma bilgileri) unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7f059-198">Note the 0 external (social logins) associated with this account.</span></span>

![Görünümü yönetme](accconfirm/_static/manage.png)

<span data-ttu-id="7f059-200">Başka bir oturum açma hizmeti bağlantısını tıklayın ve uygulama isteklerini kabul etmek.</span><span class="sxs-lookup"><span data-stu-id="7f059-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="7f059-201">Aşağıdaki görüntüde, Facebook Dış kimlik doğrulama sağlayıcısı'dır:</span><span class="sxs-lookup"><span data-stu-id="7f059-201">In the following image, Facebook is the external authentication provider:</span></span>

![Facebook listeleme görünümünüzü harici oturum açmaları yönetme](accconfirm/_static/fb.png)

<span data-ttu-id="7f059-203">İki hesap birleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7f059-203">The two accounts have been combined.</span></span> <span data-ttu-id="7f059-204">Her iki hesabıyla oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="7f059-204">You are able to sign in with either account.</span></span> <span data-ttu-id="7f059-205">Kullanıcılarınızın kendi sosyal oturum açma kimlik doğrulama hizmeti çalışmıyor veya bunlar sosyal hesaplarına erişim daha büyük bir olasılıkla kaybettiğinizde durumunda yerel hesaplar eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="7f059-206">Kullanıcılar bir siteye sahip olduktan sonra hesap onaylama etkinleştir</span><span class="sxs-lookup"><span data-stu-id="7f059-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="7f059-207">Var olan tüm kullanıcıları etkinleştirme hesap onaylama sitesinde kullanıcılarla kilitler.</span><span class="sxs-lookup"><span data-stu-id="7f059-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="7f059-208">Varolan kullanıcı hesaplarını onaylanan değildir çünkü kilitlidir.</span><span class="sxs-lookup"><span data-stu-id="7f059-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="7f059-209">Mevcut kullanıcı kilitlemesinin geçici olarak çözmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="7f059-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="7f059-210">Tüm mevcut kullanıcılar onaylanıp olarak işaretlemek için veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7f059-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="7f059-211">Mevcut kullanıcıları onaylayın.</span><span class="sxs-lookup"><span data-stu-id="7f059-211">Confirm existing users.</span></span> <span data-ttu-id="7f059-212">Örneğin, batch onay bağlantılarının ile e-posta gönderme.</span><span class="sxs-lookup"><span data-stu-id="7f059-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="7f059-213">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7f059-213">Prerequisites</span></span>

[<span data-ttu-id="7f059-214">.NET core 2.2 SDK veya üzeri</span><span class="sxs-lookup"><span data-stu-id="7f059-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="7f059-215">Bir web uygulaması oluşturma ve kimlik iskelesini</span><span class="sxs-lookup"><span data-stu-id="7f059-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="7f059-216">Kimlik doğrulaması ile bir web uygulaması oluşturmak için aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7f059-216">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="7f059-217">Test yeni kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="7f059-217">Test new user registration</span></span>

<span data-ttu-id="7f059-218">Uygulamayı çalıştırın, seçin **kaydetme** bağlamak ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="7f059-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="7f059-219">Yalnızca e-posta doğrulamasını bu noktada, olan [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="7f059-219">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="7f059-220">Kayıt gönderdikten sonra uygulamaya günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="7f059-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="7f059-221">Yeni kullanıcıların e-postasına doğrulanır kadar oturum açamazsınız için öğreticinin sonraki bölümlerinde kod güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7f059-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="7f059-222">Tablonun Not `EmailConfirmed` alandır `False`.</span><span class="sxs-lookup"><span data-stu-id="7f059-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="7f059-223">Bu e-posta uygulaması bir onay e-posta gönderdiğinde, yeniden sonraki adımda kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="7f059-224">Sağ tıklatın ve satır **Sil**.</span><span class="sxs-lookup"><span data-stu-id="7f059-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="7f059-225">E-posta diğer adı silmek, aşağıdaki adımlarda kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="7f059-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="7f059-226">E-posta onayı gerektir</span><span class="sxs-lookup"><span data-stu-id="7f059-226">Require email confirmation</span></span>

<span data-ttu-id="7f059-227">Yeni bir kullanıcı kaydı e-postayı onaylamak için iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="7f059-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="7f059-228">E-posta, bunlar değil kimliğe bürünerek başka birisi doğrulamak için onay yardımcı olur (diğer bir deyişle, bunlar başka birinin e-posta ile kayıtlı olmayabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="7f059-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="7f059-229">Tartışma Forumu var ve bu önlemek istediğinizi varsayalım "yli@example.com"olarak kaydetme"Kimdennolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="7f059-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="7f059-230">E-posta onayı olmadan "nolivetto@contoso.com" istenmeyen e-posta uygulamanızdan alabilir.</span><span class="sxs-lookup"><span data-stu-id="7f059-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="7f059-231">Kullanıcı olarak yanlışlıkla kayıtlı varsayalım "ylo@example.com" ve "yli", yazım hatası fark yüklediniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="7f059-232">Parola kurtarma uygulamayı doğru e-postasına olmadığından bunlar saptayamazdınız.</span><span class="sxs-lookup"><span data-stu-id="7f059-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="7f059-233">E-posta onayı robotlar sınırlı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="7f059-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="7f059-234">E-posta onayı, çok sayıda e-posta hesaplarına sahip kötü niyetli kullanıcıların koruma sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="7f059-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="7f059-235">Genellikle, yeni kullanıcıların onaylanan e-posta sahip oldukları önce web sitenizi herhangi bir veri gönderme engellemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="7f059-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="7f059-236">Güncelleştirme `Startup.ConfigureServices` onaylanan e-posta gerektirmek için:</span><span class="sxs-lookup"><span data-stu-id="7f059-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="7f059-237">`config.SignIn.RequireConfirmedEmail = true;` kayıtlı kullanıcıların e-postasına onaylanana kadar oturum açma engeller.</span><span class="sxs-lookup"><span data-stu-id="7f059-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="7f059-238">E-posta sağlayıcısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7f059-238">Configure email provider</span></span>

<span data-ttu-id="7f059-239">Bu öğreticide [SendGrid](https://sendgrid.com) e-posta göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7f059-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="7f059-240">SendGrid hesabı ve e-posta göndermek için anahtar ihtiyacınız var.</span><span class="sxs-lookup"><span data-stu-id="7f059-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="7f059-241">Diğer e-posta sağlayıcılarının kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-241">You can use other email providers.</span></span> <span data-ttu-id="7f059-242">ASP.NET Core 2.x içerir `System.Net.Mail`, uygulamanızdan e-posta göndermenize olanak tanıyan.</span><span class="sxs-lookup"><span data-stu-id="7f059-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="7f059-243">E-posta göndermek için SendGrid veya başka bir e-posta hizmeti kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="7f059-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="7f059-244">SMTP güvenli ve doğru bir şekilde ayarlamak zordur.</span><span class="sxs-lookup"><span data-stu-id="7f059-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="7f059-245">Güvenli e-posta anahtarı almak için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7f059-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="7f059-246">Bu örnek için oluşturma *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="7f059-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="7f059-247">SendGrid kullanıcı parolaları yapılandırın</span><span class="sxs-lookup"><span data-stu-id="7f059-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="7f059-248">Ayarlama `SendGridUser` ve `SendGridKey` ile [gizli dizi Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7f059-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7f059-249">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7f059-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="7f059-250">Windows üzerinde gizli dizi Yöneticisi'ni anahtar/değer çiftleri olarak depolar. bir *secrets.json* dosyası `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` dizin.</span><span class="sxs-lookup"><span data-stu-id="7f059-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="7f059-251">İçeriğini *secrets.json* olmayan dosya şifrelenmiş.</span><span class="sxs-lookup"><span data-stu-id="7f059-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="7f059-252">Aşağıdaki biçimlendirme gösterildiği *secrets.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="7f059-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="7f059-253">`SendGridKey` Değer kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="7f059-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="7f059-254">Daha fazla bilgi için [seçenekleri deseni](xref:fundamentals/configuration/options) ve [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7f059-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="7f059-255">SendGrid yükleyin</span><span class="sxs-lookup"><span data-stu-id="7f059-255">Install SendGrid</span></span>

<span data-ttu-id="7f059-256">Bu öğretici, e-posta bildirimleri aracılığıyla ekleneceği gösterilmiştir [SendGrid](https://sendgrid.com/), ancak e-posta SMTP veya başka mekanizmalar kullanılarak gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="7f059-257">Yükleme `SendGrid` NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="7f059-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f059-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f059-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7f059-259">Paket Yöneticisi konsolundan aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="7f059-259">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7f059-260">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7f059-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7f059-261">Konsolundan aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="7f059-261">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="7f059-262">Bkz: [SendGrid ile ücretsiz olarak kullanmaya başlayın](https://sendgrid.com/free/) ücretsiz SendGrid hesabı kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="7f059-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="7f059-263">IEmailSender uygulayın</span><span class="sxs-lookup"><span data-stu-id="7f059-263">Implement IEmailSender</span></span>

<span data-ttu-id="7f059-264">Uygulama için `IEmailSender`, oluşturma *Services/EmailSender.cs* kodu aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="7f059-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="7f059-265">Başlangıç e-posta destekleyecek şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7f059-265">Configure startup to support email</span></span>

<span data-ttu-id="7f059-266">Aşağıdaki kodu ekleyin `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7f059-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="7f059-267">Ekleme `EmailSender` geçici bir hizmet olarak.</span><span class="sxs-lookup"><span data-stu-id="7f059-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="7f059-268">Kayıt `AuthMessageSenderOptions` yapılandırma örneği.</span><span class="sxs-lookup"><span data-stu-id="7f059-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="7f059-269">Hesap onaylama ve parola kurtarmayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="7f059-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="7f059-270">Şablon hesap onaylama ve parola kurtarma kodu var.</span><span class="sxs-lookup"><span data-stu-id="7f059-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="7f059-271">Bulma `OnPostAsync` yönteminde *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="7f059-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="7f059-272">Yeni kaydettiğiniz kullanıcıların otomatik olarak aşağıdaki satırı açıklama satırı yaparak oturum açmayı engellemek:</span><span class="sxs-lookup"><span data-stu-id="7f059-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="7f059-273">Vurgulanmış satır ile tam yöntemi gösterilir:</span><span class="sxs-lookup"><span data-stu-id="7f059-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="7f059-274">Kaydetme, e-posta onayı ve parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="7f059-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="7f059-275">Web uygulamasını çalıştırın ve hesap onaylama ve parola kurtarma akışı test edin.</span><span class="sxs-lookup"><span data-stu-id="7f059-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="7f059-276">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="7f059-276">Run the app and register a new user</span></span>
* <span data-ttu-id="7f059-277">Hesap onay bağlantısı için e-postanızı kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="7f059-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="7f059-278">Bkz: [hata ayıklama, e-posta](#debug) e-posta alırsanız yok.</span><span class="sxs-lookup"><span data-stu-id="7f059-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="7f059-279">E-postanızı doğrulamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7f059-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="7f059-280">E-postanıza ve parola ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="7f059-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="7f059-281">Oturumu kapatın.</span><span class="sxs-lookup"><span data-stu-id="7f059-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="7f059-282">Yönet sayfasını görüntüle</span><span class="sxs-lookup"><span data-stu-id="7f059-282">View the manage page</span></span>

<span data-ttu-id="7f059-283">Tarayıcıda kullanıcı adınızı seçin: ![tarayıcı penceresi ile kullanıcı adı](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="7f059-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="7f059-284">İle Yönet sayfasında görüntülenen **profili** sekmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="7f059-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="7f059-285">**E-posta** e-posta belirten bir onay kutusu onaylanan gösterir.</span><span class="sxs-lookup"><span data-stu-id="7f059-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="7f059-286">Test parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="7f059-286">Test password reset</span></span>

* <span data-ttu-id="7f059-287">Oturum açmadıysanız, seçin **oturum kapatma**.</span><span class="sxs-lookup"><span data-stu-id="7f059-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="7f059-288">Seçin **oturum** seçin ve bağlama **parolanızı mı unuttunuz?** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7f059-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="7f059-289">Hesap kaydolmak için kullandığınız e-posta girin.</span><span class="sxs-lookup"><span data-stu-id="7f059-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="7f059-290">Parolanızı sıfırlamak için bir bağlantı içeren bir e-posta gönderilir.</span><span class="sxs-lookup"><span data-stu-id="7f059-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="7f059-291">E-postanızı kontrol edin ve parolanızı sıfırlamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7f059-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="7f059-292">Parolanızı başarıyla sıfırladıktan sonra e-posta ve yeni bir parola ile oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="7f059-293">E-posta ve etkinlik zaman aşımı değiştirme</span><span class="sxs-lookup"><span data-stu-id="7f059-293">Change email and activity timeout</span></span>

<span data-ttu-id="7f059-294">Varsayılan boş durma zaman aşımı 14 gündür.</span><span class="sxs-lookup"><span data-stu-id="7f059-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="7f059-295">Aşağıdaki kod 5 gün etkin olmama zaman aşımı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="7f059-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="7f059-296">Tüm veri koruma simge lifespans değiştirme</span><span class="sxs-lookup"><span data-stu-id="7f059-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="7f059-297">Aşağıdaki kod tüm veri koruma belirteçleri zaman aşımı süresi 3 saat olarak değiştirir:</span><span class="sxs-lookup"><span data-stu-id="7f059-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="7f059-298">Yerleşik kimlik kullanıcı belirteçlerinde (bkz [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) sahip bir [günlük bir zaman aşımı](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7f059-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="7f059-299">E-posta belirteci ömrü değiştirme</span><span class="sxs-lookup"><span data-stu-id="7f059-299">Change the email token lifespan</span></span>

<span data-ttu-id="7f059-300">Varsayılan belirteç ömrü [kimlik kullanıcı belirteçleri](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) olduğu [bir gün](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7f059-300">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="7f059-301">Bu bölümde, e-posta belirteci ömrü değiştirme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7f059-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="7f059-302">Özel bir ekleme [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) ve <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="7f059-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="7f059-303">Özel sağlayıcı hizmet kapsayıcıya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7f059-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="7f059-304">Onay e-postası gönder</span><span class="sxs-lookup"><span data-stu-id="7f059-304">Resend email confirmation</span></span>

<span data-ttu-id="7f059-305">Bkz: [bu GitHub sorunu](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="7f059-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="7f059-306">E-posta hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="7f059-306">Debug email</span></span>

<span data-ttu-id="7f059-307">E-posta çalışma erişemiyorsanız:</span><span class="sxs-lookup"><span data-stu-id="7f059-307">If you can't get email working:</span></span>

* <span data-ttu-id="7f059-308">Bir kesim noktası `EmailSender.Execute` doğrulamak için `SendGridClient.SendEmailAsync` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="7f059-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="7f059-309">Oluşturma bir [e-posta göndermek için konsol uygulaması](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) benzer bir kod kullanarak `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="7f059-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="7f059-310">Gözden geçirme [e-posta etkinlik](https://sendgrid.com/docs/User_Guide/email_activity.html) sayfası.</span><span class="sxs-lookup"><span data-stu-id="7f059-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="7f059-311">İstenmeyen posta klasörünüzü kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="7f059-311">Check your spam folder.</span></span>
* <span data-ttu-id="7f059-312">Farklı bir e-posta sağlayıcısı (Microsoft, Yahoo, Gmail, vb.) başka bir e-posta diğer adı deneyin</span><span class="sxs-lookup"><span data-stu-id="7f059-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="7f059-313">Farklı bir e-posta hesaplarına göndermeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="7f059-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="7f059-314">**En iyi güvenlik uygulaması** için **değil** test ve geliştirme üretim gizli dizileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="7f059-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="7f059-315">Uygulamayı Azure'a yayımlama, Web uygulamasını Azure Portalı'nda Uygulama ayarları olarak SendGrid parolaları ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="7f059-316">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="7f059-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="7f059-317">Sosyal ve yerel oturum açma hesabını birleştirin</span><span class="sxs-lookup"><span data-stu-id="7f059-317">Combine social and local login accounts</span></span>

<span data-ttu-id="7f059-318">Bu bölümde tamamlamak için önce bir dış kimlik doğrulama sağlayıcısı etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="7f059-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="7f059-319">Bkz: [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7f059-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="7f059-320">Yerel hem de sosyal hesaplar, e-posta bağlantısına tıklayarak birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="7f059-321">Aşağıdaki sırayla "RickAndMSFT@gmail.com" ilk olarak bir yerel oturum açın; oluşturulur ancak, bir sosyal oturum açma ilk hesap oluşturun, sonra bir yerel oturum açma ekleme.</span><span class="sxs-lookup"><span data-stu-id="7f059-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web uygulaması: RickAndMSFT@gmail.com kimliği doğrulanmış kullanıcı](accconfirm/_static/rick.png)

<span data-ttu-id="7f059-323">Tıklayarak **Yönet** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7f059-323">Click on the **Manage** link.</span></span> <span data-ttu-id="7f059-324">Bu hesapla ilişkili 0 dış (sosyal oturum açma bilgileri) unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7f059-324">Note the 0 external (social logins) associated with this account.</span></span>

![Görünümü yönetme](accconfirm/_static/manage.png)

<span data-ttu-id="7f059-326">Başka bir oturum açma hizmeti bağlantısını tıklayın ve uygulama isteklerini kabul etmek.</span><span class="sxs-lookup"><span data-stu-id="7f059-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="7f059-327">Aşağıdaki görüntüde, Facebook Dış kimlik doğrulama sağlayıcısı'dır:</span><span class="sxs-lookup"><span data-stu-id="7f059-327">In the following image, Facebook is the external authentication provider:</span></span>

![Facebook listeleme görünümünüzü harici oturum açmaları yönetme](accconfirm/_static/fb.png)

<span data-ttu-id="7f059-329">İki hesap birleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7f059-329">The two accounts have been combined.</span></span> <span data-ttu-id="7f059-330">Her iki hesabıyla oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="7f059-330">You are able to sign in with either account.</span></span> <span data-ttu-id="7f059-331">Kullanıcılarınızın kendi sosyal oturum açma kimlik doğrulama hizmeti çalışmıyor veya bunlar sosyal hesaplarına erişim daha büyük bir olasılıkla kaybettiğinizde durumunda yerel hesaplar eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f059-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="7f059-332">Kullanıcılar bir siteye sahip olduktan sonra hesap onaylama etkinleştir</span><span class="sxs-lookup"><span data-stu-id="7f059-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="7f059-333">Var olan tüm kullanıcıları etkinleştirme hesap onaylama sitesinde kullanıcılarla kilitler.</span><span class="sxs-lookup"><span data-stu-id="7f059-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="7f059-334">Varolan kullanıcı hesaplarını onaylanan değildir çünkü kilitlidir.</span><span class="sxs-lookup"><span data-stu-id="7f059-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="7f059-335">Mevcut kullanıcı kilitlemesinin geçici olarak çözmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="7f059-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="7f059-336">Tüm mevcut kullanıcılar onaylanıp olarak işaretlemek için veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7f059-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="7f059-337">Mevcut kullanıcıları onaylayın.</span><span class="sxs-lookup"><span data-stu-id="7f059-337">Confirm existing users.</span></span> <span data-ttu-id="7f059-338">Örneğin, batch onay bağlantılarının ile e-posta gönderme.</span><span class="sxs-lookup"><span data-stu-id="7f059-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
