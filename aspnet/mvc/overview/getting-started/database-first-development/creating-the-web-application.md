---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF veritabanıyla ilk ASP.NET MVC: veri modelleri ve Web uygulaması oluşturma | Microsoft Docs'
author: tfitzmac
description: ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 04ccc00fa48702608fdc7b5b00d73778985852f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878782"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="47c5a-104">EF veritabanıyla ilk ASP.NET MVC: veri modelleri ve Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="47c5a-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="47c5a-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="47c5a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="47c5a-106">ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47c5a-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="47c5a-107">Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="47c5a-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="47c5a-108">Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="47c5a-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="47c5a-109">Web uygulaması oluşturma ve veritabanı tablolarınızı tabanlı veri modelleri oluşturma dizisinin bu bölümü odaklanır.</span><span class="sxs-lookup"><span data-stu-id="47c5a-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="47c5a-110">Yeni bir ASP.NET Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="47c5a-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="47c5a-111">Yeni bir çözüm veya veritabanı projesi olarak aynı çözüme Visual Studio'da yeni proje oluşturma ve seçin **ASP.NET Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="47c5a-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="47c5a-112">Proje adı **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="47c5a-112">Name the project **ContosoSite**.</span></span>

![Proje oluşturma](creating-the-web-application/_static/image1.png)

<span data-ttu-id="47c5a-114">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="47c5a-114">Click **OK**.</span></span>

<span data-ttu-id="47c5a-115">Yeni ASP.NET projesi penceresinde seçin **MVC** şablonu.</span><span class="sxs-lookup"><span data-stu-id="47c5a-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="47c5a-116">Temizleyebilir **bulutta Barındır** , daha sonra uygulamayı buluta dağıtacağınız çünkü şimdilik seçeneği.</span><span class="sxs-lookup"><span data-stu-id="47c5a-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="47c5a-117">Tıklatın **Tamam** uygulaması oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="47c5a-117">Click **OK** to create the application.</span></span>

![MVC şablonunu seçin](creating-the-web-application/_static/image2.png)

<span data-ttu-id="47c5a-119">Projenin varsayılan dosya ve klasörleri ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="47c5a-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="47c5a-120">Bu öğreticide, Entity Framework 6 kullanır.</span><span class="sxs-lookup"><span data-stu-id="47c5a-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="47c5a-121">Projenizdeki NuGet paketlerini Yönet penceresinin aracılığıyla Entity Framework sürümünü denetleyin.</span><span class="sxs-lookup"><span data-stu-id="47c5a-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="47c5a-122">Gerekirse, Entity Framework sürümüne güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="47c5a-122">If necessary, update your version of Entity Framework.</span></span>

![Sürüm Göster](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="47c5a-124">Modelleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="47c5a-124">Generate the models</span></span>

<span data-ttu-id="47c5a-125">Şimdi veritabanı tablolarından Entity Framework modelleri oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="47c5a-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="47c5a-126">Bu modeller verilerle çalışmak için kullanacağınız sınıflarıdır.</span><span class="sxs-lookup"><span data-stu-id="47c5a-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="47c5a-127">Her model veritabanındaki bir tablo yansıtır ve tablosundaki sütunlara karşılık gelen özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="47c5a-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="47c5a-128">Sağ **modelleri** klasörü ve select **Ekle** ve **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="47c5a-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![Yeni Öğe Ekle](creating-the-web-application/_static/image4.png)

<span data-ttu-id="47c5a-130">Yeni Öğe Ekle penceresinde seçin **veri** sol bölmede ve **ADO.NET varlık veri modeli** Orta bölmede Seçenekler.</span><span class="sxs-lookup"><span data-stu-id="47c5a-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="47c5a-131">Yeni model dosyası adı **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="47c5a-131">Name the new model file **ContosoModel**.</span></span>

![Model oluşturma](creating-the-web-application/_static/image5.png)

<span data-ttu-id="47c5a-133">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="47c5a-133">Click **Add**.</span></span>

<span data-ttu-id="47c5a-134">Varlık veri modeli Sihirbazı'nda seçin **veritabanından EF Designer**.</span><span class="sxs-lookup"><span data-stu-id="47c5a-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![veritabanından oluştur](creating-the-web-application/_static/image6.png)

<span data-ttu-id="47c5a-136">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="47c5a-136">Click **Next**.</span></span>

<span data-ttu-id="47c5a-137">Veritabanı bağlantıları geliştirme ortamınızı içinde tanımlı varsa, önceden seçilmiş bu bağlantılarından biri görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47c5a-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="47c5a-138">Ancak, bu öğreticinin ilk bölümünde oluşturulan veritabanı için yeni bir bağlantı oluşturmak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="47c5a-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="47c5a-139">Tıklatın **yeni bağlantı** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="47c5a-139">Click the **New Connection** button.</span></span>

![veritabanına bağlan](creating-the-web-application/_static/image7.png)

<span data-ttu-id="47c5a-141">Bağlantı Özellikleri penceresinde veritabanınızı oluşturulduğu yerel sunucunun adını girin (Bu durumda **(localdb) \ProjectsV12**).</span><span class="sxs-lookup"><span data-stu-id="47c5a-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="47c5a-142">Sunucu adı sağladıktan sonra ContosoUniversityData kullanılabilir veritabanlarından seçin.</span><span class="sxs-lookup"><span data-stu-id="47c5a-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![bağlantı özelliklerini ayarlama](creating-the-web-application/_static/image8.png)

<span data-ttu-id="47c5a-144">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="47c5a-144">Click **OK**.</span></span>

<span data-ttu-id="47c5a-145">Doğru bağlantı özellikleri artık görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="47c5a-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="47c5a-146">Web.Config dosyasında bağlantı için varsayılan adı kullanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="47c5a-146">You can use the default name for connection in the Web.Config file</span></span>

![bağlantı ayarları](creating-the-web-application/_static/image9.png)

<span data-ttu-id="47c5a-148">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="47c5a-148">Click **Next**.</span></span>

<span data-ttu-id="47c5a-149">Seçin **tabloları** tüm üç tablolar için model oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="47c5a-149">Select **Tables** to generate models for all three tables.</span></span>

![tabloları seçin](creating-the-web-application/_static/image10.png)

<span data-ttu-id="47c5a-151">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="47c5a-151">Click **Finish**.</span></span>

<span data-ttu-id="47c5a-152">Bir güvenlik uyarısı alırsanız, seçin **Tamam** şablon çalıştırmaya devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="47c5a-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="47c5a-153">Modeller veritabanı tablolarının oluşturulur ve özellikleri ve tablolar arasındaki ilişkileri gösteren diyagram görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="47c5a-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![modelin diyagramı](creating-the-web-application/_static/image11.png)

<span data-ttu-id="47c5a-155">Modeller klasörü artık veritabanından oluşturulan modelleri ilgili çok sayıda yeni dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="47c5a-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![Yeni model dosyaları göster](creating-the-web-application/_static/image12.png)

<span data-ttu-id="47c5a-157">**ContosoModel.Context.cs** dosyayı içeren türeyen bir sınıf **DbContext** sınıfı ve bir veritabanı tablosuna karşılık gelen her model sınıfı bir özellik sunar.</span><span class="sxs-lookup"><span data-stu-id="47c5a-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="47c5a-158">**Course.cs**, **Enrollment.cs**, ve **Student.cs** dosyaları veritabanları tabloları temsil eden model sınıfları içerir.</span><span class="sxs-lookup"><span data-stu-id="47c5a-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="47c5a-159">Yapı iskelesi ile çalışırken, bağlam sınıfını ve modeli sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="47c5a-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="47c5a-160">Bu öğretici ile devam etmeden önce projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="47c5a-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="47c5a-161">Proje oluşturulmadı bölüm çalışmaz ancak bu, sonraki bölümde, veri modellerinde göre kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="47c5a-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="47c5a-162">[Önceki](setting-up-database.md)
> [sonraki](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="47c5a-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>
