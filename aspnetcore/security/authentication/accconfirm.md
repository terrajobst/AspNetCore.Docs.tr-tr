---
title: Hesap onaylama ve parola kurtarma ASP.NET Core
author: rick-anderson
description: E-posta onayı ve parola sıfırlama ile ASP.NET Core uygulaması oluşturmayı öğrenin.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803278"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="2646c-103">Hesap onaylama ve parola kurtarma ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2646c-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="2646c-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="2646c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="2646c-105">Bu öğretici, e-posta onayı ve parola sıfırlama ile bir ASP.NET Core uygulaması derleme işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="2646c-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="2646c-106">Bu öğretici **değil** başına konu.</span><span class="sxs-lookup"><span data-stu-id="2646c-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="2646c-107">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="2646c-107">You should be familiar with:</span></span>

* [<span data-ttu-id="2646c-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2646c-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="2646c-109">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="2646c-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="2646c-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2646c-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="2646c-111">Bkz: [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC 1.1 ve 2.x'i sürümleri.</span><span class="sxs-lookup"><span data-stu-id="2646c-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2646c-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="2646c-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="2646c-113">.NET Core CLI ile yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="2646c-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2646c-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2646c-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="2646c-115">`--auth Individual` Bireysel kullanıcı hesapları proje şablonu belirtir.</span><span class="sxs-lookup"><span data-stu-id="2646c-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="2646c-116">Windows üzerinde ekleme `-uld` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="2646c-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="2646c-117">Bu, LocalDB yerine SQLite kullanılması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="2646c-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="2646c-118">Çalıştırma `new mvc --help` bu komutla ilgili Yardım almak için.</span><span class="sxs-lookup"><span data-stu-id="2646c-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2646c-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2646c-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2646c-120">CLI veya SQLite kullanıyorsanız, bir komut penceresinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2646c-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="2646c-121">`--auth Individual` Bireysel kullanıcı hesapları proje şablonu belirtir.</span><span class="sxs-lookup"><span data-stu-id="2646c-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="2646c-122">Windows üzerinde ekleme `-uld` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="2646c-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="2646c-123">Bu, LocalDB yerine SQLite kullanılması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="2646c-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="2646c-124">Çalıştırma `new mvc --help` bu komutla ilgili Yardım almak için.</span><span class="sxs-lookup"><span data-stu-id="2646c-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="2646c-125">Alternatif olarak, Visual Studio ile yeni bir ASP.NET Core projesi oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2646c-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="2646c-126">Visual Studio'da yeni bir oluşturma **Web uygulaması** proje.</span><span class="sxs-lookup"><span data-stu-id="2646c-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="2646c-127">Seçin **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="2646c-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="2646c-128">**.NET core** aşağıdaki görüntüde, seçilen ancak seçebileceğiniz **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="2646c-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="2646c-129">Seçin **kimlik doğrulamayı Değiştir** ayarlanmış ve **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="2646c-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="2646c-130">Varsayılan tutun **Store kullanıcı hesaplarını uygulama**.</span><span class="sxs-lookup"><span data-stu-id="2646c-130">Keep the default **Store user accounts in-app**.</span></span>

!["Bireysel kullanıcı hesapları radyo seçili" gösteren yeni proje iletişim kutusu](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="2646c-132">Test yeni kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="2646c-132">Test new user registration</span></span>

<span data-ttu-id="2646c-133">Uygulamayı çalıştırın, seçin **kaydetme** bağlamak ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="2646c-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="2646c-134">Entity Framework Code migrations'ı çalıştırmak için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="2646c-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="2646c-135">Yalnızca e-posta doğrulamasını bu noktada, olan [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2646c-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="2646c-136">Kayıt gönderdikten sonra uygulamaya günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2646c-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="2646c-137">E-postasına doğrulanıncaya kadar yeni kullanıcılar oturum açamaz böylece daha sonra öğreticide kod güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2646c-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="2646c-138">Kimlik veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="2646c-138">View the Identity database</span></span>

<span data-ttu-id="2646c-139">Bkz: [ASP.NET Core MVC projesinde SQLite ile çalışma](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite veritabanını görüntülemek yönergeler.</span><span class="sxs-lookup"><span data-stu-id="2646c-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="2646c-140">Visual Studio için:</span><span class="sxs-lookup"><span data-stu-id="2646c-140">For Visual Studio:</span></span>

* <span data-ttu-id="2646c-141">Gelen **görünümü** menüsünde **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="2646c-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="2646c-142">Gidin **(localdb) (SQL Server 13) ifadesini MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="2646c-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="2646c-143">Sağ **dbo. AspNetUsers** > **verileri görüntüleme**:</span><span class="sxs-lookup"><span data-stu-id="2646c-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server Nesne Gezgini AspNetUsers tablosunda bağlamsal menü](accconfirm/_static/ssox.png)

<span data-ttu-id="2646c-145">Tablonun Not `EmailConfirmed` alandır `False`.</span><span class="sxs-lookup"><span data-stu-id="2646c-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="2646c-146">Bu e-posta uygulaması bir onay e-posta gönderdiğinde, yeniden sonraki adımda kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2646c-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="2646c-147">Sağ tıklatın ve satır **Sil**.</span><span class="sxs-lookup"><span data-stu-id="2646c-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="2646c-148">E-posta diğer adı silmek, aşağıdaki adımlarda kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="2646c-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="2646c-149">HTTPS'yi zorunlu</span><span class="sxs-lookup"><span data-stu-id="2646c-149">Require HTTPS</span></span>

<span data-ttu-id="2646c-150">Bkz: [HTTPS'yi zorunlu](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="2646c-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="2646c-151">E-posta onayı gerektir</span><span class="sxs-lookup"><span data-stu-id="2646c-151">Require email confirmation</span></span>

<span data-ttu-id="2646c-152">Yeni bir kullanıcı kaydı e-postayı onaylamak için iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="2646c-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="2646c-153">E-posta, bunlar değil kimliğe bürünerek başka birisi doğrulamak için onay yardımcı olur (diğer bir deyişle, bunlar başka birinin e-posta ile kayıtlı olmayabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="2646c-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="2646c-154">Tartışma Forumu var ve bu önlemek istediğinizi varsayalım "yli@example.com"olarak kaydetme"Kimdennolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="2646c-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="2646c-155">E-posta onayı olmadan "nolivetto@contoso.com" istenmeyen e-posta uygulamanızdan alabilir.</span><span class="sxs-lookup"><span data-stu-id="2646c-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="2646c-156">Kullanıcı olarak yanlışlıkla kayıtlı varsayalım "ylo@example.com" ve "yli", yazım hatası fark yüklediniz.</span><span class="sxs-lookup"><span data-stu-id="2646c-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="2646c-157">Parola kurtarma uygulamayı doğru e-postasına olmadığından bunlar saptayamazdınız.</span><span class="sxs-lookup"><span data-stu-id="2646c-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="2646c-158">E-posta onayı robotlar yalnızca sınırlı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="2646c-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="2646c-159">E-posta onayı, çok sayıda e-posta hesaplarına sahip kötü niyetli kullanıcıların koruma sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="2646c-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="2646c-160">Genellikle, yeni kullanıcıların onaylanan e-posta sahip oldukları önce web sitenizi herhangi bir veri gönderme engellemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="2646c-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="2646c-161">Güncelleştirme `ConfigureServices` onaylanan e-posta gerektirmek için:</span><span class="sxs-lookup"><span data-stu-id="2646c-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="2646c-162">`config.SignIn.RequireConfirmedEmail = true;` kayıtlı kullanıcıların e-postasına onaylanana kadar oturum açma engeller.</span><span class="sxs-lookup"><span data-stu-id="2646c-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="2646c-163">E-posta sağlayıcısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2646c-163">Configure email provider</span></span>

<span data-ttu-id="2646c-164">Bu öğreticide, SendGrid e-posta göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2646c-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="2646c-165">SendGrid hesabı ve e-posta göndermek için anahtar ihtiyacınız var.</span><span class="sxs-lookup"><span data-stu-id="2646c-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="2646c-166">Diğer e-posta sağlayıcılarının kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2646c-166">You can use other email providers.</span></span> <span data-ttu-id="2646c-167">ASP.NET Core 2.x içerir `System.Net.Mail`, uygulamanızdan e-posta göndermenize olanak tanıyan.</span><span class="sxs-lookup"><span data-stu-id="2646c-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="2646c-168">E-posta göndermek için SendGrid veya başka bir e-posta hizmeti kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="2646c-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="2646c-169">SMTP güvenli ve doğru bir şekilde ayarlamak zordur.</span><span class="sxs-lookup"><span data-stu-id="2646c-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="2646c-170">[Seçenekleri deseni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2646c-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="2646c-171">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="2646c-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="2646c-172">Güvenli e-posta anahtarı almak için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2646c-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="2646c-173">Bu örnek için `AuthMessageSenderOptions` sınıf oluşturulduğu *Services/AuthMessageSenderOptions.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="2646c-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="2646c-174">Ayarlama `SendGridUser` ve `SendGridKey` ile [gizli dizi Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2646c-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="2646c-175">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2646c-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="2646c-176">Windows üzerinde gizli dizi Yöneticisi'ni anahtar/değer çiftleri olarak depolar. bir *secrets.json* dosyası `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` dizin.</span><span class="sxs-lookup"><span data-stu-id="2646c-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="2646c-177">İçeriğini *secrets.json* olmayan dosya şifrelenmiş.</span><span class="sxs-lookup"><span data-stu-id="2646c-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="2646c-178">*Secrets.json* aşağıda gösterilmektedir dosyanın ( `SendGridKey` değer kaldırıldı.)</span><span class="sxs-lookup"><span data-stu-id="2646c-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="2646c-179">Başlangıç AuthMessageSenderOptions kullanmak için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2646c-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="2646c-180">Ekleme `AuthMessageSenderOptions` sonunda hizmet kapsayıcıya `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="2646c-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2646c-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2646c-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2646c-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2646c-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="2646c-183">AuthMessageSender sınıfını Yapılandır</span><span class="sxs-lookup"><span data-stu-id="2646c-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="2646c-184">Bu öğretici, e-posta bildirimleri aracılığıyla ekleneceği gösterilmiştir [SendGrid](https://sendgrid.com/), ancak e-posta SMTP veya başka mekanizmalar kullanılarak gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2646c-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="2646c-185">Yükleme `SendGrid` NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="2646c-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="2646c-186">Komut satırından:</span><span class="sxs-lookup"><span data-stu-id="2646c-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="2646c-187">Paket Yöneticisi konsolundan aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="2646c-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="2646c-188">Bkz: [SendGrid ile ücretsiz olarak kullanmaya başlayın](https://sendgrid.com/free/) ücretsiz SendGrid hesabı kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="2646c-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="2646c-189">SendGrid yapılandırın</span><span class="sxs-lookup"><span data-stu-id="2646c-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2646c-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2646c-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2646c-191">SendGrid yapılandırmak için aşağıdakine benzer bir kod ekleme *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="2646c-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2646c-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2646c-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="2646c-193">Kodda *Services/MessageServices.cs* SendGrid yapılandırmak için aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="2646c-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="2646c-194">Hesap onaylama ve parola kurtarmayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="2646c-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="2646c-195">Şablon hesap onaylama ve parola kurtarma kodu var.</span><span class="sxs-lookup"><span data-stu-id="2646c-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="2646c-196">Bulma `OnPostAsync` yönteminde *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="2646c-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2646c-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2646c-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2646c-198">Yeni kaydettiğiniz kullanıcıların otomatik olarak aşağıdaki satırı açıklama satırı yaparak oturum engellemek:</span><span class="sxs-lookup"><span data-stu-id="2646c-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="2646c-199">Vurgulanmış satır ile tam yöntemi gösterilir:</span><span class="sxs-lookup"><span data-stu-id="2646c-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2646c-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2646c-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2646c-201">Hesap onaylama etkinleştirmek için aşağıdaki kodun açıklamasını kaldırın:</span><span class="sxs-lookup"><span data-stu-id="2646c-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="2646c-202">**Not:** kodu yeni kaydettiğiniz kullanıcı otomatik olarak aşağıdaki satırı açıklama satırı yaparak oturum engelleyen:</span><span class="sxs-lookup"><span data-stu-id="2646c-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="2646c-203">Kodda uncommenting tarafından parola kurtarmayı etkinleştirmek `ForgotPassword` eylemi *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="2646c-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="2646c-204">Form öğesinde açıklamadan çıkarın *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2646c-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="2646c-205">Kaldırmak istediğiniz `<p> For more information on how to enable reset password ... </p>` bu makalede bir bağlantı içeren öğe.</span><span class="sxs-lookup"><span data-stu-id="2646c-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="2646c-206">Kaydetme, e-posta onayı ve parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="2646c-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="2646c-207">Web uygulamasını çalıştırın ve hesap onaylama ve parola kurtarma akışı test edin.</span><span class="sxs-lookup"><span data-stu-id="2646c-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="2646c-208">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="2646c-208">Run the app and register a new user</span></span>

  ![Web uygulaması görünümü hesabı Kaydet](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="2646c-210">Hesap onay bağlantısı için e-postanızı kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="2646c-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="2646c-211">Bkz: [hata ayıklama, e-posta](#debug) e-posta alırsanız yok.</span><span class="sxs-lookup"><span data-stu-id="2646c-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="2646c-212">E-postanızı doğrulamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2646c-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="2646c-213">E-posta ve parolayla oturum.</span><span class="sxs-lookup"><span data-stu-id="2646c-213">Log in with your email and password.</span></span>
* <span data-ttu-id="2646c-214">Oturumunuzu kapatın.</span><span class="sxs-lookup"><span data-stu-id="2646c-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="2646c-215">Yönet sayfasını görüntüle</span><span class="sxs-lookup"><span data-stu-id="2646c-215">View the manage page</span></span>

<span data-ttu-id="2646c-216">Tarayıcıda kullanıcı adınızı seçin: ![tarayıcı penceresi ile kullanıcı adı](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="2646c-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="2646c-217">Kullanıcı adı görmek için Gezinti genişletmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="2646c-217">You might need to expand the navbar to see user name.</span></span>

![Gezinti çubuğu](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2646c-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2646c-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2646c-220">İle Yönet sayfasında görüntülenen **profili** sekmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="2646c-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="2646c-221">**E-posta** e-posta belirten bir onay kutusu onaylanan gösterir.</span><span class="sxs-lookup"><span data-stu-id="2646c-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![Yönet sayfası](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2646c-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2646c-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2646c-224">Bu öğreticinin ilerleyen bölümlerinde açıklanan.</span><span class="sxs-lookup"><span data-stu-id="2646c-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="2646c-225">![Yönet sayfası](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="2646c-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="2646c-226">Test parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="2646c-226">Test password reset</span></span>

* <span data-ttu-id="2646c-227">Oturum açmadıysanız, seçin **oturum kapatma**.</span><span class="sxs-lookup"><span data-stu-id="2646c-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="2646c-228">Seçin **oturum** seçin ve bağlama **parolanızı mı unuttunuz?** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="2646c-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="2646c-229">Hesap kaydolmak için kullandığınız e-posta girin.</span><span class="sxs-lookup"><span data-stu-id="2646c-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="2646c-230">Parolanızı sıfırlamak için bir bağlantı içeren bir e-posta gönderilir.</span><span class="sxs-lookup"><span data-stu-id="2646c-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="2646c-231">E-postanızı kontrol edin ve parolanızı sıfırlamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2646c-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="2646c-232">Parolanız başarıyla sıfırlandı sonra e-posta ve yeni bir parola ile oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="2646c-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="2646c-233">E-posta hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="2646c-233">Debug email</span></span>

<span data-ttu-id="2646c-234">E-posta çalışma erişemiyorsanız:</span><span class="sxs-lookup"><span data-stu-id="2646c-234">If you can't get email working:</span></span>

* <span data-ttu-id="2646c-235">Oluşturma bir [e-posta göndermek için konsol uygulaması](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="2646c-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="2646c-236">Gözden geçirme [e-posta etkinlik](https://sendgrid.com/docs/User_Guide/email_activity.html) sayfası.</span><span class="sxs-lookup"><span data-stu-id="2646c-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="2646c-237">İstenmeyen posta klasörünüzü kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="2646c-237">Check your spam folder.</span></span>
* <span data-ttu-id="2646c-238">Farklı bir e-posta sağlayıcısı (Microsoft, Yahoo, Gmail, vb.) başka bir e-posta diğer adı deneyin</span><span class="sxs-lookup"><span data-stu-id="2646c-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="2646c-239">Farklı bir e-posta hesaplarına göndermeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="2646c-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="2646c-240">**En iyi güvenlik uygulaması** için **değil** test ve geliştirme üretim gizli dizileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="2646c-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="2646c-241">Uygulamayı Azure'a yayımlama, Web uygulamasını Azure Portalı'nda Uygulama ayarları olarak SendGrid parolaları ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2646c-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="2646c-242">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2646c-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="2646c-243">Sosyal ve yerel oturum açma hesabını birleştirin</span><span class="sxs-lookup"><span data-stu-id="2646c-243">Combine social and local login accounts</span></span>

<span data-ttu-id="2646c-244">Bu bölümde tamamlamak için önce bir dış kimlik doğrulama sağlayıcısı etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2646c-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="2646c-245">Bkz: [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2646c-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="2646c-246">Yerel hem de sosyal hesaplar, e-posta bağlantısına tıklayarak birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2646c-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="2646c-247">Aşağıdaki sırayla "RickAndMSFT@gmail.com" ilk olarak bir yerel oturum açın; oluşturulur ancak, bir sosyal oturum açma ilk hesap oluşturun, sonra bir yerel oturum açma ekleme.</span><span class="sxs-lookup"><span data-stu-id="2646c-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web uygulaması: RickAndMSFT@gmail.com kimliği doğrulanmış kullanıcı](accconfirm/_static/rick.png)

<span data-ttu-id="2646c-249">Tıklayarak **Yönet** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="2646c-249">Click on the **Manage** link.</span></span> <span data-ttu-id="2646c-250">Bu hesapla ilişkili 0 dış (sosyal oturum açma bilgileri) unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2646c-250">Note the 0 external (social logins) associated with this account.</span></span>

![Görünümü yönetme](accconfirm/_static/manage.png)

<span data-ttu-id="2646c-252">Başka bir oturum açma hizmeti bağlantısını tıklayın ve uygulama isteklerini kabul etmek.</span><span class="sxs-lookup"><span data-stu-id="2646c-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="2646c-253">Aşağıdaki görüntüde, Facebook Dış kimlik doğrulama sağlayıcısı'dır:</span><span class="sxs-lookup"><span data-stu-id="2646c-253">In the following image, Facebook is the external authentication provider:</span></span>

![Facebook listeleme görünümünüzü harici oturum açmaları yönetme](accconfirm/_static/fb.png)

<span data-ttu-id="2646c-255">İki hesap birleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2646c-255">The two accounts have been combined.</span></span> <span data-ttu-id="2646c-256">Herhangi bir hesabı ile oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="2646c-256">You are able to log on with either account.</span></span> <span data-ttu-id="2646c-257">Kullanıcılarınızın kendi sosyal oturum açma kimlik doğrulama hizmeti çalışmıyor veya bunlar sosyal hesaplarına erişim daha büyük bir olasılıkla kaybettiğinizde durumunda yerel hesaplar eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2646c-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="2646c-258">Kullanıcılar bir siteye sahip olduktan sonra hesap onaylama etkinleştir</span><span class="sxs-lookup"><span data-stu-id="2646c-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="2646c-259">Var olan tüm kullanıcıları etkinleştirme hesap onaylama sitesinde kullanıcılarla kilitler.</span><span class="sxs-lookup"><span data-stu-id="2646c-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="2646c-260">Varolan kullanıcı hesaplarını onaylanan değildir çünkü kilitlidir.</span><span class="sxs-lookup"><span data-stu-id="2646c-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="2646c-261">Mevcut kullanıcı kilitlemesinin geçici olarak çözmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="2646c-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="2646c-262">Tüm mevcut kullanıcılar onaylanıp olarak işaretlemek için veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="2646c-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="2646c-263">Mevcut kullanıcıları onaylayın.</span><span class="sxs-lookup"><span data-stu-id="2646c-263">Confirm exiting users.</span></span> <span data-ttu-id="2646c-264">Örneğin, batch onay bağlantılarının ile e-posta gönderme.</span><span class="sxs-lookup"><span data-stu-id="2646c-264">For example, batch-send emails with confirmation links.</span></span>
