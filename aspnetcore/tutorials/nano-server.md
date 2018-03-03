---
title: "ASP.NET Core Nano Server üzerinde"
author: shirhatti
description: "Mevcut bir ASP.NET Core uygulama ve IIS çalıştıran bir Nano Server örneğine dağıtma hakkında bilgi edinin."
manager: wpickett
ms.author: riande
ms.date: 11/04/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/nano-server
ms.openlocfilehash: 3f234c84d2354a312ad6136b43d8c29aa346ae10
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="a9447-103">Nano Server IIS ile ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="a9447-103">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="a9447-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="a9447-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="a9447-105">Bu öğreticide, var olan bir ASP.NET Core uygulamaya alın ve IIS çalıştıran bir Nano Server örneğine dağıtın.</span><span class="sxs-lookup"><span data-stu-id="a9447-105">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="a9447-106">Giriş</span><span class="sxs-lookup"><span data-stu-id="a9447-106">Introduction</span></span>

<span data-ttu-id="a9447-107">Nano Server, Windows Server 2016 küçük ayak izini, daha iyi güvenlik ve Sunucu Çekirdeği veya tam sunucu daha iyi hizmet sunumu, bir yükleme seçeneğidir.</span><span class="sxs-lookup"><span data-stu-id="a9447-107">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="a9447-108">Lütfen resmi bakın [Nano Server belgelerine](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) daha fazla ayrıntı ve 180 günlük değerlendirme sürümleri için indirme bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="a9447-108">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="a9447-109">Nano Server denemeniz için kolay üç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="a9447-109">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="a9447-110">Ne zaman MS hesabınızla oturum açın:</span><span class="sxs-lookup"><span data-stu-id="a9447-110">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="a9447-111">Windows Server 2016 ISO dosyasını indirin ve Nano Server görüntüsünü oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a9447-111">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="a9447-112">Nano Server VHD indirin.</span><span class="sxs-lookup"><span data-stu-id="a9447-112">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="a9447-113">Azure galerisinde Nano sunucu görüntüsü kullanarak azure'da bir VM oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9447-113">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="a9447-114">Azure ücretsiz deneme hub ' dir.</span><span class="sxs-lookup"><span data-stu-id="a9447-114">A free trial of Azure is avaiable.</span></span>

<span data-ttu-id="a9447-115">Bu öğreticide, biz 2 seçeneği, Windows Server 2016 önceden derlenmiş Nano Server VHD'den kullanacak.</span><span class="sxs-lookup"><span data-stu-id="a9447-115">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="a9447-116">Bu öğretici ile devam etmeden önce ihtiyacınız olacak [çıkış yayımlanan](xref:host-and-deploy/directory-structure) mevcut bir ASP.NET Core uygulama.</span><span class="sxs-lookup"><span data-stu-id="a9447-116">Before proceeding with this tutorial, you will need the [published output](xref:host-and-deploy/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="a9447-117">Uygulamanızı çalıştırmak için oluşturulan olun bir **64-bit** işlemi.</span><span class="sxs-lookup"><span data-stu-id="a9447-117">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="a9447-118">Nano Server örneği ayarlama</span><span class="sxs-lookup"><span data-stu-id="a9447-118">Setting up the Nano Server instance</span></span>

<span data-ttu-id="a9447-119">[Hyper-V kullanarak yeni bir sanal makine oluşturma](https://technet.microsoft.com/library/hh846766.aspx) önceden indirilen VHD kullanarak geliştirme makinenizdeki.</span><span class="sxs-lookup"><span data-stu-id="a9447-119">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="a9447-120">Makine oturum açmadan önce bir yönetici parolası ayarlamanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a9447-120">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="a9447-121">VM konsolda ilk oturum açtığında önce parola ayarlamak için F11 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="a9447-121">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="a9447-122">Yeni VM IP adresini ya da my DHCP sunucunuzun IP sağlanan VM'nizi sağlama sırasında veya Nano Server kurtarma konsolun ağ ayarları sabit denetleme gerekir.</span><span class="sxs-lookup"><span data-stu-id="a9447-122">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="a9447-123">Yeni VM'nin yerel V4 IP adresiyle 192.168.1.10 çalıştıran varsayalım.</span><span class="sxs-lookup"><span data-stu-id="a9447-123">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="a9447-124">Şimdi, Nano Server tam olarak yönetmek için tek yolu PowerShell uzaktan iletişimini kullanarak yönetebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a9447-124">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="a9447-125">PowerShell uzaktan iletişimi kullanarak, Nano Server örneğine bağlanma</span><span class="sxs-lookup"><span data-stu-id="a9447-125">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="a9447-126">Uzak Nano Server örneğinizi eklemek için yükseltilmiş bir PowerShell penceresi açın, `TrustedHosts` listesi.</span><span class="sxs-lookup"><span data-stu-id="a9447-126">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="a9447-127">Değişkeni Değiştir `$nanoServerIpAddress` doğru IP adresine sahip.</span><span class="sxs-lookup"><span data-stu-id="a9447-127">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="a9447-128">Nano Server örneğinizi ekledikten sonra `TrustedHosts`, PowerShell uzaktan iletişimi kullanarak bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a9447-128">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="a9447-129">Başarılı bir bağlantı gibi bir biçim arayan ile bir istem sonuçlanır: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="a9447-129">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="a9447-130">Bir dosya paylaşımı oluşturma</span><span class="sxs-lookup"><span data-stu-id="a9447-130">Creating a file share</span></span>

<span data-ttu-id="a9447-131">Yayımlanan uygulama kopyalanabilir böylece Nano sunucuda bir dosya paylaşımı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9447-131">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="a9447-132">Uzak oturumunda aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a9447-132">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="a9447-133">Yukarıdaki komutları çalıştırdıktan sonra ziyaret ederek bu paylaşıma erişim açabilmelisiniz `\\192.168.1.10\AspNetCoreSampleForNano` ana makine Windows Gezgini'nde.</span><span class="sxs-lookup"><span data-stu-id="a9447-133">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="a9447-134">Güvenlik Duvarı'nda açık bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="a9447-134">Open port in the firewall</span></span>

<span data-ttu-id="a9447-135">Bir bağlantı noktası TCP/8000 bağlantı noktasındaki TCP trafiğini dinler IIS izin vermek için Güvenlik Duvarı'nda açmak amacıyla uzak oturumunda aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a9447-135">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="a9447-136">IIS yükleme</span><span class="sxs-lookup"><span data-stu-id="a9447-136">Installing IIS</span></span>

<span data-ttu-id="a9447-137">Ekleme `NanoServerPackage` PowerShell Galerisi sağlayıcıdan.</span><span class="sxs-lookup"><span data-stu-id="a9447-137">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="a9447-138">Sağlayıcı yüklü ve içeri aktarılan sonra Windows paketlerini yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a9447-138">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="a9447-139">Daha önce oluşturulan PowerShell oturumunda aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a9447-139">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="a9447-140">IIS Kurulum doğru ise, ziyaret URL hızlı bir şekilde doğrulamak için `http://192.168.1.10/` ve Karşılama sayfasını görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a9447-140">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="a9447-141">IIS yüklü olduğunda, bir Web sitesi adı verilen `Default Web Site` dinleme bağlantı noktası 80 üzerinde varsayılan olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a9447-141">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="install-the-aspnet-core-module"></a><span data-ttu-id="a9447-142">ASP.NET Core modülünü yükleme</span><span class="sxs-lookup"><span data-stu-id="a9447-142">Install the ASP.NET Core Module</span></span>

<span data-ttu-id="a9447-143">Bir IIS 7.5 + ASP.NET Core modülüdür yönettiği işlemleri için sorumlu ASP.NET çekirdek HTTP dinleyicilerin ve proxy istekleri için işlem yönetimi modülü.</span><span class="sxs-lookup"><span data-stu-id="a9447-143">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="a9447-144">Şu anda IIS için ASP.NET Core modülünü yüklemek için el ile işlemidir.</span><span class="sxs-lookup"><span data-stu-id="a9447-144">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="a9447-145">Yükleme [.NET Core Windows Server barındırma paket](https://aka.ms/dotnetcore-2-windowshosting) normal üzerinde (değil Nano) makine.</span><span class="sxs-lookup"><span data-stu-id="a9447-145">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on a regular (not Nano) machine.</span></span> <span data-ttu-id="a9447-146">Paket normal bir makineye yükledikten sonra aşağıdaki dosyaları daha önce oluşturduğumuz dosya paylaşımına kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="a9447-146">After installing the bundle on a regular machine, copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="a9447-147">IIS ile normal (değil Nano) sunucusunda, aşağıdaki kopyalama komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a9447-147">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="a9447-148">Değiştir `C:\windows\system32\inetsrv` ile `C:\Program Files\IIS Express` bir Windows 10 makinede.</span><span class="sxs-lookup"><span data-stu-id="a9447-148">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="a9447-149">Nano tarafında, aşağıdaki dosyaları oluşturduğumuz dosya paylaşımından daha önce geçerli konuma kopyalamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a9447-149">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="a9447-150">Bu nedenle, aşağıdaki kopyalama komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a9447-150">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="a9447-151">Uzak oturumda aşağıdaki betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a9447-151">Run the following script in the remote session:</span></span>

[!code-powershell[](nano-server/enable-aspnetcoremodule.ps1)]

> [!NOTE]
> <span data-ttu-id="a9447-152">Dosyaları silmek *aspnetcore.dll* ve *aspnetcore_schema.xml* yukarıdaki adımından sonra paylaşımından.</span><span class="sxs-lookup"><span data-stu-id="a9447-152">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="a9447-153">.NET Core Framework yükleme</span><span class="sxs-lookup"><span data-stu-id="a9447-153">Installing .NET Core Framework</span></span>

<span data-ttu-id="a9447-154">Uygulamanızı olarak yayınlanıyorsa bir [framework bağımlı dağıtım (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core sunucuda yüklü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a9447-154">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="a9447-155">Kullanım [dotnet install.ps1 PowerShell Betiği](https://dot.net/v1/dotnet-install.ps1) bir uzak PowerShell oturumunda .NET Core Nano sunucunuza yükleyin.</span><span class="sxs-lookup"><span data-stu-id="a9447-155">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="a9447-156">CLI sürümüyle geçirmek `-Version` geçin:</span><span class="sxs-lookup"><span data-stu-id="a9447-156">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="a9447-157">Uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="a9447-157">Publishing the application</span></span>

<span data-ttu-id="a9447-158">Var olan dosya paylaşımının kök uygulamanıza yayımlanan çıktısını kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="a9447-158">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="a9447-159">Değişiklikler yapmanız gerekebilir, *web.config* ayıkladığınız için işaret edecek şekilde *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="a9447-159">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="a9447-160">Alternatif olarak, ekleyebileceğiniz *dotnet.exe* YOLUNUZ için.</span><span class="sxs-lookup"><span data-stu-id="a9447-160">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="a9447-161">Nasıl örnek bir *web.config* varsa görünebilir *dotnet.exe* olan **değil** yolunda:</span><span class="sxs-lookup"><span data-stu-id="a9447-161">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

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

<span data-ttu-id="a9447-162">Yeni bir site varsayılan Web sitesi'den farklı bir bağlantı noktasında yayımlanan uygulama için IIS oluşturmak üzere uzak oturumunda aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a9447-162">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="a9447-163">Ayrıca web erişimi için bu bağlantı noktasını açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a9447-163">You also need to open that port to access the web.</span></span> <span data-ttu-id="a9447-164">Bu komut dosyası kullanan `DefaultAppPool` basitleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="a9447-164">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="a9447-165">Çalışan bir uygulama havuzunun altında hakkında daha fazla konuları için bkz: [uygulama havuzları](xref:host-and-deploy/iis/index#application-pools).</span><span class="sxs-lookup"><span data-stu-id="a9447-165">For more considerations on running under an application pool, see [Application Pools](xref:host-and-deploy/iis/index#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="a9447-166">Bilinen sorun Nano sunucuda geçici çözüm .NET Core CLI çalışan</span><span class="sxs-lookup"><span data-stu-id="a9447-166">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="a9447-167">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="a9447-167">Running the application</span></span>

<span data-ttu-id="a9447-168">Yayımlanan web uygulaması bir tarayıcı içinde erişilebilir durumda `http://192.168.1.10:8000`.</span><span class="sxs-lookup"><span data-stu-id="a9447-168">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="a9447-169">Günlük açıklandığı şekilde ayarladıysanız [günlük oluşturma ve yeniden yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), günlüklerinizi adresindeki görüntüleyebilirsiniz *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span><span class="sxs-lookup"><span data-stu-id="a9447-169">If you've set up logging as described in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
