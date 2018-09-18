---
title: ASP.NET core'da kimliğe giriş
author: rick-anderson
description: Kimlik ile bir ASP.NET Core uygulaması kullanın. Parola gereksinimlerini (RequireDigit, RequiredLength, RequiredUniqueChars ve daha fazlası) ayarlama konusunda bilgi edinin.
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: ca83d07f7d93bd0cc61cd17cc373096b8e6aa2e1
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010981"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="ed6e8-104">ASP.NET core'da kimliğe giriş</span><span class="sxs-lookup"><span data-stu-id="ed6e8-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="ed6e8-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed6e8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ed6e8-106">ASP.NET Core, ASP.NET Core uygulamaları için oturum açma işlevselliğini ekleyen bir üyelik sistemi kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="ed6e8-107">Kullanıcılar, bir hesap kimliğinde depolanan oturum açma bilgileri oluşturabilir veya bir dış oturum açma sağlayıcısı kullanabilirler.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="ed6e8-108">Desteklenen dış oturum açma sağlayıcılarını içerir [Facebook, Google, Microsoft Account ve Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="ed6e8-109">Kimlik, kullanıcı adları, parolalar ve profil verilerini depolamak için bir SQL Server veritabanı kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="ed6e8-110">Alternatif olarak, başka bir kalıcı depolama, Azure tablo depolama örneği için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="ed6e8-111">Görüntüleyebilir veya örnek kodunu indirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="ed6e8-112">(Karşıdan yükleme)</span><span class="sxs-lookup"><span data-stu-id="ed6e8-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="ed6e8-113">Bu konuda, kimlik kaydolun, oturum açma için nasıl kullanacağınızı öğrenin ve bir kullanıcının oturumunu oturum.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="ed6e8-114">Kimlik kullanan uygulamalar oluşturma hakkında daha ayrıntılı yönergeler için bu makalenin sonunda sonraki adımlar bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="ed6e8-115">AddDefaultIdentity ve AddIdentity</span><span class="sxs-lookup"><span data-stu-id="ed6e8-115">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="ed6e8-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) ASP kullanıma sunulmuştur. Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.Core 2.1.</span></span> <span data-ttu-id="ed6e8-117">Çağırma `AddDefaultIdentity` aşağıdaki çağırmak için benzer:</span><span class="sxs-lookup"><span data-stu-id="ed6e8-117">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="ed6e8-118">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="ed6e8-118">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="ed6e8-119">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="ed6e8-119">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="ed6e8-120">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="ed6e8-120">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="ed6e8-121">Bkz: [AddDefaultIdentity kaynak](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-121">See [AddDefaultIdentity source](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="ed6e8-122">Kimlik doğrulama ile bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="ed6e8-122">Create a Web app with authentication</span></span>

<span data-ttu-id="ed6e8-123">Bireysel kullanıcı hesapları ile bir ASP.NET Core Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-123">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ed6e8-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed6e8-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ed6e8-125">Seçin **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-125">Select **File** > **New** > **Project**.</span></span> 
* <span data-ttu-id="ed6e8-126">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-126">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="ed6e8-127">Projeyi adlandırın **WebApp1** proje indirme ile aynı ad alanına sahip.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-127">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="ed6e8-128">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-128">Click **OK**.</span></span>
* <span data-ttu-id="ed6e8-129">Bir ASP.NET Core seçin **Web uygulaması** ASP.NET Core 2.1 için seçip **kimlik doğrulamayı Değiştir**.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-129">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="ed6e8-130">Seçin **bireysel kullanıcı hesapları** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-130">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ed6e8-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ed6e8-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="ed6e8-132">Oluşturulan proje sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-132">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="ed6e8-133">Test kayıt ve oturum açma</span><span class="sxs-lookup"><span data-stu-id="ed6e8-133">Test Register and Login</span></span>

<span data-ttu-id="ed6e8-134">Uygulamayı çalıştırın ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-134">Run the app and register a user.</span></span> <span data-ttu-id="ed6e8-135">Ekran boyutu bağlı olarak, görmek için gezinti düğmesini seçmeniz gerekebilir **kaydetme** ve **oturum açma** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-135">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![Geçiş gezinti düğmesi](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="ed6e8-137">Kimlik Hizmetleri Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ed6e8-137">Configure Identity services</span></span>

<span data-ttu-id="ed6e8-138">Hizmetleri eklenir `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-138">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="ed6e8-139">Tipik bir düzen tüm çağırmaktır `Add{Service}` yöntemleri ve sonra çağrı tüm `services.Configure{Service}` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-139">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="ed6e8-140">Aşağıdaki kod, oluşturulan şablonu içermez `CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="ed6e8-140">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="ed6e8-141">Yukarıdaki kod, varsayılan seçenek değerleriyle kimlik yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-141">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="ed6e8-142">Hizmetleri uygulama üzerinden için kullanılabilir yapılır [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-142">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="ed6e8-143">Kimliği çağırarak etkinleştirildiğinde [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-143">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="ed6e8-144">`UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-144">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="ed6e8-145">Hizmetleri üzerinden uygulama için kullanılabilir yapılır [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-145">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="ed6e8-146">Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseAuthentication` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-146">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="ed6e8-147">`UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-147">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="ed6e8-148">Bu hizmetler uygulamaya sunulur [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-148">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="ed6e8-149">Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseIdentity` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-149">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="ed6e8-150">`UseIdentity` tanımlama bilgisi tabanlı kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-150">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="ed6e8-151">Daha fazla bilgi için [IdentityOptions sınıfı](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) ve [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-151">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="ed6e8-152">İskele kaydı, oturum açma ve oturum kapatma</span><span class="sxs-lookup"><span data-stu-id="ed6e8-152">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="ed6e8-153">İzleyin [yetkilendirmesiyle Razor projesine kimlik iskelesini](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-153">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ed6e8-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed6e8-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ed6e8-155">Kaydı, oturum açma ve kapatmanın dosyaları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-155">Add the Register, Login, and LogOut files.</span></span>


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ed6e8-156">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ed6e8-156">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ed6e8-157">Proje adı ile oluşturduysanız **WebApp1**, aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-157">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="ed6e8-158">Aksi takdirde, doğru ad alanını kullanmak `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="ed6e8-158">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

<span data-ttu-id="ed6e8-159">PowerShell komut ayırıcı olarak virgül kullanır.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-159">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="ed6e8-160">PowerShell kullanarak, dosya listesi noktalı kaçış veya dosya listesi yukarıdaki örnekte gösterildiği gibi çift tırnak işareti koyun.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-160">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="ed6e8-161">Kayıt inceleyin</span><span class="sxs-lookup"><span data-stu-id="ed6e8-161">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="ed6e8-162">Kullanıcı tıkladığında **kaydetme** bağlantı `RegisterModel.OnPostAsync` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-162">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="ed6e8-163">Kullanıcı tarafından oluşturulan [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) üzerinde `_userManager` nesne.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-163">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="ed6e8-164">`_userManager` bağımlılık ekleme tarafından sağlanır):</span><span class="sxs-lookup"><span data-stu-id="ed6e8-164">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="ed6e8-165">Kullanıcı tıkladığında **kaydetme** bağlantı `Register` eylem üzerinde çağrıldığında `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-165">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="ed6e8-166">`Register` Eylem çağırarak kullanıcı oluşturur `CreateAsync` üzerinde `_userManager` nesne (sağlanan `AccountController` bağımlılık ekleme göre):</span><span class="sxs-lookup"><span data-stu-id="ed6e8-166">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="ed6e8-167">Kullanıcı başarıyla oluşturulmuş olsa bile, kullanıcı çağırarak oturum `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-167">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="ed6e8-168">**Not:** bkz [hesap onayı](xref:security/authentication/accconfirm#prevent-login-at-registration) kayıt sırasında hemen oturum açma önlemek adımlar.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-168">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="ed6e8-169">Oturum aç</span><span class="sxs-lookup"><span data-stu-id="ed6e8-169">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ed6e8-170">Oturum açma formu görüntülenir olduğunda:</span><span class="sxs-lookup"><span data-stu-id="ed6e8-170">The Login form is displayed when:</span></span>

* <span data-ttu-id="ed6e8-171">**Oturum** bağlantı seçildiğinde.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-171">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="ed6e8-172">Ne zaman bir kullanıcının eriştiği nerede bunlar kimliği doğrulanmamış bir sayfa **veya** yetkili ve bunlar için oturum açma sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-172">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span> 

<span data-ttu-id="ed6e8-173">Oturum açma sayfasındaki formu gönderildiğinde `OnPostAsync` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-173">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="ed6e8-174">`PasswordSignInAsync` üzerinde çağrılır `_signInManager` (bağımlılık ekleme tarafından sağlanan) nesne.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-174">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="ed6e8-175">Temel `Controller` sınıfı kullanıma sunan bir `User` denetleyici yöntemleri erişebileceğiniz özelliği.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-175">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="ed6e8-176">Örneğin, numaralandırma `User.Claims` ve yetkilendirme kararları.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-176">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="ed6e8-177">Daha fazla bilgi için [yetkilendirme](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-177">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ed6e8-178">Kullanıcılar oturum açma formu görüntülenir **oturum** bağlantı veya erişimi kimlik doğrulaması gerektiren bir sayfaya yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-178">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="ed6e8-179">Kullanıcı oturum açma sayfasındaki formu gönderdiğinde `AccountController` `Login` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-179">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="ed6e8-180">`Login` Eylem çağrıları `PasswordSignInAsync` üzerinde `_signInManager` nesne (sağlanan `AccountController` bağımlılık ekleme tarafından).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-180">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="ed6e8-181">Temel (`Controller` veya `PageModel`) sınıfı kullanıma sunan bir `User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-181">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="ed6e8-182">Örneğin, `User.Claims` yetkilendirme kararları vermek için listelenebilir.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-182">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="ed6e8-183">Oturumu Kapat</span><span class="sxs-lookup"><span data-stu-id="ed6e8-183">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ed6e8-184">**Oturumunuzu** bağlantı çağırır `LogoutModel.OnPost` eylem.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-184">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="ed6e8-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) depolanan bir tanımlama bilgisinde kullanıcının talepleri temizler.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="ed6e8-186">Çağırdıktan sonra yeniden yönlendirme yoksa `SignOutAsync` veya kullanıcının **değil** oturumunuz.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-186">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="ed6e8-187">POST belirtilen *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ed6e8-187">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="ed6e8-188">Tıklayarak **oturumunuzu** bağlama çağrıları `LogOut` eylem.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-188">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="ed6e8-189">Önceki kod çağrıları `_signInManager.SignOutAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-189">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="ed6e8-190">`SignOutAsync` Yöntemi, kullanıcının talepleri depolanan bir tanımlama bilgisinde temizler.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-190">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="ed6e8-191">Test kimliği</span><span class="sxs-lookup"><span data-stu-id="ed6e8-191">Test Identity</span></span>

<span data-ttu-id="ed6e8-192">Varsayılan web projesi şablonları giriş sayfaları anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-192">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="ed6e8-193">Kimliğini test etmek için ekleme [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) hakkında sayfası.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-193">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="ed6e8-194">Açmış durumdaysanız, oturumu kapatın. Uygulamayı çalıştırmak ve seçmek **hakkında** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-194">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="ed6e8-195">Oturum açma sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-195">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="ed6e8-196">Kimlik keşfedin</span><span class="sxs-lookup"><span data-stu-id="ed6e8-196">Explore Identity</span></span>

<span data-ttu-id="ed6e8-197">Kimlik daha ayrıntılı olarak keşfetmek için:</span><span class="sxs-lookup"><span data-stu-id="ed6e8-197">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="ed6e8-198">Tam kimlik UI kaynağı oluşturma</span><span class="sxs-lookup"><span data-stu-id="ed6e8-198">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="ed6e8-199">Her bir sayfasını ve hata ayıklayıcı ile adım kaynağını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-199">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="ed6e8-200">Kimlik bileşenleri</span><span class="sxs-lookup"><span data-stu-id="ed6e8-200">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ed6e8-201">Tüm kimlik bağlı NuGet paketleri dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-201">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="ed6e8-202">Birincil paket kimliği için [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-202">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="ed6e8-203">Bu paket, ASP.NET Core kimliği için arabirimleri çekirdek kümesini içerir ve tarafından eklenen `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-203">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="ed6e8-204">ASP.NET Core Identity'ye geçirme</span><span class="sxs-lookup"><span data-stu-id="ed6e8-204">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="ed6e8-205">Daha fazla bilgi ve varolan bir kimlik deposunu geçirme hakkında Yardım almak için bkz. [geçirme kimlik doğrulaması ve kimlik](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-205">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="ed6e8-206">Parola gücünü ayarlama</span><span class="sxs-lookup"><span data-stu-id="ed6e8-206">Setting password strength</span></span>

<span data-ttu-id="ed6e8-207">Bkz: [yapılandırma](#pw) en düşük Parola gereksinimlerinin ayarlayan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="ed6e8-207">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed6e8-208">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="ed6e8-208">Next Steps</span></span>

* [<span data-ttu-id="ed6e8-209">Kimliği Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ed6e8-209">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <span data-ttu-id="ed6e8-210">[Kimlik birincil anahtar veri türü yapılandırma](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="ed6e8-210">[Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
