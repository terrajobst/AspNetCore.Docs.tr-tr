---
title: "ASP.NET Core üzerinde kimliğini giriş"
author: rick-anderson
description: "Bir ASP.NET Core uygulamayla kimliğini kullanın. İçerir, ayarı parola gereksinimleri (RequireDigit, RequiredLength, RequiredUniqueChars ve daha fazla)."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity
ms.openlocfilehash: 8cbf002a9280650a08ae8d49b5b6d23bafb8be18
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET Core üzerinde kimliğini giriş

Tarafından [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [zel Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), ve [Steve Smith](https://ardalis.com/)

ASP.NET Core, uygulamanıza oturum açma işlevsellik eklemesine olanak tanıyan bir üyelik sistemi kimliğidir. Kullanıcılar bir kullanıcı adıyla bir hesap ve oturum açma oluşturabilir ve parola veya bir dış oturum açma sağlayıcısı Facebook, Google, Microsoft Account, Twitter veya diğerleri gibi kullanabilirsiniz.

ASP.NET Core kimliği kullanıcı adları, parolalar ve profil verileri depolamak için bir SQL Server veritabanını kullanmak üzere yapılandırabilirsiniz. Alternatif olarak, örneğin, bir Azure Table Storage kendi kalıcı depoya kullanabilirsiniz. Bu belge, CLI kullanarak için ve Visual Studio için yönergeler içerir.

[Görüntülemek veya örnek kodu indirin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Karşıdan yükleme)](https://docs.microsoft.com/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Kimliği'ne genel bakış

Bu konuda, oturum açma kaydetmeyi işlevselliği eklemek için ASP.NET Core kimliği kullanmayı öğrenin ve bir kullanıcı oturum. ASP.NET Core kimliği kullanarak uygulamaları oluşturma hakkında daha ayrıntılı yönergeler için bu makalenin sonunda sonraki adımlar bölümüne bakın.

1.  Bir ASP.NET çekirdek Web uygulaması projesi ile bireysel kullanıcı hesapları oluşturun.

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Visual Studio'da seçin **dosya** > **yeni** > **proje**. Seçin **ASP.NET çekirdek Web uygulaması** tıklatıp **Tamam**.

    ![Yeni Proje iletişim kutusu](identity/_static/01-new-project.png)

    Bir ASP.NET Core seçin **Web uygulaması (Model-View-Controller)** ASP.NET 2.x çekirdek ve ardından **kimlik doğrulamayı Değiştir**.

    ![Yeni Proje iletişim kutusu](identity/_static/02-new-project.png)

    Bir iletişim kutusu önerme görünür kimlik doğrulama seçenekleri. Seçin **tek tek kullanıcı hesaplarını** tıklatıp **Tamam** önceki iletişim kutusuna dönmek için.

    ![Yeni Proje iletişim kutusu](identity/_static/03-new-project-auth.png)

    Seçme **tek tek kullanıcı hesaplarını** modelleri, ViewModels, görünümler, denetleyicileri ve proje şablonu bir parçası olarak kimlik doğrulaması için gerekli diğer varlıklar oluşturmak için Visual Studio yönlendirir.

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    .NET Core CLI kullanıyorsanız, yeni projesi oluşturun ``dotnet new mvc --auth Individual``. Bu komut, Visual Studio oluşturur aynı kimlik şablonu kodu ile yeni bir proje oluşturur.

    Oluşturulan projeyi içeren `Microsoft.AspNetCore.Identity.EntityFrameworkCore` kimlik veri ve şema SQL Server kullanmaya devam ederse paket [Entity Framework Çekirdek](https://docs.microsoft.com/ef/).

    ---

2.  Kimlik hizmetlerini yapılandırmak ve Ara yazılımında eklemek `Startup`.

    Kimlik Hizmetleri Uygulaması'na eklenen `ConfigureServices` yönteminde `Startup` sınıfı:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    Bu Hizmetleri üzerinden uygulama için kullanılabilir hale getirilir [bağımlılık ekleme](xref:fundamentals/dependency-injection).
    
    Kimlik etkin uygulama için çağırarak `UseAuthentication` içinde `Configure` yöntemi. `UseAuthentication` kimlik doğrulama ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    Bu Hizmetleri üzerinden uygulama için kullanılabilir hale getirilir [bağımlılık ekleme](xref:fundamentals/dependency-injection).
    
    Kimlik etkin uygulama için çağırarak `UseIdentity` içinde `Configure` yöntemi. `UseIdentity` tanımlama bilgisi tabanlı kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.
        
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Uygulama başlatma işlemi hakkında daha fazla bilgi için bkz: [uygulama başlangıç](xref:fundamentals/startup).

3.  Bir kullanıcı oluşturun.

    Uygulamayı başlatın ve sonra tıklatın **kaydetmek** bağlantı.

    Bu eylem gerçekleştiriyorsunuz ilk kez kullanıyorsanız geçişler çalıştırmak için gerekli olabilir. Uygulama ister **uygulamak geçişler**. Gerekirse sayfayı yenileyin.
    
    ![Geçişler Web sayfasına Uygula](identity/_static/apply-migrations.png)
    
    Alternatif olarak, kullanarak, bir bellek içi veritabanına kalıcı bir veritabanı olmadan uygulamanızla ASP.NET Core kimliği test edebilirsiniz. Bellek içi veritabanı kullanmak için add ``Microsoft.EntityFrameworkCore.InMemory`` paketini uygulamanıza ve uygulamanızın çağrısına değiştirme ``AddDbContext`` içinde ``ConfigureServices`` gibi:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Kullanıcı tıkladığında **kaydetmek** bağlantı ``Register`` eylem çağrılır ``AccountController``. ``Register`` Eylem çağırarak kullanıcı oluşturur `CreateAsync` üzerinde `_userManager` nesne (için sağlanan ``AccountController`` bağımlılık ekleme göre):
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Kullanıcı başarıyla oluşturulduysa, kullanıcı çağrısıyla oturum ``_signInManager.SignInAsync``.

    **Not:** bkz [hesap onayı](xref:security/authentication/accconfirm#prevent-login-at-registration) adımların kayıt sırasında hemen oturum açma önlemek.
 
4.  Oturum aç.
 
    Kullanıcılar oturum açabilir tıklayarak **oturum** üst sitenin bağlantısını veya yetkilendirme gerektiren site parçası erişmeye çalışırsanız, oturum açma sayfasına gittiğinizde. Kullanıcı oturum açma sayfasındaki formu gönderdiğinde ``AccountController`` ``Login`` eylem çağrılır.

    ``Login`` Eylem çağrılarını ``PasswordSignInAsync`` üzerinde ``_signInManager`` nesne (için sağlanan ``AccountController`` bağımlılık ekleme tarafından).

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    Temel ``Controller`` sınıf çıkarır bir ``User`` denetleyicisi yöntemleri erişebilirsiniz özelliği. Örneğin, listeleme `User.Claims` ve yetkilendirme kararları. Daha fazla bilgi için bkz: [yetkilendirme](xref:security/authorization/index).
 
5.  Oturumu kapatın.
 
    Tıklatarak **oturumunuzu** bağlama çağrıları `LogOut` eylem.
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    Önceki kod çağrıları yukarıda `_signInManager.SignOutAsync` yöntemi. `SignOutAsync` Yöntemi bir tanımlama bilgisinde depolanan kullanıcının talepleri temizler.
 
<a name="pw"></a>
6.  Yapılandırma.

    Kimliği uygulamanın başlangıç sınıfında geçersiz kılınabilir bazı varsayılan davranışlar vardır. `IdentityOptions` varsayılan davranışları kullanırken yapılandırılması gerekmez. Aşağıdaki kod, çeşitli parola gücünü seçenekleri ayarlar:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    Kimlik yapılandırma hakkında daha fazla bilgi için bkz: [yapılandırma kimlik](xref:security/authentication/identity-configuration).
    
    Birincil anahtar, veri türünü de yapılandırabilirsiniz bkz [yapılandırma kimlik birincil anahtarlar veri türü](xref:security/authentication/identity-primary-key-configuration).
 
7.  Veritabanını görüntüleyin.

    Uygulamanızı bir SQL Server veritabanı (Windows ve Visual Studio kullanıcılar için varsayılan) kullanıyorsa, veritabanı oluşturulan uygulama görüntüleyebilirsiniz. Kullanabileceğiniz **SQL Server Management Studio**. Alternatif olarak, Visual Studio'dan seçin **Görünüm** > **SQL Server Nesne Gezgini**. Bağlanmak **(localdb) \MSSQLLocalDB**. İle eşleşen ada sahip veritabanı **aspnet - <*projenizin ad*>-<*tarih dizesi* >**  görüntülenir.

    ![Bağlam menüsünde AspNetUsers veritabanı tablosu](identity/_static/04-db.png)
    
    Veritabanı'nı genişletin ve kendi **tabloları**, sonra sağ **dbo. AspNetUsers** tablo ve seçin **görünüm verilerini**.

8. Kimliği'nin çalıştığını doğrulama

    Varsayılan *ASP.NET çekirdek Web uygulaması* proje şablonu sağlar gerek kalmadan uygulama içinde herhangi bir işlem erişmek kullanıcıların oturum açmak için. ASP.NET Identity çalıştığını doğrulamak için ekleme bir`[Authorize]` özniteliğini `About` eylemi `Home` denetleyicisi.
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Kullanarak projeyi çalıştırmak **Ctrl** + **F5** gidin **hakkında** sayfası. Yalnızca kimliği doğrulanan kullanıcılar erişebilir **hakkında** ASP.NET oturum açmak veya kaydetmek için oturum açma sayfasına yönlendirir şekilde şimdi, sayfa.

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    Bir komut penceresi açın ve projenin kök dizinine gidin dizinini içeren `.csproj` dosya. Çalıştırma [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) uygulamayı çalıştırmak için komutu:

    ```cs
    dotnet run 
    ```

    Cmdlet çıktısının belirtilen URL'ye Gözat [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) komutu. URL işaret etmelidir `localhost` ile oluşturulan bağlantı noktası numarası. Gidin **hakkında** sayfası. Yalnızca kimliği doğrulanan kullanıcılar erişebilir **hakkında** ASP.NET oturum açmak veya kaydetmek için oturum açma sayfasına yönlendirir şekilde şimdi, sayfa.

    ---

## <a name="identity-components"></a>Kimlik bileşenleri

Kimlik sistemi için birincil başvuru derleme `Microsoft.AspNetCore.Identity`. Bu paket ASP.NET Core kimliği arabirimleri çekirdek kümesini içerir ve tarafından dahil `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Bu bağımlılıklar, ASP.NET Core uygulamalarında kimlik sistemi kullanmak için gereklidir:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Identity Entity Framework Çekirdek ile kullanmak için gereken türler içerir.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Çekirdek Microsoft'un önerilen veri erişimi SQL Server gibi ilişkisel veritabanları için teknolojisidir. Kullanabileceğiniz test etmek için `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Tanımlama bilgisi tabanlı kimlik doğrulaması kullanmak bir uygulama sağlar ara yazılımı.

## <a name="migrating-to-aspnet-core-identity"></a>ASP.NET Core kimliği geçirme

Ek bilgi ve mevcut kimliğinizi geçirme konusunda yönergeler bakın depolamak için [geçirme kimlik doğrulama ve kimlik](xref:migration/identity).

## <a name="setting-password-strength"></a>Parola gücünü ayarlama

Bkz: [yapılandırma](#pw) minimum parola gereksinimlerini ayarlar bir örnek için.

## <a name="next-steps"></a>Sonraki Adımlar

* [Geçirme kimlik doğrulama ve kimlik](xref:migration/identity)
* [Hesap Onaylama ve Parola Kurtarma](xref:security/authentication/accconfirm)
* [SMS ile iki öğeli kimlik doğrulama](xref:security/authentication/2fa)
* [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index)
