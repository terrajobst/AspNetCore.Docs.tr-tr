---
title: Hesap onaylama ve parola kurtarma ASP.NET Core
author: rick-anderson
description: E-posta onayı ve parola sıfırlama ile ASP.NET Core uygulaması oluşturmayı öğrenin.
ms.author: riande
ms.date: 3/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: d102ed0a4a75f6273fcda0a8cc7e9d091ff94b50
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209942"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="7bcc6-103">Hesap onaylama ve parola kurtarma ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bcc6-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="7bcc6-104">Bkz: [bu PDF dosyası](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core 1.1 ve 2.1 sürümü için.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7bcc6-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="7bcc6-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="7bcc6-106">Bu öğretici, e-posta onayı ve parola sıfırlama ile ASP.NET Core uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="7bcc6-107">Bu öğretici **değil** başına konu.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="7bcc6-108">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-108">You should be familiar with:</span></span>

* [<span data-ttu-id="7bcc6-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bcc6-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="7bcc6-110">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="7bcc6-110">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="7bcc6-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7bcc6-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="7bcc6-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7bcc6-112">Prerequisites</span></span>

[<span data-ttu-id="7bcc6-113">.NET core 2.2 SDK veya üzeri</span><span class="sxs-lookup"><span data-stu-id="7bcc6-113">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="7bcc6-114">Bir web uygulaması oluşturma ve kimlik iskelesini</span><span class="sxs-lookup"><span data-stu-id="7bcc6-114">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="7bcc6-115">Kimlik doğrulaması ile bir web uygulaması oluşturmak için aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="7bcc6-116">Test yeni kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="7bcc6-116">Test new user registration</span></span>

<span data-ttu-id="7bcc6-117">Uygulamayı çalıştırın, seçin **kaydetme** bağlamak ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-117">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="7bcc6-118">Yalnızca e-posta doğrulamasını bu noktada, olan [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-118">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="7bcc6-119">Kayıt gönderdikten sonra uygulamaya günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-119">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="7bcc6-120">Yeni kullanıcıların e-postasına doğrulanır kadar oturum açamazsınız için öğreticinin sonraki bölümlerinde kod güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-120">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="7bcc6-121">Tablonun Not `EmailConfirmed` alandır `False`.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-121">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="7bcc6-122">Bu e-posta uygulaması bir onay e-posta gönderdiğinde, yeniden sonraki adımda kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-122">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="7bcc6-123">Sağ tıklatın ve satır **Sil**.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-123">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="7bcc6-124">E-posta diğer adı silmek, aşağıdaki adımlarda kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-124">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="7bcc6-125">E-posta onayı gerektir</span><span class="sxs-lookup"><span data-stu-id="7bcc6-125">Require email confirmation</span></span>

<span data-ttu-id="7bcc6-126">Yeni bir kullanıcı kaydı e-postayı onaylamak için iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-126">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="7bcc6-127">E-posta, bunlar değil kimliğe bürünerek başka birisi doğrulamak için onay yardımcı olur (diğer bir deyişle, bunlar başka birinin e-posta ile kayıtlı olmayabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="7bcc6-127">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="7bcc6-128">Tartışma Forumu var ve bu önlemek istediğinizi varsayalım "yli@example.com"olarak kaydetme"Kimdennolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="7bcc6-128">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="7bcc6-129">E-posta onayı olmadan "nolivetto@contoso.com" istenmeyen e-posta uygulamanızdan alabilir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-129">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="7bcc6-130">Kullanıcı olarak yanlışlıkla kayıtlı varsayalım "ylo@example.com" ve "yli", yazım hatası fark yüklediniz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-130">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="7bcc6-131">Parola kurtarma uygulamayı doğru e-postasına olmadığından bunlar saptayamazdınız.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-131">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="7bcc6-132">E-posta onayı robotlar sınırlı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-132">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="7bcc6-133">E-posta onayı, çok sayıda e-posta hesaplarına sahip kötü niyetli kullanıcıların koruma sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-133">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="7bcc6-134">Genellikle, yeni kullanıcıların onaylanan e-posta sahip oldukları önce web sitenizi herhangi bir veri gönderme engellemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-134">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="7bcc6-135">Güncelleştirme `Startup.ConfigureServices` onaylanan e-posta gerektirmek için:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-135">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="7bcc6-136">`config.SignIn.RequireConfirmedEmail = true;` kayıtlı kullanıcıların e-postasına onaylanana kadar oturum açma engeller.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-136">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="7bcc6-137">E-posta sağlayıcısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7bcc6-137">Configure email provider</span></span>

<span data-ttu-id="7bcc6-138">Bu öğreticide [SendGrid](https://sendgrid.com) e-posta göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-138">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="7bcc6-139">SendGrid hesabı ve e-posta göndermek için anahtar ihtiyacınız var.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-139">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="7bcc6-140">Diğer e-posta sağlayıcılarının kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-140">You can use other email providers.</span></span> <span data-ttu-id="7bcc6-141">ASP.NET Core 2.x içerir `System.Net.Mail`, uygulamanızdan e-posta göndermenize olanak tanıyan.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-141">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="7bcc6-142">E-posta göndermek için SendGrid veya başka bir e-posta hizmeti kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-142">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="7bcc6-143">SMTP güvenli ve doğru bir şekilde ayarlamak zordur.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-143">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="7bcc6-144">Güvenli e-posta anahtarı almak için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-144">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="7bcc6-145">Bu örnek için oluşturma *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-145">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="7bcc6-146">SendGrid kullanıcı parolaları yapılandırın</span><span class="sxs-lookup"><span data-stu-id="7bcc6-146">Configure SendGrid user secrets</span></span>

<span data-ttu-id="7bcc6-147">Ayarlama `SendGridUser` ve `SendGridKey` ile [gizli dizi Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7bcc6-147">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7bcc6-148">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-148">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="7bcc6-149">Windows üzerinde gizli dizi Yöneticisi'ni anahtar/değer çiftleri olarak depolar. bir *secrets.json* dosyası `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` dizin.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-149">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="7bcc6-150">İçeriğini *secrets.json* olmayan dosya şifrelenmiş.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-150">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="7bcc6-151">Aşağıdaki biçimlendirme gösterildiği *secrets.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-151">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="7bcc6-152">`SendGridKey` Değer kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-152">The `SendGridKey` value has been removed.</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```
 
<span data-ttu-id="7bcc6-153">Daha fazla bilgi için [seçenekleri deseni](xref:fundamentals/configuration/options) ve [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7bcc6-153">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="7bcc6-154">SendGrid yükleyin</span><span class="sxs-lookup"><span data-stu-id="7bcc6-154">Install SendGrid</span></span>

<span data-ttu-id="7bcc6-155">Bu öğretici, e-posta bildirimleri aracılığıyla ekleneceği gösterilmiştir [SendGrid](https://sendgrid.com/), ancak e-posta SMTP veya başka mekanizmalar kullanılarak gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-155">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="7bcc6-156">Yükleme `SendGrid` NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-156">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7bcc6-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7bcc6-157">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7bcc6-158">Paket Yöneticisi konsolundan aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-158">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7bcc6-159">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7bcc6-159">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7bcc6-160">Konsolundan aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-160">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="7bcc6-161">Bkz: [SendGrid ile ücretsiz olarak kullanmaya başlayın](https://sendgrid.com/free/) ücretsiz SendGrid hesabı kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-161">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="7bcc6-162">IEmailSender uygulayın</span><span class="sxs-lookup"><span data-stu-id="7bcc6-162">Implement IEmailSender</span></span>

<span data-ttu-id="7bcc6-163">Uygulama için `IEmailSender`, oluşturma *Services/EmailSender.cs* kodu aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-163">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="7bcc6-164">Başlangıç e-posta destekleyecek şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7bcc6-164">Configure startup to support email</span></span>

<span data-ttu-id="7bcc6-165">Aşağıdaki kodu ekleyin `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-165">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="7bcc6-166">Ekleme `EmailSender` geçici bir hizmet olarak.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-166">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="7bcc6-167">Kayıt `AuthMessageSenderOptions` yapılandırma örneği.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-167">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="7bcc6-168">Hesap onaylama ve parola kurtarmayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="7bcc6-168">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="7bcc6-169">Şablon hesap onaylama ve parola kurtarma kodu var.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-169">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="7bcc6-170">Bulma `OnPostAsync` yönteminde *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-170">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="7bcc6-171">Yeni kaydettiğiniz kullanıcıların otomatik olarak aşağıdaki satırı açıklama satırı yaparak oturum açmayı engellemek:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-171">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="7bcc6-172">Vurgulanmış satır ile tam yöntemi gösterilir:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-172">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="7bcc6-173">Kaydetme, e-posta onayı ve parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="7bcc6-173">Register, confirm email, and reset password</span></span>

<span data-ttu-id="7bcc6-174">Web uygulamasını çalıştırın ve hesap onaylama ve parola kurtarma akışı test edin.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-174">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="7bcc6-175">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="7bcc6-175">Run the app and register a new user</span></span>
* <span data-ttu-id="7bcc6-176">Hesap onay bağlantısı için e-postanızı kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-176">Check your email for the account confirmation link.</span></span> <span data-ttu-id="7bcc6-177">Bkz: [hata ayıklama, e-posta](#debug) e-posta alırsanız yok.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-177">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="7bcc6-178">E-postanızı doğrulamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-178">Click the link to confirm your email.</span></span>
* <span data-ttu-id="7bcc6-179">E-postanıza ve parola ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-179">Sign in with your email and password.</span></span>
* <span data-ttu-id="7bcc6-180">Oturumu kapatın.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-180">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="7bcc6-181">Yönet sayfasını görüntüle</span><span class="sxs-lookup"><span data-stu-id="7bcc6-181">View the manage page</span></span>

<span data-ttu-id="7bcc6-182">Tarayıcıda kullanıcı adınızı seçin: ![tarayıcı penceresi ile kullanıcı adı](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="7bcc6-182">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="7bcc6-183">İle Yönet sayfasında görüntülenen **profili** sekmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-183">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="7bcc6-184">**E-posta** e-posta belirten bir onay kutusu onaylanan gösterir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-184">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="7bcc6-185">Test parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="7bcc6-185">Test password reset</span></span>

* <span data-ttu-id="7bcc6-186">Oturum açmadıysanız, seçin **oturum kapatma**.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-186">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="7bcc6-187">Seçin **oturum** seçin ve bağlama **parolanızı mı unuttunuz?** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-187">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="7bcc6-188">Hesap kaydolmak için kullandığınız e-posta girin.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-188">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="7bcc6-189">Parolanızı sıfırlamak için bir bağlantı içeren bir e-posta gönderilir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-189">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="7bcc6-190">E-postanızı kontrol edin ve parolanızı sıfırlamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-190">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="7bcc6-191">Parolanızı başarıyla sıfırladıktan sonra e-posta ve yeni bir parola ile oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-191">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="7bcc6-192">E-posta ve etkinlik zaman aşımı değiştirme</span><span class="sxs-lookup"><span data-stu-id="7bcc6-192">Change email and activity timeout</span></span>

<span data-ttu-id="7bcc6-193">Varsayılan boş durma zaman aşımı 14 gündür.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-193">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="7bcc6-194">Aşağıdaki kod 5 gün etkin olmama zaman aşımı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-194">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="7bcc6-195">Tüm veri koruma simge lifespans değiştirme</span><span class="sxs-lookup"><span data-stu-id="7bcc6-195">Change all data protection token lifespans</span></span>

<span data-ttu-id="7bcc6-196">Aşağıdaki kod tüm veri koruma belirteçleri zaman aşımı süresi 3 saat olarak değiştirir:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-196">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="7bcc6-197">Yerleşik kimlik kullanıcı belirteçlerinde (bkz [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) sahip bir [günlük bir zaman aşımı](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7bcc6-197">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="7bcc6-198">E-posta belirteci ömrü değiştirme</span><span class="sxs-lookup"><span data-stu-id="7bcc6-198">Change the email token lifespan</span></span>

<span data-ttu-id="7bcc6-199">Varsayılan belirteç ömrü [kimlik kullanıcı belirteçleri](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) olduğu [bir gün](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7bcc6-199">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="7bcc6-200">Bu bölümde, e-posta belirteci ömrü değiştirme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-200">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="7bcc6-201">Özel bir ekleme [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) ve <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-201">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="7bcc6-202">Özel sağlayıcı hizmet kapsayıcıya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-202">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="7bcc6-203">Onay e-postası gönder</span><span class="sxs-lookup"><span data-stu-id="7bcc6-203">Resend email confirmation</span></span>

<span data-ttu-id="7bcc6-204">Bkz: [bu GitHub sorunu](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="7bcc6-204">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>
### <a name="debug-email"></a><span data-ttu-id="7bcc6-205">E-posta hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="7bcc6-205">Debug email</span></span>

<span data-ttu-id="7bcc6-206">E-posta çalışma erişemiyorsanız:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-206">If you can't get email working:</span></span>

* <span data-ttu-id="7bcc6-207">Bir kesim noktası `EmailSender.Execute` doğrulamak için `SendGridClient.SendEmailAsync` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-207">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="7bcc6-208">Oluşturma bir [e-posta göndermek için konsol uygulaması](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) benzer bir kod kullanarak `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-208">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="7bcc6-209">Gözden geçirme [e-posta etkinlik](https://sendgrid.com/docs/User_Guide/email_activity.html) sayfası.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-209">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="7bcc6-210">İstenmeyen posta klasörünüzü kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-210">Check your spam folder.</span></span>
* <span data-ttu-id="7bcc6-211">Farklı bir e-posta sağlayıcısı (Microsoft, Yahoo, Gmail, vb.) başka bir e-posta diğer adı deneyin</span><span class="sxs-lookup"><span data-stu-id="7bcc6-211">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="7bcc6-212">Farklı bir e-posta hesaplarına göndermeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-212">Try sending to different email accounts.</span></span>

<span data-ttu-id="7bcc6-213">**En iyi güvenlik uygulaması** için **değil** test ve geliştirme üretim gizli dizileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-213">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="7bcc6-214">Uygulamayı Azure'a yayımlama, Web uygulamasını Azure Portalı'nda Uygulama ayarları olarak SendGrid parolaları ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-214">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="7bcc6-215">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-215">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="7bcc6-216">Sosyal ve yerel oturum açma hesabını birleştirin</span><span class="sxs-lookup"><span data-stu-id="7bcc6-216">Combine social and local login accounts</span></span>

<span data-ttu-id="7bcc6-217">Bu bölümde tamamlamak için önce bir dış kimlik doğrulama sağlayıcısı etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-217">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="7bcc6-218">Bkz: [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7bcc6-218">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="7bcc6-219">Yerel hem de sosyal hesaplar, e-posta bağlantısına tıklayarak birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-219">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="7bcc6-220">Aşağıdaki sırayla "RickAndMSFT@gmail.com" ilk olarak bir yerel oturum açın; oluşturulur ancak, bir sosyal oturum açma ilk hesap oluşturun, sonra bir yerel oturum açma ekleme.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-220">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web uygulaması: RickAndMSFT@gmail.com kimliği doğrulanmış kullanıcı](accconfirm/_static/rick.png)

<span data-ttu-id="7bcc6-222">Tıklayarak **Yönet** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-222">Click on the **Manage** link.</span></span> <span data-ttu-id="7bcc6-223">Bu hesapla ilişkili 0 dış (sosyal oturum açma bilgileri) unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-223">Note the 0 external (social logins) associated with this account.</span></span>

![Görünümü yönetme](accconfirm/_static/manage.png)

<span data-ttu-id="7bcc6-225">Başka bir oturum açma hizmeti bağlantısını tıklayın ve uygulama isteklerini kabul etmek.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-225">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="7bcc6-226">Aşağıdaki görüntüde, Facebook Dış kimlik doğrulama sağlayıcısı'dır:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-226">In the following image, Facebook is the external authentication provider:</span></span>

![Facebook listeleme görünümünüzü harici oturum açmaları yönetme](accconfirm/_static/fb.png)

<span data-ttu-id="7bcc6-228">İki hesap birleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-228">The two accounts have been combined.</span></span> <span data-ttu-id="7bcc6-229">Her iki hesabıyla oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-229">You are able to sign in with either account.</span></span> <span data-ttu-id="7bcc6-230">Kullanıcılarınızın kendi sosyal oturum açma kimlik doğrulama hizmeti çalışmıyor veya bunlar sosyal hesaplarına erişim daha büyük bir olasılıkla kaybettiğinizde durumunda yerel hesaplar eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-230">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="7bcc6-231">Kullanıcılar bir siteye sahip olduktan sonra hesap onaylama etkinleştir</span><span class="sxs-lookup"><span data-stu-id="7bcc6-231">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="7bcc6-232">Var olan tüm kullanıcıları etkinleştirme hesap onaylama sitesinde kullanıcılarla kilitler.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-232">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="7bcc6-233">Varolan kullanıcı hesaplarını onaylanan değildir çünkü kilitlidir.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-233">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="7bcc6-234">Mevcut kullanıcı kilitlemesinin geçici olarak çözmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="7bcc6-234">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="7bcc6-235">Tüm mevcut kullanıcılar onaylanıp olarak işaretlemek için veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-235">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="7bcc6-236">Mevcut kullanıcıları onaylayın.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-236">Confirm existing users.</span></span> <span data-ttu-id="7bcc6-237">Örneğin, batch onay bağlantılarının ile e-posta gönderme.</span><span class="sxs-lookup"><span data-stu-id="7bcc6-237">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
