---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Öğretici: Görünümleri EF veritabanı ilk için ile ASP.NET MVC uygulaması oluşturma'
description: Bu makalede, görünümleri ve denetleyicileri oluşturmak için ASP.NET iskeleti oluşturma kullanmaya odaklanmıştır.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e1f6646cdf10d293268b92f44b018709e70c0f86
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889788"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="7345f-103">Öğretici: Görünümleri EF veritabanı ilk için ile ASP.NET MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7345f-103">Tutorial: Generate views for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="7345f-104">MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7345f-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="7345f-105">Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="7345f-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="7345f-106">Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="7345f-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="7345f-107">Bu makalede, görünümleri ve denetleyicileri oluşturmak için ASP.NET iskeleti oluşturma kullanmaya odaklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="7345f-107">This article focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>

<span data-ttu-id="7345f-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="7345f-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7345f-109">Yapı iskelesi Ekle</span><span class="sxs-lookup"><span data-stu-id="7345f-109">Add scaffold</span></span>
> * <span data-ttu-id="7345f-110">Yeni görünümlere bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="7345f-110">Add links to new views</span></span>
> * <span data-ttu-id="7345f-111">Öğrenci görünümleri görüntüler</span><span class="sxs-lookup"><span data-stu-id="7345f-111">Display student views</span></span>
> * <span data-ttu-id="7345f-112">Kayıt görünümleri görüntüler</span><span class="sxs-lookup"><span data-stu-id="7345f-112">Display enrollment views</span></span>

## <a name="prerequisite"></a><span data-ttu-id="7345f-113">Önkoşul</span><span class="sxs-lookup"><span data-stu-id="7345f-113">Prerequisite</span></span>

* [<span data-ttu-id="7345f-114">Web uygulama ve veri modelleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="7345f-114">Create the web application and data models</span></span>](creating-the-web-application.md)

## <a name="add-scaffold"></a><span data-ttu-id="7345f-115">Yapı iskelesi Ekle</span><span class="sxs-lookup"><span data-stu-id="7345f-115">Add scaffold</span></span>

<span data-ttu-id="7345f-116">Standart veri modeli sınıfları işlemlerinde sağlayacak kodu oluşturmak hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="7345f-116">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="7345f-117">Bir yapı iskelesi öğesi ekleyerek, kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7345f-117">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="7345f-118">Yapı iskelesi ekleyebileceğiniz türü için pek çok seçenek vardır; Bu öğreticide, iskele bir denetleyici ve önceki bölümde oluşturduğunuz Öğrenci ve kayıt modelleri karşılık gelen görünümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="7345f-118">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="7345f-119">Projenizdeki tutarlılık sağlamak için yeni denetleyici varolan ekleyeceksiniz **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="7345f-119">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="7345f-120">Sağ **denetleyicileri** klasörü ve select **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="7345f-120">Right-click the **Controllers** folder, and select **Add** > **New Scaffolded Item**.</span></span>

<span data-ttu-id="7345f-121">Seçin **MVC 5 denetleyici Entity Framework kullanarak görünümler ile** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="7345f-121">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="7345f-122">Bu seçenek, güncelleştirme, silme, oluşturma ve veri modelinizde görüntüleme için Görünüm ve denetleyici oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7345f-122">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![MVC denetleyicisi Ekle](generating-views/_static/image2.png)

<span data-ttu-id="7345f-124">Seçin **Öğrenci (ContosoSite.Models)** seçin ve model sınıfı için **ContosoUniversityDataEntities (ContosoSite.Models)** bağlamı sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7345f-124">Select **Student (ContosoSite.Models)** for the model class and select the **ContosoUniversityDataEntities (ContosoSite.Models)** for the context class.</span></span> <span data-ttu-id="7345f-125">Denetleyici adı olarak tutmak **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="7345f-125">Keep the controller name as **StudentsController**.</span></span>

<span data-ttu-id="7345f-126">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="7345f-126">Click **Add**.</span></span>

<span data-ttu-id="7345f-127">Bir hata alırsanız, projeyi önceki bölümde derlenmedi olabilir.</span><span class="sxs-lookup"><span data-stu-id="7345f-127">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="7345f-128">Bu durumda, projeyi derlemeyi deneyin ve ardından iskele kurulmuş öğe yeniden ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7345f-128">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="7345f-129">Kod oluşturma işlemi tamamlandıktan sonra yeni bir denetleyici ve görünüm projenizin görürsünüz **denetleyicileri** ve **görünümleri** > **Öğrenciler** klasörleri .</span><span class="sxs-lookup"><span data-stu-id="7345f-129">After the code generation process is complete, you will see a new controller and views in your project's **Controllers** and **Views** > **Students** folders.</span></span>


<span data-ttu-id="7345f-130">Aynı adımları yeniden gerçekleştirir, ancak için bir yapı iskelesi Ekle **kayıt** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7345f-130">Perform the same steps again, but add a scaffold for the **Enrollment** class.</span></span> <span data-ttu-id="7345f-131">İşiniz bittiğinde, sahip olduğunuz bir **EnrollmentsController.cs** dosya ve klasör altında **görünümleri** adlı **kayıtları** oluşturma, silme, Ayrıntılar, düzenleme ve dizin görünümlere sahip.</span><span class="sxs-lookup"><span data-stu-id="7345f-131">When finished, you have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

## <a name="add-links-to-new-views"></a><span data-ttu-id="7345f-132">Yeni görünümlere bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="7345f-132">Add links to new views</span></span>

<span data-ttu-id="7345f-133">Yeni görünümlerinizde gitmek kolaylaştırmak için birkaç köprüler dizin görünümlerine Öğrenciler ve kayıtlar için ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7345f-133">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="7345f-134">Dosyayı açmak **görünümleri** > **giriş** > *Index.cshtml*, siteniz için giriş sayfası olduğu.</span><span class="sxs-lookup"><span data-stu-id="7345f-134">Open the file at **Views** > **Home** > *Index.cshtml*, which is the home page for your site.</span></span> <span data-ttu-id="7345f-135">Jumbotron altına aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7345f-135">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="7345f-136">ActionLink için ilk parametre bağlantıdaki görüntülenecek metin yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="7345f-136">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="7345f-137">İkinci parametre eylemi ve üçüncü parametre denetleyicinin adı.</span><span class="sxs-lookup"><span data-stu-id="7345f-137">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="7345f-138">Örneğin, ilk bağlantı StudentsController dizin eylemi işaret eder.</span><span class="sxs-lookup"><span data-stu-id="7345f-138">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="7345f-139">Gerçek köprü, bu değerleri oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7345f-139">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="7345f-140">İlk bağlantı Sonuçta götüren **Index.cshtml** içinde dosya **görünümler/Öğrenciler** klasör.</span><span class="sxs-lookup"><span data-stu-id="7345f-140">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="7345f-141">Öğrenci görünümleri görüntüler</span><span class="sxs-lookup"><span data-stu-id="7345f-141">Display student views</span></span>

<span data-ttu-id="7345f-142">Doğru projenize eklediğiniz kodun Öğrenciler listesini görüntüler ve kullanıcıların düzenleme, oluşturma veya veritabanında Öğrenci kayıt silme sağlayan doğrular.</span><span class="sxs-lookup"><span data-stu-id="7345f-142">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="7345f-143">Sağ **görünümleri** > **giriş** > *Index.cshtml* dosya ve seçin **tarayıcıda görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="7345f-143">Right-click the **Views** > **Home** > *Index.cshtml* file, and select **View in Browser**.</span></span> <span data-ttu-id="7345f-144">Uygulama giriş sayfasında **Öğrenciler listesi**.</span><span class="sxs-lookup"><span data-stu-id="7345f-144">On the application home page, select **List of students**.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="7345f-145">Üzerinde **dizin** sayfasında, Öğrenciler ve bağlantıları bu verileri değiştirmek için listesi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="7345f-145">On the **Index** page, notice the list of the students and links to modify this data.</span></span> <span data-ttu-id="7345f-146">Seçin **Yeni Oluştur** bağlamak ve yeni bir öğrenci için bazı değerler sağlayın.</span><span class="sxs-lookup"><span data-stu-id="7345f-146">Select the **Create New** link and provide some values for a new student.</span></span> <span data-ttu-id="7345f-147">Tıklayın **Oluştur**ve yeni Öğrenci listenize eklenir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="7345f-147">Click **Create**, and notice the new student is added to your list.</span></span>

<span data-ttu-id="7345f-148">Yeniden **dizin** sayfasında **Düzenle** bağlamak ve bazı değerler için bir öğrenci değiştirin.</span><span class="sxs-lookup"><span data-stu-id="7345f-148">Back on the **Index** page, select the **Edit** link, and change some of the values for a student.</span></span> <span data-ttu-id="7345f-149">Tıklayın **Kaydet**, Öğrenci kayıt değiştirildi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="7345f-149">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="7345f-150">Son olarak, seçin **Sil** tıklayarak kaydı silmek istediğinizi onaylayın ve bağlama **Sil** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="7345f-150">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

<span data-ttu-id="7345f-151">Herhangi bir kod yazmadan Öğrenci tablodaki verileri ortak işlemleri görünümleri ekledik.</span><span class="sxs-lookup"><span data-stu-id="7345f-151">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="7345f-152">Bir alan için bir metin etiketi veritabanı özelliği dayalı olduğunu fark etmiş olabilirsiniz (gibi **LastName**) değil gerekmeyen web sayfasında görüntülemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="7345f-152">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="7345f-153">Örneğin, etiketin olması tercih edebilirsiniz **Soyadı**.</span><span class="sxs-lookup"><span data-stu-id="7345f-153">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="7345f-154">Öğreticinin ilerleyen bölümlerinde bu görüntüleme sorunu düzeltir.</span><span class="sxs-lookup"><span data-stu-id="7345f-154">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="7345f-155">Kayıt görünümleri görüntüler</span><span class="sxs-lookup"><span data-stu-id="7345f-155">Display enrollment views</span></span>

<span data-ttu-id="7345f-156">Veritabanınızı Öğrenci ve kayıt tabloları ve kursa ve kayıt tablolar arasında bir-çok ilişki arasında bir-çok ilişkisi içerir.</span><span class="sxs-lookup"><span data-stu-id="7345f-156">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="7345f-157">Kayıt görünümleri bu ilişkilerin düzgün bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="7345f-157">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="7345f-158">Sitenizi ve seçin için giriş sayfasına gidin **kayıtları listesi** bağlantısını ve ardından **Yeni Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7345f-158">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span>

<span data-ttu-id="7345f-159">Görünüm, yeni bir kayıt oluşturmak için bir form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="7345f-159">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="7345f-160">Özellikle, formun içerdiğini fark bir **CourseID** aşağı açılan liste ve **StudentID** aşağı açılan listesi.</span><span class="sxs-lookup"><span data-stu-id="7345f-160">In particular, notice that the form contains a **CourseID** drop-down list and a **StudentID** drop-down list.</span></span> <span data-ttu-id="7345f-161">Her ikisi de, ilgili tablolardan değerlerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="7345f-161">Both are populated with values from the related tables.</span></span>

<span data-ttu-id="7345f-162">Ayrıca, sağlanan değerlerin doğrulama otomatik olarak temel alan veri türünü uygulanır.</span><span class="sxs-lookup"><span data-stu-id="7345f-162">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="7345f-163">**Sınıf** sayı, gerekli uyumlu olmayan bir değer sağlamak denerseniz bir hata iletisi görüntülenir: *Sınıf alan bir sayı olmalıdır.*</span><span class="sxs-lookup"><span data-stu-id="7345f-163">**Grade** requires a number, so an error message is displayed if you try to provide an incompatible value: *The field Grade must be a number.*</span></span>

<span data-ttu-id="7345f-164">Otomatik olarak oluşturulan görünümler veritabanındaki verilerle çalışmak kullanıcıları etkinleştirmek doğrulanmıştır.</span><span class="sxs-lookup"><span data-stu-id="7345f-164">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="7345f-165">Bu serideki sonraki öğretici, veritabanını güncellemek ve web uygulamasında karşılık gelen değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="7345f-165">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7345f-166">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="7345f-166">Next steps</span></span>

<span data-ttu-id="7345f-167">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="7345f-167">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7345f-168">Eklenen yapı iskelesi</span><span class="sxs-lookup"><span data-stu-id="7345f-168">Added scaffold</span></span>
> * <span data-ttu-id="7345f-169">Yeni görünümlere eklenmiş bağlantılar</span><span class="sxs-lookup"><span data-stu-id="7345f-169">Added links to new views</span></span>
> * <span data-ttu-id="7345f-170">Görüntülenen Öğrenci görünümleri</span><span class="sxs-lookup"><span data-stu-id="7345f-170">Displayed student views</span></span>
> * <span data-ttu-id="7345f-171">Görüntülenen kayıt görünümleri</span><span class="sxs-lookup"><span data-stu-id="7345f-171">Displayed enrollment views</span></span>

<span data-ttu-id="7345f-172">Veritabanını değiştirme hakkında bilgi edinmek için sonraki makaleye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="7345f-172">Advance to the next article to learn how to change the database.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7345f-173">Veritabanını değiştirme</span><span class="sxs-lookup"><span data-stu-id="7345f-173">Change the database</span></span>](changing-the-database.md)