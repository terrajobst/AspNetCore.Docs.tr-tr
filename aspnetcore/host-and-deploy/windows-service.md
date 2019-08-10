---
title: Windows hizmetinde konak ASP.NET Core
author: guardrex
description: ASP.NET Core uygulamasının bir Windows hizmetinde nasıl barındıralınacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/03/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 703edff19d82d10415bedb9b92d83709c275100f
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913910"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Windows hizmetinde konak ASP.NET Core

[Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra) tarafından

Bir ASP.NET Core uygulaması, IIS kullanmadan Windows [hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) olarak Windows üzerinde barındırılabilir. Windows hizmeti olarak barındırıldığı zaman, uygulama otomatik olarak sunucu yeniden başlatıldıktan sonra başlatılır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

* [ASP.NET Core SDK 2,1 veya üzeri](https://dotnet.microsoft.com/download)
* [PowerShell 6,2 veya üzeri](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Çalışan hizmeti şablonu

ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar. Şablonu bir Windows Hizmeti uygulamasının temeli olarak kullanmak için:

1. .NET Core şablonundan bir çalışan hizmeti uygulaması oluşturun.
1. Çalışan hizmeti uygulamasını bir Windows hizmeti olarak çalışacak şekilde güncelleştirmek için [uygulama yapılandırma](#app-configuration) bölümündeki yönergeleri izleyin.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Yeni bir proje oluşturun.
1. Seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.
1. **Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin. **Oluştur**’u seçin.
1. **Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın.
1. **Çalışan hizmeti** şablonunu seçin. **Oluştur**’u seçin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Çalışan hizmeti (`worker`) şablonunu bir komut kabuğundan [DotNet New](/dotnet/core/tools/dotnet-new) komutuyla birlikte kullanın. Aşağıdaki örnekte, adlı `ContosoWorkerService`bir çalışan hizmeti uygulaması oluşturulur. Komut yürütüldüğünde, `ContosoWorkerService` uygulamanın bir klasörü otomatik olarak oluşturulur.

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a>Uygulama yapılandırması

::: moniker range=">= aspnetcore-3.0"

`IHostBuilder.UseWindowsService`, ana bilgisayar oluşturulurken [Microsoft. Extensions. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) paketi tarafından verilen çağrılır. Uygulama bir Windows hizmeti olarak çalışıyorsa, yöntemi:

* Ana bilgisayar ömrünü olarak `WindowsServiceLifetime`ayarlar.
* İçerik kökünü ayarlar.
* Varsayılan kaynak adı olarak uygulama adı ile olay günlüğüne günlük kaydını sağlar.
  * Günlük düzeyi appSettings içindeki `Logging:LogLevel:Default` anahtar kullanılarak yapılandırılabilir *. Production. JSON* dosyası.
  * Yeni olay kaynakları yalnızca yöneticiler tarafından oluşturulabilir. Uygulama adı kullanılarak bir olay kaynağı oluşturuoluşturumadığında, *uygulama* kaynağına bir uyarı kaydedilir ve olay günlükleri devre dışı bırakılır.

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Uygulama [Microsoft. AspNetCore. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) ve [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog)için paket başvuruları gerektirir.

Bir hizmetin dışında çalışırken test ve hata ayıklamak için, uygulamanın bir hizmet olarak mı yoksa bir konsol uygulaması mi çalıştığını belirleme kodu ekleyin. Hata ayıklayıcının ekli olduğunu veya bir `--console` anahtarın mevcut olup olmadığını denetleyin. Her iki koşul de geçerliyse (uygulama bir hizmet olarak çalıştırılmadıysa), çağırın <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>. Koşullar yanlışsa (uygulama bir hizmet olarak çalıştırılır):

* Uygulamanın <xref:System.IO.Directory.SetCurrentDirectory*> yayımlanmış konumunun yolunu çağırın ve kullanın. Bir Windows <xref:System.IO.Directory.GetCurrentDirectory*> hizmeti uygulaması çağrıldığında *C:\\Windows\\system32* klasörünü <xref:System.IO.Directory.GetCurrentDirectory*> döndürdüğünden yolu almak için çağırmayın. Daha fazla bilgi için [geçerli dizin ve içerik kökü](#current-directory-and-content-root) bölümüne bakın. Bu adım, uygulamanın ' de `CreateWebHostBuilder`yapılandırılmadan önce gerçekleştirilir.
* Uygulamayı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> bir hizmet olarak çalıştırmak için çağırın.

[Komut satırı yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden, `--console` bağımsız değişkenleri almadan önce <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> anahtar bağımsız değişkenlerden kaldırılır.

Windows olay günlüğü 'ne yazmak için EventLog sağlayıcısını öğesine <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>ekleyin. Günlük kaydı düzeyini `Logging:LogLevel:Default` appSettings içindeki anahtarla ayarlayın *. Production. JSON* dosyası.

Örnek uygulamadan aşağıdaki örnekte, `RunAsCustomService` uygulama içindeki ömür olaylarını işlemek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> yerine çağrılır. Daha fazla bilgi için [olayları başlatma ve durdurma olaylarını](#handle-starting-and-stopping-events) inceleyin bölümüne bakın.

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>Dağıtım türü

Dağıtım senaryoları hakkında bilgi ve öneriler için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment-fdd"></a>Çerçeveye bağımlı dağıtım (FDD)

Çerçeveye bağımlı dağıtım (FDD), hedef sistemde .NET Core 'un paylaşılan sistem genelindeki bir sürümünün varlığına dayanır. Bu makaledeki kılavuzdan sonra FDD senaryosu benimsendiği zaman SDK, *çerçeveye bağlı yürütülebilir dosya*olarak adlandırılan yürütülebilir bir dosya ( *. exe*) oluşturur.

::: moniker range=">= aspnetcore-3.0"

Aşağıdaki özellik öğelerini proje dosyasına ekleyin:

* `<OutputType>`Uygulamanın çıkış türü (`Exe` yürütülebilir için). &ndash;
* `<LangVersion>`Dil sürümü(`latest` veya`preview`). &ndash; C#

Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir. *Web. config* dosyasının oluşturulmasını devre dışı bırakmak için, `<IsTransformWebConfigDisabled>` özelliğini öğesine `true`ekleyin.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir. Aşağıdaki örnekte, RID olarak `win7-x64`ayarlanır. `<SelfContained>` Özelliği olarak`false`ayarlanır. Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.

Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir. *Web. config* dosyasının oluşturulmasını devre dışı bırakmak için, `<IsTransformWebConfigDisabled>` özelliğini öğesine `true`ekleyin.

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

Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir. Aşağıdaki örnekte, RID olarak `win7-x64`ayarlanır. `<SelfContained>` Özelliği olarak`false`ayarlanır. Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.

`<UseAppHost>` Özelliği olarak`true`ayarlanır. Bu özellik, bir FDD için bir etkinleştirme yolu (yürütülebilir, *. exe*) ile hizmeti sağlar.

Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir. *Web. config* dosyasının oluşturulmasını devre dışı bırakmak için, `<IsTransformWebConfigDisabled>` özelliğini öğesine `true`ekleyin.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a>Kendi içinde dağıtım (SCD)

Kendinden bağımsız dağıtım (SCD), ana bilgisayar sisteminde paylaşılan bir Framework varlığına güvenmez. Çalışma zamanı ve uygulamanın bağımlılıkları uygulamayla birlikte dağıtılır.

Hedef Framework 'ü içeren ' de `<PropertyGroup>` bir Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) bulunur:

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

Birden çok RID için yayımlamak için:

* RID 'leri, noktalı virgülle ayrılmış bir liste olarak belirtin.
* > (Plural) adlı [ \<runtimetanımlayıcılar](/dotnet/core/tools/csproj#runtimeidentifiers) özelliğini kullanın.

Daha fazla bilgi için bkz. [.NET Core RID kataloğu](/dotnet/core/rid-catalog).

::: moniker range="< aspnetcore-3.0"

Bir `<SelfContained>` özellik şu şekilde `true`ayarlanır:

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a>Hizmet Kullanıcı hesabı

Bir hizmet için Kullanıcı hesabı oluşturmak için, bir yönetim PowerShell 6 komut kabuğundan [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ini kullanın.

Windows 10 Ekim 2018 Güncelleştirmesi (sürüm 1809/Build 10.0.17763) veya sonraki sürümler:

```PowerShell
New-LocalUser -Name {NAME}
```

Windows 10 Ekim 2018 (sürüm 1809/Build 10.0.17763) sürümünden önceki Windows IŞLETIM sistemlerinde:

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

İstendiğinde [güçlü bir parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) sağlayın.

Parametresi New [-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ine bir süre sonu <xref:System.DateTime>ile sağlanmamışsa hesabın süresi dolmaz. `-AccountExpires`

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

### <a name="create-a-service"></a>Bir hizmet oluşturma

Bir hizmeti kaydetmek için PowerShell komutlarını kullanın. Bir yönetim PowerShell 6 komut kabuğundan aşağıdaki komutları yürütün:

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* `{EXE PATH}`Konaktaki uygulamanın klasörünün yolu (örneğin, `d:\myservice`). &ndash; Uygulamanın yürütülebilir dosyasını yola eklemeyin. Sondaki eğik çizgi gerekli değildir.
* `{DOMAIN OR COMPUTER NAME\USER}`Hizmet Kullanıcı hesabı (örneğin, `Contoso\ServiceUser`). &ndash;
* `{NAME}`Hizmet adı (örneğin, `MyService`). &ndash;
* `{EXE FILE PATH}`Uygulamanın yürütülebilir yolu (örneğin, `d:\myservice\myservice.exe`). &ndash; Yürütülebilir dosyanın dosya adını uzantısına ekleyin.
* `{DESCRIPTION}`Hizmet açıklaması (örneğin, `My sample service`). &ndash;
* `{DISPLAY NAME}`Hizmet görünen adı (örneğin, `My Service`). &ndash;

### <a name="start-a-service"></a>Hizmet başlatma

Aşağıdaki PowerShell 6 komutuyla bir hizmet başlatın:

```powershell
Start-Service -Name {NAME}
```

Komutun başlatılması birkaç saniye sürer.

### <a name="determine-a-services-status"></a>Hizmetin durumunu belirleme

Bir hizmetin durumunu denetlemek için aşağıdaki PowerShell 6 komutunu kullanın:

```powershell
Get-Service -Name {NAME}
```

Durum, aşağıdaki değerlerden biri olarak bildirilir:

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a>Bir hizmeti durdur

Aşağıdaki PowerShell 6 komutuyla bir hizmeti durdurun:

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a>Hizmeti Kaldır

Bir hizmeti durdurmak için kısa bir gecikmeden sonra, aşağıdaki PowerShell 6 komutuyla bir hizmeti kaldırın:

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a>Olayları başlatma ve durdurma olaylarını işleme

, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*> <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>Ve olaylarınıişlemekiçin:<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>

1. `OnStarting`, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> Ve`OnStarted`yöntemleriyle türetilen bir sınıfoluşturun:`OnStopping`

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. ' A <xref:Microsoft.AspNetCore.Hosting.IWebHost> `CustomWebHostService` geçişi içinbirgenişletmeyöntemioluşturun:<xref:System.ServiceProcess.ServiceBase.Run*>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. İçinde `Program.Main`, `RunAsCustomService` yerine<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>genişletme yöntemini çağırın:

   ```csharp
   host.RunAsCustomService();
   ```

   <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> Konumunu`Program.Main`görmek için, [dağıtım türü](#deployment-type) bölümünde gösterilen kod örneğine bakın.

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Internet 'ten veya şirket ağından gelen isteklerle etkileşime geçen ve bir ara sunucu veya yük dengeleyicinin arkasındaki Hizmetler ek yapılandırma gerektirebilir. Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>HTTPS 'yi yapılandırma

Hizmeti güvenli bir uç noktayla yapılandırmak için:

1. Platformunuzun sertifika alımı ve dağıtım mekanizmalarını kullanarak barındırma sistemi için bir X. 509.440 sertifikası oluşturun.

1. Sertifikayı kullanmak için bir [Kestrel Server HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) belirtin.

Hizmet uç noktasının güvenliğini sağlamak için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmez.

## <a name="current-directory-and-content-root"></a>Geçerli dizin ve içerik kökü

Windows hizmeti için çağırarak <xref:System.IO.Directory.GetCurrentDirectory*> döndürülen geçerli çalışma dizini *C:\\Windows\\system32* klasörüdür. *System32* klasörü, bir hizmetin dosyalarını (örneğin, ayarlar dosyaları) depolamak için uygun bir konum değildir. Bir hizmetin varlık ve ayar dosyalarını sürdürmek ve erişmek için aşağıdaki yaklaşımlardan birini kullanın.

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>ContentRootPath veya ContentRootFileProvider kullanın

[Ihostenvironment. contentrootpath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) kullanın veya <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> bir uygulamanın kaynaklarını bulun.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Uygulamanın klasörü için içerik kök yolunu ayarla

, <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Bir hizmet oluşturulduğunda `binPath` bağımsız değişkene aynı yol olarak sağlanır. Ayarlar dosyalarına yollar `GetCurrentDirectory` oluşturmak için çağırmak yerine, uygulamanın içerik kökünün <xref:System.IO.Directory.SetCurrentDirectory*> yolunu çağırın.

İçinde `Program.Main`, hizmetin yürütülebilir dosyasının yolunu belirleyin ve uygulamanın içerik kökünü oluşturmak için yolu kullanın:

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

Dosyaları içeren klasöre <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanırken ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> mutlak bir yol belirtin.

## <a name="additional-resources"></a>Ek kaynaklar

::: moniker range=">= aspnetcore-3.0"

* [Kestrel uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırması ve SNı desteği içerir)
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [Kestrel uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırması ve SNı desteği içerir)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
