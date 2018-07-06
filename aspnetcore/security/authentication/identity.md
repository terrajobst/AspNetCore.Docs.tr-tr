---
title: ASP.NET core'da kimliğe giriş
author: rick-anderson
description: Kimlik ile bir ASP.NET Core uygulaması kullanın. İçerir, ayarı parola gereksinimleri (RequireDigit, RequiredLength, RequiredUniqueChars ve daha fazlası).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: c231a7619a4433ce004342ce68564e4c3892e702
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829308"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET core'da kimliğe giriş

Tarafından [Pranav Rastogi'nin](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), ve [Steve Smith](https://ardalis.com/)

ASP.NET Core kimliği oturum açma işlevler uygulamanıza eklemenize olanak sağlayan bir üyelik sistemidir. Kullanıcılar bir hesap ve oturum açma kullanıcı adı ile oluşturabilir ve parola veya Facebook, Google, Microsoft Account, Twitter veya diğerleri gibi bir dış oturum açma sağlayıcısını kullanabilirsiniz.

ASP.NET Core kimliği, kullanıcı adları, parolalar ve profil verilerini depolamak için bir SQL Server veritabanını kullanacak şekilde yapılandırabilirsiniz. Alternatif olarak, kendi kalıcı depolama, Azure tablo depolama gibi kullanabilirsiniz. Bu belge, Visual Studio için ve CLI kullanarak yönergeler içerir.

[Görüntüleyebilir veya örnek kodunu indirebilirsiniz.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Karşıdan yükleme)](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Kimliği'ne genel bakış

Bu konu başlığında, ASP.NET Core kimliği kaydolun, oturum açma için işlevsellik eklemek için kullanmayı öğrenin ve bir kullanıcının oturumunu oturum. ASP.NET Core kimliği kullanan uygulamalar oluşturma hakkında daha ayrıntılı yönergeler için bu makalenin sonunda sonraki adımlar bölümüne bakın.

1. Bireysel kullanıcı hesapları ile bir ASP.NET Core Web uygulaması projesi oluşturun.

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   Visual Studio'da **dosya** > **yeni** > **proje**. Seçin **ASP.NET Core Web uygulaması** tıklatıp **Tamam**.

   ![Yeni Proje iletişim kutusu](identity/_static/01-new-project.png)

   Bir ASP.NET Core seçin **Web uygulaması (Model-View-Controller)** ASP.NET Core 2.x ve ardından **kimlik doğrulamayı Değiştir**.

   ![Yeni Proje iletişim kutusu](identity/_static/02-new-project.png)

   Bir iletişim kutusu teklifi görünen kimlik doğrulama seçenekleri. Seçin **bireysel kullanıcı hesapları** tıklatıp **Tamam** önceki iletişim kutusuna dönün.

   ![Yeni Proje iletişim kutusu](identity/_static/03-new-project-auth.png)

   Seçme **bireysel kullanıcı hesapları** modelleri, Viewmodel'lar, görünümler, denetleyicileri ve proje şablonunun bir parçası olarak kimlik doğrulaması için gerekli olan diğer varlıkları oluşturmak için Visual Studio yönlendirir.

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

   .NET Core CLI'yı kullanarak, kullanarak yeni proje oluşturma `dotnet new mvc --auth Individual`. Bu komut, Visual Studio oluşturur aynı kimlik şablonu kodu ile yeni bir proje oluşturur.

   Oluşturulan projeyi içeren `Microsoft.AspNetCore.Identity.EntityFrameworkCore` kimlik verileri hem de şemayı SQL Server'ı kullanmaya devam ederse paketini [Entity Framework Core](https://docs.microsoft.com/ef/).

   ---

2. Kimlik Hizmetleri Yapılandırması ve Ara yazılımında `Startup`.

   Kimlik Hizmetleri Uygulaması'na eklenen `ConfigureServices` yönteminde `Startup` sınıfı:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Bu hizmetler uygulamaya sunulur [bağımlılık ekleme](xref:fundamentals/dependency-injection).

   Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseAuthentication` içinde `Configure` yöntemi. `UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Bu hizmetler uygulamaya sunulur [bağımlılık ekleme](xref:fundamentals/dependency-injection).

   Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseIdentity` içinde `Configure` yöntemi. `UseIdentity` tanımlama bilgisi tabanlı kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   Uygulama başlatma işlemi hakkında daha fazla bilgi için bkz: [uygulama başlatma](xref:fundamentals/startup).

3. Bir kullanıcı oluşturun.

   Uygulamayı başlatın ve ardından **kaydetme** bağlantı.

   Bu eylem gerçekleştiriyorsunuz ilk kez buysa, geçişler çalıştırmak için gerekli olabilir. Uygulama ister **geçerli geçişleri**. Gerekirse, sayfayı yenileyin.

   ![Geçerli geçişleri Web sayfası](identity/_static/apply-migrations.png)

   Alternatif olarak, kullanarak, bir bellek içi veritabanı olmadan kalıcı bir veritabanı ile ASP.NET Core kimliği test edebilirsiniz. Bir bellek içi veritabanı kullanmak için ekleme `Microsoft.EntityFrameworkCore.InMemory` paketini uygulamanıza ve uygulamanızın çağrı değiştirme `AddDbContext` içinde `ConfigureServices` gibi:

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   Kullanıcı tıkladığında **kaydetme** bağlantı `Register` eylem üzerinde çağrıldığında `AccountController`. `Register` Eylem çağırarak kullanıcı oluşturur `CreateAsync` üzerinde `_userManager` nesne (sağlanan `AccountController` bağımlılık ekleme göre):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   Kullanıcı başarıyla oluşturulmuş olsa bile, kullanıcı çağırarak oturum `_signInManager.SignInAsync`.

   **Not:** bkz [hesap onayı](xref:security/authentication/accconfirm#prevent-login-at-registration) kayıt sırasında hemen oturum açma önlemek adımlar.

4. Oturum aç.

   Kullanıcılar oturum açabilir tıklayarak **oturum** üst sitenin bağlantısını veya yetkilendirme gerektiren site parçası erişmeye çalışırsanız, oturum açma sayfasına gittiğinizde. Kullanıcı oturum açma sayfasındaki formu gönderdiğinde `AccountController` `Login` eylem çağrılır.

   `Login` Eylem çağrıları `PasswordSignInAsync` üzerinde `_signInManager` nesne (sağlanan `AccountController` bağımlılık ekleme tarafından).

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   Temel `Controller` sınıfı kullanıma sunan bir `User` denetleyici yöntemleri erişebileceğiniz özelliği. Örneğin, numaralandırma `User.Claims` ve yetkilendirme kararları. Daha fazla bilgi için [yetkilendirme](xref:security/authorization/index).

5. Oturumunu kapatın.

   Tıklayarak **oturumunuzu** bağlama çağrıları `LogOut` eylem.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Yukarıdaki kod çağrıları yukarıda `_signInManager.SignOutAsync` yöntemi. `SignOutAsync` Yöntemi, kullanıcının talepleri depolanan bir tanımlama bilgisinde temizler.

<a name="pw"></a>
6. Yapılandırma.

   Uygulamanın başlangıç sınıfında geçersiz kılınabilir bazı varsayılan davranışlar kimliği vardır. `IdentityOptions` varsayılan davranışları kullanırken yapılandırılması gerekmez. Aşağıdaki kod, birkaç parola gücü seçeneklerini ayarlar:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   Kimlik yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma kimlik](xref:security/authentication/identity-configuration).

   Birincil anahtar veri türü de yapılandırabilirsiniz bkz [yapılandırma kimliği birincil anahtar veri türü](xref:security/authentication/identity-primary-key-configuration).

7. Veritabanı görüntüleyin.

   Uygulamanızı bir SQL Server veritabanı (Windows ve Visual Studio kullanıcılar için varsayılan) kullanıyorsa, veritabanı uygulamayı görüntüleyebilirsiniz. Kullanabileceğiniz **SQL Server Management Studio**. Alternatif olarak, Visual Studio'dan seçin **görünümü** > **SQL Server Nesne Gezgini**. Bağlanma **(localdb) \MSSQLLocalDB**. Eşleşen bir ad ile veritabanı `aspnet-<name of your project>-<guid>` görüntülenir.

   ![Bağlam menüsünde AspNetUsers veritabanı tablosu](identity/_static/04-db.png)

   Veritabanı'nı genişletin ve kendi **tabloları**, ardından sağ tıklayarak **dbo. AspNetUsers** tablosunu seçip **görünüm verilerini**.

8. Kimliği'nin çalıştığını doğrulama

    Varsayılan *ASP.NET Core Web uygulaması* proje şablonu sağlar zorunda kalmadan herhangi bir işlem uygulama erişmek kullanıcıların oturum açmak için. ASP.NET Identity çalıştığını doğrulamak için ekleme bir`[Authorize]` özniteliğini `About` eylemi `Home` denetleyicisi.

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Kullanarak projeyi Çalıştır **Ctrl** + **F5** gidin **hakkında** sayfası. Yalnızca kimliği doğrulanmış kullanıcılar erişebilir **hakkında** ASP.NET oturum açmak veya kaydetmek için oturum açma sayfasına yönlendirir, böylece artık, sayfa.

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    Bir komut penceresi açın ve projenin kök dizinine gidin dizinini içeren `.csproj` dosya. Çalıştırma [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) uygulamayı çalıştırmak için komutu:

    ```csharp
    dotnet run 
    ```

    Çıktıda belirtilen URL'ye Gözat [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) komutu. URL işaret etmelidir `localhost` oluşturulan bağlantı noktası numarası. Gidin **hakkında** sayfası. Yalnızca kimliği doğrulanmış kullanıcılar erişebilir **hakkında** ASP.NET oturum açmak veya kaydetmek için oturum açma sayfasına yönlendirir, böylece artık, sayfa.

    ---

## <a name="identity-components"></a>Kimlik bileşenleri

Kimlik sistemi için birincil başvuru derleme `Microsoft.AspNetCore.Identity`. Bu paket, ASP.NET Core kimliği için arabirimleri çekirdek kümesini içerir ve tarafından eklenen `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Bu bağımlılıklar, ASP.NET Core uygulamaları kimlik sistemi kullanmak için gereklidir:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Kimlik Entity Framework Core ile kullanmak için gerekli türleri içerir.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core SQL Server gibi ilişkisel veritabanları için Microsoft'un önerilen veri erişim teknolojisi ' dir. Test etmek için kullanacağınız `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Tanımlama bilgisi tabanlı kimlik doğrulaması kullanmak bir uygulama sağlar ara yazılımı.

## <a name="migrating-to-aspnet-core-identity"></a>ASP.NET Core Identity'ye geçirme

Bkz: Varolan kimliğinizi geçirme hakkında Yardım almak ve ek bilgi depolamak için [geçirme kimlik doğrulaması ve kimlik](xref:migration/identity).

## <a name="setting-password-strength"></a>Parola gücünü ayarlama

Bkz: [yapılandırma](#pw) en düşük Parola gereksinimlerinin ayarlayan bir örnek.

## <a name="next-steps"></a>Sonraki Adımlar

* [Kimlik doğrulaması ve kimlik geçirme](xref:migration/identity)
* [Hesap Onaylama ve Parola Kurtarma](xref:security/authentication/accconfirm)
* [SMS ile iki öğeli kimlik doğrulama](xref:security/authentication/2fa)
* [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index)
