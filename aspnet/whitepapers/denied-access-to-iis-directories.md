---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET IIS dizinlere erişimi reddedildi | Microsoft Docs
author: rick-anderson
description: Bu teknik ASP.NET uygulamanız için bir istek "DirectoryName dizinine erişimi reddedildi. hata verirse yapmanız gerekir açıklar S başarısız...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30070777"
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="c77cb-104">ASP.NET IIS dizinleri için erişim reddedildi</span><span class="sxs-lookup"><span data-stu-id="c77cb-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="c77cb-105">Bu teknik ASP.NET uygulamanız için bir istek hata verirse yapmanız gerekir açıklar "erişim reddedildi *DirectoryName* dizini.</span><span class="sxs-lookup"><span data-stu-id="c77cb-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="c77cb-106">Dizin değişikliklerini izleme başlatılamadı."</span><span class="sxs-lookup"><span data-stu-id="c77cb-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="c77cb-107">ASP.NET 1.0 ve 1.1 ASP.NET için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c77cb-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="c77cb-108">ASP.NET V1 RTM şimdi çalışıyor daha az kullanılarak ayrıcalıklı windows hesabı - yerel makinede "ASPNET" hesabı olarak kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="c77cb-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="c77cb-109">Bazı üzerinde sistemleri kilitli, bu hesap varsayılan olarak güvenlik erişimi bir Web sitesinin içeriği dizinlerini, uygulama kök dizini ya da web sitesinin kök dizini için okuma izniniz olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="c77cb-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="c77cb-110">Bu durumda verilen web uygulamasından sayfaları isteğinde bulunurken aşağıdaki hatayı alırsınız:</span><span class="sxs-lookup"><span data-stu-id="c77cb-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="c77cb-111">Bu sorunu gidermek için uygun dizinlerin güvenlik izinlerini değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="c77cb-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="c77cb-112">Özellikle, ASP okuma, yürütme ve listesinde bir web sitesi kök ASPNET hesabı için erişim (örneğin: c:\inetpub\wwwroot veya IIS içinde yapılandırılmış herhangi bir alternatif site dizini), içerik dizinini ve uygulama kök dizini yapılandırma dosyası değişiklikleri izlemek için.</span><span class="sxs-lookup"><span data-stu-id="c77cb-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="c77cb-113">Uygulama kök klasör yolu IIS Yönetim Aracı (inetmgr) uygulama sanal dizininde ilişkili karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="c77cb-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="c77cb-114">Örneğin, aşağıdaki uygulama hiyerarşi altındaki wwwroot klasörüne göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="c77cb-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="c77cb-115">Bu örnekte, ASP.NET hesabından Uygulamam ve wwwroot dizinini içeriği için yukarıda tanımlanan Okuma izinleri olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c77cb-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="c77cb-116">İç içe, kök klasöründe bir tek devralınan ACL isteğe bağlı olarak her iki dizini için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c77cb-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="c77cb-117">Bir dizine izinleri eklemek için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="c77cb-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="c77cb-118">Windows Gezgini'ni kullanarak, dizine gidin</span><span class="sxs-lookup"><span data-stu-id="c77cb-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="c77cb-119">Dizin klasörü sağ tıklatın ve "Özellikler"'i seçin</span><span class="sxs-lookup"><span data-stu-id="c77cb-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="c77cb-120">Özellik iletişim kutusunda "Güvenlik" sekmesine gidin</span><span class="sxs-lookup"><span data-stu-id="c77cb-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="c77cb-121">"Ekle" düğmesini tıklatın ve ardından ASPNET hesap adı makine adı girin.</span><span class="sxs-lookup"><span data-stu-id="c77cb-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="c77cb-122">Örneğin, "webdev" adlı bir makinede webdev\ASPNET girer ve "Tamam" düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="c77cb-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="c77cb-123">ASP.NET hesabından sahip olduğundan emin olun "okuma &amp; Execute", "Klasör içeriğini listele" ve "Okuma" onay kutusu işaretli.</span><span class="sxs-lookup"><span data-stu-id="c77cb-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="c77cb-124">İletişim kutusunu kapatmak ve değişiklikleri kaydetmek için Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="c77cb-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="c77cb-125">İsterseniz, bu değişiklikler ile Windows komut dosyaları veya gelir "cacls.exe" aracını kullanarak otomatik olarak yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="c77cb-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="c77cb-126">ASP.NET hesabından hakkında daha fazla bilgi için lütfen bkz [SSS belge](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="c77cb-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="c77cb-127">Belirli bir web uygulaması yazma sahip kullanır veya belirli klasör veya dosyanın izinlerini değiştirmek, bu "Yazma" ve/veya "Değiştirme" onay kutularını işaretleyerek ve aynı yordamı izleyerek verilebilir.</span><span class="sxs-lookup"><span data-stu-id="c77cb-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="c77cb-128">Herkes veya kullanıcılar grubunun Okuma erişimi (varsayılan yapılandırması olan) bu dizinleri izin makinelerde herhangi bir sorun karşılaştı ve yukarıdaki adımları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="c77cb-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
