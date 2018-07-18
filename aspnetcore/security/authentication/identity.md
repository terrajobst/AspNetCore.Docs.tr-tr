---
title: ASP.NET core'da kimliğe giriş
author: rick-anderson
description: Kimlik ile bir ASP.NET Core uygulaması kullanın. İçerir, ayarı parola gereksinimleri (RequireDigit, RequiredLength, RequiredUniqueChars ve daha fazlası).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: 50ddb96000e6a3f9e1762e9bb3e1f215f20d4356
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095645"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="90b81-104">ASP.NET core'da kimliğe giriş</span><span class="sxs-lookup"><span data-stu-id="90b81-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="90b81-105">Tarafından [Pranav Rastogi'nin](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="90b81-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="90b81-106">ASP.NET Core kimliği oturum açma işlevler uygulamanıza eklemenize olanak sağlayan bir üyelik sistemidir.</span><span class="sxs-lookup"><span data-stu-id="90b81-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="90b81-107">Kullanıcılar bir hesap ve oturum açma kullanıcı adı ile oluşturabilir ve parola veya Facebook, Google, Microsoft Account, Twitter veya diğerleri gibi bir dış oturum açma sağlayıcısını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90b81-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="90b81-108">ASP.NET Core kimliği, kullanıcı adları, parolalar ve profil verilerini depolamak için bir SQL Server veritabanını kullanacak şekilde yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90b81-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="90b81-109">Alternatif olarak, kendi kalıcı depolama, Azure tablo depolama gibi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90b81-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="90b81-110">Bu belge, Visual Studio için ve CLI kullanarak yönergeler içerir.</span><span class="sxs-lookup"><span data-stu-id="90b81-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="90b81-111">Görüntüleyebilir veya örnek kodunu indirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90b81-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="90b81-112">(Karşıdan yükleme)</span><span class="sxs-lookup"><span data-stu-id="90b81-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="90b81-113">Kimliği'ne genel bakış</span><span class="sxs-lookup"><span data-stu-id="90b81-113">Overview of Identity</span></span>

<span data-ttu-id="90b81-114">Bu konu başlığında, ASP.NET Core kimliği kaydolun, oturum açma için işlevsellik eklemek için kullanmayı öğrenin ve bir kullanıcının oturumunu oturum.</span><span class="sxs-lookup"><span data-stu-id="90b81-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="90b81-115">ASP.NET Core kimliği kullanan uygulamalar oluşturma hakkında daha ayrıntılı yönergeler için bu makalenin sonunda sonraki adımlar bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="90b81-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1. <span data-ttu-id="90b81-116">Bireysel kullanıcı hesapları ile bir ASP.NET Core Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="90b81-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90b81-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90b81-117">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="90b81-118">Visual Studio'da **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="90b81-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="90b81-119">Seçin **ASP.NET Core Web uygulaması** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="90b81-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

   ![Yeni Proje iletişim kutusu](identity/_static/01-new-project.png)

   <span data-ttu-id="90b81-121">Bir ASP.NET Core seçin **Web uygulaması (Model-View-Controller)** ASP.NET Core 2.x ve ardından **kimlik doğrulamayı Değiştir**.</span><span class="sxs-lookup"><span data-stu-id="90b81-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

   ![Yeni Proje iletişim kutusu](identity/_static/02-new-project.png)

   <span data-ttu-id="90b81-123">Bir iletişim kutusu teklifi görünen kimlik doğrulama seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="90b81-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="90b81-124">Seçin **bireysel kullanıcı hesapları** tıklatıp **Tamam** önceki iletişim kutusuna dönün.</span><span class="sxs-lookup"><span data-stu-id="90b81-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

   ![Yeni Proje iletişim kutusu](identity/_static/03-new-project-auth.png)

   <span data-ttu-id="90b81-126">Seçme **bireysel kullanıcı hesapları** modelleri, Viewmodel'lar, görünümler, denetleyicileri ve proje şablonunun bir parçası olarak kimlik doğrulaması için gerekli olan diğer varlıkları oluşturmak için Visual Studio yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="90b81-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="90b81-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="90b81-127">.NET Core CLI</span></span>](#tab/netcore-cli)

   <span data-ttu-id="90b81-128">.NET Core CLI'yı kullanarak, kullanarak yeni proje oluşturma `dotnet new mvc --auth Individual`.</span><span class="sxs-lookup"><span data-stu-id="90b81-128">If using the .NET Core CLI, create the new project using `dotnet new mvc --auth Individual`.</span></span> <span data-ttu-id="90b81-129">Bu komut, Visual Studio oluşturur aynı kimlik şablonu kodu ile yeni bir proje oluşturur.</span><span class="sxs-lookup"><span data-stu-id="90b81-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

   <span data-ttu-id="90b81-130">Oluşturulan projeyi içeren `Microsoft.AspNetCore.Identity.EntityFrameworkCore` kimlik verileri hem de şemayı SQL Server'ı kullanmaya devam ederse paketini [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="90b81-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

   ---

2. <span data-ttu-id="90b81-131">Kimlik Hizmetleri Yapılandırması ve Ara yazılımında `Startup`.</span><span class="sxs-lookup"><span data-stu-id="90b81-131">Configure Identity services and add middleware in `Startup`.</span></span>

   <span data-ttu-id="90b81-132">Kimlik Hizmetleri Uygulaması'na eklenen `ConfigureServices` yönteminde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="90b81-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90b81-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90b81-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="90b81-134">Bu hizmetler uygulamaya sunulur [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="90b81-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="90b81-135">Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseAuthentication` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="90b81-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="90b81-136">`UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="90b81-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90b81-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90b81-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="90b81-138">Bu hizmetler uygulamaya sunulur [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="90b81-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="90b81-139">Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseIdentity` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="90b81-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="90b81-140">`UseIdentity` tanımlama bilgisi tabanlı kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.</span><span class="sxs-lookup"><span data-stu-id="90b81-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   <span data-ttu-id="90b81-141">Uygulama başlatma işlemi hakkında daha fazla bilgi için bkz: [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="90b81-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3. <span data-ttu-id="90b81-142">Bir kullanıcı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="90b81-142">Create a user.</span></span>

   <span data-ttu-id="90b81-143">Uygulamayı başlatın ve ardından **kaydetme** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="90b81-143">Launch the application and then click on the **Register** link.</span></span>

   <span data-ttu-id="90b81-144">Bu eylem gerçekleştiriyorsunuz ilk kez buysa, geçişler çalıştırmak için gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="90b81-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="90b81-145">Uygulama ister **geçerli geçişleri**.</span><span class="sxs-lookup"><span data-stu-id="90b81-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="90b81-146">Gerekirse, sayfayı yenileyin.</span><span class="sxs-lookup"><span data-stu-id="90b81-146">Refresh the page if needed.</span></span>

   ![Geçerli geçişleri Web sayfası](identity/_static/apply-migrations.png)

   <span data-ttu-id="90b81-148">Alternatif olarak, kullanarak, bir bellek içi veritabanı olmadan kalıcı bir veritabanı ile ASP.NET Core kimliği test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90b81-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="90b81-149">Bir bellek içi veritabanı kullanmak için ekleme `Microsoft.EntityFrameworkCore.InMemory` paketini uygulamanıza ve uygulamanızın çağrı değiştirme `AddDbContext` içinde `ConfigureServices` gibi:</span><span class="sxs-lookup"><span data-stu-id="90b81-149">To use an in-memory database, add the `Microsoft.EntityFrameworkCore.InMemory` package to your app and modify your app's call to `AddDbContext` in `ConfigureServices` as follows:</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   <span data-ttu-id="90b81-150">Kullanıcı tıkladığında **kaydetme** bağlantı `Register` eylem üzerinde çağrıldığında `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="90b81-150">When the user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="90b81-151">`Register` Eylem çağırarak kullanıcı oluşturur `CreateAsync` üzerinde `_userManager` nesne (sağlanan `AccountController` bağımlılık ekleme göre):</span><span class="sxs-lookup"><span data-stu-id="90b81-151">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   <span data-ttu-id="90b81-152">Kullanıcı başarıyla oluşturulmuş olsa bile, kullanıcı çağırarak oturum `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="90b81-152">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="90b81-153">**Not:** bkz [hesap onayı](xref:security/authentication/accconfirm#prevent-login-at-registration) kayıt sırasında hemen oturum açma önlemek adımlar.</span><span class="sxs-lookup"><span data-stu-id="90b81-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

4. <span data-ttu-id="90b81-154">Oturum aç.</span><span class="sxs-lookup"><span data-stu-id="90b81-154">Log in.</span></span>

   <span data-ttu-id="90b81-155">Kullanıcılar oturum açabilir tıklayarak **oturum** üst sitenin bağlantısını veya yetkilendirme gerektiren site parçası erişmeye çalışırsanız, oturum açma sayfasına gittiğinizde.</span><span class="sxs-lookup"><span data-stu-id="90b81-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="90b81-156">Kullanıcı oturum açma sayfasındaki formu gönderdiğinde `AccountController` `Login` eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="90b81-156">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

   <span data-ttu-id="90b81-157">`Login` Eylem çağrıları `PasswordSignInAsync` üzerinde `_signInManager` nesne (sağlanan `AccountController` bağımlılık ekleme tarafından).</span><span class="sxs-lookup"><span data-stu-id="90b81-157">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   <span data-ttu-id="90b81-158">Temel `Controller` sınıfı kullanıma sunan bir `User` denetleyici yöntemleri erişebileceğiniz özelliği.</span><span class="sxs-lookup"><span data-stu-id="90b81-158">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="90b81-159">Örneğin, numaralandırma `User.Claims` ve yetkilendirme kararları.</span><span class="sxs-lookup"><span data-stu-id="90b81-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="90b81-160">Daha fazla bilgi için [yetkilendirme](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="90b81-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>

5. <span data-ttu-id="90b81-161">Oturumunu kapatın.</span><span class="sxs-lookup"><span data-stu-id="90b81-161">Log out.</span></span>

   <span data-ttu-id="90b81-162">Tıklayarak **oturumunuzu** bağlama çağrıları `LogOut` eylem.</span><span class="sxs-lookup"><span data-stu-id="90b81-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="90b81-163">Yukarıdaki kod çağrıları yukarıda `_signInManager.SignOutAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="90b81-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="90b81-164">`SignOutAsync` Yöntemi, kullanıcının talepleri depolanan bir tanımlama bilgisinde temizler.</span><span class="sxs-lookup"><span data-stu-id="90b81-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

<a name="pw"></a>
6. <span data-ttu-id="90b81-165">Yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="90b81-165">Configuration.</span></span>

   <span data-ttu-id="90b81-166">Uygulamanın başlangıç sınıfında geçersiz kılınabilir bazı varsayılan davranışlar kimliği vardır.</span><span class="sxs-lookup"><span data-stu-id="90b81-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="90b81-167">`IdentityOptions` varsayılan davranışları kullanırken yapılandırılması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="90b81-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="90b81-168">Aşağıdaki kod, birkaç parola gücü seçeneklerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="90b81-168">The following code sets several password strength options:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90b81-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90b81-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90b81-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90b81-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   <span data-ttu-id="90b81-171">Kimlik yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma kimlik](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="90b81-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>

   <span data-ttu-id="90b81-172">Birincil anahtar veri türü de yapılandırabilirsiniz bkz [yapılandırma kimliği birincil anahtar veri türü](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="90b81-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>

7. <span data-ttu-id="90b81-173">Veritabanı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="90b81-173">View the database.</span></span>

   <span data-ttu-id="90b81-174">Uygulamanızı bir SQL Server veritabanı (Windows ve Visual Studio kullanıcılar için varsayılan) kullanıyorsa, veritabanı uygulamayı görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90b81-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="90b81-175">Kullanabileceğiniz **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="90b81-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="90b81-176">Alternatif olarak, Visual Studio'dan seçin **görünümü** > **SQL Server Nesne Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="90b81-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="90b81-177">Bağlanma **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="90b81-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="90b81-178">Eşleşen bir ad ile veritabanı `aspnet-<name of your project>-<guid>` görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="90b81-178">The database with a name matching `aspnet-<name of your project>-<guid>` is displayed.</span></span>

   ![Bağlam menüsünde AspNetUsers veritabanı tablosu](identity/_static/04-db.png)

   <span data-ttu-id="90b81-180">Veritabanı'nı genişletin ve kendi **tabloları**, ardından sağ tıklayarak **dbo. AspNetUsers** tablosunu seçip **görünüm verilerini**.</span><span class="sxs-lookup"><span data-stu-id="90b81-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="90b81-181">Kimliği'nin çalıştığını doğrulama</span><span class="sxs-lookup"><span data-stu-id="90b81-181">Verify Identity works</span></span>

    <span data-ttu-id="90b81-182">Varsayılan *ASP.NET Core Web uygulaması* proje şablonu sağlar zorunda kalmadan herhangi bir işlem uygulama erişmek kullanıcıların oturum açmak için.</span><span class="sxs-lookup"><span data-stu-id="90b81-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="90b81-183">ASP.NET Identity çalıştığını doğrulamak için ekleme bir`[Authorize]` özniteliğini `About` eylemi `Home` denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="90b81-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90b81-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90b81-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="90b81-185">Kullanarak projeyi Çalıştır **Ctrl** + **F5** gidin **hakkında** sayfası.</span><span class="sxs-lookup"><span data-stu-id="90b81-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="90b81-186">Yalnızca kimliği doğrulanmış kullanıcılar erişebilir **hakkında** ASP.NET oturum açmak veya kaydetmek için oturum açma sayfasına yönlendirir, böylece artık, sayfa.</span><span class="sxs-lookup"><span data-stu-id="90b81-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="90b81-187">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="90b81-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="90b81-188">Bir komut penceresi açın ve projenin kök dizinine gidin dizinini içeren `.csproj` dosya.</span><span class="sxs-lookup"><span data-stu-id="90b81-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="90b81-189">Çalıştırma [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) uygulamayı çalıştırmak için komutu:</span><span class="sxs-lookup"><span data-stu-id="90b81-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```csharp
    dotnet run 
    ```

    <span data-ttu-id="90b81-190">Çıktıda belirtilen URL'ye Gözat [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) komutu.</span><span class="sxs-lookup"><span data-stu-id="90b81-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="90b81-191">URL işaret etmelidir `localhost` oluşturulan bağlantı noktası numarası.</span><span class="sxs-lookup"><span data-stu-id="90b81-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="90b81-192">Gidin **hakkında** sayfası.</span><span class="sxs-lookup"><span data-stu-id="90b81-192">Navigate to the **About** page.</span></span> <span data-ttu-id="90b81-193">Yalnızca kimliği doğrulanmış kullanıcılar erişebilir **hakkında** ASP.NET oturum açmak veya kaydetmek için oturum açma sayfasına yönlendirir, böylece artık, sayfa.</span><span class="sxs-lookup"><span data-stu-id="90b81-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="90b81-194">Kimlik bileşenleri</span><span class="sxs-lookup"><span data-stu-id="90b81-194">Identity Components</span></span>

<span data-ttu-id="90b81-195">Kimlik sistemi için birincil başvuru derleme `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="90b81-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="90b81-196">Bu paket, ASP.NET Core kimliği için arabirimleri çekirdek kümesini içerir ve tarafından eklenen `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="90b81-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="90b81-197">Bu bağımlılıklar, ASP.NET Core uygulamaları kimlik sistemi kullanmak için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="90b81-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="90b81-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Kimlik Entity Framework Core ile kullanmak için gerekli türleri içerir.</span><span class="sxs-lookup"><span data-stu-id="90b81-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="90b81-199">`Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core SQL Server gibi ilişkisel veritabanları için Microsoft'un önerilen veri erişim teknolojisi ' dir.</span><span class="sxs-lookup"><span data-stu-id="90b81-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="90b81-200">Test etmek için kullanacağınız `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="90b81-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="90b81-201">`Microsoft.AspNetCore.Authentication.Cookies` -Tanımlama bilgisi tabanlı kimlik doğrulaması kullanmak bir uygulama sağlar ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="90b81-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="90b81-202">ASP.NET Core Identity'ye geçirme</span><span class="sxs-lookup"><span data-stu-id="90b81-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="90b81-203">Bkz: Varolan kimliğinizi geçirme hakkında Yardım almak ve ek bilgi depolamak için [geçirme kimlik doğrulaması ve kimlik](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="90b81-203">For additional information and guidance on migrating your existing Identity store see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="90b81-204">Parola gücünü ayarlama</span><span class="sxs-lookup"><span data-stu-id="90b81-204">Setting password strength</span></span>

<span data-ttu-id="90b81-205">Bkz: [yapılandırma](#pw) en düşük Parola gereksinimlerinin ayarlayan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="90b81-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90b81-206">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="90b81-206">Next Steps</span></span>

* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:security/authentication/social/index>
* <xref:host-and-deploy/web-farm>
