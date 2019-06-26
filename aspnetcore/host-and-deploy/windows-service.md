---
title: ASP.NET Core bir Windows hizmetinde barındırma
author: guardrex
description: ASP.NET Core uygulaması bir Windows hizmetinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/03/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 3a254af4d56cb4abc7004a67b0d0b42de2b878b1
ms.sourcegitcommit: 47cc13ab90913af9a2887cef0896bb4e9aba4dd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67399109"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>ASP.NET Core bir Windows hizmetinde barındırma

Tarafından [Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core uygulaması Windows barındırılabilen bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications) IIS kullanmadan. Bir Windows hizmeti olarak barındırıldığında sunucu yeniden başlatıldıktan sonra uygulamayı otomatik olarak başlar.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

* [ASP.NET Core SDK 2.1 veya üzeri](https://dotnet.microsoft.com/download)
* [PowerShell 6.2 veya üstü](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Çalışan hizmet şablonu

ASP.NET Core çalışan hizmet şablonu yazmak için bir başlangıç noktası sağlar. hizmet uygulamaları uzun süre çalışan. Şablon, bir Windows hizmeti uygulaması için temel olarak kullanmak için:

1. .NET Core şablonundan bir çalışan Service uygulaması oluşturun.
1. Sunulan yönergeleri [uygulama yapılandırması](#app-configuration) bir Windows hizmeti olarak çalıştırabilmeniz için çalışan hizmet uygulamasını güncelleştirmek için bölüm.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Yeni bir proje oluşturun.
1. Seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.
1. Bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin. **Oluştur**’u seçin.
1. İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.
1. Seçin **çalışan hizmet** şablonu. **Oluştur**’u seçin.

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

Çalışan hizmetin kullanın (`worker`) şablonuyla [yeni dotnet](/dotnet/core/tools/dotnet-new) komut kabuğu komutunu. Aşağıdaki örnekte, bir çalışan hizmet uygulaması adlandırılmış oluşturulduğunda `ContosoWorkerService`. İçin bir klasör `ContosoWorkerService` uygulama komut yürütülürken otomatik olarak oluşturulur.

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a>Uygulama yapılandırması

::: moniker range=">= aspnetcore-3.0"

`IHostBuilder.UseWindowsService`, tarafından sağlanan [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) paketini, ana bilgisayar oluşturma sırasında çağrılır. Uygulamayı yöntemi bir Windows hizmeti olarak çalışıyorsa:

* Ana kullanım ömrü ayarlar `WindowsServiceLifetime`.
* İçerik kök ayarlar.
* Varsayılan kaynak adıyla uygulama adı ile olay günlüğü için günlük kaydını etkinleştirir.
  * Günlük düzeyi kullanılarak yapılandırılabilir `Logging:LogLevel:Default` anahtarını *appsettings. Production.JSON* dosya.
  * Yalnızca Yöneticiler yeni olay kaynakları oluşturabilirsiniz. Uygulama adı kullanarak bir olay kaynağı oluşturulamıyor olduğunda bir uyarı günlüğe kaydedilir *uygulama* kaynağı ve olay günlüklerini devre dışı bırakıldı.

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Uygulama için paket başvuruları gerekiyor. [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) ve [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Test ve hizmet dışında çalışırken hata ayıklama için uygulamayı bir hizmet veya bir konsol uygulaması olarak çalışıp çalışmadığını belirlemek için kod ekleyin. Hata ayıklayıcıyı eklediyseniz veya inceleyin `--console` anahtar. İki koşuldan birinin (uygulamayı değil çalıştırma hizmet olarak) true ise, çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>. Koşullar (uygulama, hizmet olarak çalıştırıldığında) false olduğunda:

* Çağrı <xref:System.IO.Directory.SetCurrentDirectory*> ve uygulamanın yayımlanmış konumuna bir yol kullanın. Remove() çağırmayın <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti uygulaması döndürüldüğünden yolunu almak için *C:\\WINDOWS\\system32* klasör zaman <xref:System.IO.Directory.GetCurrentDirectory*> çağrılır. Daha fazla bilgi için [geçerli dizin ve içerik kök](#current-directory-and-content-root) bölümü. Bu adım, uygulamanın yapılandırılan önce gerçekleştirilir `CreateWebHostBuilder`.
* Çağrı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> uygulamasını bir hizmet olarak çalıştırmak için.

Çünkü [komut satırı yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#command-line-configuration-provider) komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirir `--console` anahtarı bağımsız değişkenlerden önce kaldırılır <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bağımsız değişkenleri alır.

Windows olay günlüğüne yazmak için olay günlüğü Sağlayıcısı Ekle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. İle günlük tutma düzeyini ayarlamaya `Logging:LogLevel:Default` anahtarını *appsettings. Production.JSON* dosya.

Aşağıdaki örnekte, örnek uygulamadan `RunAsCustomService` yerine adlı <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> uygulama içinde ömür olayları işlemek için. Daha fazla bilgi için [başlatılması ve durdurulması olaylarını işlemek](#handle-starting-and-stopping-events) bölümü.

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>Dağıtım türü

Bilgi ve dağıtım senaryoları hakkında daha fazla öneri için bkz. [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment-fdd"></a>Framework bağımlı dağıtım (FDD)

.NET Core hedef sistemdeki bir paylaşılan sistem genelinde sürüm varlığını Framework bağımlı dağıtım (FDD) kullanır. Bu makaledeki yönergeleri izleyerek FDD senaryo benimsenen, SDK'sı bir çalıştırılabilir dosyası oluşturur ( *.exe*) adlı bir *framework bağımlı yürütülebilir*.

::: moniker range=">= aspnetcore-3.0"

Aşağıdaki özellik öğeleri, proje dosyasına ekleyin:

* `<OutputType>` &ndash; Uygulama türü Çıktı (`Exe` yürütülebilir dosya için).
* `<LangVersion>` &ndash; C# Dil sürümü (`latest` veya `preview`).

A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz. Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.

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

Windows [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) hedef Framework'ü içerir. Aşağıdaki örnekte, RID kümesine `win7-x64`. `<SelfContained>` Özelliği `false`. Bu özellikler, yürütülebilir bir dosya oluşturmak için SDK'sı isteyin ( *.exe*) Windows ve paylaşılan olan .NET Core Framework'e bağlı bir uygulama dosyası.

A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz. Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.

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

Windows [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) hedef Framework'ü içerir. Aşağıdaki örnekte, RID kümesine `win7-x64`. `<SelfContained>` Özelliği `false`. Bu özellikler, yürütülebilir bir dosya oluşturmak için SDK'sı isteyin ( *.exe*) Windows ve paylaşılan olan .NET Core Framework'e bağlı bir uygulama dosyası.

`<UseAppHost>` Özelliği `true`. Bu özellik etkinleştirme yolu hizmetiyle sağlar (bir yürütülebilir dosya *.exe*) bir FDD için.

A *web.config* normalde bir ASP.NET Core uygulaması yayımlama sırasında oluşturulur, dosya, bir Windows hizmet uygulaması için gereksiz. Oluşturulmasını devre dışı bırakmak için *web.config* ekleyin `<IsTransformWebConfigDisabled>` özelliğini `true`.

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

### <a name="self-contained-deployment-scd"></a>Kendi başına dağıtım (SCD)

Kendi başına dağıtım (SCD), konak sistemindeki paylaşılan bir çerçeve varlığına içermez. Çalışma zamanı ve uygulamanın bağımlılıklarını uygulamayla birlikte dağıtılır.

Bir Windows [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) dahil `<PropertyGroup>` , hedef Framework'ü içerir:

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

İçin birden fazla RID yayımlamak için:

* RID noktalı virgülle ayrılmış bir liste sağlar.
* Özellik adını kullanan [ \<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (çoğul).

Daha fazla bilgi için [.NET Core RID Kataloğu](/dotnet/core/rid-catalog).

::: moniker range="< aspnetcore-3.0"

A `<SelfContained>` özelliği `true`:

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a>Hizmet kullanıcı hesabı

Bir hizmet için bir kullanıcı hesabı oluşturmak için kullanın [yeni LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet'i bir yönetim PowerShell 6'yı komut kabuğundan.

Windows 10 Ekim 2018 Güncelleştirmesi (sürüm 1809/derleme 10.0.17763) veya daha sonra:

```PowerShell
New-LocalUser -Name {NAME}
```

Windows 10 Ekim 2018'den önceki Windows işletim sisteminde Update (sürüm 1809/derleme 10.0.17763):

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

Sağlayan bir [güçlü parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) istendiğinde.

Sürece `-AccountExpires` parametre tarafından sağlanan [yeni LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) bir sona erme cmdlet'iyle <xref:System.DateTime>, hesabın süresi sona ermiyor.

Daha fazla bilgi için [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) ve [hizmeti kullanıcı hesaplarını](/windows/desktop/services/service-user-accounts).

Active Directory kullanarak kullanıcıları yönetme için alternatif bir yaklaşım, yönetilen hizmet hesaplarını kullanmaktır. Daha fazla bilgi için [Grup yönetilen hizmet hesaplarına genel bakış](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).

## <a name="log-on-as-a-service-rights"></a>Bir hizmet hakları oturum açın

Kurmak için *hizmet oturum açma* rights hizmeti kullanıcı hesabı için:

1. Yerel Güvenlik İlkesi Düzenleyicisi'ni çalıştırarak açmak *secpol.msc*.
1. Genişletin **yerel ilkeler** düğümünü seçip alt **kullanıcı hakları ataması**.
1. Açık **hizmet oturum açma** ilkesi.
1. Seçin **kullanıcı veya grup ekleme**.
1. Aşağıdaki yaklaşımlardan birini kullanarak nesne adı (kullanıcı hesabı) sağlayın:
   1. Kullanıcı hesabını yazın (`{DOMAIN OR COMPUTER NAME\USER}`) nesne adını seçin ve alan **Tamam** kullanıcı ilkesine eklemek için.
   1. **Gelişmiş**'i seçin. Seçin **Şimdi Bul**. Kullanıcı hesabı listeden seçin. **Tamam**’ı seçin. Seçin **Tamam** yeniden kullanıcı ilkesine eklemek için.
1. Seçin **Tamam** veya **Uygula** değişiklikleri kabul etmek için.

## <a name="create-and-manage-the-windows-service"></a>Oluşturma ve yönetme Windows hizmeti

### <a name="create-a-service"></a>Bir hizmet oluşturma

Bir hizmeti kaydetmek için PowerShell komutlarını kullanın. Bir yönetici PowerShell 6'yı komut kabuğundan, aşağıdaki komutları yürütün:

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* `{EXE PATH}` &ndash; Konaktaki uygulamanın klasör yolu (örneğin, `d:\myservice`). Uygulamanın yürütülebilir yolu dahil değildir. Sonunda eğik çizgi gerekli değildir.
* `{DOMAIN OR COMPUTER NAME\USER}` &ndash; Hizmeti kullanıcı hesabını (örneğin, `Contoso\ServiceUser`).
* `{NAME}` &ndash; Hizmet adı (örneğin, `MyService`).
* `{EXE FILE PATH}` &ndash; Uygulamanın yürütülebilir dosya yolu (örneğin, `d:\myservice\myservice.exe`). Uzantılı dosya adı yürütülebilir dosyası içerir.
* `{DESCRIPTION}` &ndash; Hizmet açıklaması (örneğin, `My sample service`).
* `{DISPLAY NAME}` &ndash; Hizmet görünen adı (örneğin, `My Service`).

### <a name="start-a-service"></a>Bir hizmeti Başlat

Bir hizmet PowerShell 6'yı aşağıdaki komutla başlatın:

```powershell
Start-Service -Name {NAME}
```

Komut hizmeti başlatmak için birkaç saniye sürer.

### <a name="determine-a-services-status"></a>Bir hizmetin durumunu belirleme

Bir hizmetin durumunu denetlemek için aşağıdaki PowerShell 6 komutu kullanın:

```powershell
Get-Service -Name {NAME}
```

Durumu aşağıdaki değerlerden biri olarak bildirilir:

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a>Bir hizmeti Durdur

Powershell 6'yı aşağıdaki komutla bir hizmet durdurun:

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a>Bir hizmeti Kaldır

Bir hizmeti durdurmak için kısa bir gecikmeyle, hizmet Powershell 6'yı aşağıdaki komutla kaldırın:

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a>Başlatma ve durdurma olayları işleme

İşlenecek <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, ve <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> olayları:

1. Türetilen bir sınıf oluşturmanız <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> ile `OnStarting`, `OnStarted`, ve `OnStopping` yöntemleri:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Bir genişletme yöntemi için oluşturma <xref:Microsoft.AspNetCore.Hosting.IWebHost> Geçiren `CustomWebHostService` için <xref:System.ServiceProcess.ServiceBase.Run*>:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. İçinde `Program.Main`, çağrı `RunAsCustomService` genişletme yöntemi yerine <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:

   ```csharp
   host.RunAsCustomService();
   ```

   Konumu görmek için <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> içinde `Program.Main`, gösterilen kod örneği başvurmak [dağıtım türü](#deployment-type) bölümü.

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Internet'ten veya kurumsal ağ istekleri etkileşim ve bir proxy'nin arkasındaysa veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir. Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>HTTPS yapılandırma

Güvenli bir uç nokta ile bir hizmeti yapılandırmak için:

1. Barındırma system, platformun sertifika edinme ve dağıtım mekanizmalarını kullanarak bir X.509 sertifikası oluşturun.

1. Belirtin bir [Kestrel sunucu HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) sertifikayı kullanmak için.

Hizmet uç noktası güvenli hale getirmek için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmiyor.

## <a name="current-directory-and-content-root"></a>Geçerli dizin ve içerik kök

Geçerli çalışma dizini çağırarak döndürülen <xref:System.IO.Directory.GetCurrentDirectory*> bir Windows hizmeti için *C:\\WINDOWS\\system32* klasör. *System32* klasör değil bir hizmetin dosyaları (örneğin, ayarları) depolamak için uygun bir konum. Korumak ve bir hizmetin varlıklar ve ayar dosyaları erişmek için aşağıdaki yaklaşımlardan birini kullanın.

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>Kullanım ContentRootPath veya ContentRootFileProvider

Kullanım [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) veya <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> uygulamanın kaynaklar bulunamıyor.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Uygulamanın klasör için içerik kök yolu ayarlayın

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Sağlanan aynı yol `binPath` hizmet oluşturulduğunda bağımsız değişken. Çağırmak yerine `GetCurrentDirectory` ayarları dosyalara olan yolları oluşturmak için arama <xref:System.IO.Directory.SetCurrentDirectory*> ile içerik uygulamanın kök yolu.

İçinde `Program.Main`, hizmetin yürütülebilir dosya klasörü yolunu belirlemek ve uygulamanın içerik kök'kurmak için yolunu kullanın:

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a>Disk üzerinde uygun bir konumda bir hizmetin dosyaları Store

Mutlak bir yol belirtin <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> kullanırken bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> dosyaları içeren klasör.

## <a name="additional-resources"></a>Ek kaynaklar

::: moniker range=">= aspnetcore-3.0"

* [Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
