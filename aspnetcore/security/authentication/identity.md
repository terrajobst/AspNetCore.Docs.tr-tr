---
title: ASP.NET core'da kimliğe giriş
author: rick-anderson
description: Kimlik ile bir ASP.NET Core uygulaması kullanın. Parola gereksinimlerini (RequireDigit, RequiredLength, RequiredUniqueChars ve daha fazlası) ayarlama konusunda bilgi edinin.
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 1a4e7fb3ac6a767ca17127dd58a9b9e65ed9a00b
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "55739689"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="26f30-104">ASP.NET core'da kimliğe giriş</span><span class="sxs-lookup"><span data-stu-id="26f30-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="26f30-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="26f30-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="26f30-106">ASP.NET Core, ASP.NET Core uygulamaları için oturum açma işlevselliğini ekleyen bir üyelik sistemi kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="26f30-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="26f30-107">Kullanıcılar, bir hesap kimliğinde depolanan oturum açma bilgileri oluşturabilir veya bir dış oturum açma sağlayıcısı kullanabilirler.</span><span class="sxs-lookup"><span data-stu-id="26f30-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="26f30-108">Desteklenen dış oturum açma sağlayıcılarını içerir [Facebook, Google, Microsoft Account ve Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="26f30-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="26f30-109">Kimlik, kullanıcı adları, parolalar ve profil verilerini depolamak için bir SQL Server veritabanı kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="26f30-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="26f30-110">Alternatif olarak, başka bir kalıcı depolama, Azure tablo depolama örneği için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="26f30-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="26f30-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([nasıl indirileceğini)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="26f30-111">[View or download the sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="26f30-112">Bu konuda, kimlik kaydolun, oturum açma için nasıl kullanacağınızı öğrenin ve bir kullanıcının oturumunu oturum.</span><span class="sxs-lookup"><span data-stu-id="26f30-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="26f30-113">Kimlik kullanan uygulamalar oluşturma hakkında daha ayrıntılı yönergeler için bu makalenin sonunda sonraki adımlar bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="26f30-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="26f30-114">AddDefaultIdentity ve AddIdentity</span><span class="sxs-lookup"><span data-stu-id="26f30-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="26f30-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) ASP.NET Core 2.1 içinde kullanılmaya başlandı.</span><span class="sxs-lookup"><span data-stu-id="26f30-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="26f30-116">Çağırma `AddDefaultIdentity` aşağıdaki çağırmak için benzer:</span><span class="sxs-lookup"><span data-stu-id="26f30-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="26f30-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="26f30-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="26f30-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="26f30-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="26f30-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="26f30-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="26f30-120">Bkz: [AddDefaultIdentity kaynak](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="26f30-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="26f30-121">Kimlik doğrulama ile bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="26f30-121">Create a Web app with authentication</span></span>

<span data-ttu-id="26f30-122">Bireysel kullanıcı hesapları ile bir ASP.NET Core Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="26f30-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="26f30-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26f30-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="26f30-124">**Dosya** > **Yeni** > **Proje**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="26f30-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="26f30-125">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="26f30-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="26f30-126">Projeyi adlandırın **WebApp1** proje indirme ile aynı ad alanına sahip.</span><span class="sxs-lookup"><span data-stu-id="26f30-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="26f30-127">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="26f30-127">Click **OK**.</span></span>
* <span data-ttu-id="26f30-128">Bir ASP.NET Core seçin **Web uygulaması**, ardından **kimlik doğrulamayı Değiştir**.</span><span class="sxs-lookup"><span data-stu-id="26f30-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="26f30-129">Seçin **bireysel kullanıcı hesapları** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="26f30-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="26f30-130">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="26f30-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="26f30-131">Oluşturulan proje sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="26f30-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="26f30-132">Uç noktaları ile kimlik Razor sınıf kitaplığı sunan `Identity` alan.</span><span class="sxs-lookup"><span data-stu-id="26f30-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="26f30-133">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="26f30-133">For example:</span></span>

* <span data-ttu-id="26f30-134">/ Kimlik/hesabı/oturum açma</span><span class="sxs-lookup"><span data-stu-id="26f30-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="26f30-135">/ Kimlik/hesabı/oturum kapatma</span><span class="sxs-lookup"><span data-stu-id="26f30-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="26f30-136">/ Kimlik/hesabı/yönetme</span><span class="sxs-lookup"><span data-stu-id="26f30-136">/Identity/Account/Manage</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="26f30-137">Test kayıt ve oturum açma</span><span class="sxs-lookup"><span data-stu-id="26f30-137">Test Register and Login</span></span>

<span data-ttu-id="26f30-138">Uygulamayı çalıştırın ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="26f30-138">Run the app and register a user.</span></span> <span data-ttu-id="26f30-139">Ekran boyutu bağlı olarak, görmek için gezinti düğmesini seçmeniz gerekebilir **kaydetme** ve **oturum açma** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="26f30-139">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="26f30-140">Kimlik Hizmetleri Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="26f30-140">Configure Identity services</span></span>

<span data-ttu-id="26f30-141">Hizmetleri eklenir `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="26f30-141">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="26f30-142">Tipik bir düzen tüm çağırmaktır `Add{Service}` yöntemleri ve sonra çağrı tüm `services.Configure{Service}` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="26f30-142">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="26f30-143">Yukarıdaki kod, varsayılan seçenek değerleriyle kimlik yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="26f30-143">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="26f30-144">Hizmetleri uygulama üzerinden için kullanılabilir yapılır [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="26f30-144">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="26f30-145">Kimliği çağırarak etkinleştirildiğinde [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="26f30-145">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="26f30-146">`UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="26f30-146">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="26f30-147">Hizmetleri üzerinden uygulama için kullanılabilir yapılır [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="26f30-147">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="26f30-148">Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseAuthentication` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="26f30-148">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="26f30-149">`UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="26f30-149">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="26f30-150">Bu hizmetler uygulamaya sunulur [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="26f30-150">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="26f30-151">Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseIdentity` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="26f30-151">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="26f30-152">`UseIdentity` tanımlama bilgisi tabanlı kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="26f30-152">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="26f30-153">Daha fazla bilgi için [IdentityOptions sınıfı](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) ve [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="26f30-153">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="26f30-154">İskele kaydı, oturum açma ve oturum kapatma</span><span class="sxs-lookup"><span data-stu-id="26f30-154">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="26f30-155">İzleyin [yetkilendirmesiyle Razor projesine kimlik iskelesini](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) oluşturmak için yönergeler bu bölümde gösterilen kodu.</span><span class="sxs-lookup"><span data-stu-id="26f30-155">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="26f30-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26f30-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="26f30-157">Kaydı, oturum açma ve kapatmanın dosyaları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="26f30-157">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="26f30-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="26f30-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="26f30-159">Proje adı ile oluşturduysanız **WebApp1**, aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="26f30-159">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="26f30-160">Aksi takdirde, doğru ad alanını kullanmak `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="26f30-160">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="26f30-161">PowerShell komut ayırıcı olarak virgül kullanır.</span><span class="sxs-lookup"><span data-stu-id="26f30-161">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="26f30-162">PowerShell kullanarak, dosya listesi noktalı kaçış veya dosya listesi yukarıdaki örnekte gösterildiği gibi çift tırnak işareti koyun.</span><span class="sxs-lookup"><span data-stu-id="26f30-162">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="26f30-163">Kayıt inceleyin</span><span class="sxs-lookup"><span data-stu-id="26f30-163">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="26f30-164">Kullanıcı tıkladığında **kaydetme** bağlantı `RegisterModel.OnPostAsync` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="26f30-164">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="26f30-165">Kullanıcı tarafından oluşturulan [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) üzerinde `_userManager` nesne.</span><span class="sxs-lookup"><span data-stu-id="26f30-165">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="26f30-166">`_userManager` bağımlılık ekleme tarafından sağlanır):</span><span class="sxs-lookup"><span data-stu-id="26f30-166">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="26f30-167">Kullanıcı tıkladığında **kaydetme** bağlantı `Register` eylem üzerinde çağrıldığında `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="26f30-167">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="26f30-168">`Register` Eylem çağırarak kullanıcı oluşturur `CreateAsync` üzerinde `_userManager` nesne (sağlanan `AccountController` bağımlılık ekleme göre):</span><span class="sxs-lookup"><span data-stu-id="26f30-168">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="26f30-169">Kullanıcı başarıyla oluşturulmuş olsa bile, kullanıcı çağırarak oturum `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="26f30-169">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="26f30-170">**Not:** Bkz: [hesap onayı](xref:security/authentication/accconfirm#prevent-login-at-registration) adımları hemen oturum açma işleminde kayıt önlemek için.</span><span class="sxs-lookup"><span data-stu-id="26f30-170">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="26f30-171">Oturum aç</span><span class="sxs-lookup"><span data-stu-id="26f30-171">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="26f30-172">Oturum açma formu görüntülenir olduğunda:</span><span class="sxs-lookup"><span data-stu-id="26f30-172">The Login form is displayed when:</span></span>

* <span data-ttu-id="26f30-173">**Oturum** bağlantı seçildiğinde.</span><span class="sxs-lookup"><span data-stu-id="26f30-173">The **Log in** link is selected.</span></span>
* <span data-ttu-id="26f30-174">Bir kullanıcı yetkiniz yok kısıtlanmış bir sayfaya erişmeye erişim **veya** zaman henüz doğrulandığını sistem tarafından.</span><span class="sxs-lookup"><span data-stu-id="26f30-174">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="26f30-175">Oturum açma sayfasındaki formu gönderildiğinde `OnPostAsync` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="26f30-175">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="26f30-176">`PasswordSignInAsync` üzerinde çağrılır `_signInManager` (bağımlılık ekleme tarafından sağlanan) nesne.</span><span class="sxs-lookup"><span data-stu-id="26f30-176">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="26f30-177">Temel `Controller` sınıfı kullanıma sunan bir `User` denetleyici yöntemleri erişebileceğiniz özelliği.</span><span class="sxs-lookup"><span data-stu-id="26f30-177">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="26f30-178">Örneğin, numaralandırma `User.Claims` ve yetkilendirme kararları.</span><span class="sxs-lookup"><span data-stu-id="26f30-178">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="26f30-179">Daha fazla bilgi için bkz. <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="26f30-179">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="26f30-180">Kullanıcılar oturum açma formu görüntülenir **oturum** bağlantı veya erişimi kimlik doğrulaması gerektiren bir sayfaya yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="26f30-180">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="26f30-181">Kullanıcı oturum açma sayfasındaki formu gönderdiğinde `AccountController` `Login` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="26f30-181">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="26f30-182">`Login` Eylem çağrıları `PasswordSignInAsync` üzerinde `_signInManager` nesne (sağlanan `AccountController` bağımlılık ekleme tarafından).</span><span class="sxs-lookup"><span data-stu-id="26f30-182">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="26f30-183">Temel (`Controller` veya `PageModel`) sınıfı kullanıma sunan bir `User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="26f30-183">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="26f30-184">Örneğin, `User.Claims` yetkilendirme kararları vermek için listelenebilir.</span><span class="sxs-lookup"><span data-stu-id="26f30-184">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="26f30-185">Oturumu kapat</span><span class="sxs-lookup"><span data-stu-id="26f30-185">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="26f30-186">**Oturumunuzu** bağlantı çağırır `LogoutModel.OnPost` eylem.</span><span class="sxs-lookup"><span data-stu-id="26f30-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="26f30-187">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) depolanan bir tanımlama bilgisinde kullanıcının talepleri temizler.</span><span class="sxs-lookup"><span data-stu-id="26f30-187">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="26f30-188">Çağırdıktan sonra yeniden yönlendirme yoksa `SignOutAsync` veya kullanıcının **değil** oturumunuz.</span><span class="sxs-lookup"><span data-stu-id="26f30-188">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="26f30-189">POST belirtilen *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="26f30-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="26f30-190">Tıklayarak **oturumunuzu** bağlama çağrıları `LogOut` eylem.</span><span class="sxs-lookup"><span data-stu-id="26f30-190">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="26f30-191">Önceki kod çağrıları `_signInManager.SignOutAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="26f30-191">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="26f30-192">`SignOutAsync` Yöntemi, kullanıcının talepleri depolanan bir tanımlama bilgisinde temizler.</span><span class="sxs-lookup"><span data-stu-id="26f30-192">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="26f30-193">Test kimliği</span><span class="sxs-lookup"><span data-stu-id="26f30-193">Test Identity</span></span>

<span data-ttu-id="26f30-194">Varsayılan web projesi şablonları giriş sayfaları anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="26f30-194">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="26f30-195">Kimliğini test etmek için ekleme [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) gizlilik sayfasına.</span><span class="sxs-lookup"><span data-stu-id="26f30-195">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="26f30-196">Açmış durumdaysanız, oturumu kapatın. Uygulamayı çalıştırmak ve seçmek **gizlilik** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="26f30-196">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="26f30-197">Oturum açma sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="26f30-197">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="26f30-198">Kimlik keşfedin</span><span class="sxs-lookup"><span data-stu-id="26f30-198">Explore Identity</span></span>

<span data-ttu-id="26f30-199">Kimlik daha ayrıntılı olarak keşfetmek için:</span><span class="sxs-lookup"><span data-stu-id="26f30-199">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="26f30-200">Tam kimlik UI kaynağı oluşturma</span><span class="sxs-lookup"><span data-stu-id="26f30-200">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="26f30-201">Her bir sayfasını ve hata ayıklayıcı ile adım kaynağını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="26f30-201">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="26f30-202">Kimlik bileşenleri</span><span class="sxs-lookup"><span data-stu-id="26f30-202">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="26f30-203">Tüm kimlik bağlı NuGet paketleri dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="26f30-203">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="26f30-204">Birincil paket kimliği için [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="26f30-204">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="26f30-205">Bu paket, ASP.NET Core kimliği için arabirimleri çekirdek kümesini içerir ve tarafından eklenen `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="26f30-205">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="26f30-206">ASP.NET Core Identity'ye geçirme</span><span class="sxs-lookup"><span data-stu-id="26f30-206">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="26f30-207">Daha fazla bilgi ve varolan bir kimlik deposunu geçirme hakkında Yardım almak için bkz. [geçirme kimlik doğrulaması ve kimlik](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="26f30-207">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="26f30-208">Parola gücünü ayarlama</span><span class="sxs-lookup"><span data-stu-id="26f30-208">Setting password strength</span></span>

<span data-ttu-id="26f30-209">Bkz: [yapılandırma](#pw) en düşük Parola gereksinimlerinin ayarlayan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="26f30-209">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="26f30-210">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="26f30-210">Next Steps</span></span>

* [<span data-ttu-id="26f30-211">Kimliği Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="26f30-211">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
