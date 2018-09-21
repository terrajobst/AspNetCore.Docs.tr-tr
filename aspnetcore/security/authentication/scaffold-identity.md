---
title: ASP.NET Core projelerinde iskele kimlik
author: rick-anderson
description: Bir ASP.NET Core projesi içinde kimlik iskele oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 37ad9897fbc5eb1822ed2413334b4fce9050296b
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523044"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>ASP.NET Core projelerinde iskele kimlik

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2.1 ve üzeri sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:razor-pages/ui-class). Kimlik içeren uygulamaları seçmeli olarak kimlik Razor sınıf kitaplığı (RCL) yer alan kaynak kodu eklemek için iskele kurucu uygulayabilirsiniz. Kodu değiştirin ve davranışını değiştirmek için kaynak kodu oluşturmak isteyebilirsiniz. Örneğin, kayıt için kullanılan kod üretmek için iskele kurucu toplamasını. Oluşturulan kod aynı kimlik RCL kodda daha önceliklidir. Kullanıcı arabirimi tam denetimini ve varsayılan RCL kullanmamak için bölümüne bakın. [tam kimlik UI Kaynağı Oluştur](#full).

Yapan uygulamalar **değil** dahil kimlik doğrulaması RCL kimlik paketini eklemek için iskele kurucu uygulayabilirsiniz. Oluşturulacak kimlik kodu seçme seçeneğiniz vardır.

Çoğu gerekli kodu iskele kurucu oluşturur ancak işlemi tamamlamak için projeyi güncelleştirmeniz gerekir. Bu belge, bir kimlik yapı iskelesi güncelleştirmeyi tamamlamak için gereken adımları açıklar.

Kimlik iskele kurucu çalıştırdığınızda bir *ScaffoldingReadme.txt* dosyası proje dizininde oluşturulur. *ScaffoldingReadme.txt* dosyası kimlik yapı iskelesi güncelleştirmeyi tamamlamak gerekli genel yönergeleri içerir. Bu belgede daha daha eksiksiz yönergeler içeren *ScaffoldingReadme.txt* dosya.

Dosya farklılıklarını gösterir ve dışında değişiklikleri geri sağlar bir kaynak denetim sistemi kullanmanızı öneririz. Kimlik iskele kurucu çalıştırdıktan sonra değişiklikleri inceleyin.

> [!NOTE]
> Hizmetleri, kullanırken gereklidir [iki faktörlü kimlik doğrulaması](xref:security/authentication/identity-enable-qrcodes), [hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm)ve diğer güvenlik özellikleri kimliği. Hizmetlerin veya hizmet saptamalar kimlik iskele kurma özelliği çalıştırdığınızda oluşturulan değildir. Bu özellikleri etkinleştirmek için hizmetlerini el ile eklenmesi gerekir. Örneğin, [gerektiren e-posta onayı](xref:security/authentication/accconfirm#require-email-confirmation).

## <a name="scaffold-identity-into-an-empty-project"></a>Boş bir projeye iskele kimlik

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Aşağıdaki vurgulanmış çağrıları ekleyin `Startup` sınıfı:

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Mevcut bir yetkilendirme olmadan bir Razor projeye iskele kimlik

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Kimlik yapılandırılmıştır *Areas/Identity/IdentityHostingStartup.cs*. Daha fazla bilgi için [Ihostingstartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Geçişler, UseAuthentication ve düzeni

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>Kimlik doğrulamasını etkinleştirme

İçinde `Configure` yöntemi `Startup` sınıfı, çağrı [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sonra `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Düzen değişiklikleri

İsteğe bağlı: Kısmi oturum açma ekleme (`_LoginPartial`) için Düzen dosyası:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Razor projesine yetkilendirmesiyle iskele kimlik

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth

dotnet new webapp -au Individual -o RPauth

dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Bazı kimlik seçeneklerini yapılandırılan *Areas/Identity/IdentityHostingStartup.cs*. Daha fazla bilgi için [Ihostingstartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Var olan bir yetkilendirme olmadan bir MVC projeye iskele kimlik

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

İsteğe bağlı: Kısmi oturum açma ekleme (`_LoginPartial`) için *Views/Shared/_Layout.cshtml* dosyası:

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Taşıma *Pages/Shared/_LoginPartial.cshtml* dosyasını *Views/Shared/_LoginPartial.cshtml*

Kimlik yapılandırılmıştır *Areas/Identity/IdentityHostingStartup.cs*. Ihostingstartup daha fazla bilgi için bkz.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Çağrı [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sonra `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Bir yetkilendirme ile MVC projeye iskele kimlik

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Silme *sayfaları/paylaşılan* klasörünü ve klasördeki dosyaları.

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Tam kimlik UI kaynağı oluşturma

Kimlik UI tam denetimi korumak için kimlik iskele kurucu çalıştırın ve seçin **tüm dosyaları geçersiz kılma**.

Aşağıdaki vurgulanmış kodu varsayılan kimlik UI kimliği ile bir ASP.NET Core 2.1 web uygulamasında değiştirin. değişiklik gösterir. Kimlik UI tam denetim sahibi olmak için bunu yapmak isteyebilirsiniz.

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Varsayılan kimlik, aşağıdaki kodda değiştirilir:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

Aşağıdaki kod kümeleri [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), ve [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Kayıt bir `IEmailSender` uygulaması örneği için:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core 2.1 ve daha sonra kimlik doğrulama kodu değişiklikleri](xref:migration/20_21#changes-to-authentication-code)