---
title: ASP.NET core'da kimliğe giriş
author: rick-anderson
description: Kimlik ile bir ASP.NET Core uygulaması kullanın. Parola gereksinimlerini (RequireDigit, RequiredLength, RequiredUniqueChars ve daha fazlası) ayarlama konusunda bilgi edinin.
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: fd5fa2fd1e069bf10f3baea38b1fe9f951dc4a7d
ms.sourcegitcommit: fd461c60b5e36c7019f81da0138cc859d0fddaa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "41752116"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET core'da kimliğe giriş

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core, ASP.NET Core uygulamaları için oturum açma işlevselliğini ekleyen bir üyelik sistemi kimliğidir. Kullanıcılar, bir hesap kimliğinde depolanan oturum açma bilgileri oluşturabilir veya bir dış oturum açma sağlayıcısı kullanabilirler. Desteklenen dış oturum açma sağlayıcılarını içerir [Facebook, Google, Microsoft Account ve Twitter](xref:security/authentication/social/index).

Kimlik, kullanıcı adları, parolalar ve profil verilerini depolamak için bir SQL Server veritabanı kullanılarak yapılandırılabilir. Alternatif olarak, başka bir kalıcı depolama, Azure tablo depolama örneği için kullanılabilir.

[Görüntüleyebilir veya örnek kodunu indirebilirsiniz.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Karşıdan yükleme)](xref:tutorials/index#how-to-download-a-sample)

Bu konuda, kimlik kaydolun, oturum açma için nasıl kullanacağınızı öğrenin ve bir kullanıcının oturumunu oturum. Kimlik kullanan uygulamalar oluşturma hakkında daha ayrıntılı yönergeler için bu makalenin sonunda sonraki adımlar bölümüne bakın.

## <a name="create-a-web-app-with-authentication"></a>Kimlik doğrulama ile bir Web uygulaması oluşturma

Bireysel kullanıcı hesapları ile bir ASP.NET Core Web uygulaması projesi oluşturun.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Seçin **dosya** > **yeni** > **proje**. 
* Seçin **ASP.NET Core Web uygulaması**. Projeyi adlandırın **WebApp1** proje indirme ile aynı ad alanına sahip. **Tamam**'ı tıklatın.
* Bir ASP.NET Core seçin **Web uygulaması** ASP.NET Core 2.1 için seçip **kimlik doğrulamayı Değiştir**.
* Seçin **bireysel kullanıcı hesapları** tıklatıp **Tamam**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

Oluşturulan proje sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:razor-pages/ui-class).

### <a name="test-register-and-login"></a>Test kayıt ve oturum açma

Uygulamayı çalıştırın ve bir kullanıcı kaydı. Ekran boyutu bağlı olarak, görmek için gezinti düğmesini seçmeniz gerekebilir **kaydetme** ve **oturum açma** bağlantıları.

![Geçiş gezinti düğmesi](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a>Kimlik Hizmetleri Yapılandırma

Hizmetleri eklenir `ConfigureServices`.

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

Yukarıdaki kod, varsayılan seçenek değerleriyle kimlik yapılandırır. Hizmetleri uygulama üzerinden için kullanılabilir yapılır [bağımlılık ekleme](xref:fundamentals/dependency-injection).

   Kimliği çağırarak etkinleştirildiğinde [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_). `UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Hizmetleri üzerinden uygulama için kullanılabilir yapılır [bağımlılık ekleme](xref:fundamentals/dependency-injection).

   Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseAuthentication` içinde `Configure` yöntemi. `UseAuthentication` kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Bu hizmetler uygulamaya sunulur [bağımlılık ekleme](xref:fundamentals/dependency-injection).

   Kimliği, uygulama için çağırarak etkinleştirildiğinde `UseIdentity` içinde `Configure` yöntemi. `UseIdentity` tanımlama bilgisi tabanlı kimlik doğrulaması ekler [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenine.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Daha fazla bilgi için [IdentityOptions sınıfı](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) ve [uygulama başlatma](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>İskele kaydı, oturum açma ve oturum kapatma

İzleyin [yetkilendirmesiyle Razor projesine kimlik iskelesini](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) yönergeleri.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Kaydı, oturum açma ve kapatmanın dosyaları ekleyin.


# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Proje adı ile oluşturduysanız **WebApp1**, aşağıdaki komutları çalıştırın. Aksi takdirde, doğru ad alanını kullanmak `ApplicationDbContext`:


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

PowerShell komut ayırıcı olarak virgül kullanır. PowerShell kullanarak, dosya listesi noktalı kaçış veya dosya listesi yukarıdaki örnekte gösterildiği gibi çift tırnak işareti koyun.

---

### <a name="examine-register"></a>Kayıt inceleyin

::: moniker range=">= aspnetcore-2.1"

   Kullanıcı tıkladığında **kaydetme** bağlantı `RegisterModel.OnPostAsync` eylem çağrılır. Kullanıcı tarafından oluşturulan [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) üzerinde `_userManager` nesne. `_userManager` bağımlılık ekleme tarafından sağlanır):

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

   Kullanıcı tıkladığında **kaydetme** bağlantı `Register` eylem üzerinde çağrıldığında `AccountController`. `Register` Eylem çağırarak kullanıcı oluşturur `CreateAsync` üzerinde `_userManager` nesne (sağlanan `AccountController` bağımlılık ekleme göre):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Kullanıcı başarıyla oluşturulmuş olsa bile, kullanıcı çağırarak oturum `_signInManager.SignInAsync`.

   **Not:** bkz [hesap onayı](xref:security/authentication/accconfirm#prevent-login-at-registration) kayıt sırasında hemen oturum açma önlemek adımlar.

### <a name="log-in"></a>Oturum aç

::: moniker range=">= aspnetcore-2.1"

Oturum açma formu görüntülenir olduğunda:

* **Oturum** bağlantı seçildiğinde.
* Ne zaman bir kullanıcının eriştiği nerede bunlar kimliği doğrulanmamış bir sayfa **veya** yetkili ve bunlar için oturum açma sayfasına yönlendirilirsiniz. 

Oturum açma sayfasındaki formu gönderildiğinde `OnPostAsync` eylem çağrılır. `PasswordSignInAsync` üzerinde çağrılır `_signInManager` (bağımlılık ekleme tarafından sağlanan) nesne.

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   Temel `Controller` sınıfı kullanıma sunan bir `User` denetleyici yöntemleri erişebileceğiniz özelliği. Örneğin, numaralandırma `User.Claims` ve yetkilendirme kararları. Daha fazla bilgi için [yetkilendirme](xref:security/authorization/index).

::: moniker-end
::: moniker range="= aspnetcore-2.0"

Kullanıcılar oturum açma formu görüntülenir **oturum** bağlantı veya erişimi kimlik doğrulaması gerektiren bir sayfaya yönlendirilirsiniz. Kullanıcı oturum açma sayfasındaki formu gönderdiğinde `AccountController` `Login` eylem çağrılır.

`Login` Eylem çağrıları `PasswordSignInAsync` üzerinde `_signInManager` nesne (sağlanan `AccountController` bağımlılık ekleme tarafından).

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

Temel (`Controller` veya `PageModel`) sınıfı kullanıma sunan bir `User` özelliği. Örneğin, `User.Claims` yetkilendirme kararları vermek için listelenebilir.

::: moniker-end

### <a name="log-out"></a>Oturumu Kapat

::: moniker range=">= aspnetcore-2.1"

**Oturumunuzu** bağlantı çağırır `LogoutModel.OnPost` eylem. 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) depolanan bir tanımlama bilgisinde kullanıcının talepleri temizler. Çağırdıktan sonra yeniden yönlendirme yoksa `SignOutAsync` veya kullanıcının **değil** oturumunuz.

POST belirtilen *Pages/Shared/_LoginPartial.cshtml*:

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"
   Tıklayarak **oturumunuzu** bağlama çağrıları `LogOut` eylem.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Önceki kod çağrıları `_signInManager.SignOutAsync` yöntemi. `SignOutAsync` Yöntemi, kullanıcının talepleri depolanan bir tanımlama bilgisinde temizler.
::: moniker-end

## <a name="test-identity"></a>Test kimliği

Varsayılan web projesi şablonları giriş sayfaları anonim erişime izin verin. Kimliğini test etmek için ekleme [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) hakkında sayfası.

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

Açmış durumdaysanız, oturumu kapatın. Uygulamayı çalıştırmak ve seçmek **hakkında** bağlantı. Oturum açma sayfasına yönlendirilirsiniz.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Kimlik keşfedin

Kimlik daha ayrıntılı olarak keşfetmek için:

* [Tam kimlik UI kaynağı oluşturma](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Her bir sayfasını ve hata ayıklayıcı ile adım kaynağını inceleyin.

::: moniker-end

## <a name="identity-components"></a>Kimlik bileşenleri

::: moniker range=">= aspnetcore-2.1"

Tüm kimlik bağlı NuGet paketleri dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).
::: moniker-end

Birincil paket kimliği için [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/). Bu paket, ASP.NET Core kimliği için arabirimleri çekirdek kümesini içerir ve tarafından eklenen `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

## <a name="migrating-to-aspnet-core-identity"></a>ASP.NET Core Identity'ye geçirme

Daha fazla bilgi ve varolan bir kimlik deposunu geçirme hakkında Yardım almak için bkz. [geçirme kimlik doğrulaması ve kimlik](xref:migration/identity).

## <a name="setting-password-strength"></a>Parola gücünü ayarlama

Bkz: [yapılandırma](#pw) en düşük Parola gereksinimlerinin ayarlayan bir örnek.

## <a name="next-steps"></a>Sonraki Adımlar

* [Kimliği Yapılandırma](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* [Kimlik birincil anahtar veri türü yapılandırma](xref:security/authentication/identity-primary-key-configuration).
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
