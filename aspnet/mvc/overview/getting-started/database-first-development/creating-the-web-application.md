---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Öğretici: Oluşturma veritabanı ilk ASP.NET MVC ile EF için veri modelleri ve Web uygulaması'
description: Bu öğreticide web uygulaması oluşturma ve veritabanı tablolarınızı dayalı veri modelleri oluşturma odaklanır.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: dced55386c3f810e406c5c2b3f0071b45e3b2dbd
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236373"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="7a800-103">Öğretici: Oluşturma veritabanı ilk ASP.NET MVC ile EF için veri modelleri ve Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="7a800-103">Tutorial: Create the the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="7a800-104">MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7a800-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="7a800-105">Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="7a800-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="7a800-106">Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="7a800-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="7a800-107">Bu öğreticide web uygulaması oluşturma ve veritabanı tablolarınızı dayalı veri modelleri oluşturma odaklanır.</span><span class="sxs-lookup"><span data-stu-id="7a800-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="7a800-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="7a800-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7a800-109">ASP.NET web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7a800-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="7a800-110">Modelleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="7a800-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7a800-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7a800-111">Prerequisites</span></span>

* [<span data-ttu-id="7a800-112">Entity Framework 6 veritabanı MVC 5 kullanarak First ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="7a800-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="7a800-113">ASP.NET web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7a800-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="7a800-114">Yeni bir çözüm veya veritabanı projesini aynı çözümde Visual Studio'da yeni bir proje oluşturun ve seçin **ASP.NET Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="7a800-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="7a800-115">Projeyi adlandırın **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="7a800-115">Name the project **ContosoSite**.</span></span>

![Proje oluşturma](creating-the-web-application/_static/image1.png)

<span data-ttu-id="7a800-117">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="7a800-117">Click **OK**.</span></span>

<span data-ttu-id="7a800-118">Yeni ASP.NET projesi penceresinde **MVC** şablonu.</span><span class="sxs-lookup"><span data-stu-id="7a800-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="7a800-119">Temizleyebilir **bulutta Barındır** , daha sonra uygulamanızı buluta dağıtmadan çünkü şu an için seçenek.</span><span class="sxs-lookup"><span data-stu-id="7a800-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="7a800-120">Tıklayın **Tamam** uygulama oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="7a800-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="7a800-121">Projenin varsayılan dosya ve klasörlerle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7a800-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="7a800-122">Bu öğreticide, Entity Framework 6 kullanır.</span><span class="sxs-lookup"><span data-stu-id="7a800-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="7a800-123">NuGet paketlerini Yönet penceresi projenizdeki Entity Framework sürümünü denetleyin.</span><span class="sxs-lookup"><span data-stu-id="7a800-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="7a800-124">Gerekirse, Entity Framework sürümünüzü güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7a800-124">If necessary, update your version of Entity Framework.</span></span>

![Sürüm Göster](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="7a800-126">Modelleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="7a800-126">Generate the models</span></span>

<span data-ttu-id="7a800-127">Bu gibi durumlarda, Entity Framework modelleri artık veritabanı tablolarından oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="7a800-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="7a800-128">Bu modeli verilerle çalışmak için kullanacağınız sınıflardır.</span><span class="sxs-lookup"><span data-stu-id="7a800-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="7a800-129">Her model, veritabanındaki bir tabloda yansıtır ve tablosundaki sütunlara karşılık gelen özelliklerle içerir.</span><span class="sxs-lookup"><span data-stu-id="7a800-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="7a800-130">Sağ **modelleri** klasörü ve select **Ekle** ve **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="7a800-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="7a800-131">Yeni Öğe Ekle penceresinde **veri** sol bölmede ve **ADO.NET varlık veri modeli** Orta bölmedeki seçeneklerden.</span><span class="sxs-lookup"><span data-stu-id="7a800-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="7a800-132">Yeni model dosyası adı **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="7a800-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="7a800-133">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="7a800-133">Click **Add**.</span></span>

<span data-ttu-id="7a800-134">Varlık veri modeli Sihirbazı'nda seçin **EF veritabanı Tasarımcısından**.</span><span class="sxs-lookup"><span data-stu-id="7a800-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="7a800-135">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7a800-135">Click **Next**.</span></span>

<span data-ttu-id="7a800-136">Geliştirme ortamınızda tanımlanmış veritabanı bağlantınız varsa, önceden seçilmiş Bu bağlantılardan birini görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7a800-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="7a800-137">Ancak, bu öğreticinin ilk bölümünde oluşturduğunuz veritabanına yeni bir bağlantı oluşturmak istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="7a800-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="7a800-138">Tıklayın **yeni bağlantı** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="7a800-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="7a800-139">Bağlantı Özellikleri penceresinde veritabanınızı oluşturulduğu yerel sunucunun adını belirtin (Bu durumda **(localdb) \Projects13**).</span><span class="sxs-lookup"><span data-stu-id="7a800-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\Projects13**).</span></span> <span data-ttu-id="7a800-140">Sunucu adı girdikten sonra ContosoUniversityData kullanılabilir veritabanlarını seçin.</span><span class="sxs-lookup"><span data-stu-id="7a800-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![bağlantı özelliklerini ayarlama](creating-the-web-application/_static/image8.png)

<span data-ttu-id="7a800-142">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="7a800-142">Click **OK**.</span></span>

<span data-ttu-id="7a800-143">Doğru bağlantı özellikleri artık görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7a800-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="7a800-144">Web.Config dosyasında bağlantı için varsayılan adı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7a800-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="7a800-145">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7a800-145">Click **Next**.</span></span>

<span data-ttu-id="7a800-146">Entity Framework'ün en son sürümü seçin.</span><span class="sxs-lookup"><span data-stu-id="7a800-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="7a800-147">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7a800-147">Click **Next**.</span></span>

<span data-ttu-id="7a800-148">Seçin **tabloları** üç tüm tablolar için modeller oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="7a800-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="7a800-149">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7a800-149">Click **Finish**.</span></span>

<span data-ttu-id="7a800-150">Bir güvenlik uyarısı alırsanız seçin **Tamam** şablon çalıştırmaya devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="7a800-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="7a800-151">Veritabanı tablolarından modelleri oluşturulur ve özelliklerini ve tablolar arasındaki ilişkileri gösteren bir diyagram görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7a800-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![modelin diyagramı](creating-the-web-application/_static/image11.png)

<span data-ttu-id="7a800-153">Modeller klasörü artık veritabanından oluşturulan modelleri ilgili çok sayıda yeni dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="7a800-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="7a800-154">**ContosoModel.Context.cs** dosyası içerir, türetilen bir sınıf **DbContext** sınıfı ve bir veritabanı tablosuna karşılık gelen her bir model sınıfı için bir özellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="7a800-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="7a800-155">**Course.cs**, **Enrollment.cs**, ve **Student.cs** dosyaları veritabanı tablolarını temsil eden model sınıfları içerir.</span><span class="sxs-lookup"><span data-stu-id="7a800-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="7a800-156">Yapı iskelesi ile çalışırken bağlam sınıfını hem model sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="7a800-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="7a800-157">Bu öğretici ile devam etmeden önce projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="7a800-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="7a800-158">Bölüm projesi oluşturulmadı çalışmaz ancak bu, sonraki bölümde, veri modellerine göre kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7a800-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a800-159">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="7a800-159">Next steps</span></span>

<span data-ttu-id="7a800-160">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="7a800-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7a800-161">ASP.NET web uygulaması oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="7a800-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="7a800-162">Model</span><span class="sxs-lookup"><span data-stu-id="7a800-162">Generated the models</span></span>

<span data-ttu-id="7a800-163">Veri modellerine göre kod oluşturmak nasıl oluşturulacağını öğrenmek için sonraki öğreticiye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="7a800-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7a800-164">Görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="7a800-164">Generating views</span></span>](generating-views.md)