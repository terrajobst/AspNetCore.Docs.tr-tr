---
title: ASP.NET core'da kimliğe giriş
author: rick-anderson
description: Kimlik ile bir ASP.NET Core uygulaması kullanın. Parola gereksinimlerini (RequireDigit, RequiredLength, RequiredUniqueChars ve daha fazlası) ayarlama konusunda bilgi edinin.
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 96f446ad9ec1ef5d807a8648e68308ee20583365
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040034"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="fb7e1-104">ASP.NET core'da kimliğe giriş</span><span class="sxs-lookup"><span data-stu-id="fb7e1-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="fb7e1-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fb7e1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fb7e1-106">ASP.NET Core, ASP.NET Core uygulamaları için oturum açma işlevselliğini ekleyen bir üyelik sistemi kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="fb7e1-107">Kullanıcılar, bir hesap kimliğinde depolanan oturum açma bilgileri oluşturabilir veya bir dış oturum açma sağlayıcısı kullanabilirler.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="fb7e1-108">Desteklenen dış oturum açma sağlayıcılarını içerir [Facebook, Google, Microsoft Account ve Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="fb7e1-109">Kimlik, kullanıcı adları, parolalar ve profil verilerini depolamak için bir SQL Server veritabanı kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="fb7e1-110">Alternatif olarak, başka bir kalıcı depolama, Azure tablo depolama örneği için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="fb7e1-111">Görüntüleyebilir veya örnek kodunu indirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="fb7e1-112">(Karşıdan yükleme)</span><span class="sxs-lookup"><span data-stu-id="fb7e1-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="fb7e1-113">Bu konuda, kimlik kaydolun, oturum açma için nasıl kullanacağınızı öğrenin ve bir kullanıcının oturumunu oturum.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="fb7e1-114">Kimlik kullanan uygulamalar oluşturma hakkında daha ayrıntılı yönergeler için bu makalenin sonunda sonraki adımlar bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="fb7e1-115">Kimlik doğrulama ile bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="fb7e1-115">Create a Web app with authentication</span></span>

<span data-ttu-id="fb7e1-116">Bireysel kullanıcı hesapları ile bir ASP.NET Core Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb7e1-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb7e1-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fb7e1-118">Seçin **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-118">Select **File** > **New** > **Project**.</span></span> 
* <span data-ttu-id="fb7e1-119">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="fb7e1-120">Projeyi adlandırın **WebApp1** proje indirme ile aynı ad alanına sahip.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-120">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="fb7e1-121">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-121">Click **OK**.</span></span>
* <span data-ttu-id="fb7e1-122">Bir ASP.NET Core seçin **Web uygulaması** ASP.NET Core 2.1 için seçip **kimlik doğrulamayı Değiştir**.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-122">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="fb7e1-123">Seçin **bireysel kullanıcı hesapları** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-123">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fb7e1-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fb7e1-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="fb7e1-125">Oluşturulan proje sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-125">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="fb7e1-126">Test kayıt ve oturum açma</span><span class="sxs-lookup"><span data-stu-id="fb7e1-126">Test Register and Login</span></span>

<span data-ttu-id="fb7e1-127">Uygulamayı çalıştırın ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-127">Run the app and register a user.</span></span> <span data-ttu-id="fb7e1-128">Ekran boyutu bağlı olarak, görmek için gezinti düğmesini seçmeniz gerekebilir **kaydetme** ve **oturum açma** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-128">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![Geçiş gezinti düğmesi](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="fb7e1-130">Kimlik Hizmetleri Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fb7e1-130">Configure Identity services</span></span>

<span data-ttu-id="fb7e1-131">Hizmetleri eklenir `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-131">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="fb7e1-132">Tipik bir düzen tüm çağırmaktır `Add{Service}` yöntemleri ve sonra çağrı tüm `services.Configure{Service}` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-132">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="fb7e1-133">Aşağıdaki kod, oluşturulan şablonu içermez `CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="fb7e1-133">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="fb7e1-134">Yukarıdaki kod, varsayılan seçenek değerleriyle kimlik yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-134">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="fb7e1-135">Hizmetleri uygulama üzerinden için kullanılabilir yapılır [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-135">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="fb7e1-136">Kimliği çağırarak etkinleştirildiğinde [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-136">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="fb7e1-137">`UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-137">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="fb7e1-138">Hizmetleri üzerinden uygulama için kullanılabilir yapılır [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-138">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="fb7e1-139">Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseAuthentication` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-139">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="fb7e1-140">`UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-140">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="fb7e1-141">Bu hizmetler uygulamaya sunulur [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-141">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="fb7e1-142">Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseIdentity` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-142">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="fb7e1-143">`UseIdentity` tanımlama bilgisi tabanlı kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-143">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="fb7e1-144">Daha fazla bilgi için [IdentityOptions sınıfı](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) ve [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-144">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="fb7e1-145">İskele kaydı, oturum açma ve oturum kapatma</span><span class="sxs-lookup"><span data-stu-id="fb7e1-145">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="fb7e1-146">İzleyin [yetkilendirmesiyle Razor projesine kimlik iskelesini](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-146">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb7e1-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb7e1-147">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fb7e1-148">Kaydı, oturum açma ve kapatmanın dosyaları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-148">Add the Register, Login, and LogOut files.</span></span>


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fb7e1-149">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fb7e1-149">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fb7e1-150">Proje adı ile oluşturduysanız **WebApp1**, aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-150">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="fb7e1-151">Aksi takdirde, doğru ad alanını kullanmak `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="fb7e1-151">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

<span data-ttu-id="fb7e1-152">PowerShell komut ayırıcı olarak virgül kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-152">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="fb7e1-153">PowerShell kullanarak, dosya listesi noktalı kaçış veya dosya listesi yukarıdaki örnekte gösterildiği gibi çift tırnak işareti koyun.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-153">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="fb7e1-154">Kayıt inceleyin</span><span class="sxs-lookup"><span data-stu-id="fb7e1-154">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="fb7e1-155">Kullanıcı tıkladığında **kaydetme** bağlantı `RegisterModel.OnPostAsync` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-155">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="fb7e1-156">Kullanıcı tarafından oluşturulan [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) üzerinde `_userManager` nesne.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-156">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="fb7e1-157">`_userManager` bağımlılık ekleme tarafından sağlanır):</span><span class="sxs-lookup"><span data-stu-id="fb7e1-157">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="fb7e1-158">Kullanıcı tıkladığında **kaydetme** bağlantı `Register` eylem üzerinde çağrıldığında `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-158">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="fb7e1-159">`Register` Eylem çağırarak kullanıcı oluşturur `CreateAsync` üzerinde `_userManager` nesne (sağlanan `AccountController` bağımlılık ekleme göre):</span><span class="sxs-lookup"><span data-stu-id="fb7e1-159">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="fb7e1-160">Kullanıcı başarıyla oluşturulmuş olsa bile, kullanıcı çağırarak oturum `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-160">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="fb7e1-161">**Not:** bkz [hesap onayı](xref:security/authentication/accconfirm#prevent-login-at-registration) kayıt sırasında hemen oturum açma önlemek adımlar.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-161">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="fb7e1-162">Oturum aç</span><span class="sxs-lookup"><span data-stu-id="fb7e1-162">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="fb7e1-163">Oturum açma formu görüntülenir olduğunda:</span><span class="sxs-lookup"><span data-stu-id="fb7e1-163">The Login form is displayed when:</span></span>

* <span data-ttu-id="fb7e1-164">**Oturum** bağlantı seçildiğinde.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-164">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="fb7e1-165">Ne zaman bir kullanıcının eriştiği nerede bunlar kimliği doğrulanmamış bir sayfa **veya** yetkili ve bunlar için oturum açma sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-165">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span> 

<span data-ttu-id="fb7e1-166">Oturum açma sayfasındaki formu gönderildiğinde `OnPostAsync` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-166">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="fb7e1-167">`PasswordSignInAsync` üzerinde çağrılır `_signInManager` (bağımlılık ekleme tarafından sağlanan) nesne.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-167">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="fb7e1-168">Temel `Controller` sınıfı kullanıma sunan bir `User` denetleyici yöntemleri erişebileceğiniz özelliği.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-168">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="fb7e1-169">Örneğin, numaralandırma `User.Claims` ve yetkilendirme kararları.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-169">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="fb7e1-170">Daha fazla bilgi için [yetkilendirme](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-170">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="fb7e1-171">Kullanıcılar oturum açma formu görüntülenir **oturum** bağlantı veya erişimi kimlik doğrulaması gerektiren bir sayfaya yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-171">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="fb7e1-172">Kullanıcı oturum açma sayfasındaki formu gönderdiğinde `AccountController` `Login` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-172">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="fb7e1-173">`Login` Eylem çağrıları `PasswordSignInAsync` üzerinde `_signInManager` nesne (sağlanan `AccountController` bağımlılık ekleme tarafından).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-173">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="fb7e1-174">Temel (`Controller` veya `PageModel`) sınıfı kullanıma sunan bir `User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-174">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="fb7e1-175">Örneğin, `User.Claims` yetkilendirme kararları vermek için listelenebilir.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-175">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="fb7e1-176">Oturumu Kapat</span><span class="sxs-lookup"><span data-stu-id="fb7e1-176">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="fb7e1-177">**Oturumunuzu** bağlantı çağırır `LogoutModel.OnPost` eylem.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-177">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="fb7e1-178">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) depolanan bir tanımlama bilgisinde kullanıcının talepleri temizler.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-178">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="fb7e1-179">Çağırdıktan sonra yeniden yönlendirme yoksa `SignOutAsync` veya kullanıcının **değil** oturumunuz.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-179">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="fb7e1-180">POST belirtilen *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb7e1-180">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"
   <span data-ttu-id="fb7e1-181">Tıklayarak **oturumunuzu** bağlama çağrıları `LogOut` eylem.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-181">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="fb7e1-182">Önceki kod çağrıları `_signInManager.SignOutAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-182">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="fb7e1-183">`SignOutAsync` Yöntemi, kullanıcının talepleri depolanan bir tanımlama bilgisinde temizler.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-183">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="fb7e1-184">Test kimliği</span><span class="sxs-lookup"><span data-stu-id="fb7e1-184">Test Identity</span></span>

<span data-ttu-id="fb7e1-185">Varsayılan web projesi şablonları giriş sayfaları anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-185">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="fb7e1-186">Kimliğini test etmek için ekleme [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) hakkında sayfası.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-186">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="fb7e1-187">Açmış durumdaysanız, oturumu kapatın. Uygulamayı çalıştırmak ve seçmek **hakkında** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-187">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="fb7e1-188">Oturum açma sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-188">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="fb7e1-189">Kimlik keşfedin</span><span class="sxs-lookup"><span data-stu-id="fb7e1-189">Explore Identity</span></span>

<span data-ttu-id="fb7e1-190">Kimlik daha ayrıntılı olarak keşfetmek için:</span><span class="sxs-lookup"><span data-stu-id="fb7e1-190">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="fb7e1-191">Tam kimlik UI kaynağı oluşturma</span><span class="sxs-lookup"><span data-stu-id="fb7e1-191">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="fb7e1-192">Her bir sayfasını ve hata ayıklayıcı ile adım kaynağını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-192">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="fb7e1-193">Kimlik bileşenleri</span><span class="sxs-lookup"><span data-stu-id="fb7e1-193">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="fb7e1-194">Tüm kimlik bağlı NuGet paketleri dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-194">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
::: moniker-end

<span data-ttu-id="fb7e1-195">Birincil paket kimliği için [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-195">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="fb7e1-196">Bu paket, ASP.NET Core kimliği için arabirimleri çekirdek kümesini içerir ve tarafından eklenen `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="fb7e1-197">ASP.NET Core Identity'ye geçirme</span><span class="sxs-lookup"><span data-stu-id="fb7e1-197">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="fb7e1-198">Daha fazla bilgi ve varolan bir kimlik deposunu geçirme hakkında Yardım almak için bkz. [geçirme kimlik doğrulaması ve kimlik](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-198">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="fb7e1-199">Parola gücünü ayarlama</span><span class="sxs-lookup"><span data-stu-id="fb7e1-199">Setting password strength</span></span>

<span data-ttu-id="fb7e1-200">Bkz: [yapılandırma](#pw) en düşük Parola gereksinimlerinin ayarlayan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="fb7e1-200">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb7e1-201">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="fb7e1-201">Next Steps</span></span>

* [<span data-ttu-id="fb7e1-202">Kimliği Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fb7e1-202">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <span data-ttu-id="fb7e1-203">[Kimlik birincil anahtar veri türü yapılandırma](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="fb7e1-203">[Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
