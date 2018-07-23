---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Visual Studio 2005 geliştirmeleri | Microsoft Docs
author: microsoft
description: Visual Studio 2005 iyileştirmeler ve geliştirmeler Web projeleri için uzun bir liste ile Web uygulaması geliştiricileri sağlar.
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 0a42699381fd326891898e01b4e98662e9ce22bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811376"
---
<a name="improvements-in-visual-studio-2005"></a><span data-ttu-id="838f4-103">Visual Studio 2005 geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="838f4-103">Improvements in Visual Studio 2005</span></span>
====================
<span data-ttu-id="838f4-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="838f4-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="838f4-105">Visual Studio 2005 iyileştirmeler ve geliştirmeler Web projeleri için uzun bir liste ile Web uygulaması geliştiricileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="838f4-105">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span>


<span data-ttu-id="838f4-106">Visual Studio 2005 iyileştirmeler ve geliştirmeler Web projeleri için uzun bir liste ile Web uygulaması geliştiricileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="838f4-106">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span> <span data-ttu-id="838f4-107">Güçlü Visual Studio .NET 2002 ve 2003 olduğu gibi vardı birçok şikayetlerinin şekilde Web projeleri işlendi.</span><span class="sxs-lookup"><span data-stu-id="838f4-107">As powerful as Visual Studio .NET 2002 and 2003 are, there were many complaints in the way that Web projects were handled.</span></span> <span data-ttu-id="838f4-108">Visual Studio 2005 bu şikayetlerinin değinmek için çok sayıda yeni özellikleri ekler.</span><span class="sxs-lookup"><span data-stu-id="838f4-108">Visual Studio 2005 adds a significant number of new features in order to address these complaints.</span></span> <span data-ttu-id="838f4-109">Kişiler için Visual Studio .NET 2003 Web uygulamalarının derlenmesi işlenme tercih, bkz: [Web Uygulama projeleri](https://go.microsoft.com/fwlink/?LinkId=57870).</span><span class="sxs-lookup"><span data-stu-id="838f4-109">For those who prefer the way that Visual Studio .NET 2003 handled compilation of Web applications, see [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).</span></span>

<span data-ttu-id="838f4-110">Bu modülde, Web projesi oluşturma, yönetim ve geliştirme iyileştirmeleri de içerir.</span><span class="sxs-lookup"><span data-stu-id="838f4-110">In this module, well cover improvements in Web project creation, management, and development.</span></span> <span data-ttu-id="838f4-111">Daha sonra bir modülde, Web projeleri oluşturmak ve bunları dağıtmaya yönelik geliştirmeler de kapsar.</span><span class="sxs-lookup"><span data-stu-id="838f4-111">In a later module, well cover improvements in building Web projects and deploying them.</span></span>

## <a name="frontpage-server-extensions"></a><span data-ttu-id="838f4-112">FrontPage Server Extensions</span><span class="sxs-lookup"><span data-stu-id="838f4-112">FrontPage Server Extensions</span></span>

<span data-ttu-id="838f4-113">Visual Studio .NET 2002 ve 2003 FrontPage Server Extensions kutusunda veya Web projeleri oluşturma için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="838f4-113">Visual Studio .NET 2002 and 2003 required FrontPage Server Extensions on the box in order to create or build Web projects.</span></span> <span data-ttu-id="838f4-114">Geliştiriciler sahip iki farklı erişim modları (FrontPage Server Extensions veya dosya erişim modu) arasında seçim yapma, FrontPage Server Extensions hem de uygulama kökü ayarlama, IIS, vb. gibi görevleri gerçekleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="838f4-114">Developers did have a choice between two different access modes (FrontPage Server Extensions or File access mode), both used FrontPage Server Extensions to perform tasks such as setting the application root in IIS, etc.</span></span>

<span data-ttu-id="838f4-115">FrontPage Server Extensions güvenme yerel projeleri için Visual Studio 2005 kaldırır.</span><span class="sxs-lookup"><span data-stu-id="838f4-115">Visual Studio 2005 removes the reliance on FrontPage Server Extensions for local projects.</span></span> <span data-ttu-id="838f4-116">Visual Studio 2005 artık IIS metabase FrontPage Server Extensions kullanmak yerine doğrudan erişir.</span><span class="sxs-lookup"><span data-stu-id="838f4-116">Visual Studio 2005 now accesses the IIS metabase directly instead of using the FrontPage Server Extensions.</span></span> <span data-ttu-id="838f4-117">Visual Studio 2005 ayrıca FrontPage Server Extensions gerek kalmadan uzak proje erişimi sağlayan FTP için destek ekler.</span><span class="sxs-lookup"><span data-stu-id="838f4-117">Visual Studio 2005 also adds support for FTP which allows for remote project access without requiring FrontPage Server Extensions.</span></span>

<span data-ttu-id="838f4-118">Kendi projeleri FrontPage Server Extensions kullanmak istediğiniz geliştiriciler seçenek yine de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-118">For those developers who want to use FrontPage Server Extensions in their projects, the option is still available.</span></span> <span data-ttu-id="838f4-119">Ancak, ASP.NET Geliştirici topluluğu güçlü görüşleri göre bu bir gereksinim değildir.</span><span class="sxs-lookup"><span data-stu-id="838f4-119">However, based upon strong feedback from the ASP.NET developer community, it is not a requirement.</span></span>

> [!NOTE]
> <span data-ttu-id="838f4-120">FrontPage Server Extensions uzak proje oluşturma, açma, vb. için hala gereklidir.</span><span class="sxs-lookup"><span data-stu-id="838f4-120">FrontPage Server Extensions are still required for remote project creation, opening, etc.</span></span>


## <a name="aspnet-development-server"></a><span data-ttu-id="838f4-121">ASP.NET Geliştirme Sunucusu</span><span class="sxs-lookup"><span data-stu-id="838f4-121">ASP.NET Development Server</span></span>

<span data-ttu-id="838f4-122">Visual Studio 2005 ASP.NET Geliştirme Sunucusu adı verilen yeni bir Web sunucusu ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="838f4-122">Visual Studio 2005 ships with a new Web server called ASP.NET Development Server.</span></span> <span data-ttu-id="838f4-123">(Bu Web sunucusu daha önce Cassini biliniyordu.)</span><span class="sxs-lookup"><span data-stu-id="838f4-123">(This Web server was previously known as Cassini.)</span></span>

<span data-ttu-id="838f4-124">ASP.NET Geliştirme Sunucusu, çeşitli avantajları vardır.</span><span class="sxs-lookup"><span data-stu-id="838f4-124">There are several benefits of the ASP.NET Development Server.</span></span>

- <span data-ttu-id="838f4-125">Yönetici olmayanlar için geliştirme ve bir Web sunucusunda hata ayıklama artık mümkündür.</span><span class="sxs-lookup"><span data-stu-id="838f4-125">It is now possible for non-Administrators to develop and debug against a Web server.</span></span>
- <span data-ttu-id="838f4-126">ASP.NET Geliştirme Sunucusu herhangi bir yere sanal dizinler için esnek bir proje konumları sağlayan dosya sistemindeki dinamik olarak eşler.</span><span class="sxs-lookup"><span data-stu-id="838f4-126">The ASP.NET Development Server dynamically maps virtual directories to any location in the file system allowing for flexible project locations.</span></span>
- <span data-ttu-id="838f4-127">IIS zaten kullanan kullanıcılar Windows XP Professional, artık dosya veya klasör yapısı, kendi varsayılan Web sitesi IIS'de etkilemez yeni Web uygulamaları oluşturmak mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="838f4-127">Users on Windows XP Professional who are already using IIS will now be able to create new Web applications that will not affect the file or folder structure of their Default Web Site in IIS.</span></span>

<span data-ttu-id="838f4-128">ASP.NET Geliştirme Sunucusu yararlanmak için özel bir yapılandırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="838f4-128">No special configuration is required to take advantage of the ASP.NET Development Server.</span></span> <span data-ttu-id="838f4-129">Dosya sistemi üzerinde barındırılan bir Web projesi hata ayıklaması veya göz, Visual Studio 2005 ASP.NET Geliştirme Sunucusu örneği isteğe hizmet vermek için rastgele bir bağlantı noktası üzerinde otomatik olarak başlatın.</span><span class="sxs-lookup"><span data-stu-id="838f4-129">When a Web project that is hosted on the file system is debugged or browsed, Visual Studio 2005 will automatically start an instance of the ASP.NET Development Server on a random port to service the request.</span></span>

<span data-ttu-id="838f4-130">ASP.NET geliştirme Sunucusu'nda daha sonra bu modül hakkında daha fazla bilgi ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="838f4-130">More information will be covered on the ASP.NET Development Server later in this module.</span></span>

## <a name="improved-file-management"></a><span data-ttu-id="838f4-131">Gelişmiş dosya yönetimi</span><span class="sxs-lookup"><span data-stu-id="838f4-131">Improved File Management</span></span>

<span data-ttu-id="838f4-132">Visual Studio 2002 ve 2003'te bir proje dosyası (.vbproj VB.NET için) ve C# için .csproj Web uygulamasındaki tüm dosyalar bilgileri depolanır.</span><span class="sxs-lookup"><span data-stu-id="838f4-132">In Visual Studio 2002 and 2003, a project file (.vbproj for VB.NET and .csproj for C#) stored information on all files in the Web application.</span></span> <span data-ttu-id="838f4-133">Çözüm Gezgini görünen proje dosyasındaki dosya bilgilere dayanır.</span><span class="sxs-lookup"><span data-stu-id="838f4-133">The Solution Explorer display is based upon the file information in the project file.</span></span> <span data-ttu-id="838f4-134">Bu nedenle, Çözüm Gezgini'nde genellikle hatalı bilgilerin dış düzenleyicileri kullanıldığı durumlarda görüntülenebilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-134">Because of this, the Solution Explorer would often display inaccurate information in cases where external editors were used.</span></span> <span data-ttu-id="838f4-135">Visual Studio 2002 ve 2003 genellikle dosya değişiklikleri üzerine mi dosyaların en son sürümünü görüntüleme.</span><span class="sxs-lookup"><span data-stu-id="838f4-135">Visual Studio 2002 and 2003 would often overwrite file changes or not display the most recent version of files.</span></span>

<span data-ttu-id="838f4-136">Visual Studio 2005 yerine proje dosyasıyla birlikte yapar.</span><span class="sxs-lookup"><span data-stu-id="838f4-136">Visual Studio 2005 does away with the project file.</span></span> <span data-ttu-id="838f4-137">Bunun yerine, projenizdeki dosyaları doğru bir görünümünü výsledek doğrudan diskten dosya ve klasör bilgilerini okur.</span><span class="sxs-lookup"><span data-stu-id="838f4-137">Instead, it reads the file and folder information directly from the disk, resulting in an accurate display of the files in your project.</span></span> <span data-ttu-id="838f4-138">Visual Studio 2002 ve 2003'nde başvurular klasörünün Web uygulamanızda gerçek bir klasör göstermediğinden, Visual Studio 2005 başvuruları klasörü Çözüm Gezgini'nden de kaldırır.</span><span class="sxs-lookup"><span data-stu-id="838f4-138">Because the References folder in Visual Studio 2002 and 2003 does not represent an actual folder in your Web application, Visual Studio 2005 also removes the References folder from Solution Explorer.</span></span> <span data-ttu-id="838f4-139">Visual Studio 2005 projeniz için başvuruları erişmek için proje için özellik sayfaları kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="838f4-139">To access the references for your project in Visual Studio 2005, you should use the Property pages for the project.</span></span>

## <a name="creating-web-projects"></a><span data-ttu-id="838f4-140">Web projeleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="838f4-140">Creating Web Projects</span></span>

<span data-ttu-id="838f4-141">Web geliştiricileri, Visual Studio 2005'te proje oluşturmaya yönelik birçok yeni seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="838f4-141">Web developers have many new options available for project creation in Visual Studio 2005.</span></span> <span data-ttu-id="838f4-142">Web siteleri artık herhangi bir dosya sisteminde oluşturulabilir ve ardından ayıklanabilir veya yeni ASP.NET geliştirme sunucusu kullanarak göz.</span><span class="sxs-lookup"><span data-stu-id="838f4-142">Web sites can now be created anywhere in the file system and can then be debugged or browsed using the new ASP.NET Development Server.</span></span> <span data-ttu-id="838f4-143">Geliştiriciler ayrıca FTP kullanarak yeni Web siteleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-143">Developers can also create new Web sites using FTP.</span></span>

<span data-ttu-id="838f4-144">Visual Studio 2005'te Web projeleri oluşturma videosu görüntülemek için buraya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="838f4-144">Click here to view a video walkthrough of creating Web projects in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image1.png)


[<span data-ttu-id="838f4-145">Açık tam ekran görüntü</span><span class="sxs-lookup"><span data-stu-id="838f4-145">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a><span data-ttu-id="838f4-146">Dosya sistemi projeleri</span><span class="sxs-lookup"><span data-stu-id="838f4-146">File System Projects</span></span>

<span data-ttu-id="838f4-147">Video kılavuzda gördüğünüz gibi yerel makinede veya bir dosya paylaşımı aracılığıyla uzak bir konumda dosya sistemi Web siteleri oluşturmak seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-147">As you saw in the video walkthrough, you can choose to create Web sites on the file system either on the local machine or on a remote location via a file share.</span></span> <span data-ttu-id="838f4-148">Dosya sistemi üzerinde oluşturulan Web siteleri, taranan ve ASP.NET geliştirme sunucusu kullanarak görüntüde hata ayıklamayı.</span><span class="sxs-lookup"><span data-stu-id="838f4-148">Web sites that are created on the file system are browsed and debugged using the ASP.NET Development Server.</span></span>

> [!NOTE]
> <span data-ttu-id="838f4-149">ASP.NET Geliştirme Sunucusu müşteriler için karışıklığa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-149">The ASP.NET Development Server may cause some confusion for customers.</span></span> <span data-ttu-id="838f4-150">IISs dizin yapısına (örneğin c:/inetpub/wwwroot) dosya sistemindeki bir Web projesi oluşturduysanız, yine de Web sitesi ile ASP.NET geliştirme sunucusu içinde Visual Studio 2005 başlatıldığında taranmasına.</span><span class="sxs-lookup"><span data-stu-id="838f4-150">If a Web project is created on the file system in IISs directory structure (i.e. c:/inetpub/wwwroot), the Web site will still be browsed via the ASP.NET Development Server when launched from within Visual Studio 2005.</span></span> <span data-ttu-id="838f4-151">Bu nedenle, herhangi bir IIS yapılandırması (yani, kimlik doğrulama yöntemleri) geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="838f4-151">Therefore, any IIS configuration (i.e. authentication methods) is not applicable.</span></span>


<span data-ttu-id="838f4-152">Varsayılan web projesi de çok kaldırır yük tarafından yalnızca bir Default.aspx sayfasında, default.cs dosya ve uygulama/_veri klasör içerir.</span><span class="sxs-lookup"><span data-stu-id="838f4-152">The default web project also removes a lot of the overhead by only includes a Default.aspx page, default.cs file, and an App/_Data folder.</span></span> <span data-ttu-id="838f4-153">Gerektiğinde özel klasör (yani uygulama/_kod) ve web.config eklenir.</span><span class="sxs-lookup"><span data-stu-id="838f4-153">The web.config and special folders (i.e. app/_code) are added as they are needed.</span></span> <span data-ttu-id="838f4-154">Web projenizi yalnızca gereksinim duyduğunuz klasör ve dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="838f4-154">Your web project only includes the files and folders that you need.</span></span>

### <a name="http-projects"></a><span data-ttu-id="838f4-155">HTTP projeleri</span><span class="sxs-lookup"><span data-stu-id="838f4-155">HTTP Projects</span></span>

<span data-ttu-id="838f4-156">HTTP projeleri yerel IIS Web sitesinde veya bir uzak Web sitesinde oluşturulan projeleri ya da olabilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-156">HTTP projects can either be projects that are created on a local IIS Web site or on a remote Web site.</span></span> <span data-ttu-id="838f4-157">Varsayılan proje konumu `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="838f4-157">The default project location is `http://localhost`.</span></span> <span data-ttu-id="838f4-158">Göz at düğmesine tıklarsanız, HTTP iki seçenek vardır: Yerel IIS ve uzak Site.</span><span class="sxs-lookup"><span data-stu-id="838f4-158">If you click the Browse button, there are two HTTP options: Local IIS and Remote Site.</span></span> <span data-ttu-id="838f4-159">Bu iki seçenek de temel fark, web sitesi bilgilerin konumu seçin iletişim kutusunda ve dosyalar Web sunucusuna nasıl kopyalanır görüntülenir yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="838f4-159">The main difference in these two options is the method in which the web site information is displayed in the Choose Location dialog and in how the files are copied to the Web server.</span></span>

<span data-ttu-id="838f4-160">Yerel IIS seçeneği metatabanı yerel makinede site bilgilerini okur ve dosya sistemi kullanılarak dosyalar kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="838f4-160">The Local IIS option reads the site information from the metabase on the local machine and files are copied using the file system.</span></span> <span data-ttu-id="838f4-161">Uzak Site seçeneğini FrontPage Server Extensions ve site bilgilerini kullanır ve HTTP kullanarak dosyalar kopyalanır ve FrontPage Server Extensions RPC çağırır.</span><span class="sxs-lookup"><span data-stu-id="838f4-161">The Remote Site option uses the FrontPage Server Extensions and the site information and files are copied using HTTP and FrontPage Server Extensions RPC calls.</span></span>

> [!NOTE]
> <span data-ttu-id="838f4-162">Artık get/_aspx/_ver.aspx ve vs###/_tmp.htm dosya sürüm bilgisi belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="838f4-162">The vs###/_tmp.htm file and get/_aspx/_ver.aspx are no longer used to determine version information.</span></span>


<span data-ttu-id="838f4-163">Varsayılan HTTP yerel IIS seçenektir.</span><span class="sxs-lookup"><span data-stu-id="838f4-163">The default HTTP option is Local IIS.</span></span> <span data-ttu-id="838f4-164">Bu seçenek, hangi siteleri kullanılabilir belirlemek için IIS metatabanı ve içerik oluşturulacağı konumu okur.</span><span class="sxs-lookup"><span data-stu-id="838f4-164">This option reads the IIS Metabase to determine which sites are available and the location in which to create content.</span></span> <span data-ttu-id="838f4-165">Ağaç görünümünde seçerek farklı bir klasör veya sanal dizin seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-165">You can select a different folder or virtual directory by selecting it in the tree view.</span></span> <span data-ttu-id="838f4-166">Ayrıca yeni bir sanal dizin oluşturma, klasörleri uygulamalar olarak işaretlemek, yapabilir bu iletişim kutusundan mevcut sanal dizinleri silin.</span><span class="sxs-lookup"><span data-stu-id="838f4-166">You can also create a new virtual directory, mark folders as applications, as well as delete existing virtual directories from this dialog box.</span></span>


![Konum iletişim seçin](improvements-in-visual-studio-2005/_static/image1.gif)

<span data-ttu-id="838f4-168">**Şekil 1**: konumu iletişim seçin</span><span class="sxs-lookup"><span data-stu-id="838f4-168">**Figure 1**: The Choose Location Dialog</span></span>


<span data-ttu-id="838f4-169">Farklı Visual Studio'nun, işaretlerseniz, önceki sürümlerde **Güvenli Yuva Katmanı kullan** , yaptığınız isteyen bir güvenlik uyarısı iletişim kutusu ile sunulan, onay ve SSL sertifikasını kullandığınız URL eşleşmiyor devam etmek istiyor.</span><span class="sxs-lookup"><span data-stu-id="838f4-169">Unlike in earlier versions of Visual Studio, if you check the **Use Secure Sockets Layer** checkbox and the SSL certificate does not match the URL you are browsing, you will be presented with a Security Alert dialog asking you if you would like to proceed.</span></span> <span data-ttu-id="838f4-170">Eşleşen bir sertifika olmasaydı Visual Studio .NET 2003 kullanarak, proje oluşturma başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="838f4-170">Using Visual Studio .NET 2003, if the certificate was not a matching one, creating the project would fail.</span></span>


![Güvenlik Uyarısı ilgili SSL sertifikası](improvements-in-visual-studio-2005/_static/image2.gif)

<span data-ttu-id="838f4-172">**Şekil 2**: SSL sertifikası ile ilgili güvenlik uyarısı</span><span class="sxs-lookup"><span data-stu-id="838f4-172">**Figure 2**: Security Alert Regarding SSL Certificate</span></span>


### <a name="note-on-host-headers"></a><span data-ttu-id="838f4-173">Konak üstbilgileri ilgili not</span><span class="sxs-lookup"><span data-stu-id="838f4-173">Note on Host Headers</span></span>

<span data-ttu-id="838f4-174">Bir sitede belirli bir IP için bağlı bir Web uygulaması oluşturuyorsanız, bir konak üstbilgisi yapılandırıldığından emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="838f4-174">If you are creating a Web application on a site bound to a specific IP, you will need to ensure that a host header is configured.</span></span> <span data-ttu-id="838f4-175">Aksi takdirde, Visual Studio sitede oluşturacak `http://localhost`, ancak IP adresi doğru olduğunda site göz veya gelen ve IDE içinde hata ayıklama çözümlemez.</span><span class="sxs-lookup"><span data-stu-id="838f4-175">Otherwise, Visual Studio will create the site at `http://localhost`, but the IP address will not resolve correctly when the site is browsed or debugged from within the IDE.</span></span>

<span data-ttu-id="838f4-176">Uzak Site seçeneğini seçerseniz, yeni Web sitesi için hedef URL'sini olanak tanımak için iletişim değiştirir.</span><span class="sxs-lookup"><span data-stu-id="838f4-176">If you select the Remote Site option, the dialog changes to allow you to enter the destination URL for the new Web site.</span></span> <span data-ttu-id="838f4-177">Bu URL, FrontPage Server Extensions etkin olan bir sunucu üzerinde olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="838f4-177">This URL must be on a server that has the FrontPage Server Extensions enabled.</span></span> <span data-ttu-id="838f4-178">FrontPage Server Extensions'ı kullanarak yerel Web sunucunuz ile çalışmak istiyorsanız, uzak Site seçeneğini kullanın ve yerel bir URL belirtin.</span><span class="sxs-lookup"><span data-stu-id="838f4-178">If you want to work with your local Web server using the FrontPage Server Extensions, you can use the Remote Site option and specify a local URL.</span></span>


![Uzak bir sunucuda bir Web sitesi oluşturma](improvements-in-visual-studio-2005/_static/image1.jpg)

<span data-ttu-id="838f4-180">**Şekil 3**: Uzak bir sunucuda bir Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="838f4-180">**Figure 3**: Creating a Web Site on a Remote Server</span></span>


<span data-ttu-id="838f4-181">SSL sertifikası eşleşmiyorsa SSL üzerinden uzak bir sitede bir uygulama oluştururken, onay iletişim kutusunda yerel IIS seçeneği kullanılırken görüntülenen iletişim biraz farklıdır.</span><span class="sxs-lookup"><span data-stu-id="838f4-181">When creating an application on a remote site via SSL, if the SSL certificate does not match, the confirmation dialog is slightly different than the dialog displayed when using the Local IIS option.</span></span>


![Uzak Site Güvenlik Uyarısı](improvements-in-visual-studio-2005/_static/image3.gif)

<span data-ttu-id="838f4-183">**Şekil 4**: Uzak Site Güvenlik Uyarısı</span><span class="sxs-lookup"><span data-stu-id="838f4-183">**Figure 4**: The Remote Site Security Alert</span></span>


<a id="_Toc116100243"></a>

#### <a name="ftp"></a><span data-ttu-id="838f4-184">FTP</span><span class="sxs-lookup"><span data-stu-id="838f4-184">FTP</span></span>

<span data-ttu-id="838f4-185">Visual Studio 2005 FTP üzerinden Web siteleri oluşturmak için bu seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="838f4-185">Visual Studio 2005 introduces the option to create Web sites via FTP.</span></span> <span data-ttu-id="838f4-186">Bu seçeneği kullandığınızda, IDE dosyaları yerel olarak kullanıcıların geçici klasörde oluşturur ve ardından dosyaları FTP konuma taşımak için FTP kullanır.</span><span class="sxs-lookup"><span data-stu-id="838f4-186">When you use this option, the IDE creates the files locally in the users temp folder and then uses FTP to move the files to the FTP location.</span></span>

> [!NOTE]
> <span data-ttu-id="838f4-187">Geçici klasör konumu c:/belgeler ve ayarlar olduğu /&lt;kullanıcı&gt;/yerel ayarları/Temp/VWDWebCache/&lt;sunucu&gt;/_&lt;uygulama adı&gt;</span><span class="sxs-lookup"><span data-stu-id="838f4-187">The temp folder location is c:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>


<span data-ttu-id="838f4-188">FTP seçeneği kullanılırken, bir konum seçin iletişim kutusu ile sunulur.</span><span class="sxs-lookup"><span data-stu-id="838f4-188">When using the FTP option, you will be presented with a Choose Location dialog.</span></span> <span data-ttu-id="838f4-189">Aşağıda gösterildiği gibi bu iletişim kutusunda, gerekli FTP bağlantı bilgilerini girin.</span><span class="sxs-lookup"><span data-stu-id="838f4-189">You enter the required FTP connection information into this dialog as shown below.</span></span>


![FTP konumu iletişim seçin](improvements-in-visual-studio-2005/_static/image2.jpg)

<span data-ttu-id="838f4-191">**Şekil 5**: FTP konumu iletişim seçin</span><span class="sxs-lookup"><span data-stu-id="838f4-191">**Figure 5**: The Choose Location Dialog for FTP</span></span>


## <a name="lab-setup-ftp-site-and-create-a-project"></a><span data-ttu-id="838f4-192">Laboratuvar: Kurulum FTP sitesi ve bir proje oluşturun</span><span class="sxs-lookup"><span data-stu-id="838f4-192">Lab: Setup FTP site and create a project</span></span>

<span data-ttu-id="838f4-193">Bir kullanıcı yalnızca bunlar için FTP aracılığıyla karşıya yükleyebilecek bir konuma sahip olacak şekilde aşağıdaki adımları FTP sitesini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="838f4-193">The following steps configure the FTP site so that a user has a location that only they can upload to via FTP.</span></span>

### <a name="install-the-ftp-service"></a><span data-ttu-id="838f4-194">FTP hizmetinin yükleme</span><span class="sxs-lookup"><span data-stu-id="838f4-194">Install the FTP Service</span></span>

1. <span data-ttu-id="838f4-195">Program Ekle/Kaldır'ı açın, Windows Bileşenlerini Ekle/Kaldır'ı seçin</span><span class="sxs-lookup"><span data-stu-id="838f4-195">Open Add Remove Programs, select Add/Remove Windows Components</span></span>
2. <span data-ttu-id="838f4-196">Internet Information Services (Windows 2003'te uygulama sunucusu) seçin ve tıklayın **ayrıntıları**.</span><span class="sxs-lookup"><span data-stu-id="838f4-196">Select Internet Information Services (Application Server on Windows 2003) and click **Details**.</span></span>
3. <span data-ttu-id="838f4-197">Denetleme **Dosya Aktarım Protokolü (FTP) hizmet** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="838f4-197">Check **File Transfer Protocol (FTP) Service** and click **OK**.</span></span>
4. <span data-ttu-id="838f4-198">Tıklayın **sonraki** FTP hizmeti yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="838f4-198">Click **Next** to install the FTP service.</span></span>

### <a name="create-a-new-folder-for-content"></a><span data-ttu-id="838f4-199">Yeni bir klasör için içerik oluşturun</span><span class="sxs-lookup"><span data-stu-id="838f4-199">Create a New Folder for Content</span></span>

1. <span data-ttu-id="838f4-200">Windows Gezgini'nde, adlı yeni bir klasör oluşturma **User1** inetpub/c:/wwwroot içinde.</span><span class="sxs-lookup"><span data-stu-id="838f4-200">In Windows Explorer, create a new folder called **User1** inside of c:/inetpub/wwwroot.</span></span>

#### <a name="configure-folders-and-permissions-on-folders"></a><span data-ttu-id="838f4-201">Klasörleri ve izinlerini klasörleri yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="838f4-201">Configure folders and permissions on folders.</span></span>

1. <span data-ttu-id="838f4-202">Internet Information Services ek bileşenini Yönetim Araçları'ndan açın.</span><span class="sxs-lookup"><span data-stu-id="838f4-202">Open the Internet Information Services snap-in from Administrative Tools.</span></span> <span data-ttu-id="838f4-203">Bilgisayar adı düğümün altında bir FTP siteleri klasörünü şimdi gerekir.</span><span class="sxs-lookup"><span data-stu-id="838f4-203">You will now have an FTP Sites folder under the computer name node.</span></span>
2. <span data-ttu-id="838f4-204">Genişletin **FTP siteleri**.</span><span class="sxs-lookup"><span data-stu-id="838f4-204">Expand **FTP Sites**.</span></span>
3. <span data-ttu-id="838f4-205">Sağ **varsayılan FTP sitesi**seçin **yeni**, ardından **sanal dizin**, ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="838f4-205">Right-click the **Default FTP Site**, select **New**, then **Virtual Directory**, then click **Next**.</span></span>
4. <span data-ttu-id="838f4-206">Girin **User1** tıklayın ve sanal dizin adı için **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="838f4-206">Enter **User1** for the virtual directory name and click **Next**.</span></span>
5. <span data-ttu-id="838f4-207">Girin **c:/inetpub/wwwroot/User1** tıklayın ve yolu için **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="838f4-207">Enter **c:/inetpub/wwwroot/User1** for the path and click **Next**.</span></span>
6. <span data-ttu-id="838f4-208">Tıklayın **sonraki** ardından **son** Sihirbazı tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="838f4-208">Click **Next** and then **Finish** to complete the wizard.</span></span>
7. <span data-ttu-id="838f4-209">Sağ **User1** varsayılan FTP sitesi ve select altında sanal dizin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="838f4-209">Right-click the **User1** virtual directory under Default FTP Site and select **Properties**.</span></span>
8. <span data-ttu-id="838f4-210">Denetleme **yazma** onay kutusunu tıklatıp **Tamam** iletişim kutusunu kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="838f4-210">Check the **Write** checkbox and click **OK** to close the dialog.</span></span>
9. <span data-ttu-id="838f4-211">Sağ **varsayılan FTP sitesi** seçip **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="838f4-211">Right-click **Default FTP Site** and select **Properties**.</span></span>
10. <span data-ttu-id="838f4-212">Üzerinde **güvenlik hesapları** sekmesinde, onay kutusunu temizleyin **anonim bağlantılara izin ver**.</span><span class="sxs-lookup"><span data-stu-id="838f4-212">On the **Security Accounts** tab, uncheck **Allow Anonymous Connections**.</span></span>
11. <span data-ttu-id="838f4-213">Tıklayın **Evet** devam etmek isteyip istemediğinizi soran iletişim kutusunda.</span><span class="sxs-lookup"><span data-stu-id="838f4-213">Click **Yes** in the dialog asking if you want to continue.</span></span>
12. <span data-ttu-id="838f4-214">Tıklayın **Tamam** iletişim kutusunu kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="838f4-214">Click **OK** to close the dialog.</span></span>
13. <span data-ttu-id="838f4-215">Genişletin **varsayılan Web sitesi** altında **Web siteleri** düğümü.</span><span class="sxs-lookup"><span data-stu-id="838f4-215">Expand the **Default Web Site** under the **Web Sites** node.</span></span>
14. <span data-ttu-id="838f4-216">Sağ **User1** dizin ve select **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="838f4-216">Right-click the **User1** directory and select **Properties**</span></span>
15. <span data-ttu-id="838f4-217">İçinde **uygulama ayarları** bölümünde **Oluştur** klasörü bir uygulama olarak işaretlenecek.</span><span class="sxs-lookup"><span data-stu-id="838f4-217">In the **Application Settings** section, click **Create** to mark the folder as an application.</span></span>
16. <span data-ttu-id="838f4-218">Tıklayın **Tamam** iletişim kutusunu kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="838f4-218">Click **OK** to close the dialog.</span></span>
17. <span data-ttu-id="838f4-219">Internet Information Services eklentisini kapatın.</span><span class="sxs-lookup"><span data-stu-id="838f4-219">Close the Internet Information Services snap-in.</span></span>

### <a name="create-web-project"></a><span data-ttu-id="838f4-220">Web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="838f4-220">Create web project</span></span>

1. <span data-ttu-id="838f4-221">Visual Studio 2005'i açın.</span><span class="sxs-lookup"><span data-stu-id="838f4-221">Open Visual Studio 2005.</span></span>
2. <span data-ttu-id="838f4-222">Gelen **dosya** menüsünde **yeni Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="838f4-222">From the **File** menu, select **New Web Site**.</span></span>
3. <span data-ttu-id="838f4-223">İçinde **konumu** açılır menüsünde, select **FTP**.</span><span class="sxs-lookup"><span data-stu-id="838f4-223">In the **Location** dropdown, select **FTP**.</span></span>
4. <span data-ttu-id="838f4-224">**Gözat**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="838f4-224">Click **Browse**.</span></span>
5. <span data-ttu-id="838f4-225">Girin **localhost** içinde **sunucu** metin.</span><span class="sxs-lookup"><span data-stu-id="838f4-225">Enter **localhost** in the **Server** textbox.</span></span>
6. <span data-ttu-id="838f4-226">Girin **User1** dizin metin kutusuna.</span><span class="sxs-lookup"><span data-stu-id="838f4-226">Enter **User1** in the Directory textbox.</span></span>
7. <span data-ttu-id="838f4-227">Tıklayın **açık**.</span><span class="sxs-lookup"><span data-stu-id="838f4-227">Click **Open**.</span></span> <span data-ttu-id="838f4-228">FTP konumu yeni Web sitesi iletişim kutusuna girilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-228">The FTP location will be entered into the New Web Site dialog.</span></span>
8. <span data-ttu-id="838f4-229">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="838f4-229">Click **OK**.</span></span>
9. <span data-ttu-id="838f4-230">Onay kutusunu temizleyin **anonim oturum açma** FTP oturum açma iletişim kutusunda, kimlik bilgilerinizi girin ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="838f4-230">Uncheck **Anonymous log on** in the FTP Log On dialog, enter your credentials, and click **OK**.</span></span>
10. <span data-ttu-id="838f4-231">Proje URL'si nedir?</span><span class="sxs-lookup"><span data-stu-id="838f4-231">What is the URL for the project?</span></span> <span data-ttu-id="838f4-232">(Çözüm Gezgini'nde proje URL'sini görüntülenir.)</span><span class="sxs-lookup"><span data-stu-id="838f4-232">(The URL for the project will be displayed in Solution Explorer.)</span></span>
11. <span data-ttu-id="838f4-233">Gelen **derleme** menüsünde **derleme Web sitesi** veya **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="838f4-233">From the **Build** menu, select **Build Web Site** or **Build Solution**.</span></span>
12. <span data-ttu-id="838f4-234">Çözüm Gezgini içinde default.aspx öğesini sağ tıklatın ve seçin **tarayıcıda görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="838f4-234">Right-click on Default.aspx in Solution Explorer and select **View in Browser**.</span></span>
13. <span data-ttu-id="838f4-235">Web sitesi URL'si gerekli iletişim kutusuna girin `http://localhost/user1` tıklatın ve URL için **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="838f4-235">In the Web Site URL Required dialog, enter `http://localhost/user1` for the URL and click **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="838f4-236">Türü /_Default yüklenecek bağlantı kurma sorunu olduğunu belirten bir hata alırsanız, Web siteniz ve daha önceki bir sürümü üzerinde ASP.NET 2.0 çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="838f4-236">If you get a error indicating an inability to load the type /_Default, make sure that you are running ASP.NET 2.0 on your Web site and not an earlier version.</span></span> <span data-ttu-id="838f4-237">Internet Information Services'ın ASP.NET sekmesinden bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-237">You can do that from the ASP.NET tab in Internet Information Services.</span></span>


## <a name="opening-web-projects"></a><span data-ttu-id="838f4-238">Açılış Web projeleri</span><span class="sxs-lookup"><span data-stu-id="838f4-238">Opening Web Projects</span></span>

<span data-ttu-id="838f4-239">Web projelerini açmak, proje oluşturma işlemiyle benzerdir.</span><span class="sxs-lookup"><span data-stu-id="838f4-239">Opening Web projects is similar to creating projects.</span></span> <span data-ttu-id="838f4-240">Aşağıdaki bölümlerde, çıkış için IDE içinde çalışırken takip etmek için alanları ilgilendiren.</span><span class="sxs-lookup"><span data-stu-id="838f4-240">The following sections call out areas to keep an eye out for while working within the IDE.</span></span> <span data-ttu-id="838f4-241">Ayrıca, HTTP ve FTP kullanarak Web projeleri ile çalışmayı kapsar.</span><span class="sxs-lookup"><span data-stu-id="838f4-241">It also covers working with Web projects using HTTP and FTP.</span></span>

<span data-ttu-id="838f4-242">Bir Web projesi açmak için Dosya menüsünden Web sitesini Aç'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="838f4-242">To open a Web project, select Open Web Site from the File menu.</span></span> <span data-ttu-id="838f4-243">Daha önce ele aynı konumu seçin iletişim kutusu ile istenir ve kullanabileceğiniz aynı dört seçenekleriniz vardır: dosya sistemi, yerel IIS, FTP ve uzak Site.</span><span class="sxs-lookup"><span data-stu-id="838f4-243">You will be prompted with the same Choose Location dialog covered previously and you have the same four options available to you: File System, Local IIS, FTP, and Remote Site.</span></span>

<a id="_Toc116100245"></a>

## <a name="file-system"></a><span data-ttu-id="838f4-244">Dosya sistemi</span><span class="sxs-lookup"><span data-stu-id="838f4-244">File System</span></span>

<span data-ttu-id="838f4-245">Bu modülde daha önce belirtildiği gibi Visual Studio artık proje dosyası kullanır.</span><span class="sxs-lookup"><span data-stu-id="838f4-245">As indicated previously in this module, Visual Studio no longer uses a project file.</span></span> <span data-ttu-id="838f4-246">Bu nedenle, dosya sisteminden bir Web sitesini açmak isterseniz, aslında seçtiğiniz klasöre başlangıçta Visual Studio'da bir Web projesi olarak oluşturulmamış olsa bile, istediğiniz herhangi bir klasör seçme seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="838f4-246">Therefore, if you choose to open a Web site from the file system, you actually have the option of choosing any folder that you wish, even if the folder you choose was not created as a Web project initially in Visual Studio.</span></span> <span data-ttu-id="838f4-247">Örneğin, bir Web sitesi olarak Belgelerim klasörünü açmak seçin ve Visual Studio sonsuza dek açın ve aşağıda gösterildiği gibi dosyalarınızı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="838f4-247">For example, you can choose to open the My Documents folder as a Web site and Visual Studio will happily open it and display your files as shown below.</span></span>


![Bir Web sitesi açılan Belgelerim](improvements-in-visual-studio-2005/_static/image3.jpg)

<span data-ttu-id="838f4-249">**Şekil 6**: *Belgelerim* bir Web sitesi açılır</span><span class="sxs-lookup"><span data-stu-id="838f4-249">**Figure 6**: *My Documents* Opened As a Web Site</span></span>


<span data-ttu-id="838f4-250">Visual Studio, yalnızca ek dosyaları ve klasörleri gerektiğinde oluşturduğundan, hiçbir ek dosya veya klasör, açık konuma eklenir.</span><span class="sxs-lookup"><span data-stu-id="838f4-250">Because Visual Studio only creates additional files and folders when necessary, no additional files or folders are added to the location you open.</span></span> <span data-ttu-id="838f4-251">Bu mimarinin yan etkisi, Web siteleri dosya sisteminde iç içe engellenmesidir.</span><span class="sxs-lookup"><span data-stu-id="838f4-251">A side-effect of this architecture is that it prevents you from nesting Web sites on the file system.</span></span> <span data-ttu-id="838f4-252">Örneğin, aşağıdaki dizin yapısını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="838f4-252">For example, consider the following directory structure.</span></span>

<span data-ttu-id="838f4-253">C:/numaralı Web projesi</span><span class="sxs-lookup"><span data-stu-id="838f4-253">Web project at C:/MyWebSite</span></span>

<span data-ttu-id="838f4-254">C:/numaralı/iç içe başka bir web projesi</span><span class="sxs-lookup"><span data-stu-id="838f4-254">Another web project at C:/MyWebSite/Nested</span></span>

<span data-ttu-id="838f4-255">C:/numaralı Web sitesinde açtığınızda, iç içe geçmiş klasör bu uygulamanın bir alt klasör görünür.</span><span class="sxs-lookup"><span data-stu-id="838f4-255">When you open the Web site at c:/MyWebSite, the Nested folder will appear as a sub-folder of that application.</span></span>

<a id="_Toc116100246"></a>

## <a name="http"></a><span data-ttu-id="838f4-256">HTTP</span><span class="sxs-lookup"><span data-stu-id="838f4-256">HTTP</span></span>

<span data-ttu-id="838f4-257">HTTP üzerinden Web siteleri açarken ayarları (yerel IIS) IIS metatabanı veya FrontPage Server Extensions (Uzak Site.) kullanarak okunur İç içe geçmiş web uygulamaları varsa, bunlar da bunları bir uygulama olarak tanımlayan bir simge ile gösterilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-257">When opening Web sites via HTTP, settings are read either from the IIS metabase (Local IIS) or using FrontPage Server Extensions (Remote Site.) If there are nested web applications, these are displayed as well with an icon identifying them as an application.</span></span> <span data-ttu-id="838f4-258">FrontPage web uygulamaları ile birlikte çalışma bilginiz varsa, Visual Studio 2005 davranış benzerdir.</span><span class="sxs-lookup"><span data-stu-id="838f4-258">If you are familiar with working with web applications in FrontPage, the behavior in Visual Studio 2005 is similar.</span></span>

<span data-ttu-id="838f4-259">Visual Studio IDE içinde şu anda açıldığında uygulamanın altında iç içe uygulamalar için bir simge görüntüler olsa da, bunları içeriklerini görmek için genişletmenize sağlayamaz.</span><span class="sxs-lookup"><span data-stu-id="838f4-259">Even though Visual Studio will display an icon for applications that are nested beneath the application that is currently opened within the IDE, it will not allow you to expand them to see their content.</span></span> <span data-ttu-id="838f4-260">Ancak, bunları açmak için çift tıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-260">You can, however, double-click on them to open them.</span></span> <span data-ttu-id="838f4-261">Bunu yaptığınızda ya da web uygulamasını (ve şu anda açık olan çözümü değiştirmek için) isteyen bir iletişim kutusu ile sunulur veya geçerli çözümünüzden Web uygulamasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="838f4-261">When you do, you will be presented with a dialog prompting you to either open the web application (and replace the currently open solution) or add the Web application to your current solution.</span></span>


![İç içe geçmiş uygulama simgesini çift ile bu iletişim kutusunu gösterir](improvements-in-visual-studio-2005/_static/image4.jpg)

<span data-ttu-id="838f4-263">**Şekil 7**: iç içe geçmiş uygulama simgesini çift sunar, bu iletişim kutusunda</span><span class="sxs-lookup"><span data-stu-id="838f4-263">**Figure 7**: Double-clicking a nested application icon presents you with this dialog</span></span>


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a><span data-ttu-id="838f4-264">FTP sitesi</span><span class="sxs-lookup"><span data-stu-id="838f4-264">FTP Site</span></span>

<span data-ttu-id="838f4-265">FTP aracılığıyla bir siteyi açtığınızda, dosyalar tüm yerel geçici klasörünüze kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="838f4-265">When you open a site via FTP, the files are all copied locally to your temp folder.</span></span> <span data-ttu-id="838f4-266">Yerel depolama konumu için tam yol, proje için Özellikler bölmesi görüntülenir ve aşağıdaki biçimi kullanarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="838f4-266">The full path for the local storage location is displayed in the Properties pane for the project and is created using the following format.</span></span>

<span data-ttu-id="838f4-267">C:/belgeler ve ayarlar /&lt;kullanıcı&gt;/yerel ayarları/Temp/VWDWebCache/&lt;sunucu&gt;/_&lt;uygulama adı&gt;</span><span class="sxs-lookup"><span data-stu-id="838f4-267">C:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>

<span data-ttu-id="838f4-268">FTP kullanarak, Visual Studio projeniz için temel URL'yi aşağıda gösterildiği gibi gözatabilirsiniz belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="838f4-268">When using FTP, Visual Studio will need to specify the base URL for your project so that you can browse it as shown below.</span></span> <span data-ttu-id="838f4-269">Temel bir URL belirtmezseniz, Visual Studio, bunları Web sitesinde bir sayfaya göz atın girişimi ilk kez isteriz.</span><span class="sxs-lookup"><span data-stu-id="838f4-269">If you do not specify a base URL, Visual Studio will ask you for it the first time you attempt to browse a page in the Web site.</span></span>


![FTP siteleri için temel URL belirtme](improvements-in-visual-studio-2005/_static/image5.jpg)

<span data-ttu-id="838f4-271">**Şekil 8**: FTP siteleri için temel URL belirtme</span><span class="sxs-lookup"><span data-stu-id="838f4-271">**Figure 8**: Specifying a Base URL for FTP Sites</span></span>


## <a name="improvements-in-compilation"></a><span data-ttu-id="838f4-272">Derleme geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="838f4-272">Improvements in Compilation</span></span>

<span data-ttu-id="838f4-273">Web uygulamaları Visual Studio 2005 ile çalışma önceki sürümlerden önemli ölçüde daha hızlı gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="838f4-273">Working with Web applications in Visual Studio 2005 is noticeably faster than previous versions.</span></span> <span data-ttu-id="838f4-274">Derleme mimari değişiklikler nedeniyle hiçbir küçük bir bölümü budur.</span><span class="sxs-lookup"><span data-stu-id="838f4-274">This is due in no small part to the changes in compilation architecture.</span></span>

<span data-ttu-id="838f4-275">Visual Studio 2002 ve 2003'te Web uygulamaları / Bin klasöründe bulunan bir birincil derlemesini derlendi.</span><span class="sxs-lookup"><span data-stu-id="838f4-275">In Visual Studio 2002 and 2003, Web applications were compiled into one primary assembly residing in the /bin folder.</span></span> <span data-ttu-id="838f4-276">Visual Studio 2005'te, bir uygulama/_kod klasör eklendi.</span><span class="sxs-lookup"><span data-stu-id="838f4-276">In Visual Studio 2005, an App/_Code folder was added.</span></span> <span data-ttu-id="838f4-277">Sınıfları ve diğer UI olmayan kod uygulama/_kod klasörüne eklenir.</span><span class="sxs-lookup"><span data-stu-id="838f4-277">Classes and other non-UI code are added to the App/_Code folder.</span></span> <span data-ttu-id="838f4-278">Visual Studio projeyi oluşturduğunda, uygulama/_kod klasördeki tüm dosyaları tek bir App/_Code.dll dosyasına derlenir.</span><span class="sxs-lookup"><span data-stu-id="838f4-278">When Visual Studio builds the project, all files in the App/_Code folder are compiled into a single App/_Code.dll file.</span></span> <span data-ttu-id="838f4-279">Bu değişikliğin sonucu ardışık derlemeler önceki sürümlerde çok daha hızlı olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="838f4-279">The result of this change is that subsequent builds are much faster than in previous versions.</span></span>

> [!NOTE]
> <span data-ttu-id="838f4-280">MSBuild komut satırı yardımcı programı, ASP.NET Web uygulamaları oluşturmak için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-280">The MSBuild command line utility can also be used to build ASP.NET Web applications.</span></span> <span data-ttu-id="838f4-281">Bu aracı 9 modülünde ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="838f4-281">That tool will be covered in module 9.</span></span>


<span data-ttu-id="838f4-282">Başka bir derleme geliştirme derle menüsünde yeni yapı sayfası seçeneğidir.</span><span class="sxs-lookup"><span data-stu-id="838f4-282">Another compilation enhancement is the new Build Page option on the Build menu.</span></span> <span data-ttu-id="838f4-283">Bu özellik, böylece değişiklikleri daha hızlı bir şekilde derlenebilir yalnızca geçerli sayfa (ile birlikte, kurs ve bağımlılıkları) yeniden derlemek bir geliştirici sağlar.</span><span class="sxs-lookup"><span data-stu-id="838f4-283">This feature allows a developer to rebuild only the current page (along with, of course, and dependencies) so that changes can be compiled more quickly.</span></span> <span data-ttu-id="838f4-284">C#, IntelliSense, vb. güncelleştirmek amacıyla arka plan derlemesi sağlamaz çünkü yalnızca tek bir sayfayı yeniden oluşturmayı hızlı bir şekilde güncelleştirilmesi IntelliSense izin bunlar iyice bu özellikten yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-284">Because C# does not offer background compilation for purposes of updating IntelliSense, etc., they will benefit immensely from this feature because it will allow for IntelliSense to be updated quickly by simply rebuilding a single page.</span></span>

<span data-ttu-id="838f4-285">Proje derleme özelliklerini başlangıç sayfasını yürütülmeden önce oluşan yapı türünü yapılandırmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="838f4-285">The Build properties for a project allow you to configure the type of build that occurs before the startup page is executed.</span></span> <span data-ttu-id="838f4-286">Geliştiriciler Visual Studio uygulamalarında kod değişikliklerinden sonra daha hızlı hata ayıklama başlayabilmesi yalnızca geçerli sayfayı yapı seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-286">Developers can choose to only build the current page so that Visual Studio can start debugging applications more quickly after code changes.</span></span>


![Derleme sayfası başlangıç eylemi](improvements-in-visual-studio-2005/_static/image6.jpg)

<span data-ttu-id="838f4-288">**Şekil 9**: derleme sayfa başlangıç eylemi</span><span class="sxs-lookup"><span data-stu-id="838f4-288">**Figure 9**: The Build Page Start Action</span></span>


<span data-ttu-id="838f4-289">Visual Studio ve ASP.NET mimarisinin harika geliştirme başka bir düzenleme alanındadır ve devam edin.</span><span class="sxs-lookup"><span data-stu-id="838f4-289">Another great enhancement to Visual Studio and the ASP.NET architecture is in the area of edit and continue.</span></span> <span data-ttu-id="838f4-290">Geliştiriciler Visual Studio 2005'te bir projesinde hata ayıklamaya başlama ve hata ayıklayıcı ayırma olmadan projede kod değişiklikleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-290">In Visual Studio 2005, developers can start debugging a project and make code changes on the project without detaching the debugger.</span></span> <span data-ttu-id="838f4-291">Aslında, gerçek anlamda bir projede hata ayıklamaya başlayabilirsiniz yeni bir sınıf daha ekleyin, o sınıf için kod ekleme, kod bu sınıfın yeni bir örneğini oluşturan sayfanıza eklemek ve sınıfın tüm hata ayıklayıcı ayırma olmadan bir yöntem yürütülmeye.</span><span class="sxs-lookup"><span data-stu-id="838f4-291">In fact, you can literally start debugging a project, add a new class, add code to that class, add code to your page that creates a new instance of that class and execute a method of the class, all without detaching the debugger.</span></span> <span data-ttu-id="838f4-292">Yeni kod yürüten tarayıcıyı yenilemeyi olarak tam anlamıyla oldukça kolaydır!</span><span class="sxs-lookup"><span data-stu-id="838f4-292">Executing the new code is literally as easy as refreshing the browser!</span></span>

<span data-ttu-id="838f4-293">Video düzenleme bkz ve Visual Studio 2005'te özellik devam etmek için buraya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="838f4-293">Click here to see a video walkthrough of the edit and continue feature in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image2.png)


[<span data-ttu-id="838f4-294">Açık tam ekran görüntü</span><span class="sxs-lookup"><span data-stu-id="838f4-294">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


<span data-ttu-id="838f4-295">Güçlü düzenleyebilir veya işlevselliği ASP.NET 2.0 ile devam edin ve bir değişimdir ASP.NET uygulamaları için Visual Studio 2005 kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="838f4-295">The robust edit and continue functionality in ASP.NET 2.0 and Visual Studio 2005 is due to an architectural change for ASP.NET applications.</span></span> <span data-ttu-id="838f4-296">ASP.NET'te 1.x, Visual Studio 2002/2003'te oluşturulan uygulamalar / bin klasöründeki birincil bir bütünleştirilmiş kod içine derlenmiş.</span><span class="sxs-lookup"><span data-stu-id="838f4-296">In ASP.NET 1.x, applications created in Visual Studio 2002/2003 were compiled into a primary assembly that was stored in the /bin folder.</span></span> <span data-ttu-id="838f4-297">Tüm sınıflar, sayfalar, vb. uygulama derlenmiş için tek bir DLL.</span><span class="sxs-lookup"><span data-stu-id="838f4-297">All classes, pages, etc. for the application were compiled into that one DLL.</span></span> <span data-ttu-id="838f4-298">Ardından çalışma zamanında ASP.NET tüm denetimleri, biçimlendirme ve sayfaları içinde ASP.NET kodu derleyin ve bu DLL'leri ASP.NET geçici klasöre kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="838f4-298">Then at runtime, ASP.NET would compile all of the controls, markup, and ASP.NET code within pages and copy those DLLs into the ASP.NET temporary folder.</span></span>

<span data-ttu-id="838f4-299">ASP.NET 2. 0'da, çalışma zamanında iki derleme modelleri (Visual Studio için bir tane) ve ASP.NET için yukarıda ana hatlarıyla kullanarak Visual Studio 2005 ortak derleme modeline birleştirildi.</span><span class="sxs-lookup"><span data-stu-id="838f4-299">In Visual Studio 2005 using ASP.NET 2.0, the two compilation models outline above (one for Visual Studio and one for ASP.NET at runtime) have been merged into one common compilation model.</span></span> <span data-ttu-id="838f4-300">Tüm derleme sorunları artık yerine geliştirme aşamasında çalışma zamanında yakalanabilmesini ve anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="838f4-300">That means that all compilation issues are now caught during the development stage instead of at runtime.</span></span> <span data-ttu-id="838f4-301">Tasarımcı ve kullanıcı denetimleri ve ana sayfalar gibi özellikler için IntelliSense desteği de sağlar.</span><span class="sxs-lookup"><span data-stu-id="838f4-301">It also allows for designer and IntelliSense support for features such as user controls and master pages.</span></span>

<span data-ttu-id="838f4-302">Kullanıcı denetimleri için tasarımcı desteği videosu görmek için buraya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="838f4-302">Click here to see a video walkthrough of designer support for user controls.</span></span>


![](improvements-in-visual-studio-2005/_static/image3.png)


[<span data-ttu-id="838f4-303">Açık tam ekran görüntü</span><span class="sxs-lookup"><span data-stu-id="838f4-303">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> <span data-ttu-id="838f4-304">Bir kullanıcı denetimi bir sayfadan kaldırıldığında @Register yönergesi işaretlemede kalır ve kullanıcı denetimi Web sitesinden silinirse ayrıştırıcı hatalarını önlemek için el ile kaldırılması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="838f4-304">When a user control is removed from a page, the @Register directive remains in the markup and should be removed manually in order to avoid parser errors if the user control is deleted from the Web site.</span></span>


<span data-ttu-id="838f4-305">Visual Studio derleme modelinde başka bir geliştirme, Web sitesi yayımlama özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="838f4-305">Another improvement in the Visual Studio compilation model is the Publish Web Site feature.</span></span> <span data-ttu-id="838f4-306">Geliştiriciler, Yayımla özelliğini Web sitesi işlemini gerçekleştirir. çünkü derleme isteğe bağlı herhangi bir şey yapmak zorunda eklenen performansı keyfini çıkarabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-306">Because the Publish feature precompiles the Web site, developers can enjoy the added performance of not having to compile anything on demand.</span></span> <span data-ttu-id="838f4-307">Dağıtılacak kaynak kodu yok sahip olacak şekilde, aynı zamanda uygulama/_kod klasöründeki tüm kaynak kodu bir DLL içine işlemini gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="838f4-307">It also precompiles all source code in the App/_Code folder into a DLL so that no source code has to be deployed.</span></span>


![Yayımla Web sitesi iletişim kutusu](improvements-in-visual-studio-2005/_static/image7.jpg)

<span data-ttu-id="838f4-309">**Şekil 10**: Yayımla Web sitesi iletişim kutusu</span><span class="sxs-lookup"><span data-stu-id="838f4-309">**Figure 10**: The Publish Web Site Dialog</span></span>


> [!NOTE]
> <span data-ttu-id="838f4-310">Aspnet/_compile.exe yardımcı programı, bir ASP.NET Web uygulamasına önceden derlemek için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-310">The aspnet/_compile.exe utility can also be used to pre-compile an ASP.NET Web application.</span></span> <span data-ttu-id="838f4-311">Bu aracı 9 modülünde ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="838f4-311">That tool will be covered in module 9.</span></span>


<span data-ttu-id="838f4-312">Ne zaman aşağıda gösterildiği gibi Yayımla bir Web sitesi önceden derlenmiş dosyaları ASP.NET dosyaları klasöründe depolanır.</span><span class="sxs-lookup"><span data-stu-id="838f4-312">When you Publish a Web site, the precompiled files are stored in the Temporary ASP.NET Files folder as shown below.</span></span> <span data-ttu-id="838f4-313">İle dosyaları bir *.compiled* dosya uzantısı olan belirli DLL'ler için bağımlıkları tanımlama XML dosyaları.</span><span class="sxs-lookup"><span data-stu-id="838f4-313">Files with a *.compiled* file extension are XML files that define dependencies for particular DLLs.</span></span> <span data-ttu-id="838f4-314">Herhangi bir Webform veya kullanıcı denetimleri ile başlayan rastgele DLL'leri içine derlenmiş *uygulama /_Web /_*.</span><span class="sxs-lookup"><span data-stu-id="838f4-314">Any Webform or user controls are compiled into random DLLs that begin with *App/_Web/_*.</span></span>

<span data-ttu-id="838f4-315">Bırakırsanız *güncelleştirilebilir bu önceden derlenmiş sitenin izin* onay kutusu seçili, biçimlendirme, Webforms ve kullanıcı denetimleri içinde dağıtımdan sonra değişiklik yapmanızı sağlayan bir DLL içine önceden derlenmiş olmayacak.</span><span class="sxs-lookup"><span data-stu-id="838f4-315">If you leave the *Allow this precompiled site to be updatable* checkbox checked, markup inside of your Webforms and user controls will not be pre-compiled into a DLL allowing you to make changes after deployment.</span></span> <span data-ttu-id="838f4-316">Böylece değişiklikleri dağıtılan içerik için izin verilmeyen biçimlendirme kilitlemek tercih ederseniz, bu kutunun işaretini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="838f4-316">If you would prefer to lock down the markup so that changes to the deployed content are not allowed, uncheck this box.</span></span>

<span data-ttu-id="838f4-317">*Kullanım sabit adlandırma ve tek sayfa bütünleştirilmiş kodlarını* , böylece her sayfada sabit adlandırılmış bir bütünleştirilmiş kod içine derlenmiş toplu derlemeyi devre dışı bırakmak onay kutusunu olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="838f4-317">The *Use fixed naming and single page assemblies* checkbox allows you to disable batch compilation so that each page is compiled into a fixed-named assembly.</span></span> <span data-ttu-id="838f4-318">Bu kutusunun işaretlenmemesi Toplu derleme avantajlarından yararlanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="838f4-318">Leaving this box unchecked allows you to take advantage of batch compilation.</span></span>

<span data-ttu-id="838f4-319">*Önceden derlenmiş derlemeleri etkinleştir üzerinde güçlü adlandırma* onay verir tanımlayıcı ad için önceden derlenmiş bütünleştirilmiş kodlarınızı.</span><span class="sxs-lookup"><span data-stu-id="838f4-319">The *Enable strong naming on precompiled assemblies* checkbox allows you to strong-name your precompiled assemblies.</span></span>

> [!NOTE]
> <span data-ttu-id="838f4-320">ASP.NET'te 1.x, tanımlayıcı adlı derlemeler genel derleme önbelleği (GAC) içinde yüklü gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="838f4-320">In ASP.NET 1.x, strong-named assemblies had to be installed into the Global Assembly Cache (GAC).</span></span> <span data-ttu-id="838f4-321">ASP.NET 2. 0 ', katı adlı derlemeler GAC içine yüklemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="838f4-321">In ASP.NET 2.0, you are not required to install strong-named assemblies into the GAC.</span></span>


![Bir ASP.NET uygulamaları önceden derlenmiş dosyalar](improvements-in-visual-studio-2005/_static/image8.jpg)

<span data-ttu-id="838f4-323">**Şekil 11**: bir ASP.NET uygulamaları önceden derlenmiş dosyalar</span><span class="sxs-lookup"><span data-stu-id="838f4-323">**Figure 11**: An ASP.NET Applications Pre-Compiled Files</span></span>


> [!NOTE]
> <span data-ttu-id="838f4-324">Yukarıdaki uygulama web.config dosyası yoktu.</span><span class="sxs-lookup"><span data-stu-id="838f4-324">In the application above, there was no web.config file.</span></span> <span data-ttu-id="838f4-325">Geliştirilmişse, onu çağrılmış *PrecompiledApp.config* sonra yayımlama Web sitesi işlemi.</span><span class="sxs-lookup"><span data-stu-id="838f4-325">If there had been, it would have been called *PrecompiledApp.config* after the Publish Web site process.</span></span>


## <a name="improvements-in-deployment"></a><span data-ttu-id="838f4-326">Dağıtım geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="838f4-326">Improvements in Deployment</span></span>

<span data-ttu-id="838f4-327">Olarak bir kopyası proje özelliği Visual Studio 2005 Visual Studio 2002 ve 2003 ile sunar.</span><span class="sxs-lookup"><span data-stu-id="838f4-327">As with Visual Studio 2002 and 2003, Visual Studio 2005 offers a Copy Project feature.</span></span> <span data-ttu-id="838f4-328">Ancak, özellik Visual Studio 2005'te barındıracak ve artık Web sitesini kopyalama çağrılır.</span><span class="sxs-lookup"><span data-stu-id="838f4-328">However, the feature has been beefed up in Visual Studio 2005 and is now called Copy Web Site.</span></span>

<span data-ttu-id="838f4-329">Kopyalama Web sitesi iletişim kutusunda, sol çerçeve ve doğru bir çerçeve ayrılır.</span><span class="sxs-lookup"><span data-stu-id="838f4-329">The Copy Web Site dialog is split into a left frame and a right frame.</span></span> <span data-ttu-id="838f4-330">Sol çerçeve kaynak Web sitesinde ve uzak Web sitesinde doğru çerçeve olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="838f4-330">The left frame is called the Source Web Site and the right frame is called the Remote Web Site.</span></span> <span data-ttu-id="838f4-331">Bazı geliştiriciler karışıklığa neden olabilir bir şey doğru çerçevede gösterilen site mutlaka bir uzak site olmamasıdır.</span><span class="sxs-lookup"><span data-stu-id="838f4-331">One thing that may confuse some developers is that the site displayed in the right frame is not necessarily a remote site.</span></span> <span data-ttu-id="838f4-332">Yerel dosya sisteminde veya yerel IIS örneği üzerinde bir site olabilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-332">It could be a site on the local file system or on the local instance of IIS.</span></span> <span data-ttu-id="838f4-333">İletişim kutusu uzak Web sitesinden yayımlamanıza olanak tanır. Ayrıca, sol çerçevede gösterilen site mutlaka kaynak Web sitesinde olmadığından *için* kaynak Web sitesinde.</span><span class="sxs-lookup"><span data-stu-id="838f4-333">Additionally, the site displayed in the left frame is not necessarily the source Web site because the dialog allows you to publish from the remote Web site *to* the source Web site.</span></span>

<span data-ttu-id="838f4-334">Bu site, uzak bir Web sitesi için bir proje kopyalıyorsanız FrontPage Server Extensions'in yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="838f4-334">If you are copying a project to a remote Web site, that site must have the FrontPage Server Extensions installed on it.</span></span> <span data-ttu-id="838f4-335">Aksi takdirde, FTP kullanarak bağlanmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="838f4-335">If it does not, you will need to connect using FTP.</span></span> <span data-ttu-id="838f4-336">Öte yandan, bir proje için yerel IIS örneği kopyalanıyorsa, FrontPage Server Extensions gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="838f4-336">On the other hand, if you are copying a project to the local IIS instance, FrontPage Server Extensions are not required.</span></span>

> [!NOTE]
> <span data-ttu-id="838f4-337">Yerel IIS örneğinde yeni bir Web sitesi oluşturmak deneyin ve FrontPage 2002 Sunucu Uzantıları yüklenir, Web siteleri oluşturma bir SharePoint sunucusuna desteklenmediğini söyleyen bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="838f4-337">If you try to create a new Web site on the local IIS instance and the FrontPage 2002 Server Extensions are installed, you will get an error message telling you that creating Web sites is not supported on a SharePoint server.</span></span> <span data-ttu-id="838f4-338">Bu durumda, FrontPage 2000 Server Uzantıları yükleme veya FrontPage Server Extensions kaldırma seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="838f4-338">In that case, you have the option of installing the FrontPage 2000 Server Extensions or removing the FrontPage Server Extensions.</span></span>


<span data-ttu-id="838f4-339">Web sitesini kopyalama özelliğinin bir videosu için buraya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="838f4-339">Click here for a video walkthrough of the Copy Web Site feature.</span></span>


![](improvements-in-visual-studio-2005/_static/image4.png)


[<span data-ttu-id="838f4-340">Açık tam ekran görüntü</span><span class="sxs-lookup"><span data-stu-id="838f4-340">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a><span data-ttu-id="838f4-341">Hata ayıklama geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="838f4-341">Improvements in Debugging</span></span>

<span data-ttu-id="838f4-342">Visual Studio 2005'te hata ayıklama dört önemli geliştirmeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="838f4-342">There are four key improvements in debugging in Visual Studio 2005.</span></span>

- <span data-ttu-id="838f4-343">Yerel yönetici olmayan hata ayıklama, kullanıma hazır mümkündür.</span><span class="sxs-lookup"><span data-stu-id="838f4-343">Debugging locally as a non-administrator is possible out of the box.</span></span>
- <span data-ttu-id="838f4-344">Derleme ögesi hata ayıklama özelliği şimdi varsayılan olarak false değil.</span><span class="sxs-lookup"><span data-stu-id="838f4-344">The Debug attribute for the Compilation element is now false by default.</span></span>
- <span data-ttu-id="838f4-345">Uzaktan hata ayıklama kurulumu ve yapılandırması, önce daha kolay olur.</span><span class="sxs-lookup"><span data-stu-id="838f4-345">Remote debugging setup and configuration is easier than before.</span></span>
- <span data-ttu-id="838f4-346">Artık bir Web sitesi FTP konumu açılan ayıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-346">You can now debug a Web site opened via an FTP location.</span></span>

## <a name="debugging-as-a-non-administrator"></a><span data-ttu-id="838f4-347">Yönetici olmayan hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="838f4-347">Debugging as a Non-Administrator</span></span>

<span data-ttu-id="838f4-348">ASP.NET Geliştirme Sunucusu eklenmesi yönetici olmayanların kolayca çıktığı ASP.NET uygulamalarının hatalarını ayıklamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="838f4-348">The addition of the ASP.NET Development Server allows non-administrators to easily debug ASP.NET applications right out of the box.</span></span> <span data-ttu-id="838f4-349">Yerel dosya sistemi üzerinde çalışan bir ASP.NET uygulamanın hataları ayıklanırken, Visual Studio ASP.NET geliştirme sunucusu altında oturum açmış kullanıcının bağlamını başlatır.</span><span class="sxs-lookup"><span data-stu-id="838f4-349">When an ASP.NET application running on the local file system is debugged, Visual Studio launches the ASP.NET Development Server under the context of the logged-on user.</span></span> <span data-ttu-id="838f4-350">Bu kullanıcı ardından herhangi bir ek yapılandırma olmadan bu uygulamada hata ayıklaması yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-350">That user can then debug that application without any additional configuration.</span></span>

## <a name="debug-is-false-by-default"></a><span data-ttu-id="838f4-351">Hata ayıklama False olduğundan varsayılan olarak</span><span class="sxs-lookup"><span data-stu-id="838f4-351">Debug is False by Default</span></span>

<span data-ttu-id="838f4-352">ASP.NET'te 1.x, *hata ayıklama* özniteliğini *derleme* web.config dosyasının sonuna öğe ayarlanmıştır *true* varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="838f4-352">In ASP.NET 1.x, the *debug* attribute in the *compilation* element of the web.config file was set to *true* by default.</span></span> <span data-ttu-id="838f4-353">Geliştiriciler bu öznitelik ayarlanmış olduğunu her zaman öneriliyordu *false* üretim, bir uygulamayı dağıtmadan önce ancak çoğu Geliştirici hata ayıklama özniteliği bırakarak sonuçlarını tam olarak anlamadığınız TRUE, bunlar yalnızca olarak sol-olduğu.</span><span class="sxs-lookup"><span data-stu-id="838f4-353">It has always been recommended that developers set this attribute to *false* before deploying an application to production, but because most developers don't fully understand the consequences of leaving the debug attribute set to true, they simply left it as-is.</span></span>

<span data-ttu-id="838f4-354">En önemli bir sorun hata ayıklama özniteliğine sahip ile true ASP.NETs Toplu derleme modeli devre dışı olarak ayarlanması.</span><span class="sxs-lookup"><span data-stu-id="838f4-354">The most severe problem with having the debug attribute set to true is that it disables ASP.NETs batch compilation model.</span></span> <span data-ttu-id="838f4-355">Bu nedenle, her sayfaya ayrı bir DLL içine derlenir.</span><span class="sxs-lookup"><span data-stu-id="838f4-355">Therefore, each page is compiled into a separate DLL.</span></span> <span data-ttu-id="838f4-356">Anlamına gelir bir Web sayfaları (değil duyulmamış herhangi bir yolla olarak), binlerce uygulama oluşur, birkaç bin küçük DLL'leri o uygulama tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="838f4-356">If a Web application consists of thousands of pages (not unheard of by any means), that means several thousand small DLLs will be created by that application.</span></span> <span data-ttu-id="838f4-357">Bu DLL'leri boyutunda küçük olsa da, bunlar herhangi belirli bir konuma bellekteki yüklü değildir.</span><span class="sxs-lookup"><span data-stu-id="838f4-357">While these DLLs are small in size, they are not loaded into any particular location in memory.</span></span> <span data-ttu-id="838f4-358">Bu nedenle, sistem belleğini parçalanmasına neden ve OutOfMemoryException oluşum katkıda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="838f4-358">Therefore, they cause fragmentation in system memory and can contribute to OutOfMemoryException occurrences.</span></span>

<span data-ttu-id="838f4-359">ASP.NET 2. 0'da, hata ayıklama öznitelik varsayılan olarak false olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="838f4-359">In ASP.NET 2.0, the debug attribute is set to false by default.</span></span> <span data-ttu-id="838f4-360">Zaten, ne zaman bir geliştirici bir ASP.NET uygulamasını Visual Studio 2005 ' te hata ayıklamasına gördüğünüz gibi hata ayıklama etkin olan web.config dosyası eklemeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="838f4-360">As you have already seen, when a developer debugs an ASP.NET application in Visual Studio 2005, they are prompted to add a web.config file with debugging enabled.</span></span> <span data-ttu-id="838f4-361">Bunun yapılması, ASP.NET'te mevcut aynı dezavantajları doğurur 1.x, ama şimdi Geliştirici açıkça uyarılır uygulamayı üretim ortamına geçmeden önce özniteliği false değerine sıfırlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="838f4-361">Doing so incurs the same drawbacks that were present in ASP.NET 1.x, but now the developer is clearly warned that the attribute should be reset to false before moving the application to production.</span></span>

## <a name="remote-debugging-setup-and-configuration"></a><span data-ttu-id="838f4-362">Uzaktan hata ayıklama kurulumu ve yapılandırması</span><span class="sxs-lookup"><span data-stu-id="838f4-362">Remote Debugging Setup and Configuration</span></span>

<span data-ttu-id="838f4-363">Visual Studio 2002/2003'te, uzaktan hata ayıklama için Makine Hata Ayıklama Yöneticisi (mdm.exe) ve vs7jit.exe işlem yararlandı.</span><span class="sxs-lookup"><span data-stu-id="838f4-363">In Visual Studio 2002/2003, remote debugging relied on the Machine Debug Manager (mdm.exe) and the vs7jit.exe process.</span></span> <span data-ttu-id="838f4-364">Nedeniyle, uzaktan hata ayıklama sorunlarını giderme genellikle olan müşteriler için bir siyah kutu ve PSS için çok daha iyi değildir.</span><span class="sxs-lookup"><span data-stu-id="838f4-364">Because of that, troubleshooting remote debugging problems was often a black box for customers and it was often not much better for PSS.</span></span>

<span data-ttu-id="838f4-365">Visual Studio 2005 mdm.exe ve vs7jit.exe işlemleri güvenme kaldırır.</span><span class="sxs-lookup"><span data-stu-id="838f4-365">Visual Studio 2005 removes the reliance on the mdm.exe and vs7jit.exe processes.</span></span> <span data-ttu-id="838f4-366">Bunun yerine, artık uzaktan hata ayıklama İzleyicisi (msvsmon.exe) hizmeti kullanır</span><span class="sxs-lookup"><span data-stu-id="838f4-366">Instead, it now uses the Remote Debug Monitor service (msvsmon.exe.)</span></span>

<span data-ttu-id="838f4-367">Visual Studio 2005'te uzaktan hata ayıklama gereksinimi oldukça basittir.</span><span class="sxs-lookup"><span data-stu-id="838f4-367">The requirement for debugging in Visual Studio 2005 remotely is quite simple.</span></span> <span data-ttu-id="838f4-368">Hata ayıklama önce uzak sunucuda msvsmon.exe çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="838f4-368">You need to run msvsmon.exe on the remote server prior to debugging.</span></span> <span data-ttu-id="838f4-369">Visual Studio CD'den uzaktan hata ayıklama İzleyicisi'ni yükleyebilirsiniz veya hiçbir şey hiç Web sunucusunda yüklemeden msvsmon.exe bir paylaşımdan yalnızca çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-369">You can install the Remote Debug Monitor from the Visual Studio CD or you can simply run msvsmon.exe from a share without installing anything at all on the Web server.</span></span>

<span data-ttu-id="838f4-370">Msvsmon.exe çalıştırdığınızda, uzaktan hata ayıklama için engellenme bağlantı noktaları hakkında şikayet olasıdır.</span><span class="sxs-lookup"><span data-stu-id="838f4-370">When you run msvsmon.exe, it is likely that it will complain about ports being blocked for remote debugging.</span></span> <span data-ttu-id="838f4-371">Neyse ki, kolayca bağlantı noktalarından sağ Uyarısı iletişim kutusu içinde aşağıda gösterildiği gibi engelini kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-371">Fortunately, you can easily unblock the ports from right within the warning dialog as shown below.</span></span>


![Windows Güvenlik Duvarı uzaktan hata ayıklamayı engelleyen olduğunu söyleyen bir bildirim](improvements-in-visual-studio-2005/_static/image9.jpg)

<span data-ttu-id="838f4-373">**Şekil 12**: bildirim, Windows Güvenlik duvarı olup engelleme uzaktan hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="838f4-373">**Figure 12**: Notification that Windows Firewall is Blocking Remote Debugging</span></span>


<span data-ttu-id="838f4-374">Hata ayıklama için gerekli bağlantı noktalarını engeli kaldırılmış sonra aşağıda gösterildiği gibi uzaktan hata ayıklama İzleyicisi'ni göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-374">Once you have unblocked the ports necessary for debugging, you will see the Remote Debugging Monitor as shown below.</span></span> <span data-ttu-id="838f4-375">Bu arabirimden bağlantıları izleyebilir ve izinler bir kolayca hata ayıklama değiştirin.</span><span class="sxs-lookup"><span data-stu-id="838f4-375">From this interface, you can monitor connections and change debugging permissions easily.</span></span>


![Uzaktan hata ayıklama İzleyicisi](improvements-in-visual-studio-2005/_static/image10.jpg)

<span data-ttu-id="838f4-377">**Şekil 13**: uzaktan hata ayıklama İzleyicisi</span><span class="sxs-lookup"><span data-stu-id="838f4-377">**Figure 13**: The Remote Debugging Monitor</span></span>


<span data-ttu-id="838f4-378">FTP aracılığıyla açık bir Web uygulaması uzaktan hata ayıklamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="838f4-378">It is also possible to remotely debug a Web application opened via FTP.</span></span> <span data-ttu-id="838f4-379">Adımları bu daha önce ele aynıdır.</span><span class="sxs-lookup"><span data-stu-id="838f4-379">The steps are the same as those previously covered.</span></span> <span data-ttu-id="838f4-380">Ancak, bu modülün daha önce belirtildiği gibi FTP projeye göz atmak için bir temel URL'si belirtmeniz gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="838f4-380">However, you will need to specify a base URL for browsing the FTP project as outlined earlier in this module.</span></span>

## <a name="lab-2"></a><span data-ttu-id="838f4-381">Laboratuvar 2</span><span class="sxs-lookup"><span data-stu-id="838f4-381">Lab 2</span></span>

## <a name="remote-debugging-with-visual-studio-2005"></a><span data-ttu-id="838f4-382">Visual Studio 2005 ile uzaktan hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="838f4-382">Remote Debugging with Visual Studio 2005</span></span>

<span data-ttu-id="838f4-383">Bu Laboratuvar, Visual Studio 2005 ile uzaktan hata ayıklama aracılığıyla anlatılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="838f4-383">This lab will walk you through remote debugging with Visual Studio 2005.</span></span>

<span data-ttu-id="838f4-384">Bu laboratuvarı bir videosu için buraya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="838f4-384">Click here for a video walkthrough of this lab.</span></span>


![](improvements-in-visual-studio-2005/_static/image5.png)


[<span data-ttu-id="838f4-385">Açık tam ekran görüntü</span><span class="sxs-lookup"><span data-stu-id="838f4-385">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


<span data-ttu-id="838f4-386">Bu Laboratuvar çalışan Visual Studio 2005 ve diğer çalışan IIS 5 veya daha büyük olmak üzere iki makine olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="838f4-386">This lab requires you to have two machines, one running Visual Studio 2005 and the other running IIS 5 or greater.</span></span>

1. <span data-ttu-id="838f4-387">Visual Studio 2005'i açın ve uzak sunucuda yeni bir Web sitesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="838f4-387">Open Visual Studio 2005 and create a new Web site on the remote server.</span></span>

> [!NOTE]
> <span data-ttu-id="838f4-388">Uzak bir IIS örneği veya FTP üzerinden Web sitesi oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="838f4-388">You can create the Web site on a remote IIS instance or via FTP.</span></span>


1. <span data-ttu-id="838f4-389">Uzak Web sunucusundan bir UNC yolu kullanarak geliştirme makinede msvsmon.exe bulun ve yürütün.</span><span class="sxs-lookup"><span data-stu-id="838f4-389">From the remote Web server, locate msvsmon.exe on the development machine using a UNC path and execute it.</span></span>  
 <span data-ttu-id="838f4-390">//Server/c$/Program dosyaları/Microsoft Visual Studio 8/Common7/IDE/uzaktan hata ayıklayıcı/x86 msvsmon.exe için varsayılan konumdur.</span><span class="sxs-lookup"><span data-stu-id="838f4-390">The default location for msvsmon.exe is //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.</span></span>
2. <span data-ttu-id="838f4-391">Uzaktan hata ayıklama için bağlantı noktalarını engellemesini kaldırmak isteyip istemediğiniz sorulduğunda bunu yapın.</span><span class="sxs-lookup"><span data-stu-id="838f4-391">If prompted to unblock ports for remote debugging, do so.</span></span>
3. <span data-ttu-id="838f4-392">Geliştirme makinesinden Default.aspx için gerideki açın ve sayfa/_yük yöntemde bir kesme noktası ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="838f4-392">From the development machine, open the code-behind for Default.aspx and set a breakpoint in the Page/_Load method.</span></span>
4. <span data-ttu-id="838f4-393">Geliştirme makinesinden hatalarını ayıklamaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="838f4-393">Start debugging from the development machine.</span></span>

<span data-ttu-id="838f4-394">Beklenen şekilde kesme noktasına isabet.</span><span class="sxs-lookup"><span data-stu-id="838f4-394">You should hit the breakpoint as expected.</span></span>

## <a name="aspnet-development-server"></a><span data-ttu-id="838f4-395">ASP.NET Geliştirme Sunucusu</span><span class="sxs-lookup"><span data-stu-id="838f4-395">ASP.NET Development Server</span></span>

<span data-ttu-id="838f4-396">Zaten ele weve Visual Studio 2005 ASP.NET geliştirme sunucusu olarak adlandırılan bir Web sunucusu ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="838f4-396">As weve already discussed, Visual Studio 2005 ships with a Web server called the ASP.NET Development Server.</span></span> <span data-ttu-id="838f4-397">(ASP.NET Geliştirme Sunucusu bazen Cassini adlandırılır.) Bu Web sunucusu, göz atmak ve dosya sistemi üzerinde çalışan Web uygulamalarında hata ayıklama için kullanışlı bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="838f4-397">(The ASP.NET Development Server is sometimes referred to as Cassini.) This Web server is a convenient means to browse and debug Web applications running on the file system.</span></span>

<span data-ttu-id="838f4-398">ASP.NET Geliştirme Sunucusu kısıtlı bir Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="838f4-398">The ASP.NET Development Server is a restricted Web server.</span></span> <span data-ttu-id="838f4-399">Uzak bağlantılara izin vermediğinden, uygulamanın tüm istekleri Web sunucusu başlatan kullanıcının dışındaki herhangi bir kullanıcının izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="838f4-399">It does not allow remote connections, it does not allow any requests from any user other than the user who started the Web server.</span></span> <span data-ttu-id="838f4-400">ASP sayfalarını sunmadan yeteneği de yok.</span><span class="sxs-lookup"><span data-stu-id="838f4-400">It also does not have the capability of serving ASP pages.</span></span> <span data-ttu-id="838f4-401">Yalnızca ASP.NET ve HTML kaynaklarının (görüntüler, CSS dosyaları, vb. dahil) sunulur.</span><span class="sxs-lookup"><span data-stu-id="838f4-401">Only ASP.NET resources and HTML resources (including images, CSS files, etc.) are served.</span></span>

<span data-ttu-id="838f4-402">ASP.NET Geliştirme Sunucusu c:/Windows/Microsoft.NET/Framework/v2.0./ bulunan WebDev.WebServer.exe dosyasını çalıştırarak komut satırı üzerinden başlatılabilir*/* /  */*/\*.</span><span class="sxs-lookup"><span data-stu-id="838f4-402">The ASP.NET Development Server can be launched via the command line by running the WebDev.WebServer.exe file located at c:/Windows/Microsoft.NET/Framework/v2.0./*/*/*/*/\*.</span></span> <span data-ttu-id="838f4-403">Kullanılabilir parametreler aşağıdaki iletişim kutusunu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="838f4-403">The following dialog displays the parameters that are available.</span></span>


![](improvements-in-visual-studio-2005/_static/image11.jpg)

<span data-ttu-id="838f4-404">**Şekil 14**</span><span class="sxs-lookup"><span data-stu-id="838f4-404">**Figure 14**</span></span>


> [!NOTE]
> <span data-ttu-id="838f4-405">ASP.NET Geliştirme Sunucusu açıkça komut satırı üzerinden başlatıldığında desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="838f4-405">The ASP.NET Development Server is not supported when launched explicitly via the command line.</span></span>
