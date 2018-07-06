---
uid: whitepapers/side-by-side-with-10
title: .NET Framework 1.0 ve 1.1 ASP.NET yan yana yürütülmesi | Microsoft Docs
author: rick-anderson
description: Bu teknik incelemede, makinenizde ya da çerçeve sürümünde çalıştırmak bir ASP.NET Web uygulamasına izin verme ve .NET 1.0 ve 1.1 .NET yüklemek açıklar...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1018845e3d2c6603732bfbbde78f4a9183e49d5d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808401"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="c4291-103">.NET Framework 1.0 ve 1.1 ASP.NET yan yana yürütme</span><span class="sxs-lookup"><span data-stu-id="c4291-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="c4291-104">Bu teknik incelemeyi ve .NET 1.0 ve 1.1 .NET framework'ün her iki sürümde de çalıştırmak bir ASP.NET Web uygulamasına izin verme makinenize yükleyin açıklar.</span><span class="sxs-lookup"><span data-stu-id="c4291-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="c4291-105">ASP.NET 1.0 ve 1.1 ASP.NET için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c4291-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="c4291-106">ASP.NET'te, uygulamaları aynı bilgisayarda yüklü ancak .NET Framework'ün farklı sürümlerini kullanan yan yana çalıştırılması söylenir.</span><span class="sxs-lookup"><span data-stu-id="c4291-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="c4291-107">Aşağıdaki konuda yan yana yürütme için ASP.NET uygulamalarının nasıl yapılandırılacağını açıklar ve için ayrıntılı adımlar verilmektedir:</span><span class="sxs-lookup"><span data-stu-id="c4291-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="c4291-108">Yükleme sırasında .NET Framework sürüm 1.0, Web uygulamanızın eşlemesini koru</span><span class="sxs-lookup"><span data-stu-id="c4291-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="c4291-109">.NET Framework'ün belirli bir sürümünü bir Web uygulamasına eşleme</span><span class="sxs-lookup"><span data-stu-id="c4291-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="c4291-110">Bir Web sitesini kullanarak .NET Framework sürümünü bulun</span><span class="sxs-lookup"><span data-stu-id="c4291-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="c4291-111">Geleneksel olarak, bir bilgisayar üzerinde bir bileşen ya da uygulama güncelleştirildiğinde, eski sürüm kaldırılır ve yeni sürümle değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="c4291-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="c4291-112">Yeni sürümü önceki sürümüyle uyumlu değilse, bu genellikle bileşen veya uygulama kullanan diğer uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="c4291-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="c4291-113">.NET Framework sağlayan bir derleme veya aynı anda aynı bilgisayara yüklenecek uygulama birden çok sürümünü yan yana yürütme için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="c4291-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="c4291-114">Birden çok sürümü aynı anda yüklenebildiğinden, yönetilen uygulamaların hangi sürümün farklı bir sürümünü kullanan uygulamaları etkilemeden kullanılacağını seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4291-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="c4291-115">Varsayılan olarak, .NET Framework sürüm 1.1, yükleme sırasında tüm ASP.NET uygulamalarının otomatik olarak .NET Framework'ün en son sürümünü kullanacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="c4291-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="c4291-116">.NET Framework 1. 1'için varsayılan olarak, ASP.NET uygulamaları istemiyorsanız tıklayın [burada](#1) yükleme sırasında bunu önlemek öğrenin.</span><span class="sxs-lookup"><span data-stu-id="c4291-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="c4291-117">Web sunucunuzu güncelleştirmek için .NET Framework 1.1 ve istediğiniz .NET Framework 1.0 çalıştırılacak bir veya daha fazla Web uygulamaları, Internet Information Services (IIS) betik eşlemesi güncelleştirmeniz gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="c4291-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="c4291-118">Betik eşlemesi, .NET Framework sürümü için belirli bir Web uygulaması dosya uzantısı .aspx eşlemek için yönelik mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="c4291-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="c4291-119">Tıklayın [burada](#2) .NET Framework'ün belirli bir sürüme bir Web uygulaması eşlemeyle ilgili bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="c4291-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="c4291-120">Internet Bilgi Yöneticisi'ni veya ASP.NET IIS Kayıt aracını kullanın (Aspnet\_regiis.exe) hangi .NET Framework sürümünü çalıştıran belirli bir Web uygulaması bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="c4291-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="c4291-121">Tıklayın [burada](#3) bir Web sitesini kullanarak .NET Framework sürümünü bulmak öğrenin.</span><span class="sxs-lookup"><span data-stu-id="c4291-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="c4291-122">.NET Framework 1.1 olarak geçiş sırasında bir içeri aktarma göz önünde bulundurarak her .NET Framework sürümü kendi Machine.config dosyasının kullanmasıdır.</span><span class="sxs-lookup"><span data-stu-id="c4291-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="c4291-123">Sonuç olarak, bir Web Yöneticisi Machine.config dosyasına değişiklikler yaptı, bu değişiklikleri .NET Framework 1.1 Machine.config dosyasına geçirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c4291-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="c4291-124">Yükleme sırasında .NET Framework 1.0 için Web uygulamanızın eşlemesini bakımını yapma</span><span class="sxs-lookup"><span data-stu-id="c4291-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="c4291-125">Varsayılan olarak, tüm mevcut ASP.NET uygulamaları, .NET Framework'ün daha yeni sürümü kullanmak için yükleme sırasında otomatik olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="c4291-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="c4291-126">.NET Framework'ün daha yeni sürümünü kullanarak, uygulamaları geliştirmeleri ve yeni sürümde sunulan yeni özelliklerle tam avantajlarından yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4291-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="c4291-127">Aynı anda kimin hangi uygulamaları üzerinde ayrıntılı denetim isteyebilirsiniz Web Yöneticisi güncelleştirilir, otomatik tüm ASP.NET uygulamalarına .NET Framework'ün bir yükleme sırasında yeniden eşleme engelleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4291-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="c4291-128">Otomatik tüm ASP.NET uygulamasının .NET Framework'ün daha yeni sürüme yeniden eşleme önlemek için Web Yöneticisi Dotnetfx.exe Kurulum programını/noaspupgrade komut satırı seçeneğini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4291-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="c4291-129">**ASP.NET uygulamasının daha yeni sürüme toplam yeniden eşleme önlemek için**</span><span class="sxs-lookup"><span data-stu-id="c4291-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="c4291-130">Git **Başlat**.</span><span class="sxs-lookup"><span data-stu-id="c4291-130">Go to **Start**.</span></span>
2. <span data-ttu-id="c4291-131">Tıklayarak **çalıştırma**.</span><span class="sxs-lookup"><span data-stu-id="c4291-131">Click on **run**.</span></span>
3. <span data-ttu-id="c4291-132">Tür **cmd**.</span><span class="sxs-lookup"><span data-stu-id="c4291-132">Type **cmd**.</span></span>
4. <span data-ttu-id="c4291-133">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="c4291-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="c4291-134">Komut İstemi'nden .NET Framework'ün yüklemesini başlatmak için şu satırı girin: **Dotnetfx.exe/c: "/ noaspupgrade yükleme?**.</span><span class="sxs-lookup"><span data-stu-id="c4291-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="c4291-135">Tıklayın **Evet** Microsoft .NET Framework 1.1 kurulumunda.</span><span class="sxs-lookup"><span data-stu-id="c4291-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="c4291-136">Bu, .NET Framework 1.1 Kurulum işlemi başlatır.</span><span class="sxs-lookup"><span data-stu-id="c4291-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="c4291-137">.NET Framework'ün belirli bir sürümünü bir Web uygulamasına eşleme</span><span class="sxs-lookup"><span data-stu-id="c4291-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="c4291-138">ASP.NET IIS Kayıt Aracı sürümü .NET Framework'ün her sürümü içerir (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="c4291-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="c4291-139">Bu araç, bir Web uygulaması belirli bir .NET Framework sürümünün altında çalıştırılması yöneticilerin sağlar.</span><span class="sxs-lookup"><span data-stu-id="c4291-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="c4291-140">Bu .NET Framework sürümü için bir Web uygulaması eşleme olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="c4291-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="c4291-141">Yöneticiler, ASP.NET seçmelisiniz\_Web uygulaması ile ilişkilendirilecek olan .NET Framework sürümüne karşılık gelen regiis.exe.</span><span class="sxs-lookup"><span data-stu-id="c4291-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="c4291-142">Örneğin, bir Web sitesi .NET Framework 1.1 kullandığını belirtmek için isteyen bir yönetici Aspnet kullanmalısınız\_.NET Framework 1.1 ile birlikte gelen regiis.exe.</span><span class="sxs-lookup"><span data-stu-id="c4291-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="c4291-143">ASP.NET\_regiis.exe sürüm 1.0 için konumu:</span><span class="sxs-lookup"><span data-stu-id="c4291-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="c4291-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="c4291-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="c4291-145">ASP.NET\_regiis.exe sürümünün 1,1 yer:</span><span class="sxs-lookup"><span data-stu-id="c4291-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="c4291-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="c4291-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="c4291-147">ASP.NET\_regiis.exe betik eşlemesi bir Web uygulaması için iki seçenek sağlar:</span><span class="sxs-lookup"><span data-stu-id="c4291-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="c4291-148">**-s** ayarlar betik eşlemesi yolundaki ve alt dizinleri.</span><span class="sxs-lookup"><span data-stu-id="c4291-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="c4291-149">**-sn** yolda yalnızca betik eşlemesi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c4291-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="c4291-150">W3SVC/ROOT biçiminde tanımlanan Web uygulamasını IIS meta veri yolu yolunu tanımlar / {WebSiteNumber} / {uygulama\_adı}.</span><span class="sxs-lookup"><span data-stu-id="c4291-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="c4291-151">Örneğin, bir Web uygulaması için varsayılan Web sitesi altında bulunan portalı adlı metataban yolu W3SVC/1/kök/Portal ' dir.</span><span class="sxs-lookup"><span data-stu-id="c4291-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="c4291-152">Metatabanı yolu metatabanı düzenleyici olarak adlandırılan bir araç kullanabilirsiniz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c4291-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="c4291-153">Bu araç Microsoft Support sitesini yükleyebilir [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="c4291-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="c4291-154">ASP.NET çalıştırma\_Haritası ve kendi subapplication regiis.exe -s W3SVC/1/kök/IIS portalı güncelleştirmek için Portal komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="c4291-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="c4291-155">ASP.NET çalıştırma\_regiis.exe -sn W3SVC/1/kök/portal IIS betik güncelleştirmek için Portal harita, portal uygulamaları etkilemeden? s alt dizinleri.</span><span class="sxs-lookup"><span data-stu-id="c4291-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="c4291-156">Bir Web uygulaması kullanarak .NET Framework sürümünü bulun</span><span class="sxs-lookup"><span data-stu-id="c4291-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="c4291-157">Yönetici, hangi .NET Framework sürümünü çalıştıran bir Web sitesi bulmak için Internet Hizmet Yöneticisi'ni kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4291-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="c4291-158">Farklı işletim sistemi sürümleri, farklı Internet Hizmet Yöneticisi'ni başlatın.</span><span class="sxs-lookup"><span data-stu-id="c4291-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="c4291-159">Hizmet Yöneticisi'ni başlatmak için aşağıda listelenen adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="c4291-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="c4291-160">**Internet Hizmet Yöneticisi'ni başlatmak için**</span><span class="sxs-lookup"><span data-stu-id="c4291-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="c4291-161">Git **Başlat**.</span><span class="sxs-lookup"><span data-stu-id="c4291-161">Go to **Start**.</span></span>
2. <span data-ttu-id="c4291-162">Tıklayarak **çalıştırma**.</span><span class="sxs-lookup"><span data-stu-id="c4291-162">Click on **run**.</span></span>
3. <span data-ttu-id="c4291-163">Tür **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="c4291-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="c4291-164">Internet Hizmet Yöneticisi'nden, .NET Framework sürümü bilmek istediğiniz Web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="c4291-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="c4291-165">Web uygulamasına sağ tıklayın ve tıklayarak **özellikleri.**</span><span class="sxs-lookup"><span data-stu-id="c4291-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="c4291-166">Özellik penceresinden seçmek **yapılandırma.**</span><span class="sxs-lookup"><span data-stu-id="c4291-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="c4291-167">Uygulama eşlemesi tablosundan seçin **.aspx**, tıklatıp **Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="c4291-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="c4291-168">Gelen **yürütülebilir** metin kutusuna kaydırarak sürümü dizininden bakın.</span><span class="sxs-lookup"><span data-stu-id="c4291-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="c4291-169">Sürüm dizini v.1.1.4322 ise, uygulama için .NET Framework 1.1 eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c4291-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="c4291-170">Buna karşılık, sürüm dizini v1.0.3705 ise, uygulama için .NET Framework 1.0 eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c4291-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
