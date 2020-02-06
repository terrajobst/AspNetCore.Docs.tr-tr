---
title: Windows hizmetinde konak ASP.NET Core
author: guardrex
description: ASP.NET Core uygulamasının bir Windows hizmetinde nasıl barındıralınacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/06/2020
uid: host-and-deploy/windows-service
ms.openlocfilehash: 71f7bf3f5dcf8068d0ada03675ef7948267b79f4
ms.sourcegitcommit: bd896935e91236e03241f75e6534ad6debcecbbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77044890"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Windows hizmetinde konak ASP.NET Core

[Luke Latham](https://github.com/guardrex) tarafından

Bir ASP.NET Core uygulaması, IIS kullanmadan Windows [hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) olarak Windows üzerinde barındırılabilir. Windows hizmeti olarak barındırıldığı zaman, uygulama otomatik olarak sunucu yeniden başlatıldıktan sonra başlatılır.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

* [ASP.NET Core SDK 2,1 veya üzeri](https://dotnet.microsoft.com/download)
* [PowerShell 6,2 veya üzeri](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Çalışan hizmeti şablonu

ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar. Şablonu bir Windows Hizmeti uygulamasının temeli olarak kullanmak için:

1. .NET Core şablonundan bir çalışan hizmeti uygulaması oluşturun.
1. Çalışan hizmeti uygulamasını bir Windows hizmeti olarak çalışacak şekilde güncelleştirmek için [uygulama yapılandırma](#app-configuration) bölümündeki yönergeleri izleyin.

[!INCLUDE[](~/includes/worker-template-instructions.md)]

::: moniker-end

## <a name="app-configuration"></a>Uygulama yapılandırması

::: moniker range=">= aspnetcore-3.0"

Uygulama, [Microsoft. Extensions. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices)için bir paket başvurusu gerektirir.

`IHostBuilder.UseWindowsService` ana bilgisayar oluşturulurken çağrılır. Uygulama bir Windows hizmeti olarak çalışıyorsa, yöntemi:

* Ana bilgisayar ömrünü `WindowsServiceLifetime`olarak ayarlar.
* [İçerik kökünü](xref:fundamentals/index#content-root) [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory)olarak ayarlar. Daha fazla bilgi için [geçerli dizin ve içerik kökü](#current-directory-and-content-root) bölümüne bakın.
* Olay günlüğüne kaydetmeyi sağlar:
  * Uygulama adı varsayılan kaynak adı olarak kullanılır.
  * Varsayılan günlük düzeyi, ana bilgisayarı oluşturmak için `CreateDefaultBuilder` çağıran ASP.NET Core şablona dayalı bir uygulama için *Uyarı* veya daha yüksek bir uygulamadır.
  * Varsayılan günlük düzeyini *appSettings. json*/appsettings içindeki `Logging:EventLog:LogLevel:Default` anahtarıyla geçersiz kılın *. { Environment}. JSON* veya diğer yapılandırma sağlayıcısı.
  * Yeni olay kaynakları yalnızca yöneticiler tarafından oluşturulabilir. Uygulama adı kullanılarak bir olay kaynağı oluşturuoluşturumadığında, *uygulama* kaynağına bir uyarı kaydedilir ve olay günlükleri devre dışı bırakılır.

*Program.cs*`CreateHostBuilder`:

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

Aşağıdaki örnek uygulamalar bu konuya eşlik eder:

* Arka plan çalışan hizmeti örneği, arka plan görevleri için [barındırılan Hizmetleri](xref:fundamentals/host/hosted-services) kullanan [çalışan hizmeti şablonunu](#worker-service-template) temel alan, Web olmayan bir uygulama örneğini &ndash;.
* Web App Service örneği, arka plan görevleri için [barındırılan hizmetlerle](xref:fundamentals/host/hosted-services) Windows hizmeti olarak çalışan bir Razor pages Web uygulaması örneği &ndash;.

MVC Kılavuzu için <xref:mvc/overview> ve <xref:migration/22-to-30>altındaki makalelere bakın.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Uygulama [Microsoft. AspNetCore. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) ve [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog)için paket başvuruları gerektirir.

Bir hizmetin dışında çalışırken test ve hata ayıklamak için, uygulamanın bir hizmet olarak mı yoksa bir konsol uygulaması mi çalıştığını belirleme kodu ekleyin. Hata ayıklayıcının ekli olduğunu veya `--console` bir anahtarın mevcut olup olmadığını denetleyin. Her iki koşul de geçerliyse (uygulama hizmet olarak çalıştırılmadıysa) <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>çağırın. Koşullar yanlışsa (uygulama bir hizmet olarak çalıştırılır):

* <xref:System.IO.Directory.SetCurrentDirectory*> çağırın ve uygulamanın yayımlanan konumunun yolunu kullanın. Bir Windows hizmeti uygulaması, <xref:System.IO.Directory.GetCurrentDirectory*> çağrıldığında *C:\\windows\\system32* klasörünü döndürdüğünden yolu almak için <xref:System.IO.Directory.GetCurrentDirectory*> çağırmayın. Daha fazla bilgi için [geçerli dizin ve içerik kökü](#current-directory-and-content-root) bölümüne bakın. Bu adım uygulama `CreateWebHostBuilder`' de yapılandırılmadan önce gerçekleştirilir.
* Uygulamayı bir hizmet olarak çalıştırmak için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> çağırın.

Komut satırı [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bağımsız değişkenleri almadan önce `--console` anahtarı bağımsız değişkenlerden kaldırılır.

Windows olay günlüğü 'ne yazmak için, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>EventLog sağlayıcısını ekleyin. Günlük kaydı düzeyini appSettings 'teki `Logging:LogLevel:Default` anahtarıyla ayarlayın *. Production. JSON* dosyası.

Örnek uygulamadan aşağıdaki örnekte, uygulama içindeki ömür olaylarını işlemek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> yerine `RunAsCustomService` çağırılır. Daha fazla bilgi için [olayları başlatma ve durdurma olaylarını](#handle-starting-and-stopping-events) inceleyin bölümüne bakın.

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>Dağıtım türü

Dağıtım senaryoları hakkında bilgi ve öneriler için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).

### <a name="sdk"></a>SDK

Razor Pages veya MVC çerçevelerini kullanan bir Web uygulaması tabanlı hizmet için, proje dosyasında Web SDK 'sını belirtin:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Hizmet yalnızca arka plan görevlerini (örneğin, [barındırılan hizmetler](xref:fundamentals/host/hosted-services)) yürütülüyorsa, proje dosyasında çalışan SDK 'sını belirtin:

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a>Çerçeveye bağımlı dağıtım (FDD)

Çerçeveye bağımlı dağıtım (FDD), hedef sistemde .NET Core 'un paylaşılan sistem genelindeki bir sürümünün varlığına dayanır. Bu makaledeki kılavuzdan sonra FDD senaryosu benimsendiği zaman SDK, *çerçeveye bağlı yürütülebilir dosya*olarak adlandırılan yürütülebilir bir dosya ( *. exe*) oluşturur.

::: moniker range=">= aspnetcore-3.0"

[Web SDK](#sdk)kullanıyorsanız, normalde bir ASP.NET Core uygulaması yayımlarken üretilen bir *Web. config* dosyası Windows Hizmetleri uygulaması için gereksizdir. *Web. config* dosyasının oluşturulmasını devre dışı bırakmak için `<IsTransformWebConfigDisabled>` özelliği `true`olarak ayarlayın.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir. Aşağıdaki örnekte, RID `win7-x64`olarak ayarlanmıştır. `<SelfContained>` özelliği `false`olarak ayarlanır. Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.

Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir. *Web. config* dosyasının oluşturulmasını devre dışı bırakmak için `<IsTransformWebConfigDisabled>` özelliği `true`olarak ayarlayın.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir. Aşağıdaki örnekte, RID `win7-x64`olarak ayarlanmıştır. `<SelfContained>` özelliği `false`olarak ayarlanır. Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.

`<UseAppHost>` özelliği `true`olarak ayarlanır. Bu özellik, bir FDD için bir etkinleştirme yolu (yürütülebilir, *. exe*) ile hizmeti sağlar.

Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir. *Web. config* dosyasının oluşturulmasını devre dışı bırakmak için `<IsTransformWebConfigDisabled>` özelliği `true`olarak ayarlayın.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a>Kendi içinde dağıtım (SCD)

Kendinden bağımsız dağıtım (SCD), ana bilgisayar sisteminde paylaşılan bir Framework varlığına güvenmez. Çalışma zamanı ve uygulamanın bağımlılıkları uygulamayla birlikte dağıtılır.

Hedef Framework 'ü içeren `<PropertyGroup>` bir Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) bulunur:

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

Birden çok RID için yayımlamak için:

* RID 'leri, noktalı virgülle ayrılmış bir liste olarak belirtin.
* [\<Runtimetanýmlayýcýtanımlayıcıları >](/dotnet/core/tools/csproj#runtimeidentifiers) (plural) için özellik adını kullanın.

Daha fazla bilgi için bkz. [.NET Core RID kataloğu](/dotnet/core/rid-catalog).

::: moniker range="< aspnetcore-3.0"

`<SelfContained>` Özellik `true`olarak ayarlanır:

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a>Hizmet Kullanıcı hesabı

Bir hizmet için Kullanıcı hesabı oluşturmak için, bir yönetim PowerShell 6 komut kabuğundan [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ini kullanın.

Windows 10 Ekim 2018 Güncelleştirmesi (sürüm 1809/Build 10.0.17763) veya sonraki sürümler:

```PowerShell
New-LocalUser -Name {SERVICE NAME}
```

Windows 10 Ekim 2018 (sürüm 1809/Build 10.0.17763) sürümünden önceki Windows IŞLETIM sistemlerinde:

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

İstendiğinde [güçlü bir parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) sağlayın.

`-AccountExpires` parametresi [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ine bir süre sonu <xref:System.DateTime>sağlanmamışsa hesabın süresi dolmaz.

Daha fazla bilgi için bkz. [Microsoft. PowerShell. LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) ve [hizmet Kullanıcı hesapları](/windows/desktop/services/service-user-accounts).

Active Directory kullanırken kullanıcıları yönetmeye yönelik alternatif bir yaklaşım, yönetilen hizmet hesaplarını kullanmaktır. Daha fazla bilgi için bkz. [Grup yönetilen hizmet hesaplarına genel bakış](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).

## <a name="log-on-as-a-service-rights"></a>Hizmet hakları olarak oturum açma

Hizmet Kullanıcı hesabı için *hizmet hakları olarak oturum* açma oluşturmak için:

1. Yerel Güvenlik Ilkesi düzenleyicisini, *secpol. msc*' i çalıştırarak açın.
1. **Yerel ilkeler** düğümünü genişletin ve **Kullanıcı hakları ataması**' nı seçin.
1. **Hizmet olarak oturum** açma ilkesi açın.
1. **Kullanıcı veya Grup Ekle**' yi seçin.
1. Aşağıdaki yaklaşımlardan birini kullanarak nesne adını (Kullanıcı hesabı) sağlayın:
   1. Kullanıcı hesabını (`{DOMAIN OR COMPUTER NAME\USER}`) nesne adı alanına yazın ve kullanıcıyı ilkeye eklemek için **Tamam** ' ı seçin.
   1. **Gelişmiş**'i seçin. **Şimdi bul**' u seçin. Listeden Kullanıcı hesabını seçin. **Tamam**’ı seçin. Kullanıcıyı ilkeye eklemek için yeniden **Tamam ' ı** seçin.
1. Değişiklikleri kabul etmek için **Tamam ' ı** veya **Uygula** ' yı seçin.

## <a name="create-and-manage-the-windows-service"></a>Windows hizmetini oluşturma ve yönetme

### <a name="create-a-service"></a>Hizmet oluşturma

Bir hizmeti kaydetmek için PowerShell komutlarını kullanın. Bir yönetim PowerShell 6 komut kabuğundan aşağıdaki komutları yürütün:

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* Ana bilgisayardaki uygulamanın klasörünün yolunu &ndash; `{EXE PATH}` (örneğin, `d:\myservice`). Uygulamanın yürütülebilir dosyasını yola eklemeyin. Sondaki eğik çizgi gerekli değildir.
* `{DOMAIN OR COMPUTER NAME\USER}` &ndash; hizmet Kullanıcı hesabı (örneğin, `Contoso\ServiceUser`).
* `{SERVICE NAME}` &ndash; hizmet adı (örneğin, `MyService`).
* uygulamanın yürütülebilir yolunu &ndash; `{EXE FILE PATH}` (örneğin, `d:\myservice\myservice.exe`). Yürütülebilir dosyanın dosya adını uzantısına ekleyin.
* `{DESCRIPTION}` &ndash; hizmet açıklaması (örneğin, `My sample service`).
* `{DISPLAY NAME}` &ndash; hizmet görünen adı (örneğin, `My Service`).

### <a name="start-a-service"></a>Hizmet başlatma

Aşağıdaki PowerShell 6 komutuyla bir hizmet başlatın:

```powershell
Start-Service -Name {SERVICE NAME}
```

Komutun başlatılması birkaç saniye sürer.

### <a name="determine-a-services-status"></a>Hizmetin durumunu belirleme

Bir hizmetin durumunu denetlemek için aşağıdaki PowerShell 6 komutunu kullanın:

```powershell
Get-Service -Name {SERVICE NAME}
```

Durum, aşağıdaki değerlerden biri olarak bildirilir:

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a>Bir hizmeti durdur

Aşağıdaki PowerShell 6 komutuyla bir hizmeti durdurun:

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a>Hizmeti Kaldır

Bir hizmeti durdurmak için kısa bir gecikmeden sonra, aşağıdaki PowerShell 6 komutuyla bir hizmeti kaldırın:

```powershell
Remove-Service -Name {SERVICE NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a>Olayları başlatma ve durdurma olaylarını işleme

<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>ve <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> olaylarını işlemek için:

1. `OnStarting`, `OnStarted`ve `OnStopping` yöntemlerle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> türetilen bir sınıf oluşturun:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <xref:System.ServiceProcess.ServiceBase.Run*>`CustomWebHostService` ileten <xref:Microsoft.AspNetCore.Hosting.IWebHost> için bir genişletme yöntemi oluşturun:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. `Program.Main`' de, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>yerine `RunAsCustomService` uzantısı metodunu çağırın:

   ```csharp
   host.RunAsCustomService();
   ```

   `Program.Main`<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> konumunu görmek için, [dağıtım türü](#deployment-type) bölümünde gösterilen kod örneğine bakın.

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Internet 'ten veya şirket ağından gelen isteklerle etkileşime geçen ve bir ara sunucu veya yük dengeleyicinin arkasındaki Hizmetler ek yapılandırma gerektirebilir. Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-endpoints"></a>Uç noktaları yapılandırma

Varsayılan olarak, ASP.NET Core `http://localhost:5000`bağlar. `ASPNETCORE_URLS` ortam değişkenini ayarlayarak URL 'YI ve bağlantı noktasını yapılandırın.

Ek URL ve bağlantı noktası yapılandırma yaklaşımları için ilgili sunucu makalesine bakın:

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

Yukarıdaki kılavuz, HTTPS uç noktaları için desteği içerir. Örneğin, bir Windows hizmeti ile kimlik doğrulaması kullanıldığında HTTPS için uygulamayı yapılandırın.

> [!NOTE]
> Hizmet uç noktasının güvenliğini sağlamak için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmez.

## <a name="current-directory-and-content-root"></a>Geçerli dizin ve içerik kökü

Bir Windows hizmeti için <xref:System.IO.Directory.GetCurrentDirectory*> çağırarak döndürülen geçerli çalışma dizini *C:\\windows\\system32* klasörüdür. *System32* klasörü, bir hizmetin dosyalarını (örneğin, ayarlar dosyaları) depolamak için uygun bir konum değildir. Bir hizmetin varlık ve ayar dosyalarını sürdürmek ve erişmek için aşağıdaki yaklaşımlardan birini kullanın.

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>ContentRootPath veya ContentRootFileProvider kullanın

Bir uygulamanın kaynaklarını bulmak için [ıhostenvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) veya <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> kullanın.

Uygulama bir hizmet olarak çalıştığında, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory)olarak ayarlar.

Uygulamanın varsayılan ayar dosyaları, *appSettings. JSON* ve *appSettings. { '. JSON ortamı*, [konak oluşturma sırasında Createdefaultbuilder](xref:fundamentals/host/generic-host#set-up-a-host)' ı çağırarak uygulamanın içerik kökünden yüklenir.

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>geliştirici kodu tarafından yüklenen diğer ayar dosyaları için, <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>çağırmanız gerekmez. Aşağıdaki örnekte, *custom_settings. JSON* dosyası uygulamanın içerik kökünde bulunur ve açıkça bir temel yolu ayarlamadan yüklenir:

[!code-csharp[](windows-service/samples_snapshot/CustomSettingsExample.cs?highlight=13)]

Bir Windows hizmeti uygulaması geçerli dizini olarak *C:\\Windows\\system32* klasörünü döndürdüğünden, kaynak yolunu almak için <xref:System.IO.Directory.GetCurrentDirectory*> kullanmayı denemeyin.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Uygulamanın klasörü için içerik kök yolunu ayarla

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*>, bir hizmet oluşturulduğunda `binPath` bağımsız değişkenine sunulan yoldur. Ayar dosyalarına yollar oluşturmak için `GetCurrentDirectory` çağırmak yerine, uygulamanın [içerik kökünün](xref:fundamentals/index#content-root)yolunu kullanarak <xref:System.IO.Directory.SetCurrentDirectory*> çağırın.

`Program.Main`, hizmetin yürütülebilir dosyasının yolunu belirleyin ve uygulamanın içerik kökünü oluşturmak için yolu kullanın:

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a>Hizmetin dosyalarını diskte uygun bir konumda depolayın

Dosyaları içeren klasörde <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanırken <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> mutlak bir yol belirtin.

## <a name="troubleshoot"></a>Sorun giderme

Windows hizmet uygulamasının sorunlarını gidermek için bkz. <xref:test/troubleshoot>.

### <a name="common-errors"></a>Sık karşılaşılan hatalar

* PowerShell 'in eski veya yayın öncesi sürümü kullanımda.
* Kayıtlı hizmet, [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutundan uygulamanın **yayımlanmış** çıktısını kullanmaz. [DotNet Build](/dotnet/core/tools/dotnet-build) komutunun çıkışı, uygulama dağıtımı için desteklenmez. Yayımlanan varlıklar, dağıtım türüne bağlı olarak aşağıdaki klasörlerden birinde bulunur:
  * *bin/Release/{Target Framework}/Publish* (FDD)
  * *bin/Release/{Target Framework}/{RUNTIME tanımlayıcısı}/Publish* (SCD)
* Hizmet çalışır durumda değil.
* Uygulamanın kullandığı kaynakların yolları (örneğin, sertifikalar) yanlış. Windows hizmetinin temel yolu *c:\\windows\\system32*' dir.
* Kullanıcı, hizmet hakları *olarak oturum* açma hakkına sahip değil.
* `New-Service` PowerShell komutu çalıştırılırken kullanıcının parolasının kullanım dışı veya yanlış şekilde geçirilmesi.
* Uygulama ASP.NET Core kimlik doğrulaması gerektiriyor, ancak güvenli bağlantılar (HTTPS) için yapılandırılmamış.
* İstek URL 'SI bağlantı noktası yanlış veya uygulamada doğru şekilde yapılandırılmamış.

### <a name="system-and-application-event-logs"></a>Sistem ve uygulama olay günlükleri

Sistem ve uygulama olay günlüklerine erişin:

1. Başlat menüsünü açın, *Olay Görüntüleyicisi*araması yapın ve **Olay Görüntüleyicisi** uygulamayı seçin.
1. **Olay Görüntüleyicisi**, **Windows günlükleri** düğümünü açın.
1. Sistem olay günlüğünü açmak için **sistem** ' i seçin. Uygulama olay günlüğünü açmak için **uygulama** ' yı seçin.
1. Başarısız olan uygulama ile ilişkili hataları arayın.

### <a name="run-the-app-at-a-command-prompt"></a>Uygulamayı bir komut isteminde aşağıdakini çalıştırın

Birçok başlatma hatası olay günlüklerinde yararlı bilgiler oluşturmaz. Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz. Uygulamadan ek ayrıntı günlüğe kaydetmek için, [günlük düzeyini](xref:fundamentals/logging/index#log-level) düşürün veya [geliştirme ortamında](xref:fundamentals/environments)uygulamayı çalıştırın.

### <a name="clear-package-caches"></a>Paket önbelleklerini temizle

Çalışan bir uygulama, geliştirme makinesindeki .NET Core SDK yükseltmeden veya uygulama içindeki paket sürümlerini değiştirirken hemen başarısız olabilir. Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir. Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:

1. *Bin* ve *obj* klasörlerini silin.
1. Bir komut kabuğundan [DotNet NuGet yerelleri, Tümünü Temizle](/dotnet/core/tools/dotnet-nuget-locals) ' i yürüterek paket önbelleklerini temizleyin.

   Paket önbelleklerini Temizleme, [NuGet. exe](https://www.nuget.org/downloads) aracı ile de gerçekleştirilebilir ve komut `nuget locals all -clear`yürütülebilir. *NuGet. exe* , Windows masaüstü işletim sistemiyle birlikte paketlenmiş bir yüklemedir ve [NuGet Web sitesinden](https://www.nuget.org/downloads)ayrı olarak alınmalıdır.

1. Geri yükle ve projeyi yeniden derleyin.
1. Uygulamayı yeniden dağıtmadan önce sunucusundaki dağıtım klasöründeki tüm dosyaları silin.

### <a name="slow-or-hanging-app"></a>Yavaş veya asılı uygulama

*Kilitlenme dökümü* , sistem belleğinin bir anlık görüntüsüdür ve uygulama kilitlenmesinin, başlatma hatasının veya yavaş uygulamanın nedenini belirlemenize yardımcı olabilir.

#### <a name="app-crashes-or-encounters-an-exception"></a>Uygulama kilitleniyor veya bir özel durumla karşılaşırsa

Windows Hata Bildirimi bir döküm edinin ve çözümleyin [(WER)](/windows/desktop/wer/windows-error-reporting):

1. Kilitlenme döküm dosyalarını `c:\dumps`tutmak için bir klasör oluşturun.
1. [Enabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) uygulama yürütülebilir adıyla çalıştırın:

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. Uygulamayı kilitlenmenin oluşmasına neden olan koşullar altında çalıştırın.
1. Kilitlenme gerçekleştirildikten sonra, [Disabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1)çalıştırın:

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

Uygulama kilitlenmeleri ve döküm koleksiyonu tamamlandıktan sonra, uygulamanın normal olarak sonlandırılmasına izin verilir. PowerShell betiği, WER 'i uygulama başına en fazla beş döküm toplayacak şekilde yapılandırır.

> [!WARNING]
> Kilitlenme dökümleri büyük miktarda disk alanı kaplar (her birine kadar çok gigabayt kadar).

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>Uygulama askıda kalıyor, başlatma sırasında başarısız oluyor veya normal şekilde çalışıyor

Bir uygulama *askıda* kaldığında (yanıt vermeyi keser ancak kilitlenmez), başlatma sırasında başarısız olur veya normal şekilde çalışır. [Kullanıcı modu döküm dosyaları:](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) döküm oluşturmak için uygun bir aracı seçmek üzere en iyi aracı seçme.

#### <a name="analyze-the-dump"></a>Dökümü çözümle

Bir döküm çeşitli yaklaşımlar kullanılarak analiz edilebilir. Daha fazla bilgi için bkz. [Kullanıcı modu döküm dosyasını çözümleme](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).

## <a name="additional-resources"></a>Ek kaynaklar

::: moniker range=">= aspnetcore-3.0"

* [Kestrel uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (https yapılandırması ve SNI desteği içerir)
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [Kestrel uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (https yapılandırması ve SNI desteği içerir)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
