---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: bir SQL Server veritabanı güncelleştirmesi - 11 / 12 dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8cc072c631900937f31c8f2637869b53d46cf54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="0830a-103">SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: bir SQL Server veritabanı güncelleştirmesi - 11 / 12 dağıtma</span><span class="sxs-lookup"><span data-stu-id="0830a-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="0830a-104">by [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0830a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="0830a-105">Başlangıç projesi indirme</span><span class="sxs-lookup"><span data-stu-id="0830a-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="0830a-106">Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi.</span><span class="sxs-lookup"><span data-stu-id="0830a-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="0830a-107">Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0830a-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="0830a-108">Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="0830a-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="0830a-109">Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Windows Azure Web sitelerine dağıtma gösterir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0830a-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="0830a-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="0830a-110">Overview</span></span>

<span data-ttu-id="0830a-111">Bu öğreticide bir veritabanı güncelleştirmenin tam bir SQL Server veritabanına nasıl dağıtılacağı gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0830a-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="0830a-112">Code First Migrations veritabanını güncelleştirme tüm işini yaptığından işlemi için SQL Server Compact içinde yaptıklarınızın için neredeyse aynıdır [veritabanı güncelleştirme dağıtımı](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) Öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="0830a-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="0830a-113">Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="0830a-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="0830a-114">Bir tablo için yeni bir sütun ekleme</span><span class="sxs-lookup"><span data-stu-id="0830a-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="0830a-115">Öğreticinin bu bölümünde bir veritabanı değiştirin ve ardından bunları Visual Studio'da test ve üretim ortamlarına dağıtmadan hazırlığı test karşılık gelen kod değişiklikleri hale getireceğiz.</span><span class="sxs-lookup"><span data-stu-id="0830a-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="0830a-116">Değişiklik ekleme içerir bir `OfficeHours` sütuna `Instructor` varlık ve yeni bilgilerini görüntüleme **Eğitmen** web sayfası.</span><span class="sxs-lookup"><span data-stu-id="0830a-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="0830a-117">ContosoUniversity.DAL projeyi açın *Instructor.cs* ve aşağıdaki özellik arasında ekleme `HireDate` ve `Courses` özellikleri:</span><span class="sxs-lookup"><span data-stu-id="0830a-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="0830a-118">Başlatıcı sınıfı güncelleştirin, böylece test verileri olan yeni bir sütun çekirdeğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0830a-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="0830a-119">Açık *Migrations\Configuration.cs* ve başlar kod bloğunu Değiştir `var instructors = new List<Instructor>` yeni bir sütun içeren aşağıdaki kod bloğu ile:</span><span class="sxs-lookup"><span data-stu-id="0830a-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="0830a-120">ContosoUniversity projeyi açın *Instructors.aspx* ve yalnızca kapatmadan önce ofis saatleri için yeni bir şablon alan ekleme `</Columns>` ilk etiketinde `GridView` denetimi:</span><span class="sxs-lookup"><span data-stu-id="0830a-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="0830a-121">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0830a-121">Build the solution.</span></span>

<span data-ttu-id="0830a-122">Açık **Paket Yöneticisi Konsolu** penceresi ve select ContosoUniversity.DAL olarak **varsayılan proje**.</span><span class="sxs-lookup"><span data-stu-id="0830a-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="0830a-123">Aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="0830a-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="0830a-124">Uygulamayı çalıştırın ve seçin **Eğitmen** sayfası.</span><span class="sxs-lookup"><span data-stu-id="0830a-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="0830a-125">Sayfa Entity Framework veritabanı yeniden oluşturur ve test verilerle çekirdeğini oluşturur, oluştuğundan normalden biraz daha uzun sürer.</span><span class="sxs-lookup"><span data-stu-id="0830a-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="0830a-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0830a-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="0830a-127">Veritabanı Güncelleştirme Test ortamına dağıtma</span><span class="sxs-lookup"><span data-stu-id="0830a-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="0830a-128">Code First Migrations kullandığınızda, SQL Server veritabanı değişikliği dağıtma yöntemi SQL Server Compact aynıdır.</span><span class="sxs-lookup"><span data-stu-id="0830a-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="0830a-129">Bununla birlikte, Test değiştirmek zorunda bunu hala SQL Server Compact'dan SQL Server'a geçirmek için ayarlandığından yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="0830a-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="0830a-130">İlk adım, önceki öğreticide oluşturduğunuz bağlantı dize dönüşümleri kaldırmaktır.</span><span class="sxs-lookup"><span data-stu-id="0830a-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="0830a-131">Bağlantı dize dönüşümleri yayımlama profilinde belirttiğiniz çünkü yapılandırdığınız önce yaptığınız gibi bunlar artık gerekli **SQL Paketle/Yayımla** sekmesini geçiş SQL Server için.</span><span class="sxs-lookup"><span data-stu-id="0830a-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="0830a-132">Açık *Web.Test.config* dosya ve kaldırma `connectionStrings` öğesi.</span><span class="sxs-lookup"><span data-stu-id="0830a-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="0830a-133">Yalnızca kalan dönüşümünde *Web.Test.config* dosyasıdır için `Environment` değeri `appSettings` öğesi.</span><span class="sxs-lookup"><span data-stu-id="0830a-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="0830a-134">Şimdi sınama ortamında Yayımla ve yayımlama profili güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0830a-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="0830a-135">Açık **Web'i Yayımla** Sihirbazı'nı ve ardından anahtara **profil** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="0830a-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="0830a-136">Seçin **Test** yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="0830a-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="0830a-137">Seçin **ayarları** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="0830a-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="0830a-138">Tıklatın **geliştirmeleri yayımlama yeni veritabanı etkinleştirme**.</span><span class="sxs-lookup"><span data-stu-id="0830a-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="0830a-139">Bağlantı dizesi kutusunda **SchoolContext**, içinde kullanılan aynı değeri girin *Web.Test.config* önceki öğreticideki dönüşüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="0830a-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="0830a-140">Seçin **yürütme önce kod uygulamalı geçişler (uygulama başlatılırken çalışır)**.</span><span class="sxs-lookup"><span data-stu-id="0830a-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="0830a-141">(Visual Studio, sürümünde etiketli onay kutusunun **geçerli Code First Migrations**.)</span><span class="sxs-lookup"><span data-stu-id="0830a-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="0830a-142">Bağlantı dizesi kutusunda **DefaultConnection**, içinde kullanılan aynı değeri girin *Web.Test.config* önceki öğreticideki dönüşüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="0830a-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="0830a-143">Bırakın **güncelleştirme veritabanı** temizlendi.</span><span class="sxs-lookup"><span data-stu-id="0830a-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="0830a-144">Tıklatın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="0830a-144">Click **Publish**.</span></span>

<span data-ttu-id="0830a-145">Visual Studio test ortamına kod değişiklikleri dağıtır ve Contoso University giriş sayfasını tarayıcıda açılır.</span><span class="sxs-lookup"><span data-stu-id="0830a-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="0830a-146">Eğitmen sayfasını seçin.</span><span class="sxs-lookup"><span data-stu-id="0830a-146">Select the Instructors page.</span></span>

<span data-ttu-id="0830a-147">Uygulama bu sayfa çalıştığında, veritabanına erişmek çalışır.</span><span class="sxs-lookup"><span data-stu-id="0830a-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="0830a-148">Code First geçişleri veritabanı geçerli ve son geçiş henüz uygulanmamış olduğunu bulur olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="0830a-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="0830a-149">Code First geçişleri son geçiş uygular, çalışan `Seed` yöntemi ve sayfa çalıştıran genellikle.</span><span class="sxs-lookup"><span data-stu-id="0830a-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="0830a-150">Yeni bir ofis saatleri sütun köklü verilerle bakın.</span><span class="sxs-lookup"><span data-stu-id="0830a-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="0830a-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="0830a-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="0830a-152">Veritabanı Güncelleştirme üretim ortamına dağıtma</span><span class="sxs-lookup"><span data-stu-id="0830a-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="0830a-153">Üretim ortamı için yayımlama profili aynı zamanda değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="0830a-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="0830a-154">Bu durumda varolan profilini kaldırın ve güncelleştirilmiş .publishsettings dosyasını alarak yeni bir tane oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0830a-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="0830a-155">Güncelleştirilmiş dosya Cytanium SQL Server veritabanı için bağlantı dizesini içerir.</span><span class="sxs-lookup"><span data-stu-id="0830a-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="0830a-156">Test ortamına dağıttığınızda gördüğünüz gibi bağlantı dizesi dönüşümler artık ihtiyaç *Web.Production.config* dönüşüm dosyası.</span><span class="sxs-lookup"><span data-stu-id="0830a-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="0830a-157">Dosya ve kaldıran açık `connectionStrings` öğesi.</span><span class="sxs-lookup"><span data-stu-id="0830a-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="0830a-158">Kalan dönüşümleri içindir `Environment` değeri `appSettings` öğesi ve `location` Elmah hata raporlarını erişimi kısıtlayan öğesi.</span><span class="sxs-lookup"><span data-stu-id="0830a-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="0830a-159">Üretim için yeni bir yayımlama profili oluşturmadan önce daha önce yaptığınız gibi bir güncelleştirilmiş .publishsettings dosyasını karşıdan [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="0830a-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="0830a-160">(Cytanium Denetim Masası'nda tıklatın **Web siteleri**ve ardından **contosouniversity.com** Web sitesi.</span><span class="sxs-lookup"><span data-stu-id="0830a-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="0830a-161">Select **Web'de Yayımlama** sekmesini ve ardından **bu web sitesi için yayımlama profili indirme**.) Bu yapmakta olduğunuz .publishsettings dosyasını veritabanı bağlantı dizesinde alması nedenidir.</span><span class="sxs-lookup"><span data-stu-id="0830a-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="0830a-162">Bağlantı dizesi hala SQL Server Compact kullanmakta olduğunuz ve SQL Server veritabanı Cytanium henüz oluşturduğunuz çalıştırmadıysanız çünkü dosya karşıdan ilk kez kullanılabilir değildi.</span><span class="sxs-lookup"><span data-stu-id="0830a-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="0830a-163">Artık üretim ortamına yayınlama ve yayımlama profili güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0830a-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="0830a-164">Açık **Web'i Yayımla** Sihirbazı'nı ve ardından anahtara **profil** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="0830a-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="0830a-165">Tıklatın **profillerini yönetme**ve üretim profili silin.</span><span class="sxs-lookup"><span data-stu-id="0830a-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="0830a-166">Kapat **Web'i Yayımla** bu değişikliği kaydetmek için Sihirbazı.</span><span class="sxs-lookup"><span data-stu-id="0830a-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="0830a-167">Açık **Web'i Yayımla** Sihirbazı'nı yeniden ve ardından **alma**.</span><span class="sxs-lookup"><span data-stu-id="0830a-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="0830a-168">Üzerinde **bağlantı** sekmesinde, değiştirmek **hedef URL** geçici bir URL kullanıyorsanız, uygun değere.</span><span class="sxs-lookup"><span data-stu-id="0830a-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="0830a-169">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0830a-169">Click **Next**.</span></span>

<span data-ttu-id="0830a-170">Üzerinde **ayarları** sekmesini tıklatın, **geliştirmeleri yayımlama yeni veritabanı etkinleştirme**.</span><span class="sxs-lookup"><span data-stu-id="0830a-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="0830a-171">Bağlantı dizesi aşağı açılan listesinde **SchoolContext**, Cytanium bağlantı dizesi seçin.</span><span class="sxs-lookup"><span data-stu-id="0830a-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="0830a-173">Seçin **Code First yürütme geçişler (uygulama başlatılırken çalışır)**.</span><span class="sxs-lookup"><span data-stu-id="0830a-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="0830a-174">Bağlantı dizesi aşağı açılan listesinde **DefaultConnection**, Cytanium bağlantı dizesi seçin.</span><span class="sxs-lookup"><span data-stu-id="0830a-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="0830a-175">Seçin **profil** sekmesini tıklatın, **profillerini yönetme**ve profil "contosouniversity.com - Web Dağıtımı", "Üretim" yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="0830a-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="0830a-176">Değişikliği kaydetmek için yayımlama profili kapatın, sonra yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="0830a-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="0830a-177">Tıklatın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="0830a-177">Click **Publish**.</span></span> <span data-ttu-id="0830a-178">(Gerçek üretim Web sitesi için kopyaladığınız *uygulama\_offline.htm* üretim ve put, yayımlamadan önce proje klasörünüzdeki sonra kaldırmak, dağıtım tamamlandığında.)</span><span class="sxs-lookup"><span data-stu-id="0830a-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="0830a-179">Visual Studio test ortamına kod değişiklikleri dağıtır ve Contoso University giriş sayfasını tarayıcıda açılır.</span><span class="sxs-lookup"><span data-stu-id="0830a-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="0830a-180">Eğitmen sayfasını seçin.</span><span class="sxs-lookup"><span data-stu-id="0830a-180">Select the Instructors page.</span></span>

<span data-ttu-id="0830a-181">Code First geçişleri Test ortamında yaptığınız gibi veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="0830a-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="0830a-182">Yeni bir ofis saatleri sütun köklü verilerle bakın.</span><span class="sxs-lookup"><span data-stu-id="0830a-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="0830a-184">Artık bir SQL Server veritabanı kullanarak veritabanı değişikliği dahil bir uygulama güncelleştirmesi başarıyla dağıtmış olmanız.</span><span class="sxs-lookup"><span data-stu-id="0830a-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="0830a-185">Daha fazla bilgi</span><span class="sxs-lookup"><span data-stu-id="0830a-185">More Information</span></span>

<span data-ttu-id="0830a-186">Bu, bir üçüncü taraf barındırma sağlayıcısına ASP.NET web uygulaması dağıtma öğreticileri bu dizi tamamlar.</span><span class="sxs-lookup"><span data-stu-id="0830a-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="0830a-187">Herhangi biri bu öğreticileri kapsanan konular hakkında daha fazla bilgi için bkz: [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) MSDN web sitesinde.</span><span class="sxs-lookup"><span data-stu-id="0830a-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="0830a-188">Katkıda Bulunanlar</span><span class="sxs-lookup"><span data-stu-id="0830a-188">Acknowledgements</span></span>

<span data-ttu-id="0830a-189">Bu öğretici dizisinin içeriğe önemli ölçüde katkıda yapılan aşağıdaki kişilere teşekkür ederiz ister misiniz:</span><span class="sxs-lookup"><span data-stu-id="0830a-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="0830a-190">Adı Poblacion, MVP &amp; MCT, İspanya</span><span class="sxs-lookup"><span data-stu-id="0830a-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="0830a-191">Jarod Ferguson, veri Platform geliştirme MVP, Amerika Birleşik Devletleri</span><span class="sxs-lookup"><span data-stu-id="0830a-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="0830a-192">Harsh Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="0830a-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="0830a-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="0830a-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="0830a-194">Mike Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="0830a-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="0830a-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="0830a-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="0830a-196">Raffaele Rialdi, Italy</span><span class="sxs-lookup"><span data-stu-id="0830a-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="0830a-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="0830a-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="0830a-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="0830a-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="0830a-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="0830a-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="0830a-200">[Tan Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="0830a-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="0830a-201">Srđan Božović, Sırbistan</span><span class="sxs-lookup"><span data-stu-id="0830a-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="0830a-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="0830a-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0830a-203">[Önceki](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [sonraki](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="0830a-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
