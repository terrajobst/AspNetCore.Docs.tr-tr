---
title: "ASP.NET Core Nano Server üzerinde"
author: shirhatti
description: "Mevcut bir ASP.NET Core uygulama ve IIS çalıştıran bir Nano Server örneğine dağıtma hakkında bilgi edinin."
keywords: ASP.NET Core, nano server
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: 337cc69ef522452c17cdd6ea4a5e71cd122035dc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-with-iis-on-nano-server"></a>Nano Server IIS ile ASP.NET Çekirdeği

Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Bu öğreticide, var olan bir ASP.NET Core uygulamaya alın ve IIS çalıştıran bir Nano Server örneğine dağıtın.

## <a name="introduction"></a>Giriş

Nano Server, Windows Server 2016 küçük ayak izini, daha iyi güvenlik ve Sunucu Çekirdeği veya tam sunucu daha iyi hizmet sunumu, bir yükleme seçeneğidir. Lütfen resmi bakın [Nano Server belgelerine](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) daha fazla ayrıntı ve 180 günlük değerlendirme sürümleri için indirme bağlantıları. 

Nano Server denemeniz için kolay üç yolu vardır. Ne zaman MS hesabınızla oturum açın:

1. Windows Server 2016 ISO dosyasını indirin ve Nano Server görüntüsünü oluşturabilirsiniz.

2. Nano Server VHD indirin.

3. Azure galerisinde Nano sunucu görüntüsü kullanarak azure'da bir VM oluşturun. Bir Azure hesabınız yoksa, ücretsiz 30 günlük deneme alabilirsiniz.

Bu öğreticide, biz 2 seçeneği, Windows Server 2016 önceden derlenmiş Nano Server VHD'den kullanacak.

Bu öğretici ile devam etmeden önce ihtiyacınız olacak [çıkış yayımlanan](xref:hosting/directory-structure) mevcut bir ASP.NET Core uygulama. Uygulamanızı çalıştırmak için oluşturulan olun bir **64-bit** işlemi.

## <a name="setting-up-the-nano-server-instance"></a>Nano Server örneği ayarlama

[Hyper-V kullanarak yeni bir sanal makine oluşturma](https://technet.microsoft.com/library/hh846766.aspx) önceden indirilen VHD kullanarak geliştirme makinenizdeki. Makine oturum açmadan önce bir yönetici parolası ayarlamanızı gerektirir. VM konsolda ilk oturum açtığında önce parola ayarlamak için F11 tuşuna basın. Yeni VM IP adresini ya da my DHCP sunucunuzun IP sağlanan VM'nizi sağlama sırasında veya Nano Server kurtarma konsolun ağ ayarları sabit denetleme gerekir.

> [!NOTE]
> Yeni VM'nin yerel V4 IP adresiyle 192.168.1.10 çalıştıran varsayalım.

Şimdi, Nano Server tam olarak yönetmek için tek yolu PowerShell uzaktan iletişimini kullanarak yönetebilirsiniz.

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>PowerShell uzaktan iletişimi kullanarak, Nano Server örneğine bağlanma

Uzak Nano Server örneğinizi eklemek için yükseltilmiş bir PowerShell penceresi açın, `TrustedHosts` listesi.

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> Değişkeni Değiştir `$nanoServerIpAddress` doğru IP adresine sahip.

Nano Server örneğinizi ekledikten sonra `TrustedHosts`, PowerShell uzaktan iletişimi kullanarak bağlanabilir.

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

Başarılı bir bağlantı gibi bir biçim arayan ile bir istem sonuçlanır:`[192.168.1.10]: PS C:\Users\Administrator\Documents>`

## <a name="creating-a-file-share"></a>Bir dosya paylaşımı oluşturma

Yayımlanan uygulama kopyalanabilir böylece Nano sunucuda bir dosya paylaşımı oluşturun. Uzak oturumunda aşağıdaki komutları çalıştırın:

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

Yukarıdaki komutları çalıştırdıktan sonra ziyaret ederek bu paylaşıma erişim açabilmelisiniz `\\192.168.1.10\AspNetCoreSampleForNano` ana makine Windows Gezgini'nde.

## <a name="open-port-in-the-firewall"></a>Güvenlik Duvarı'nda açık bağlantı noktası

Bir bağlantı noktası TCP/8000 bağlantı noktasındaki TCP trafiğini dinler IIS izin vermek için Güvenlik Duvarı'nda açmak amacıyla uzak oturumunda aşağıdaki komutları çalıştırın.

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>IIS yükleme

Ekleme `NanoServerPackage` PowerShell Galerisi sağlayıcıdan. Sağlayıcı yüklü ve içeri aktarılan sonra Windows paketlerini yükleyebilirsiniz.

Daha önce oluşturulan PowerShell oturumunda aşağıdaki komutları çalıştırın:

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

IIS Kurulum doğru ise, ziyaret URL hızlı bir şekilde doğrulamak için `http://192.168.1.10/` ve Karşılama sayfasını görmeniz gerekir. IIS yüklü olduğunda, bir Web sitesi adı verilen `Default Web Site` dinleme bağlantı noktası 80 üzerinde varsayılan olarak oluşturulur.

## <a name="installing-the-aspnet-core-module-ancm"></a>ASP.NET çekirdeği Modülü (ANCM) yükleniyor

Bir IIS 7.5 + ASP.NET Core modülüdür yönettiği işlemleri için sorumlu ASP.NET çekirdek HTTP dinleyicilerin ve proxy istekleri için işlem yönetimi modülü. Şu anda IIS için ASP.NET Core modülünü yüklemek için el ile işlemidir. Yüklemeniz gerekecek [.NET Core Windows Server barındırma paket](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) normal üzerinde (değil Nano) makine. Paket normal bir makineye yükledikten sonra aşağıdaki dosyaları daha önce oluşturduğumuz dosya paylaşımına kopyalamanız gerekir.

IIS ile normal (değil Nano) sunucusunda, aşağıdaki kopyalama komutları çalıştırın:

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

Değiştir `C:\windows\system32\inetsrv` ile `C:\Program Files\IIS Express` bir Windows 10 makinede.

Nano tarafında, aşağıdaki dosyaları oluşturduğumuz dosya paylaşımından daha önce geçerli konuma kopyalamanız gerekir. Bu nedenle, aşağıdaki kopyalama komutları çalıştırın:

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

Uzak oturumda aşağıdaki betiği çalıştırın:

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> Dosyaları silmek *aspnetcore.dll* ve *aspnetcore_schema.xml* yukarıdaki adımından sonra paylaşımından.

## <a name="installing-net-core-framework"></a>.NET Core Framework yükleme

Uygulamanızı olarak yayınlanıyorsa bir [framework bağımlı dağıtım (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core sunucuda yüklü olmalıdır. Kullanım [dotnet install.ps1 PowerShell Betiği](https://dot.net/v1/dotnet-install.ps1) bir uzak PowerShell oturumunda .NET Core Nano sunucunuza yükleyin. CLI sürümüyle geçirmek `-Version` geçin:

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>Uygulama yayımlama

Var olan dosya paylaşımının kök uygulamanıza yayımlanan çıktısını kopyalayın.

Değişiklikler yapmanız gerekebilir, *web.config* ayıkladığınız için işaret edecek şekilde *dotnet.exe*. Alternatif olarak, ekleyebileceğiniz *dotnet.exe* YOLUNUZ için.

Nasıl örnek bir *web.config* varsa görünebilir *dotnet.exe* olan **değil** yolunda:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

Yeni bir site varsayılan Web sitesi'den farklı bir bağlantı noktasında yayımlanan uygulama için IIS oluşturmak üzere uzak oturumunda aşağıdaki komutları çalıştırın. Ayrıca web erişimi için bu bağlantı noktasını açmanız gerekir. Bu komut dosyası kullanan `DefaultAppPool` basitleştirmek için. Çalışan bir uygulama havuzunun altında hakkında daha fazla konuları için bkz: [uygulama havuzları](xref:publishing/iis#application-pools).

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>Bilinen sorun Nano sunucuda geçici çözüm .NET Core CLI çalışan
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Yayımlanan web uygulaması bir tarayıcı içinde erişilebilir durumda `http://192.168.1.10:8000`. Günlük açıklandığı şekilde ayarladıysanız [günlük oluşturma ve yeniden yönlendirme](xref:hosting/aspnet-core-module#log-creation-and-redirection), günlüklerinizi adresindeki görüntüleyebilirsiniz *C:\PublishedApps\AspNetCoreSampleForNano\logs*.
