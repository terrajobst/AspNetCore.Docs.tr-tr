---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: 12 9 - veritabanı güncelleştirmesi dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: b15d27a07207110187b897624814125c9e030493
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753583"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a><span data-ttu-id="2e7f4-103">SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: 12 9 - veritabanı güncelleştirmesi dağıtma</span><span class="sxs-lookup"><span data-stu-id="2e7f4-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Database Update - 9 of 12</span></span>
====================
<span data-ttu-id="2e7f4-104">tarafından [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2e7f4-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="2e7f4-105">Başlangıç projesini indirin</span><span class="sxs-lookup"><span data-stu-id="2e7f4-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="2e7f4-106">Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="2e7f4-107">Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="2e7f4-108">Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="2e7f4-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="2e7f4-109">Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2e7f4-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="2e7f4-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="2e7f4-110">Overview</span></span>

<span data-ttu-id="2e7f4-111">Bu öğreticide, bir veritabanı değişikliği ve ilgili kod değişiklikleri, yaptığınız değişiklikleri Visual Studio'da Test etmek ve ardından güncelleştirme test ve üretim ortamlarına dağıtın.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-111">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to both the test and production environments.</span></span>

<span data-ttu-id="2e7f4-112">Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="2e7f4-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="2e7f4-113">Tabloya yeni bir sütun ekleme</span><span class="sxs-lookup"><span data-stu-id="2e7f4-113">Adding a New Column to a Table</span></span>

<span data-ttu-id="2e7f4-114">Bu bölümde, bir doğum tarihi sütun eklemek `Person` için temel sınıf `Student` ve `Instructor` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-114">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="2e7f4-115">Ardından yeni bir sütun görüntüler Eğitmen veri görüntüleyen sayfa güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-115">Then you update the page that displays instructor data so that it displays the new column.</span></span>

<span data-ttu-id="2e7f4-116">İçinde *ContosoUniversity.DAL* projesini açarsanız *Person.cs* ve sonunda aşağıdaki özelliği ekleyin `Person` sınıfı (bulunmamalıdır iki kapatma küme ayraçlarını aşağıdaki):</span><span class="sxs-lookup"><span data-stu-id="2e7f4-116">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

<span data-ttu-id="2e7f4-117">Ardından, yeni bir sütun için bir değer sağlar, böylece Seed yöntemi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-117">Next, update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="2e7f4-118">Açık *Migrations\Configuration.cs* ve başlayan kod bloğunu `var instructors = new List<Instructor>` doğum tarihi bilgi içeren aşağıdaki kod bloğu ile:</span><span class="sxs-lookup"><span data-stu-id="2e7f4-118">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

<span data-ttu-id="2e7f4-119">ContosoUniversity projeyi *Instructors.aspx* ve doğum tarihini görüntülemek için yeni bir şablon alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-119">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="2e7f4-120">İşe alma tarih ve office atama yönelik olanlar arasında ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2e7f4-120">Add it between the ones for hire date and office assignment:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

<span data-ttu-id="2e7f4-121">(Kod girintilemesinin eşitlenmemiş alırsa, otomatik olarak dosyayı yeniden biçimlendirmek için CTRL-K ve ardından CTRL-D tuşlarına basabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="2e7f4-121">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>

<span data-ttu-id="2e7f4-122">Çözümü derleyin ve ardından açın **Paket Yöneticisi Konsolu** penceresi.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-122">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="2e7f4-123">Olarak ContosoUniversity.DAL hala seçili olduğundan emin olun **varsayılan proje**.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-123">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>

<span data-ttu-id="2e7f4-124">İçinde **Paket Yöneticisi Konsolu** penceresinde **ContosoUniversity.DAL** olarak **varsayılan proje**ve ardından aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="2e7f4-124">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

<span data-ttu-id="2e7f4-125">Bu komut tamamlandığında, Visual Studio yeni tanımlayan sınıf dosyasını açar `DbMIgration` sınıfı ve `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-125">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span>

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

<span data-ttu-id="2e7f4-127">Çözümü derleyin ve ardından aşağıdaki komutu girin **Paket Yöneticisi Konsolu** penceresi (ContosoUniversity.DAL proje hala seçili olduğundan emin olun):</span><span class="sxs-lookup"><span data-stu-id="2e7f4-127">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

<span data-ttu-id="2e7f4-128">Komut tamamlandığında, uygulamayı çalıştırmak ve Eğitmenler sayfasını seçin.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-128">When the command finishes, run the application and select the Instructors page.</span></span> <span data-ttu-id="2e7f4-129">Sayfa yüklendiğinde yeni olduğunu gördüğünüz Doğum Tarihi alanı.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-129">When the page loads, you see that it has the new birth date field.</span></span>

<span data-ttu-id="2e7f4-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="2e7f4-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="2e7f4-131">Test ortamına veritabanı güncelleştirmesi dağıtma</span><span class="sxs-lookup"><span data-stu-id="2e7f4-131">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="2e7f4-132">İçinde **Çözüm Gezgini** ContosoUniversity projeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-132">In **Solution Explorer** select the ContosoUniversity project.</span></span>

<span data-ttu-id="2e7f4-133">İçinde **Web tek tık Yayımla** araç, select **Test** yayımlama profili ve ardından **Web'i Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-133">In the **Web One Click Publish** toolbar, select the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="2e7f4-134">(Araç çubuğunda devre dışı bırakılırsa ContosoUniversity projesinde seçin **Çözüm Gezgini**.)</span><span class="sxs-lookup"><span data-stu-id="2e7f4-134">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

<span data-ttu-id="2e7f4-135">Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-135">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span> <span data-ttu-id="2e7f4-136">Güncelleştirme başarıyla dağıtıldığını doğrulamak için Eğitmenler sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-136">Run the Instructors page to verify that the update was successfully deployed.</span></span> <span data-ttu-id="2e7f4-137">Uygulama veritabanı için bu sayfaya erişmeye çalıştığında, Code First veritabanı şemasını güncelleştirir ve çalışan `Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-137">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="2e7f4-138">Sayfası görüntülendiğinde, beklenen gördüğünüz **doğum tarihi** içindeki tarihler içeren sütun.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-138">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>

<span data-ttu-id="2e7f4-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2e7f4-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="2e7f4-140">Veritabanı güncelleştirmesi üretim ortamına dağıtma</span><span class="sxs-lookup"><span data-stu-id="2e7f4-140">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="2e7f4-141">Artık üretime dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-141">You can now deploy to production.</span></span> <span data-ttu-id="2e7f4-142">Tek fark, kullanacaksınız *uygulama\_offline.htm* kullanıcıların değişiklikleri dağıtıyorsanız sırada bu nedenle veritabanı güncelleştiriliyor ve siteye erişmesini önlemek için.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-142">The only difference is that you'll use *app\_offline.htm* to prevent users from accessing the site and thus updating the database while you're deploying changes.</span></span> <span data-ttu-id="2e7f4-143">Üretim dağıtımı için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="2e7f4-143">For production deployment perform the following steps:</span></span>

- <span data-ttu-id="2e7f4-144">Karşıya yükleme *uygulama\_offline.htm* üretim sitesi için dosya.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-144">Upload the *app\_offline.htm* file to the production site.</span></span>
- <span data-ttu-id="2e7f4-145">Visual Studio'da üretim profili seçin **Web tek tık Yayımla** araç çubuğu ve tıklatın **Web'i Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-145">In Visual Studio, choose the Production profile in the **Web One Click Publish** toolbar and click **Publish Web**.</span></span>
- <span data-ttu-id="2e7f4-146">Silme *uygulama\_offline.htm* üretim sitesini dosyasından.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-146">Delete the *app\_offline.htm* file from the production site.</span></span>

> [!NOTE]
> <span data-ttu-id="2e7f4-147">Uygulamanızı üretim ortamında olarak kullanılırken bir yedekleme planı uygulanması.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-147">While your application is in use in the production environment you should be implementing a backup plan.</span></span> <span data-ttu-id="2e7f4-148">Diğer bir deyişle, düzenli aralıklarla kopyaladığınız *Okul-Prod.sdf* ve *aspnet Prod.sdf* üretim dosyalarından site bir güvenli depolama konumuna ve gibi çeşitli nesil tutulması yedeklemeler.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-148">That is, you should be periodically copying the *School-Prod.sdf* and *aspnet-Prod.sdf* files from the production site to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="2e7f4-149">Veritabanı güncelleştirdiğinizde, bir yedek kopyadan hemen önce yapmalısınız.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-149">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="2e7f4-150">Daha sonra bir hata yaparsanız ve üretime dağıttıktan sonra kadar Bul yok, hala onu bozulmasından önceki olduğu duruma veritabanına kurtarmanız mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-150">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span>


<span data-ttu-id="2e7f4-151">Visual Studio giriş sayfası URL tarayıcıda açıldığında *uygulama\_offline.htm* sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-151">When Visual Studio opens the home page URL in the browser, the *app\_offline.htm* page is displayed.</span></span> <span data-ttu-id="2e7f4-152">Sildikten sonra *uygulama\_offline.htm* dosyasını yeniden güncelleştirmeyi başarıyla dağıtıldığını doğrulamak için giriş sayfasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-152">After you delete the *app\_offline.htm* file, you can browse to your home page again to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="2e7f4-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="2e7f4-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span></span>

<span data-ttu-id="2e7f4-154">Artık hem test hem de üretim veritabanı için değişiklik bulunan bir uygulama güncelleştirmesi dağıttınız.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-154">You've now deployed an application update that included a database change to both test and production.</span></span> <span data-ttu-id="2e7f4-155">Sonraki öğreticiye, veritabanınızı SQL Server Compact'dan SQL Server Express ve SQL Server için geçirme işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="2e7f4-155">The next tutorial shows you how to migrate your database from SQL Server Compact to SQL Server Express and SQL Server.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2e7f4-156">[Önceki](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [İleri](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="2e7f4-156">[Previous](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Next](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span></span>
