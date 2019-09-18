---
title: ASP.NET Core kimliğe giriş
author: rick-anderson
description: ASP.NET Core bir uygulamayla kimlik kullanın. Parola gereksinimlerini (RequireDigit, RequiredLength, RequiredUniqueChars ve daha fazlasını) ayarlamayı öğrenin.
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 979681cfc196aca9fb5097583d99a086e1c597ba
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082459"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="f70f6-104">ASP.NET Core kimliğe giriş</span><span class="sxs-lookup"><span data-stu-id="f70f6-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="f70f6-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f70f6-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f70f6-106">ASP.NET Core kimlik, ASP.NET Core uygulamalara oturum açma işlevselliği ekleyen bir üyelik sistemidir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="f70f6-107">Kullanıcılar, kimlik içinde depolanan oturum açma bilgilerini içeren bir hesap oluşturabilir veya bir dış oturum açma sağlayıcısı kullanabilirler.</span><span class="sxs-lookup"><span data-stu-id="f70f6-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="f70f6-108">Desteklenen dış oturum açma sağlayıcıları [Facebook, Google, Microsoft hesabı ve Twitter](xref:security/authentication/social/index)içerir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="f70f6-109">Kimlik, Kullanıcı adlarını, parolaları ve profil verilerini depolamak için bir SQL Server veritabanı kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="f70f6-110">Alternatif olarak, başka bir kalıcı mağaza da kullanılabilir, örneğin Azure Tablo depolaması.</span><span class="sxs-lookup"><span data-stu-id="f70f6-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="f70f6-111">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([indirme)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f70f6-111">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="f70f6-112">Bu konu başlığında, bir kullanıcıyı kaydetmek, oturum açmak ve oturumu kapatmak için kimlik kullanmayı öğrenirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f70f6-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="f70f6-113">Kimlik kullanan uygulamalar oluşturma hakkında daha ayrıntılı yönergeler için, bu makalenin sonundaki sonraki adımlar bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="f70f6-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="f70f6-114">Adddefaultıdentity ve AddEntity</span><span class="sxs-lookup"><span data-stu-id="f70f6-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="f70f6-115">[Adddefaultıdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) ASP.NET Core 2,1 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="f70f6-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="f70f6-116">Çağırma `AddDefaultIdentity` , aşağıdakileri çağırmaya benzer:</span><span class="sxs-lookup"><span data-stu-id="f70f6-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="f70f6-117">AddEntity</span><span class="sxs-lookup"><span data-stu-id="f70f6-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="f70f6-118">Adddefaultuı</span><span class="sxs-lookup"><span data-stu-id="f70f6-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="f70f6-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="f70f6-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="f70f6-120">Daha fazla bilgi için bkz. [Adddefaultıdentity kaynağı](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="f70f6-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="f70f6-121">Kimlik doğrulamasıyla bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="f70f6-121">Create a Web app with authentication</span></span>

<span data-ttu-id="f70f6-122">Bireysel kullanıcı hesaplarıyla bir ASP.NET Core Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f70f6-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f70f6-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f70f6-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f70f6-124">**Dosya** > **Yeni** > **Proje**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="f70f6-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="f70f6-125">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="f70f6-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f70f6-126">Projeyi Proje **WebApp1** aynı ad alanına sahip olacak şekilde adlandırın.</span><span class="sxs-lookup"><span data-stu-id="f70f6-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="f70f6-127">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f70f6-127">Click **OK**.</span></span>
* <span data-ttu-id="f70f6-128">Bir ASP.NET Core **Web uygulaması**seçip **kimlik doğrulamasını Değiştir**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="f70f6-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="f70f6-129">**Bireysel kullanıcı hesapları** ' nı seçip **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f70f6-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f70f6-130">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f70f6-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="f70f6-131">Oluşturulan proje, [Razor sınıf kitaplığı](xref:razor-pages/ui-class)olarak [ASP.NET Core kimliği](xref:security/authentication/identity) sağlar.</span><span class="sxs-lookup"><span data-stu-id="f70f6-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="f70f6-132">Identity Razor sınıfı kitaplığı, `Identity` alanı ile uç noktaları kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="f70f6-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="f70f6-133">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f70f6-133">For example:</span></span>

* <span data-ttu-id="f70f6-134">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="f70f6-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="f70f6-135">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="f70f6-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="f70f6-136">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="f70f6-136">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="f70f6-137">Geçişleri Uygula</span><span class="sxs-lookup"><span data-stu-id="f70f6-137">Apply migrations</span></span>

<span data-ttu-id="f70f6-138">Veritabanını başlatılabilir şekilde geçişleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="f70f6-138">Apply the migrations to initialise the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f70f6-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f70f6-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f70f6-140">Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın (PMC):</span><span class="sxs-lookup"><span data-stu-id="f70f6-140">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f70f6-141">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f70f6-141">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="f70f6-142">Test kaydı ve oturum açma</span><span class="sxs-lookup"><span data-stu-id="f70f6-142">Test Register and Login</span></span>

<span data-ttu-id="f70f6-143">Uygulamayı çalıştırın ve bir Kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f70f6-143">Run the app and register a user.</span></span> <span data-ttu-id="f70f6-144">Ekran boyutunuza bağlı olarak, **kayıt** ve **oturum açma** bağlantılarını görmek için gezinti geçiş düğmesini seçmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-144">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="f70f6-145">Kimlik hizmetlerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f70f6-145">Configure Identity services</span></span>

<span data-ttu-id="f70f6-146">Hizmetler ' de `ConfigureServices`eklenir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-146">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="f70f6-147">Tipik model, tüm `Add{Service}` yöntemleri çağırmalıdır ve sonra `services.Configure{Service}` tüm yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-147">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="f70f6-148">Yukarıdaki kod, varsayılan seçenek değerleriyle kimliği yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-148">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="f70f6-149">Hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama için kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-149">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="f70f6-150">Kimlik, [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)çağırarak etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-150">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="f70f6-151">`UseAuthentication`istek ardışık düzenine kimlik doğrulama [ara yazılımı](xref:fundamentals/middleware/index) ekler.</span><span class="sxs-lookup"><span data-stu-id="f70f6-151">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="f70f6-152">Hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama için kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-152">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="f70f6-153">Yöntemi`Configure` çağırarak `UseAuthentication` , uygulama için kimlik etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-153">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="f70f6-154">`UseAuthentication`istek ardışık düzenine kimlik doğrulama [ara yazılımı](xref:fundamentals/middleware/index) ekler.</span><span class="sxs-lookup"><span data-stu-id="f70f6-154">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="f70f6-155">Bu hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama için kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-155">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="f70f6-156">Yöntemi`Configure` çağırarak `UseIdentity` , uygulama için kimlik etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-156">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="f70f6-157">`UseIdentity`istek ardışık düzenine tanımlama bilgisi tabanlı kimlik doğrulama [ara yazılımı](xref:fundamentals/middleware/index) ekler.</span><span class="sxs-lookup"><span data-stu-id="f70f6-157">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="f70f6-158">Daha fazla bilgi için bkz. [ıdentityoptions sınıfı](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) ve [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="f70f6-158">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="f70f6-159">Yapı iskelesi kaydı, oturum açma ve oturum kapatma</span><span class="sxs-lookup"><span data-stu-id="f70f6-159">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="f70f6-160">Bu bölümde gösterilen kodu oluşturmak için, yetkilendirme yönergeleriyle [birlikte bir Razor projesinde yapı iskelesi kimliğini](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) izleyin.</span><span class="sxs-lookup"><span data-stu-id="f70f6-160">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f70f6-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f70f6-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f70f6-162">Kayıt, oturum açma ve oturum kapatma dosyalarını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f70f6-162">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f70f6-163">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f70f6-163">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f70f6-164">Projeyi **WebApp1**adıyla oluşturduysanız aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f70f6-164">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="f70f6-165">Aksi takdirde, için `ApplicationDbContext`doğru ad alanını kullanın:</span><span class="sxs-lookup"><span data-stu-id="f70f6-165">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="f70f6-166">PowerShell, bir komut ayırıcısı olarak noktalı virgül kullanır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-166">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="f70f6-167">PowerShell kullanırken, dosya listesinde noktalı virgül karakterini kaçış veya dosya listesini, yukarıdaki örnekte gösterildiği gibi çift tırnak içine koyun.</span><span class="sxs-lookup"><span data-stu-id="f70f6-167">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="f70f6-168">Kaydı İncele</span><span class="sxs-lookup"><span data-stu-id="f70f6-168">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="f70f6-169">Kullanıcı **Kaydet** bağlantısına `RegisterModel.OnPostAsync` tıkladığında eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-169">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="f70f6-170">Kullanıcı, `_userManager` nesnesi üzerinde [createasync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f70f6-170">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="f70f6-171">`_userManager`bağımlılık ekleme tarafından sağlanır):</span><span class="sxs-lookup"><span data-stu-id="f70f6-171">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="f70f6-172">Kullanıcı **Kaydet** bağlantısına `Register` tıkladığında eylem üzerinde `AccountController`çağrılır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-172">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="f70f6-173">Eylem, nesneyi`_userManager` çağırarak `CreateAsync` kullanıcıyı oluşturur (bağımlılık eklenmesine `AccountController` göre tarafından verilir): `Register`</span><span class="sxs-lookup"><span data-stu-id="f70f6-173">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="f70f6-174">Kullanıcı başarıyla oluşturulduysa, Kullanıcı çağrısıyla `_signInManager.SignInAsync`oturum açar.</span><span class="sxs-lookup"><span data-stu-id="f70f6-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="f70f6-175">**Not:** Kayıt sırasında anında oturum açmayı önlemeye yönelik adımlar için bkz. [Hesap onayı](xref:security/authentication/accconfirm#prevent-login-at-registration) .</span><span class="sxs-lookup"><span data-stu-id="f70f6-175">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="f70f6-176">Oturum aç</span><span class="sxs-lookup"><span data-stu-id="f70f6-176">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f70f6-177">Oturum açma formu şu durumlarda görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="f70f6-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="f70f6-178">**Oturum aç** bağlantısı seçilidir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="f70f6-179">Kullanıcı, erişim yetkisi olmayan **veya** sistem tarafından kimliği doğrulanmamış olan sınırlı bir sayfaya erişmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="f70f6-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="f70f6-180">Oturum açma sayfasındaki form gönderildiğinde, `OnPostAsync` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="f70f6-181">`PasswordSignInAsync``_signInManager` nesnesi üzerinde çağrılır (bağımlılık ekleme tarafından sağlanır).</span><span class="sxs-lookup"><span data-stu-id="f70f6-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="f70f6-182">Temel `Controller` sınıf, denetleyici yöntemlerinden `User` erişebileceğiniz bir özellik sunar.</span><span class="sxs-lookup"><span data-stu-id="f70f6-182">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="f70f6-183">Örneğin, yetkilendirme kararlarını numaralandırabilirsiniz `User.Claims` ve yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f70f6-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="f70f6-184">Daha fazla bilgi için bkz. <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="f70f6-184">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f70f6-185">Oturum açma formu, kullanıcılar bağlantı **oturumunu** seçerken veya kimlik doğrulaması gerektiren bir sayfaya erişirken yeniden yönlendirildiğinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-185">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="f70f6-186">Kullanıcı oturum açma sayfasında formu gönderdiğinde, `AccountController` `Login` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-186">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="f70f6-187">Eylem, `PasswordSignInAsync` nesne üzerinde çağrılar (bağımlılık eklenmesine göre `AccountController` ' ye verilir). `_signInManager` `Login`</span><span class="sxs-lookup"><span data-stu-id="f70f6-187">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="f70f6-188">Taban (`Controller` veya `PageModel`) sınıfı bir `User` özelliği kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="f70f6-188">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="f70f6-189">Örneğin, `User.Claims` yetkilendirme kararları almak için Numaralandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-189">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="f70f6-190">Oturumu kapat</span><span class="sxs-lookup"><span data-stu-id="f70f6-190">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f70f6-191">**Oturumu kapatma** bağlantısı `LogoutModel.OnPost` eylemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-191">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="f70f6-192">[Signoutasync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) , kullanıcının tanımlama bilgisinde depolanan taleplerini temizler.</span><span class="sxs-lookup"><span data-stu-id="f70f6-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="f70f6-193">Postala */paylaşılan/_LoginPartial. cshtml*dosyasında gönderi belirtilir:</span><span class="sxs-lookup"><span data-stu-id="f70f6-193">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="f70f6-194">**Oturum çıkış** bağlantısını tıklatmak `LogOut` eylemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-194">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="f70f6-195">Önceki kod `_signInManager.SignOutAsync` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-195">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="f70f6-196">`SignOutAsync` Yöntemi, kullanıcının tanımlama bilgisinde depolanan taleplerini temizler.</span><span class="sxs-lookup"><span data-stu-id="f70f6-196">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="f70f6-197">Test kimliği</span><span class="sxs-lookup"><span data-stu-id="f70f6-197">Test Identity</span></span>

<span data-ttu-id="f70f6-198">Varsayılan Web projesi şablonları, giriş sayfalarına anonim erişime izin verir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-198">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="f70f6-199">Kimliği test etmek için Gizlilik [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f70f6-199">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="f70f6-200">Oturumunuz açık ise oturumu kapatın. Uygulamayı çalıştırın ve **Gizlilik** bağlantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="f70f6-200">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="f70f6-201">Oturum açma sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f70f6-201">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="f70f6-202">Kimliği keşfet</span><span class="sxs-lookup"><span data-stu-id="f70f6-202">Explore Identity</span></span>

<span data-ttu-id="f70f6-203">Kimliği daha ayrıntılı incelemek için:</span><span class="sxs-lookup"><span data-stu-id="f70f6-203">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="f70f6-204">Tam kimlik UI kaynağı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f70f6-204">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="f70f6-205">Her sayfanın kaynağını inceleyin ve hata ayıklayıcıda ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="f70f6-205">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="f70f6-206">Kimlik bileşenleri</span><span class="sxs-lookup"><span data-stu-id="f70f6-206">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f70f6-207">Tüm kimlik bağımlı NuGet paketleri [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'e dahildir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-207">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="f70f6-208">Kimliğin birincil paketi [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)' dır.</span><span class="sxs-lookup"><span data-stu-id="f70f6-208">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="f70f6-209">Bu paket, ASP.NET Core kimliği için çekirdek arabirim kümesini içerir ve tarafından `Microsoft.AspNetCore.Identity.EntityFrameworkCore`dahildir.</span><span class="sxs-lookup"><span data-stu-id="f70f6-209">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="f70f6-210">ASP.NET Core kimliğe geçiriliyor</span><span class="sxs-lookup"><span data-stu-id="f70f6-210">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="f70f6-211">Mevcut kimlik deponuzu geçirme hakkında daha fazla bilgi ve yönergeler için bkz. [kimlik doğrulama ve kimlik geçişi](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="f70f6-211">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="f70f6-212">Parola gücünü ayarlama</span><span class="sxs-lookup"><span data-stu-id="f70f6-212">Setting password strength</span></span>

<span data-ttu-id="f70f6-213">Minimum parola gereksinimlerini ayarlayan bir örnek için bkz. [yapılandırma](#pw) .</span><span class="sxs-lookup"><span data-stu-id="f70f6-213">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f70f6-214">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="f70f6-214">Next Steps</span></span>

* [<span data-ttu-id="f70f6-215">Kimliği Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f70f6-215">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
