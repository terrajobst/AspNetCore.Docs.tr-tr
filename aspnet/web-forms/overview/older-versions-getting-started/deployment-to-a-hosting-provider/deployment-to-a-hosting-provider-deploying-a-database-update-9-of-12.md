---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: veritabanı güncelleştirme - 12 9 dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a><span data-ttu-id="72614-103">SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: veritabanı güncelleştirme - 12 9'u dağıtma</span><span class="sxs-lookup"><span data-stu-id="72614-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Database Update - 9 of 12</span></span>
====================
<span data-ttu-id="72614-104">by [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="72614-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="72614-105">Başlangıç projesi indirme</span><span class="sxs-lookup"><span data-stu-id="72614-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="72614-106">Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi.</span><span class="sxs-lookup"><span data-stu-id="72614-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="72614-107">Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72614-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="72614-108">Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="72614-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="72614-109">Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="72614-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="72614-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="72614-110">Overview</span></span>

<span data-ttu-id="72614-111">Bu öğreticide, veritabanı değişikliği ve ilgili kod değişiklikler, yaptığınız değişiklikleri Visual Studio'da test ve ardından test ve üretim ortamları için güncelleştirme dağıtın.</span><span class="sxs-lookup"><span data-stu-id="72614-111">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to both the test and production environments.</span></span>

<span data-ttu-id="72614-112">Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="72614-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="72614-113">Bir tablo için yeni bir sütun ekleme</span><span class="sxs-lookup"><span data-stu-id="72614-113">Adding a New Column to a Table</span></span>

<span data-ttu-id="72614-114">Bu bölümde, bir doğum tarihi sütuna eklemek `Person` için temel sınıf `Student` ve `Instructor` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="72614-114">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="72614-115">Ardından yeni bir sütun görüntüler böylece Eğitmen verileri görüntüleyen sayfası güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="72614-115">Then you update the page that displays instructor data so that it displays the new column.</span></span>

<span data-ttu-id="72614-116">İçinde *ContosoUniversity.DAL* proje, açık *Person.cs* ve aşağıdaki özelliği sonuna Ekle `Person` sınıfı (olmamalıdır iki aşağıdaki küme ayraçları kapatma):</span><span class="sxs-lookup"><span data-stu-id="72614-116">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

<span data-ttu-id="72614-117">Ardından, yeni bir sütun için bir değer sağlar şekilde Seed yöntemi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="72614-117">Next, update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="72614-118">Açık *Migrations\Configuration.cs* ve başlar kod bloğunu Değiştir `var instructors = new List<Instructor>` doğum tarihi bilgi içeren aşağıdaki kod bloğu ile:</span><span class="sxs-lookup"><span data-stu-id="72614-118">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

<span data-ttu-id="72614-119">ContosoUniversity projeyi açın *Instructors.aspx* ve doğum tarihini görüntülemek için yeni bir şablon alanını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="72614-119">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="72614-120">İşe alma tarihi ve office atama dilinin arasında ekleyin:</span><span class="sxs-lookup"><span data-stu-id="72614-120">Add it between the ones for hire date and office assignment:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

<span data-ttu-id="72614-121">(Kod girintileme eşitlenmemiş alırsa, otomatik olarak dosyayı yeniden biçimlendirmek için CTRL-K ve sonra da CTRL-D basabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="72614-121">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>

<span data-ttu-id="72614-122">Çözümü derlemek ve ardından açın **Paket Yöneticisi Konsolu** penceresi.</span><span class="sxs-lookup"><span data-stu-id="72614-122">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="72614-123">Olarak ContosoUniversity.DAL hala seçili olduğundan emin olun **varsayılan proje**.</span><span class="sxs-lookup"><span data-stu-id="72614-123">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>

<span data-ttu-id="72614-124">İçinde **Paket Yöneticisi Konsolu** penceresinde, seçin **ContosoUniversity.DAL** olarak **varsayılan proje**ve ardından aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="72614-124">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

<span data-ttu-id="72614-125">Bu komut sona erdiğinde, Visual Studio yeni tanımlar sınıfı dosyasını açar `DbMIgration` sınıfı ve `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72614-125">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span>

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

<span data-ttu-id="72614-127">Çözümü derlemek ve ardından aşağıdaki komutu girin **Paket Yöneticisi Konsolu** penceresi (ContosoUniversity.DAL proje hala seçili olduğundan emin olun):</span><span class="sxs-lookup"><span data-stu-id="72614-127">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

<span data-ttu-id="72614-128">Komut tamamlandığında uygulamayı çalıştırın ve Eğitmen sayfasını seçin.</span><span class="sxs-lookup"><span data-stu-id="72614-128">When the command finishes, run the application and select the Instructors page.</span></span> <span data-ttu-id="72614-129">Sayfa yüklendiğinde yeni olduğunu görürsünüz Doğum Tarihi alanı.</span><span class="sxs-lookup"><span data-stu-id="72614-129">When the page loads, you see that it has the new birth date field.</span></span>

<span data-ttu-id="72614-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="72614-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="72614-131">Veritabanı Güncelleştirme Test ortamına dağıtma</span><span class="sxs-lookup"><span data-stu-id="72614-131">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="72614-132">İçinde **Çözüm Gezgini** ContosoUniversity projesini seçin.</span><span class="sxs-lookup"><span data-stu-id="72614-132">In **Solution Explorer** select the ContosoUniversity project.</span></span>

<span data-ttu-id="72614-133">İçinde **Web tek tık Yayımla** araç, select **Test** yayımlama profili ve ardından **Web'i Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="72614-133">In the **Web One Click Publish** toolbar, select the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="72614-134">(Araç çubuğu devre dışı bırakılırsa ContosoUniversity projesinde seçin **Çözüm Gezgini**.)</span><span class="sxs-lookup"><span data-stu-id="72614-134">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

<span data-ttu-id="72614-135">Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="72614-135">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span> <span data-ttu-id="72614-136">Güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için Eğitmen sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="72614-136">Run the Instructors page to verify that the update was successfully deployed.</span></span> <span data-ttu-id="72614-137">Uygulama veritabanı için bu sayfayı erişmeyi denediğinde, Code First veritabanı şeması güncelleştirir ve çalışan `Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="72614-137">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="72614-138">Sayfası görüntülendiğinde beklenen gördüğünüz **doğum tarihi** da tarihleri içeren sütun.</span><span class="sxs-lookup"><span data-stu-id="72614-138">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>

<span data-ttu-id="72614-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="72614-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="72614-140">Veritabanı Güncelleştirme üretim ortamına dağıtma</span><span class="sxs-lookup"><span data-stu-id="72614-140">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="72614-141">Artık üretime dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72614-141">You can now deploy to production.</span></span> <span data-ttu-id="72614-142">Tek fark, kullanacağınız olan *uygulama\_offline.htm* kullanıcıların siteye erişmesini ve bu nedenle değişiklikler dağıtıyorsunuz sırada veritabanını güncelleştirme önlemek için.</span><span class="sxs-lookup"><span data-stu-id="72614-142">The only difference is that you'll use *app\_offline.htm* to prevent users from accessing the site and thus updating the database while you're deploying changes.</span></span> <span data-ttu-id="72614-143">Üretim dağıtımı için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="72614-143">For production deployment perform the following steps:</span></span>

- <span data-ttu-id="72614-144">Karşıya yükleme *uygulama\_offline.htm* üretim sitesini dosyasına.</span><span class="sxs-lookup"><span data-stu-id="72614-144">Upload the *app\_offline.htm* file to the production site.</span></span>
- <span data-ttu-id="72614-145">Visual Studio'da üretim profilinde seçin **Web tek tık Yayımla** araç çubuğu ve tıklatın **Web'i Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="72614-145">In Visual Studio, choose the Production profile in the **Web One Click Publish** toolbar and click **Publish Web**.</span></span>
- <span data-ttu-id="72614-146">Silme *uygulama\_offline.htm* üretim sitesinden dosyası.</span><span class="sxs-lookup"><span data-stu-id="72614-146">Delete the *app\_offline.htm* file from the production site.</span></span>

> [!NOTE]
> <span data-ttu-id="72614-147">Uygulamanızı üretim ortamında kullanımdayken bir yedekleme planı uygulanması.</span><span class="sxs-lookup"><span data-stu-id="72614-147">While your application is in use in the production environment you should be implementing a backup plan.</span></span> <span data-ttu-id="72614-148">Diğer bir deyişle, düzenli aralıklarla kopyaladığınız *Okul-Prod.sdf* ve *aspnet Prod.sdf* üretim dosyalarından site bir güvenli depolama konumuna ve gibi birkaç nesli tutulması yedeklemeler.</span><span class="sxs-lookup"><span data-stu-id="72614-148">That is, you should be periodically copying the *School-Prod.sdf* and *aspnet-Prod.sdf* files from the production site to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="72614-149">Veritabanını güncelleştirirken bir yedek kopyadan hemen önce olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="72614-149">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="72614-150">Bir hata yaparsanız ve üretim dağıttıktan sonra kadar Bul yok, daha sonra onu bozulmasından önceki durumla durum veritabanına kurtarabilmek için devam edersiniz.</span><span class="sxs-lookup"><span data-stu-id="72614-150">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span>


<span data-ttu-id="72614-151">Visual Studio tarayıcıda, giriş sayfası URL'si açıldığında *uygulama\_offline.htm* sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="72614-151">When Visual Studio opens the home page URL in the browser, the *app\_offline.htm* page is displayed.</span></span> <span data-ttu-id="72614-152">Sildikten sonra *uygulama\_offline.htm* dosyası, yeniden güncelleştirmeyi başarıyla dağıtıldı doğrulamak için giriş sayfasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="72614-152">After you delete the *app\_offline.htm* file, you can browse to your home page again to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="72614-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="72614-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span></span>

<span data-ttu-id="72614-154">Şimdi, test ve üretim için veritabanı değişikliği dahil bir uygulama güncelleştirmesi dağıttıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="72614-154">You've now deployed an application update that included a database change to both test and production.</span></span> <span data-ttu-id="72614-155">Sonraki öğretici veritabanınızı SQL Server Compact'dan SQL Server Express ve SQL Server için nasıl geçirileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="72614-155">The next tutorial shows you how to migrate your database from SQL Server Compact to SQL Server Express and SQL Server.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="72614-156">[Önceki](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [sonraki](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="72614-156">[Previous](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Next](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span></span>
