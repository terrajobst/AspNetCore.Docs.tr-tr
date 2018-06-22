---
title: ASP.NET Core Windows kimlik doğrulamasını yapılandırma
author: ardalis
description: Bu makalede, ASP.NET çekirdek IIS Express, IIS, HTTP.sys ve WebListener kullanarak, Windows kimlik doğrulamasını yapılandırmak açıklar.
ms.author: riande
ms.date: 10/24/2017
uid: security/authentication/windowsauth
ms.openlocfilehash: d3fdf8534e92ea306c2fef7bc4fda6d644c7d911
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276217"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>ASP.NET Core Windows kimlik doğrulamasını yapılandırma

Tarafından [Steve Smith](https://ardalis.com) ve [Scott Addie](https://twitter.com/Scott_Addie)

Windows kimlik doğrulaması, IIS ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [HTTP.sys](xref:fundamentals/servers/httpsys), veya [WebListener](xref:fundamentals/servers/weblistener).

## <a name="what-is-windows-authentication"></a>Windows kimlik doğrulaması nedir?

Windows kimlik doğrulaması ASP.NET Core uygulamaları kullanıcıların kimliklerini işletim sistemine bağlıdır. Sunucunuzu kullanıcıları tanımlamak için Active Directory etki alanı kimlikleri veya diğer Windows hesaplarını kullanarak bir şirket ağında çalıştığında, Windows kimlik doğrulaması kullanabilirsiniz. Windows kimlik doğrulaması kullanıcılar, istemci uygulamaları ve web sunucuları aynı Windows etki alanına ait intranet ortamları için uygundur.

[Windows kimlik doğrulaması ve IIS için yükleme hakkında daha fazla bilgi](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Bir ASP.NET Core uygulamasında Windows kimlik doğrulamasını etkinleştir

Visual Studio Web uygulaması şablonu, Windows kimlik doğrulamayı destekleyecek şekilde yapılandırılabilir.

### <a name="use-the-windows-authentication-app-template"></a>Windows kimlik doğrulaması uygulama şablonu kullanın

Visual Studio'da:
1. Yeni bir ASP.NET çekirdek Web uygulaması oluşturun. 
1. Web uygulaması şablonları listesinden seçin.
1. Seçin **kimlik doğrulamayı Değiştir** düğmesine tıklayın ve ardından **Windows kimlik doğrulaması**. 

Uygulamayı çalıştırın. Kullanıcı adı görünür uygulama sağında.

![Windows kimlik doğrulaması tarayıcı ekran görüntüsü](windowsauth/_static/browser-screenshot.png)

IIS Express kullanarak geliştirme iş için Windows kimlik doğrulaması kullanmak için gerekli tüm yapılandırma şablonu sağlar. Aşağıdaki bölümde el ile bir ASP.NET Core uygulama Windows kimlik doğrulaması için nasıl yapılandırılacağını gösterir.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Windows ve anonim kimlik doğrulaması için Visual Studio ayarları

Visual Studio projesi **özellikleri** sayfanın **hata ayıklama** sekmesi, Windows kimlik doğrulaması ve anonim kimlik doğrulaması için onay kutularını sağlar.

![Windows kimlik doğrulaması tarayıcı ekran görüntüsü](windowsauth/_static/vs-auth-property-menu.png)

Alternatif olarak, bu iki özellik de yapılandırılabilir *launchSettings.json* dosyası:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>IIS Windows kimlik doğrulamasını etkinleştirme

IIS kullanan [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) konak ASP.NET Core uygulamaları için. Modül IIS için varsayılan akış için Windows kimlik doğrulaması sağlar. Windows kimlik doğrulaması, IIS, uygulama yapılandırılır. Aşağıdaki bölümlerde ASP.NET Core uygulamayı Windows kimlik doğrulaması kullanacak şekilde yapılandırmak için IIS Yöneticisi'ni kullanmayı gösterir.

### <a name="create-a-new-iis-site"></a>Yeni bir IIS sitesi oluştur

Bir ad ve klasörü belirtin ve yeni bir uygulama havuzu oluşturmak izin verin.

### <a name="customize-authentication"></a>Kimlik doğrulama özelleştirme

Site için kimlik doğrulama menüsünü açın.

![IIS kimlik doğrulaması menüsü](windowsauth/_static/iis-authentication-menu.png)

Anonim kimlik doğrulamasını devre dışı bırakın ve Windows kimlik doğrulamasını etkinleştirin.

![IIS kimlik doğrulama ayarları](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>IIS site klasörüne projenizin Yayımla

Visual Studio veya .NET Core CLI kullanarak, hedef klasöre uygulamayı yayımlayın.

![Visual Studio yayımlama iletişim kutusu](windowsauth/_static/vs-publish-app.png)

Daha fazla bilgi edinmek [IIS yayımlama](xref:host-and-deploy/iis/index).

Windows kimlik doğrulaması çalıştığını doğrulamak için uygulamayı başlatın.

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>HTTP.sys veya WebListener Windows kimlik doğrulamasını etkinleştirme

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Kestrel Windows kimlik doğrulamasını desteklemez, ancak kullanabilirsiniz [HTTP.sys](xref:fundamentals/servers/httpsys) Windows kendini barındıran senaryoları desteklemek için. Aşağıdaki örnek HTTP.sys Windows kimlik doğrulaması ile kullanmak için uygulamanın web ana bilgisayarı yapılandırır:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Kestrel Windows kimlik doğrulamasını desteklemez, ancak kullanabilirsiniz [WebListener](xref:fundamentals/servers/weblistener) Windows kendini barındıran senaryoları desteklemek için. Aşağıdaki örnek WebListener Windows kimlik doğrulaması ile kullanmak için uygulamanın web ana bilgisayarı yapılandırır:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>Windows kimlik doğrulaması ile çalışma

Anonim erişim yapılandırma durumunu şekilde belirler `[Authorize]` ve `[AllowAnonymous]` öznitelikleri uygulamada kullanılır. Aşağıdaki iki bölümü anonim erişime izin verilmeyen ve izin verilen yapılandırma durumlarını nasıl ele alınacağını açıklamaktadır.

### <a name="disallow-anonymous-access"></a>Anonim erişime izin verme

Windows kimlik doğrulaması etkinleştirildi ve adsız erişim devre dışıysa, `[Authorize]` ve `[AllowAnonymous]` öznitelikleri etkisi yoktur. IIS sitesi (veya HTTP.sys veya WebListener sunucusu) anonim erişime izin vermeyecek şekilde yapılandırılmışsa, istek uygulamanıza hiçbir zaman ulaşır. Bu nedenle, `[AllowAnonymous]` özniteliği geçerli değil.

### <a name="allow-anonymous-access"></a>Anonim erişime izin ver

Windows kimlik doğrulaması ve anonim erişim etkin olduğunda, kullanın `[Authorize]` ve `[AllowAnonymous]` öznitelikleri. `[Authorize]` Özniteliği gerçekten Windows kimlik doğrulaması gerektiren uygulama parçalarını güvenli olanak tanır. `[AllowAnonymous]` Özniteliği geçersiz kılmaları `[Authorize]` özniteliği anonim erişime izin veren uygulamaların içindeki kullanım. Bkz: [basit yetkilendirme](xref:security/authorization/simple) özniteliği kullanım ayrıntıları için.

ASP.NET Core içinde 2.x `[Authorize]` öznitelik, bir ek yapılandırma gerektirir *haline* Windows kimlik doğrulaması için anonim isteklere sınama için. Önerilen yapılandırma kullanılan web sunucusu göre biraz değişir.

> [!NOTE]
> Varsayılan olarak, bir sayfaya erişmek için yetkilendirme eksikliği kullanıcılar ile boş bir HTTP 403 yanıt sunulur. [StatusCodePages Ara](xref:fundamentals/error-handling#configuring-status-code-pages) kullanıcıları daha iyi bir "Erişim engellendi" deneyimi sunmak için yapılandırılabilir.

#### <a name="iis"></a>IIS

IIS kullanıyorsanız, aşağıdaki ekleyin `ConfigureServices` yöntemi: 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

HTTP.sys kullanıyorsanız, aşağıdaki ekleyin `ConfigureServices` yöntemi:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Kimliğe bürünme

ASP.NET Core kimliğe bürünme uygulamaz. Uygulamalar, uygulama havuzu veya işlem kimliğini kullanarak, tüm istekleri için uygulama kimliği ile çalıştırın. Açıkça bir kullanıcı adına bir eylem gerçekleştirmeniz gerekirse, kullanmak `WindowsIdentity.RunImpersonated`. Bu bağlamda tek bir eylem çalıştırın ve ardından bağlam kapatın.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Unutmayın `RunImpersonated` zaman uyumsuz işlemleri desteklemez ve karmaşık senaryolar için kullanılmaması gerekir. Örneğin, tüm istekleri ya da Ara yazılım zincirleri kaydırma desteklenen önerilen veya değil.

---
