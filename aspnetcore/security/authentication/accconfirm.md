---
title: Hesap onaylama ve parola kurtarma ASP.NET Core
author: rick-anderson
description: E-posta onayı ve parola sıfırlama ile ASP.NET Core uygulaması oluşturmayı öğrenin.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 8e175cd19ca4a9de1e7cf6b330b3d82f309b6501
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063344"
---
::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="425fc-103">Bkz: [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core 1.1 ve 2.1 sürümü için.</span><span class="sxs-lookup"><span data-stu-id="425fc-103">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="425fc-104">Hesap onaylama ve parola kurtarma ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="425fc-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="425fc-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="425fc-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="425fc-106">Bu öğretici, e-posta onayı ve parola sıfırlama ile ASP.NET Core uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="425fc-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="425fc-107">Bu öğretici **değil** başına konu.</span><span class="sxs-lookup"><span data-stu-id="425fc-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="425fc-108">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="425fc-108">You should be familiar with:</span></span>

* [<span data-ttu-id="425fc-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="425fc-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="425fc-110">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="425fc-110">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="425fc-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="425fc-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="425fc-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="425fc-112">Prerequisites</span></span>

<span data-ttu-id="425fc-113">[! Dahil etme [] (~ / includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="425fc-113">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span></span>

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="425fc-114">Bir web uygulaması oluşturma ve kimlik iskelesini</span><span class="sxs-lookup"><span data-stu-id="425fc-114">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="425fc-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="425fc-115">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="425fc-116">Visual Studio'da yeni bir oluşturma **Web uygulaması** proje.</span><span class="sxs-lookup"><span data-stu-id="425fc-116">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="425fc-117">Seçin **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="425fc-117">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="425fc-118">Varsayılan tutun **kimlik doğrulaması** kümesine **kimlik doğrulaması yok**.</span><span class="sxs-lookup"><span data-stu-id="425fc-118">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="425fc-119">Kimlik doğrulaması, bir sonraki adımda eklenir.</span><span class="sxs-lookup"><span data-stu-id="425fc-119">Authentication is added in the next step.</span></span>

<span data-ttu-id="425fc-120">Sonraki adımda:</span><span class="sxs-lookup"><span data-stu-id="425fc-120">In the next step:</span></span>

* <span data-ttu-id="425fc-121">Düzen sayfası kümesine *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="425fc-121">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="425fc-122">Seçin *hesabı/kaydı*</span><span class="sxs-lookup"><span data-stu-id="425fc-122">Select *Account/Register*</span></span>
* <span data-ttu-id="425fc-123">Yeni bir **veri bağlamı sınıfı**</span><span class="sxs-lookup"><span data-stu-id="425fc-123">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="425fc-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="425fc-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

<span data-ttu-id="425fc-125">Çalıştırma `dotnet aspnet-codegenerator identity --help` yapı iskelesi aracın Yardım almak için.</span><span class="sxs-lookup"><span data-stu-id="425fc-125">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="425fc-126">Bölümündeki yönergeleri [kimlik doğrulamasını etkinleştirme](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="425fc-126">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="425fc-127">Ekleme `app.UseAuthentication();` için `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="425fc-127">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="425fc-128">Ekleme `<partial name="_LoginPartial" />` Düzen dosyası için.</span><span class="sxs-lookup"><span data-stu-id="425fc-128">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="425fc-129">Test yeni kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="425fc-129">Test new user registration</span></span>

<span data-ttu-id="425fc-130">Uygulamayı çalıştırın, seçin **kaydetme** bağlamak ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="425fc-130">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="425fc-131">Yalnızca e-posta doğrulamasını bu noktada, olan [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="425fc-131">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="425fc-132">Kayıt gönderdikten sonra uygulamaya günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="425fc-132">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="425fc-133">E-postasına doğrulanır kadar yeni kullanıcılar oturum açamaz böylece daha sonra öğreticide kod güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="425fc-133">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="425fc-134">Kimlik veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="425fc-134">View the Identity database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="425fc-135">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="425fc-135">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="425fc-136">Gelen **görünümü** menüsünde **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="425fc-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="425fc-137">Gidin **(localdb) (SQL Server 13) ifadesini MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="425fc-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="425fc-138">Sağ **dbo. AspNetUsers** > **verileri görüntüleme**:</span><span class="sxs-lookup"><span data-stu-id="425fc-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server Nesne Gezgini AspNetUsers tablosunda bağlamsal menü](accconfirm/_static/ssox.png)

<span data-ttu-id="425fc-140">Tablonun Not `EmailConfirmed` alandır `False`.</span><span class="sxs-lookup"><span data-stu-id="425fc-140">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="425fc-141">Bu e-posta uygulaması bir onay e-posta gönderdiğinde, yeniden sonraki adımda kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425fc-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="425fc-142">Sağ tıklatın ve satır **Sil**.</span><span class="sxs-lookup"><span data-stu-id="425fc-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="425fc-143">E-posta diğer adı silmek, aşağıdaki adımlarda kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="425fc-143">Deleting the email alias makes it easier in the following steps.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="425fc-144">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="425fc-144">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="425fc-145">Bkz: [ASP.NET Core MVC projesinde SQLite ile çalışma](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite veritabanını görüntülemek yönergeler.</span><span class="sxs-lookup"><span data-stu-id="425fc-145">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

------

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="425fc-146">E-posta onayı gerektir</span><span class="sxs-lookup"><span data-stu-id="425fc-146">Require email confirmation</span></span>

<span data-ttu-id="425fc-147">Yeni bir kullanıcı kaydı e-postayı onaylamak için iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="425fc-147">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="425fc-148">E-posta, bunlar değil kimliğe bürünerek başka birisi doğrulamak için onay yardımcı olur (diğer bir deyişle, bunlar başka birinin e-posta ile kayıtlı olmayabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="425fc-148">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="425fc-149">Tartışma Forumu var ve bu önlemek istediğinizi varsayalım "yli@example.com"olarak kaydetme"Kimdennolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="425fc-149">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="425fc-150">E-posta onayı olmadan "nolivetto@contoso.com" istenmeyen e-posta uygulamanızdan alabilir.</span><span class="sxs-lookup"><span data-stu-id="425fc-150">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="425fc-151">Kullanıcı olarak yanlışlıkla kayıtlı varsayalım "ylo@example.com" ve "yli", yazım hatası fark yüklediniz.</span><span class="sxs-lookup"><span data-stu-id="425fc-151">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="425fc-152">Parola kurtarma uygulamayı doğru e-postasına olmadığından bunlar saptayamazdınız.</span><span class="sxs-lookup"><span data-stu-id="425fc-152">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="425fc-153">E-posta onayı robotlar sınırlı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="425fc-153">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="425fc-154">E-posta onayı, çok sayıda e-posta hesaplarına sahip kötü niyetli kullanıcıların koruma sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="425fc-154">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="425fc-155">Genellikle, yeni kullanıcıların onaylanan e-posta sahip oldukları önce web sitenizi herhangi bir veri gönderme engellemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="425fc-155">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="425fc-156">Güncelleştirme *Areas/Identity/IdentityHostingStartup.cs* onaylanan e-posta gerektirmek için:</span><span class="sxs-lookup"><span data-stu-id="425fc-156">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="425fc-157">`config.SignIn.RequireConfirmedEmail = true;` kayıtlı kullanıcıların e-postasına onaylanana kadar oturum açma engeller.</span><span class="sxs-lookup"><span data-stu-id="425fc-157">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="425fc-158">E-posta sağlayıcısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="425fc-158">Configure email provider</span></span>

<span data-ttu-id="425fc-159">Bu öğreticide [SendGrid](https://sendgrid.com) e-posta göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="425fc-159">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="425fc-160">SendGrid hesabı ve e-posta göndermek için anahtar ihtiyacınız var.</span><span class="sxs-lookup"><span data-stu-id="425fc-160">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="425fc-161">Diğer e-posta sağlayıcılarının kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425fc-161">You can use other email providers.</span></span> <span data-ttu-id="425fc-162">ASP.NET Core 2.x içerir `System.Net.Mail`, uygulamanızdan e-posta göndermenize olanak tanıyan.</span><span class="sxs-lookup"><span data-stu-id="425fc-162">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="425fc-163">E-posta göndermek için SendGrid veya başka bir e-posta hizmeti kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="425fc-163">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="425fc-164">SMTP güvenli ve doğru bir şekilde ayarlamak zordur.</span><span class="sxs-lookup"><span data-stu-id="425fc-164">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="425fc-165">[Seçenekleri deseni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="425fc-165">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="425fc-166">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="425fc-166">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="425fc-167">Güvenli e-posta anahtarı almak için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="425fc-167">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="425fc-168">Bu örnek için oluşturma *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="425fc-168">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="425fc-169">SendGrid kullanıcı parolaları yapılandırın</span><span class="sxs-lookup"><span data-stu-id="425fc-169">Configure SendGrid user secrets</span></span>

<span data-ttu-id="425fc-170">Benzersiz bir ekleme `<UserSecretsId>` değerini `<PropertyGroup>` proje dosyasının öğe:</span><span class="sxs-lookup"><span data-stu-id="425fc-170">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="425fc-171">Ayarlama `SendGridUser` ve `SendGridKey` ile [gizli dizi Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="425fc-171">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="425fc-172">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="425fc-172">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="425fc-173">Windows üzerinde gizli dizi Yöneticisi'ni anahtar/değer çiftleri olarak depolar. bir *secrets.json* dosyası `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` dizin.</span><span class="sxs-lookup"><span data-stu-id="425fc-173">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="425fc-174">İçeriğini *secrets.json* olmayan dosya şifrelenmiş.</span><span class="sxs-lookup"><span data-stu-id="425fc-174">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="425fc-175">*Secrets.json* aşağıda gösterilmektedir dosyanın ( `SendGridKey` değer kaldırıldı.)</span><span class="sxs-lookup"><span data-stu-id="425fc-175">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="install-sendgrid"></a><span data-ttu-id="425fc-176">SendGrid yükleyin</span><span class="sxs-lookup"><span data-stu-id="425fc-176">Install SendGrid</span></span>

<span data-ttu-id="425fc-177">Bu öğretici, e-posta bildirimleri aracılığıyla ekleneceği gösterilmiştir [SendGrid](https://sendgrid.com/), ancak e-posta SMTP veya başka mekanizmalar kullanılarak gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425fc-177">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="425fc-178">Yükleme `SendGrid` NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="425fc-178">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="425fc-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="425fc-179">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="425fc-180">Paket Yöneticisi konsolundan aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="425fc-180">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="425fc-181">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="425fc-181">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="425fc-182">Konsolundan aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="425fc-182">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="425fc-183">Bkz: [SendGrid ile ücretsiz olarak kullanmaya başlayın](https://sendgrid.com/free/) ücretsiz SendGrid hesabı kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="425fc-183">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="425fc-184">IEmailSender uygulayın</span><span class="sxs-lookup"><span data-stu-id="425fc-184">Implement IEmailSender</span></span>

<span data-ttu-id="425fc-185">Uygulama için `IEmailSender`, oluşturma *Services/EmailSender.cs* kodu aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="425fc-185">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="425fc-186">Başlangıç e-posta destekleyecek şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="425fc-186">Configure startup to support email</span></span>

<span data-ttu-id="425fc-187">Aşağıdaki kodu ekleyin `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="425fc-187">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="425fc-188">Ekleme `EmailSender` tek bir hizmet olarak.</span><span class="sxs-lookup"><span data-stu-id="425fc-188">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="425fc-189">Kayıt `AuthMessageSenderOptions` yapılandırma örneği.</span><span class="sxs-lookup"><span data-stu-id="425fc-189">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="425fc-190">Hesap onaylama ve parola kurtarmayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="425fc-190">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="425fc-191">Şablon hesap onaylama ve parola kurtarma kodu var.</span><span class="sxs-lookup"><span data-stu-id="425fc-191">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="425fc-192">Bulma `OnPostAsync` yönteminde *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="425fc-192">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="425fc-193">Yeni kaydettiğiniz kullanıcıların otomatik olarak aşağıdaki satırı açıklama satırı yaparak oturum engellemek:</span><span class="sxs-lookup"><span data-stu-id="425fc-193">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="425fc-194">Vurgulanmış satır ile tam yöntemi gösterilir:</span><span class="sxs-lookup"><span data-stu-id="425fc-194">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="425fc-195">Kaydetme, e-posta onayı ve parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="425fc-195">Register, confirm email, and reset password</span></span>

<span data-ttu-id="425fc-196">Web uygulamasını çalıştırın ve hesap onaylama ve parola kurtarma akışı test edin.</span><span class="sxs-lookup"><span data-stu-id="425fc-196">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="425fc-197">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="425fc-197">Run the app and register a new user</span></span>

  ![Web uygulaması görünümü hesabı Kaydet](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="425fc-199">Hesap onay bağlantısı için e-postanızı kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="425fc-199">Check your email for the account confirmation link.</span></span> <span data-ttu-id="425fc-200">Bkz: [hata ayıklama, e-posta](#debug) e-posta alırsanız yok.</span><span class="sxs-lookup"><span data-stu-id="425fc-200">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="425fc-201">E-postanızı doğrulamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="425fc-201">Click the link to confirm your email.</span></span>
* <span data-ttu-id="425fc-202">E-posta ve parolayla oturum.</span><span class="sxs-lookup"><span data-stu-id="425fc-202">Log in with your email and password.</span></span>
* <span data-ttu-id="425fc-203">Oturumunuzu kapatın.</span><span class="sxs-lookup"><span data-stu-id="425fc-203">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="425fc-204">Yönet sayfasını görüntüle</span><span class="sxs-lookup"><span data-stu-id="425fc-204">View the manage page</span></span>

<span data-ttu-id="425fc-205">Tarayıcıda kullanıcı adınızı seçin: ![tarayıcı penceresi ile kullanıcı adı](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="425fc-205">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="425fc-206">Kullanıcı adı görmek için Gezinti genişletmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="425fc-206">You might need to expand the navbar to see user name.</span></span>

![Gezinti çubuğu](accconfirm/_static/x.png)

<span data-ttu-id="425fc-208">İle Yönet sayfasında görüntülenen **profili** sekmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="425fc-208">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="425fc-209">**E-posta** e-posta belirten bir onay kutusu onaylanan gösterir.</span><span class="sxs-lookup"><span data-stu-id="425fc-209">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="425fc-210">Test parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="425fc-210">Test password reset</span></span>

* <span data-ttu-id="425fc-211">Oturum açmadıysanız, seçin **oturum kapatma**.</span><span class="sxs-lookup"><span data-stu-id="425fc-211">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="425fc-212">Seçin **oturum** seçin ve bağlama **parolanızı mı unuttunuz?** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="425fc-212">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="425fc-213">Hesap kaydolmak için kullandığınız e-posta girin.</span><span class="sxs-lookup"><span data-stu-id="425fc-213">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="425fc-214">Parolanızı sıfırlamak için bir bağlantı içeren bir e-posta gönderilir.</span><span class="sxs-lookup"><span data-stu-id="425fc-214">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="425fc-215">E-postanızı kontrol edin ve parolanızı sıfırlamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="425fc-215">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="425fc-216">Parolanız başarıyla sıfırlandı sonra e-posta ve yeni bir parola ile oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="425fc-216">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="425fc-217">E-posta hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="425fc-217">Debug email</span></span>

<span data-ttu-id="425fc-218">E-posta çalışma erişemiyorsanız:</span><span class="sxs-lookup"><span data-stu-id="425fc-218">If you can't get email working:</span></span>

* <span data-ttu-id="425fc-219">Bir kesim noktası `EmailSender.Execute` doğrulamak için `SendGridClient.SendEmailAsync` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="425fc-219">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="425fc-220">Oluşturma bir [e-posta göndermek için konsol uygulaması](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) benzer bir kod kullanarak `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="425fc-220">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="425fc-221">Gözden geçirme [e-posta etkinlik](https://sendgrid.com/docs/User_Guide/email_activity.html) sayfası.</span><span class="sxs-lookup"><span data-stu-id="425fc-221">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="425fc-222">İstenmeyen posta klasörünüzü kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="425fc-222">Check your spam folder.</span></span>
* <span data-ttu-id="425fc-223">Farklı bir e-posta sağlayıcısı (Microsoft, Yahoo, Gmail, vb.) başka bir e-posta diğer adı deneyin</span><span class="sxs-lookup"><span data-stu-id="425fc-223">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="425fc-224">Farklı bir e-posta hesaplarına göndermeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="425fc-224">Try sending to different email accounts.</span></span>

<span data-ttu-id="425fc-225">**En iyi güvenlik uygulaması** için **değil** test ve geliştirme üretim gizli dizileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="425fc-225">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="425fc-226">Uygulamayı Azure'a yayımlama, Web uygulamasını Azure Portalı'nda Uygulama ayarları olarak SendGrid parolaları ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425fc-226">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="425fc-227">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="425fc-227">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="425fc-228">Sosyal ve yerel oturum açma hesabını birleştirin</span><span class="sxs-lookup"><span data-stu-id="425fc-228">Combine social and local login accounts</span></span>

<span data-ttu-id="425fc-229">Bu bölümde tamamlamak için önce bir dış kimlik doğrulama sağlayıcısı etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="425fc-229">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="425fc-230">Bkz: [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="425fc-230">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="425fc-231">Yerel hem de sosyal hesaplar, e-posta bağlantısına tıklayarak birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425fc-231">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="425fc-232">Aşağıdaki sırayla "RickAndMSFT@gmail.com" ilk olarak bir yerel oturum açın; oluşturulur ancak, bir sosyal oturum açma ilk hesap oluşturun, sonra bir yerel oturum açma ekleme.</span><span class="sxs-lookup"><span data-stu-id="425fc-232">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web uygulaması: RickAndMSFT@gmail.com kimliği doğrulanmış kullanıcı](accconfirm/_static/rick.png)

<span data-ttu-id="425fc-234">Tıklayarak **Yönet** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="425fc-234">Click on the **Manage** link.</span></span> <span data-ttu-id="425fc-235">Bu hesapla ilişkili 0 dış (sosyal oturum açma bilgileri) unutmayın.</span><span class="sxs-lookup"><span data-stu-id="425fc-235">Note the 0 external (social logins) associated with this account.</span></span>

![Görünümü yönetme](accconfirm/_static/manage.png)

<span data-ttu-id="425fc-237">Başka bir oturum açma hizmeti bağlantısını tıklayın ve uygulama isteklerini kabul etmek.</span><span class="sxs-lookup"><span data-stu-id="425fc-237">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="425fc-238">Aşağıdaki görüntüde, Facebook Dış kimlik doğrulama sağlayıcısı'dır:</span><span class="sxs-lookup"><span data-stu-id="425fc-238">In the following image, Facebook is the external authentication provider:</span></span>

![Facebook listeleme görünümünüzü harici oturum açmaları yönetme](accconfirm/_static/fb.png)

<span data-ttu-id="425fc-240">İki hesap birleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="425fc-240">The two accounts have been combined.</span></span> <span data-ttu-id="425fc-241">Herhangi bir hesabı ile oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="425fc-241">You are able to log on with either account.</span></span> <span data-ttu-id="425fc-242">Kullanıcılarınızın kendi sosyal oturum açma kimlik doğrulama hizmeti çalışmıyor veya bunlar sosyal hesaplarına erişim daha büyük bir olasılıkla kaybettiğinizde durumunda yerel hesaplar eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425fc-242">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="425fc-243">Kullanıcılar bir siteye sahip olduktan sonra hesap onaylama etkinleştir</span><span class="sxs-lookup"><span data-stu-id="425fc-243">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="425fc-244">Var olan tüm kullanıcıları etkinleştirme hesap onaylama sitesinde kullanıcılarla kilitler.</span><span class="sxs-lookup"><span data-stu-id="425fc-244">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="425fc-245">Varolan kullanıcı hesaplarını onaylanan değildir çünkü kilitlidir.</span><span class="sxs-lookup"><span data-stu-id="425fc-245">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="425fc-246">Mevcut kullanıcı kilitlemesinin geçici olarak çözmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="425fc-246">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="425fc-247">Tüm mevcut kullanıcılar onaylanıp olarak işaretlemek için veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="425fc-247">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="425fc-248">Mevcut kullanıcıları onaylayın.</span><span class="sxs-lookup"><span data-stu-id="425fc-248">Confirm exiting users.</span></span> <span data-ttu-id="425fc-249">Örneğin, batch onay bağlantılarının ile e-posta gönderme.</span><span class="sxs-lookup"><span data-stu-id="425fc-249">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end