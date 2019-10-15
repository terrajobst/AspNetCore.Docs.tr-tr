---
title: Windows hizmetinde konak ASP.NET Core
author: guardrex
description: ASP.NET Core uygulamasının bir Windows hizmetinde nasıl barındıralınacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: b02e627af875f15a81d68b0d625a2eccf25c0657
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333797"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Windows hizmetinde konak ASP.NET Core

[Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra) tarafından

Bir ASP.NET Core uygulaması, IIS kullanmadan Windows [hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) olarak Windows üzerinde barındırılabilir. Windows hizmeti olarak barındırıldığı zaman, uygulama otomatik olarak sunucu yeniden başlatıldıktan sonra başlatılır.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

* [ASP.NET Core SDK 2,1 veya üzeri](https://dotnet.microsoft.com/download)
* [PowerShell 6,2 veya üzeri](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Çalışan hizmeti şablonu

ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar. Şablonu bir Windows Hizmeti uygulamasının temeli olarak kullanmak için:

1. .NET Core şablonundan bir çalışan hizmeti uygulaması oluşturun.
1. Çalışan hizmeti uygulamasını bir Windows hizmeti olarak çalışacak şekilde güncelleştirmek için [uygulama yapılandırma](#app-configuration) bölümündeki yönergeleri izleyin.

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

::: moniker-end

## <a name="app-configuration"></a>Uygulama yapılandırması

::: moniker range=">= aspnetcore-3.0"

Ana bilgisayar oluşturulurken [Microsoft. Extensions. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) paketi tarafından verilen `IHostBuilder.UseWindowsService` çağrılır. Uygulama bir Windows hizmeti olarak çalışıyorsa, yöntemi:

* Ana bilgisayar ömrünü `WindowsServiceLifetime` olarak ayarlar.
* [İçerik kökünü](xref:fundamentals/index#content-root)ayarlar.
* Varsayılan kaynak adı olarak uygulama adı ile olay günlüğüne günlük kaydını sağlar.
  * Günlük düzeyi, appSettings 'de `Logging:LogLevel:Default` anahtarı kullanılarak yapılandırılabilir *. Production. JSON* dosyası.
  * Yeni olay kaynakları yalnızca yöneticiler tarafından oluşturulabilir. Uygulama adı kullanılarak bir olay kaynağı oluşturuoluşturumadığında, *uygulama* kaynağına bir uyarı kaydedilir ve olay günlükleri devre dışı bırakılır.

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Uygulama [Microsoft. AspNetCore. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) ve [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog)için paket başvuruları gerektirir.

Bir hizmetin dışında çalışırken test ve hata ayıklamak için, uygulamanın bir hizmet olarak mı yoksa bir konsol uygulaması mi çalıştığını belirleme kodu ekleyin. Hata ayıklayıcının ekli olduğunu veya bir `--console` anahtarının mevcut olup olmadığını denetleyin. Her iki koşul de geçerliyse (uygulama bir hizmet olarak çalıştırılmadıysa) <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> ' ı çağırın. Koşullar yanlışsa (uygulama bir hizmet olarak çalıştırılır):

* @No__t-0 çağrısı yapın ve uygulamanın yayımlanan konumunun yolunu kullanın. Bir Windows hizmeti uygulaması, <xref:System.IO.Directory.GetCurrentDirectory*> çağrıldığında *C: \\WINDOWS @ no__t-3system32* klasörünü döndürdüğünden yolu almak için <xref:System.IO.Directory.GetCurrentDirectory*> çağırmayın. Daha fazla bilgi için [geçerli dizin ve içerik kökü](#current-directory-and-content-root) bölümüne bakın. Bu adım, uygulama `CreateWebHostBuilder` ' da yapılandırılmadan önce gerçekleştirilir.
* Uygulamayı bir hizmet olarak çalıştırmak için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> çağrısı yapın.

Komut satırı [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bağımsız değişkenleri almadan önce `--console` anahtarı bağımsız değişkenlerden kaldırılır.

Windows olay günlüğü 'ne yazmak için, EventLog sağlayıcısını <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*> ' a ekleyin. Günlük kaydı düzeyini appSettings 'de `Logging:LogLevel:Default` anahtarıyla ayarlayın *. Production. JSON* dosyası.

Örnek uygulamadan aşağıdaki örnekte, uygulamadaki ömür olaylarını işlemek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> yerine `RunAsCustomService` çağrılır. Daha fazla bilgi için [olayları başlatma ve durdurma olaylarını](#handle-starting-and-stopping-events) inceleyin bölümüne bakın.

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>Dağıtım türü

Dağıtım senaryoları hakkında bilgi ve öneriler için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment-fdd"></a>Çerçeveye bağımlı dağıtım (FDD)

Çerçeveye bağımlı dağıtım (FDD), hedef sistemde .NET Core 'un paylaşılan sistem genelindeki bir sürümünün varlığına dayanır. Bu makaledeki kılavuzdan sonra FDD senaryosu benimsendiği zaman SDK, *çerçeveye bağlı yürütülebilir dosya*olarak adlandırılan yürütülebilir bir dosya ( *. exe*) oluşturur.

::: moniker range=">= aspnetcore-3.0"

Aşağıdaki özellik öğelerini proje dosyasına ekleyin:

* `<OutputType>` &ndash; uygulamanın çıkış türü (yürütülebilir için `Exe`).
* `<LangVersion>` &ndash; C# dil sürümü (`latest` veya `preview`).

Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir. *Web. config* dosyasının oluşturulmasını devre dışı bırakmak için `<IsTransformWebConfigDisabled>` özelliğini `true` olarak ayarlayın.

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

Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir. Aşağıdaki örnekte, RID `win7-x64` olarak ayarlanmıştır. @No__t-0 özelliği `false` olarak ayarlanır. Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.

Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir. *Web. config* dosyasının oluşturulmasını devre dışı bırakmak için `<IsTransformWebConfigDisabled>` özelliğini `true` olarak ayarlayın.

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

Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) ([\<runtimeıdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)), hedef Framework 'ü içerir. Aşağıdaki örnekte, RID `win7-x64` olarak ayarlanmıştır. @No__t-0 özelliği `false` olarak ayarlanır. Bu özellikler SDK 'nın Windows için bir yürütülebilir ( *. exe*) dosya ve paylaşılan .NET Core çerçevesine bağlı bir uygulama oluşturmasını ister.

@No__t-0 özelliği `true` olarak ayarlanır. Bu özellik, bir FDD için bir etkinleştirme yolu (yürütülebilir, *. exe*) ile hizmeti sağlar.

Bir ASP.NET Core uygulaması yayımlandığında normalde üretilen bir *Web. config* dosyası, Windows Hizmetleri uygulaması için gereksizdir. *Web. config* dosyasının oluşturulmasını devre dışı bırakmak için `<IsTransformWebConfigDisabled>` özelliğini `true` olarak ayarlayın.

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

Hedef Framework 'ü içeren `<PropertyGroup>` ' de bir Windows [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog) bulunur:

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

Birden çok RID için yayımlamak için:

* RID 'leri, noktalı virgülle ayrılmış bir liste olarak belirtin.
* [@No__t-1Runtimetanımlayıcılar >](/dotnet/core/tools/csproj#runtimeidentifiers) (plural) özellik adını kullanın.

Daha fazla bilgi için bkz. [.NET Core RID kataloğu](/dotnet/core/rid-catalog).

::: moniker range="< aspnetcore-3.0"

@No__t-0 özelliği `true` olarak ayarlanır:

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

@No__t-0 parametresi [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet 'ine bir süre sonu <xref:System.DateTime> ile sağlanmamışsa, hesabın süresi dolmaz.

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
   1. **Gelişmiş**' i seçin. **Şimdi bul**' u seçin. Listeden Kullanıcı hesabını seçin. **Tamam ' ı**seçin. Kullanıcıyı ilkeye eklemek için yeniden **Tamam ' ı** seçin.
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

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* `{EXE PATH}` @no__t-konaktaki uygulamanın klasörünün yolu (örneğin, `d:\myservice`). Uygulamanın yürütülebilir dosyasını yola eklemeyin. Sondaki eğik çizgi gerekli değildir.
* `{DOMAIN OR COMPUTER NAME\USER}` &ndash; hizmet Kullanıcı hesabı (örneğin, `Contoso\ServiceUser`).
* `{NAME}` &ndash; hizmet adı (örneğin, `MyService`).
* `{EXE FILE PATH}` &ndash; uygulamanın yürütülebilir yolu (örneğin, `d:\myservice\myservice.exe`). Yürütülebilir dosyanın dosya adını uzantısına ekleyin.
* `{DESCRIPTION}` &ndash; hizmet açıklaması (örneğin, `My sample service`).
* `{DISPLAY NAME}` &ndash; hizmet görünen adı (örneğin, `My Service`).

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

@No__t-0, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> ve <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> olaylarını işlemek için:

1. @No__t-1, `OnStarted` ve `OnStopping` yöntemleriyle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> ' dan türetilen bir sınıf oluşturun:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. @No__t-1 ' i <xref:System.ServiceProcess.ServiceBase.Run*> ' ye geçiren <xref:Microsoft.AspNetCore.Hosting.IWebHost> için bir genişletme yöntemi oluşturun:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. @No__t-0 ' da, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> yerine `RunAsCustomService` uzantı metodunu çağırın:

   ```csharp
   host.RunAsCustomService();
   ```

   @No__t-1 ' de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> konumunu görmek için [dağıtım türü](#deployment-type) bölümünde gösterilen kod örneğine bakın.

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy sunucusu ve yük dengeleyici senaryoları

Internet 'ten veya şirket ağından gelen isteklerle etkileşime geçen ve bir ara sunucu veya yük dengeleyicinin arkasındaki Hizmetler ek yapılandırma gerektirebilir. Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-endpoints"></a>Uç noktaları yapılandırma

Varsayılan olarak, ASP.NET Core `http://localhost:5000` ' a bağlanır. @No__t-0 ortam değişkenini ayarlayarak URL 'YI ve bağlantı noktasını yapılandırın.

Ek URL ve bağlantı noktası yapılandırma yaklaşımları için ilgili sunucu makalesine bakın:

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

Yukarıdaki kılavuz, HTTPS uç noktaları için desteği içerir. Örneğin, bir Windows hizmeti ile kimlik doğrulaması kullanıldığında HTTPS için uygulamayı yapılandırın.

> [!NOTE]
> Hizmet uç noktasının güvenliğini sağlamak için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmez.

## <a name="current-directory-and-content-root"></a>Geçerli dizin ve içerik kökü

Bir Windows hizmeti için <xref:System.IO.Directory.GetCurrentDirectory*> çağırarak döndürülen geçerli çalışma dizini *C: \\WINDOWS @ no__t-3system32* klasörüdür. *System32* klasörü, bir hizmetin dosyalarını (örneğin, ayarlar dosyaları) depolamak için uygun bir konum değildir. Bir hizmetin varlık ve ayar dosyalarını sürdürmek ve erişmek için aşağıdaki yaklaşımlardan birini kullanın.

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>ContentRootPath veya ContentRootFileProvider kullanın

Bir uygulamanın kaynaklarını bulmak için [ıhostenvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) veya <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> kullanın.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Uygulamanın klasörü için içerik kök yolunu ayarla

@No__t-0, bir hizmet oluşturulduğunda `binPath` bağımsız değişkenine girilen yoldur. @No__t-0 ' ı çağırmak yerine, ayarlar dosyalarına yollar oluşturmak için, uygulamanın [içerik kökünün](xref:fundamentals/index#content-root)yoluyla <xref:System.IO.Directory.SetCurrentDirectory*> ' i çağırın.

@No__t-0 ' da, hizmetin yürütülebilir dosyasının yolunu belirleyin ve uygulamanın içerik kökünü oluşturmak için yolu kullanın:

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

Dosyaları içeren klasöre <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanırken <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> ile mutlak bir yol belirtin.

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
