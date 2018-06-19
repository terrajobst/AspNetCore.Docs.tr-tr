---
uid: whitepapers/side-by-side-with-10
title: .NET Framework 1.0 ve 1.1 ASP.NET yan yana yürütme | Microsoft Docs
author: rick-anderson
description: Bu teknik ya da çerçeve sürümünde çalıştırmak bir ASP.NET Web uygulamasına izin vererek, makinenizde .NET 1.0 ve .NET 1.1 yüklemeyi açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573360"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="15433-103">.NET Framework 1.0 ve 1.1 ASP.NET yan yana yürütme</span><span class="sxs-lookup"><span data-stu-id="15433-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="15433-104">Bu teknik inceleme, .NET 1.0 ve 1.1 .NET framework'ün her iki sürümde de çalıştırmak bir ASP.NET Web uygulamasına izin verme makinenizde yüklemek açıklar.</span><span class="sxs-lookup"><span data-stu-id="15433-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="15433-105">ASP.NET 1.0 ve 1.1 ASP.NET için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="15433-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="15433-106">ASP.NET, uygulamaların aynı bilgisayarda yüklü olan ancak .NET Framework'ün farklı sürümlerini kullanan yan yana çalıştırılması söylenir.</span><span class="sxs-lookup"><span data-stu-id="15433-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="15433-107">Aşağıdaki konu yan yana yürütme için ASP.NET uygulamalarının nasıl yapılandırılacağını açıklar ve ayrıntılı adımlar için sağlar:</span><span class="sxs-lookup"><span data-stu-id="15433-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="15433-108">Yükleme sırasında Web uygulamanızın eşleme .NET Framework sürüm 1.0 için koru</span><span class="sxs-lookup"><span data-stu-id="15433-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="15433-109">Bir Web uygulaması .NET Framework'ün belirli bir sürüme eşleme</span><span class="sxs-lookup"><span data-stu-id="15433-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="15433-110">Bir Web sitesini kullanarak .NET Framework sürümü bulunamıyor</span><span class="sxs-lookup"><span data-stu-id="15433-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="15433-111">Geleneksel olarak, bir bilgisayarda bir bileşeni ya da uygulama güncelleştirildiğinde, eski sürüm kaldırılır ve yeni sürüm ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="15433-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="15433-112">Yeni sürüm önceki sürümü ile uyumlu değilse, bu genellikle bileşen veya uygulamayı kullanan diğer uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="15433-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="15433-113">.NET Framework birden fazla sürümünü bir derlemeyi ya da uygulamanın aynı bilgisayarda aynı anda yüklenmesine izin veren yan yana yürütme için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="15433-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="15433-114">Birden çok sürümü aynı anda yüklenebildiğinden, yönetilen uygulamaların hangi sürümün farklı bir sürümünü kullanan uygulamaları etkilemeden kullanılacağını seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15433-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="15433-115">Varsayılan olarak, .NET Framework sürüm 1.1, yükleme sırasında tüm mevcut ASP.NET uygulamalarını otomatik olarak .NET Framework'ün en son sürümünü kullanacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="15433-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="15433-116">.NET Framework 1.1 için varsayılan olarak, ASP.NET uygulamalarınızın istemiyorsanız tıklayın [burada](#1) yükleme sırasında bunu önlemek hakkında bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="15433-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="15433-117">Web sunucunuzun .NET Framework 1.1 güncelleştirmek ve .NET Framework 1.0 çalıştırmak için bir veya daha fazla Web uygulaması istiyorsanız Internet Information Services (IIS) betik eşlemesi güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="15433-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="15433-118">Komut dosyası eşlemeleri .NET Framework sürümü için belirli bir Web uygulaması için .aspx dosya uzantısı eşlemek için mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="15433-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="15433-119">Tıklatın [burada](#2) belirli bir .NET Framework sürümünü bir Web uygulamasına eşlenen öğrenin.</span><span class="sxs-lookup"><span data-stu-id="15433-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="15433-120">Internet Bilgi Yöneticisi veya ASP.NET IIS Kayıt Aracı'nı kullanabilirsiniz (Aspnet\_regiis.exe) belirli bir Web uygulamasına hangi .NET Framework sürümü bulunamıyor.</span><span class="sxs-lookup"><span data-stu-id="15433-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="15433-121">Tıklatın [burada](#3) bir Web sitesini kullanarak .NET Framework sürümünü bulmak nasıl öğrenin.</span><span class="sxs-lookup"><span data-stu-id="15433-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="15433-122">.NET Framework 1.1 geçirirken bir alma göz önünde bulundurarak her .NET Framework sürümü kendi Machine.config dosyasının kullanmasıdır.</span><span class="sxs-lookup"><span data-stu-id="15433-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="15433-123">Sonuç olarak, Web Yöneticisi Machine.config dosyasının değişiklikler yaptı, bu değişiklikleri için .NET Framework 1.1 Machine.config dosyasının geçirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="15433-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="15433-124">Yükleme sırasında Web uygulamanızın eşleme .NET Framework 1.0 için koruma</span><span class="sxs-lookup"><span data-stu-id="15433-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="15433-125">Varsayılan olarak, tüm ASP.NET uygulamalarının .NET Framework'ün daha yeni sürümü kullanmak için yükleme sırasında otomatik olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="15433-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="15433-126">.NET Framework'in daha yeni sürümünü kullanan, uygulamaları tam geliştirmeleri ve yeni sürümde sunulan yeni özelliklerle yararlanabilir.</span><span class="sxs-lookup"><span data-stu-id="15433-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="15433-127">Aynı zamanda, hangi uygulamalar üzerinde ayrıntılı denetim isteyebilirsiniz yönetici Web güncelleştirilir, otomatik varolan tüm ASP.NET uygulamalarına .NET Framework'ün bir yükleme sırasında yeniden eşleme engelleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15433-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="15433-128">Tüm ASP.NET uygulama .NET Framework sürümü için otomatik yeniden önlemek için Web Yöneticisi Dotnetfx.exe Kurulum programını/noaspupgrade komut satırı seçeneğini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15433-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="15433-129">**ASP.NET uygulamasının daha yeni sürüme toplam yeniden eşleme engellemek için**</span><span class="sxs-lookup"><span data-stu-id="15433-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="15433-130">Git **Başlat**.</span><span class="sxs-lookup"><span data-stu-id="15433-130">Go to **Start**.</span></span>
2. <span data-ttu-id="15433-131">Tıklayın **çalıştırmak**.</span><span class="sxs-lookup"><span data-stu-id="15433-131">Click on **run**.</span></span>
3. <span data-ttu-id="15433-132">Tür **cmd**.</span><span class="sxs-lookup"><span data-stu-id="15433-132">Type **cmd**.</span></span>
4. <span data-ttu-id="15433-133">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="15433-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="15433-134">.NET Framework'ün yüklemeyi başlatmak için aşağıdaki satırı komut isteminde aşağıdakileri yazın: **Dotnetfx.exe. / c: "/ noaspupgrade yükleme?**.</span><span class="sxs-lookup"><span data-stu-id="15433-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="15433-135">Tıklatın **Evet** Microsoft .NET Framework 1.1 kurulumunda.</span><span class="sxs-lookup"><span data-stu-id="15433-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="15433-136">Bu, .NET Framework 1.1 Kurulum işlemini başlatır.</span><span class="sxs-lookup"><span data-stu-id="15433-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="15433-137">Bir Web uygulaması .NET Framework'ün belirli bir sürüme eşleme</span><span class="sxs-lookup"><span data-stu-id="15433-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="15433-138">ASP.NET IIS Kayıt Aracı sürümünü her .NET Framework sürümü içeriyor (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="15433-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="15433-139">Bu araç, yöneticilerin bir Web uygulaması belirli bir .NET Framework sürümü altında çalıştırılması gerektiğini belirtin olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="15433-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="15433-140">Bu için .NET Framework sürümü için bir Web uygulaması eşleme olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="15433-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="15433-141">Yöneticiler, Aspnet seçmelisiniz\_Web uygulamasıyla ilişkilendirilmesi .NET Framework sürümünü karşılık gelen regiis.exe.</span><span class="sxs-lookup"><span data-stu-id="15433-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="15433-142">Örneğin, bir Web sitesi .NET Framework 1.1 kullanacağını belirtin isteyen bir yönetici Aspnet kullanmalısınız\_.NET Framework 1.1 ile birlikte gelen regiis.exe.</span><span class="sxs-lookup"><span data-stu-id="15433-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="15433-143">Aspnet\_sürüm 1.0 için regiis.exe adresindedir:</span><span class="sxs-lookup"><span data-stu-id="15433-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="15433-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="15433-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="15433-145">Aspnet\_sürüm 1,1 regiis.exe adresindedir:</span><span class="sxs-lookup"><span data-stu-id="15433-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="15433-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="15433-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="15433-147">Aspnet\_regiis.exe bir Web uygulaması eşleme komut dosyası için iki seçenek sunar:</span><span class="sxs-lookup"><span data-stu-id="15433-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="15433-148">**-s** ayarlar betik eşlemesi yolundaki ve alt dizinleri.</span><span class="sxs-lookup"><span data-stu-id="15433-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="15433-149">**-sn** betik eşlemesi yalnızca yolunda ayarlar.</span><span class="sxs-lookup"><span data-stu-id="15433-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="15433-150">W3SVC/ROOT formunda tanımlanan Web uygulama IIS meta veri yolu yolunu tanımlar / {WebSiteNumber} / {uygulama\_adı}.</span><span class="sxs-lookup"><span data-stu-id="15433-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="15433-151">Örneğin, varsayılan Web sitesi altında bulunan portalı adlı bir Web uygulaması için metatabanı W3SVC/1/kök/Portal yoludur.</span><span class="sxs-lookup"><span data-stu-id="15433-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="15433-152">Metatabanı yolu metatabanı düzenleyici olarak adlandırılan bir aracı kullanabilirsiniz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="15433-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="15433-153">Bu araç Microsoft Support sitesini indirebileceğiniz [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="15433-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="15433-154">ASPNET çalıştırmak\_regiis.exe -s W3SVC/1/kök/portal IIS güncelleştirmek için Portal harita ve kendi subapplication komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="15433-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="15433-155">ASPNET çalıştırmak\_regiis.exe -sn W3SVC/1/kök/portal IIS komut dosyası güncelleştirmek için Portal eşleme, portal uygulamaları etkilemeden? s alt dizinleri.</span><span class="sxs-lookup"><span data-stu-id="15433-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="15433-156">Bir Web uygulaması kullanarak .NET Framework sürümünü bulma</span><span class="sxs-lookup"><span data-stu-id="15433-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="15433-157">Bir yönetici, hangi .NET Framework sürümünü çalıştıran bir Web sitesi bulmak için Internet Hizmet Yöneticisi'ni kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15433-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="15433-158">Farklı işletim sistemi sürümleri farklı Internet Hizmet Yöneticisi'ni başlatın.</span><span class="sxs-lookup"><span data-stu-id="15433-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="15433-159">Hizmet Yöneticisi'ni başlatmak için aşağıda listelenen adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="15433-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="15433-160">**Internet Hizmet Yöneticisi'ni başlatmak için**</span><span class="sxs-lookup"><span data-stu-id="15433-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="15433-161">Git **Başlat**.</span><span class="sxs-lookup"><span data-stu-id="15433-161">Go to **Start**.</span></span>
2. <span data-ttu-id="15433-162">Tıklayın **çalıştırmak**.</span><span class="sxs-lookup"><span data-stu-id="15433-162">Click on **run**.</span></span>
3. <span data-ttu-id="15433-163">Tür **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="15433-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="15433-164">Internet Hizmet Yöneticisi'nden, .NET Framework sürümünü bilmek ister Web uygulaması seçin.</span><span class="sxs-lookup"><span data-stu-id="15433-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="15433-165">Web uygulamasına sağ tıklayın ve tıklayın **özellikleri.**</span><span class="sxs-lookup"><span data-stu-id="15433-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="15433-166">Özellik penceresinden seçin **yapılandırma.**</span><span class="sxs-lookup"><span data-stu-id="15433-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="15433-167">Uygulama eşleme tablosundan seçmek **.aspx**, tıklatıp **Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="15433-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="15433-168">Gelen **yürütülebilir** metin kutusuna, kaydırarak sürüm dizini bakın.</span><span class="sxs-lookup"><span data-stu-id="15433-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="15433-169">Sürüm dizini v.1.1.4322 ise, uygulama için .NET Framework 1.1 eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="15433-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="15433-170">Buna karşılık, sürüm dizini v1.0.3705 ise, uygulama için .NET Framework 1.0 eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="15433-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
