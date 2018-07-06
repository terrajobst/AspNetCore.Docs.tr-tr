---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET IIS dizinlerine erişim reddedildi | Microsoft Docs
author: rick-anderson
description: Bu teknik incelemede ASP.NET uygulamanız için bir istek "DirectoryName dizinine erişim engellendi. hata verirse ne yapmalısınız açıklar. S başarısız oldu...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 4853ee29d2468c4b67375123c5b2ec15089fe09b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842416"
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="e6e4f-104">ASP.NET IIS dizinlerine erişim reddedildi</span><span class="sxs-lookup"><span data-stu-id="e6e4f-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="e6e4f-105">Bu teknik incelemede, ASP.NET uygulamanız için bir istek hata verirse ne yapmalısınız açıklar "erişim izni verilmeyen *DirectoryName* dizin.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="e6e4f-106">Dizin değişikliklerini izleme başlatılamadı."</span><span class="sxs-lookup"><span data-stu-id="e6e4f-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="e6e4f-107">ASP.NET 1.0 ve 1.1 ASP.NET için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="e6e4f-108">ASP.NET V1 RTM artık çalışan bir less kullanarak ayrıcalıklı windows hesabı - yerel bir makinede "ASPNET" hesabı olarak kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="e6e4f-109">Bazı üzerinde sistemleri kilitli, bu hesap varsayılan olarak güvenlik erişimi bir Web sitesinin içerik dizinleri, uygulamanın kök dizinine veya web sitesi kök dizini için okuma izniniz olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="e6e4f-110">Bu durumda sayfaları belirli bir web uygulamasından isteğinde bulunurken aşağıdaki hatayı alırsınız:</span><span class="sxs-lookup"><span data-stu-id="e6e4f-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="e6e4f-111">Bu sorunu gidermek için uygun dizinleri güvenlik izinlerini değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="e6e4f-112">Özellikle, ASP okuma, yürütün ve listeleme erişimi ASPNET hesabı web sitesinin kök için (örneğin: c:\inetpub\wwwroot veya IIS içinde yapılandırılmış herhangi bir alternatif site dizini), içerik dizinini ve uygulama kök dizini yapılandırma dosya değişiklikleri izlemek için.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="e6e4f-113">Uygulama kökü uygulama sanal dizini IIS Yönetim Aracı (inetmgr) içinde ilişkili klasör yoluna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="e6e4f-114">Örneğin, aşağıdaki uygulama hiyerarşisi wwwroot klasörü altında göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="e6e4f-115">Bu örnekte, myapp hem de wwwroot dizinini içerik için yukarıda tanımlanan Okuma izinleri ASPNET hesabı gerekir.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="e6e4f-116">İç içe Kök klasörde tek bir devralınan ACL isteğe bağlı olarak her iki dizini için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="e6e4f-117">Bir dizin için izinler eklemek için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="e6e4f-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="e6e4f-118">Windows explorer'ı kullanarak, dizine gidin</span><span class="sxs-lookup"><span data-stu-id="e6e4f-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="e6e4f-119">Dizin klasörü sağ tıklatın ve "Özellikler" öğesini seçin</span><span class="sxs-lookup"><span data-stu-id="e6e4f-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="e6e4f-120">Özellik iletişim kutusundaki "Güvenlik" sekmesine gidin</span><span class="sxs-lookup"><span data-stu-id="e6e4f-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="e6e4f-121">"Ekle" düğmesine tıklayın ve ardından ASP.NET hesap adı makine adı girin.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="e6e4f-122">Örneğin, "webdev" adlı bir makinede webdev\ASPNET girer ve "Tamam" düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="e6e4f-123">ASP.NET hesabından olduğundan emin olun "okuma &amp; Çalıştır", "Klasör içeriğini listele" ve "Okuma" işaretli.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="e6e4f-124">İletişim kutusunu kapatmak ve değişiklikleri kaydetmek için Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="e6e4f-125">İsterseniz, bu değişiklikler ile Windows betikleri veya birlikte verilen "cacls.exe" aracı kullanılarak otomatikleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="e6e4f-126">ASP.NET hesabından hakkında daha fazla bilgi için lütfen bkz [SSS belge](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="e6e4f-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="e6e4f-127">Belirli bir web uygulaması yazma olması kullanır veya belirli bir klasör veya dosyanın izinlerini değiştirmek, bu yordamın aynısını uygulayarak ve "Yazma" ve/veya "Değiştirme" onay kutuları denetleyerek verilebilir.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="e6e4f-128">Herkesin veya kullanıcılar grubu okuma erişimini (Bu varsayılan yapılandırma) bu dizinler izin makinelerde sorun ile karşılaşılan ve yukarıdaki adımları gerekmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="e6e4f-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
