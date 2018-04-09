---
title: Hesap doğrulama ve ASP.NET Core parola kurtarma
author: rick-anderson
description: E-posta onayı ve parola sıfırlama ile ASP.NET Core uygulama oluşturmayı öğrenin.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 8ad2a63ce007a68eac3b607db454c6b4fc834444
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="4a94b-103">Hesap doğrulama ve ASP.NET Core parola kurtarma</span><span class="sxs-lookup"><span data-stu-id="4a94b-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="4a94b-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="4a94b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="4a94b-105">Bu öğretici, e-posta onayı ve parola sıfırlama ile ASP.NET Core uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="4a94b-106">Bu öğretici olan **değil** başına konu.</span><span class="sxs-lookup"><span data-stu-id="4a94b-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="4a94b-107">Hakkında bilginiz olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="4a94b-107">You should be familiar with:</span></span>

* [<span data-ttu-id="4a94b-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a94b-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="4a94b-109">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="4a94b-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="4a94b-110">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="4a94b-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="4a94b-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4a94b-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="4a94b-112">Bkz: [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC 1.1 ve 2.x sürümleri için.</span><span class="sxs-lookup"><span data-stu-id="4a94b-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a94b-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="4a94b-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="4a94b-114">.NET Core CLI ile yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="4a94b-114">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a94b-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a94b-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* <span data-ttu-id="4a94b-116">`--auth Individual` Bireysel kullanıcı hesapları proje şablonu belirtir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-116">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="4a94b-117">Windows üzerinde eklemek `-uld` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="4a94b-117">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="4a94b-118">Yerel veritabanı yerine SQLite kullanılması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-118">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="4a94b-119">Çalıştırma `new mvc --help` bu komutla ilgili Yardım almak için.</span><span class="sxs-lookup"><span data-stu-id="4a94b-119">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a94b-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a94b-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4a94b-121">CLI veya SQLite kullanıyorsanız, bir komut penceresinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4a94b-121">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="4a94b-122">`--auth Individual` Bireysel kullanıcı hesapları proje şablonu belirtir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-122">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="4a94b-123">Windows üzerinde eklemek `-uld` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="4a94b-123">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="4a94b-124">Yerel veritabanı yerine SQLite kullanılması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-124">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="4a94b-125">Çalıştırma `new mvc --help` bu komutla ilgili Yardım almak için.</span><span class="sxs-lookup"><span data-stu-id="4a94b-125">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="4a94b-126">Alternatif olarak, Visual Studio ile yeni bir ASP.NET Core proje oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4a94b-126">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="4a94b-127">Visual Studio'da yeni bir oluşturma **Web uygulaması** projesi.</span><span class="sxs-lookup"><span data-stu-id="4a94b-127">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="4a94b-128">Seçin **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="4a94b-128">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="4a94b-129">**.NET core** aşağıdaki görüntüde, seçili ancak seçebilirsiniz **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="4a94b-129">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="4a94b-130">Seçin **kimlik doğrulamayı Değiştir** ve kümesine **tek tek kullanıcı hesaplarını**.</span><span class="sxs-lookup"><span data-stu-id="4a94b-130">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="4a94b-131">Varsayılan tutmak **deposu kullanıcı hesapları uygulama**.</span><span class="sxs-lookup"><span data-stu-id="4a94b-131">Keep the default **Store user accounts in-app**.</span></span>

!["Seçilen bireysel kullanıcı hesapları radyo" gösteren yeni proje iletişim kutusu](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="4a94b-133">Test yeni kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="4a94b-133">Test new user registration</span></span>

<span data-ttu-id="4a94b-134">Uygulama, belirleyin **kaydetmek** bağlantı ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="4a94b-134">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="4a94b-135">Entity Framework Çekirdek geçişler çalıştırmak için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="4a94b-135">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="4a94b-136">Bu noktada, yalnızca doğrulama e-posta ile olan [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4a94b-136">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="4a94b-137">Kayıt gönderdikten sonra uygulamaya günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-137">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="4a94b-138">E-postalarına doğrulanıncaya kadar yeni kullanıcılar oturum açamaz böylece daha sonra öğreticide kodu güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-138">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="4a94b-139">Görünüm kimliği veritabanı</span><span class="sxs-lookup"><span data-stu-id="4a94b-139">View the Identity database</span></span>

<span data-ttu-id="4a94b-140">Bkz: [SQLite ile ASP.NET Core MVC projesinde iş](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite veritabanı görüntülemek yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="4a94b-140">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="4a94b-141">Visual Studio için:</span><span class="sxs-lookup"><span data-stu-id="4a94b-141">For Visual Studio:</span></span>

* <span data-ttu-id="4a94b-142">Gelen **Görünüm** menüsünde, select **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="4a94b-142">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="4a94b-143">Gidin **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="4a94b-143">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="4a94b-144">Sağ **dbo. AspNetUsers** > **verileri görüntüleme**:</span><span class="sxs-lookup"><span data-stu-id="4a94b-144">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server Nesne Gezgini AspNetUsers tabloda bağlam menüsünde](accconfirm/_static/ssox.png)

<span data-ttu-id="4a94b-146">Tablonun Not `EmailConfirmed` alanı `False`.</span><span class="sxs-lookup"><span data-stu-id="4a94b-146">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="4a94b-147">Uygulama bir onay e-posta gönderirken bu e-posta yeniden sonraki adımda kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4a94b-147">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="4a94b-148">Sağ tıklatın ve satır **silmek**.</span><span class="sxs-lookup"><span data-stu-id="4a94b-148">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="4a94b-149">E-posta diğer silme, aşağıdaki adımlarda kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="4a94b-149">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="4a94b-150">HTTPS gerektirir</span><span class="sxs-lookup"><span data-stu-id="4a94b-150">Require HTTPS</span></span>

<span data-ttu-id="4a94b-151">Bkz: [HTTPS gerektiren](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="4a94b-151">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="4a94b-152">E-posta onayı iste</span><span class="sxs-lookup"><span data-stu-id="4a94b-152">Require email confirmation</span></span>

<span data-ttu-id="4a94b-153">Yeni bir kullanıcı kaydı e-posta onaylamak için en iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="4a94b-153">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="4a94b-154">E-posta, bunlar değil kimliğine bürünmek başkası doğrulamak için onayı yardımcı olur (diğer bir deyişle, başka birinin e-posta ile kayıtlı olmayan).</span><span class="sxs-lookup"><span data-stu-id="4a94b-154">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="4a94b-155">Bir tartışma Forumu var ve bu engellemek istediğinizi varsayalım "yli@example.com"Kimden olarak kaydetme"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="4a94b-155">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="4a94b-156">E-posta onaysız "nolivetto@contoso.com" istenmeyen e-posta uygulamanızdan alabilir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-156">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="4a94b-157">Kullanıcı olarak yanlışlıkla kayıtlı varsayalım "ylo@example.com" ve "yli", yazım hatası fark yüklediniz.</span><span class="sxs-lookup"><span data-stu-id="4a94b-157">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="4a94b-158">Parola kurtarma uygulama doğru e-postalarına sahip olmadığından bunlar bağlanamayacak.</span><span class="sxs-lookup"><span data-stu-id="4a94b-158">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="4a94b-159">E-posta onayı yalnızca sınırlı koruma aracılarını sağlar.</span><span class="sxs-lookup"><span data-stu-id="4a94b-159">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="4a94b-160">E-posta onayı birçok e-posta hesaplarıyla kötü niyetli kullanıcıların koruma sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="4a94b-160">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="4a94b-161">Genellikle, yeni kullanıcılar sahip oldukları onaylanan bir e-posta önce web siteniz için herhangi bir veri gönderme önlemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4a94b-161">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="4a94b-162">Güncelleştirme `ConfigureServices` onaylanan bir e-posta istemek için:</span><span class="sxs-lookup"><span data-stu-id="4a94b-162">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="4a94b-163">`config.SignIn.RequireConfirmedEmail = true;` kayıtlı kullanıcıların e-postalarına onaylandıktan kadar oturum açmayı engeller.</span><span class="sxs-lookup"><span data-stu-id="4a94b-163">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="4a94b-164">E-posta sağlayıcısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4a94b-164">Configure email provider</span></span>

<span data-ttu-id="4a94b-165">Bu öğreticide, SendGrid e-posta göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4a94b-165">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="4a94b-166">SendGrid hesabı ve e-posta göndermek için anahtarı gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-166">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="4a94b-167">Diğer e-posta sağlayıcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4a94b-167">You can use other email providers.</span></span> <span data-ttu-id="4a94b-168">ASP.NET Core 2.x içeren `System.Net.Mail`, uygulamanızdan e-posta Gönder sağlar.</span><span class="sxs-lookup"><span data-stu-id="4a94b-168">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="4a94b-169">E-posta göndermek için SendGrid veya başka bir e-posta hizmeti kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="4a94b-169">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="4a94b-170">SMTP güvenli ve düzgün şekilde ayarlanmış zordur.</span><span class="sxs-lookup"><span data-stu-id="4a94b-170">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="4a94b-171">[Seçenekleri düzeni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4a94b-171">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="4a94b-172">Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="4a94b-172">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="4a94b-173">Güvenli e-posta anahtar getirmek için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4a94b-173">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="4a94b-174">Bu örnek için `AuthMessageSenderOptions` sınıfı oluşturulur *Services/AuthMessageSenderOptions.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="4a94b-174">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="4a94b-175">Ayarlama `SendGridUser` ve `SendGridKey` ile [gizli Yöneticisi aracını](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="4a94b-175">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="4a94b-176">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4a94b-176">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="4a94b-177">Windows, gizli Yöneticisi anahtarları/değer çiftleri olarak depolar. bir *secrets.json* dosyasını `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` dizin.</span><span class="sxs-lookup"><span data-stu-id="4a94b-177">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="4a94b-178">İçeriğini *secrets.json* olmayan dosya şifreli.</span><span class="sxs-lookup"><span data-stu-id="4a94b-178">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="4a94b-179">*Secrets.json* dosya aşağıda gösterilen ( `SendGridKey` değeri kaldırıldı.)</span><span class="sxs-lookup"><span data-stu-id="4a94b-179">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="4a94b-180">Başlangıç AuthMessageSenderOptions kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4a94b-180">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="4a94b-181">Ekleme `AuthMessageSenderOptions` sonunda hizmet kapsayıcısı `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="4a94b-181">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a94b-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a94b-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a94b-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a94b-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

* * *
### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="4a94b-184">AuthMessageSender sınıfını Yapılandır</span><span class="sxs-lookup"><span data-stu-id="4a94b-184">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="4a94b-185">Bu öğretici aracılığıyla e-posta bildirimleri eklemeyi gösterir [SendGrid](https://sendgrid.com/), ancak SMTP ve diğer mekanizmalarını kullanarak e-posta gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-185">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="4a94b-186">Yükleme `SendGrid` NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="4a94b-186">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="4a94b-187">Komut satırından:</span><span class="sxs-lookup"><span data-stu-id="4a94b-187">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="4a94b-188">Paket Yöneticisi konsolundan aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="4a94b-188">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="4a94b-189">Bkz: [SendGrid ücretsiz olarak başlayın](https://sendgrid.com/free/) ücretsiz SendGrid hesap için kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="4a94b-189">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="4a94b-190">SendGrid yapılandırın</span><span class="sxs-lookup"><span data-stu-id="4a94b-190">Configure SendGrid</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a94b-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a94b-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="4a94b-192">SendGrid yapılandırmak için aşağıdakine benzer bir kod ekleyin *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="4a94b-192">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a94b-193">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a94b-193">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
* <span data-ttu-id="4a94b-194">Kod ekleme *Services/MessageServices.cs* SendGrid yapılandırmak için aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="4a94b-194">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

* * *
## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="4a94b-195">Hesap onayı ve parola kurtarmayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="4a94b-195">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="4a94b-196">Hesap onaylama ve parolayı kurtarma için kod şablonu yok.</span><span class="sxs-lookup"><span data-stu-id="4a94b-196">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="4a94b-197">Bul `OnPostAsync` yönteminde *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="4a94b-197">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a94b-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a94b-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="4a94b-199">Yeni kaydettiğiniz kullanıcıların otomatik olarak aşağıdaki satırını yorum oluşturma oturum açmış engellemek:</span><span class="sxs-lookup"><span data-stu-id="4a94b-199">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="4a94b-200">Tam yöntem vurgulanmış değiştirilen satırıyla gösterilir:</span><span class="sxs-lookup"><span data-stu-id="4a94b-200">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a94b-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a94b-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="4a94b-202">Hesap doğrulama etkinleştirmek için aşağıdaki kodu açıklamadan çıkarın:</span><span class="sxs-lookup"><span data-stu-id="4a94b-202">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="4a94b-203">**Not:** kodu yeni kayıtlı kullanıcı otomatik olarak aşağıdaki satırını yorum oluşturma oturum açmış engelleyen:</span><span class="sxs-lookup"><span data-stu-id="4a94b-203">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="4a94b-204">Kodda uncommenting tarafından parola kurtarmayı etkinleştirme `ForgotPassword` eylemi *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="4a94b-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="4a94b-205">Form öğesinde açıklamadan çıkarın *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4a94b-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="4a94b-206">Kaldırmak istediğiniz `<p> For more information on how to enable reset password ... </p>` bu makaleye bir bağlantı içeren öğe.</span><span class="sxs-lookup"><span data-stu-id="4a94b-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

* * *
## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="4a94b-207">Kayıt, e-posta onaylayın ve parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="4a94b-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="4a94b-208">Web uygulaması çalıştırın ve hesap ve parola kurtarma akışı sınayın.</span><span class="sxs-lookup"><span data-stu-id="4a94b-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="4a94b-209">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="4a94b-209">Run the app and register a new user</span></span>

  ![Web uygulaması hesabını kaydetmek görünümü](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="4a94b-211">Hesap onay bağlantısı için e-postanızı kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="4a94b-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="4a94b-212">Bkz: [Debug e-posta](#debug) e-posta alamazsanız.</span><span class="sxs-lookup"><span data-stu-id="4a94b-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="4a94b-213">E-postanızı onaylamak için bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="4a94b-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="4a94b-214">E-posta ve parola ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="4a94b-214">Log in with your email and password.</span></span>
* <span data-ttu-id="4a94b-215">Oturumunuzu kapatın.</span><span class="sxs-lookup"><span data-stu-id="4a94b-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="4a94b-216">Yönet sayfasını görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="4a94b-216">View the manage page</span></span>

<span data-ttu-id="4a94b-217">Kullanıcı adınızı tarayıcıda seçin: ![kullanıcı adıyla bir tarayıcı penceresi](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="4a94b-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="4a94b-218">Kullanıcı adı görmek için gezinti çubuğu genişletmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-218">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a94b-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a94b-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4a94b-221">Yönet sayfası görüntülenir **profil** sekmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="4a94b-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="4a94b-222">**E-posta** e-posta belirten bir onay kutusu onaylanıp gösterir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-222">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![Yönet sayfası](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a94b-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a94b-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4a94b-225">Bu öğreticide daha sonra belirtiliyor.</span><span class="sxs-lookup"><span data-stu-id="4a94b-225">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="4a94b-226">![Yönet sayfası](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="4a94b-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="4a94b-227">Test parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="4a94b-227">Test password reset</span></span>

* <span data-ttu-id="4a94b-228">Oturum açtınız, seçin **oturum kapatma**.</span><span class="sxs-lookup"><span data-stu-id="4a94b-228">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="4a94b-229">Seçin **oturum** seçin ve bağlama **parolanızı mı unuttunuz?** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="4a94b-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="4a94b-230">Hesabını kaydetmek için kullanılan e-posta girin.</span><span class="sxs-lookup"><span data-stu-id="4a94b-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="4a94b-231">Parolanızı sıfırlamak için bir bağlantı içeren bir e-posta gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-231">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="4a94b-232">E-postanızı kontrol edin ve parolanızı sıfırlamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4a94b-232">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="4a94b-233">Parolanız başarıyla sıfırlandı sonra e-posta ve yeni parolayla oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-233">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="4a94b-234">E-posta hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="4a94b-234">Debug email</span></span>

<span data-ttu-id="4a94b-235">E-posta çalışma alınamıyor ise:</span><span class="sxs-lookup"><span data-stu-id="4a94b-235">If you can't get email working:</span></span>

* <span data-ttu-id="4a94b-236">Oluşturma bir [e-posta göndermek için konsol uygulaması](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="4a94b-236">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="4a94b-237">Gözden geçirme [e-posta etkinliğinin](https://sendgrid.com/docs/User_Guide/email_activity.html) sayfası.</span><span class="sxs-lookup"><span data-stu-id="4a94b-237">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="4a94b-238">İstenmeyen posta klasörünüzü kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="4a94b-238">Check your spam folder.</span></span>
* <span data-ttu-id="4a94b-239">Farklı bir e-posta sağlayıcısı (Microsoft, Yahoo, Gmail, vb.) başka bir e-posta diğer deneyin</span><span class="sxs-lookup"><span data-stu-id="4a94b-239">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="4a94b-240">Farklı bir e-posta hesaplarına göndermeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="4a94b-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="4a94b-241">**En iyi güvenlik uygulaması** için **değil** test ve geliştirme üretim gizlilikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="4a94b-241">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="4a94b-242">Uygulamayı Azure'a yayımlama, uygulama ayarları Azure Web uygulaması portal olarak SendGrid parolaları ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4a94b-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="4a94b-243">Yapılandırma sistemi ortam değişkenlerinin anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4a94b-243">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="4a94b-244">Sosyal ve yerel oturum açma hesaplarını birleştirmek</span><span class="sxs-lookup"><span data-stu-id="4a94b-244">Combine social and local login accounts</span></span>

<span data-ttu-id="4a94b-245">Bu bölümde tamamlamak için önce bir dış kimlik doğrulama sağlayıcısı etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-245">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="4a94b-246">Bkz: [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4a94b-246">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="4a94b-247">Yerel ve sosyal hesapları, e-posta bağlantısını tıklatarak birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4a94b-247">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="4a94b-248">Aşağıdaki sırayla "RickAndMSFT@gmail.com" ilk bir yerel oturum açma; oluşturulur ancak, ilk sosyal bir oturum açma hesabı oluşturmak sonra yerel oturum açma ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4a94b-248">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web uygulaması: RickAndMSFT@gmail.com kimliği doğrulanmış kullanıcı](accconfirm/_static/rick.png)

<span data-ttu-id="4a94b-250">Tıklayın **Yönet** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="4a94b-250">Click on the **Manage** link.</span></span> <span data-ttu-id="4a94b-251">Bu hesapla ilişkilendirilen 0 dış (sosyal oturum açma bilgileri) unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4a94b-251">Note the 0 external (social logins) associated with this account.</span></span>

![Görünüm yönetme](accconfirm/_static/manage.png)

<span data-ttu-id="4a94b-253">Başka bir oturum açma hizmeti bağlantısını tıklatın ve uygulama isteklerini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="4a94b-253">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="4a94b-254">Aşağıdaki görüntüde, Facebook Dış kimlik doğrulama sağlayıcısı'dır:</span><span class="sxs-lookup"><span data-stu-id="4a94b-254">In the following image, Facebook is the external authentication provider:</span></span>

![Harici oturum görünümünüzü Facebook listeleme yönetme](accconfirm/_static/fb.png)

<span data-ttu-id="4a94b-256">İki hesap birleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-256">The two accounts have been combined.</span></span> <span data-ttu-id="4a94b-257">Herhangi bir hesabı ile oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-257">You are able to log on with either account.</span></span> <span data-ttu-id="4a94b-258">Kullanıcılarınızın kendi sosyal oturum açma kimlik doğrulama hizmeti çalışmıyor ya da erişim sosyal hesaplarında daha büyük bir olasılıkla kaybettiğini durumda yerel hesaplar eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4a94b-258">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="4a94b-259">Kullanıcıların bir siteye atandıktan sonra Hesap doğrulama etkinleştir</span><span class="sxs-lookup"><span data-stu-id="4a94b-259">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="4a94b-260">Kullanıcılarla etkinleştirme Hesap doğrulama sitesinde var olan tüm kullanıcıların kilitler.</span><span class="sxs-lookup"><span data-stu-id="4a94b-260">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="4a94b-261">Hesaplarında onaylanıp değil çünkü mevcut kullanıcılar kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="4a94b-261">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="4a94b-262">Kullanıcı kilitlemesi çıkma geçici olarak çözmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="4a94b-262">To work around exiting user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="4a94b-263">Onaylanan olarak tüm mevcut kullanıcılar işaretlemek için veritabanını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="4a94b-263">Update the database to mark all existing users as being confirmed</span></span>
* <span data-ttu-id="4a94b-264">Mevcut kullanıcıları onaylayın.</span><span class="sxs-lookup"><span data-stu-id="4a94b-264">Confirm exiting users.</span></span> <span data-ttu-id="4a94b-265">Örneğin, batch onay bağlantılar ile e-postaları gönderme.</span><span class="sxs-lookup"><span data-stu-id="4a94b-265">For example, batch-send emails with confirmation links.</span></span>
