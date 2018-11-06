---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Entity Framework 6 veritabanı MVC 5 kullanarak First ile çalışmaya başlama | Microsoft Docs
author: Rick-Anderson
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 7fcb2b82dfa27ae192e1890c0c771d68658760a4
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021150"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="d3d15-104">MVC 5 Kullanarak Entity Framework 6 Database First ile Çalışmaya Başlama</span><span class="sxs-lookup"><span data-stu-id="d3d15-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="d3d15-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d3d15-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d3d15-106">MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="d3d15-107">Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="d3d15-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="d3d15-108">Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d3d15-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="d3d15-109">Serisinin son bölümünde sitenizi ve veritabanınızı Azure'a dağıtır.</span><span class="sxs-lookup"><span data-stu-id="d3d15-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="d3d15-110">Bu serinin veritabanı oluşturma ve verilerle doldurma odaklanır.</span><span class="sxs-lookup"><span data-stu-id="d3d15-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="d3d15-111">Bu seri, Tom Dykstra ve Rick Anderson katkılar ile yazılmıştır.</span><span class="sxs-lookup"><span data-stu-id="d3d15-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="d3d15-112">Bu temel kullanıcıların yorumlar bölümünde geri bildirim üzerinde geliştirildi.</span><span class="sxs-lookup"><span data-stu-id="d3d15-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="d3d15-113">Giriş</span><span class="sxs-lookup"><span data-stu-id="d3d15-113">Introduction</span></span>

<span data-ttu-id="d3d15-114">Bu konuda, başlama mevcut bir veritabanı ve hızlı bir şekilde kullanıcıların verilerle etkileşime olanak sağlayan bir web uygulaması oluşturma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d3d15-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="d3d15-115">Bu Entity Framework 6 ve MVC 5 web uygulaması oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="d3d15-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="d3d15-116">ASP.NET iskeleti oluşturma özelliği, görüntülemek, güncelleştirmek, oluşturmak ve verileri silme kod otomatik olarak oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d3d15-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="d3d15-117">Visual Studio'dan yayımlama araçları kullanarak, kolayca sitenizi ve veritabanınızı Azure'a dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="d3d15-118">Bu konuda, bir veritabanına sahip ve bu veritabanının alanlara göre bir web uygulaması için kod oluşturmak istediğiniz durumu ele alır.</span><span class="sxs-lookup"><span data-stu-id="d3d15-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="d3d15-119">Bu yaklaşım, ilk veritabanı geliştirme adı verilir.</span><span class="sxs-lookup"><span data-stu-id="d3d15-119">This approach is called Database First development.</span></span> <span data-ttu-id="d3d15-120">Mevcut bir veritabanı zaten yoksa, bunun yerine veri sınıfları tanımlama ve veritabanı oluşturma sınıfı özelliklerinden içerir Code First geliştirme olarak adlandırılan bir yaklaşımı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="d3d15-121">Code First geliştirmeye giriş örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="d3d15-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="d3d15-122">Daha gelişmiş bir örnek için bkz: [ASP.NET MVC 4 uygulaması için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="d3d15-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="d3d15-123">Kullanmak için hangi Entity Framework yaklaşım seçme konusunda yönergeler için bkz [Entity Framework Geliştirme yaklaşımları](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="d3d15-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3d15-124">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="d3d15-124">Prerequisites</span></span>

<span data-ttu-id="d3d15-125">Visual Studio 2013 veya Visual Studio Web için Express 2013</span><span class="sxs-lookup"><span data-stu-id="d3d15-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="d3d15-126">Veritabanı ayarlama</span><span class="sxs-lookup"><span data-stu-id="d3d15-126">Set up the database</span></span>

<span data-ttu-id="d3d15-127">Mevcut bir veritabanına sahip olmanın ortamınızın benzetimini yapmak için önce önceden doldurulmuş bazı verilerle bir veritabanı oluşturun ve ardından veritabanına bağlanan web uygulamanızı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="d3d15-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="d3d15-128">Bu öğreticide, Web için Visual Studio 2013 veya Visual Studio Express 2013 ile LocalDB kullanımıyla geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d3d15-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="d3d15-129">LocalDB yerine mevcut bir veritabanı sunucusunu kullanabilirsiniz, ancak Visual Studio ve veritabanı türüne, sürümüne bağlı olarak, tüm Visual Studio veri Araçları'nın desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="d3d15-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="d3d15-130">Araçları, veritabanı için mevcut değilse, veritabanınız için bazı yönetim paketi içinde veritabanı özgü adımlarını gerçekleştirmek gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="d3d15-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="d3d15-131">Visual Studio sürümünde veritabanı araçları ile ilgili bir sorun varsa, Veritabanı Araçları'nın en son sürümünü yüklediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="d3d15-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="d3d15-132">Güncelleştirme veya veritabanı araçlarını yükleme hakkında daha fazla bilgi için bkz: [Microsoft SQL Server veri Araçları](https://msdn.microsoft.com/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="d3d15-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="d3d15-133">Visual Studio'yu başlatın ve oluşturma bir **SQL Server veritabanı projesi**.</span><span class="sxs-lookup"><span data-stu-id="d3d15-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="d3d15-134">Projeyi adlandırın **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="d3d15-134">Name the project **ContosoUniversityData**.</span></span>

![veritabanı projesi oluşturun](setting-up-database/_static/image1.png)

<span data-ttu-id="d3d15-136">Artık bir boş veritabanı projesi vardır.</span><span class="sxs-lookup"><span data-stu-id="d3d15-136">You now have an empty database project.</span></span> <span data-ttu-id="d3d15-137">Proje için hedef platform olarak Azure SQL veritabanı ihtiyacınız olacak şekilde bu veritabanı Bu öğreticide daha sonra Azure'a dağıtır.</span><span class="sxs-lookup"><span data-stu-id="d3d15-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="d3d15-138">Hedef platform ayarlama, veritabanı gerçekten dağıtmayan; Bu, yalnızca veritabanı projesini veritabanı tasarımı hedef platform ile uyumlu olduğunu doğrular anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d3d15-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="d3d15-139">Hedef platform ayarlamak için açın **özellikleri** seçin ve proje için **Microsoft Azure SQL veritabanı** hedef platformu için.</span><span class="sxs-lookup"><span data-stu-id="d3d15-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![kümesi hedef platform](setting-up-database/_static/image2.png)

<span data-ttu-id="d3d15-141">Tabloları tanımlama SQL komut dosyaları ekleyerek Bu öğretici için gerekli olan tablolar oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="d3d15-142">Projenize sağ tıklayın ve yeni bir öğe ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d3d15-142">Right-click your project and add a new item.</span></span>

![Yeni Öğe Ekle](setting-up-database/_static/image3.png)

<span data-ttu-id="d3d15-144">Öğrenci adlı yeni bir tablo ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d3d15-144">Add a new table named Student.</span></span>

![Öğrenci tablosu ekleme](setting-up-database/_static/image4.png)

<span data-ttu-id="d3d15-146">Tablo dosyasında T-SQL komutu tablo oluşturmak için aşağıdaki kod ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d3d15-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="d3d15-147">Tasarım penceresinde kod ile otomatik olarak eşitler dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="d3d15-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="d3d15-148">Kod veya Tasarımcısı ile çalışabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-148">You can work with either the code or designer.</span></span>

![kod ve tasarımının Göster](setting-up-database/_static/image5.png)

<span data-ttu-id="d3d15-150">Başka bir tablo ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d3d15-150">Add another table.</span></span> <span data-ttu-id="d3d15-151">Bu kez, kurs adlandırın ve aşağıdaki T-SQL komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="d3d15-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="d3d15-152">Ayrıca, kayıt adında bir tablo oluşturmak için bir kez daha yineleyin.</span><span class="sxs-lookup"><span data-stu-id="d3d15-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="d3d15-153">Veritabanınızı veritabanı dağıtıldıktan sonra çalıştırılacak bir komut dosyası aracılığıyla verilerle doldurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="d3d15-154">Dağıtım sonrası komut dosyası projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d3d15-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="d3d15-155">Varsayılan adı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-155">You can use the default name.</span></span>

![dağıtım sonrası komut dosyası Ekle](setting-up-database/_static/image6.png)

<span data-ttu-id="d3d15-157">Dağıtım sonrası betiği aşağıdaki T-SQL kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d3d15-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="d3d15-158">Eşleşen bir kaydı bulunduğunda bu betik yalnızca veritabanına veri ekler.</span><span class="sxs-lookup"><span data-stu-id="d3d15-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="d3d15-159">Üzerine değil veya veritabanına girilir tüm verileri silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="d3d15-160">Veritabanı projenizde dağıttığınız her zaman dağıtım sonrası betiği çalıştırılır dikkat edin önemlidir.</span><span class="sxs-lookup"><span data-stu-id="d3d15-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="d3d15-161">Bu nedenle, gereksinimlerinizi bu betik yazarken dikkatli bir şekilde göz önünde bulundurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d3d15-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="d3d15-162">Bazı durumlarda, projeyi dağıtılan her zaman bilinen bir veri kümesinden başlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="d3d15-163">Diğer durumlarda, var olan verilere herhangi bir şekilde alter istemeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="d3d15-164">Gereksinimlerinize göre bir dağıtım sonrası komut dosyası veya betik eklemek gerekenler ihtiyacınız karar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3d15-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="d3d15-165">Uygulamanızın bir dağıtım sonrası betiği veritabanıyla doldurma hakkında daha fazla bilgi için bkz. [dahil olmak üzere verileri bir SQL Server veritabanı projesi](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="d3d15-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="d3d15-166">Artık 4 SQL komut dosyaları ancak gerçek tablo vardır.</span><span class="sxs-lookup"><span data-stu-id="d3d15-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="d3d15-167">Yerel veritabanına, veritabanı projenizi dağıtmaya hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="d3d15-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="d3d15-168">Visual Studio'da oluşturmak ve veritabanı projenizi dağıtmak için Başlat düğmesine (veya F5) tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d3d15-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="d3d15-169">Derleme ve dağıtım başarılı olduğunu doğrulamak için çıktı sekmesini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="d3d15-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![Çıktıyı Göster](setting-up-database/_static/image7.png)

<span data-ttu-id="d3d15-171">Yeni veritabanı oluşturulduğunu görmek için **SQL Server Nesne Gezgini** ve doğru yerel veritabanı sunucusundaki projesinin adını bulun (Bu durumda **(localdb) \ProjectsV12**).</span><span class="sxs-lookup"><span data-stu-id="d3d15-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![Yeni veritabanı Göster](setting-up-database/_static/image8.png)

<span data-ttu-id="d3d15-173">Tabloları verilerle doldurulmuş olduğunu görmek için tabloyu sağ tıklatın ve seçin **görünüm verilerini**.</span><span class="sxs-lookup"><span data-stu-id="d3d15-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![tablo verilerini Göster](setting-up-database/_static/image9.png)

<span data-ttu-id="d3d15-175">Tablo verilerini düzenlenebilir bir görünümü görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d3d15-175">An editable view of the table data is displayed.</span></span>

![Tablo veri sonuçlarını göster](setting-up-database/_static/image10.png)

<span data-ttu-id="d3d15-177">Veritabanınızı ayarlayın ve verilerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="d3d15-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="d3d15-178">Sonraki öğreticide, veritabanı için bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="d3d15-178">In the next tutorial, you will create a web application for the database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d3d15-179">Next</span><span class="sxs-lookup"><span data-stu-id="d3d15-179">Next</span></span>](creating-the-web-application.md)
