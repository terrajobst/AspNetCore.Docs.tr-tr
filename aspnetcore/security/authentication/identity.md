---
title: ASP.NET Core kimliğe giriş
author: rick-anderson
description: ASP.NET Core bir uygulamayla kimlik kullanın. Parola gereksinimlerini (RequireDigit, RequiredLength, RequiredUniqueChars ve daha fazlasını) ayarlamayı öğrenin.
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 325a61e6038e79b9a0db72c8360a5cbff2c8ddae
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011201"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET Core kimliğe giriş

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core kimlik, ASP.NET Core uygulamalara oturum açma işlevselliği ekleyen bir üyelik sistemidir. Kullanıcılar, kimlik içinde depolanan oturum açma bilgilerini içeren bir hesap oluşturabilir veya bir dış oturum açma sağlayıcısı kullanabilirler. Desteklenen dış oturum açma sağlayıcıları [Facebook, Google, Microsoft hesabı ve Twitter](xref:security/authentication/social/index)içerir.

Kimlik, Kullanıcı adlarını, parolaları ve profil verilerini depolamak için bir SQL Server veritabanı kullanılarak yapılandırılabilir. Alternatif olarak, başka bir kalıcı mağaza da kullanılabilir, örneğin Azure Tablo depolaması.

[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([indirme)](xref:index#how-to-download-a-sample)).

Bu konu başlığında, bir kullanıcıyı kaydetmek, oturum açmak ve oturumu kapatmak için kimlik kullanmayı öğrenirsiniz. Kimlik kullanan uygulamalar oluşturma hakkında daha ayrıntılı yönergeler için, bu makalenin sonundaki sonraki adımlar bölümüne bakın.

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a>Adddefaultıdentity ve AddEntity

[Adddefaultıdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) ASP.NET Core 2,1 ' de tanıtılmıştı. Çağırma `AddDefaultIdentity` , aşağıdakileri çağırmaya benzer:

* [AddEntity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [Adddefaultuı](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

Daha fazla bilgi için bkz. [Adddefaultıdentity kaynağı](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>Kimlik doğrulamasıyla bir Web uygulaması oluşturma

Bireysel kullanıcı hesaplarıyla bir ASP.NET Core Web uygulaması projesi oluşturun.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Dosya** > **Yeni** > **Proje**’yi seçin.
* Seçin **ASP.NET Core Web uygulaması**. Projeyi Proje **WebApp1** aynı ad alanına sahip olacak şekilde adlandırın. **Tamam**'ı tıklatın.
* Bir ASP.NET Core **Web uygulaması**seçip **kimlik doğrulamasını Değiştir**' i seçin.
* **Bireysel kullanıcı hesapları** ' nı seçip **Tamam**' a tıklayın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

Oluşturulan proje, [Razor sınıf kitaplığı](xref:razor-pages/ui-class)olarak [ASP.NET Core kimliği](xref:security/authentication/identity) sağlar. Identity Razor sınıfı kitaplığı, `Identity` alanı ile uç noktaları kullanıma sunar. Örneğin:

* /Identity/Account/Login
* /Identity/Account/Logout
* /Identity/Account/Manage

### <a name="apply-migrations"></a>Geçişleri Uygula

Veritabanını başlatılabilir şekilde geçişleri uygulayın.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın (PMC):

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a>Test kaydı ve oturum açma

Uygulamayı çalıştırın ve bir Kullanıcı kaydedin. Ekran boyutunuza bağlı olarak, **kayıt** ve **oturum açma** bağlantılarını görmek için gezinti geçiş düğmesini seçmeniz gerekebilir.

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>Kimlik hizmetlerini yapılandırma

Hizmetler ' de `ConfigureServices`eklenir. Tipik model, tüm `Add{Service}` yöntemleri çağırmalıdır ve sonra `services.Configure{Service}` tüm yöntemleri çağırır.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

Yukarıdaki kod, varsayılan seçenek değerleriyle kimliği yapılandırır. Hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama için kullanılabilir hale getirilir.

   Kimlik, [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)çağırarak etkinleştirilir. `UseAuthentication`istek ardışık düzenine kimlik doğrulama [ara yazılımı](xref:fundamentals/middleware/index) ekler.

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama için kullanılabilir hale getirilir.

   Yöntemi`Configure` çağırarak `UseAuthentication` , uygulama için kimlik etkinleştirilir. `UseAuthentication`istek ardışık düzenine kimlik doğrulama [ara yazılımı](xref:fundamentals/middleware/index) ekler.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Bu hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama için kullanılabilir hale getirilir.

   Yöntemi`Configure` çağırarak `UseIdentity` , uygulama için kimlik etkinleştirilir. `UseIdentity`istek ardışık düzenine tanımlama bilgisi tabanlı kimlik doğrulama [ara yazılımı](xref:fundamentals/middleware/index) ekler.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Daha fazla bilgi için bkz. [ıdentityoptions sınıfı](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) ve [uygulama başlatma](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>Yapı iskelesi kaydı, oturum açma ve oturum kapatma

Bu bölümde gösterilen kodu oluşturmak için, yetkilendirme yönergeleriyle [birlikte bir Razor projesinde yapı iskelesi kimliğini](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) izleyin.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Kayıt, oturum açma ve oturum kapatma dosyalarını ekleyin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Projeyi **WebApp1**adıyla oluşturduysanız aşağıdaki komutları çalıştırın. Aksi takdirde, için `ApplicationDbContext`doğru ad alanını kullanın:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell, bir komut ayırıcısı olarak noktalı virgül kullanır. PowerShell kullanırken, dosya listesinde noktalı virgül karakterini kaçış veya dosya listesini, yukarıdaki örnekte gösterildiği gibi çift tırnak içine koyun.

---

### <a name="examine-register"></a>Kaydı İncele

::: moniker range=">= aspnetcore-2.1"

   Kullanıcı **Kaydet** bağlantısına `RegisterModel.OnPostAsync` tıkladığında eylem çağrılır. Kullanıcı, `_userManager` nesnesi üzerinde [createasync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) tarafından oluşturulur. `_userManager`bağımlılık ekleme tarafından sağlanır):

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Kullanıcı **Kaydet** bağlantısına `Register` tıkladığında eylem üzerinde `AccountController`çağrılır. Eylem, nesneyi`_userManager` çağırarak `CreateAsync` kullanıcıyı oluşturur (bağımlılık eklenmesine `AccountController` göre tarafından verilir): `Register`

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Kullanıcı başarıyla oluşturulduysa, Kullanıcı çağrısıyla `_signInManager.SignInAsync`oturum açar.

   **Not:** Kayıt sırasında anında oturum açmayı önlemeye yönelik adımlar için bkz. [Hesap onayı](xref:security/authentication/accconfirm#prevent-login-at-registration) .

### <a name="log-in"></a>Oturum aç

::: moniker range=">= aspnetcore-2.1"

Oturum açma formu şu durumlarda görüntülenir:

* **Oturum aç** bağlantısı seçilidir.
* Kullanıcı, erişim yetkisi olmayan **veya** sistem tarafından kimliği doğrulanmamış olan sınırlı bir sayfaya erişmeyi dener.

Oturum açma sayfasındaki form gönderildiğinde, `OnPostAsync` eylem çağrılır. `PasswordSignInAsync``_signInManager` nesnesi üzerinde çağrılır (bağımlılık ekleme tarafından sağlanır).

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   Temel `Controller` sınıf, denetleyici yöntemlerinden `User` erişebileceğiniz bir özellik sunar. Örneğin, yetkilendirme kararlarını numaralandırabilirsiniz `User.Claims` ve yapabilirsiniz. Daha fazla bilgi için bkz. <xref:security/authorization/introduction>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Oturum açma formu, kullanıcılar bağlantı **oturumunu** seçerken veya kimlik doğrulaması gerektiren bir sayfaya erişirken yeniden yönlendirildiğinde görüntülenir. Kullanıcı oturum açma sayfasında formu gönderdiğinde, `AccountController` `Login` eylem çağrılır.

Eylem, `PasswordSignInAsync` nesne üzerinde çağrılar (bağımlılık eklenmesine göre `AccountController` ' ye verilir). `_signInManager` `Login`

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

Taban (`Controller` veya `PageModel`) sınıfı bir `User` özelliği kullanıma sunar. Örneğin, `User.Claims` yetkilendirme kararları almak için Numaralandırılabilir.

::: moniker-end

### <a name="log-out"></a>Oturumu kapat

::: moniker range=">= aspnetcore-2.1"

**Oturumu kapatma** bağlantısı `LogoutModel.OnPost` eylemi çağırır. 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

[Signoutasync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) , kullanıcının tanımlama bilgisinde depolanan taleplerini temizler.

Postala */paylaşılan/_LoginPartial. cshtml*dosyasında gönderi belirtilir:

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   **Oturum çıkış** bağlantısını tıklatmak `LogOut` eylemi çağırır.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Önceki kod `_signInManager.SignOutAsync` yöntemini çağırır. `SignOutAsync` Yöntemi, kullanıcının tanımlama bilgisinde depolanan taleplerini temizler.

::: moniker-end

## <a name="test-identity"></a>Test kimliği

Varsayılan Web projesi şablonları, giriş sayfalarına anonim erişime izin verir. Kimliği test etmek için Gizlilik [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) sayfasına ekleyin.

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

Oturumunuz açık ise oturumu kapatın. Uygulamayı çalıştırın ve **Gizlilik** bağlantısını seçin. Oturum açma sayfasına yönlendirilirsiniz.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Kimliği keşfet

Kimliği daha ayrıntılı incelemek için:

* [Tam kimlik UI kaynağı oluşturma](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Her sayfanın kaynağını inceleyin ve hata ayıklayıcıda ilerleyin.

::: moniker-end

## <a name="identity-components"></a>Kimlik bileşenleri

::: moniker range=">= aspnetcore-2.1"

Tüm kimlik bağımlı NuGet paketleri [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'e dahildir.

::: moniker-end

Kimliğin birincil paketi [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)' dır. Bu paket, ASP.NET Core kimliği için çekirdek arabirim kümesini içerir ve tarafından `Microsoft.AspNetCore.Identity.EntityFrameworkCore`dahildir.

## <a name="migrating-to-aspnet-core-identity"></a>ASP.NET Core kimliğe geçiriliyor

Mevcut kimlik deponuzu geçirme hakkında daha fazla bilgi ve yönergeler için bkz. [kimlik doğrulama ve kimlik geçişi](xref:migration/identity).

## <a name="setting-password-strength"></a>Parola gücünü ayarlama

Minimum parola gereksinimlerini ayarlayan bir örnek için bkz. [yapılandırma](#pw) .

## <a name="next-steps"></a>Sonraki Adımlar

* [Kimliği Yapılandırma](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
