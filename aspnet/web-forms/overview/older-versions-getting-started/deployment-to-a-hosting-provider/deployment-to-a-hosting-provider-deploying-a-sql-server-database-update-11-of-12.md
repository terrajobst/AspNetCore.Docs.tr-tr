---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: SQL Server veritabanı güncelleştirmesi - 11 / 12 dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816638"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="b2b4e-103">SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: SQL Server veritabanı güncelleştirmesi - 11 / 12 dağıtma</span><span class="sxs-lookup"><span data-stu-id="b2b4e-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="b2b4e-104">tarafından [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b2b4e-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="b2b4e-105">Başlangıç projesini indirin</span><span class="sxs-lookup"><span data-stu-id="b2b4e-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="b2b4e-106">Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="b2b4e-107">Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="b2b4e-108">Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="b2b4e-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="b2b4e-109">Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Windows Azure Web sitelerine dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b2b4e-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="b2b4e-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="b2b4e-110">Overview</span></span>

<span data-ttu-id="b2b4e-111">Bu öğreticide, tam bir SQL Server veritabanı için veritabanı güncelleştirmesi dağıtma işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="b2b4e-112">Code First Migrations veritabanı güncelleştirme tüm işler gerçekleştirdiğinden, işlem için SQL Server Compact'ta yaptıklarımız için neredeyse aynıdır [veritabanı güncelleştirmesi dağıtma](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="b2b4e-113">Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="b2b4e-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="b2b4e-114">Tabloya yeni bir sütun ekleme</span><span class="sxs-lookup"><span data-stu-id="b2b4e-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="b2b4e-115">Öğreticinin bu bölümünde veritabanını Değiştir ve ardından bunları Visual Studio'da test ve üretim ortamlarına dağıtmadan hazırlığı test karşılık gelen kod değişiklikleri yapacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="b2b4e-116">Değişiklik eklenmesini kapsar bir `OfficeHours` sütuna `Instructor` varlık ve yeni bilgileri görüntüleme **Eğitmenler** web sayfası.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="b2b4e-117">ContosoUniversity.DAL projeyi *Instructor.cs* ve arasında aşağıdaki özelliği ekleyin `HireDate` ve `Courses` özellikleri:</span><span class="sxs-lookup"><span data-stu-id="b2b4e-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="b2b4e-118">Test verileri ile yeni bir sütun çekirdeğini Başlatıcı sınıfı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="b2b4e-119">Açık *Migrations\Configuration.cs* ve başlayan kod bloğunu `var instructors = new List<Instructor>` yeni bir sütun içeren aşağıdaki kod bloğu ile:</span><span class="sxs-lookup"><span data-stu-id="b2b4e-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="b2b4e-120">ContosoUniversity projeyi *Instructors.aspx* ve kapatmadan önce yalnızca ofis saatleri için yeni bir şablon alan ekleme `</Columns>` ilk etiket `GridView` denetimi:</span><span class="sxs-lookup"><span data-stu-id="b2b4e-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="b2b4e-121">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-121">Build the solution.</span></span>

<span data-ttu-id="b2b4e-122">Açık **Paket Yöneticisi Konsolu** penceresi ve select ContosoUniversity.DAL olarak **varsayılan proje**.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="b2b4e-123">Aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="b2b4e-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="b2b4e-124">Uygulamayı çalıştırmak ve seçmek **Eğitmenler** sayfası.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="b2b4e-125">Sayfa yüklenemiyor çünkü Entity Framework veritabanını yeniden oluşturur ve test verileri ile çekirdeğini normalden biraz daha uzun sürer.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="b2b4e-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b2b4e-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="b2b4e-127">Test ortamına veritabanı güncelleştirmesi dağıtma</span><span class="sxs-lookup"><span data-stu-id="b2b4e-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="b2b4e-128">Code First Migrations'ı kullandığınızda, SQL Server veritabanı değişikliği dağıtmak için yöntemi SQL Server Compact aynıdır.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="b2b4e-129">Test değiştirmek zorunda ancak yine de SQL Server Compact'dan SQL Server'a geçirmek üzere kurulmamış için yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="b2b4e-130">İlk adım, önceki öğreticide oluşturduğunuz bağlantı dizesi dönüşümleri kaldırmaktır.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="b2b4e-131">Bağlantı dizesi dönüştürmeleri yayımlama profilinde belirttiğiniz çünkü yapılandırdığınız önce yaptığınız gibi bunlar artık gerekmeyen **SQL Paketle/Yayımla** SQL Server'a geçiş için sekmesinde.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="b2b4e-132">Açık *Web.Test.config* dosya ve kaldırma `connectionStrings` öğesi.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="b2b4e-133">Yalnızca kalan dönüşümünde *Web.Test.config* dosyasıdır için `Environment` değerini `appSettings` öğesi.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="b2b4e-134">Artık test ortamı için yayımlama profili ve yayımlama güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="b2b4e-135">Açık **Web'i Yayımla** Sihirbazı'nı ve ardından anahtara **profili** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="b2b4e-136">Seçin **Test** yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="b2b4e-137">Seçin **ayarları** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="b2b4e-138">Tıklayın **yeni veritabanı yayımlama iyileştirmelerini etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="b2b4e-139">Bağlantı dizesi kutusunda **SchoolContext**, içinde kullandığınız aynı değeri girin *Web.Test.config* önceki öğreticide dönüşüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="b2b4e-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="b2b4e-140">Seçin **yürütme Code First Migrations (uygulama başlatılırken çalışır)**.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="b2b4e-141">(, Visual Studio sürümünde etiketli onay kutusunun **uygulamak Code First Migrations**.)</span><span class="sxs-lookup"><span data-stu-id="b2b4e-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="b2b4e-142">Bağlantı dizesi kutusunda **DefaultConnection**, içinde kullandığınız aynı değeri girin *Web.Test.config* önceki öğreticide dönüşüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="b2b4e-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="b2b4e-143">Bırakın **veritabanını Güncelleştir** temizlenir.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="b2b4e-144">Tıklayın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-144">Click **Publish**.</span></span>

<span data-ttu-id="b2b4e-145">Visual Studio kod değişikliklerini test ortamı dağıtır ve tarayıcıya Contoso University giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="b2b4e-146">Eğitmenler sayfasını seçin.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-146">Select the Instructors page.</span></span>

<span data-ttu-id="b2b4e-147">Bu sayfa uygulama çalışırken, veritabanına erişmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="b2b4e-148">Code First geçişleri, veritabanı güncel ve en son geçiş henüz uygulanmamış olduğunu bulur denetler.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="b2b4e-149">Code First geçişleri son geçiş uygular, çalışan `Seed` yöntemi ve sayfa çalışır normalde.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="b2b4e-150">Yeni bir ofis saatleri sütun çekirdeği oluşturulmuş veri görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="b2b4e-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b2b4e-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="b2b4e-152">Veritabanı güncelleştirmesi üretim ortamına dağıtma</span><span class="sxs-lookup"><span data-stu-id="b2b4e-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="b2b4e-153">Üretim ortamı için yayımlama profili de değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="b2b4e-154">Bu durumda mevcut profilini kaldırın ve güncelleştirilmiş .publishsettings dosyasını içe tarafından yeni bir tane oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="b2b4e-155">Güncelleştirilmiş dosyayı Cytanium SQL Server veritabanı için bağlantı dizesini içerir.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="b2b4e-156">Test ortamı dağıttığınızda gördüğünüz gibi içindeki bağlantı dizesi dönüşümler artık ihtiyacınız *Web.Production.config* dönüşüm dosyası.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="b2b4e-157">Dosya ve kaldıran açık `connectionStrings` öğesi.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="b2b4e-158">Kalan dönüştürmeleri içindir `Environment` değerini `appSettings` öğesi ve `location` Elmah hata raporlarının erişimi kısıtlayan öğesi.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="b2b4e-159">Üretim için yeni bir yayımlama profili oluşturmadan önce daha önce yaptığınız gibi bir güncelleştirilmiş .publishsettings dosyasını indirmek [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="b2b4e-160">(Cytanium Denetim Masası'nda tıklatın **Web siteleri**ve ardından **contosouniversity.com** Web sitesi.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="b2b4e-161">Select **Web'de Yayımlama** sekmesine ve ardından **bu web sitesi için yayımlama profilini indirin**.) .Publishsettings dosyasındaki veritabanı bağlantı dizesini almak için bu gibi bir işlem yapmakta olduğunuz nedeni olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="b2b4e-162">Bağlantı dizesi yine de SQL Server Compact kullanıyor ve SQL Server veritabanı Cytanium henüz oluşturduğunuz yüklediniz çünkü dosya yüklediğiniz ilk kez kullanılabilir değildi.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="b2b4e-163">Artık üretim ortamına yayınlama ve yayımlama profili güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="b2b4e-164">Açık **Web'i Yayımla** Sihirbazı'nı ve ardından anahtara **profili** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="b2b4e-165">Tıklayın **Spravovat Profily**ve üretim profili silin.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="b2b4e-166">Kapat **Web'i Yayımla** bu değişikliği kaydetmek için Sihirbazı.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="b2b4e-167">Açık **Web'i Yayımla** Sihirbazı yeniden ve ardından **alma**.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="b2b4e-168">Üzerinde **bağlantı** sekmesinde, **hedef URL** uygun değere geçici bir URL kullanmanız durumunda.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="b2b4e-169">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-169">Click **Next**.</span></span>

<span data-ttu-id="b2b4e-170">Üzerinde **ayarları** sekmesinde **yeni veritabanı yayımlama iyileştirmelerini etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="b2b4e-171">Bağlantı dizesi aşağı açılan listesinde **SchoolContext**, Cytanium bağlantı dizesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="b2b4e-173">Seçin **yürütme Code First migrations'ı (uygulama başlatılırken çalışır)**.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="b2b4e-174">Bağlantı dizesi aşağı açılan listesinde **DefaultConnection**, Cytanium bağlantı dizesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="b2b4e-175">Seçin **profili** sekmesinde **Spravovat Profily**ve "Üretim" için "contosouniversity.com - Web Dağıtımı" profil olarak yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="b2b4e-176">Değişikliği kaydetmek için yayımlama profilini kapatın ve yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="b2b4e-177">Tıklayın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-177">Click **Publish**.</span></span> <span data-ttu-id="b2b4e-178">(Gerçek üretim Web sitesi için kopyalamak *uygulama\_offline.htm* üretim ve put proje klasörünüze yayımlamadan önce ardından kaldırın, dağıtım tamamlandığında.)</span><span class="sxs-lookup"><span data-stu-id="b2b4e-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="b2b4e-179">Visual Studio kod değişikliklerini test ortamı dağıtır ve tarayıcıya Contoso University giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="b2b4e-180">Eğitmenler sayfasını seçin.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-180">Select the Instructors page.</span></span>

<span data-ttu-id="b2b4e-181">Code First geçişleri Test ortamında yaptığınız gibi veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="b2b4e-182">Yeni bir ofis saatleri sütun çekirdeği oluşturulmuş veri görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="b2b4e-184">Artık bir SQL Server veritabanını kullanarak bir veritabanı değişiklik bulunan bir uygulama güncelleştirmesi başarıyla dağıtmış olmanız.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="b2b4e-185">Daha fazla bilgi</span><span class="sxs-lookup"><span data-stu-id="b2b4e-185">More Information</span></span>

<span data-ttu-id="b2b4e-186">Bu, Bu öğretici serisinde, bir üçüncü taraf barındırma sağlayıcısı bir ASP.NET web uygulamasına dağıtma tamamlar.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="b2b4e-187">Aşağıdaki öğreticilerde kapsamdaki konularına hakkında daha fazla bilgi için bkz. [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) MSDN web sitesinde.</span><span class="sxs-lookup"><span data-stu-id="b2b4e-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="b2b4e-188">Onayları</span><span class="sxs-lookup"><span data-stu-id="b2b4e-188">Acknowledgements</span></span>

<span data-ttu-id="b2b4e-189">Bu öğretici serisinin önemli katkılar içeriğe yapılan aşağıdaki kişilerin teşekkür ister misiniz:</span><span class="sxs-lookup"><span data-stu-id="b2b4e-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="b2b4e-190">Alberto Poblacion, MVP &amp; MCT, İspanya</span><span class="sxs-lookup"><span data-stu-id="b2b4e-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="b2b4e-191">Jarod Ferguson, veri platformu geliştirme MVP, Amerika Birleşik Devletleri</span><span class="sxs-lookup"><span data-stu-id="b2b4e-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="b2b4e-192">Sert Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="b2b4e-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="b2b4e-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="b2b4e-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="b2b4e-194">Mike Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="b2b4e-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="b2b4e-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="b2b4e-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="b2b4e-196">Raffaele Rialdi, İtalya</span><span class="sxs-lookup"><span data-stu-id="b2b4e-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="b2b4e-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="b2b4e-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="b2b4e-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="b2b4e-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="b2b4e-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="b2b4e-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="b2b4e-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="b2b4e-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="b2b4e-201">Srđan Božović, Sırbistan</span><span class="sxs-lookup"><span data-stu-id="b2b4e-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="b2b4e-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="b2b4e-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2b4e-203">[Önceki](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [İleri](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="b2b4e-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
