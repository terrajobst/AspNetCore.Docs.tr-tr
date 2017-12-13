---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: "Entity Framework 6 veritabanı MVC 5 kullanarak ilk ile çalışmaya başlama | Microsoft Docs"
author: tfitzmac
description: "ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 9ecde847841eb727a0440f0483c69d1df6757815
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="9a29d-104">Entity Framework 6 veritabanı MVC 5 kullanarak ilk ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9a29d-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="9a29d-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9a29d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9a29d-106">ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a29d-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="9a29d-107">Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="9a29d-108">Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="9a29d-109">Dizisinin son bölümü site ve veritabanı için Azure dağıtır.</span><span class="sxs-lookup"><span data-stu-id="9a29d-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="9a29d-110">Veritabanı oluşturma ve verilerle doldurma dizisinin bu bölümü odaklanır.</span><span class="sxs-lookup"><span data-stu-id="9a29d-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="9a29d-111">Bu seri Katkıları ile zel Dykstra ve Rick Anderson yazıldı.</span><span class="sxs-lookup"><span data-stu-id="9a29d-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="9a29d-112">Bu temel görüşler açıklamalar bölümünde kullanıcılardan geliştirildi.</span><span class="sxs-lookup"><span data-stu-id="9a29d-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="9a29d-113">Giriş</span><span class="sxs-lookup"><span data-stu-id="9a29d-113">Introduction</span></span>

<span data-ttu-id="9a29d-114">Bu konu, başlama var olan veritabanı ve hızlı bir şekilde kullanıcıların verilerle etkileşime olanak sağlayan bir web uygulaması oluşturmak gösterir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="9a29d-115">Web uygulaması oluşturmak için Entity Framework 6 ve MVC 5 kullanır.</span><span class="sxs-lookup"><span data-stu-id="9a29d-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="9a29d-116">ASP.NET İskele özelliği otomatik olarak görüntüleme, güncelleştirme, oluşturma ve verileri silme için kod oluşturmak üzere sağlar.</span><span class="sxs-lookup"><span data-stu-id="9a29d-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="9a29d-117">Visual Studio içinde yayımlama Araçları'nı kullanarak kolayca site ve veritabanı Azure'a dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a29d-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="9a29d-118">Bu konuda nerede bir veritabanınız var ve veritabanının alanlara göre bir web uygulaması için kod oluşturmak istediğinizde sorununu gidermeye yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="9a29d-119">Bu yaklaşım, veritabanı ilk geliştirme adı verilir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-119">This approach is called Database First development.</span></span> <span data-ttu-id="9a29d-120">Var olan bir veritabanı zaten yoksa, bunun yerine veri sınıfları tanımlama ve sınıf özelliklerinden veritabanı oluşturma kapsar Code First geliştirme adlı bir yaklaşım kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a29d-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="9a29d-121">Code First geliştirme giriş örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="9a29d-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="9a29d-122">Daha gelişmiş bir örnek için bkz: [bir ASP.NET MVC 4 uygulama için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="9a29d-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="9a29d-123">Hangi Entity Framework yaklaşımı kullanacak şekilde seçme ile ilgili yönergeler için bkz: [Entity Framework Geliştirme yaklaşımları](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="9a29d-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a29d-124">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9a29d-124">Prerequisites</span></span>

<span data-ttu-id="9a29d-125">Visual Studio 2013 veya Visual Studio Express 2013 için Web</span><span class="sxs-lookup"><span data-stu-id="9a29d-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="9a29d-126">Veritabanı ayarlama</span><span class="sxs-lookup"><span data-stu-id="9a29d-126">Set up the database</span></span>

<span data-ttu-id="9a29d-127">Varolan bir veritabanını sahip olmanın ortamı görüntüsü vermek için önce bazı önceden doldurulmuş verilerle bir veritabanı oluşturun ve veritabanına bağlanan web uygulamanızı oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="9a29d-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="9a29d-128">Bu öğretici, Web için Visual Studio 2013 veya Visual Studio Express 2013 ile LocalDB kullanımıyla geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="9a29d-129">Yerel veritabanı yerine var olan bir veritabanı sunucusu kullanabilirsiniz, ancak Visual Studio ve türünüzü veritabanının sürümüne bağlı olarak, tüm verileri araçları Visual Studio desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="9a29d-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="9a29d-130">Araçlar veritabanınız için kullanılabilir değilse, bazı yönetim paketi içinde veritabanı özgü adımlarını veritabanınız için gerçekleştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="9a29d-131">Visual Studio sürümünüzde veritabanı araçları ile ilgili bir sorun varsa, veritabanı araçları en son sürümünü yüklediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="9a29d-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="9a29d-132">Güncelleştirme veya veritabanı araçlarını yükleme hakkında daha fazla bilgi için bkz: [Microsoft SQL Server veri Araçları](https://msdn.microsoft.com/en-us/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="9a29d-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/en-us/data/hh297027).</span></span>

<span data-ttu-id="9a29d-133">Visual Studio'yu başlatın ve oluşturma bir **SQL Server veritabanı projesi**.</span><span class="sxs-lookup"><span data-stu-id="9a29d-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="9a29d-134">Proje adı **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="9a29d-134">Name the project **ContosoUniversityData**.</span></span>

![veritabanı projesi oluşturma](setting-up-database/_static/image1.png)

<span data-ttu-id="9a29d-136">Artık bir boş veritabanı projesi var.</span><span class="sxs-lookup"><span data-stu-id="9a29d-136">You now have an empty database project.</span></span> <span data-ttu-id="9a29d-137">Azure SQL veritabanı projesi için hedef platformu olarak ayarlamanız gerekir böylece daha sonra Bu öğreticide Azure için bu veritabanını dağıtır.</span><span class="sxs-lookup"><span data-stu-id="9a29d-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="9a29d-138">Hedef platform ayarı veritabanı gerçekte dağıtmayan; Bu, yalnızca veritabanı projesi veritabanı tasarım hedef platform ile uyumlu olup olmadığını doğrular anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="9a29d-139">Hedef platform ayarlamak için açık **özellikleri** seçin ve proje için **Microsoft Azure SQL veritabanı** hedef platformu için.</span><span class="sxs-lookup"><span data-stu-id="9a29d-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![kümesi hedef platformu](setting-up-database/_static/image2.png)

<span data-ttu-id="9a29d-141">Tabloları tanımlamanız SQL komut dosyaları ekleyerek Bu öğretici için gerekli tabloları oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a29d-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="9a29d-142">Projenize sağ tıklayın ve yeni bir öğe ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9a29d-142">Right-click your project and add a new item.</span></span>

![Yeni Öğe Ekle](setting-up-database/_static/image3.png)

<span data-ttu-id="9a29d-144">Öğrenci adlı yeni bir tablo ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9a29d-144">Add a new table named Student.</span></span>

![Öğrenci tablo ekleme](setting-up-database/_static/image4.png)

<span data-ttu-id="9a29d-146">Tablo dosyasında T-SQL komutu tablo oluşturmak için aşağıdaki kod ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9a29d-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="9a29d-147">Tasarım penceresini koduyla otomatik olarak eşitleyen dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9a29d-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="9a29d-148">Kod veya Tasarımcısı ile çalışabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a29d-148">You can work with either the code or designer.</span></span>

![kod ve tasarım Göster](setting-up-database/_static/image5.png)

<span data-ttu-id="9a29d-150">Başka bir tablo ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9a29d-150">Add another table.</span></span> <span data-ttu-id="9a29d-151">Bu süre indirmelere adlandırın ve aşağıdaki T-SQL komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="9a29d-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="9a29d-152">Ve kayıt adlı bir tablo oluşturmak için bir kez daha yineleyin.</span><span class="sxs-lookup"><span data-stu-id="9a29d-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="9a29d-153">Veritabanınızın veritabanı dağıtıldıktan sonra çalıştırılan bir komut dosyası aracılığıyla verilerle doldurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a29d-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="9a29d-154">Bir dağıtım sonrası komut dosyası projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9a29d-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="9a29d-155">Varsayılan adı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a29d-155">You can use the default name.</span></span>

![dağıtım sonrası komut dosyası ekleme](setting-up-database/_static/image6.png)

<span data-ttu-id="9a29d-157">Aşağıdaki T-SQL kodunu dağıtım sonrası komut dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9a29d-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="9a29d-158">Eşleşen bir kaydı bulunduğunda bu komut dosyası yalnızca veritabanına veri ekler.</span><span class="sxs-lookup"><span data-stu-id="9a29d-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="9a29d-159">Üzerine yazmaz veya veritabanına girilir herhangi bir veri silin.</span><span class="sxs-lookup"><span data-stu-id="9a29d-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="9a29d-160">Veritabanı projesi dağıttığınız her zaman, dağıtım sonrası komut dosyasını çalıştırmak dikkate almak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="9a29d-161">Bu nedenle, bu komut dosyası yazılırken gereksinimlerinizi dikkatlice düşünün gerekir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="9a29d-162">Bazı durumlarda, proje dağıtılan her zaman içinde bilinen bir veri kümesinden başlatmak istediğinizi.</span><span class="sxs-lookup"><span data-stu-id="9a29d-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="9a29d-163">Diğer durumlarda, var olan verileri herhangi bir şekilde alter istemeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a29d-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="9a29d-164">Gereksinimlerinize bağlı olarak, bir dağıtım sonrası komut dosyası veya komut dosyasında dahil etmeniz ihtiyacınız karar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a29d-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="9a29d-165">Bir dağıtım sonrası komut dosyası kullanarak veritabanınızı doldurma hakkında daha fazla bilgi için bkz: [bir SQL Server veritabanı projesi verileri de dahil olmak üzere](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="9a29d-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="9a29d-166">Artık 4 SQL komut dosyaları, ancak gerçek tablo vardır.</span><span class="sxs-lookup"><span data-stu-id="9a29d-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="9a29d-167">Yerel veritabanı için veritabanı projenizi dağıtmaya hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="9a29d-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="9a29d-168">Visual Studio'da oluşturmak ve veritabanı projenizi dağıtmak için Başlat düğmesi (veya F5) tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9a29d-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="9a29d-169">Derleme ve dağıtım başarılı olduğunu doğrulamak için çıktı sekmesini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="9a29d-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![Çıktıyı Göster](setting-up-database/_static/image7.png)

<span data-ttu-id="9a29d-171">Yeni Veritabanı oluşturuldu görmek için açın **SQL Server Nesne Gezgini** ve doğru yerel veritabanı sunucusu projesinde adını arayın (Bu durumda **(localdb) \ProjectsV12**).</span><span class="sxs-lookup"><span data-stu-id="9a29d-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![Yeni veritabanı Göster](setting-up-database/_static/image8.png)

<span data-ttu-id="9a29d-173">Tabloları verilerle doldurulur görmek için tabloyu sağ tıklatın ve seçin **görünüm verilerini**.</span><span class="sxs-lookup"><span data-stu-id="9a29d-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![Tablo verisi Göster](setting-up-database/_static/image9.png)

<span data-ttu-id="9a29d-175">Tablo verisi düzenlenebilir bir görünümünü görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9a29d-175">An editable view of the table data is displayed.</span></span>

![Tablo verisi sonuçlarının Göster](setting-up-database/_static/image10.png)

<span data-ttu-id="9a29d-177">Veritabanınızı ayarlayabilir ve verilerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="9a29d-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="9a29d-178">Sonraki öğreticide veritabanı için bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="9a29d-178">In the next tutorial, you will create a web application for the database.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="9a29d-179">Sonraki</span><span class="sxs-lookup"><span data-stu-id="9a29d-179">Next</span></span>](creating-the-web-application.md)
