---
title: ASP.NET Core Windows kimlik doğrulamasını yapılandırma
author: scottaddie
description: IIS Express, IIS ve HTTP.sys kullanarak ASP.NET Core Windows kimlik doğrulaması yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 04/03/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: bd4ffa79c4d1e0070c820fa9c06b0a84c3aaae74
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902849"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>ASP.NET Core Windows kimlik doğrulamasını yapılandırma

Tarafından [Scott Addie](https://twitter.com/Scott_Addie) ve [Luke Latham](https://github.com/guardrex)

[Windows kimlik doğrulaması](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [IIS](xref:host-and-deploy/iis/index) veya [HTTP.sys](xref:fundamentals/servers/httpsys).

Windows kimlik doğrulaması, ASP.NET Core uygulamaları, kullanıcıların kimliklerini doğrulamak için işletim sistemi kullanır. Sunucunuz, kullanıcıları tanımlamak için Active Directory etki alanı kimlikleri veya Windows hesaplarını kullanarak bir şirket ağında çalıştığında, Windows kimlik doğrulaması kullanabilirsiniz. Windows kimlik doğrulaması nerede kullanıcılar, istemci uygulamaları ve web sunucuları aynı Windows etki alanına ait intranet ortamları için idealdir.

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>ASP.NET Core uygulaması Windows kimlik doğrulamasını etkinleştirin

**Web uygulaması** şablonu Visual Studio veya .NET Core CLI aracılığıyla kullanılabilir, Windows kimlik doğrulamayı destekleyecek şekilde yapılandırılabilir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a>Yeni bir proje için Windows kimlik doğrulama uygulaması şablonunu kullanma

Visual Studio'da:

1. Yeni bir proje oluşturun.
1. Seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.
1. Bir ad sağlayın **proje adı** alan. Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın. **Oluştur**’u seçin.
1. Seçin **değişiklik** altında **kimlik doğrulaması**.
1. İçinde **kimlik doğrulamayı Değiştir** penceresinde **Windows kimlik doğrulaması**. **Tamam**’ı seçin.
1. Seçin **Web uygulaması**.
1. **Oluştur**’u seçin.

Uygulamayı çalıştırın. Kullanıcı işlenen uygulamanın kullanıcı arabiriminde görüntülenir.

### <a name="manual-configuration-for-an-existing-project"></a>Var olan bir proje için el ile yapılandırma

Projenin özelliklerini Windows kimlik doğrulamasını etkinleştirmenizi ve anonim kimlik doğrulamasını devre dışı izin ver:

1. Visual Studio'nun'nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**.
1. Seçin **hata ayıklama** sekmesi.
1. Onay kutusunu temizleyin **anonim kimlik doğrulamasını etkinleştir**.
1. Onay kutusunu seçin **Windows kimlik doğrulamasını etkinleştir**.

Alternatif olarak, özellikler, yapılandırılabilir `iisSettings` düğümünün *launchSettings.json* dosyası:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Kullanım **Windows kimlik doğrulaması** uygulaması şablonu.

Yürütme [yeni dotnet](/dotnet/core/tools/dotnet-new) komutunu `webapp` bağımsız değişken (ASP.NET Core Web uygulaması) ve `--auth Windows` geçin:

```console
dotnet new webapp --auth Windows
```

---

Mevcut bir projeyi değiştirirken, proje dosyası için bir paket başvurusu içerdiğini onaylamak [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **veya** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet paketi.

## <a name="enable-windows-authentication-with-iis"></a>IIS ile Windows kimlik doğrulamasını etkinleştirme

IIS kullanan [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) konak ASP.NET Core uygulamaları için. Windows kimlik doğrulaması için IIS yapılandırılır *web.config* dosya. Aşağıdaki bölümlerde show nasıl yapılır:

* Yerel bir sağlamak *web.config* dosya uygulama dağıtıldığında, sunucu üzerinde Windows kimlik doğrulamasını etkinleştirir.
* IIS Yöneticisi'ni yapılandırmak için kullanın *web.config* dosyası sunucuda zaten dağıtılmış bir ASP.NET Core uygulaması.

### <a name="iis-configuration"></a>IIS yapılandırması

Zaten yapmadıysanız, IIS için ASP.NET Core uygulamaları barındırın etkinleştirin. Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/index>.

Windows kimlik doğrulaması için IIS rolü hizmetini etkinleştirin. Daha fazla bilgi için [IIS rol hizmetlerini (bkz. 2. adım), Windows kimlik doğrulamasını etkinleştir](xref:host-and-deploy/iis/index#iis-configuration).

IIS tümleştirme ara yazılımı varsayılan olarak, varsayılan olarak otomatik olarak isteklerinde kimlik doğrulamak üzere yapılandırılır. Daha fazla bilgi için [ana bilgisayar Windows IIS üzerinde ASP.NET Core: IIS seçeneklerini (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

ASP.NET Core modülü varsayılan olarak, varsayılan olarak Windows kimlik doğrulaması belirteci uygulamaya iletmek için yapılandırılır. Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu: AspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Yeni bir IIS sitesi oluşturun

Bir ad ve klasör belirtin ve yeni bir uygulama havuzu oluşturmak için izin verin.

### <a name="enable-windows-authentication-for-the-app-in-iis"></a>IIS uygulama için Windows kimlik doğrulamasını etkinleştirme

Kullanım **ya da** aşağıdaki yaklaşımlardan biri:

* [Uygulamayı yayımlamadan önce geliştirme tarafı Yapılandırması](#development-side-configuration-with-a-local-webconfig-file) (*önerilen*)
* [Uygulamayı yayımladıktan sonra sunucu tarafı yapılandırması](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a>Yerel web.config dosyasıyla geliştirme tarafında yapılandırma

Aşağıdaki adımları gerçekleştirin **önce** , [projenizi dağıtma ve yayımlama](#publish-and-deploy-your-project-to-the-iis-site-folder).

Aşağıdaki *web.config* proje kök dosya:

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

Proje SDK'sı tarafından yayımlanmıştır ne zaman (olmadan `<IsTransformWebConfigDisabled>` özelliğini `true` proje dosyasında), yayımlanan *web.config* dosyasını içeren `<location><system.webServer><security><authentication>` bölümü. Daha fazla bilgi için `<IsTransformWebConfigDisabled>` özelliği bkz <xref:host-and-deploy/iis/index#webconfig-file>.

#### <a name="server-side-configuration-with-the-iis-manager"></a>IIS Yöneticisi ile sunucu tarafı yapılandırması

Aşağıdaki adımları gerçekleştirin **sonra** , [projenizi dağıtma ve yayımlama](#publish-and-deploy-your-project-to-the-iis-site-folder).

1. IIS Yöneticisi'nde IIS sitesi altında seçin **siteleri** düğümünün **bağlantıları** kenar çubuğu.
1. Çift **kimlik doğrulaması** içinde **IIS** alan.
1. Seçin **anonim kimlik doğrulaması**. Seçin **devre dışı** içinde **eylemleri** kenar çubuğu.
1. Seçin **Windows kimlik doğrulaması**. Seçin **etkinleştirme** içinde **eylemleri** kenar çubuğu.

Bu eylemler gerçekleştirildikçe, IIS Yöneticisi'ni uygulamanın değiştirir *web.config* dosya. A `<system.webServer><security><authentication>` düğümü için güncelleştirilmiş ayarlarla eklenir `anonymousAuthentication` ve `windowsAuthentication`:

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

`<system.webServer>` Bölümüne eklenen *web.config* dosyasıdır IIS Yöneticisi tarafından uygulamanın dışında `<location>` uygulama yayımlandığında, .NET Core SDK'sı tarafından eklenen bölümü. Bölümün dışında eklendiğinden `<location>` düğümünü ayarları tarafından devralınır [alt uygulamaları](xref:host-and-deploy/iis/index#sub-applications) geçerli uygulama. Devralma önlemek için ek taşıma `<security>` içine bölümünde `<location><system.webServer>` SDK'da bölümü.

IIS Yöneticisi, IIS yapılandırması eklemek için kullanıldığında, uygulamanın yalnızca etkiler *web.config* dosya sunucusunda. Uygulamanın bir sonraki dağıtım sunucudaki ayarları varsa üzerine yazabilir sunucunun kopyasını *web.config* projenin tarafından değiştirilir *web.config* dosya. Kullanım **ya da** ayarlarını yönetmek için aşağıdaki yaklaşımlardan biri:

* Ayarlar sıfırlamak için IIS Yöneticisi'ni kullanın *web.config* dağıtımı dosyanın üzerine yazılır sonra dosya.
* Ekleme bir *web.config dosyasını* uygulamada yerel olarak ayarlar. Daha fazla bilgi için [geliştirme tarafı Yapılandırması](#development-side-configuration-with-a-local-webconfig-file) bölümü.

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a>Yayımlama ve IIS site klasöre projenizi dağıtın

Visual Studio veya .NET Core CLI'yı kullanarak, yayımlayın ve hedef klasöre uygulamayı dağıtın.

IIS ile barındırma ile ilgili daha fazla bilgi için yayımlama ve dağıtım, aşağıdaki konulara bakın:

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

Windows kimlik doğrulaması çalıştığını doğrulamak için uygulamayı başlatın.

## <a name="enable-windows-authentication-with-httpsys"></a>HTTP.sys ile Windows kimlik doğrulamasını etkinleştirme

Windows kimlik doğrulaması Kestrel desteklemiyor olsa da, kullanabileceğiniz [HTTP.sys](xref:fundamentals/servers/httpsys) Windows üzerinde şirket içinde barındırılan senaryoları desteklemek için. Aşağıdaki örnek, HTTP.sys ile Windows kimlik doğrulaması kullanmak için uygulamanın web ana bilgisayarı yapılandırır:

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder. Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez. Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir. Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.

> [!NOTE]
> HTTP.sys sürüm 1709 veya üzeri Nano Sunucu'da desteklenmemektedir. Windows kimlik doğrulaması ve HTTP.sys Nano Server ile kullanmak için bir [(microsoft/windowsservercore) Server Core kapsayıcı](https://hub.docker.com/r/microsoft/windowsservercore/). Sunucu Çekirdeği hakkında daha fazla bilgi için bkz. [Windows Server'da Sunucu Çekirdeği yükleme seçeneği nedir?](/windows-server/administration/server-core/what-is-server-core).

## <a name="work-with-windows-authentication"></a>Windows kimlik doğrulaması ile çalışma

Anonim erişim yapılandırma durumunu yolla belirler `[Authorize]` ve `[AllowAnonymous]` öznitelikleri, uygulamada kullanılır. Aşağıdaki iki bölümü anonim erişime izin verilmeyen ve izin verilen yapılandırma durumunu nasıl ele alınacağını açıklar.

### <a name="disallow-anonymous-access"></a>Anonim erişime izin verme

Windows kimlik doğrulaması etkinleştirildiğinde ve adsız erişim devre dışıysa, `[Authorize]` ve `[AllowAnonymous]` özniteliklerinin etkisi yoktur. IIS sitesi (veya HTTP.sys), anonim erişime izin vermeyecek şekilde yapılandırıldığından, istek, uygulamanızı hiçbir zaman ulaşır. Bu nedenle, `[AllowAnonymous]` özniteliği geçerli değil.

### <a name="allow-anonymous-access"></a>Anonim erişime izin ver

Windows kimlik doğrulaması hem anonim erişim etkin olduğunda, kullanmak `[Authorize]` ve `[AllowAnonymous]` öznitelikleri. `[Authorize]` Özniteliği gerçekten Windows kimlik doğrulaması gerektiren uygulama parçalarını güvenli olanak tanır. `[AllowAnonymous]` Öznitelik geçersiz kılmalarını `[Authorize]` anonim erişime izin veren uygulamaların içindeki kullanım özniteliği. Bkz: [basit yetkilendirme](xref:security/authorization/simple) özniteliği kullanım ayrıntıları için.

ASP.NET core'da 2.x `[Authorize]` öznitelik ek yapılandırma gerektirir *Startup.cs* anonim istekler için Windows kimlik doğrulaması meydan okuyun. Önerilen yapılandırma kullanılan web sunucusu göre biraz farklılık gösterir.

> [!NOTE]
> Varsayılan olarak, yetersiz bir sayfaya erişmek için yetkilendirme kullanıcılar ile boş bir HTTP 403 yanıtı sunulur. [StatusCodePages ara yazılım](xref:fundamentals/error-handling#usestatuscodepages) kullanıcılar daha iyi bir "Erişim engellendi" deneyimi sunmak için yapılandırılabilir.

#### <a name="iis"></a>IIS

IIS kullanarak aşağıdakileri ekleyin `ConfigureServices` yöntemi:

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

HTTP.sys kullanıyorsanız, ekleyin `ConfigureServices` yöntemi:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Kimliğe bürünme

ASP.NET Core, kimliğe bürünme uygulamaz. Uygulamalar, uygulama havuzu veya işlem kimliğini kullanarak, tüm istekler için uygulamanın kimlik ile çalıştırın. Açıkça bir kullanıcı adına eylem gerçekleştirmek ihtiyacınız varsa [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) içinde bir [terminal satır içi ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) içinde `Startup.Configure`. Bu bağlamda tek bir eylem çalıştırın ve ardından bağlam kapatın.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` zaman uyumsuz işlemleri desteklemeyen ve karmaşık senaryolar için kullanılmamalıdır. Örneğin, tüm istekleri veya bir ara yazılım zincirleri sarmalama desteklenen önerilen veya değil.

### <a name="claims-transformations"></a>Talep dönüştürmeleri

IIS işlem içi moduyla barındırırken <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak bir kullanıcı başlatmak için çağırılır değil. Bu nedenle, bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması varsayılan olarak etkinleştirilmez sonra talepleri dönüştürmek için kullanılan uygulama. Daha fazla bilgi ve barındırma işlemi içinde talep dönüştürmeleri etkinleştiren bir kod örneği için bkz: <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.
