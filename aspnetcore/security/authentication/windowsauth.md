---
title: ASP.NET Core Windows kimlik doğrulamasını yapılandırma
author: ardalis
description: Bu makalede, IIS Express, IIS, HTTP.sys ve WebListener kullanarak ASP.NET Core Windows kimlik doğrulaması yapılandırma açıklanır.
ms.author: riande
ms.date: 08/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: a8066d248c0d4db1d1f61b2a14bdb4656a2f4265
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312418"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>ASP.NET Core Windows kimlik doğrulamasını yapılandırma

Tarafından [Steve Smith](https://ardalis.com) ve [Scott Addie](https://twitter.com/Scott_Addie)

Windows kimlik doğrulaması, IIS ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [HTTP.sys](xref:fundamentals/servers/httpsys), veya [WebListener](xref:fundamentals/servers/weblistener).

## <a name="windows-authentication"></a>Windows Kimlik Doğrulaması

Windows kimlik doğrulaması, ASP.NET Core uygulamaları, kullanıcıların kimliklerini doğrulamak için işletim sistemi kullanır. Sunucunuz, kullanıcıları tanımlamak için Active Directory etki alanı kimlikleri veya diğer Windows hesaplarını kullanarak bir şirket ağında çalıştığında, Windows kimlik doğrulaması kullanabilirsiniz. Windows kimlik doğrulaması kullanıcılar, istemci uygulamaları ve web sunucuları aynı Windows etki alanına ait intranet ortamları için idealdir.

[Windows kimlik doğrulaması hakkında daha fazla bilgi edinin ve IIS yükleniyor](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>ASP.NET Core uygulaması Windows kimlik doğrulamasını etkinleştirin

Visual Studio Web uygulama şablonu, Windows kimlik doğrulamayı destekleyecek şekilde yapılandırılabilir.

### <a name="use-the-windows-authentication-app-template"></a>Windows kimlik doğrulama uygulaması şablonunu kullanma

Visual Studio'da:

1. Yeni bir ASP.NET Core Web uygulaması oluşturun.
1. Web uygulaması şablonları listesinden seçin.
1. Seçin **kimlik doğrulamayı Değiştir** düğmesini tıklatın ve seçin **Windows kimlik doğrulaması**.

Uygulamayı çalıştırın. Kullanıcı adı görünür uygulamasının sağ.

![Windows kimlik doğrulaması tarayıcı ekran görüntüsü](windowsauth/_static/browser-screenshot.png)

IIS Express kullanarak, geliştirme iş için Windows kimlik doğrulamasını kullanmak gerekli tüm yapılandırma şablon sağlar. Aşağıdaki bölümde el ile ASP.NET Core uygulaması Windows kimlik doğrulaması için nasıl yapılandırılacağı gösterilmektedir.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Windows ve anonim kimlik doğrulaması için Visual Studio ayarları

Visual Studio projesini **özellikleri** sayfanın **hata ayıklama** sekmesi, Windows kimlik doğrulaması ve anonim kimlik doğrulaması için onay kutularını sağlar.

![Windows kimlik doğrulaması tarayıcı ekran görüntüsü](windowsauth/_static/vs-auth-property-menu.png)

Alternatif olarak, bu iki özellik de yapılandırılabilir *launchSettings.json* dosyası:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>IIS ile Windows kimlik doğrulamasını etkinleştirme

IIS kullanan [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) konak ASP.NET Core uygulamaları için. Windows kimlik doğrulaması, IIS, uygulama yapılandırılır. Aşağıdaki bölümlerde, ASP.NET Core uygulaması Windows kimlik doğrulaması kullanacak şekilde yapılandırmak için IIS Yöneticisi'ni kullanmayı göstermektedir.

### <a name="iis-configuration"></a>IIS yapılandırması

Windows kimlik doğrulaması için IIS rolü hizmetini etkinleştirin. Daha fazla bilgi için [IIS rol hizmetlerini (bkz. 2. adım), Windows kimlik doğrulamasını etkinleştir](xref:host-and-deploy/iis/index#iis-configuration).

IIS tümleştirme ara yazılımı varsayılan olarak, varsayılan olarak otomatik olarak isteklerinde kimlik doğrulamak üzere yapılandırılır. Daha fazla bilgi için [ana bilgisayar Windows IIS üzerinde ASP.NET Core: IIS seçenekleri (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

ASP.NET Core modülü varsayılan olarak, varsayılan olarak Windows kimlik doğrulaması belirteci uygulamaya iletmek için yapılandırılır. Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu: aspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Yeni bir IIS sitesi oluşturun

Bir ad ve klasör belirtin ve yeni bir uygulama havuzu oluşturmak için izin verin.

### <a name="customize-authentication"></a>Kimlik doğrulaması'nı özelleştirme

Sitesi için kimlik doğrulama özelliklerini açın.

![IIS kimlik doğrulaması menüsü](windowsauth/_static/iis-authentication-menu.png)

Anonim kimlik doğrulamasını devre dışı bırakın ve Windows kimlik doğrulamasını etkinleştirin.

![IIS kimlik doğrulama ayarları](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Projenizi yayımlamak için IIS site klasörü

Visual Studio veya .NET Core CLI'yı kullanarak uygulamayı hedef klasöre yayımlayın.

![Visual Studio yayımlama iletişim kutusu](windowsauth/_static/vs-publish-app.png)

Daha fazla bilgi edinin [IIS yayımlama](xref:host-and-deploy/iis/index).

Windows kimlik doğrulaması çalıştığını doğrulamak için uygulamayı başlatın.

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a>HTTP.sys ile Windows kimlik doğrulamasını etkinleştirme

Windows kimlik doğrulaması Kestrel desteklemiyor olsa da, kullanabileceğiniz [HTTP.sys](xref:fundamentals/servers/httpsys) Windows üzerinde şirket içinde barındırılan senaryoları desteklemek için. Aşağıdaki örnek, HTTP.sys ile Windows kimlik doğrulaması kullanmak için uygulamanın web ana bilgisayarı yapılandırır:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder. Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez. Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir. Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a>WebListener ile Windows kimlik doğrulamasını etkinleştirme

Windows kimlik doğrulaması Kestrel desteklemiyor olsa da, kullanabileceğiniz [WebListener](xref:fundamentals/servers/weblistener) Windows üzerinde şirket içinde barındırılan senaryoları desteklemek için. Aşağıdaki örnekte, uygulamanın web ana bilgisayarı WebListener Windows kimlik doğrulaması ile kullanılacak yapılandırır:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü WebListener temsil eder. Kullanıcı modu kimlik doğrulaması, Kerberos ve WebListener ile desteklenmez. Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir. Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.

::: moniker-end

## <a name="work-with-windows-authentication"></a>Windows kimlik doğrulaması ile çalışma

Anonim erişim yapılandırma durumunu yolla belirler `[Authorize]` ve `[AllowAnonymous]` öznitelikleri, uygulamada kullanılır. Aşağıdaki iki bölümü anonim erişime izin verilmeyen ve izin verilen yapılandırma durumunu nasıl ele alınacağını açıklar.

### <a name="disallow-anonymous-access"></a>Anonim erişime izin verme

Windows kimlik doğrulaması etkinleştirildiğinde ve adsız erişim devre dışıysa, `[Authorize]` ve `[AllowAnonymous]` özniteliklerinin etkisi yoktur. IIS sitesi (veya HTTP.sys veya WebListener sunucusu), anonim erişime izin vermeyecek şekilde yapılandırıldığından, istek, uygulamanızı hiçbir zaman ulaşır. Bu nedenle, `[AllowAnonymous]` özniteliği geçerli değil.

### <a name="allow-anonymous-access"></a>Anonim erişime izin ver

Windows kimlik doğrulaması hem anonim erişim etkin olduğunda, kullanmak `[Authorize]` ve `[AllowAnonymous]` öznitelikleri. `[Authorize]` Özniteliği gerçekten Windows kimlik doğrulaması gerektiren uygulama parçalarını güvenli olanak tanır. `[AllowAnonymous]` Öznitelik geçersiz kılmalarını `[Authorize]` anonim erişime izin veren uygulamaların içindeki kullanım özniteliği. Bkz: [basit yetkilendirme](xref:security/authorization/simple) özniteliği kullanım ayrıntıları için.

ASP.NET core'da 2.x `[Authorize]` öznitelik ek yapılandırma gerektirir *Startup.cs* anonim istekler için Windows kimlik doğrulaması meydan okuyun. Önerilen yapılandırma kullanılan web sunucusu göre biraz farklılık gösterir.

> [!NOTE]
> Varsayılan olarak, yetersiz bir sayfaya erişmek için yetkilendirme kullanıcılar ile boş bir HTTP 403 yanıtı sunulur. [StatusCodePages ara yazılım](xref:fundamentals/error-handling#configure-status-code-pages) kullanıcılar daha iyi bir "Erişim engellendi" deneyimi sunmak için yapılandırılabilir.

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

ASP.NET Core, kimliğe bürünme uygulamaz. Uygulamalar, uygulama havuzu veya işlem kimliğini kullanarak, tüm istekler için uygulama kimliği ile çalışır. Açıkça bir kullanıcı adına eylem gerçekleştirmek ihtiyacınız varsa `WindowsIdentity.RunImpersonated`. Bu bağlamda tek bir eylem çalıştırın ve ardından bağlam kapatın.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Unutmayın `RunImpersonated` zaman uyumsuz işlemleri desteklemeyen ve karmaşık senaryolar için kullanılmamalıdır. Örneğin, tüm istekleri veya bir ara yazılım zincirleri sarmalama desteklenen önerilen veya değil.
