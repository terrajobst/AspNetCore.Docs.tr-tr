---
title: ASP.NET core'da kimliğe giriş
author: rick-anderson
description: Kimlik ile bir ASP.NET Core uygulaması kullanın. Parola gereksinimlerini (RequireDigit, RequiredLength, RequiredUniqueChars ve daha fazlası) ayarlama konusunda bilgi edinin.
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 03f89114b516a37ee1d06934f2e549b4d56ff099
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098775"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="c54b9-104">ASP.NET core'da kimliğe giriş</span><span class="sxs-lookup"><span data-stu-id="c54b9-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="c54b9-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c54b9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c54b9-106">ASP.NET Core, ASP.NET Core uygulamaları için oturum açma işlevselliğini ekleyen bir üyelik sistemi kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="c54b9-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="c54b9-107">Kullanıcılar, bir hesap kimliğinde depolanan oturum açma bilgileri oluşturabilir veya bir dış oturum açma sağlayıcısı kullanabilirler.</span><span class="sxs-lookup"><span data-stu-id="c54b9-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="c54b9-108">Desteklenen dış oturum açma sağlayıcılarını içerir [Facebook, Google, Microsoft Account ve Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c54b9-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="c54b9-109">Kimlik, kullanıcı adları, parolalar ve profil verilerini depolamak için bir SQL Server veritabanı kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="c54b9-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="c54b9-110">Alternatif olarak, başka bir kalıcı depolama, Azure tablo depolama örneği için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c54b9-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="c54b9-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([nasıl indirileceğini)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c54b9-111">[View or download the sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="c54b9-112">Bu konuda, kimlik kaydolun, oturum açma için nasıl kullanacağınızı öğrenin ve bir kullanıcının oturumunu oturum.</span><span class="sxs-lookup"><span data-stu-id="c54b9-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="c54b9-113">Kimlik kullanan uygulamalar oluşturma hakkında daha ayrıntılı yönergeler için bu makalenin sonunda sonraki adımlar bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="c54b9-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="c54b9-114">AddDefaultIdentity ve AddIdentity</span><span class="sxs-lookup"><span data-stu-id="c54b9-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="c54b9-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) ASP.NET Core 2.1 içinde kullanılmaya başlandı.</span><span class="sxs-lookup"><span data-stu-id="c54b9-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="c54b9-116">Çağırma `AddDefaultIdentity` aşağıdaki çağırmak için benzer:</span><span class="sxs-lookup"><span data-stu-id="c54b9-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="c54b9-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="c54b9-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="c54b9-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="c54b9-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="c54b9-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="c54b9-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="c54b9-120">Bkz: [AddDefaultIdentity kaynak](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c54b9-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="c54b9-121">Kimlik doğrulama ile bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="c54b9-121">Create a Web app with authentication</span></span>

<span data-ttu-id="c54b9-122">Bireysel kullanıcı hesapları ile bir ASP.NET Core Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c54b9-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c54b9-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c54b9-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c54b9-124">**Dosya** > **Yeni** > **Proje**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="c54b9-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="c54b9-125">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="c54b9-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="c54b9-126">Projeyi adlandırın **WebApp1** proje indirme ile aynı ad alanına sahip.</span><span class="sxs-lookup"><span data-stu-id="c54b9-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="c54b9-127">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="c54b9-127">Click **OK**.</span></span>
* <span data-ttu-id="c54b9-128">Bir ASP.NET Core seçin **Web uygulaması** ASP.NET Core 2.1 için seçip **kimlik doğrulamayı Değiştir**.</span><span class="sxs-lookup"><span data-stu-id="c54b9-128">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="c54b9-129">Seçin **bireysel kullanıcı hesapları** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="c54b9-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c54b9-130">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c54b9-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="c54b9-131">Oluşturulan proje sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="c54b9-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="c54b9-132">Test kayıt ve oturum açma</span><span class="sxs-lookup"><span data-stu-id="c54b9-132">Test Register and Login</span></span>

<span data-ttu-id="c54b9-133">Uygulamayı çalıştırın ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="c54b9-133">Run the app and register a user.</span></span> <span data-ttu-id="c54b9-134">Ekran boyutu bağlı olarak, görmek için gezinti düğmesini seçmeniz gerekebilir **kaydetme** ve **oturum açma** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="c54b9-134">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![Geçiş gezinti düğmesi](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="c54b9-136">Kimlik Hizmetleri Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c54b9-136">Configure Identity services</span></span>

<span data-ttu-id="c54b9-137">Hizmetleri eklenir `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c54b9-137">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="c54b9-138">Tipik bir düzen tüm çağırmaktır `Add{Service}` yöntemleri ve sonra çağrı tüm `services.Configure{Service}` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="c54b9-138">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="c54b9-139">Aşağıdaki kod, oluşturulan şablonu içermez `CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="c54b9-139">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="c54b9-140">Yukarıdaki kod, varsayılan seçenek değerleriyle kimlik yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c54b9-140">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="c54b9-141">Hizmetleri uygulama üzerinden için kullanılabilir yapılır [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c54b9-141">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="c54b9-142">Kimliği çağırarak etkinleştirildiğinde [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="c54b9-142">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="c54b9-143">`UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="c54b9-143">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="c54b9-144">Hizmetleri üzerinden uygulama için kullanılabilir yapılır [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c54b9-144">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="c54b9-145">Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseAuthentication` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c54b9-145">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="c54b9-146">`UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="c54b9-146">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="c54b9-147">Bu hizmetler uygulamaya sunulur [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c54b9-147">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="c54b9-148">Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseIdentity` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c54b9-148">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="c54b9-149">`UseIdentity` tanımlama bilgisi tabanlı kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="c54b9-149">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="c54b9-150">Daha fazla bilgi için [IdentityOptions sınıfı](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) ve [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="c54b9-150">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="c54b9-151">İskele kaydı, oturum açma ve oturum kapatma</span><span class="sxs-lookup"><span data-stu-id="c54b9-151">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="c54b9-152">İzleyin [yetkilendirmesiyle Razor projesine kimlik iskelesini](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) oluşturmak için yönergeler bu bölümde gösterilen kodu.</span><span class="sxs-lookup"><span data-stu-id="c54b9-152">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c54b9-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c54b9-153">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c54b9-154">Kaydı, oturum açma ve kapatmanın dosyaları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c54b9-154">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c54b9-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c54b9-155">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c54b9-156">Proje adı ile oluşturduysanız **WebApp1**, aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c54b9-156">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="c54b9-157">Aksi takdirde, doğru ad alanını kullanmak `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="c54b9-157">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="c54b9-158">PowerShell komut ayırıcı olarak virgül kullanır.</span><span class="sxs-lookup"><span data-stu-id="c54b9-158">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="c54b9-159">PowerShell kullanarak, dosya listesi noktalı kaçış veya dosya listesi yukarıdaki örnekte gösterildiği gibi çift tırnak işareti koyun.</span><span class="sxs-lookup"><span data-stu-id="c54b9-159">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="c54b9-160">Kayıt inceleyin</span><span class="sxs-lookup"><span data-stu-id="c54b9-160">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="c54b9-161">Kullanıcı tıkladığında **kaydetme** bağlantı `RegisterModel.OnPostAsync` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c54b9-161">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="c54b9-162">Kullanıcı tarafından oluşturulan [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) üzerinde `_userManager` nesne.</span><span class="sxs-lookup"><span data-stu-id="c54b9-162">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="c54b9-163">`_userManager` bağımlılık ekleme tarafından sağlanır):</span><span class="sxs-lookup"><span data-stu-id="c54b9-163">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="c54b9-164">Kullanıcı tıkladığında **kaydetme** bağlantı `Register` eylem üzerinde çağrıldığında `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="c54b9-164">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="c54b9-165">`Register` Eylem çağırarak kullanıcı oluşturur `CreateAsync` üzerinde `_userManager` nesne (sağlanan `AccountController` bağımlılık ekleme göre):</span><span class="sxs-lookup"><span data-stu-id="c54b9-165">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="c54b9-166">Kullanıcı başarıyla oluşturulmuş olsa bile, kullanıcı çağırarak oturum `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="c54b9-166">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="c54b9-167">**Not:** Bkz: [hesap onayı](xref:security/authentication/accconfirm#prevent-login-at-registration) adımları hemen oturum açma işleminde kayıt önlemek için.</span><span class="sxs-lookup"><span data-stu-id="c54b9-167">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="c54b9-168">Oturum aç</span><span class="sxs-lookup"><span data-stu-id="c54b9-168">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c54b9-169">Oturum açma formu görüntülenir olduğunda:</span><span class="sxs-lookup"><span data-stu-id="c54b9-169">The Login form is displayed when:</span></span>

* <span data-ttu-id="c54b9-170">**Oturum** bağlantı seçildiğinde.</span><span class="sxs-lookup"><span data-stu-id="c54b9-170">The **Log in** link is selected.</span></span>
* <span data-ttu-id="c54b9-171">Bir kullanıcı yetkiniz yok kısıtlanmış bir sayfaya erişmeye erişim **veya** zaman henüz doğrulandığını sistem tarafından.</span><span class="sxs-lookup"><span data-stu-id="c54b9-171">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="c54b9-172">Oturum açma sayfasındaki formu gönderildiğinde `OnPostAsync` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c54b9-172">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="c54b9-173">`PasswordSignInAsync` üzerinde çağrılır `_signInManager` (bağımlılık ekleme tarafından sağlanan) nesne.</span><span class="sxs-lookup"><span data-stu-id="c54b9-173">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="c54b9-174">Temel `Controller` sınıfı kullanıma sunan bir `User` denetleyici yöntemleri erişebileceğiniz özelliği.</span><span class="sxs-lookup"><span data-stu-id="c54b9-174">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="c54b9-175">Örneğin, numaralandırma `User.Claims` ve yetkilendirme kararları.</span><span class="sxs-lookup"><span data-stu-id="c54b9-175">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="c54b9-176">Daha fazla bilgi için bkz. <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="c54b9-176">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c54b9-177">Kullanıcılar oturum açma formu görüntülenir **oturum** bağlantı veya erişimi kimlik doğrulaması gerektiren bir sayfaya yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c54b9-177">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="c54b9-178">Kullanıcı oturum açma sayfasındaki formu gönderdiğinde `AccountController` `Login` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c54b9-178">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="c54b9-179">`Login` Eylem çağrıları `PasswordSignInAsync` üzerinde `_signInManager` nesne (sağlanan `AccountController` bağımlılık ekleme tarafından).</span><span class="sxs-lookup"><span data-stu-id="c54b9-179">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="c54b9-180">Temel (`Controller` veya `PageModel`) sınıfı kullanıma sunan bir `User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="c54b9-180">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="c54b9-181">Örneğin, `User.Claims` yetkilendirme kararları vermek için listelenebilir.</span><span class="sxs-lookup"><span data-stu-id="c54b9-181">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="c54b9-182">Oturumu kapat</span><span class="sxs-lookup"><span data-stu-id="c54b9-182">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c54b9-183">**Oturumunuzu** bağlantı çağırır `LogoutModel.OnPost` eylem.</span><span class="sxs-lookup"><span data-stu-id="c54b9-183">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="c54b9-184">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) depolanan bir tanımlama bilgisinde kullanıcının talepleri temizler.</span><span class="sxs-lookup"><span data-stu-id="c54b9-184">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="c54b9-185">Çağırdıktan sonra yeniden yönlendirme yoksa `SignOutAsync` veya kullanıcının **değil** oturumunuz.</span><span class="sxs-lookup"><span data-stu-id="c54b9-185">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="c54b9-186">POST belirtilen *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c54b9-186">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="c54b9-187">Tıklayarak **oturumunuzu** bağlama çağrıları `LogOut` eylem.</span><span class="sxs-lookup"><span data-stu-id="c54b9-187">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="c54b9-188">Önceki kod çağrıları `_signInManager.SignOutAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c54b9-188">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="c54b9-189">`SignOutAsync` Yöntemi, kullanıcının talepleri depolanan bir tanımlama bilgisinde temizler.</span><span class="sxs-lookup"><span data-stu-id="c54b9-189">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="c54b9-190">Test kimliği</span><span class="sxs-lookup"><span data-stu-id="c54b9-190">Test Identity</span></span>

<span data-ttu-id="c54b9-191">Varsayılan web projesi şablonları giriş sayfaları anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="c54b9-191">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="c54b9-192">Kimliğini test etmek için ekleme [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) hakkında sayfası.</span><span class="sxs-lookup"><span data-stu-id="c54b9-192">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="c54b9-193">Açmış durumdaysanız, oturumu kapatın. Uygulamayı çalıştırmak ve seçmek **hakkında** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="c54b9-193">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="c54b9-194">Oturum açma sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c54b9-194">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="c54b9-195">Kimlik keşfedin</span><span class="sxs-lookup"><span data-stu-id="c54b9-195">Explore Identity</span></span>

<span data-ttu-id="c54b9-196">Kimlik daha ayrıntılı olarak keşfetmek için:</span><span class="sxs-lookup"><span data-stu-id="c54b9-196">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="c54b9-197">Tam kimlik UI kaynağı oluşturma</span><span class="sxs-lookup"><span data-stu-id="c54b9-197">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="c54b9-198">Her bir sayfasını ve hata ayıklayıcı ile adım kaynağını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="c54b9-198">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="c54b9-199">Kimlik bileşenleri</span><span class="sxs-lookup"><span data-stu-id="c54b9-199">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c54b9-200">Tüm kimlik bağlı NuGet paketleri dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c54b9-200">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="c54b9-201">Birincil paket kimliği için [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="c54b9-201">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="c54b9-202">Bu paket, ASP.NET Core kimliği için arabirimleri çekirdek kümesini içerir ve tarafından eklenen `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="c54b9-202">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="c54b9-203">ASP.NET Core Identity'ye geçirme</span><span class="sxs-lookup"><span data-stu-id="c54b9-203">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="c54b9-204">Daha fazla bilgi ve varolan bir kimlik deposunu geçirme hakkında Yardım almak için bkz. [geçirme kimlik doğrulaması ve kimlik](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="c54b9-204">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="c54b9-205">Parola gücünü ayarlama</span><span class="sxs-lookup"><span data-stu-id="c54b9-205">Setting password strength</span></span>

<span data-ttu-id="c54b9-206">Bkz: [yapılandırma](#pw) en düşük Parola gereksinimlerinin ayarlayan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="c54b9-206">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c54b9-207">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="c54b9-207">Next Steps</span></span>

* [<span data-ttu-id="c54b9-208">Kimliği Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c54b9-208">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
