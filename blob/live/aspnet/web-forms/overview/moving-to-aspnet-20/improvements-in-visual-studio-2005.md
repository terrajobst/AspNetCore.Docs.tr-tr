---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: "Visual Studio 2005'te geliştirmeleri | Microsoft Docs"
author: microsoft
description: "Visual Studio 2005 yenilikleri ve geliştirmeleri Web projeleri için uzun bir listesi ile Web uygulaması geliştiricileri sağlar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 2c1f9a7291d8eab675bac3e1c37d6922131e3761
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="improvements-in-visual-studio-2005"></a><span data-ttu-id="1d7b9-103">Visual Studio 2005'te geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="1d7b9-103">Improvements in Visual Studio 2005</span></span>
====================
<span data-ttu-id="1d7b9-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1d7b9-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1d7b9-105">Visual Studio 2005 yenilikleri ve geliştirmeleri Web projeleri için uzun bir listesi ile Web uygulaması geliştiricileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-105">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span>


<span data-ttu-id="1d7b9-106">Visual Studio 2005 yenilikleri ve geliştirmeleri Web projeleri için uzun bir listesi ile Web uygulaması geliştiricileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-106">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span> <span data-ttu-id="1d7b9-107">Visual Studio .NET 2002 ve 2003 olarak güçlü olarak, vardı birçok şikayetlerinden Web projeleri işlenen biçiminde.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-107">As powerful as Visual Studio .NET 2002 and 2003 are, there were many complaints in the way that Web projects were handled.</span></span> <span data-ttu-id="1d7b9-108">Visual Studio 2005 bu şikayetlerinden değinmek için çok sayıda yeni özellikleri ekler.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-108">Visual Studio 2005 adds a significant number of new features in order to address these complaints.</span></span> <span data-ttu-id="1d7b9-109">Kişiler için Visual Studio .NET 2003 derleme Web uygulamalarının işlenme tercih ederseniz, bkz: [Web Uygulama projeleri](https://go.microsoft.com/fwlink/?LinkId=57870).</span><span class="sxs-lookup"><span data-stu-id="1d7b9-109">For those who prefer the way that Visual Studio .NET 2003 handled compilation of Web applications, see [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).</span></span>

<span data-ttu-id="1d7b9-110">Bu modül de Web projesi oluştururken, yönetim ve geliştirme geliştirmeler kapsar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-110">In this module, well cover improvements in Web project creation, management, and development.</span></span> <span data-ttu-id="1d7b9-111">Bir sonraki modüle Web projeleri oluşturmak ve bunları dağıtma iyileştirmeleri de kapsar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-111">In a later module, well cover improvements in building Web projects and deploying them.</span></span>

## <a name="frontpage-server-extensions"></a><span data-ttu-id="1d7b9-112">FrontPage Server Extensions</span><span class="sxs-lookup"><span data-stu-id="1d7b9-112">FrontPage Server Extensions</span></span>

<span data-ttu-id="1d7b9-113">Visual Studio .NET 2002 ve 2003 FrontPage Server Extensions kutusunda oluşturun veya Web projeleri oluşturmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-113">Visual Studio .NET 2002 and 2003 required FrontPage Server Extensions on the box in order to create or build Web projects.</span></span> <span data-ttu-id="1d7b9-114">Geliştiriciler iki farklı erişim modları (FrontPage Server Extensions veya dosya erişim modu) arasında bir seçim sahip, FrontPage Server Extensions hem de uygulama kökü ayarlama, IIS vb. gibi görevleri gerçekleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-114">Developers did have a choice between two different access modes (FrontPage Server Extensions or File access mode), both used FrontPage Server Extensions to perform tasks such as setting the application root in IIS, etc.</span></span>

<span data-ttu-id="1d7b9-115">Visual Studio 2005 FrontPage Server Extensions bağımlılık yerel projeleri için kaldırır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-115">Visual Studio 2005 removes the reliance on FrontPage Server Extensions for local projects.</span></span> <span data-ttu-id="1d7b9-116">Visual Studio 2005 şimdi IIS metatabanı FrontPage Server Extensions kullanmak yerine doğrudan erişir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-116">Visual Studio 2005 now accesses the IIS metabase directly instead of using the FrontPage Server Extensions.</span></span> <span data-ttu-id="1d7b9-117">Visual Studio 2005 ayrıca FrontPage Server Extensions gerek kalmadan uzak proje erişim sağlayan FTP için destek ekler.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-117">Visual Studio 2005 also adds support for FTP which allows for remote project access without requiring FrontPage Server Extensions.</span></span>

<span data-ttu-id="1d7b9-118">FrontPage Server Extensions kendi projelerine kullanmak istediğiniz geliştiriciler seçeneği kullanılabilir durumda kalır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-118">For those developers who want to use FrontPage Server Extensions in their projects, the option is still available.</span></span> <span data-ttu-id="1d7b9-119">Ancak, güçlü ASP.NET Geliştirici topluluğu görüşleri göre bu zorunlu değildir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-119">However, based upon strong feedback from the ASP.NET developer community, it is not a requirement.</span></span>

> [!NOTE]
> <span data-ttu-id="1d7b9-120">FrontPage Server Extensions uzak proje oluşturma, açma, vb. için hala gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-120">FrontPage Server Extensions are still required for remote project creation, opening, etc.</span></span>


## <a name="aspnet-development-server"></a><span data-ttu-id="1d7b9-121">ASP.NET Geliştirme Sunucusu</span><span class="sxs-lookup"><span data-stu-id="1d7b9-121">ASP.NET Development Server</span></span>

<span data-ttu-id="1d7b9-122">Visual Studio 2005 ASP.NET Geliştirme Sunucusu adı verilen yeni bir Web sunucusu ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-122">Visual Studio 2005 ships with a new Web server called ASP.NET Development Server.</span></span> <span data-ttu-id="1d7b9-123">(Bu Web sunucusu daha önce Cassini bilinir.)</span><span class="sxs-lookup"><span data-stu-id="1d7b9-123">(This Web server was previously known as Cassini.)</span></span>

<span data-ttu-id="1d7b9-124">ASP.NET Geliştirme Sunucusu çeşitli avantajları vardır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-124">There are several benefits of the ASP.NET Development Server.</span></span>

- <span data-ttu-id="1d7b9-125">Şimdi yönetici olmayanların geliştirmek ve bir Web sunucusuna karşı hata ayıklamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-125">It is now possible for non-Administrators to develop and debug against a Web server.</span></span>
- <span data-ttu-id="1d7b9-126">ASP.NET Geliştirme Sunucusu herhangi bir yere sanal dizinler için esnek proje konumlarını izin vererek dosya sistemindeki dinamik olarak eşler.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-126">The ASP.NET Development Server dynamically maps virtual directories to any location in the file system allowing for flexible project locations.</span></span>
- <span data-ttu-id="1d7b9-127">IIS zaten kullanan kullanıcılar Windows XP Professional şimdi dosya veya klasör yapısı, kendi varsayılan Web sitesi IIS'de etkilemez yeni Web uygulamaları oluşturmak mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-127">Users on Windows XP Professional who are already using IIS will now be able to create new Web applications that will not affect the file or folder structure of their Default Web Site in IIS.</span></span>

<span data-ttu-id="1d7b9-128">ASP.NET Geliştirme Sunucusu yararlanmak için özel yapılandırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-128">No special configuration is required to take advantage of the ASP.NET Development Server.</span></span> <span data-ttu-id="1d7b9-129">Dosya sistemi üzerinde barındırılan bir Web projesi hata ayıklaması veya taranan, Visual Studio 2005 ASP.NET Geliştirme Sunucusu örneği isteğe hizmet verecek bir rastgele bağlantı noktası otomatik olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-129">When a Web project that is hosted on the file system is debugged or browsed, Visual Studio 2005 will automatically start an instance of the ASP.NET Development Server on a random port to service the request.</span></span>

<span data-ttu-id="1d7b9-130">Daha fazla bilgi ASP.NET geliştirme sunucusu bu modüldeki daha sonra ele alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-130">More information will be covered on the ASP.NET Development Server later in this module.</span></span>

## <a name="improved-file-management"></a><span data-ttu-id="1d7b9-131">İyileştirilmiş dosya yönetimi</span><span class="sxs-lookup"><span data-stu-id="1d7b9-131">Improved File Management</span></span>

<span data-ttu-id="1d7b9-132">Visual Studio 2002 ve 2003'te bir proje dosyası (.vbproj VB.NET için) ve C# .csproj bilgi Web uygulamasındaki tüm dosyalarda depolanır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-132">In Visual Studio 2002 and 2003, a project file (.vbproj for VB.NET and .csproj for C#) stored information on all files in the Web application.</span></span> <span data-ttu-id="1d7b9-133">Çözüm Gezgini ekran proje dosyasında dosya bilgileri temel alır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-133">The Solution Explorer display is based upon the file information in the project file.</span></span> <span data-ttu-id="1d7b9-134">Bu nedenle, Çözüm Gezgini genellikle yanlış bilgi dış düzenleyicileri kullanıldığı durumlarda görüntüleyecektir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-134">Because of this, the Solution Explorer would often display inaccurate information in cases where external editors were used.</span></span> <span data-ttu-id="1d7b9-135">Visual Studio 2002 ve 2003 genellikle dosya değişiklikleri üzerine veya dosyaların en son sürümünü görüntülemeyecek.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-135">Visual Studio 2002 and 2003 would often overwrite file changes or not display the most recent version of files.</span></span>

<span data-ttu-id="1d7b9-136">Visual Studio 2005 hemen proje dosyasıyla birlikte yapar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-136">Visual Studio 2005 does away with the project file.</span></span> <span data-ttu-id="1d7b9-137">Bunun yerine, doğrudan projenizdeki dosyaların doğru bir görüntüde kaynaklanan diskten, dosya ve klasör bilgileri okur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-137">Instead, it reads the file and folder information directly from the disk, resulting in an accurate display of the files in your project.</span></span> <span data-ttu-id="1d7b9-138">Visual Studio 2002 ve 2003 başvuruları klasöründe, Web uygulamanızı gerçek bir klasörde göstermiyor olduğundan, Visual Studio 2005 başvurular klasörünü Çözüm Gezgini'nden de kaldırır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-138">Because the References folder in Visual Studio 2002 and 2003 does not represent an actual folder in your Web application, Visual Studio 2005 also removes the References folder from Solution Explorer.</span></span> <span data-ttu-id="1d7b9-139">Projeniz Visual Studio 2005'te başvurularını erişmek için proje için özellik sayfaları kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-139">To access the references for your project in Visual Studio 2005, you should use the Property pages for the project.</span></span>

## <a name="creating-web-projects"></a><span data-ttu-id="1d7b9-140">Web projeleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="1d7b9-140">Creating Web Projects</span></span>

<span data-ttu-id="1d7b9-141">Web geliştiricileri Visual Studio 2005'te proje oluşturmak için kullanılabilir birçok yeni seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-141">Web developers have many new options available for project creation in Visual Studio 2005.</span></span> <span data-ttu-id="1d7b9-142">Web siteleri artık herhangi bir yere dosya sistemini de oluşturulabilir ve sonra hata ayıklaması yapılabilir veya yeni ASP.NET geliştirme sunucusu kullanarak taranan.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-142">Web sites can now be created anywhere in the file system and can then be debugged or browsed using the new ASP.NET Development Server.</span></span> <span data-ttu-id="1d7b9-143">Geliştiriciler ayrıca FTP kullanarak yeni Web siteleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-143">Developers can also create new Web sites using FTP.</span></span>

<span data-ttu-id="1d7b9-144">Visual Studio 2005'te Web projeleri oluşturma videosu görüntülemek için burayı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-144">Click here to view a video walkthrough of creating Web projects in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image1.png)


[<span data-ttu-id="1d7b9-145">Açık Tam Ekran Video</span><span class="sxs-lookup"><span data-stu-id="1d7b9-145">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a><span data-ttu-id="1d7b9-146">Dosya sistemi projeleri</span><span class="sxs-lookup"><span data-stu-id="1d7b9-146">File System Projects</span></span>

<span data-ttu-id="1d7b9-147">Videosu içinde anlatıldığı gibi yerel makine üzerinde veya bir dosya paylaşımı üzerinden uzak bir konumdan dosya sisteminde Web siteleri oluşturmayı seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-147">As you saw in the video walkthrough, you can choose to create Web sites on the file system either on the local machine or on a remote location via a file share.</span></span> <span data-ttu-id="1d7b9-148">Dosya sisteminde oluşturan Web siteleri taranan ve ASP.NET geliştirme sunucusu kullanarak hata ayıklaması.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-148">Web sites that are created on the file system are browsed and debugged using the ASP.NET Development Server.</span></span>

> [!NOTE]
> <span data-ttu-id="1d7b9-149">ASP.NET Geliştirme Sunucusu müşteriler için bazı karışıklığa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-149">The ASP.NET Development Server may cause some confusion for customers.</span></span> <span data-ttu-id="1d7b9-150">IISs dizin yapısına (yani, c:\inetpub\wwwroot) dosya sisteminde bir Web projesi oluşturduysanız, Visual Studio 2005 içinde başlatıldığında ASP.NET geliştirme sunucusu aracılığıyla Web sitesi yine taranmasına.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-150">If a Web project is created on the file system in IISs directory structure (i.e. c:\inetpub\wwwroot), the Web site will still be browsed via the ASP.NET Development Server when launched from within Visual Studio 2005.</span></span> <span data-ttu-id="1d7b9-151">Bu nedenle, herhangi bir IIS yapılandırması (yani, kimlik doğrulama yöntemleri) geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-151">Therefore, any IIS configuration (i.e. authentication methods) is not applicable.</span></span>


<span data-ttu-id="1d7b9-152">Varsayılan web projesi de çok kaldırır yük tarafından yalnızca Default.aspx sayfasında, default.cs dosyası ve bir uygulama içeren\_veri klasörü.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-152">The default web project also removes a lot of the overhead by only includes a Default.aspx page, default.cs file, and an App\_Data folder.</span></span> <span data-ttu-id="1d7b9-153">Özel klasörler ve web.config (örn. uygulama\_kodu) gerektiği gibi eklenir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-153">The web.config and special folders (i.e. app\_code) are added as they are needed.</span></span> <span data-ttu-id="1d7b9-154">Web projenize yalnızca gereksinim duyduğunuz klasörleri ve dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-154">Your web project only includes the files and folders that you need.</span></span>

### <a name="http-projects"></a><span data-ttu-id="1d7b9-155">HTTP projeleri</span><span class="sxs-lookup"><span data-stu-id="1d7b9-155">HTTP Projects</span></span>

<span data-ttu-id="1d7b9-156">HTTP projeleri ya da yerel bir IIS Web sitesi veya uzak bir Web sitesinde oluşturulan projeleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-156">HTTP projects can either be projects that are created on a local IIS Web site or on a remote Web site.</span></span> <span data-ttu-id="1d7b9-157">Varsayılan proje konumu `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-157">The default project location is `http://localhost`.</span></span> <span data-ttu-id="1d7b9-158">Gözat düğmesine tıklayın, iki HTTP seçenek vardır: Yerel IIS ve uzak Site.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-158">If you click the Browse button, there are two HTTP options: Local IIS and Remote Site.</span></span> <span data-ttu-id="1d7b9-159">Bu iki seçenek ana fark web sitesi bilgilerini Choose Location iletişim ve dosyalar Web sunucusuna nasıl kopyalanır görüntülenir yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-159">The main difference in these two options is the method in which the web site information is displayed in the Choose Location dialog and in how the files are copied to the Web server.</span></span>

<span data-ttu-id="1d7b9-160">Yerel IIS seçeneği metatabanı yerel makinedeki site bilgilerini okur ve dosya sistemi kullanılarak dosyalar kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-160">The Local IIS option reads the site information from the metabase on the local machine and files are copied using the file system.</span></span> <span data-ttu-id="1d7b9-161">Uzak Site seçeneği FrontPage Server Extensions ve site bilgilerini kullanır, HTTP kullanarak dosyalar kopyalanır ve FrontPage Server Extensions RPC çağırır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-161">The Remote Site option uses the FrontPage Server Extensions and the site information and files are copied using HTTP and FrontPage Server Extensions RPC calls.</span></span>

> [!NOTE]
> <span data-ttu-id="1d7b9-162">Vs ###\_tmp.htm dosya ve get\_aspx\_ver.aspx artık sürüm bilgileri belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-162">The vs###\_tmp.htm file and get\_aspx\_ver.aspx are no longer used to determine version information.</span></span>


<span data-ttu-id="1d7b9-163">Varsayılan HTTP yerel IIS seçeneğidir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-163">The default HTTP option is Local IIS.</span></span> <span data-ttu-id="1d7b9-164">Bu seçenek, hangi siteleri kullanılabilir olduğunu belirlemek için IIS metatabanı ve içerik oluşturulacağı konum okur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-164">This option reads the IIS Metabase to determine which sites are available and the location in which to create content.</span></span> <span data-ttu-id="1d7b9-165">Ağaç görünümünde seçerek farklı bir klasör veya sanal dizin seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-165">You can select a different folder or virtual directory by selecting it in the tree view.</span></span> <span data-ttu-id="1d7b9-166">Ayrıca yeni bir sanal dizin oluşturma, klasörleri uygulamaları olarak işaretle yanı bu iletişim kutusundan mevcut sanal dizinleri silin.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-166">You can also create a new virtual directory, mark folders as applications, as well as delete existing virtual directories from this dialog box.</span></span>


![Konum iletişim seçin](improvements-in-visual-studio-2005/_static/image1.gif)

<span data-ttu-id="1d7b9-168">**Şekil 1**: konumu iletişim seçin</span><span class="sxs-lookup"><span data-stu-id="1d7b9-168">**Figure 1**: The Choose Location Dialog</span></span>


<span data-ttu-id="1d7b9-169">Farklı görsel, onay Studio'nun önceki sürümlerinde **kullanım Güvenli Yuva Katmanı** onay ve SSL sertifikası gözatma URL eşleşmiyor, varsa yaptığınız soran bir güvenlik uyarısı iletişim kutusu sunulur devam etmek istiyor.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-169">Unlike in earlier versions of Visual Studio, if you check the **Use Secure Sockets Layer** checkbox and the SSL certificate does not match the URL you are browsing, you will be presented with a Security Alert dialog asking you if you would like to proceed.</span></span> <span data-ttu-id="1d7b9-170">Sertifika eşleşen bir olmadıysa Visual Studio .NET 2003 kullanarak proje oluşturma başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-170">Using Visual Studio .NET 2003, if the certificate was not a matching one, creating the project would fail.</span></span>


![Güvenlik Uyarısı ilgili SSL sertifikası](improvements-in-visual-studio-2005/_static/image2.gif)

<span data-ttu-id="1d7b9-172">**Şekil 2**: SSL sertifikası ile ilgili güvenlik uyarısı</span><span class="sxs-lookup"><span data-stu-id="1d7b9-172">**Figure 2**: Security Alert Regarding SSL Certificate</span></span>


### <a name="note-on-host-headers"></a><span data-ttu-id="1d7b9-173">Ana bilgisayar üst bilgileri Not</span><span class="sxs-lookup"><span data-stu-id="1d7b9-173">Note on Host Headers</span></span>

<span data-ttu-id="1d7b9-174">Bir sitede belirli bir IP bağlı bir Web uygulaması oluşturuyorsanız, bir ana bilgisayar üstbilgisi yapılandırıldığından emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-174">If you are creating a Web application on a site bound to a specific IP, you will need to ensure that a host header is configured.</span></span> <span data-ttu-id="1d7b9-175">Aksi takdirde, Visual Studio sitede oluşturacak `http://localhost`, ancak IP adresi doğru olduğunda site göz atılamaz veya gelen ve IDE içinde hata ayıklaması çözümlemez.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-175">Otherwise, Visual Studio will create the site at `http://localhost`, but the IP address will not resolve correctly when the site is browsed or debugged from within the IDE.</span></span>

<span data-ttu-id="1d7b9-176">Uzak Site seçeneğini seçerseniz, yeni Web sitesi için hedef URL girin olanak tanımak için iletişim değiştirir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-176">If you select the Remote Site option, the dialog changes to allow you to enter the destination URL for the new Web site.</span></span> <span data-ttu-id="1d7b9-177">Bu URL FrontPage Server Extensions etkin olan bir sunucuda olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-177">This URL must be on a server that has the FrontPage Server Extensions enabled.</span></span> <span data-ttu-id="1d7b9-178">FrontPage Server Extensions kullanarak yerel Web sunucunuz ile çalışmak istiyorsanız, uzak Site seçeneğini kullanın ve yerel bir URL belirtin.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-178">If you want to work with your local Web server using the FrontPage Server Extensions, you can use the Remote Site option and specify a local URL.</span></span>


![Uzak bir sunucuda bir Web sitesi oluşturma](improvements-in-visual-studio-2005/_static/image1.jpg)

<span data-ttu-id="1d7b9-180">**Şekil 3**: Uzak bir sunucuda bir Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="1d7b9-180">**Figure 3**: Creating a Web Site on a Remote Server</span></span>


<span data-ttu-id="1d7b9-181">SSL sertifikası eşleşmiyorsa, SSL üzerinden uzak bir sitedeki bir uygulama oluştururken, onaylama iletişim kutusunda yerel IIS seçeneği kullanılırken görüntülenen iletişim biraz farklıdır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-181">When creating an application on a remote site via SSL, if the SSL certificate does not match, the confirmation dialog is slightly different than the dialog displayed when using the Local IIS option.</span></span>


![Uzak Site Güvenlik Uyarısı](improvements-in-visual-studio-2005/_static/image3.gif)

<span data-ttu-id="1d7b9-183">**Şekil 4**: Uzak Site Güvenlik Uyarısı</span><span class="sxs-lookup"><span data-stu-id="1d7b9-183">**Figure 4**: The Remote Site Security Alert</span></span>


<a id="_Toc116100243"></a>

#### <a name="ftp"></a><span data-ttu-id="1d7b9-184">FTP</span><span class="sxs-lookup"><span data-stu-id="1d7b9-184">FTP</span></span>

<span data-ttu-id="1d7b9-185">Visual Studio 2005 FTP üzerinden Web siteleri oluşturma seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-185">Visual Studio 2005 introduces the option to create Web sites via FTP.</span></span> <span data-ttu-id="1d7b9-186">Bu seçeneği kullandığınızda, IDE kullanıcıların geçici klasörde dosyaları yerel olarak oluşturur ve ardından dosyaları FTP konumuna taşımak için FTP kullanır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-186">When you use this option, the IDE creates the files locally in the users temp folder and then uses FTP to move the files to the FTP location.</span></span>

> [!NOTE]
> <span data-ttu-id="1d7b9-187">Geçici klasör konumu c:\Documents and ayarları olan\&lt; Kullanıcı&gt;\Local Settings\Temp\VWDWebCache\&lt; Sunucu&gt;\_&lt;uygulama adı&gt;</span><span class="sxs-lookup"><span data-stu-id="1d7b9-187">The temp folder location is c:\Documents and Settings\&lt;User&gt;\Local Settings\Temp\VWDWebCache\&lt;Server&gt;\_&lt;application name&gt;</span></span>


<span data-ttu-id="1d7b9-188">FTP seçeneğini kullanırken, bir konum seçin iletişim kutusu sunulur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-188">When using the FTP option, you will be presented with a Choose Location dialog.</span></span> <span data-ttu-id="1d7b9-189">Aşağıda gösterildiği gibi bu iletişim kutusunu içine FTP bağlantı bilgileri girin.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-189">You enter the required FTP connection information into this dialog as shown below.</span></span>


![Konum iletişim FTP seçin](improvements-in-visual-studio-2005/_static/image2.jpg)

<span data-ttu-id="1d7b9-191">**Şekil 5**: FTP konumu iletişim seçin</span><span class="sxs-lookup"><span data-stu-id="1d7b9-191">**Figure 5**: The Choose Location Dialog for FTP</span></span>


## <a name="lab-setup-ftp-site-and-create-a-project"></a><span data-ttu-id="1d7b9-192">Laboratuvar: Kurulum FTP sitesi ve bir proje oluşturun</span><span class="sxs-lookup"><span data-stu-id="1d7b9-192">Lab: Setup FTP site and create a project</span></span>

<span data-ttu-id="1d7b9-193">Bir kullanıcı yalnızca bunlar için FTP aracılığıyla karşıya yükleyebilir bir konuma sahip olması aşağıdaki adımları FTP sitesini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-193">The following steps configure the FTP site so that a user has a location that only they can upload to via FTP.</span></span>

### <a name="install-the-ftp-service"></a><span data-ttu-id="1d7b9-194">FTP hizmetini yükleme</span><span class="sxs-lookup"><span data-stu-id="1d7b9-194">Install the FTP Service</span></span>

1. <span data-ttu-id="1d7b9-195">Program Ekle veya Kaldır'ni açın, Windows Bileşenlerini Ekle/Kaldır</span><span class="sxs-lookup"><span data-stu-id="1d7b9-195">Open Add Remove Programs, select Add/Remove Windows Components</span></span>
2. <span data-ttu-id="1d7b9-196">Internet Information Services (Windows 2003'te uygulama sunucusu) seçin ve tıklatın **ayrıntıları**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-196">Select Internet Information Services (Application Server on Windows 2003) and click **Details**.</span></span>
3. <span data-ttu-id="1d7b9-197">Denetleme **Dosya Aktarım Protokolü (FTP) hizmeti** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-197">Check **File Transfer Protocol (FTP) Service** and click **OK**.</span></span>
4. <span data-ttu-id="1d7b9-198">Tıklatın **sonraki** FTP hizmetini yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-198">Click **Next** to install the FTP service.</span></span>

### <a name="create-a-new-folder-for-content"></a><span data-ttu-id="1d7b9-199">İçerik için yeni bir klasör oluşturun</span><span class="sxs-lookup"><span data-stu-id="1d7b9-199">Create a New Folder for Content</span></span>

1. <span data-ttu-id="1d7b9-200">Windows Gezgini'nde adlı yeni bir klasör oluşturun **kullanıcı1** c:\inetpub\wwwroot içinde.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-200">In Windows Explorer, create a new folder called **User1** inside of c:\inetpub\wwwroot.</span></span>

#### <a name="configure-folders-and-permissions-on-folders"></a><span data-ttu-id="1d7b9-201">Klasörleri ve izinlerini klasörlerde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-201">Configure folders and permissions on folders.</span></span>

1. <span data-ttu-id="1d7b9-202">Internet Information Services eklentisini Yönetim Araçları'ndan açın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-202">Open the Internet Information Services snap-in from Administrative Tools.</span></span> <span data-ttu-id="1d7b9-203">Bilgisayar adı düğümü altında bir FTP siteleri klasörünü artık bulunacaktır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-203">You will now have an FTP Sites folder under the computer name node.</span></span>
2. <span data-ttu-id="1d7b9-204">Genişletme **FTP siteleri**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-204">Expand **FTP Sites**.</span></span>
3. <span data-ttu-id="1d7b9-205">Sağ **varsayılan FTP sitesi**seçin **yeni**, ardından **sanal dizin**, ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-205">Right-click the **Default FTP Site**, select **New**, then **Virtual Directory**, then click **Next**.</span></span>
4. <span data-ttu-id="1d7b9-206">Girin **kullanıcı1** tıklatın ve sanal dizin adı için **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-206">Enter **User1** for the virtual directory name and click **Next**.</span></span>
5. <span data-ttu-id="1d7b9-207">Girin **c:\inetpub\wwwroot\User1** tıklayın ve yolu için **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-207">Enter **c:\inetpub\wwwroot\User1** for the path and click **Next**.</span></span>
6. <span data-ttu-id="1d7b9-208">Tıklatın **sonraki** ve ardından **son** Sihirbazı tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-208">Click **Next** and then **Finish** to complete the wizard.</span></span>
7. <span data-ttu-id="1d7b9-209">Sağ **kullanıcı1** varsayılan FTP sitesi ve select altında sanal dizin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-209">Right-click the **User1** virtual directory under Default FTP Site and select **Properties**.</span></span>
8. <span data-ttu-id="1d7b9-210">Denetleme **yazma** onay kutusunu tıklatıp **Tamam** iletişim kutusunu kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-210">Check the **Write** checkbox and click **OK** to close the dialog.</span></span>
9. <span data-ttu-id="1d7b9-211">Sağ **varsayılan FTP sitesi** seçip **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-211">Right-click **Default FTP Site** and select **Properties**.</span></span>
10. <span data-ttu-id="1d7b9-212">Üzerinde **güvenlik hesapları** sekmesinde, işaretini **anonim bağlantılara izin ver**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-212">On the **Security Accounts** tab, uncheck **Allow Anonymous Connections**.</span></span>
11. <span data-ttu-id="1d7b9-213">Tıklatın **Evet** devam etmek isteyip istemediğinizi soran iletişim.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-213">Click **Yes** in the dialog asking if you want to continue.</span></span>
12. <span data-ttu-id="1d7b9-214">Tıklatın **Tamam** iletişim kutusunu kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-214">Click **OK** to close the dialog.</span></span>
13. <span data-ttu-id="1d7b9-215">Genişletme **varsayılan Web sitesi** altında **Web siteleri** düğümü.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-215">Expand the **Default Web Site** under the **Web Sites** node.</span></span>
14. <span data-ttu-id="1d7b9-216">Sağ **kullanıcı1** seçin ve dizin **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="1d7b9-216">Right-click the **User1** directory and select **Properties**</span></span>
15. <span data-ttu-id="1d7b9-217">İçinde **uygulama ayarları** 'yi tıklatın **oluşturma** klasörü bir uygulama olarak işaretlenecek.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-217">In the **Application Settings** section, click **Create** to mark the folder as an application.</span></span>
16. <span data-ttu-id="1d7b9-218">Tıklatın **Tamam** iletişim kutusunu kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-218">Click **OK** to close the dialog.</span></span>
17. <span data-ttu-id="1d7b9-219">Internet Information Services eklentisini kapatın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-219">Close the Internet Information Services snap-in.</span></span>

### <a name="create-web-project"></a><span data-ttu-id="1d7b9-220">Web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="1d7b9-220">Create web project</span></span>

1. <span data-ttu-id="1d7b9-221">Visual Studio 2005 açın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-221">Open Visual Studio 2005.</span></span>
2. <span data-ttu-id="1d7b9-222">Gelen **dosya** menüsünde, select **yeni Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-222">From the **File** menu, select **New Web Site**.</span></span>
3. <span data-ttu-id="1d7b9-223">İçinde **konumu** açılan listesinde, select **FTP**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-223">In the **Location** dropdown, select **FTP**.</span></span>
4. <span data-ttu-id="1d7b9-224">**Gözat**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-224">Click **Browse**.</span></span>
5. <span data-ttu-id="1d7b9-225">Girin **localhost** içinde **Server** metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-225">Enter **localhost** in the **Server** textbox.</span></span>
6. <span data-ttu-id="1d7b9-226">Girin **kullanıcı1** dizin metin kutusuna.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-226">Enter **User1** in the Directory textbox.</span></span>
7. <span data-ttu-id="1d7b9-227">Tıklatın **açık**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-227">Click **Open**.</span></span> <span data-ttu-id="1d7b9-228">Yeni Web sitesi iletişim kutusuna FTP konumunu girilecektir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-228">The FTP location will be entered into the New Web Site dialog.</span></span>
8. <span data-ttu-id="1d7b9-229">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-229">Click **OK**.</span></span>
9. <span data-ttu-id="1d7b9-230">İşaretini **anonim oturum** FTP oturum açma iletişim kutusunda, kimlik bilgilerinizi girin ve **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-230">Uncheck **Anonymous log on** in the FTP Log On dialog, enter your credentials, and click **OK**.</span></span>
10. <span data-ttu-id="1d7b9-231">Proje için URL nedir?</span><span class="sxs-lookup"><span data-stu-id="1d7b9-231">What is the URL for the project?</span></span> <span data-ttu-id="1d7b9-232">(URL projesi için Çözüm Gezgini'nde görüntülenir.)</span><span class="sxs-lookup"><span data-stu-id="1d7b9-232">(The URL for the project will be displayed in Solution Explorer.)</span></span>
11. <span data-ttu-id="1d7b9-233">Gelen **yapı** menüsünde, select **yapı Web sitesi** veya **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-233">From the **Build** menu, select **Build Web Site** or **Build Solution**.</span></span>
12. <span data-ttu-id="1d7b9-234">Çözüm Gezgini'nde Default.aspx sağ tıklatın ve seçin **tarayıcıda görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-234">Right-click on Default.aspx in Solution Explorer and select **View in Browser**.</span></span>
13. <span data-ttu-id="1d7b9-235">Web sitesi URL'si gerekli iletişim kutusuna girin `http://localhost/user1` tıklatın ve URL için **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-235">In the Web Site URL Required dialog, enter `http://localhost/user1` for the URL and click **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="1d7b9-236">Sorunu yük türü belirten bir hata alırsanız \_varsayılan, ASP.NET 2.0 Web sitenizi ve önceki bir sürümünü çalıştırdığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-236">If you get a error indicating an inability to load the type \_Default, make sure that you are running ASP.NET 2.0 on your Web site and not an earlier version.</span></span> <span data-ttu-id="1d7b9-237">Internet Information Services'ın ASP.NET sekmesinden, yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-237">You can do that from the ASP.NET tab in Internet Information Services.</span></span>


## <a name="opening-web-projects"></a><span data-ttu-id="1d7b9-238">Açılış Web projeleri</span><span class="sxs-lookup"><span data-stu-id="1d7b9-238">Opening Web Projects</span></span>

<span data-ttu-id="1d7b9-239">Web projeleri açma projeleri oluşturmaya benzer.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-239">Opening Web projects is similar to creating projects.</span></span> <span data-ttu-id="1d7b9-240">Aşağıdaki bölümlerde bir çıkışı için IDE içinde çalışırken takip etmek alanları çıkışı çağırın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-240">The following sections call out areas to keep an eye out for while working within the IDE.</span></span> <span data-ttu-id="1d7b9-241">HTTP ve FTP kullanarak Web projeleri ile çalışma ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-241">It also covers working with Web projects using HTTP and FTP.</span></span>

<span data-ttu-id="1d7b9-242">Bir Web projesi açmak için Dosya menüsünden açmak Web sitesini seçin.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-242">To open a Web project, select Open Web Site from the File menu.</span></span> <span data-ttu-id="1d7b9-243">Daha önce ele alınan aynı konumu seçin iletişim kutusu ile istenir ve kullanabileceğiniz aynı dört seçeneğiniz vardır: dosya sistemi, yerel IIS, FTP ve uzak Site.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-243">You will be prompted with the same Choose Location dialog covered previously and you have the same four options available to you: File System, Local IIS, FTP, and Remote Site.</span></span>

<a id="_Toc116100245"></a>

## <a name="file-system"></a><span data-ttu-id="1d7b9-244">Dosya sistemi</span><span class="sxs-lookup"><span data-stu-id="1d7b9-244">File System</span></span>

<span data-ttu-id="1d7b9-245">Bu modülde daha önce belirtildiği gibi Visual Studio artık bir proje dosyası kullanır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-245">As indicated previously in this module, Visual Studio no longer uses a project file.</span></span> <span data-ttu-id="1d7b9-246">Dosya sisteminden bir Web sitesini açmak seçerseniz, bu nedenle, aslında seçtiğiniz klasör başlangıçta Visual Studio'da bir Web projesi olarak oluşturulmamış olsa bile, istediğiniz herhangi bir klasör seçme seçeneğiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-246">Therefore, if you choose to open a Web site from the file system, you actually have the option of choosing any folder that you wish, even if the folder you choose was not created as a Web project initially in Visual Studio.</span></span> <span data-ttu-id="1d7b9-247">Örneğin, bir Web sitesi olarak Belgelerim klasörünü açmak seçebilirsiniz ve Visual Studio sonsuza dek açmak ve aşağıda gösterildiği gibi dosyalarınızı görüntülemek.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-247">For example, you can choose to open the My Documents folder as a Web site and Visual Studio will happily open it and display your files as shown below.</span></span>


![Bir Web sitesi olarak açılmış Belgelerim](improvements-in-visual-studio-2005/_static/image3.jpg)

<span data-ttu-id="1d7b9-249">**Şekil 6**: *Belgelerim* bir Web sitesi olarak açılmış</span><span class="sxs-lookup"><span data-stu-id="1d7b9-249">**Figure 6**: *My Documents* Opened As a Web Site</span></span>


<span data-ttu-id="1d7b9-250">Visual Studio, yalnızca ek dosyalar ve klasörler gerektiğinde oluşturduğundan, hiçbir ek dosya veya klasör açtığınız konuma eklenir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-250">Because Visual Studio only creates additional files and folders when necessary, no additional files or folders are added to the location you open.</span></span> <span data-ttu-id="1d7b9-251">Bu mimarinin bir yan etkisi, bu, Web siteleri dosya sisteminde iç içe engellediğini ' dir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-251">A side-effect of this architecture is that it prevents you from nesting Web sites on the file system.</span></span> <span data-ttu-id="1d7b9-252">Örneğin, aşağıdaki dizin yapısını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-252">For example, consider the following directory structure.</span></span>

<span data-ttu-id="1d7b9-253">Web projesi C:\MyWebSite adresindeki</span><span class="sxs-lookup"><span data-stu-id="1d7b9-253">Web project at C:\MyWebSite</span></span>

<span data-ttu-id="1d7b9-254">C:\MyWebSite\Nested konumundaki başka bir web projesi</span><span class="sxs-lookup"><span data-stu-id="1d7b9-254">Another web project at C:\MyWebSite\Nested</span></span>

<span data-ttu-id="1d7b9-255">C:\MyWebSite Web sitesinde açtığınızda, iç içe klasör uygulamasının bir alt klasör olarak görünür.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-255">When you open the Web site at c:\MyWebSite, the Nested folder will appear as a sub-folder of that application.</span></span>

<a id="_Toc116100246"></a>

## <a name="http"></a><span data-ttu-id="1d7b9-256">HTTP</span><span class="sxs-lookup"><span data-stu-id="1d7b9-256">HTTP</span></span>

<span data-ttu-id="1d7b9-257">Web siteleri HTTP üzerinden açarken ayarları (yerel IIS) IIS metatabanı veya FrontPage Server Extensions (Uzak Site.) kullanarak okunur İç içe geçmiş web uygulaması varsa, bunlar da bir uygulama olarak tanımlayan bir simgeyle görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-257">When opening Web sites via HTTP, settings are read either from the IIS metabase (Local IIS) or using FrontPage Server Extensions (Remote Site.) If there are nested web applications, these are displayed as well with an icon identifying them as an application.</span></span> <span data-ttu-id="1d7b9-258">Visual Studio 2005 davranış FrontPage web uygulamaları ile çalışma ile bilginiz varsa, benzer.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-258">If you are familiar with working with web applications in FrontPage, the behavior in Visual Studio 2005 is similar.</span></span>

<span data-ttu-id="1d7b9-259">Visual Studio IDE içinde şu anda açıldığında uygulama altındaki iç içe uygulamalar için bir simge görüntülenir olsa bile, onu içeriklerini bakmalarını genişletmenize izin vermez.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-259">Even though Visual Studio will display an icon for applications that are nested beneath the application that is currently opened within the IDE, it will not allow you to expand them to see their content.</span></span> <span data-ttu-id="1d7b9-260">Ancak, bunlar üzerinde bunları açmak için çift tıklatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-260">You can, however, double-click on them to open them.</span></span> <span data-ttu-id="1d7b9-261">Bunu yaptığınızda, ya da web uygulaması açın (ve şu anda açık olan çözüm değiştirin) isteyen bir iletişim kutusu sunulur veya geçerli çözümünüz için Web uygulaması ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-261">When you do, you will be presented with a dialog prompting you to either open the web application (and replace the currently open solution) or add the Web application to your current solution.</span></span>


![İç içe geçmiş uygulama simgesini çift tıklatarak bu iletişim kutusunda gösterir](improvements-in-visual-studio-2005/_static/image4.jpg)

<span data-ttu-id="1d7b9-263">**Şekil 7**: iç içe geçmiş uygulama simgesini çift gösterir, bu iletişim kutusunda</span><span class="sxs-lookup"><span data-stu-id="1d7b9-263">**Figure 7**: Double-clicking a nested application icon presents you with this dialog</span></span>


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a><span data-ttu-id="1d7b9-264">FTP sitesi</span><span class="sxs-lookup"><span data-stu-id="1d7b9-264">FTP Site</span></span>

<span data-ttu-id="1d7b9-265">Bir siteyi FTP aracılığıyla açtığınızda, dosyalar tüm yerel geçici klasörünüze kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-265">When you open a site via FTP, the files are all copied locally to your temp folder.</span></span> <span data-ttu-id="1d7b9-266">Yerel depolama konumunun tam yolunu projesi için Özellikler bölmesinde görüntülenir ve aşağıdaki biçimi kullanarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-266">The full path for the local storage location is displayed in the Properties pane for the project and is created using the following format.</span></span>

<span data-ttu-id="1d7b9-267">C:\Documents and Settings\&lt; Kullanıcı&gt;\Local Settings\Temp\VWDWebCache\&lt; Sunucu&gt;\_&lt;uygulama adı&gt;</span><span class="sxs-lookup"><span data-stu-id="1d7b9-267">C:\Documents and Settings\&lt;User&gt;\Local Settings\Temp\VWDWebCache\&lt;Server&gt;\_&lt;application name&gt;</span></span>

<span data-ttu-id="1d7b9-268">FTP kullanırken, Visual Studio aşağıda gösterildiği gibi gözatabilirsiniz projeniz için temel URL belirtmeniz gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-268">When using FTP, Visual Studio will need to specify the base URL for your project so that you can browse it as shown below.</span></span> <span data-ttu-id="1d7b9-269">Temel bir URL belirtmezseniz, Visual Studio için Web sitesindeki bir sayfasına göz girişimi ilk kez istenir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-269">If you do not specify a base URL, Visual Studio will ask you for it the first time you attempt to browse a page in the Web site.</span></span>


![FTP siteleri için temel URL belirtme](improvements-in-visual-studio-2005/_static/image5.jpg)

<span data-ttu-id="1d7b9-271">**Şekil 8**: FTP siteleri için temel URL belirtme</span><span class="sxs-lookup"><span data-stu-id="1d7b9-271">**Figure 8**: Specifying a Base URL for FTP Sites</span></span>


## <a name="improvements-in-compilation"></a><span data-ttu-id="1d7b9-272">Derleme geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="1d7b9-272">Improvements in Compilation</span></span>

<span data-ttu-id="1d7b9-273">Visual Studio 2005'te Web uygulamaları ile çalışma önemli ölçüde önceki sürümlerden daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-273">Working with Web applications in Visual Studio 2005 is noticeably faster than previous versions.</span></span> <span data-ttu-id="1d7b9-274">Bu derleme mimarisi yapılan değişiklikler nedeniyle küçük bir parçası olur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-274">This is due in no small part to the changes in compilation architecture.</span></span>

<span data-ttu-id="1d7b9-275">Visual Studio 2002 ve 2003, Web uygulamaları / bin'in klasöründe bulunan bir birincil derlemesini derlendi.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-275">In Visual Studio 2002 and 2003, Web applications were compiled into one primary assembly residing in the /bin folder.</span></span> <span data-ttu-id="1d7b9-276">Visual Studio 2005, bir uygulama\_kod klasörü eklendi.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-276">In Visual Studio 2005, an App\_Code folder was added.</span></span> <span data-ttu-id="1d7b9-277">Sınıfları ve başka bir kullanıcı Arabirimi olmayan kod uygulamaya eklenir\_kod klasör.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-277">Classes and other non-UI code are added to the App\_Code folder.</span></span> <span data-ttu-id="1d7b9-278">Visual Studio projesi, uygulamadaki tüm dosyaları ne zaman derlemeler\_kod klasörü tek bir uygulamaya derlenmiş\_Code.dll dosya.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-278">When Visual Studio builds the project, all files in the App\_Code folder are compiled into a single App\_Code.dll file.</span></span> <span data-ttu-id="1d7b9-279">Bu değişikliğin sonucu sonraki derlemeleri önceki sürümlerde çok daha hızlı olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-279">The result of this change is that subsequent builds are much faster than in previous versions.</span></span>

> [!NOTE]
> <span data-ttu-id="1d7b9-280">MSBuild komut satırı yardımcı programı, ASP.NET Web uygulamaları geliştirmek için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-280">The MSBuild command line utility can also be used to build ASP.NET Web applications.</span></span> <span data-ttu-id="1d7b9-281">Bu aracı Modülü 9 ele alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-281">That tool will be covered in module 9.</span></span>


<span data-ttu-id="1d7b9-282">Başka bir derleme geliştirme, derleme menüsünde yeni yapı sayfasını seçenektir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-282">Another compilation enhancement is the new Build Page option on the Build menu.</span></span> <span data-ttu-id="1d7b9-283">Bu özellik, böylece değişiklikleri daha hızlı derlenebilir yalnızca geçerli sayfa (ile birlikte, indirmelere ve bağımlılıkları) yeniden oluşturmak bir geliştirici sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-283">This feature allows a developer to rebuild only the current page (along with, of course, and dependencies) so that changes can be compiled more quickly.</span></span> <span data-ttu-id="1d7b9-284">C# arka plan derleme güncelleştirme IntelliSense, vb. amacıyla sağlamadığından, yalnızca tek bir sayfayla yeniden oluşturma hızlı bir şekilde güncelleştirilmesi IntelliSense için izin verdiğinden bunlar çok bu özelliğinden yararlanabilir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-284">Because C# does not offer background compilation for purposes of updating IntelliSense, etc., they will benefit immensely from this feature because it will allow for IntelliSense to be updated quickly by simply rebuilding a single page.</span></span>

<span data-ttu-id="1d7b9-285">Proje derleme özellikleri başlangıç sayfasını yürütülmeden önce oluşan yapı türü yapılandırmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-285">The Build properties for a project allow you to configure the type of build that occurs before the startup page is executed.</span></span> <span data-ttu-id="1d7b9-286">Geliştiriciler Visual Studio kod değişikliklerinden sonra daha hızlı uygulamalarında hata ayıklama başlayabilmeniz için yalnızca geçerli sayfayı oluşturmak seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-286">Developers can choose to only build the current page so that Visual Studio can start debugging applications more quickly after code changes.</span></span>


![Derleme sayfası başlangıç eylemi](improvements-in-visual-studio-2005/_static/image6.jpg)

<span data-ttu-id="1d7b9-288">**Şekil 9**: derleme sayfa başlangıç eylemi</span><span class="sxs-lookup"><span data-stu-id="1d7b9-288">**Figure 9**: The Build Page Start Action</span></span>


<span data-ttu-id="1d7b9-289">Visual Studio ve ASP.NET mimarisi için başka bir önemli geliştirme düzenleme alanındadır ve devam edin.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-289">Another great enhancement to Visual Studio and the ASP.NET architecture is in the area of edit and continue.</span></span> <span data-ttu-id="1d7b9-290">Geliştiriciler Visual Studio 2005'te Proje hata ayıklamayı Başlat ve hata ayıklayıcı ayırma olmadan projede kod değişiklikleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-290">In Visual Studio 2005, developers can start debugging a project and make code changes on the project without detaching the debugger.</span></span> <span data-ttu-id="1d7b9-291">Aslında, bir proje hata ayıklama tam anlamıyla başlatmak için yeni bir sınıf ekleyin, o sınıfın kodu ekleyin, kod sayfanıza o sınıfın yeni bir örneğini oluşturur ekleyin ve sınıfın tüm hata ayıklayıcı ayırma olmadan bir yöntem yürütülmeye.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-291">In fact, you can literally start debugging a project, add a new class, add code to that class, add code to your page that creates a new instance of that class and execute a method of the class, all without detaching the debugger.</span></span> <span data-ttu-id="1d7b9-292">Yeni kod yürütmek tarayıcıyı yenilemeyi olarak tam anlamıyla kadar kolaydır!</span><span class="sxs-lookup"><span data-stu-id="1d7b9-292">Executing the new code is literally as easy as refreshing the browser!</span></span>

<span data-ttu-id="1d7b9-293">Videosu düzenleme görmek ve Visual Studio 2005'te özellik devam etmek için burayı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-293">Click here to see a video walkthrough of the edit and continue feature in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image2.png)


[<span data-ttu-id="1d7b9-294">Açık Tam Ekran Video</span><span class="sxs-lookup"><span data-stu-id="1d7b9-294">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


<span data-ttu-id="1d7b9-295">Sağlam düzenleyin ve işlevsellik ASP.NET 2.0 ile devam etmek ve Visual Studio 2005 ASP.NET uygulamaları için Mimari değişikliği nedeniyle.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-295">The robust edit and continue functionality in ASP.NET 2.0 and Visual Studio 2005 is due to an architectural change for ASP.NET applications.</span></span> <span data-ttu-id="1d7b9-296">ASP.NET 1.x, Visual Studio 2002/2003'te oluşturulan uygulamaların / bin'in klasöründe depolanan birincil bir bütünleştirilmiş koda derlenmiş.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-296">In ASP.NET 1.x, applications created in Visual Studio 2002/2003 were compiled into a primary assembly that was stored in the /bin folder.</span></span> <span data-ttu-id="1d7b9-297">Tüm sınıflar, sayfalar, vb. uygulama derlenmiş için bu bir DLL'e.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-297">All classes, pages, etc. for the application were compiled into that one DLL.</span></span> <span data-ttu-id="1d7b9-298">Ardından çalışma zamanında ASP.NET tüm denetimler, biçimlendirme ve ASP.NET kodu sayfaları içinde derleyin ve bu DLL'ler ASP.NET geçici klasöre kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-298">Then at runtime, ASP.NET would compile all of the controls, markup, and ASP.NET code within pages and copy those DLLs into the ASP.NET temporary folder.</span></span>

<span data-ttu-id="1d7b9-299">ASP.NET 2. 0'da, çalışma zamanında (Visual Studio için bir tane) ve bir ASP.NET için yukarıdaki iki derleme modelleri anahat kullanarak Visual Studio 2005'te bir ortak derleme modeline birleştirildi.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-299">In Visual Studio 2005 using ASP.NET 2.0, the two compilation models outline above (one for Visual Studio and one for ASP.NET at runtime) have been merged into one common compilation model.</span></span> <span data-ttu-id="1d7b9-300">Tüm derleme sorunları şimdi yerine geliştirme aşaması sırasında çalışma zamanında yakalanan olduğunu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-300">That means that all compilation issues are now caught during the development stage instead of at runtime.</span></span> <span data-ttu-id="1d7b9-301">Ayrıca Tasarımcısı ve kullanıcı denetimleri ve ana sayfalar gibi özellikler için IntelliSense desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-301">It also allows for designer and IntelliSense support for features such as user controls and master pages.</span></span>

<span data-ttu-id="1d7b9-302">Videosu kullanıcı denetimleri için tasarımcı desteği görmek için burayı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-302">Click here to see a video walkthrough of designer support for user controls.</span></span>


![](improvements-in-visual-studio-2005/_static/image3.png)


[<span data-ttu-id="1d7b9-303">Açık Tam Ekran Video</span><span class="sxs-lookup"><span data-stu-id="1d7b9-303">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> <span data-ttu-id="1d7b9-304">Bir kullanıcı denetimi bir sayfadan kaldırıldığında @Register yönergesi biçimlendirme içinde kalır ve kullanıcı denetimini Web sitesinden silinirse ayrıştırıcı hataları önlemek için el ile kaldırılması.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-304">When a user control is removed from a page, the @Register directive remains in the markup and should be removed manually in order to avoid parser errors if the user control is deleted from the Web site.</span></span>


<span data-ttu-id="1d7b9-305">Visual Studio derleme modelinde başka bir geliştirme Web sitesi yayımlama özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-305">Another improvement in the Visual Studio compilation model is the Publish Web Site feature.</span></span> <span data-ttu-id="1d7b9-306">Web sitesi Yayımlama özelliği işlemini gerçekleştirir olduğundan, geliştiricilerin isteğe bağlı herhangi bir şey derlemek olmamasından eklenen performansı keyfini çıkarabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-306">Because the Publish feature precompiles the Web site, developers can enjoy the added performance of not having to compile anything on demand.</span></span> <span data-ttu-id="1d7b9-307">Ayrıca uygulamanın tüm kaynak kodunda işlemini gerçekleştirir\_dağıtılacak hiçbir kaynak koduna sahip olması DLL'e kod klasör.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-307">It also precompiles all source code in the App\_Code folder into a DLL so that no source code has to be deployed.</span></span>


![Yayımla Web sitesi iletişim kutusu](improvements-in-visual-studio-2005/_static/image7.jpg)

<span data-ttu-id="1d7b9-309">**Şekil 10**: Yayımla Web sitesi iletişim kutusu</span><span class="sxs-lookup"><span data-stu-id="1d7b9-309">**Figure 10**: The Publish Web Site Dialog</span></span>


> [!NOTE]
> <span data-ttu-id="1d7b9-310">Aspnet\_compile.exe yardımcı programını ayrıca bir ASP.NET Web uygulaması önceden derlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-310">The aspnet\_compile.exe utility can also be used to pre-compile an ASP.NET Web application.</span></span> <span data-ttu-id="1d7b9-311">Bu aracı Modülü 9 ele alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-311">That tool will be covered in module 9.</span></span>


<span data-ttu-id="1d7b9-312">Ne zaman aşağıda gösterildiği gibi Yayımla bir Web sitesi önceden derlenmiş dosyaları ASP.NET dosyaları klasöründe depolanır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-312">When you Publish a Web site, the precompiled files are stored in the Temporary ASP.NET Files folder as shown below.</span></span> <span data-ttu-id="1d7b9-313">İle dosyaları bir *.compiled* dosya uzantısı olan belirli DLL'ler için bağımlıkları tanımlama XML dosyaları.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-313">Files with a *.compiled* file extension are XML files that define dependencies for particular DLLs.</span></span> <span data-ttu-id="1d7b9-314">Tüm Webform ya da kullanıcı denetimleri ile başlayan rastgele DLL'leri içine derlenen *uygulama\_Web\_*.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-314">Any Webform or user controls are compiled into random DLLs that begin with *App\_Web\_*.</span></span>

<span data-ttu-id="1d7b9-315">Bırakır *güncelleştirilebilir olması için bu önceden derlenmiş sitenin izin* onay kutusu işaretli değilse, biçimlendirme Webforms ve kullanıcı denetimleri içinde dağıtımdan sonra değişiklik olanak tanıyan bir DLL içerisine önceden derlenmiş olmayacak.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-315">If you leave the *Allow this precompiled site to be updatable* checkbox checked, markup inside of your Webforms and user controls will not be pre-compiled into a DLL allowing you to make changes after deployment.</span></span> <span data-ttu-id="1d7b9-316">Böylece değişiklikleri dağıtılan içerik için izin verilmeyen biçimlendirme kilitlemek tercih ederseniz, bu kutunun işaretini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-316">If you would prefer to lock down the markup so that changes to the deployed content are not allowed, uncheck this box.</span></span>

<span data-ttu-id="1d7b9-317">*Kullanım sabit adlandırma ve tek sayfa derlemelerinin* , böylece her sayfanın sabit adlı bir derlemeye derlenmiş toplu iş derlemesi devre dışı bırakmak onay kutusunu izin verir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-317">The *Use fixed naming and single page assemblies* checkbox allows you to disable batch compilation so that each page is compiled into a fixed-named assembly.</span></span> <span data-ttu-id="1d7b9-318">Bu kutu işaretlenmemesi, toplu iş derlemesi yararlanmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-318">Leaving this box unchecked allows you to take advantage of batch compilation.</span></span>

<span data-ttu-id="1d7b9-319">*Önceden derlenmiş derlemeleri etkinleştir üzerinde güçlü adlandırma* onay kutusunu verir tanımlayıcı adlı için derlenmiş derlemeleriniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-319">The *Enable strong naming on precompiled assemblies* checkbox allows you to strong-name your precompiled assemblies.</span></span>

> [!NOTE]
> <span data-ttu-id="1d7b9-320">ASP.NET 1.x, tanımlayıcı adlı derlemeler gerekiyordu Genel Derleme Önbelleği (GAC) içinde yüklü olması.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-320">In ASP.NET 1.x, strong-named assemblies had to be installed into the Global Assembly Cache (GAC).</span></span> <span data-ttu-id="1d7b9-321">ASP.NET 2. 0'da, siz tanımlayıcı adlı derlemeler GAC içine yüklemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-321">In ASP.NET 2.0, you are not required to install strong-named assemblies into the GAC.</span></span>


![Bir ASP.NET uygulamaları önceden derlenmiş dosyalar](improvements-in-visual-studio-2005/_static/image8.jpg)

<span data-ttu-id="1d7b9-323">**Şekil 11**: bir ASP.NET uygulamaları önceden derlenmiş dosyalar</span><span class="sxs-lookup"><span data-stu-id="1d7b9-323">**Figure 11**: An ASP.NET Applications Pre-Compiled Files</span></span>


> [!NOTE]
> <span data-ttu-id="1d7b9-324">Yukarıdaki uygulamada herhangi bir web.config dosyası vardı.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-324">In the application above, there was no web.config file.</span></span> <span data-ttu-id="1d7b9-325">Geliştirilmişse, onu çağrılmış *PrecompiledApp.config* sonra yayımlama Web sitesi işlemi.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-325">If there had been, it would have been called *PrecompiledApp.config* after the Publish Web site process.</span></span>


## <a name="improvements-in-deployment"></a><span data-ttu-id="1d7b9-326">Dağıtım yenilikleri</span><span class="sxs-lookup"><span data-stu-id="1d7b9-326">Improvements in Deployment</span></span>

<span data-ttu-id="1d7b9-327">Visual Studio 2002 ve 2003, Visual Studio 2005 kopyalama proje özellik önerir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-327">As with Visual Studio 2002 and 2003, Visual Studio 2005 offers a Copy Project feature.</span></span> <span data-ttu-id="1d7b9-328">Ancak, özellik Visual Studio 2005'te barındıracak ve şimdi Web sitesini kopyalama çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-328">However, the feature has been beefed up in Visual Studio 2005 and is now called Copy Web Site.</span></span>

<span data-ttu-id="1d7b9-329">Kopyalama Web sitesi iletişim kutusunda, sol çerçeve ve doğru bir çerçeve ayrılır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-329">The Copy Web Site dialog is split into a left frame and a right frame.</span></span> <span data-ttu-id="1d7b9-330">Sol çerçeve kaynak Web sitesi adı verilir ve sağ çerçeve uzak Web sitesi adı verilir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-330">The left frame is called the Source Web Site and the right frame is called the Remote Web Site.</span></span> <span data-ttu-id="1d7b9-331">Bazı geliştiriciler karışıklığa neden olabilir bir şey sağ çerçevede görüntülenen site mutlaka bir uzak site olmamasıdır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-331">One thing that may confuse some developers is that the site displayed in the right frame is not necessarily a remote site.</span></span> <span data-ttu-id="1d7b9-332">Yerel dosya sistemine veya IIS yerel örneği bir site olabilir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-332">It could be a site on the local file system or on the local instance of IIS.</span></span> <span data-ttu-id="1d7b9-333">İletişim kutusu, Uzak Web sitesinden yayımlamak tanır Ayrıca, sol çerçevede görüntülenen site mutlaka kaynak Web sitesi olmadığından *için* kaynak Web sitesi.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-333">Additionally, the site displayed in the left frame is not necessarily the source Web site because the dialog allows you to publish from the remote Web site *to* the source Web site.</span></span>

<span data-ttu-id="1d7b9-334">Bu site, Uzak Web sitesi için bir proje kopyalıyorsanız FrontPage Server Extensions yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-334">If you are copying a project to a remote Web site, that site must have the FrontPage Server Extensions installed on it.</span></span> <span data-ttu-id="1d7b9-335">Yoksa, FTP kullanarak bağlanmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-335">If it does not, you will need to connect using FTP.</span></span> <span data-ttu-id="1d7b9-336">Diğer taraftan, yerel IIS örneği için bir proje kopyalıyorsanız FrontPage Server Extensions gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-336">On the other hand, if you are copying a project to the local IIS instance, FrontPage Server Extensions are not required.</span></span>

> [!NOTE]
> <span data-ttu-id="1d7b9-337">Yerel IIS örneğinde yeni bir Web sitesi oluşturmayı deneyin ve FrontPage 2002 Sunucu Uzantıları yüklü Web siteleri oluşturma bir SharePoint sunucusu üzerine desteklenmiyor bildiren bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-337">If you try to create a new Web site on the local IIS instance and the FrontPage 2002 Server Extensions are installed, you will get an error message telling you that creating Web sites is not supported on a SharePoint server.</span></span> <span data-ttu-id="1d7b9-338">Bu durumda, FrontPage 2000 Server Extensions yükleme veya FrontPage Server Extensions kaldırma seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-338">In that case, you have the option of installing the FrontPage 2000 Server Extensions or removing the FrontPage Server Extensions.</span></span>


<span data-ttu-id="1d7b9-339">Web sitesini kopyalama özelliğinin bir videosu için burayı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-339">Click here for a video walkthrough of the Copy Web Site feature.</span></span>


![](improvements-in-visual-studio-2005/_static/image4.png)


[<span data-ttu-id="1d7b9-340">Açık Tam Ekran Video</span><span class="sxs-lookup"><span data-stu-id="1d7b9-340">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a><span data-ttu-id="1d7b9-341">Hata ayıklama içinde geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="1d7b9-341">Improvements in Debugging</span></span>

<span data-ttu-id="1d7b9-342">Visual Studio 2005'te hata ayıklama, dört anahtar geliştirmeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-342">There are four key improvements in debugging in Visual Studio 2005.</span></span>

- <span data-ttu-id="1d7b9-343">Yerel bir yönetici olmayan hata ayıklama kutu dışı mümkündür.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-343">Debugging locally as a non-administrator is possible out of the box.</span></span>
- <span data-ttu-id="1d7b9-344">Hata ayıklama için derleme öğesi artık varsayılan olarak false özniteliktir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-344">The Debug attribute for the Compilation element is now false by default.</span></span>
- <span data-ttu-id="1d7b9-345">Uzaktan hata ayıklama kurulumu ve yapılandırması önce daha kolay olur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-345">Remote debugging setup and configuration is easier than before.</span></span>
- <span data-ttu-id="1d7b9-346">Şimdi bir Web sitesi FTP konumu açılan ayıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-346">You can now debug a Web site opened via an FTP location.</span></span>

## <a name="debugging-as-a-non-administrator"></a><span data-ttu-id="1d7b9-347">Yönetici olmayan hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="1d7b9-347">Debugging as a Non-Administrator</span></span>

<span data-ttu-id="1d7b9-348">ASP.NET Geliştirme Sunucusu eklenmesi, yönetici olmayanların kolayca kutunun sağ dışında ASP.NET uygulamalarında hata ayıklamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-348">The addition of the ASP.NET Development Server allows non-administrators to easily debug ASP.NET applications right out of the box.</span></span> <span data-ttu-id="1d7b9-349">Yerel dosya sistemi üzerinde çalışan bir ASP.NET uygulaması hata ayıklaması, Visual Studio oturum açan kullanıcının bağlamında ASP.NET geliştirme sunucusu başlatır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-349">When an ASP.NET application running on the local file system is debugged, Visual Studio launches the ASP.NET Development Server under the context of the logged-on user.</span></span> <span data-ttu-id="1d7b9-350">Bu kullanıcının bu uygulamayı herhangi bir ek yapılandırma olmadan sonra ayıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-350">That user can then debug that application without any additional configuration.</span></span>

## <a name="debug-is-false-by-default"></a><span data-ttu-id="1d7b9-351">Hata ayıklama değeri: False varsayılan</span><span class="sxs-lookup"><span data-stu-id="1d7b9-351">Debug is False by Default</span></span>

<span data-ttu-id="1d7b9-352">ASP.NET 1.x, *hata ayıklama* özniteliğini *derleme* web.config dosyasının öğesi ayarlandığı *true* varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-352">In ASP.NET 1.x, the *debug* attribute in the *compilation* element of the web.config file was set to *true* by default.</span></span> <span data-ttu-id="1d7b9-353">Bu her zaman geliştiriciler bu öznitelik ayarlanırsa, önerilen *false* üretim, uygulamaya dağıtmadan önce ancak çoğu Geliştirici ayarlamak hata ayıklama özniteliği bırakarak sonuçlarıyla tam olarak anlamadığınız TRUE, bunlar yalnızca olarak sol-değil.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-353">It has always been recommended that developers set this attribute to *false* before deploying an application to production, but because most developers don't fully understand the consequences of leaving the debug attribute set to true, they simply left it as-is.</span></span>

<span data-ttu-id="1d7b9-354">En ciddi sorun hata ayıklama özniteliğine sahip olan true, ASP.NETs Toplu derleme modeli devre dışı bırakır kümesidir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-354">The most severe problem with having the debug attribute set to true is that it disables ASP.NETs batch compilation model.</span></span> <span data-ttu-id="1d7b9-355">Bu nedenle, her sayfanın ayrı bir DLL'e derlenir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-355">Therefore, each page is compiled into a separate DLL.</span></span> <span data-ttu-id="1d7b9-356">Anlamına gelir bir Web sayfası (değil unheard herhangi bir yolla süresi), binlerce uygulama oluşur, birkaç bin küçük DLL'leri uygulama tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-356">If a Web application consists of thousands of pages (not unheard of by any means), that means several thousand small DLLs will be created by that application.</span></span> <span data-ttu-id="1d7b9-357">Bu DLL'ler boyutunda küçük olsa da, bunlar belirli bir konuma bellekte yüklü değildir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-357">While these DLLs are small in size, they are not loaded into any particular location in memory.</span></span> <span data-ttu-id="1d7b9-358">Bu nedenle, sistem belleğini parçalanması neden ve OutOfMemoryException tekrarlara katkıda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-358">Therefore, they cause fragmentation in system memory and can contribute to OutOfMemoryException occurrences.</span></span>

<span data-ttu-id="1d7b9-359">ASP.NET 2. 0'da, hata ayıklama özniteliği varsayılan olarak false değerine ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-359">In ASP.NET 2.0, the debug attribute is set to false by default.</span></span> <span data-ttu-id="1d7b9-360">Zaten, ne zaman bir ASP.NET uygulaması Visual Studio 2005 ' te bir geliştirici debugs gördüğünüz gibi hata ayıklama etkin durumdayken bir web.config dosyası eklemeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-360">As you have already seen, when a developer debugs an ASP.NET application in Visual Studio 2005, they are prompted to add a web.config file with debugging enabled.</span></span> <span data-ttu-id="1d7b9-361">Bunun yapılması, ASP.NET mevcut aynı dezavantajları doğurur 1.x ancak şimdi Geliştirici açıkça uyarılır uygulama üretime geçmeden önce öznitelik false değerine sıfırlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-361">Doing so incurs the same drawbacks that were present in ASP.NET 1.x, but now the developer is clearly warned that the attribute should be reset to false before moving the application to production.</span></span>

## <a name="remote-debugging-setup-and-configuration"></a><span data-ttu-id="1d7b9-362">Uzaktan hata ayıklama kurulumu ve yapılandırması</span><span class="sxs-lookup"><span data-stu-id="1d7b9-362">Remote Debugging Setup and Configuration</span></span>

<span data-ttu-id="1d7b9-363">Visual Studio 2002/2003'te, uzaktan hata ayıklama için Makine Hata Ayıklama Yöneticisi (mdm.exe) ve vs7jit.exe işlem dayanıyordu.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-363">In Visual Studio 2002/2003, remote debugging relied on the Machine Debug Manager (mdm.exe) and the vs7jit.exe process.</span></span> <span data-ttu-id="1d7b9-364">Nedeniyle, uzaktan hata ayıklama sorunlarını giderme genellikle müşteriler için siyah bir kutu oldu ve PSS için daha iyi genellikle değil.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-364">Because of that, troubleshooting remote debugging problems was often a black box for customers and it was often not much better for PSS.</span></span>

<span data-ttu-id="1d7b9-365">Visual Studio 2005 mdm.exe ve vs7jit.exe işlemleri bağımlılığı ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-365">Visual Studio 2005 removes the reliance on the mdm.exe and vs7jit.exe processes.</span></span> <span data-ttu-id="1d7b9-366">Bunun yerine, uzaktan hata ayıklama İzleyicisi hizmet (msvsmon.exe) şimdi kullanır</span><span class="sxs-lookup"><span data-stu-id="1d7b9-366">Instead, it now uses the Remote Debug Monitor service (msvsmon.exe.)</span></span>

<span data-ttu-id="1d7b9-367">Visual Studio 2005'te uzaktan hata ayıklama gereksinimi oldukça basittir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-367">The requirement for debugging in Visual Studio 2005 remotely is quite simple.</span></span> <span data-ttu-id="1d7b9-368">Hata ayıklama önce uzak sunucuda msvsmon.exe çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-368">You need to run msvsmon.exe on the remote server prior to debugging.</span></span> <span data-ttu-id="1d7b9-369">Visual Studio CD'den uzaktan hata ayıklama İzleyicisi'ni yükleyebilirsiniz veya hiçbir şey hiç Web sunucusunda yüklemeden msvsmon.exe bir paylaşımdan basitçe çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-369">You can install the Remote Debug Monitor from the Visual Studio CD or you can simply run msvsmon.exe from a share without installing anything at all on the Web server.</span></span>

<span data-ttu-id="1d7b9-370">Msvsmon.exe çalıştırdığınızda, uzaktan hata ayıklama için engellenme bağlantı noktaları hakkında şikayetçi olasıdır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-370">When you run msvsmon.exe, it is likely that it will complain about ports being blocked for remote debugging.</span></span> <span data-ttu-id="1d7b9-371">Neyse ki, kolayca uyarı iletişim kutusu içinde sağa bağlantı noktalarından aşağıda gösterildiği gibi engellemesini kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-371">Fortunately, you can easily unblock the ports from right within the warning dialog as shown below.</span></span>


![Windows Güvenlik Duvarı Engelleme uzaktan hata ayıklama olduğunu söyleyen bir bildirim](improvements-in-visual-studio-2005/_static/image9.jpg)

<span data-ttu-id="1d7b9-373">**Şekil 12**: Windows Güvenlik Duvarı bildirimidir engelleme uzaktan hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="1d7b9-373">**Figure 12**: Notification that Windows Firewall is Blocking Remote Debugging</span></span>


<span data-ttu-id="1d7b9-374">Hata ayıklama için gerekli bağlantı noktalarını engellemesini sonra aşağıda gösterildiği gibi uzaktan hata ayıklama İzleyicisi'ni görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-374">Once you have unblocked the ports necessary for debugging, you will see the Remote Debugging Monitor as shown below.</span></span> <span data-ttu-id="1d7b9-375">Bu arabirimden bağlantılarını izleyebilir ve izinleri kolayca hata ayıklama değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-375">From this interface, you can monitor connections and change debugging permissions easily.</span></span>


![Uzaktan hata ayıklama İzleyicisi](improvements-in-visual-studio-2005/_static/image10.jpg)

<span data-ttu-id="1d7b9-377">**Şekil 13**: uzaktan hata ayıklama İzleyicisi</span><span class="sxs-lookup"><span data-stu-id="1d7b9-377">**Figure 13**: The Remote Debugging Monitor</span></span>


<span data-ttu-id="1d7b9-378">FTP aracılığıyla açılan bir Web uygulaması uzaktan hata ayıklama mümkündür.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-378">It is also possible to remotely debug a Web application opened via FTP.</span></span> <span data-ttu-id="1d7b9-379">Adımları daha önce ele aynıdır.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-379">The steps are the same as those previously covered.</span></span> <span data-ttu-id="1d7b9-380">Ancak, daha önce bu modülde özetlendiği gibi FTP projesi göz atmak için bir temel URL'yi belirtmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-380">However, you will need to specify a base URL for browsing the FTP project as outlined earlier in this module.</span></span>

## <a name="lab-2"></a><span data-ttu-id="1d7b9-381">Laboratuvar 2</span><span class="sxs-lookup"><span data-stu-id="1d7b9-381">Lab 2</span></span>

## <a name="remote-debugging-with-visual-studio-2005"></a><span data-ttu-id="1d7b9-382">Visual Studio 2005 ile uzaktan hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="1d7b9-382">Remote Debugging with Visual Studio 2005</span></span>

<span data-ttu-id="1d7b9-383">Bu Laboratuvar Visual Studio 2005 ile uzaktan hata ayıklama size yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-383">This lab will walk you through remote debugging with Visual Studio 2005.</span></span>

<span data-ttu-id="1d7b9-384">Bu laboratuvarı bir videosu için burayı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-384">Click here for a video walkthrough of this lab.</span></span>


![](improvements-in-visual-studio-2005/_static/image5.png)


[<span data-ttu-id="1d7b9-385">Açık Tam Ekran Video</span><span class="sxs-lookup"><span data-stu-id="1d7b9-385">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


<span data-ttu-id="1d7b9-386">Bu Laboratuvar iki makine, bir çalışan Visual Studio 2005 ve diğer çalışan IIS 5 veya üzeri olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-386">This lab requires you to have two machines, one running Visual Studio 2005 and the other running IIS 5 or greater.</span></span>

1. <span data-ttu-id="1d7b9-387">Visual Studio 2005 açın ve uzak sunucuda yeni bir Web sitesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-387">Open Visual Studio 2005 and create a new Web site on the remote server.</span></span>

> [!NOTE]
> <span data-ttu-id="1d7b9-388">Uzak bir IIS örneği veya FTP üzerinden Web sitesi oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-388">You can create the Web site on a remote IIS instance or via FTP.</span></span>


1. <span data-ttu-id="1d7b9-389">Uzak Web sunucusundan bir UNC yolu kullanarak geliştirme makinenizde msvsmon.exe bulun ve yürütebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-389">From the remote Web server, locate msvsmon.exe on the development machine using a UNC path and execute it.</span></span>  
 <span data-ttu-id="1d7b9-390">Msvsmon.exe varsayılan konumu \\server\c$ \Program Visual Studio 8\Common7\IDE\Remote Debugger\x86.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-390">The default location for msvsmon.exe is \\server\c$\Program Files\Microsoft Visual Studio 8\Common7\IDE\Remote Debugger\x86.</span></span>
2. <span data-ttu-id="1d7b9-391">Uzaktan hata ayıklama için bağlantı noktalarını engellemesini kaldırmak isteyip istemediğiniz sorulduğunda bunu yapar.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-391">If prompted to unblock ports for remote debugging, do so.</span></span>
3. <span data-ttu-id="1d7b9-392">Geliştirme makineden arka plan koduna Default.aspx açın ve sayfanın bir kesme noktası ayarlayın\_yükleme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-392">From the development machine, open the code-behind for Default.aspx and set a breakpoint in the Page\_Load method.</span></span>
4. <span data-ttu-id="1d7b9-393">Geliştirme makineden hata ayıklamayı Başlat.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-393">Start debugging from the development machine.</span></span>

<span data-ttu-id="1d7b9-394">Beklendiği gibi kesme noktası isabet.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-394">You should hit the breakpoint as expected.</span></span>

## <a name="aspnet-development-server"></a><span data-ttu-id="1d7b9-395">ASP.NET Geliştirme Sunucusu</span><span class="sxs-lookup"><span data-stu-id="1d7b9-395">ASP.NET Development Server</span></span>

<span data-ttu-id="1d7b9-396">Zaten ele weve Visual Studio 2005 ASP.NET Geliştirme Sunucusu adı verilen bir Web sunucusu ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-396">As weve already discussed, Visual Studio 2005 ships with a Web server called the ASP.NET Development Server.</span></span> <span data-ttu-id="1d7b9-397">(ASP.NET Geliştirme Sunucusu bazen Cassini adlandırılır.) Bu Web sunucusu göz atmak ve dosya sistemi üzerinde çalışan Web uygulamalarında hata ayıklamak için kullanışlı bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-397">(The ASP.NET Development Server is sometimes referred to as Cassini.) This Web server is a convenient means to browse and debug Web applications running on the file system.</span></span>

<span data-ttu-id="1d7b9-398">ASP.NET Geliştirme Sunucusu kısıtlı bir Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-398">The ASP.NET Development Server is a restricted Web server.</span></span> <span data-ttu-id="1d7b9-399">Uzak bağlantılara izin vermez, onu tüm istekler Web sunucusu başlatan kullanıcı dışındaki herhangi bir kullanıcıdan izin vermiyor.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-399">It does not allow remote connections, it does not allow any requests from any user other than the user who started the Web server.</span></span> <span data-ttu-id="1d7b9-400">Ayrıca ASP sayfalarını sunmadan özelliği yok.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-400">It also does not have the capability of serving ASP pages.</span></span> <span data-ttu-id="1d7b9-401">Yalnızca ASP.NET ve HTML kaynaklarının (görüntüleri, CSS dosyaları, vb. dahil) sunulur.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-401">Only ASP.NET resources and HTML resources (including images, CSS files, etc.) are served.</span></span>

<span data-ttu-id="1d7b9-402">ASP.NET Geliştirme Sunucusu c:\Windows\Microsoft.NET\Framework\v2.0 bulunan WebDev.WebServer.exe dosyasını çalıştırarak komut satırı üzerinden başlatılabilir. \*\*\*\*\*.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-402">The ASP.NET Development Server can be launched via the command line by running the WebDev.WebServer.exe file located at c:\Windows\Microsoft.NET\Framework\v2.0.\*\*\*\*\*.</span></span> <span data-ttu-id="1d7b9-403">Kullanılabilir parametreler aşağıdaki iletişim kutusunu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-403">The following dialog displays the parameters that are available.</span></span>


![](improvements-in-visual-studio-2005/_static/image11.jpg)

<span data-ttu-id="1d7b9-404">**Şekil 14**</span><span class="sxs-lookup"><span data-stu-id="1d7b9-404">**Figure 14**</span></span>


> [!NOTE]
> <span data-ttu-id="1d7b9-405">ASP.NET Geliştirme Sunucusu açıkça komut satırı başlatıldığında desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="1d7b9-405">The ASP.NET Development Server is not supported when launched explicitly via the command line.</span></span>
