---
uid: whitepapers/aspnet-and-iis6
title: ASP.NET 1.1 IIS 6.0 ile çalışan | Microsoft Docs
author: rick-anderson
description: Bu bileşenler, Windows Server 2003 hem IIS 6.0 hem de ASP.NET 1.1 içerse de, varsayılan olarak devre dışıdır. Bu teknik incelemede IIS 6.0 etkinleştirmeyi açıklar bir...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 38cd0abc1e9133b9b86cff6dd2759ce98ac5a115
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753917"
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="3b659-104">IIS 6.0 ile çalışan ASP.NET 1.1</span><span class="sxs-lookup"><span data-stu-id="3b659-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="3b659-105">Bu bileşenler, Windows Server 2003 hem IIS 6.0 hem de ASP.NET 1.1 içerse de, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="3b659-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="3b659-106">Bu teknik incelemede IIS 6.0 ve ASP.NET 1.1 etkinleştirmeyi açıklar ve IIS ve ASP.NET en iyi performans almak için çeşitli yapılandırma ayarlarını önerir.</span><span class="sxs-lookup"><span data-stu-id="3b659-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="3b659-107">1.1 ASP.NET ve IIS 6.0 için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3b659-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="3b659-108">ASP.NET 1.1, Windows Server Internet Information Server (IIS) sürüm 6.0, en son sürümünü içeren 2003 ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="3b659-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="3b659-109">IIS 6.0 ve ASP.NET 1.1 sorunsuz bir şekilde tümleştirmek için tasarlanmıştır ve artık varsayılan olarak yeni IIS 6.0 çalışan işlem modeli ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3b659-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="3b659-110">ASP.NET 1.1 varsayılan olarak yüklü değil</span><span class="sxs-lookup"><span data-stu-id="3b659-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="3b659-111">Microsoft'un sunucu işletim sistemlerinin önceki sürümlerden farklı olarak, Internet Information Server (IIS) varsayılan olarak etkin değildir; ya da ASP.NET 1.1 silinmez.</span><span class="sxs-lookup"><span data-stu-id="3b659-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="3b659-112">IIS etkinleştirmek için iki seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="3b659-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="3b659-113">Seçenek #1 - IIS Sunucu Yapılandırma Sihirbazı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="3b659-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="3b659-114">Windows Server 2003, bir yeni 'sunucu yapılandırma sunucunuzu istenen modunda düzgün şekilde yapılandırmanıza yardımcı olması için Sihirbazı' birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="3b659-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="3b659-115">Unutmayın, oturum açmış olmanız gerekir yönetici olarak - sihirbazını çalıştırma - sihirbazını başlatmak için Git: Başlangıç | Programlar | Yönetim Araçları ve Seç 'sunucunuzu'.</span><span class="sxs-lookup"><span data-stu-id="3b659-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="3b659-116">Bir kez seçili 'Sunucu Yapılandırma Sihirbazı' açılış ekranı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="3b659-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="3b659-117">Tıklatın ' sonraki &gt;':</span><span class="sxs-lookup"><span data-stu-id="3b659-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="3b659-118">Tıklatın ' sonraki &gt;'</span><span class="sxs-lookup"><span data-stu-id="3b659-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="3b659-119">Bu ekranda seçmeniz gerekecek ' yapılandırma seçenekleri olarak uygulama sunucusu (IIS, ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="3b659-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="3b659-120">Tıklatın ' sonraki &gt;'.</span><span class="sxs-lookup"><span data-stu-id="3b659-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="3b659-121">Sunucuyu, uygulama sunucusu olarak yapılandırma seçtikten sonra bu ekran ek yetenekler yüklenmelidir isteyen görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3b659-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="3b659-122">Her iki seçeneği varsayılan olarak seçilidir.</span><span class="sxs-lookup"><span data-stu-id="3b659-122">Neither option is selected by default.</span></span> <span data-ttu-id="3b659-123">ASP.NET otomatik olarak etkinleştirmek için seçmeniz gerekir ' ASP etkinleştirin. NET'.</span><span class="sxs-lookup"><span data-stu-id="3b659-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="3b659-124">Tıklatın ' sonraki &gt;'.</span><span class="sxs-lookup"><span data-stu-id="3b659-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="3b659-125">Bu ekran, yüklenecek seçenekleri nelerdir görüntüler.</span><span class="sxs-lookup"><span data-stu-id="3b659-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="3b659-126">Tıklatın ' sonraki &gt;'.</span><span class="sxs-lookup"><span data-stu-id="3b659-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="3b659-127">Seçtiğiniz seçenekler yüklenirken şu ekranı görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="3b659-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="3b659-128">Diğer iletişim kutuları Hizmetleri yüklü olarak görünür görmek için normal bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="3b659-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="3b659-129">Ayrıca Windows 2003 Server yükleme CD'sinde konumunu istenebilir.</span><span class="sxs-lookup"><span data-stu-id="3b659-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="3b659-130">Tıklatın ' sonraki &gt;' tamamlandığında.</span><span class="sxs-lookup"><span data-stu-id="3b659-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="3b659-131">'Son' düğmesini tıklatın,-Windows Server 2003 IIS 6.0 ve ASP.NET 1.1 desteklemek için artık yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="3b659-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="3b659-132">IIS, #2 - el ile IIS ve ASP.NET yapılandırma seçeneğini etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="3b659-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="3b659-133">'Sunucu Yapılandırma Sihirbazı' kullanmak istemiyorsanız, isteğe bağlı olarak IIS 6.0 ve ASP.NET 1.1 'Program Ekle veya Kaldır' kullanarak Denetim Masası'ndan yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3b659-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="3b659-134">İlk kullanarak denetim masasını açın:</span><span class="sxs-lookup"><span data-stu-id="3b659-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="3b659-135">Ardından, ' Ekle/Kaldır Windows 'Windows Bileşenleri Sihirbazı' açılır bileşenleri üzerinde' tıklayın:</span><span class="sxs-lookup"><span data-stu-id="3b659-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="3b659-136">Vurgulayın ve 'Uygulama sunucusu' denetleyin ve 'Ayrıntılar?' ı</span><span class="sxs-lookup"><span data-stu-id="3b659-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="3b659-137">Düğme:</span><span class="sxs-lookup"><span data-stu-id="3b659-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="3b659-138">ASP.NET yüklemek için kontrol ' ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="3b659-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="3b659-139">Windows Bileşen Sihirbazı için ' Tamam' ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="3b659-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="3b659-140">Tıklatın ' sonraki &gt;' yüklemeye başlamak için Windows Bileşen Sihirbazı:</span><span class="sxs-lookup"><span data-stu-id="3b659-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="3b659-141">Diğer iletişim kutuları Hizmetleri yüklü olarak görünür görmek için normal bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="3b659-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="3b659-142">Ayrıca Windows 2003 Server yükleme CD'sinde konumunu istenebilir.</span><span class="sxs-lookup"><span data-stu-id="3b659-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="3b659-143">Yükleme tamamlandığında Windows Bileşen Sihirbazı'nın son ekranı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="3b659-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="3b659-144">IIS 6.0 ve ASP.NET 1.1 artık yapılandırılmış ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3b659-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="3b659-145">Önerilen ayarlar</span><span class="sxs-lookup"><span data-stu-id="3b659-145">Recommended Settings</span></span>

<span data-ttu-id="3b659-146">ASP.NET 1.1 IIS 6.0 ile çalışan ASP.NET tarafından en iyi performans almak için önerilen bazı yapılandırma ayarları vardır:</span><span class="sxs-lookup"><span data-stu-id="3b659-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="3b659-147">Yapılandırma çalışan işlem bellek sınırları</span><span class="sxs-lookup"><span data-stu-id="3b659-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="3b659-148">Çalışan işlemi geri dönüşümü yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3b659-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="3b659-149">Yapılandırma çalışan işlem bellek sınırları</span><span class="sxs-lookup"><span data-stu-id="3b659-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="3b659-150">Varsayılan olarak IIS 6.0 IIS kullanmasına izin verilen bellek miktarı sınırı ayarlamaz.</span><span class="sxs-lookup"><span data-stu-id="3b659-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="3b659-151">ASP. Önbellek, proaktif olarak kullanılmayan öğeleri bellekten kaldırabilmeniz için NET önbellek özelliği sınırlaması bellek kullanır.</span><span class="sxs-lookup"><span data-stu-id="3b659-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="3b659-152">Bu, IIS 6.0 özelliğini geri dönüştürme bellek yapılandırmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="3b659-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="3b659-153">Bu açık Internet Information Services Manager'ı yapılandırmak için (Başlangıç | Programlar | Yönetimsel Araçlar | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="3b659-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="3b659-154">Açık bir kez 'Uygulama havuzları' klasörünü genişletin:</span><span class="sxs-lookup"><span data-stu-id="3b659-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="3b659-155">Her bir uygulama havuzu için:</span><span class="sxs-lookup"><span data-stu-id="3b659-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="3b659-156">Örneğin uygulama havuzunda sağ tıklayın 'DefaultAppPool' ve 'Özellikler' i seçin:</span><span class="sxs-lookup"><span data-stu-id="3b659-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="3b659-157">Ardından, üzerinde tıklayarak bellek geri dönüşümü Etkinleştir ' en çok kullanılan bellek (megabayt cinsinden):'.</span><span class="sxs-lookup"><span data-stu-id="3b659-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="3b659-158">Birden çok sunucu (sanal olmayan) fiziksel bellek miktarı değeri olmamalıdır, iyi bir yaklaşık % 60 ' 512 MB fiziksel bellek seçin 310 ile bir sunucu için başka bir deyişle, fiziksel bellek.</span><span class="sxs-lookup"><span data-stu-id="3b659-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="3b659-159">Ayrıca, en fazla 800 MB 2 GB adres alanı kullanırken aşmaması, önerilir.</span><span class="sxs-lookup"><span data-stu-id="3b659-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="3b659-160">Sunucunun bellek adresi alanının 3 GB ise, çalışan işlem için maksimum bellek sınırını 1 800 MB olarak yüksek olabilir:</span><span class="sxs-lookup"><span data-stu-id="3b659-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="3b659-161">Özellikler iletişim kutusundan çıkmak için 'Uygula' ve 'Tamam' seçeneğine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3b659-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="3b659-162">Bu, tüm kullanılabilir uygulama havuzları için yineleyin.</span><span class="sxs-lookup"><span data-stu-id="3b659-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="3b659-163">Geri Dönüşüm çalışan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3b659-163">Configuring worker recycling</span></span>

<span data-ttu-id="3b659-164">Varsayılan olarak, IIS 6.0, 29 saatte bir çalışan işlemi geri dönüştürmek için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="3b659-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="3b659-165">ASP.NET çalışan bir uygulama için biraz agresif budur ve otomatik çalışan işlem geri dönüşümü devre dışı bırakıldı önerilir.</span><span class="sxs-lookup"><span data-stu-id="3b659-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="3b659-166">Otomatik çalışan işlem geri dönüşümü devre dışı bırakmak için önce Internet Information Services Manager açın (Başlat | Programlar | Yönetimsel Araçlar | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="3b659-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="3b659-167">Açık bir kez 'Uygulama havuzları' klasörünü genişletin:</span><span class="sxs-lookup"><span data-stu-id="3b659-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="3b659-168">Her bir uygulama havuzu için:</span><span class="sxs-lookup"><span data-stu-id="3b659-168">For each application pool:</span></span>

1. <span data-ttu-id="3b659-169">Örneğin uygulama havuzunda sağ tıklayın 'DefaultAppPool' ve 'Özellikler' i seçin:</span><span class="sxs-lookup"><span data-stu-id="3b659-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="3b659-170">Onay kutusunu temizleyin ' Geri Dönüşüm çalışan işlemi (dakika):':</span><span class="sxs-lookup"><span data-stu-id="3b659-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="3b659-171">Özellikler iletişim kutusundan çıkmak için 'Uygula' ve 'Tamam' seçeneğine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3b659-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="3b659-172">Bu, tüm kullanılabilir uygulama havuzları için yineleyin.</span><span class="sxs-lookup"><span data-stu-id="3b659-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="3b659-173">Dosya sistemine yazma erişim izni verme</span><span class="sxs-lookup"><span data-stu-id="3b659-173">Granting write access to the file system</span></span>

<span data-ttu-id="3b659-174">Uygulamanızın gerektirdiği dosya sistemine yazma erişimi ve NTFS kullanıyorsanız, bir erişim denetimi listesi (ASP.NET erişim izni vermek için ACL) klasör veya dosya çubuğunda değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3b659-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="3b659-175">Örneğin, ASP.NET vermek c:\inetpub\wwwroot yazma erişimi ilk Gezgini'ni açın ve dizine gidin:</span><span class="sxs-lookup"><span data-stu-id="3b659-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="3b659-176">Ardından, sağ tıklayarak dizine, örneğin 'wwwroot' ve Özellikler'i seçin.</span><span class="sxs-lookup"><span data-stu-id="3b659-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="3b659-177">Özellikler iletişim kutusu açıldıktan sonra 'Güvenlik' sekmesinde'ı seçin:</span><span class="sxs-lookup"><span data-stu-id="3b659-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="3b659-178">C:\inetpub\wwwroot\ dizindir, özel bir dizinde özel IIS 6.0 grup ' IIS\_WPG' okuma zaten verilmiş &amp; yürütme, klasör içeriklerini listeleme ve Okuma izinleri.</span><span class="sxs-lookup"><span data-stu-id="3b659-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="3b659-179">Ancak, yazma izni vermek için yazmaya izin ver onay kutusuna tıklayın gerekir:</span><span class="sxs-lookup"><span data-stu-id="3b659-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="3b659-180">IIS 6.0, artık bu klasörde yazma izni vardır.</span><span class="sxs-lookup"><span data-stu-id="3b659-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="3b659-181">Şu adımları - izleyin diğer klasörler üzerinde yazma izinleri vermek için Not: IIS eklemeniz gerekebilir\_zaten yoksa, WPG grubu.</span><span class="sxs-lookup"><span data-stu-id="3b659-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="3b659-182">IIS için yazma izni verme\_WPG bu dizine yazabilmek herhangi bir ASP.NET uygulama sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="3b659-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="3b659-183">SQL sunucusu ile tümleşik kimlik doğrulamasını destekleme</span><span class="sxs-lookup"><span data-stu-id="3b659-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="3b659-184">Tümleşik kimlik doğrulaması, SQL Server oturum açma hesapları doğrulamak için SQL Server'ın Windows NT kimlik doğrulaması yararlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="3b659-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="3b659-185">Bu kullanıcının standart SQL Server oturum açma işlemi atlamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="3b659-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="3b659-186">Bu yaklaşımda, bir ağ kullanıcı bir SQL Server veritabanını SQL Server Windows NT ağ güvenlik işleminden elde ettiği, parola bilgisi ve kullanıcı için ayrı bir oturum açma kimliği veya parola sağlamadan erişebilir.</span><span class="sxs-lookup"><span data-stu-id="3b659-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="3b659-187">Kimlik bilgisi bağlantı dizenizi uygulamanızın içinde hiç olmadığı kadar depolandığından ASP.NET uygulamaları için tümleşik kimlik doğrulaması'nı seçerek iyi bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="3b659-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="3b659-188">Bunun yerine SQL'e bağlanmak için kullanılan bağlantı dizesi şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="3b659-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="3b659-189">Bu bağlantı dizesi, SQL Server'ın SQL Server erişmeye çalışan uygulamanın Windows kimlik bilgilerini söyler.</span><span class="sxs-lookup"><span data-stu-id="3b659-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="3b659-190">ASP.NET/IIS 6 söz konusu olduğunda bu bir hesap IIS'de olacaktır\_WPG grubu.</span><span class="sxs-lookup"><span data-stu-id="3b659-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="3b659-191">SQL Server ve ASP.NET arasında tümleşik kimlik doğrulamasını etkinleştirmek için önce SQL Server tümleşik kimlik doğrulaması veya karma mod kimlik doğrulaması - için yapılandırılmış olduğundan emin olun gerekecektir bunu belirlemek için DBA ile denetleyin.</span><span class="sxs-lookup"><span data-stu-id="3b659-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="3b659-192">SQL Server, bu iki moddan birini ise, tümleşik kimlik doğrulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3b659-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="3b659-193">SQL Server Enterprise Manager'ı açın (Başlat | Programlar | Microsoft SQL Server | Kurumsal Yöneticisi), uygun sunucuyu seçin ve güvenlik klasörü genişletin:</span><span class="sxs-lookup"><span data-stu-id="3b659-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="3b659-194">Varsa ' BUILTINT\IIS\_WPG' grup listelenmiyorsa, oturum açma bilgilerini sağ tıklayın ve 'Yeni oturum açma' seçin:</span><span class="sxs-lookup"><span data-stu-id="3b659-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="3b659-195">İçinde ' adı:' metin girin ya da ' [sunucu/etki alanı adı] \IIS\_WPG' ya da Windows NT kullanıcı/Grup Seçici'yi açmak için üç nokta düğmesine tıklayın:</span><span class="sxs-lookup"><span data-stu-id="3b659-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="3b659-196">Geçerli makinenin IIS seçin\_WPG grubu tıklatın 'Add' ve seçiciyi kapatmak için Tamam.</span><span class="sxs-lookup"><span data-stu-id="3b659-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="3b659-197">Ardından varsayılan veritabanı ve veritabanına erişmek için izinleri ayrıca ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3b659-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="3b659-198">Ayarlamak için varsayılan veritabanını seçin ve açılan listesinde, örneğin aşağıdaki Northwind seçilir:</span><span class="sxs-lookup"><span data-stu-id="3b659-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="3b659-199">Ardından, veritabanı erişimi sekmesine tıklayın:</span><span class="sxs-lookup"><span data-stu-id="3b659-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="3b659-200">Erişime izin vermek istediğiniz her veritabanı için izin ver onay kutusuna tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3b659-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="3b659-201">Ayrıca veritabanı rolleri seçmek db denetimi ihtiyacınız olacak\_sahibi, oturum açma bilgilerinizi yönetmek ve seçili veritabanını kullanmak için tüm gerekli izinlere sahip olmadığını olun.</span><span class="sxs-lookup"><span data-stu-id="3b659-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="3b659-202">Özellik iletişim kutusundan çıkmak için Tamam'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3b659-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="3b659-203">ASP.NET uygulamanızı, tümleşik SQL Server kimlik doğrulamasını desteklemek için artık yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="3b659-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="3b659-204">ASP.NET 1.0 IIS 6.0 yerel modda çalıştırma</span><span class="sxs-lookup"><span data-stu-id="3b659-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="3b659-205">ASP.NET 1.0 IIS 6.0, yalnızca IIS 5 uyumluluk modunda desteklenir.</span><span class="sxs-lookup"><span data-stu-id="3b659-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="3b659-206">ASP.NET IIS 5.0 uyumluluk modunda çalışacak şekilde 1.0 yapılandırmak için Internet Hizmetleri Yöneticisi'ni açın ve Web siteleri sağ tıklayın ve Özellikler'i seçin:</span><span class="sxs-lookup"><span data-stu-id="3b659-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="3b659-207">Hizmet sekmesine gidin ve kontrol? WWW hizmetini IIS 5.0 yalıtım modunda çalıştır?:</span><span class="sxs-lookup"><span data-stu-id="3b659-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
