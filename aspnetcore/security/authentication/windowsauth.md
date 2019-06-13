---
title: ASP.NET Core Windows kimlik doğrulamasını yapılandırma
author: scottaddie
description: Windows kimlik doğrulaması için IIS ve HTTP.sys içinde ASP.NET Core yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/12/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 93f833adff95f25d570947cd1a9035d652f522c2
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034957"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>ASP.NET Core Windows kimlik doğrulamasını yapılandırma

Tarafından [Scott Addie](https://twitter.com/Scott_Addie) ve [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

Windows kimlik doğrulaması (anlaşma, Kerberos veya NTLM kimlik olarak da bilinir) ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), veya [HTTP.sys](xref:fundamentals/servers/httpsys) .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Windows kimlik doğrulaması (anlaşma, Kerberos veya NTLM kimlik olarak da bilinir) ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [IIS](xref:host-and-deploy/iis/index) veya [HTTP.sys](xref:fundamentals/servers/httpsys).

::: moniker-end

Windows kimlik doğrulaması, ASP.NET Core uygulamaları, kullanıcıların kimliklerini doğrulamak için işletim sistemi kullanır. Sunucunuz, kullanıcıları tanımlamak için Active Directory etki alanı kimlikleri veya Windows hesaplarını kullanarak bir şirket ağında çalıştığında, Windows kimlik doğrulaması kullanabilirsiniz. Windows kimlik doğrulaması nerede kullanıcılar, istemci uygulamaları ve web sunucuları aynı Windows etki alanına ait intranet ortamları için idealdir.

> [!NOTE]
> Windows kimlik doğrulaması, HTTP/2 ile desteklenmiyor. Kimlik doğrulama sınaması, HTTP/2 yanıtları gönderilebilir, ancak önce kimlik doğrulaması, istemci HTTP/1.1 düşürme gerekir.

## <a name="iisiis-express"></a>IIS/IIS Express

Kimlik doğrulama hizmetleri çağırarak ekleme <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> ad alanı) içinde `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a>Başlatma ayarları (hata ayıklayıcı)

Başlatma ayarları yapılandırması yalnızca etkiler *Properties/launchSettings.json* dosya için IIS Express ve IIS için Windows kimlik doğrulaması yapılandırmaz. Sunucu Yapılandırması içinde açıklanan [IIS](#iis) bölümü.

**Web uygulaması** şablonu Visual Studio veya .NET Core CLI aracılığıyla kullanılabilir, Windows kimlik doğrulamasını güncelleştiren destekleyecek şekilde yapılandırılabilir *Properties/launchSettings.json* dosyası otomatik olarak.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**Yeni Proje**

1. Yeni bir proje oluşturun.
1. Seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.
1. Bir ad sağlayın **proje adı** alan. Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın. **Oluştur**’u seçin.
1. Seçin **değişiklik** altında **kimlik doğrulaması**.
1. İçinde **kimlik doğrulamayı Değiştir** penceresinde **Windows kimlik doğrulaması**. **Tamam**’ı seçin.
1. Seçin **Web uygulaması**.
1. **Oluştur**’u seçin.

Uygulamayı çalıştırın. Kullanıcı işlenen uygulamanın kullanıcı arabiriminde görüntülenir.

**Mevcut proje**

Projenin özelliklerini Windows kimlik doğrulamasını etkinleştirmek ve anonim kimlik doğrulamasını devre dışı bırakın:

1. Projeye sağ **Çözüm Gezgini** seçip **özellikleri**.
1. Seçin **hata ayıklama** sekmesi.
1. Onay kutusunu temizleyin **anonim kimlik doğrulamasını etkinleştir**.
1. Onay kutusunu seçin **Windows kimlik doğrulamasını etkinleştir**.
1. Kaydet ve özellik sayfasını kapatın.

Alternatif olarak, özellikler, yapılandırılabilir `iisSettings` düğümünün *launchSettings.json* dosyası:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

**Yeni Proje**

Yürütme [yeni dotnet](/dotnet/core/tools/dotnet-new) komutunu `webapp` bağımsız değişken (ASP.NET Core Web uygulaması) ve `--auth Windows` geçin:

```console
dotnet new webapp --auth Windows
```

**Mevcut proje**

Güncelleştirme `iisSettings` düğümünün *launchSettings.json* dosyası:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

Mevcut bir projeyi değiştirirken, proje dosyası için bir paket başvurusu içerdiğini onaylamak [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **veya** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet paketi.

### <a name="iis"></a>IIS

IIS kullanan [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) konak ASP.NET Core uygulamaları için. Windows kimlik doğrulaması için IIS yapılandırılır *web.config* dosya. Aşağıdaki bölümlerde show nasıl yapılır:

* Yerel bir sağlamak *web.config* dosya uygulama dağıtıldığında, sunucu üzerinde Windows kimlik doğrulamasını etkinleştirir.
* IIS Yöneticisi'ni yapılandırmak için kullanın *web.config* dosyası sunucuda zaten dağıtılmış bir ASP.NET Core uygulaması.

Zaten yapmadıysanız, IIS için ASP.NET Core uygulamaları barındırın etkinleştirin. Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/index>.

Windows kimlik doğrulaması için IIS rolü hizmetini etkinleştirin. Daha fazla bilgi için [IIS rol hizmetlerini (bkz. 2. adım), Windows kimlik doğrulamasını etkinleştir](xref:host-and-deploy/iis/index#iis-configuration).

[IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) otomatik olarak isteklerinin kimliğini doğrulamak için varsayılan olarak yapılandırılır. Daha fazla bilgi için [ana bilgisayar Windows IIS üzerinde ASP.NET Core: IIS seçeneklerini (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

ASP.NET Core modülü varsayılan olarak, varsayılan olarak Windows kimlik doğrulaması belirteci uygulamaya iletmek için yapılandırılır. Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu: AspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

Kullanım **ya da** aşağıdaki yaklaşımlardan biri:

* **Projeyi dağıtma ve yayımlama önce** aşağıdaki *web.config* proje kök dosya:

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  Proje .NET Core SDK'sı tarafından yayımlanmıştır ne zaman (olmadan `<IsTransformWebConfigDisabled>` özelliğini `true` proje dosyasında), yayımlanan *web.config* dosyasını içeren `<location><system.webServer><security><authentication>` bölümü. Daha fazla bilgi için `<IsTransformWebConfigDisabled>` özelliği bkz <xref:host-and-deploy/iis/index#webconfig-file>.

* **Sonra projeyi dağıtma ve yayımlama** sunucu tarafı yapılandırması IIS Yöneticisi ile gerçekleştirin:

  1. IIS Yöneticisi'nde IIS sitesi altında seçin **siteleri** düğümünün **bağlantıları** kenar çubuğu.
  1. Çift **kimlik doğrulaması** içinde **IIS** alan.
  1. Seçin **anonim kimlik doğrulaması**. Seçin **devre dışı** içinde **eylemleri** kenar çubuğu.
  1. Seçin **Windows kimlik doğrulaması**. Seçin **etkinleştirme** içinde **eylemleri** kenar çubuğu.

  Bu eylemler gerçekleştirildikçe, IIS Yöneticisi'ni uygulamanın değiştirir *web.config* dosya. A `<system.webServer><security><authentication>` düğümü için güncelleştirilmiş ayarlarla eklenir `anonymousAuthentication` ve `windowsAuthentication`:

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  `<system.webServer>` Bölümüne eklenen *web.config* dosyasıdır IIS Yöneticisi tarafından uygulamanın dışında `<location>` uygulama yayımlandığında, .NET Core SDK'sı tarafından eklenen bölümü. Bölümün dışında eklendiğinden `<location>` düğümünü ayarları tarafından devralınır [alt uygulamaları](xref:host-and-deploy/iis/index#sub-applications) geçerli uygulama. Devralma önlemek için ek taşıma `<security>` içine bölümünde `<location><system.webServer>` .NET Core SDK'sını sağlanan bölüm.

  IIS Yöneticisi, IIS yapılandırması eklemek için kullanıldığında, uygulamanın yalnızca etkiler *web.config* dosya sunucusunda. Uygulamanın bir sonraki dağıtım sunucudaki ayarları varsa üzerine yazabilir sunucunun kopyasını *web.config* projenin tarafından değiştirilir *web.config* dosya. Kullanım **ya da** ayarlarını yönetmek için aşağıdaki yaklaşımlardan biri:

  * Ayarlar sıfırlamak için IIS Yöneticisi'ni kullanın *web.config* dağıtımı dosyanın üzerine yazılır sonra dosya.
  * Ekleme bir *web.config dosyasını* uygulamada yerel olarak ayarlar.

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a>Kestrel

 [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet paketi ile kullanılabilir [Kestrel](xref:fundamentals/servers/kestrel) Windows Windows, Linux ve Macos'ta anlaşma, Kerberos ve NTLM kullanarak kimlik doğrulamasını desteklemek için.

> [!WARNING]
> Kimlik bilgileri bağlantı istekleri arasında kalıcı. *Anlaşma kimlik doğrulaması değil kullanılmalıdır proxy'leriyle sürece proxy Kestrel ile 1:1 bağlantı benzeşimi (kalıcı bir bağlantı) korur.* Anlaşma kimlik doğrulamasını Kestrel arkasında IIS ile kullanılmamalıdır, yani [ASP.NET Core Modülü (ANCM) giden işlem](xref:host-and-deploy/iis/index#out-of-process-hosting-model).

 Kimlik doğrulama hizmetleri çağırarak ekleme <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` ad alanı) ve `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` ad alanı) içinde `Startup.ConfigureServices`:

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

Kimlik doğrulaması ara yazılımı çağırarak ekleme <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> içinde `Startup.Configure`:

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

Ara yazılım hakkında daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.

Anonim isteklere izin verilir. Kullanım [ASP.NET Core yetkilendirme](xref:security/authorization/introduction) kimlik doğrulaması için anonim isteklere meydan okuyun.

### <a name="windows-environment-configuration"></a>Windows ortamını yapılandırma

[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) bileşeni kullanıcı modu kimlik doğrulaması gerçekleştirir. Hizmet asıl adları (SPN) makine hesabı değil hizmetini çalıştıran kullanıcı hesabına eklenmelidir. Yürütme `setspn -S HTTP/mysrevername.mydomain.com myuser` bir yönetim komut kabuğu'nda.

### <a name="linux-and-macos-environment-configuration"></a>Linux ve Macos'ta ortamı yapılandırma

Bir Linux veya Macos'ta makine bir Windows etki alanına katmak için yönergeler kullanılabilir [Azure veri Studio Windows kimlik doğrulaması - Kerberos kullanarak SQL sunucunuza bağlanmak](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) makalesi. Yönergeleri Linux makinesi için bir makine hesabı etki alanında oluşturun. Bu makine hesabı için SPN eklenmesi gerekir.

> [!NOTE]
> ' Deki yönergeleri takip ederken [Azure veri Studio Windows kimlik doğrulaması - Kerberos kullanarak SQL sunucunuza bağlanmak](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) makalesi, yerine `python-software-properties` ile `python3-software-properties` gerekirse.

Linux veya Macos'ta makine etki alanına katılmış sonra sağlamak için ek adımlar gereklidir bir [anahtar tablosu dosya](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) SPN'ler ile:

* Etki alanı denetleyicisinde makine hesabı için yeni bir web hizmeti SPN'ler ekleyin:
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* Kullanım [ktpass](/windows-server/administration/windows-commands/ktpass) anahtar tablosu dosyası oluşturmak için:
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * Bazı alanları belirtilmelidir gösterildiği gibi büyük.
* Anahtar tablosu dosyasını Linux veya Macos'ta makineye kopyalayın.
* Bir ortam değişkeni aracılığıyla anahtar tablosu dosyayı seçin: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`
* Çağırma `klist` şu anda kullanılabilir SPN'ler gösterilecek.

> [!NOTE]
> Bir anahtar tablosu dosyası, etki alanı erişim kimlik bilgileri içeriyor ve uygun şekilde korunması gerekir.

::: moniker-end

## <a name="httpsys"></a>HTTP.sys

[HTTP.sys](xref:fundamentals/servers/httpsys) çekirdek modu Windows Negotiate, NTLM veya temel kimlik doğrulamasını kullanarak kimlik doğrulaması destekler.

Kimlik doğrulama hizmetleri çağırarak ekleme <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> ad alanı) içinde `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

Uygulamanın web ana HTTP.sys Windows kimlik doğrulaması için yapılandırma (*Program.cs*). <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> içinde <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> ad alanı.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder. Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez. Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir. Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.

> [!NOTE]
> HTTP.sys sürüm 1709 veya üzeri Nano Sunucu'da desteklenmemektedir. Windows kimlik doğrulaması ve HTTP.sys Nano Server ile kullanmak için bir [(microsoft/windowsservercore) Server Core kapsayıcı](https://hub.docker.com/r/microsoft/windowsservercore/). Sunucu Çekirdeği hakkında daha fazla bilgi için bkz. [Windows Server'da Sunucu Çekirdeği yükleme seçeneği nedir?](/windows-server/administration/server-core/what-is-server-core).

## <a name="authorize-users"></a>Kullanıcıları yetkilendirme

Anonim erişim yapılandırma durumunu yolla belirler `[Authorize]` ve `[AllowAnonymous]` öznitelikleri, uygulamada kullanılır. Aşağıdaki iki bölümü anonim erişime izin verilmeyen ve izin verilen yapılandırma durumunu nasıl ele alınacağını açıklar.

### <a name="disallow-anonymous-access"></a>Anonim erişime izin verme

Windows kimlik doğrulaması etkinleştirildiğinde ve adsız erişim devre dışıysa, `[Authorize]` ve `[AllowAnonymous]` özniteliklerinin etkisi yoktur. İstek, hiçbir zaman bir IIS sitesi anonim erişime izin vermeyecek şekilde yapılandırılmışsa, uygulama ulaşır. Bu nedenle, `[AllowAnonymous]` özniteliği geçerli değil.

### <a name="allow-anonymous-access"></a>Anonim erişime izin ver

Windows kimlik doğrulaması hem anonim erişim etkin olduğunda, kullanmak `[Authorize]` ve `[AllowAnonymous]` öznitelikleri. `[Authorize]` Özniteliği güvenli kimlik doğrulaması gerektiren uygulama uç olanak tanır. `[AllowAnonymous]` Öznitelik geçersiz kılmalarını `[Authorize]` anonim erişime izin veren uygulamalarında öznitelik. Öznitelik kullanım ayrıntıları için bkz. <xref:security/authorization/simple>.

> [!NOTE]
> Varsayılan olarak, yetersiz bir sayfaya erişmek için yetkilendirme kullanıcılar ile boş bir HTTP 403 yanıtı sunulur. [StatusCodePages ara yazılım](xref:fundamentals/error-handling#usestatuscodepages) kullanıcılar daha iyi bir "Erişim engellendi" deneyimi sunmak için yapılandırılabilir.

## <a name="impersonation"></a>Kimliğe bürünme

ASP.NET Core, kimliğe bürünme uygulamaz. Uygulamalar, uygulama havuzu veya işlem kimliğini kullanarak, tüm istekler için uygulamanın kimlik ile çalıştırın. Uygulamanın kullanıcı adına bir eylem gerçekleştirmeniz gereken kullanırsanız [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) içinde bir [terminal satır içi ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) içinde `Startup.Configure`. Bu bağlamda tek bir eylem çalıştırın ve ardından bağlam kapatın.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` zaman uyumsuz işlemleri desteklemeyen ve karmaşık senaryolar için kullanılmamalıdır. Örneğin, tüm istekleri veya bir ara yazılım zincirleri sarmalama desteklenen önerilen veya değil.

::: moniker range=">= aspnetcore-3.0"

Sırada [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) paket, Windows kimlik doğrulamasını etkinleştirir, Linux ve Macos'ta kimliğe bürünme yalnızca Windows üzerinde desteklenir.

::: moniker-end

## <a name="claims-transformations"></a>Talep dönüştürmeleri

IIS işlem içi moduyla barındırırken <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak bir kullanıcı başlatmak için çağırılır değil. Bu nedenle, bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması varsayılan olarak etkinleştirilmez sonra talepleri dönüştürmek için kullanılan uygulama. Daha fazla bilgi ve barındırma işlemi içinde talep dönüştürmeleri etkinleştiren bir kod örneği için bkz: <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.

## <a name="additional-resources"></a>Ek kaynaklar

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
