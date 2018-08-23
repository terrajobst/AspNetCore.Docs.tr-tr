---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'İlk ASP.NET MVC ile EF veritabanında: veri modelleri ve Web uygulaması oluşturma | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 343131d45ed0b2442f1b0b557c5b63f3877e5d0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751938"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="081f5-104">İlk ASP.NET MVC ile EF veritabanında: veri modelleri ve Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="081f5-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="081f5-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="081f5-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="081f5-106">MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="081f5-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="081f5-107">Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="081f5-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="081f5-108">Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="081f5-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="081f5-109">Web uygulaması oluşturma ve veritabanı tablolarınızı dayalı veri modelleri oluşturma bu serinin odaklanır.</span><span class="sxs-lookup"><span data-stu-id="081f5-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="081f5-110">Yeni bir ASP.NET Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="081f5-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="081f5-111">Yeni bir çözüm veya veritabanı projesini aynı çözümde Visual Studio'da yeni bir proje oluşturun ve seçin **ASP.NET Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="081f5-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="081f5-112">Projeyi adlandırın **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="081f5-112">Name the project **ContosoSite**.</span></span>

![Proje oluşturma](creating-the-web-application/_static/image1.png)

<span data-ttu-id="081f5-114">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="081f5-114">Click **OK**.</span></span>

<span data-ttu-id="081f5-115">Yeni ASP.NET projesi penceresinde **MVC** şablonu.</span><span class="sxs-lookup"><span data-stu-id="081f5-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="081f5-116">Temizleyebilir **bulutta Barındır** , daha sonra uygulamanızı buluta dağıtmadan çünkü şu an için seçenek.</span><span class="sxs-lookup"><span data-stu-id="081f5-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="081f5-117">Tıklayın **Tamam** uygulama oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="081f5-117">Click **OK** to create the application.</span></span>

![MVC şablonu seçin](creating-the-web-application/_static/image2.png)

<span data-ttu-id="081f5-119">Projenin varsayılan dosya ve klasörlerle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="081f5-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="081f5-120">Bu öğreticide, Entity Framework 6 kullanır.</span><span class="sxs-lookup"><span data-stu-id="081f5-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="081f5-121">NuGet paketlerini Yönet penceresi projenizdeki Entity Framework sürümünü denetleyin.</span><span class="sxs-lookup"><span data-stu-id="081f5-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="081f5-122">Gerekirse, Entity Framework sürümünüzü güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="081f5-122">If necessary, update your version of Entity Framework.</span></span>

![Sürüm Göster](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="081f5-124">Modelleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="081f5-124">Generate the models</span></span>

<span data-ttu-id="081f5-125">Bu gibi durumlarda, Entity Framework modelleri artık veritabanı tablolarından oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="081f5-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="081f5-126">Bu modeli verilerle çalışmak için kullanacağınız sınıflardır.</span><span class="sxs-lookup"><span data-stu-id="081f5-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="081f5-127">Her model, veritabanındaki bir tabloda yansıtır ve tablosundaki sütunlara karşılık gelen özelliklerle içerir.</span><span class="sxs-lookup"><span data-stu-id="081f5-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="081f5-128">Sağ **modelleri** klasörü ve select **Ekle** ve **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="081f5-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![Yeni Öğe Ekle](creating-the-web-application/_static/image4.png)

<span data-ttu-id="081f5-130">Yeni Öğe Ekle penceresinde **veri** sol bölmede ve **ADO.NET varlık veri modeli** Orta bölmedeki seçeneklerden.</span><span class="sxs-lookup"><span data-stu-id="081f5-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="081f5-131">Yeni model dosyası adı **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="081f5-131">Name the new model file **ContosoModel**.</span></span>

![Model oluşturma](creating-the-web-application/_static/image5.png)

<span data-ttu-id="081f5-133">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="081f5-133">Click **Add**.</span></span>

<span data-ttu-id="081f5-134">Varlık veri modeli Sihirbazı'nda seçin **EF veritabanı Tasarımcısından**.</span><span class="sxs-lookup"><span data-stu-id="081f5-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![veritabanından oluştur](creating-the-web-application/_static/image6.png)

<span data-ttu-id="081f5-136">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="081f5-136">Click **Next**.</span></span>

<span data-ttu-id="081f5-137">Geliştirme ortamınızda tanımlanmış veritabanı bağlantınız varsa, önceden seçilmiş Bu bağlantılardan birini görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="081f5-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="081f5-138">Ancak, bu öğreticinin ilk bölümünde oluşturduğunuz veritabanına yeni bir bağlantı oluşturmak istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="081f5-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="081f5-139">Tıklayın **yeni bağlantı** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="081f5-139">Click the **New Connection** button.</span></span>

![Veritabanı'na bağlanma](creating-the-web-application/_static/image7.png)

<span data-ttu-id="081f5-141">Bağlantı Özellikleri penceresinde veritabanınızı oluşturulduğu yerel sunucunun adını belirtin (Bu durumda **(localdb) \ProjectsV12**).</span><span class="sxs-lookup"><span data-stu-id="081f5-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="081f5-142">Sunucu adı girdikten sonra ContosoUniversityData kullanılabilir veritabanlarını seçin.</span><span class="sxs-lookup"><span data-stu-id="081f5-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![bağlantı özelliklerini ayarlama](creating-the-web-application/_static/image8.png)

<span data-ttu-id="081f5-144">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="081f5-144">Click **OK**.</span></span>

<span data-ttu-id="081f5-145">Doğru bağlantı özellikleri artık görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="081f5-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="081f5-146">Web.Config dosyasında bağlantı için varsayılan adı kullanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="081f5-146">You can use the default name for connection in the Web.Config file</span></span>

![bağlantı ayarları](creating-the-web-application/_static/image9.png)

<span data-ttu-id="081f5-148">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="081f5-148">Click **Next**.</span></span>

<span data-ttu-id="081f5-149">Seçin **tabloları** üç tüm tablolar için modeller oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="081f5-149">Select **Tables** to generate models for all three tables.</span></span>

![tabloları seçme](creating-the-web-application/_static/image10.png)

<span data-ttu-id="081f5-151">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="081f5-151">Click **Finish**.</span></span>

<span data-ttu-id="081f5-152">Bir güvenlik uyarısı alırsanız seçin **Tamam** şablon çalıştırmaya devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="081f5-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="081f5-153">Veritabanı tablolarından modelleri oluşturulur ve özelliklerini ve tablolar arasındaki ilişkileri gösteren bir diyagram görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="081f5-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![modelin diyagramı](creating-the-web-application/_static/image11.png)

<span data-ttu-id="081f5-155">Modeller klasörü artık veritabanından oluşturulan modelleri ilgili çok sayıda yeni dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="081f5-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![Yeni model dosyaları göster](creating-the-web-application/_static/image12.png)

<span data-ttu-id="081f5-157">**ContosoModel.Context.cs** dosyası içerir, türetilen bir sınıf **DbContext** sınıfı ve bir veritabanı tablosuna karşılık gelen her bir model sınıfı için bir özellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="081f5-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="081f5-158">**Course.cs**, **Enrollment.cs**, ve **Student.cs** dosyaları veritabanı tablolarını temsil eden model sınıfları içerir.</span><span class="sxs-lookup"><span data-stu-id="081f5-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="081f5-159">Yapı iskelesi ile çalışırken bağlam sınıfını hem model sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="081f5-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="081f5-160">Bu öğretici ile devam etmeden önce projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="081f5-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="081f5-161">Bölüm projesi oluşturulmadı çalışmaz ancak bu, sonraki bölümde, veri modellerine göre kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="081f5-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="081f5-162">[Önceki](setting-up-database.md)
> [İleri](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="081f5-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>
