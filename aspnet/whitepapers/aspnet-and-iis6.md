---
uid: whitepapers/aspnet-and-iis6
title: IIS 6.0 ile ASP.NET 1.1 çalıştıran | Microsoft Docs
author: rick-anderson
description: Windows Server 2003, IIS 6.0 ve ASP.NET 1.1 içerir, ancak bu bileşenler varsayılan olarak devre dışıdır. Bu teknik, IIS 6.0 etkinleştirmeyi açıklar bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573339"
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="0495c-104">IIS 6.0 ile çalışan ASP.NET 1.1</span><span class="sxs-lookup"><span data-stu-id="0495c-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="0495c-105">Windows Server 2003, IIS 6.0 ve ASP.NET 1.1 içerir, ancak bu bileşenler varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="0495c-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="0495c-106">Bu teknik inceleme, IIS 6.0 ve ASP.NET 1.1 etkinleştirmeyi açıklar ve IIS ve ASP.NET en iyi performans almak için birkaç yapılandırma ayarlarını önerir.</span><span class="sxs-lookup"><span data-stu-id="0495c-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="0495c-107">ASP.NET 1.1 ve IIS 6.0 için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0495c-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="0495c-108">ASP.NET 1.1, Windows Server Internet Information Server (IIS) sürüm 6.0, en son sürümünü de içeren 2003 ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="0495c-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="0495c-109">IIS 6.0 ve ASP.NET 1.1 sorunsuz bir şekilde tümleştirmek için tasarlanmıştır ve ASP.NET Şimdi yeni IIS 6.0 çalışan işlem modeli varsayılan olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0495c-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="0495c-110">ASP.NET 1.1 varsayılan olarak yüklü değil</span><span class="sxs-lookup"><span data-stu-id="0495c-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="0495c-111">Microsoft'un sunucu işletim sistemlerinin önceki sürümlerden farklı olarak, Internet Information Server (IIS) varsayılan olarak etkin değildir; veya ASP.NET 1.1 değil.</span><span class="sxs-lookup"><span data-stu-id="0495c-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="0495c-112">IIS etkinleştirmek için iki seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="0495c-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="0495c-113">IIS, seçeneği #1 - Sunucu Yapılandırma Sihirbazı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="0495c-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="0495c-114">Windows Server 2003 bir yeni ', sunucu yapılandırma sunucunuz istenen modunda düzgün şekilde yapılandırmanıza yardımcı olması için Sihirbazı' gelir.</span><span class="sxs-lookup"><span data-stu-id="0495c-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="0495c-115">-Sihirbazını başlatmak için oturum açmış olmanız gerekir bir yönetici olarak - Sihirbazı'nı çalıştırmak için Not gitmek için: Başlat | Programlar | Yönetim Araçları ve Seç 'sunucunuzu yapılandırın'.</span><span class="sxs-lookup"><span data-stu-id="0495c-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="0495c-116">Bir kez seçili ', Sunucu Yapılandırma Sihirbazı' Açılış ekranında görmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="0495c-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="0495c-117">Tıklatın ' sonraki &gt;':</span><span class="sxs-lookup"><span data-stu-id="0495c-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="0495c-118">Tıklatın ' sonraki &gt;'</span><span class="sxs-lookup"><span data-stu-id="0495c-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="0495c-119">Bu ekranda seçmeniz gerekir ' uygulama sunucusu (IIS, ASP.NET) olarak yapılandırmak için Seçenekler.</span><span class="sxs-lookup"><span data-stu-id="0495c-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="0495c-120">Tıklatın ' sonraki &gt;'.</span><span class="sxs-lookup"><span data-stu-id="0495c-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="0495c-121">Sunucuyu uygulama sunucusu olarak yapılandırmak için seçtikten sonra bu ekranı ek yetenekler yüklenmelidir isteyen görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0495c-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="0495c-122">Her iki seçenek varsayılan olarak seçilidir.</span><span class="sxs-lookup"><span data-stu-id="0495c-122">Neither option is selected by default.</span></span> <span data-ttu-id="0495c-123">ASP.NET otomatik olarak etkinleştirmek için seçmeniz gerekir ' ASP etkinleştirin. NET'.</span><span class="sxs-lookup"><span data-stu-id="0495c-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="0495c-124">Tıklatın ' sonraki &gt;'.</span><span class="sxs-lookup"><span data-stu-id="0495c-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="0495c-125">Bu ekran, yüklenecek seçenekleri nelerdir görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0495c-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="0495c-126">Tıklatın ' sonraki &gt;'.</span><span class="sxs-lookup"><span data-stu-id="0495c-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="0495c-127">Seçtiğiniz seçeneklerini yüklenirken bu ekran görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="0495c-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="0495c-128">Diğer iletişim kutuları Hizmetleri yüklü olarak görünmesini görmek için normal bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="0495c-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="0495c-129">Ayrıca Windows Server 2003 CD'sini konumunu belirtmeniz istenebilir.</span><span class="sxs-lookup"><span data-stu-id="0495c-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="0495c-130">Tıklatın ' sonraki &gt;' tamamlandığında.</span><span class="sxs-lookup"><span data-stu-id="0495c-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="0495c-131">'Son' düğmesini tıklatın - Windows Server 2003 IIS 6.0 ve ASP.NET 1.1 desteklemek üzere yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="0495c-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="0495c-132">IIS, #2 - el ile IIS ve ASP.NET yapılandırma seçeneğini etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="0495c-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="0495c-133">', Sunucu Yapılandırma Sihirbazı' kullanmak istemiyorsanız, isteğe bağlı olarak IIS 6.0 ve ASP.NET 'Ekle veya Kaldır' kullanarak 1.1 Denetim Masası'ndan yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0495c-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="0495c-134">İlk Denetim Masası'nı açın:</span><span class="sxs-lookup"><span data-stu-id="0495c-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="0495c-135">Ardından, '/ Windows Bileşenlerini Ekle Kaldır 'Windows Bileşenleri Sihirbazı' açın üzerinde' tıklatın:</span><span class="sxs-lookup"><span data-stu-id="0495c-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="0495c-136">Vurgulayın ve 'Uygulama sunucusu' denetleyin ve sonra '?' Detaylar</span><span class="sxs-lookup"><span data-stu-id="0495c-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="0495c-137">düğmesi:</span><span class="sxs-lookup"><span data-stu-id="0495c-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="0495c-138">ASP.NET yüklemek için kontrol ' ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="0495c-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="0495c-139">Windows Bileşen Sihirbazı dönmek için ' Tamam' ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="0495c-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="0495c-140">Tıklatın ' sonraki &gt;' yüklemeye başlamak için Windows Bileşen Sihirbazı:</span><span class="sxs-lookup"><span data-stu-id="0495c-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="0495c-141">Diğer iletişim kutuları Hizmetleri yüklü olarak görünmesini görmek için normal bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="0495c-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="0495c-142">Ayrıca Windows Server 2003 CD'sini konumunu belirtmeniz istenebilir.</span><span class="sxs-lookup"><span data-stu-id="0495c-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="0495c-143">Yükleme tamamlandığında Windows Bileşen Sihirbazı'nın son ekranı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="0495c-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="0495c-144">IIS 6.0 ve ASP.NET 1.1 şimdi yapılandırılır ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0495c-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="0495c-145">Önerilen ayarları</span><span class="sxs-lookup"><span data-stu-id="0495c-145">Recommended Settings</span></span>

<span data-ttu-id="0495c-146">ASP.NET 1.1 IIS 6.0 ile çalışırken ASP.NET tarafından en iyi performansı elde etmek için önerilen çeşitli yapılandırma ayarları vardır:</span><span class="sxs-lookup"><span data-stu-id="0495c-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="0495c-147">Yapılandırma çalışan işlem bellek sınırları</span><span class="sxs-lookup"><span data-stu-id="0495c-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="0495c-148">Çalışan işlem geri dönüştürme yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0495c-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="0495c-149">Yapılandırma çalışan işlem bellek sınırları</span><span class="sxs-lookup"><span data-stu-id="0495c-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="0495c-150">Varsayılan olarak IIS 6.0 IIS kullanmasına izin verilen bellek miktarı sınırı ayarlamaz.</span><span class="sxs-lookup"><span data-stu-id="0495c-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="0495c-151">ASP. Önbelleği önceden bellekten kullanılmayan öğeleri kaldırabilmeniz için NET'in önbellek özelliğinin bellek sınırlaması kullanır.</span><span class="sxs-lookup"><span data-stu-id="0495c-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="0495c-152">IIS 6.0 özelliğini geri dönüştürme belleği yapılandırma önerilir.</span><span class="sxs-lookup"><span data-stu-id="0495c-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="0495c-153">Bu açık Internet Information Services Yöneticisi yapılandırmak için (Başlat | Programlar | Yönetimsel Araçlar | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="0495c-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="0495c-154">Açık bir kez 'Uygulama havuzları' klasörünü genişletin:</span><span class="sxs-lookup"><span data-stu-id="0495c-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="0495c-155">Her bir uygulama havuzu için:</span><span class="sxs-lookup"><span data-stu-id="0495c-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="0495c-156">Uygulama havuzunda, örneğin sağ tıklayın 'DefaultAppPool' ve 'Özellikler' seçin:</span><span class="sxs-lookup"><span data-stu-id="0495c-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="0495c-157">Ardından, bellek üzerinde tıklayarak geri dönüşümü Etkinleştir ' en çok kullanılan bellek (megabayt cinsinden):'.</span><span class="sxs-lookup"><span data-stu-id="0495c-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="0495c-158">Değeri sunucuda fiziksel (sanal) bellek miktarından daha fazla olmamalıdır, yaklaşık yani 512 MB fiziksel bellek seçimi 310 sahip bir sunucu için fiziksel belleğin % 60.</span><span class="sxs-lookup"><span data-stu-id="0495c-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="0495c-159">Ayrıca, en fazla 800 MB 2 GB adres alanı kullanırken aşmaması, önerilir.</span><span class="sxs-lookup"><span data-stu-id="0495c-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="0495c-160">Sunucunun bellek adres alanı 3 GB ise, çalışan işlem maksimum bellek sınırını 1, 800 MB olarak yüksek olabilir:</span><span class="sxs-lookup"><span data-stu-id="0495c-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="0495c-161">'Apply' ve 'Tamam' özellikleri iletişim kutusundan çıkmak için tıklatın.</span><span class="sxs-lookup"><span data-stu-id="0495c-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="0495c-162">Bu, tüm kullanılabilir uygulama havuzları için işlemi yineleyin.</span><span class="sxs-lookup"><span data-stu-id="0495c-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="0495c-163">Geri dönüştürme çalışan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0495c-163">Configuring worker recycling</span></span>

<span data-ttu-id="0495c-164">Varsayılan olarak, IIS 6.0, 29 saatte bir çalışan işlemi geri dönüştürmek için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="0495c-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="0495c-165">Bu ASP.NET çalışan bir uygulama için bir bit agresif ve otomatik çalışan işlem geri dönüşümü devre dışı bırakılır önerilir.</span><span class="sxs-lookup"><span data-stu-id="0495c-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="0495c-166">Otomatik çalışan işlem geri dönüşümü devre dışı bırakmak için önce Internet Information Services Yöneticisi açın (Başlat | Programlar | Yönetimsel Araçlar | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="0495c-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="0495c-167">Açık bir kez 'Uygulama havuzları' klasörünü genişletin:</span><span class="sxs-lookup"><span data-stu-id="0495c-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="0495c-168">Her bir uygulama havuzu için:</span><span class="sxs-lookup"><span data-stu-id="0495c-168">For each application pool:</span></span>

1. <span data-ttu-id="0495c-169">Uygulama havuzunda, örneğin sağ tıklayın 'DefaultAppPool' ve 'Özellikler' seçin:</span><span class="sxs-lookup"><span data-stu-id="0495c-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="0495c-170">İşaretini ' Geri Dönüşüm çalışan işlemi (dakika cinsinden):':</span><span class="sxs-lookup"><span data-stu-id="0495c-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="0495c-171">'Apply' ve 'Tamam' özellikleri iletişim kutusundan çıkmak için tıklatın.</span><span class="sxs-lookup"><span data-stu-id="0495c-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="0495c-172">Bu, tüm kullanılabilir uygulama havuzları için işlemi yineleyin.</span><span class="sxs-lookup"><span data-stu-id="0495c-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="0495c-173">Dosya sistemine yazma erişim izni verme</span><span class="sxs-lookup"><span data-stu-id="0495c-173">Granting write access to the file system</span></span>

<span data-ttu-id="0495c-174">Uygulamanızın dosya sistemine yazma erişimi gerektirir ve NTFS kullanıyorsanız bir erişim denetimi listesi (ASP.NET erişim izni vermek için ACL) klasör veya dosya değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="0495c-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="0495c-175">Örneğin, ASP.NET vermek için c:\inetpub\wwwroot yazma erişimi ilk Gezgini'ni açın ve dizine gidin:</span><span class="sxs-lookup"><span data-stu-id="0495c-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="0495c-176">Ardından, sağ tıklayın dizinde, örneğin 'wwwroot' ve Özellikler'i seçin.</span><span class="sxs-lookup"><span data-stu-id="0495c-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="0495c-177">Özellikler iletişim kutusu açıldıktan sonra 'Security' sekmesini seçin:</span><span class="sxs-lookup"><span data-stu-id="0495c-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="0495c-178">Özel bir dizinde c:\inetpub\wwwroot\ dizindir özel IIS 6.0 grup ' IIS\_WPG' zaten okuma izni &amp; yürütme, klasör içeriğini listele ve Okuma izinleri.</span><span class="sxs-lookup"><span data-stu-id="0495c-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="0495c-179">Ancak, yazma izni vermek için yazma işlemi için izin ver onay kutusuna tıklayın gerekir:</span><span class="sxs-lookup"><span data-stu-id="0495c-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="0495c-180">IIS 6.0, artık bu klasörde yazma izni vardır.</span><span class="sxs-lookup"><span data-stu-id="0495c-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="0495c-181">Diğer klasörlerde, şu adımları - izleyin yazma izinleri vermek için Not, IIS eklemeniz gerekebilir\_zaten yoksa, WPG grubu.</span><span class="sxs-lookup"><span data-stu-id="0495c-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="0495c-182">IIS için yazma izni verme\_WPG izin vermek bu dizine yazmak herhangi bir ASP.NET uygulama.</span><span class="sxs-lookup"><span data-stu-id="0495c-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="0495c-183">SQL Server ile tümleşik kimlik doğrulaması destekleme</span><span class="sxs-lookup"><span data-stu-id="0495c-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="0495c-184">Tümleşik kimlik doğrulaması SQL Server oturum açma hesabı doğrulamak için SQL Server'ın Windows NT kimlik doğrulaması kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="0495c-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="0495c-185">Bu kullanıcının standart SQL Server oturum açma işlemini atlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="0495c-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="0495c-186">Bu yaklaşımda, SQL Server Windows NT ağ güvenlik işleminden elde ettiği, parola bilgilerini ve kullanıcı için ayrı oturum açma kimliği veya parola girmeden bir SQL Server veritabanını bir ağ kullanıcısı erişebilir.</span><span class="sxs-lookup"><span data-stu-id="0495c-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="0495c-187">Kimlik bilgileri, uygulamanız için bağlantı dizenizi içinde hiç depolandığından ASP.NET uygulamaları için tümleşik kimlik doğrulaması seçme iyi bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="0495c-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="0495c-188">Bunun yerine SQL ile bağlantı için kullanılan bağlantı dizesi şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="0495c-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="0495c-189">Bu bağlantı dizesi SQL Server SQL sunucusuna erişmeye çalışan uygulama Windows kimlik bilgilerini kullanacak şekilde söyler.</span><span class="sxs-lookup"><span data-stu-id="0495c-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="0495c-190">ASP.NET/IIS 6 söz konusu olduğunda bu bir hesap IIS'de olacaktır\_WPG grubu.</span><span class="sxs-lookup"><span data-stu-id="0495c-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="0495c-191">SQL Server ve ASP.NET arasında tümleşik kimlik doğrulamasını etkinleştirmek için önce SQL Server tümleşik kimlik doğrulaması veya karma mod kimlik doğrulaması - için yapılandırıldığından emin olun gerekecektir bu belirlemek için DBA ile denetleyin.</span><span class="sxs-lookup"><span data-stu-id="0495c-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="0495c-192">SQL Server bu iki moddan birini ise, tümleşik kimlik doğrulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0495c-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="0495c-193">SQL Server Enterprise Manager'ı açın (Başlat | Programlar | Microsoft SQL Server | Kuruluş Yöneticisi), uygun sunucuyu seçin ve güvenlik klasörünü genişletin:</span><span class="sxs-lookup"><span data-stu-id="0495c-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="0495c-194">Varsa ' BUILTINT\IIS\_WPG' grubu listelenmiyorsa, üzerinde oturum açmalar ve 'Yeni oturum açma' seçin:</span><span class="sxs-lookup"><span data-stu-id="0495c-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="0495c-195">İçinde ' adı:' ya da girin textbox ' [sunucu/etki alanı adı] \IIS\_WPG' ya da Windows NT kullanıcı/Grup Seçici'yi açmak için üç nokta düğmesini tıklatın:</span><span class="sxs-lookup"><span data-stu-id="0495c-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="0495c-196">Geçerli makinenin IIS seçin\_WPG grubu tıklatın 'Add' ve seçiciyi kapatmak için Tamam.</span><span class="sxs-lookup"><span data-stu-id="0495c-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="0495c-197">Sonra da varsayılan veritabanı ve veritabanına erişmek için izinleri ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0495c-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="0495c-198">Ayarlamak için varsayılan veritabanı seçin açılır listeden, örneğin aşağıdaki Northwind seçilir:</span><span class="sxs-lookup"><span data-stu-id="0495c-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="0495c-199">Ardından, veritabanı erişimi sekmesinde tıklatın:</span><span class="sxs-lookup"><span data-stu-id="0495c-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="0495c-200">Erişmesine izin vermek istediğiniz her veritabanı için izin ver onay kutusuna tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0495c-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="0495c-201">De veritabanı rolleri seçmek veritabanı denetleniyor gerekir\_sahibi bulunduğundan emin olun, oturum açma yönetmek ve seçili veritabanını kullanmak için tüm gerekli izinler.</span><span class="sxs-lookup"><span data-stu-id="0495c-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="0495c-202">Özellik iletişim kutusundan çıkmak için Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="0495c-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="0495c-203">ASP.NET uygulamanızı tümleşik SQL Server kimlik doğrulamayı destekleyecek şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="0495c-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="0495c-204">ASP.NET 1.0 IIS 6.0 yerel modda çalıştırma</span><span class="sxs-lookup"><span data-stu-id="0495c-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="0495c-205">ASP.NET 1.0 IIS 6.0, yalnızca IIS 5 uyumluluk modunda desteklenir.</span><span class="sxs-lookup"><span data-stu-id="0495c-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="0495c-206">ASP.NET 1. 0'ı IIS 5.0 uyumluluk modunda çalışacak şekilde yapılandırmak için Internet Hizmetleri Yöneticisi'ni açın ve Web siteleri sağ tıklayın ve Özellikler'i seçin:</span><span class="sxs-lookup"><span data-stu-id="0495c-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="0495c-207">Hizmet sekmesine geçin ve denetleme? WWW hizmetini IIS 5.0 yalıtım modunda çalıştır?:</span><span class="sxs-lookup"><span data-stu-id="0495c-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
