---
uid: web-pages/readme/overview
title: WebMatrix Benioku | Microsoft Docs
author: rick-anderson
description: WebMatrix ve ASP.NET Web sayfaları (Razor) 1.0 sürümü Benioku
ms.author: aspnetcontent
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 8aa0eedb3156ca33905c106b72d5fa553a917b16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822868"
---
<a name="webmatrix-readme"></a><span data-ttu-id="21945-103">WebMatrix Benioku dosyası</span><span class="sxs-lookup"><span data-stu-id="21945-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="21945-104">13 Ocak 2011</span><span class="sxs-lookup"><span data-stu-id="21945-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="21945-105">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="21945-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="21945-106">Bu Benioku WebMatrix 1.0 sürümü için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="21945-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="21945-107">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="21945-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="21945-108">Yükleme</span><span class="sxs-lookup"><span data-stu-id="21945-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="21945-109">Uygulamaların nasıl yayımlanacağı</span><span class="sxs-lookup"><span data-stu-id="21945-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="21945-110">Değişiklikleri ve sorunları</span><span class="sxs-lookup"><span data-stu-id="21945-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="21945-111">WebMatrix 1.0 yükleme</span><span class="sxs-lookup"><span data-stu-id="21945-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="21945-112">ASP.NET Web Sayfaları</span><span class="sxs-lookup"><span data-stu-id="21945-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="21945-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="21945-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="21945-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="21945-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="21945-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="21945-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="21945-116">Uygulamaları yükleme</span><span class="sxs-lookup"><span data-stu-id="21945-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="21945-117">Uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="21945-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="21945-118">Daha fazla bilgi için</span><span class="sxs-lookup"><span data-stu-id="21945-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="21945-119">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="21945-119">Overview</span></span>

> <span data-ttu-id="21945-120">Microsoft WebMatrix 1.0 dakikalar içinde yüklenen bir ücretsiz web geliştirme yığınıdır.</span><span class="sxs-lookup"><span data-stu-id="21945-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="21945-121">Veritabanını ve programlama çerçevelerini tek, tümleşik bir deneyim oluşturmak için bir web sunucusu tümleştirir.</span><span class="sxs-lookup"><span data-stu-id="21945-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="21945-122">DotNetNuke, Umbraco, WordPress ve Joomla gibi popüler açık kaynak uygulamalar kullanarak yeni bir Web sitesini başlatmak için WebMatrix kullanma veya WebMatrix kod, test ve kendi ASP.NET veya PHP Web sitesi yayımlama yolu kolaylaştırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21945-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="21945-123">WebMatrix, aynı güçlü web sunucusu, veritabanı altyapısı ve Web sitenizi geliştirmeden üretime geçişinizin düzgün ve sorunsuz kılan Internet üzerinde çalışır ve çerçeveler ortamının kullanır.</span><span class="sxs-lookup"><span data-stu-id="21945-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="21945-124">Yükleme</span><span class="sxs-lookup"><span data-stu-id="21945-124">Installation</span></span>

> <span data-ttu-id="21945-125">WebMatrix 1.0 yüklemek için önce yüklemelisiniz [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="21945-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="21945-126">Web Platformu yükleyicisi yükledikten sonra WebMatrix yüklemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21945-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="21945-127">Yükleme sırasında sorunlarla karşılaşırsanız, başvurmak [Microsoft Web Platformu Yükleyicisi ile sorunları giderme](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="21945-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="21945-128">Uygulamaların nasıl yayımlanacağı</span><span class="sxs-lookup"><span data-stu-id="21945-128">How to Publish Applications</span></span>

> <span data-ttu-id="21945-129">Bkz: [uygulamaları yayımlama hakkında adım adım yönergeler](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="21945-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="21945-130">Değişiklikleri ve sorunları</span><span class="sxs-lookup"><span data-stu-id="21945-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="21945-131">WebMatrix 1.0 yükleme sorunları</span><span class="sxs-lookup"><span data-stu-id="21945-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="21945-132">Sorun: WebMatrix 1.0 yalnızca Microsoft .NET Framework 4'ü destekleyen platformlar üzerinde kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="21945-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="21945-133">WebMatrix için .NET Framework sürüm 4 gereklidir.</span><span class="sxs-lookup"><span data-stu-id="21945-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="21945-134">Bazı durumlarda, WebMatrix 1.0 yükleyici, desteklenen bir yapılandırma kümesinin parçası olmayan bir platformda yüklemeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="21945-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="21945-135">Özellikle, Windows Vista SP1 Güncelleştirmesi olmadan, WebMatrix yüklemesini başlatmak olanak tanır, ancak .NET Framework 4 bileşeni başarısız olur ve yüklemenizi engelleme.</span><span class="sxs-lookup"><span data-stu-id="21945-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="21945-136">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-136">**Workaround**</span></span>  
> <span data-ttu-id="21945-137">İçeren desteklenen bir platform üzerinde yükleyin:</span><span class="sxs-lookup"><span data-stu-id="21945-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="21945-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="21945-138">Windows 7</span></span>
> - <span data-ttu-id="21945-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="21945-139">Windows Server 2008</span></span>
> - <span data-ttu-id="21945-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="21945-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="21945-141">Windows Vista SP1 veya sonraki sürümü</span><span class="sxs-lookup"><span data-stu-id="21945-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="21945-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="21945-142">Windows XP SP3</span></span>
> - <span data-ttu-id="21945-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="21945-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="21945-144">Sorun: Microsoft Visual Studio 2008 Microsoft Visual Studio 2008 SP1 yüklediyseniz WebMatrix 1.0 yüklenemiyor</span><span class="sxs-lookup"><span data-stu-id="21945-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="21945-145">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-145">**Workaround**</span></span>  
> <span data-ttu-id="21945-146">Yükleme [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) Microsoft İndirme Merkezi'nden.</span><span class="sxs-lookup"><span data-stu-id="21945-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="21945-147">Sorun: GAC'ye bazı derlemeler için SQL Server Compact 4.0 yüklü değil</span><span class="sxs-lookup"><span data-stu-id="21945-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="21945-148">SQL Server Compact 4.0 için yönetilen derlemeleri genel derleme önbelleğinde (GAC), 64-bit bir bilgisayarda SQL Server Compact 4.0 yükledikten ve bilgisayarın yalnızca .NET Framework 3.5 SP1 istemci yüklü profili olduğunda yerleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="21945-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="21945-149">GAC'de kurulu değil Yönetilen derlemeler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="21945-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="21945-150">*System.Data.SqlServerCe.dll'ye* (ADO.NET sağlayıcısı)</span><span class="sxs-lookup"><span data-stu-id="21945-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="21945-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET varlık çerçevesi)</span><span class="sxs-lookup"><span data-stu-id="21945-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="21945-152">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-152">**Workaround**</span></span>  
> <span data-ttu-id="21945-153">Kaldırma SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="21945-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="21945-154">İndirin ve .NET Framework 3.5 SP1'in tam sürümünü şu konumdan yükleyin:</span><span class="sxs-lookup"><span data-stu-id="21945-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="21945-155">Microsoft .NET Framework 3.5 Service pack 1 (tam paket)</span><span class="sxs-lookup"><span data-stu-id="21945-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="21945-156">Ardından SQL Server Compact 4.0 yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="21945-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="21945-157">Sorun: SQL Server için komut satırını kullanarak Compact kaldırılamıyor</span><span class="sxs-lookup"><span data-stu-id="21945-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="21945-158">SQL Server komut satırı seçeneklerini kullanarak Compact kaldırılması, bu sürümde çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="21945-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="21945-159">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-159">**Workaround**</span></span>  
> <span data-ttu-id="21945-160">Kullanım *programlar ve Özellikler* Microsoft SQL Server Compact 4.0 kaldırmak için Windows Denetim Masası'nda.</span><span class="sxs-lookup"><span data-stu-id="21945-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="21945-161">ASP.NET Web Sayfaları</span><span class="sxs-lookup"><span data-stu-id="21945-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="21945-162">Belgenin bu bölümünde, yeni özellikleri, değişiklikler ve Razor sözdizimi olan ASP.NET Web sayfaları, 1.0 sürümü ile ilgili bilinen sorunlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="21945-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="21945-163">Yeni Özellikler</span><span class="sxs-lookup"><span data-stu-id="21945-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="21945-164">Değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="21945-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="21945-165">Sorunları</span><span class="sxs-lookup"><span data-stu-id="21945-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="21945-166">Yeni Özellikler</span><span class="sxs-lookup"><span data-stu-id="21945-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="21945-167">Yeni: Paket Yöneticisi devre dışı bırakmak için yapılandırma ayarı eklendi</span><span class="sxs-lookup"><span data-stu-id="21945-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="21945-168">Yeni bir `asp:AdminManagerEnabled` anahtarı, kullanılabilir `<appSettings>` öğesinde *web.config* tamamen Paket Yöneticisi devre dışı bırakmanıza olanak sağlayan dosya.</span><span class="sxs-lookup"><span data-stu-id="21945-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="21945-169">Bu öğe için varsayılan değer true olarak dahil edilmemişse, yani *web.config* dosyasını, Paket Yöneticisi etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="21945-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="21945-170">Paket Yöneticisi devre dışı bırakmak için aşağıdaki öğeyi ekleyin *web.config* Web sitesinin kök dosyasında:</span><span class="sxs-lookup"><span data-stu-id="21945-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="21945-171">Değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="21945-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="21945-172">Değiştir: "webPages:AdminFolderVirtualPath" anahtarını "asp: AdminFolderVirtualPath" olarak yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="21945-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="21945-173">`webPages:AdminFolderVirtualPath` Eklenebilir anahtarı *web.config* Paket Yöneticisi konumunu belirtmek için dosya kullanmak için adlandırıldı `asp:` yerine ad alanı `webPages` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="21945-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="21945-174">Bu öğe kullandıysanız, yapılandırma dosyasında yeniden adlandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="21945-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="21945-175">Bilinen sorunlar</span><span class="sxs-lookup"><span data-stu-id="21945-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="21945-176">Sorun: Üyelik kullanıcılarının artık tanınmadığı parolaları</span><span class="sxs-lookup"><span data-stu-id="21945-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="21945-177">Algoritma oluşturmak ve üyelik (oturum açma bilgileri) depolamak için daha güvenli olacak şekilde değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="21945-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="21945-178">Sonuç olarak, ASP.NET Razor Beta sürümlerinde oluşturulan üyeleri (kullanıcılar) için saklanan parolalar tanınmaz.</span><span class="sxs-lookup"><span data-stu-id="21945-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="21945-179">**Geçici çözüm** site henüz üretime alındığından değil, üyelik veritabanından kullanıcı kayıtlarını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="21945-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="21945-180">Veritabanının Canlı olması durumunda, üyelik veritabanında var olan parolaların programlı olarak yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="21945-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="21945-181">Sorun: bir özel bir kullanıcı tablosu için üyeliği kullanılırken beklenmeyen davranışı</span><span class="sxs-lookup"><span data-stu-id="21945-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="21945-182">Bir ASP.NET Razor Web sitesi için üyelik sağlayıcıyı başlatmak için çağrı `WebSecurity.InitializeDatabaseConnection` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="21945-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="21945-183">(Webmatrix'te, başlangıç sitesi şablonunda bu yönteme bir çağrı içerir.  *\_AppStart.cshtml* dosyası.) Varsa `autoCreateTables` bu yöntemin bir parametrenin ayarlanmış true (varsayılan olarak ayarlanır başlangıç sitesi şablonunda true), ve bir tanınmayan tablo adı (ikinci parametresi) yöntemine geçirilen, yöntem bir hata oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="21945-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="21945-184">Bunun yerine, otomatik olarak bir tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="21945-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="21945-185">Üyelik için bir özel bir kullanıcı tablosu kullanır ancak yanlış tablo adına geçirmek istiyorsanız, bu bir sorun olabilir `WebSecurity.InitializeDatabaseConnection` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="21945-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="21945-186">Belirttiğiniz tablo mevcut değilse yöntemi varsayılan olarak bir hata oluşturmaz, çünkü ve bunun yerine yeni bir tablo oluşturur çünkü uygulama çalışıyor gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="21945-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="21945-187">Ancak, özel kullanıcı tablonuzda (ve bu alanlara) kullanan uygulama kodu sonunda beklenmeyen hatalarla başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="21945-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="21945-188">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-188">**Workaround**</span></span>  
> <span data-ttu-id="21945-189">İçinde geçirilen ad emin `InitializeDatabaseConnection` kullanıcı profili tablosunda üyelik veritabanında veya devre dışı olduğundan emin olun yöntemi eşleşme `autoCreateTables` parametresini false olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="21945-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="21945-190">Sorun: Hata iletisi "Yönetici modülü ~/App erişmesi\_veri"</span><span class="sxs-lookup"><span data-stu-id="21945-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="21945-191">Bazı durumlarda, kullanıcılar oluşturma veya aksi takdirde ASP.NET üyelik sistemi ile iş çalışılırken hata görüntülemek sayfanın neden olabilir *yönetim modülü ~/App erişmesi gerekiyor\_veri*.</span><span class="sxs-lookup"><span data-stu-id="21945-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="21945-192">IIS veya IIS Express altında çalıştığı hesabı oluşturma ve yazma izinlerine sahip değilse bu gerçekleşir *uygulama\_veri* altında Web sitesinin kök klasörü.</span><span class="sxs-lookup"><span data-stu-id="21945-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="21945-193">**Geçici çözüm** el ile oluşturmak bir *uygulama\_veri* Web sitesi için bir klasör.</span><span class="sxs-lookup"><span data-stu-id="21945-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="21945-194">Uygulama (genellikle altında ağ hizmeti) çalıştıran Windows hesabının kök klasörleri uygulamanın ve uygulama gibi alt klasörleri için okuma/yazma izinleri bulunduğundan emin\_veri.</span><span class="sxs-lookup"><span data-stu-id="21945-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="21945-195">Bilgi Bankası makalesinde daha ayrıntılı bilgi kullanılabilir [sorunları kullanıcı SQL Server Express örneği oluşturmayı ve ASP.net Web uygulaması projelerinde](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="21945-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="21945-196">Sorun: "SQL Server'ın bir kullanıcı örneği başarısız" hatası</span><span class="sxs-lookup"><span data-stu-id="21945-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="21945-197">Bir WebMatrix Web uygulaması, SQL Server Express kullanan ve IIS 7.5 Windows 7 veya Windows Server 2008 R2 çalıştıran, SQL Server çalışma zamanında kullanıcının yerel uygulama yolu alınamıyor belirten bir hata görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21945-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="21945-198">**Geçici çözüm** uygulama (genellikle altında ağ hizmeti) çalıştıran Windows hesabı gibi uygulamanın kök klasörler ve alt klasörleri için okuma/yazma izinlerine sahip olduğundan emin olun *uygulama\_veri*.</span><span class="sxs-lookup"><span data-stu-id="21945-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="21945-199">Bilgi Bankası makalesinde daha ayrıntılı bilgi kullanılabilir [sorunları kullanıcı SQL Server Express örneği oluşturmayı ve ASP.net Web uygulaması projelerinde](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="21945-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="21945-200">Sorun: Paket Yöneticisi parolaları veya Paket Yöneticisi kaynaklar içeren dosyaları servable altında IIS 6.0 ve önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="21945-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="21945-201">RC2 yayın kullanılarak oluşturulmuş bir ASP.NET Web sayfaları (Razor) uygulama dağıtırsanız ve uygulama içeriyorsa, bir *password.txt* veya *packagesources.txt* altında dosya */App\_ Veri/admin*, IIS 6.0, dosyanın Paket Yöneticisi Örneğiniz için parolaları potansiyel olarak gösterme, istenmesi halinde hizmet edecektir.</span><span class="sxs-lookup"><span data-stu-id="21945-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="21945-202">**Geçici çözüm** Yeniden Adlandır *password.txt* veya *packagesources.txt* dosyasını *password.config* veya *packagesources.config*. Varsayılan olarak, IIS 6.0 olan dosyalar sunmuyor *.config* uzantısı.</span><span class="sxs-lookup"><span data-stu-id="21945-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="21945-203">(IIS 7 ' de hiçbir dosya *uygulama\_veri* klasör sunulan, dosyaları yeniden adlandırmak gerek yoktur.)</span><span class="sxs-lookup"><span data-stu-id="21945-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="21945-204">Sorun: Beta 3 yayını kullanarak yüklü paketleri kaldırılıyor tamamen paket bileşenleri kaldırmaz</span><span class="sxs-lookup"><span data-stu-id="21945-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="21945-205">Beta 3 sürümde Paket Yöneticisi'ni kullanarak bir paket yüklü ve mevcut sürümde kullanarak kaldırmak deneyin, paket tümüyle kaldırılmamış.</span><span class="sxs-lookup"><span data-stu-id="21945-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="21945-206">Paket Yöneticisi'nin kullanarak **kaldırma** düğmesi bazı bileşenleri kaldırır ancak paket kitaplık kodu bırakır ve güncelleştirilmediği *package.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="21945-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="21945-207">**Geçici çözüm** </span><span class="sxs-lookup"><span data-stu-id="21945-207">**Workaround** </span></span>  
> <span data-ttu-id="21945-208">Aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="21945-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="21945-209">Silme *uygulama\_Data\packages* klasör.</span><span class="sxs-lookup"><span data-stu-id="21945-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="21945-210">Bu, tüm paketler kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="21945-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="21945-211">Silme *packages.config* Web sitesinin kök dosyasında.</span><span class="sxs-lookup"><span data-stu-id="21945-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="21945-212">Sorun: Visual Studio'da, web tabanlı Paket Yöneticisi çağırma uygulamayı çevrimdışı duruma getirir</span><span class="sxs-lookup"><span data-stu-id="21945-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="21945-213">Visual Studio'da (WebMatrix değil) çalışıyor ve kullanmak  *\_yönetici* Visual Studio Paket Yöneticisi'ni başlatmak için işlevsellik uygulamayı çevrimdışı duruma getirir ve gönderileri *uygulama\_ Offline.htm* Web sitesinin kök ile hangi bozar yeteneğinizi paket yöneticisini kullanın.</span><span class="sxs-lookup"><span data-stu-id="21945-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="21945-214">En yaygın web tabanlı Paket Yöneticisi arabirimi kullanılırken bu davranışı görür ancak aynı davranış ekleyin, kaldırın veya herhangi bir dosyayı değiştirmek ortaya çıkar *uygulama\_veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="21945-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="21945-215">**Geçici çözüm** </span><span class="sxs-lookup"><span data-stu-id="21945-215">**Workaround** </span></span>  
> <span data-ttu-id="21945-216">Visual Studio'da paketleriyle çalışmak için NuGet uzantısı yerine web tabanlı Paket Yöneticisi'ni kullanın.</span><span class="sxs-lookup"><span data-stu-id="21945-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="21945-217">Bilgi için [NuGet belgeleri](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="21945-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="21945-218">Diğer dosyalarıyla çalışıyorsanız *uygulama\_veri* klasör, bu sorunu önlemek için başka bir yerde dosyaları tutmaya dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="21945-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="21945-219">Bu pratik değilse Sil *uygulama\_offline.htm* dosyasını el ile veya otomatik olarak (varsayılan olarak 30 saniyeden sonra) sitenin tekrar çevrimiçi gelene kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="21945-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="21945-220">Sorun: Visual Studio IntelliSense ve proje şablonları yalnızca ASP.NET MVC 3. sürüm kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="21945-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="21945-221">ASP.NET Web sayfaları yüklenmesi de araçları Visual Studio için ASP.NET Web Pages uygulamaları için IntelliSense ve proje şablonları gibi yüklemez.</span><span class="sxs-lookup"><span data-stu-id="21945-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="21945-222">**Geçici çözüm** IntelliSense ve proje şablonları ASP.NET Web Pages uygulamaları Visual Studio'da kullanmak için ASP.NET MVC 3 RC Web Platformu yükleyicisi aracılığıyla yükleyin veya [tek başına yükleyici](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="21945-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="21945-223">Sorun: akışları ya da bir proxy sunucu üzerinden diğer dış veri okuma</span><span class="sxs-lookup"><span data-stu-id="21945-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="21945-224">Site sunucusunu bir proxy sunucusu arkasında ise, Ara sunucu bilgileri yapılandırmanız gerekebilir *web.config* dosyasını, site dışında geldiği bilgileri okuyabilir.</span><span class="sxs-lookup"><span data-stu-id="21945-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="21945-225">Örneğin, kullanırsanız `ReCaptcha` yardımcı, yardımcı reCAPTCHA hizmetiyle iletişim kurar, ancak proxy sunucunuz tarafından engelleniyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="21945-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="21945-226">Benzer şekilde, Paket Yöneticisi tarafından kullanılan akış gibi ASP.NET Web sayfaları'nda kullanılan akışları proxy yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="21945-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="21945-227">Bir dış hizmet çalışmaya veya akış paketi ile çalışırken sorunlarla karşılaşırsanız, aşağıdaki öğeleri, uygulamanızın kök ile put *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="21945-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="21945-228">Bir proxy sunucusu yapılandırma hakkında daha fazla bilgi için bkz. [ &lt;proxy&gt; öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web sitesinde.</span><span class="sxs-lookup"><span data-stu-id="21945-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="21945-229">Sorun: .NET Framework sürüm 4 kaldırma Razor sözdizimi olan ASP.NET Web sayfaları devre dışı bırakır</span><span class="sxs-lookup"><span data-stu-id="21945-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="21945-230">.NET Framework sürüm 4 kaldırın ve daha sonra yeniden yüklerseniz, Razor sözdizimi olan ASP.NET Web sayfaları devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="21945-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="21945-231">İle sayfaları *.cshtml* uzantısı düzgün çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="21945-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="21945-232">ASP.NET Web sayfaları kaydeder derleme makinesi kök dizininde *web.config* dosyasını ve .NET Framework kaldırarak bu dosyayı kaldırır.</span><span class="sxs-lookup"><span data-stu-id="21945-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="21945-233">.NET Framework yeniden yapılandırma dosyasının yeni bir sürümü yükler, ancak başvuru için ASP.NET Web Pages derleme eklemez.</span><span class="sxs-lookup"><span data-stu-id="21945-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="21945-234">**Geçici çözüm** .NET Framework yeniden yükledikten sonra Razor sözdizimi olan ASP.NET Web sayfaları yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="21945-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="21945-235">Bu şu öğeye ekler *web.config* genellikle şu konumdadır makine kök dosyasında:</span><span class="sxs-lookup"><span data-stu-id="21945-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="21945-236">Sorun: IIS 7 ya da IIS 7.5.cshtml/.vbhtml dosyada uzantısız URL'leri bulmaz</span><span class="sxs-lookup"><span data-stu-id="21945-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="21945-237">IIS 7 veya IIS 7.5, aşağıdaki gibi bir URL isteklerle sahip sayfalar bulmak mümkün değildir *.cshtml* veya *.vbhtml* uzantısı:</span><span class="sxs-lookup"><span data-stu-id="21945-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="21945-238">URL yeniden yazma varsayılan olarak IIS 7 veya IIS 7.5 için etkin olmadığından, sorun ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="21945-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="21945-239">IIS Express kullanarak yerel olarak test ederken sorun görmüyorsanız, ancak Web sitenizi barındıran bir Web sitesine dağıttığınızda deneyimi, denetçilerinde bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="21945-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="21945-240">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="21945-241">Sunucu bilgisayarı üzerinde denetime sahip olursunuz, sunucu bilgisayarda açıklanan güncelleştirmeyi yükleyin. [etkinleştirir işlemek için IIS 7.0 veya IIS 7.5 işleyicilerinin URL'leri istekleri belirli bir nokta ile bitmeyen bir güncelleştirme kullanılabilir](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="21945-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="21945-242">Sunucu bilgisayarı üzerinde denetim yoksa (örneğin, bir barındırma Web sitesine dağıtıyorsanız), Web sitenizin ekleyin *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="21945-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="21945-243">Sorun: SQL Server yüklü Compact sahip olmayan bir bilgisayara uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="21945-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="21945-244">SQL Server Compact veritabanı içeren uygulamalar, SQL Server Compact değil yüklü olduğu bir bilgisayarda çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21945-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="21945-245">Microsoft WebMatrix 1.0 otomatik olarak bu ikili dosyalar, kopyalar ve uygun gerçekleştirir *web.config* dosya dönüşümler.</span><span class="sxs-lookup"><span data-stu-id="21945-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="21945-246">**Geçici çözüm** bu dosyaları kopyalayın ve gerekirse *web.config* dosya değişikliklerini el ile şunları yapın:</span><span class="sxs-lookup"><span data-stu-id="21945-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="21945-247">Veritabanı altyapısı derlemeleri kopyalamak *Bin* uygulamanın hedef bilgisayardaki klasör (ve klasörleri):</span><span class="sxs-lookup"><span data-stu-id="21945-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="21945-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="21945-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="21945-249">**için** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="21945-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="21945-250">Kopyalama <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>için</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="21945-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="21945-251">Kopyalama <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>için</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="21945-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="21945-252">Web sitesinin kök klasöründeki oluşturun veya açın bir *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="21945-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="21945-253">(WebMatrix 1. 0'da, bu dosya türü tıklarsanız kullanılabilir **tüm** içinde **bir dosya türünü seçin** iletişim kutusu.)</span><span class="sxs-lookup"><span data-stu-id="21945-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="21945-254">Bir alt öğesi olarak aşağıdaki öğeyi ekleyin `<configuration>` öğesi (değilken `<system.web>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="21945-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="21945-255">Sorun: Visual Basic'te Medium Trust ile de "Veritabanı" ve "WebGrid" Yardımcıları çalışmıyor</span><span class="sxs-lookup"><span data-stu-id="21945-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="21945-256">Visual Basic kullanıyorsanız (oluşturma *.vbhtml* dosyaları), `Database` ve `WebGrid` uygulama Medium Trust kullanmak üzere ayarlanmışsa Yardımcıları çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="21945-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="21945-257">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-257">**Workaround**</span></span>  
> <span data-ttu-id="21945-258">Visual Studio 2010 kullanıyorsanız, Service Pack 1 sürüm yükleyerek bu sorunu çözebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21945-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="21945-259">SP1 sürümüne ait son sürüm kullanılabilir oluncaya kadar SP1'den Beta sürümü indirebilirsiniz [Microsoft Visual Studio 2010 Service Pack 1 Beta'ya](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Microsoft Download Center sayfasında.</span><span class="sxs-lookup"><span data-stu-id="21945-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="21945-260">Bunun pratik olmadığı veya Visual Studio 2010 kullanmazsanız, geçici olarak tam güven kullanmak için uygulamayı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="21945-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="21945-261">Sorun: "ApplicationPart" kaynakları harici olarak erişilebilen</span><span class="sxs-lookup"><span data-stu-id="21945-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="21945-262">Bir derlemeyi öğesinden türetilen nesneler içeriyorsa `ApplicationPart` sınıfı, derlemenin kaynakları tarafından kullanıma sunulan `ResourceRouteHandler` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="21945-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="21945-263">Örneğin, aşağıdaki URL'ye göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="21945-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="21945-264">Bu istek kaynak dizeleri tüm indirmeleri *System.Web.WebPages.Administration.dll* derleme.</span><span class="sxs-lookup"><span data-stu-id="21945-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="21945-265">Tüm katıştırılmış kaynaklar (olanlar statik içeriği olarak sunulmasını amaçlanmayan) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="21945-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="21945-266">Katıştırılmış kaynakları hassas bilgileri içeriyorsa, bu bir güvenlik riski temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="21945-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="21945-267">**Geçici çözüm** </span><span class="sxs-lookup"><span data-stu-id="21945-267">**Workaround** </span></span>  
> <span data-ttu-id="21945-268">Oluşturursanız, bir **ApplicationPart** nesne, gömülü kaynaklar ile ilişkili olduğundan emin olun **ApplicationPart** nesnenin derleme hassas bilgileri içermez.</span><span class="sxs-lookup"><span data-stu-id="21945-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="21945-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="21945-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="21945-270">WebMatrix için yükleme sorunları hakkında ek bilgi için bkz. [WebMatrix yükleme sorunlarını](#Known_Issues_Installation) bu belgenin daha öncesinde.</span><span class="sxs-lookup"><span data-stu-id="21945-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="21945-271">Belgenin bu bölümü WebMatrix geliştirme ortamı için bilinen sorunlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="21945-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="21945-272">Sorun: Değişiklikler kullanıcı adı veya parola web.config dosyasındaki veritabanı bağlantı dizesi veritabanı çalışma alanında yansıtılmaz</span><span class="sxs-lookup"><span data-stu-id="21945-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="21945-273">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="21945-274">İçinde *web.config* dosya, bağlantı dizesinde veritabanı adını değiştirin (örneğin, "1" ekleyin).</span><span class="sxs-lookup"><span data-stu-id="21945-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="21945-275">Kaydet *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="21945-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="21945-276">Tıklayın **veritabanları** ve yenileyin.</span><span class="sxs-lookup"><span data-stu-id="21945-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="21945-277">Bağlantı dizesinde veritabanının adını değiştirmek *web.config* dosyasını yeniden özgün veritabanı adı.</span><span class="sxs-lookup"><span data-stu-id="21945-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="21945-278">Kaydet *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="21945-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="21945-279">Tıklayın **veritabanları** ve yenileyin.</span><span class="sxs-lookup"><span data-stu-id="21945-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="21945-280">Sorun: WebMatrix tarafından oluşturulan klasörler silinemiyor</span><span class="sxs-lookup"><span data-stu-id="21945-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="21945-281">WebMatrix yükseltilmiş izinlerle çalışıyorsa (diğer bir deyişle, WebMatrix kullanarak başlattığınız **yönetici olarak çalıştır** Windows seçeneği), Windows Gezgini'ni kullanarak WebMatrix tarafından oluşturulan klasörlere silinemiyor.</span><span class="sxs-lookup"><span data-stu-id="21945-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="21945-282">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-282">**Workaround**</span></span>  
> <span data-ttu-id="21945-283">Yükseltilmiş izinlerle Windows Explorer'ı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="21945-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="21945-284">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="21945-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="21945-285">Windows içinde tıklayın **Başlat**.</span><span class="sxs-lookup"><span data-stu-id="21945-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="21945-286">"Windows Gezgini" girin ve girişini sağ tıklatın **Windows Explorer**.</span><span class="sxs-lookup"><span data-stu-id="21945-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="21945-287">Tıklayın **yönetici olarak çalıştır**.</span><span class="sxs-lookup"><span data-stu-id="21945-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="21945-288">Ardından, klasörler de silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21945-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="21945-289">Sorun: Ayrıcalık gerektiren belirli görevleri gerçekleştirmek WebMatrix 1.0 alamıyor</span><span class="sxs-lookup"><span data-stu-id="21945-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="21945-290">WebMatrix 1.0 aşağıdaki durumlarda ek bileşenler yükleme gibi yetki yükseltmesi gerektiren bazı görevleri gerçekleştirmek üzere yüklenemiyor:</span><span class="sxs-lookup"><span data-stu-id="21945-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="21945-291">Windows Vista veya Windows 7'de, yönetici ayrıcalıklarına sahip olmayan bir hesapla oturum günlüğe kaydedilir ve kullanıcı hesabı denetimi (UAC) devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="21945-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="21945-292">Microsoft Windows XP veya Microsoft Windows Server 2003 kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="21945-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="21945-293">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-293">**Workaround**</span></span>  
> <span data-ttu-id="21945-294">Çoğu görevi WebMatrix 1.0 Yönetim iznini gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="21945-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="21945-295">Olmayanlar için yönetici olarak işlemi gerçekleştirebilir, veya bu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="21945-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="21945-296">UAC Windows Vista veya Windows 7'de etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="21945-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="21945-297">Windows XP'de, kullanıcı Administrators güvenlik grubuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="21945-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="21945-298">Sorun: "Web Galeri'den Site" devre dışı bırakıldı</span><span class="sxs-lookup"><span data-stu-id="21945-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="21945-299">**Web Galerisi sitesinden** Web Platformu yükleyicisi 3.0 yüklü değilse seçeneği devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="21945-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="21945-300">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-300">**Workaround**</span></span>  
> <span data-ttu-id="21945-301">Yükleme [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="21945-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="21945-302">Sorun: Google Chrome çalıştırma seçeneği olarak kullanılabilir değil</span><span class="sxs-lookup"><span data-stu-id="21945-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="21945-303">Google Chrome, tarayıcılar altında listesinde görüntülenmez **çalıştırma** üzerinde **giriş** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="21945-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="21945-304">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-304">**Workaround**</span></span>  
> <span data-ttu-id="21945-305">Google Chrome'nün bazı sürümleri kendilerini doğru Windows varsayılan programlar özelliğiyle kaydetmeyin.</span><span class="sxs-lookup"><span data-stu-id="21945-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="21945-306">Geçici bir çözüm olarak, Google Chrome Başlat'a tıklayın *özelleştirme ve denetim Google Chrome* menüsünde tıklatın *seçenekleri*ve ardından *yapma Google Chrome varsayılan tarayıcımda*.</span><span class="sxs-lookup"><span data-stu-id="21945-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="21945-307">Sorun: "Yabancı anahtarı" iletişim kutusunda birincil bir anahtar girilmesi izin vermez</span><span class="sxs-lookup"><span data-stu-id="21945-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="21945-308">**Yabancı anahtar** iletişim kutusu izin vermemektedir birincil anahtar tablosunda birincil anahtar adı girin.</span><span class="sxs-lookup"><span data-stu-id="21945-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="21945-309">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-309">**Workaround**</span></span>  
> <span data-ttu-id="21945-310">Bu kasıtlıdır.</span><span class="sxs-lookup"><span data-stu-id="21945-310">This is intentional.</span></span> <span data-ttu-id="21945-311">Birincil anahtar tablosundaki birincil anahtarın adını girmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="21945-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="21945-312">Sorun: IntelliSense Webmatrix'te Razor sözdizimi, C# veya Visual Basic için kullanılabilir değil</span><span class="sxs-lookup"><span data-stu-id="21945-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="21945-313">IntelliSense WebMatrix, HTML ve CSS için desteklenir.</span><span class="sxs-lookup"><span data-stu-id="21945-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="21945-314">Ancak, diğer diller için kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="21945-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="21945-315">**Geçici çözüm** </span><span class="sxs-lookup"><span data-stu-id="21945-315">**Workaround** </span></span>  
> <span data-ttu-id="21945-316">Yok.</span><span class="sxs-lookup"><span data-stu-id="21945-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="21945-317">Sorun: IntelliSense için HTML ve CSS bağlamsal uygun olmayan öğeler önerir.</span><span class="sxs-lookup"><span data-stu-id="21945-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="21945-318">WebMatrix işaretlemesi için IntelliSense'i destekler HTML kullanarak [XHTML 1.0 geçiş şema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) ve CSS kullanarak [CSS 2.1 şema](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="21945-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="21945-319">IntelliSense bu belirli şemaları temel aldığından, bazı etiketler, öznitelik veya özellikleri geçerli sayfa veya stil tanımı için uygun olmayan önerilen.</span><span class="sxs-lookup"><span data-stu-id="21945-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="21945-320">HTML için hatalı biçimlendirilmiş XHTML (örneğin, etiketleri kapalı) olarak yorumlanabilir içerik beklenmeyen önerileri için de açabilir.</span><span class="sxs-lookup"><span data-stu-id="21945-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="21945-321">Bu sorun, tamamlanmamış bir etiketin içine ekleme noktasını ise daha belirgin olabilir; Bu durumda, IntelliSense bir yeni etiketler açma önerin veya yanlış diğer öneriler sunar.</span><span class="sxs-lookup"><span data-stu-id="21945-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="21945-322">**Geçici çözüm** </span><span class="sxs-lookup"><span data-stu-id="21945-322">**Workaround** </span></span>  
> <span data-ttu-id="21945-323">HTML için doğru biçimlendirilmiş ve eksiksiz bir XHTML sayfasında çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="21945-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="21945-324">CSS için geçici çözüm yoktur.</span><span class="sxs-lookup"><span data-stu-id="21945-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="21945-325">Sorun: siz yazarken IntelliSense çağrılır</span><span class="sxs-lookup"><span data-stu-id="21945-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="21945-326">HTML veya CSS Düzenleyicisi'nde giriliyor gibi zamanlarda, IntelliSense çağrılmasına değil.</span><span class="sxs-lookup"><span data-stu-id="21945-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="21945-327">Özellikle, ekleme noktasını başka bir öğenin yanında doğrudan veya bir dosya sonunda olduğunda bu gerçekleşebilir.</span><span class="sxs-lookup"><span data-stu-id="21945-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="21945-328">**Geçici çözüm** </span><span class="sxs-lookup"><span data-stu-id="21945-328">**Workaround** </span></span>  
> <span data-ttu-id="21945-329">Ekleme noktası etrafındaki boşluk olduğunu ve ekleme noktasını dosya sonunda değil emin olun.</span><span class="sxs-lookup"><span data-stu-id="21945-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="21945-330">Ctrl + Boşluk tuşlarına basarak IntelliSense el ile de çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21945-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="21945-331">Sorun: Kullanıcı Arabirimi IntelliSense devre dışı bırakmak için kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="21945-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="21945-332">WebMatrix 1.0 hiçbir kullanıcı Arabirimi veya hareket IntelliSense devre dışı bırakmak için sağlar.</span><span class="sxs-lookup"><span data-stu-id="21945-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="21945-333">**Geçici çözüm** </span><span class="sxs-lookup"><span data-stu-id="21945-333">**Workaround** </span></span>  
> <span data-ttu-id="21945-334">WebMatrix IntelliSense devre dışı bırakan bir anahtar içerir aşağıdaki komutu kullanarak başlatın:</span><span class="sxs-lookup"><span data-stu-id="21945-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="21945-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="21945-335">IIS Express</span></span>

<span data-ttu-id="21945-336">IIS Express, aşağıdaki URL'de kullanılabilir olduğundan, kendi Benioku dosyası vardır:</span><span class="sxs-lookup"><span data-stu-id="21945-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="21945-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409</span><span class="sxs-lookup"><span data-stu-id="21945-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="21945-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="21945-338">SQL Server Compact</span></span>

<span data-ttu-id="21945-339">SQL Server Compact, aşağıdaki URL'de kullanılabilir olduğundan, kendi Benioku dosyası vardır:</span><span class="sxs-lookup"><span data-stu-id="21945-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="21945-340">WebMatrix bir parçası olarak SQL Server Compact yükleme ilgili sorunlar hakkında daha fazla bilgi için bkz. [WebMatrix yükleme sorunlarını](#Known_Issues_Installation) bu belgenin daha öncesinde.</span><span class="sxs-lookup"><span data-stu-id="21945-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="21945-341">Uygulamaları yükleme</span><span class="sxs-lookup"><span data-stu-id="21945-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="21945-342">Sorun: kullanıcının Belgeler klasörünü bir ağ paylaşımına yönlendirilir, bir uygulamanın yüklenmesi bir uzun zaman alabilir</span><span class="sxs-lookup"><span data-stu-id="21945-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="21945-343">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-343">**Workaround**</span></span>  
> <span data-ttu-id="21945-344">Yok.</span><span class="sxs-lookup"><span data-stu-id="21945-344">None.</span></span> <span data-ttu-id="21945-345">Uygulama yüklemek için biraz sürebilir ancak düzgün yüklenecektir.</span><span class="sxs-lookup"><span data-stu-id="21945-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="21945-346">Uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="21945-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="21945-347">SQL Compact veritabanını yayımlarken sorunu: gerekli"izinleri alınamıyor" hatası</span><span class="sxs-lookup"><span data-stu-id="21945-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="21945-348">WebMatrix, bir orta güven yapılandırması ile .NET Framework sürüm 3.5 çalıştıran bir sunucuda SQL Server Compact için destekleyici ikili dosyaları dağıtma tam desteklemez.</span><span class="sxs-lookup"><span data-stu-id="21945-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="21945-349">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-349">**Workaround**</span></span>  
> <span data-ttu-id="21945-350">.NET Framework 4 sunucusuna yüklemek için tercih edilen çözüm olabilir.</span><span class="sxs-lookup"><span data-stu-id="21945-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="21945-351">Alternatif olarak, aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="21945-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="21945-352">Aşağıdaki öğeleri ekleyin `SecurityClasses` konusundaki *Web\_MediumTrust.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="21945-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="21945-353">Yeni bir izin kümesi oluşturma *Web\_MediumTrust.config* aşağıdaki gerekli izinlere sahip dosya:</span><span class="sxs-lookup"><span data-stu-id="21945-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="21945-354">İzin şu öğeleri koyarak SQL Server Compact kümesine uygulama *Web\_MediumTrust.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="21945-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="21945-355">Sorun: Bir "Hizmet kullanılamıyor" hatası galeri ve PhpBB web uygulamaları, yayımladıktan sonra görüntüler.</span><span class="sxs-lookup"><span data-stu-id="21945-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="21945-356">Bazı durumlarda, bir "Hizmet kullanılamıyor" hatası olan bir uygulama yayımlama neden olur.</span><span class="sxs-lookup"><span data-stu-id="21945-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="21945-357">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-357">**Workaround**</span></span>  
> <span data-ttu-id="21945-358">Webmatrix'te, ters eğik çizgi ekleyin (\) sunucu adı sonuna **yayımlama ayarları** penceresi ve sonra uygulamayı yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="21945-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="21945-359">Sorun: Moodle Web sitesi düzenini ve bağlantıları yayımladıktan sonra bozuk</span><span class="sxs-lookup"><span data-stu-id="21945-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="21945-360">Moodle uygulaması yayımladıktan sonra uygulama düzgün çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="21945-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="21945-361">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-361">**Workaround**</span></span>  
> <span data-ttu-id="21945-362">Webmatrix'te, sonuna bir eğik çizgi (/) ekleyin **Site adı** alanındaki **yayımlama ayarları** penceresi ve sonra uygulamayı yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="21945-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="21945-363">Sorun: nopCommerce yayımlama bir veritabanı hatasıyla başarısız oluyor</span><span class="sxs-lookup"><span data-stu-id="21945-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="21945-364">Yayımlama nopCommerce başarısız olur ve bir veritabanı hatası gibi raporları "nop Ekle\_günlüğü tablosu başarısız oldu."</span><span class="sxs-lookup"><span data-stu-id="21945-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="21945-365">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="21945-366">Webmatrix'te, tıklayın **çalıştırma** nopCommerce yerel olarak başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="21945-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="21945-367">İçinde Yönetim sayfasını açın.</span><span class="sxs-lookup"><span data-stu-id="21945-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="21945-368">Tıklayın **sistem** menüsü.</span><span class="sxs-lookup"><span data-stu-id="21945-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="21945-369">Tıklayın **günlük** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="21945-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="21945-370">Tıklayın **Günlüğü Temizle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="21945-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="21945-371">NopCommerce yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="21945-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="21945-372">Sorun: Silverstripe CMS, yayımlanmış bir siteyi yüklediğinizde bir "HTTP 500 PHP FCGI Error" görüntüler</span><span class="sxs-lookup"><span data-stu-id="21945-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="21945-373">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-373">**Workaround**</span></span>  
> <span data-ttu-id="21945-374">Tıkladıktan sonra **indirme sitesinde yayımlanan**, atlama `silverstripe-cache/manifest_main` içinde **yayımlama önizlemesi**.</span><span class="sxs-lookup"><span data-stu-id="21945-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="21945-375">Bu dosya tarafından önbelleğe alma işlemleri için kullanılır ve her bilgisayar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="21945-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="21945-376">Sorun: "'/' uygulamasında sunucu hatası" alt görüntüler yayımlanmış bir siteyi yüklediğiniz zaman</span><span class="sxs-lookup"><span data-stu-id="21945-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="21945-377">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-377">**Workaround**</span></span>  
> <span data-ttu-id="21945-378">Sitenin açın *web.config* dosya ve kullanıcı kimliği ve veritabanı bağlantı dizesine parolayı ("sa" kimlik bilgileri) SQL Server yönetici kimlik bilgileriyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="21945-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="21945-379">Alternatif olarak, kullanıcı hesabı ile oturum vermek için bu adımları izleyin `db_owner` izinleri:</span><span class="sxs-lookup"><span data-stu-id="21945-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="21945-380">SQL Server Management Studio kullanarak Web Platformu Yükleyicisi'ni yükleyin.</span><span class="sxs-lookup"><span data-stu-id="21945-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="21945-381">Yerel SQL Server Express örneğine bağlanın (varsayılan olarak, `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="21945-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="21945-382">Tıklayın **veritabanları** &gt; *[localSubtextDatabase]* &gt; **güvenlik** &gt; **kullanıcılar** &gt; *[localSubtextUser*] (varsayılan değer `subtextuser`], sağ tıklatın seçeneğine tıklayıp **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="21945-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="21945-383">Seçin **db\_sahibi** rol üyeliği bölümünde.</span><span class="sxs-lookup"><span data-stu-id="21945-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="21945-384">Sorun: Site "Hedef URL'si" alanında http:// veya https:// ile eklenmedi, yayımladıktan sonra çalışmayabilir</span><span class="sxs-lookup"><span data-stu-id="21945-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="21945-385">İçinde **yayımlama ayarları** iletişim kutusunda, hedef URL ile başlamıyorsa `http://` veya `https://`, site dağıtımdan sonra çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="21945-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="21945-386">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-386">**Workaround**</span></span>  
> <span data-ttu-id="21945-387">Bir siteyi hedef URL yayımlamadan önce emin **yayımlama ayarları** iletişim kutusu ile başlayan `http://` veya `https://`.</span><span class="sxs-lookup"><span data-stu-id="21945-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="21945-388">Sorun: bir MySQL veritabanı yayımlama hatası ile "veritabanı yayımlanamadı. başarısız</span><span class="sxs-lookup"><span data-stu-id="21945-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="21945-389">Uzak veritabanı betiği çalıştırırsanız bu durum oluşabilir."</span><span class="sxs-lookup"><span data-stu-id="21945-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="21945-390">Hata, bir dizi nedenden ötürü ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="21945-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="21945-391">Bu hatayı görebilirsiniz. bir veritabanı betik, tek tırnak karakterini (') içerir ve hedef MySQL veritabanının varsayılan karakter kümesini UTF-8'e değil nedenidir.</span><span class="sxs-lookup"><span data-stu-id="21945-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="21945-392">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-392">**Workaround**</span></span>  
> <span data-ttu-id="21945-393">Uzak bir MySQL veritabanı için UTF-8'e ayarlanmış varsayılan karakter kümesi.</span><span class="sxs-lookup"><span data-stu-id="21945-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="21945-394">Sorun: Bazı bağlantılar yayımlama veya site yükleme sonrasında DotNetNuke görünür değildir</span><span class="sxs-lookup"><span data-stu-id="21945-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="21945-395">Yayımlama veya DotNetNuke site indirin, sitesinde görünen yeni bağlantılar almak için önbelleğini temizlemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="21945-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="21945-396">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="21945-397">"Ana" oturum açın.</span><span class="sxs-lookup"><span data-stu-id="21945-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="21945-398">Ana menüye gidip seçin **konak ayarları**.</span><span class="sxs-lookup"><span data-stu-id="21945-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="21945-399">Kaydırma aşağı ve altında **Gelişmiş ayarlar**, genişletme **performans ayarları**.</span><span class="sxs-lookup"><span data-stu-id="21945-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="21945-400">Tıklayın **Önbelleği Temizle** sayfaları için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="21945-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="21945-401">Sayfanın alt kısmına gidin ve uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="21945-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="21945-402">Sorun: yayımlanmış bir siteyi yükledikten sonra bazı bağlantılar AtomSite bozuk</span><span class="sxs-lookup"><span data-stu-id="21945-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="21945-403">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-403">**Workaround**</span></span>  
> <span data-ttu-id="21945-404">İçinde *service.config* dosyası *users.config* dosyasını ve tüm *.xml* dosyaları, URL dizesini değiştirin (örneğin, `http://myhost.com/atomsite`) yerel bir (örneğin, `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="21945-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="21945-405">Sorun: Yayımlama ve bir veritabanı hatası raporlamak MySQL tabanlı uygulamalar WordPress gibi başarısız.</span><span class="sxs-lookup"><span data-stu-id="21945-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="21945-406">Varsayılan olarak, WebMatrix, MySQL ile UTF-8 karakter kümesini yükler.</span><span class="sxs-lookup"><span data-stu-id="21945-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="21945-407">MySQL, kendi yüklemeniz ve karakter kümesini UTF-8 değilse (örneğin, Latin1 olduğu), veritabanları için yayımlama işlemi başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="21945-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="21945-408">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="21945-409">Karakter kümesi için MySQL UTF-8 olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="21945-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="21945-410">(Ayrıntılar için bkz [sunucu karakter kümesi ve harmanlama](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) MySQL Web sitesinde.)</span><span class="sxs-lookup"><span data-stu-id="21945-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="21945-411">Uygulamayı yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="21945-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="21945-412">Uygulamayı yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="21945-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="21945-413">Sorun: "İndirme sitesinde yayımlanan" kurulumun tarayıcı tabanlı uygulamalar için başarısız oluyor</span><span class="sxs-lookup"><span data-stu-id="21945-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="21945-414">Bazı uygulamalar (örneğin, Kentico CMS), bunları bir veritabanı oluşturma gibi yükleme sonrası kurulumu gerçekleştirmek için tarayıcıda başlatmak gerektirir.</span><span class="sxs-lookup"><span data-stu-id="21945-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="21945-415">Tarayıcı tabanlı Kurulumu Tamamlanıyor olmadan bu gibi bir uygulama yayımladığınızda, aynı sitede bir Uzak sunucudan indirme girişimi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="21945-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="21945-416">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-416">**Workaround**</span></span>  
> <span data-ttu-id="21945-417">Tarayıcı tabanlı Kurulum, site yayımlamadan önce tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="21945-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="21945-418">Sorun: "İndirme sitesinde yayımlanan" ile bir veritabanı hatası DotNetNuke ve Kooboo CMS başarısız</span><span class="sxs-lookup"><span data-stu-id="21945-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="21945-419">Bir sunucudan bir uygulamanın yüklemeye ve veritabanı bağlantı dizesine, yönetici kimlik bilgilerine sahip **yayımlama ayarları** iletişim kutusunda, yayımlama günlüğünde şu hatayı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="21945-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="21945-420">**Geçici çözüm**</span><span class="sxs-lookup"><span data-stu-id="21945-420">**Workaround**</span></span>  
> <span data-ttu-id="21945-421">Mümkünse, site yeniden yayımlamanız (veya yayımlanan sahip) veritabanı için yönetici olmayan kimlik bilgilerini kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="21945-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="21945-422">Daha Fazla Bilgi İçin</span><span class="sxs-lookup"><span data-stu-id="21945-422">For More Information</span></span>

<span data-ttu-id="21945-423">WebMatrix 1.0 hakkında daha fazla bilgi için aşağıdaki Web sitelerine bakın:</span><span class="sxs-lookup"><span data-stu-id="21945-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="21945-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="21945-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="21945-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="21945-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="21945-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="21945-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="21945-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="21945-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="21945-428">Tüm hakları saklıdır.</span><span class="sxs-lookup"><span data-stu-id="21945-428">All Rights Reserved.</span></span> <span data-ttu-id="21945-429">[Kullanım koşullarını](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="21945-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
