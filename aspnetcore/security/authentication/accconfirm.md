---
title: "Hesap doğrulama ve ASP.NET Core parola kurtarma"
author: rick-anderson
description: "E-posta onayı ve parola sıfırlama ile ASP.NET Core uygulamasının nasıl oluşturulacağını gösterir."
ms.author: riande
manager: wpickett
ms.date: 12/1/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: bc9febc41d0637be9f83a02799d360489f257849
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="4e5d3-103">Hesap doğrulama ve ASP.NET Core parola kurtarma</span><span class="sxs-lookup"><span data-stu-id="4e5d3-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="4e5d3-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="4e5d3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="4e5d3-105">Bu öğretici, e-posta onayı ve parola sıfırlama ile ASP.NET Core uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="4e5d3-106">Yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="4e5d3-106">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e5d3-107">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-107">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4e5d3-108">Bu adım Windows Visual Studio için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-108">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="4e5d3-109">CLI yönergeleri için sonraki bölüme bakın.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-109">See the next section for CLI instructions.</span></span>

<span data-ttu-id="4e5d3-110">Öğretici, Visual Studio 2017 Preview 2 veya üstünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-110">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="4e5d3-111">Visual Studio'da yeni bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-111">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="4e5d3-112">Seçin **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-112">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="4e5d3-113">Aşağıdaki resim Göster **.NET Core** seçildi, ancak seçebilirsiniz **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-113">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="4e5d3-114">Seçin **kimlik doğrulamayı Değiştir** ve kümesine **tek tek kullanıcı hesaplarını**.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-114">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="4e5d3-115">Varsayılan tutmak **deposu kullanıcı hesapları uygulama**.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-115">Keep the default **Store user accounts in-app**.</span></span>

!["Seçilen bireysel kullanıcı hesapları radyo" gösteren yeni proje iletişim kutusu](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e5d3-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e5d3-118">Öğretici Visual Studio 2017 veya üstünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-118">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="4e5d3-119">Visual Studio'da yeni bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-119">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="4e5d3-120">Seçin **kimlik doğrulamayı Değiştir** ve kümesine **tek tek kullanıcı hesaplarını**.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-120">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

!["Seçilen bireysel kullanıcı hesapları radyo" gösteren yeni proje iletişim kutusu](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="4e5d3-122">MacOS ve Linux için .NET core CLI proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="4e5d3-122">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="4e5d3-123">CLI veya SQLite kullanıyorsanız, bir komut penceresinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-123">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="4e5d3-124">`--auth Individual`Bireysel kullanıcı hesapları şablonu belirtir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-124">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="4e5d3-125">Windows üzerinde eklemek `-uld` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-125">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="4e5d3-126">`-uld` Seçenek bir yerel veritabanı bağlantı dizesi yerine bir SQLite veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-126">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="4e5d3-127">Çalıştırma `new mvc --help` bu komutla ilgili Yardım almak için.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-127">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="4e5d3-128">Test yeni kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="4e5d3-128">Test new user registration</span></span>

<span data-ttu-id="4e5d3-129">Uygulama, belirleyin **kaydetmek** bağlantı ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="4e5d3-130">Entity Framework Çekirdek geçişler çalıştırmak için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-130">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="4e5d3-131">Bu noktada, yalnızca doğrulama e-posta ile olan [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-131">At this  point, the only validation on the email is with the [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="4e5d3-132">Kayıt gönderdikten sonra uygulamaya günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-132">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="4e5d3-133">E-postalarına doğrulanıncaya kadar yeni kullanıcılar oturum açamaz böylece daha sonra öğreticide Biz bu değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-133">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="4e5d3-134">Görünüm kimliği veritabanı</span><span class="sxs-lookup"><span data-stu-id="4e5d3-134">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="4e5d3-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="4e5d3-135">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="4e5d3-136">Gelen **Görünüm** menüsünde, select **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="4e5d3-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="4e5d3-137">Gidin **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="4e5d3-138">Sağ **dbo. AspNetUsers** > **verileri görüntüleme**:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server Nesne Gezgini AspNetUsers tabloda bağlam menüsünde](accconfirm/_static/ssox.png)

<span data-ttu-id="4e5d3-140">Not `EmailConfirmed` alanı `False`.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-140">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="4e5d3-141">Uygulama bir onay e-posta gönderirken bu e-posta yeniden sonraki adımda kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="4e5d3-142">Sağ tıklatın ve satır **silmek**.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="4e5d3-143">E-posta diğer adı şimdi silme aşağıdaki adımlarda kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-143">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="4e5d3-144">SQLite</span><span class="sxs-lookup"><span data-stu-id="4e5d3-144">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="4e5d3-145">Bkz: [SQLite ile ASP.NET Core MVC projesinde çalışma](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite DB görüntülemek yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-145">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="4e5d3-146">SSL gerektirecek ve IIS Express için SSL Kurulumu</span><span class="sxs-lookup"><span data-stu-id="4e5d3-146">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="4e5d3-147">Bkz: [SSL zorlamayı](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="4e5d3-147">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="4e5d3-148">E-posta onayı iste</span><span class="sxs-lookup"><span data-stu-id="4e5d3-148">Require email confirmation</span></span>

<span data-ttu-id="4e5d3-149">Bunlar değil kimliğine bürünmek başkası doğrulamak için yeni bir kullanıcı kaydı e-posta onaylamak için en iyi bir uygulamadır (diğer bir deyişle, başka birinin e-posta ile kayıtlı olmayan).</span><span class="sxs-lookup"><span data-stu-id="4e5d3-149">It's a best practice to confirm the email of a new user registration to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="4e5d3-150">Bir tartışma Forumu var ve bu engellemek istediğinizi varsayalım "yli@example.com"Kimden olarak kaydetme"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="4e5d3-150">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="4e5d3-151">E-posta onaysız "nolivetto@contoso.com" istenmeyen e-posta uygulamanızdan alabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-151">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="4e5d3-152">Kullanıcı olarak yanlışlıkla kayıtlı varsayalım "ylo@example.com" ve yazım hatası fark yüklediniz "yli," uygulama doğru e-postalarına sahip olmadığından parola kurtarma kullanılacak karşılayamazlar.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-152">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="4e5d3-153">E-posta onayı aracılarını, yalnızca sınırlı koruma sağlar ve kaydetmek için kullanabilirsiniz çok sayıda çalışan e-posta diğer adları olan belirlendiği istenmeyen posta gönderenlerin koruma sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-153">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="4e5d3-154">Genellikle, yeni kullanıcılar sahip oldukları onaylanan bir e-posta önce web siteniz için herhangi bir veri gönderme önlemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="4e5d3-155">Güncelleştirme `ConfigureServices` onaylanan bir e-posta istemek için:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-155">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e5d3-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e5d3-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="4e5d3-158">Önceki satır kayıtlı kullanıcıların e-postalarına onaylandıktan kadar oturum açılmış engeller.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-158">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="4e5d3-159">Ancak, o satırdaki yeni kullanıcıların bunlar kaydettikten sonra oturum açılmış engellemek değil.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-159">However, that line doesn't prevent new users from being logged in after they register.</span></span> <span data-ttu-id="4e5d3-160">Bunlar kaydettikten sonra varsayılan kod kullanıcı olarak günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-160">The default code logs in a user after they register.</span></span> <span data-ttu-id="4e5d3-161">Oturum kapatma sonra bunlar kaydoluncaya kadar yeniden oturum açmak değiştiremezler.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-161">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="4e5d3-162">Daha sonra değiştirmek öğreticide kadar yeni kaydedilen kod kullanıcısıysanız **değil** oturum.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-162">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="4e5d3-163">E-posta sağlayıcısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4e5d3-163">Configure email provider</span></span>

<span data-ttu-id="4e5d3-164">Bu öğreticide, SendGrid e-posta göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="4e5d3-165">SendGrid hesabı ve e-posta göndermek için anahtarı gerekir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="4e5d3-166">Diğer e-posta sağlayıcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-166">You can use other email providers.</span></span> <span data-ttu-id="4e5d3-167">ASP.NET Core 2.x içeren `System.Net.Mail`, uygulamanızdan e-posta Gönder sağlar.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="4e5d3-168">E-posta göndermek için SendGrid veya başka bir e-posta hizmeti kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-168">We recommend you use SendGrid or another email service to send email.</span></span>

<span data-ttu-id="4e5d3-169">[Seçenekleri düzeni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-169">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="4e5d3-170">Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="4e5d3-170">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="4e5d3-171">Güvenli e-posta anahtar getirmek için bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-171">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="4e5d3-172">Bu örnek için `AuthMessageSenderOptions` sınıfı oluşturulur *Services/AuthMessageSenderOptions.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-172">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="4e5d3-173">Ayarlama `SendGridUser` ve `SendGridKey` ile [gizli Yöneticisi aracını](../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="4e5d3-173">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="4e5d3-174">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-174">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="4e5d3-175">Windows, gizli Yöneticisi anahtarları/değer çiftleri olarak depolar. bir *secrets.json* %APPDATA%/Microsoft/UserSecrets/ < WebAppName-userSecretsId > dizindeki dosyayı.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-175">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="4e5d3-176">İçeriğini *secrets.json* dosya şifrelenmez.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-176">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="4e5d3-177">*Secrets.json* dosya aşağıda gösterilen ( `SendGridKey` değeri kaldırıldı.)</span><span class="sxs-lookup"><span data-stu-id="4e5d3-177">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="4e5d3-178">Başlangıç AuthMessageSenderOptions kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4e5d3-178">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="4e5d3-179">Ekleme `AuthMessageSenderOptions` sonunda hizmet kapsayıcısı `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-179">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e5d3-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e5d3-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="4e5d3-182">AuthMessageSender sınıfını Yapılandır</span><span class="sxs-lookup"><span data-stu-id="4e5d3-182">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="4e5d3-183">Bu öğretici aracılığıyla e-posta bildirimleri eklemeyi gösterir [SendGrid](https://sendgrid.com/), ancak SMTP ve diğer mekanizmalarını kullanarak e-posta gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-183">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="4e5d3-184">Yükleme `SendGrid` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-184">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="4e5d3-185">Paket Yöneticisi konsolundan aşağıdaki girin aşağıdaki komutu:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-185">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="4e5d3-186">Bkz: [SendGrid ücretsiz olarak başlayın](https://sendgrid.com/free/) ücretsiz SendGrid hesap için kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-186">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="4e5d3-187">SendGrid yapılandırın</span><span class="sxs-lookup"><span data-stu-id="4e5d3-187">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e5d3-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="4e5d3-189">Kod ekleme *Services/EmailSender.cs* SendGrid yapılandırmak için aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-189">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e5d3-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="4e5d3-191">Kod ekleme *Services/MessageServices.cs* SendGrid yapılandırmak için aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-191">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="4e5d3-192">Hesap onayı ve parola kurtarmayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="4e5d3-192">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="4e5d3-193">Hesap onaylama ve parolayı kurtarma için kod şablonu yok.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-193">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="4e5d3-194">Bul `[HttpPost] Register` yönteminde *AccountController.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-194">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e5d3-195">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-195">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4e5d3-196">Yeni kaydettiğiniz kullanıcıların otomatik olarak aşağıdaki satırını yorum oluşturma oturum açmış engellemek:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-196">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="4e5d3-197">Tam yöntem vurgulanmış değiştirilen satırıyla gösterilir:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-197">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

<span data-ttu-id="4e5d3-198">Not: uygulamanız önceki kod başarısız olur `IEmailSender` ve düz metin e-posta gönderin.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-198">Note: The previous code will fail if you implement `IEmailSender` and send a plain text email.</span></span> <span data-ttu-id="4e5d3-199">Bkz: [bu sorunu](https://github.com/aspnet/Home/issues/2152) daha fazla bilgi ve geçici bir çözüm için.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-199">See [this issue](https://github.com/aspnet/Home/issues/2152) for more information and a workaround.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e5d3-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e5d3-201">Hesap doğrulama etkinleştirmek için kodun açıklamasını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-201">Uncomment the code to enable account confirmation.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="4e5d3-202">Not: Biz de kullanıcı yeni kayıtlı otomatik olarak aşağıdaki satırını yorum oluşturma oturum açmış engelleyen:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-202">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="4e5d3-203">Kodda uncommenting tarafından parola kurtarmayı etkinleştirme `ForgotPassword` eylemde *Controllers/AccountController.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="4e5d3-204">Form öğesinde açıklamadan çıkarın *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="4e5d3-205">Kaldırmak istediğiniz `<p> For more information on how to enable reset password ... </p>` bu makaleye bir bağlantı içeren öğe.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="4e5d3-206">Kayıt, e-posta onaylayın ve parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="4e5d3-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="4e5d3-207">Web uygulaması çalıştırın ve hesap ve parola kurtarma akışı sınayın.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="4e5d3-208">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="4e5d3-208">Run the app and register a new user</span></span>

 ![Web uygulaması hesabını kaydetmek görünümü](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="4e5d3-210">Hesap onay bağlantısı için e-postanızı kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="4e5d3-211">Bkz: [Debug e-posta](#debug) e-posta alamazsanız.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="4e5d3-212">E-postanızı onaylamak için bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="4e5d3-213">E-posta ve parola ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-213">Log in with your email and password.</span></span>
* <span data-ttu-id="4e5d3-214">Oturumunuzu kapatın.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="4e5d3-215">Yönet sayfasını görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="4e5d3-215">View the manage page</span></span>

<span data-ttu-id="4e5d3-216">Kullanıcı adınızı tarayıcıda seçin: ![kullanıcı adıyla bir tarayıcı penceresi](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="4e5d3-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="4e5d3-217">Kullanıcı adı görmek için gezinti çubuğu genişletmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-217">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e5d3-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4e5d3-220">Yönet sayfası görüntülenir **profil** sekmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="4e5d3-221">**E-posta** e-posta belirten bir onay kutusu onaylanıp gösterir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-221">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![Yönet sayfası](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e5d3-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e5d3-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e5d3-224">Biz, öğreticide daha sonra bu sayfa hakkında konuşun.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-224">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="4e5d3-225">![Yönet sayfası](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="4e5d3-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="4e5d3-226">Test parola sıfırlama</span><span class="sxs-lookup"><span data-stu-id="4e5d3-226">Test password reset</span></span>

* <span data-ttu-id="4e5d3-227">Oturum açtınız, seçin **oturum kapatma**.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-227">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="4e5d3-228">Seçin **oturum** seçin ve bağlama **parolanızı mı unuttunuz?** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="4e5d3-229">Hesabını kaydetmek için kullanılan e-posta girin.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="4e5d3-230">Parolanızı sıfırlamak için bir bağlantı içeren bir e-posta gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-230">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="4e5d3-231">E-postanızı kontrol edin ve parolanızı sıfırlamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-231">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="4e5d3-232">Parolanızı başarıyla sıfırladıktan sonra e-posta ve yeni bir parola ile oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-232">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="4e5d3-233">E-posta hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="4e5d3-233">Debug email</span></span>

<span data-ttu-id="4e5d3-234">E-posta çalışma alınamıyor ise:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-234">If you can't get email working:</span></span>

* <span data-ttu-id="4e5d3-235">Gözden geçirme [e-posta etkinliğinin](https://sendgrid.com/docs/User_Guide/email_activity.html) sayfası.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-235">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="4e5d3-236">İstenmeyen posta klasörünüzü kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-236">Check your spam folder.</span></span>
* <span data-ttu-id="4e5d3-237">Farklı bir e-posta sağlayıcısı (Microsoft, Yahoo, Gmail, vb.) başka bir e-posta diğer deneyin</span><span class="sxs-lookup"><span data-stu-id="4e5d3-237">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="4e5d3-238">Oluşturma bir [e-posta göndermek için konsol uygulaması](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="4e5d3-238">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="4e5d3-239">Farklı bir e-posta hesaplarına göndermeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="4e5d3-240">**Not:** en iyi güvenlik uygulaması test ve geliştirme üretim parolalarında kullanmamaktır.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-240">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="4e5d3-241">Uygulamayı Azure'a yayımlama, uygulama ayarları Azure Web uygulaması portal olarak SendGrid parolaları ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="4e5d3-242">Ortam değişkenlerinin anahtarları okumak için kurulum yapılandırma sistemidir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-242">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="4e5d3-243">Kayıt sırasında oturum açma engelle</span><span class="sxs-lookup"><span data-stu-id="4e5d3-243">Prevent login at registration</span></span>

<span data-ttu-id="4e5d3-244">Bir kullanıcı kayıt formunu tamamladıktan sonra geçerli şablonlarıyla bunlar oturum açtınız (kimliği doğrulanmış).</span><span class="sxs-lookup"><span data-stu-id="4e5d3-244">With the current templates, once a user completes the registration form, they're logged in (authenticated).</span></span> <span data-ttu-id="4e5d3-245">Genellikle, oturum açmayı önce e-postalarına doğrulamak istersiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-245">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="4e5d3-246">Aşağıdaki bölümde, biz gerektirecek şekilde kodu değiştirecek yeni kullanıcınız bunlar oturum açtınız önce onaylanan bir e-posta.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-246">In the section below, we will modify the code to require new users have a confirmed email before they're logged in.</span></span> <span data-ttu-id="4e5d3-247">Güncelleştirme `[HttpPost] Login` eylemde *AccountController.cs* aşağıdaki vurgulanan değişikliklerle dosya.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-247">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

<span data-ttu-id="4e5d3-248">**Not:** en iyi güvenlik uygulaması test ve geliştirme üretim parolalarında kullanmamaktır.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-248">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="4e5d3-249">Uygulamayı Azure'a yayımlama, uygulama ayarları Azure Web uygulaması portal olarak SendGrid parolaları ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-249">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="4e5d3-250">Ortam değişkenlerinin anahtarları okumak için kurulum yapılandırma sistemidir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-250">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="4e5d3-251">Sosyal ve yerel oturum açma hesaplarını birleştirmek</span><span class="sxs-lookup"><span data-stu-id="4e5d3-251">Combine social and local login accounts</span></span>

<span data-ttu-id="4e5d3-252">Not: Bu bölüm, yalnızca ASP.NET Core geçerlidir 1.x.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-252">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="4e5d3-253">ASP.NET 2.x çekirdek bkz [bu](https://github.com/aspnet/Docs/issues/3753) sorun.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-253">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="4e5d3-254">Bu bölümde tamamlamak için önce bir dış kimlik doğrulama sağlayıcısı etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-254">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="4e5d3-255">Bkz: [Facebook, Google ve diğer dış sağlayıcılarını kullanarak kimlik doğrulamasını etkinleştirme](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="4e5d3-255">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="4e5d3-256">Yerel ve sosyal hesapları, e-posta bağlantısını tıklatarak birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-256">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="4e5d3-257">Aşağıdaki sırayla "RickAndMSFT@gmail.com" ilk bir yerel oturum açma; oluşturulur ancak, ilk sosyal bir oturum açma hesabı oluşturmak sonra yerel oturum açma ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-257">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web uygulaması: RickAndMSFT@gmail.com kimliği doğrulanmış kullanıcı](accconfirm/_static/rick.png)

<span data-ttu-id="4e5d3-259">Tıklayın **Yönet** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-259">Click on the **Manage** link.</span></span> <span data-ttu-id="4e5d3-260">Bu hesapla ilişkilendirilen 0 dış (sosyal oturum açma bilgileri) unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-260">Note the 0 external (social logins) associated with this account.</span></span>

![Görünüm yönetme](accconfirm/_static/manage.png)

<span data-ttu-id="4e5d3-262">Başka bir oturum açma hizmeti bağlantısını tıklatın ve uygulama isteklerini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-262">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="4e5d3-263">Aşağıdaki resimde Facebook Dış kimlik doğrulama sağlayıcısı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-263">In the image below, Facebook is the external authentication provider:</span></span>

![Harici oturum görünümünüzü Facebook listeleme yönetme](accconfirm/_static/fb.png)

<span data-ttu-id="4e5d3-265">İki hesap birleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-265">The two accounts have been combined.</span></span> <span data-ttu-id="4e5d3-266">Herhangi bir hesabı ile oturum açabilecek olacaktır.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-266">You will be able to log on with either account.</span></span> <span data-ttu-id="4e5d3-267">Yerel hesaplar durumunda kullanıcının sosyal oturum açtığında kimlik doğrulama hizmeti çalışmıyor ya da erişim sosyal hesaplarında daha büyük bir olasılıkla kaybettiğini eklemek için kullanıcılarınızın isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-267">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they've lost access to their social account.</span></span>
