---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'İlk ASP.NET MVC ile EF veritabanında: görünümler oluşturma | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 74c7abdc2d0f8fff9ad769d013fb001e2b9e427b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755177"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="a0ff6-104">İlk ASP.NET MVC ile EF veritabanında: görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="a0ff6-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="a0ff6-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a0ff6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a0ff6-106">MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="a0ff6-107">Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="a0ff6-108">Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="a0ff6-109">Bu serinin görünümleri ve denetleyicileri oluşturmak için ASP.NET iskeleti oluşturma kullanmaya odaklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="a0ff6-110">Yapı iskelesi Ekle</span><span class="sxs-lookup"><span data-stu-id="a0ff6-110">Add scaffold</span></span>

<span data-ttu-id="a0ff6-111">Standart veri modeli sınıfları işlemlerinde sağlayacak kodu oluşturmak hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="a0ff6-112">Bir yapı iskelesi öğesi ekleyerek, kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="a0ff6-113">Yapı iskelesi ekleyebileceğiniz türü için pek çok seçenek vardır; Bu öğreticide, iskele bir denetleyici ve önceki bölümde oluşturduğunuz Öğrenci ve kayıt modelleri karşılık gelen görünümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="a0ff6-114">Projenizdeki tutarlılık sağlamak için yeni denetleyici varolan ekleyeceksiniz **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="a0ff6-115">Sağ **denetleyicileri** klasörü ve select **Ekle** – **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Yapı iskelesi Ekle](generating-views/_static/image1.png)

<span data-ttu-id="a0ff6-117">Seçin **MVC 5 denetleyici Entity Framework kullanarak görünümler ile** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="a0ff6-118">Bu seçenek, güncelleştirme, silme, oluşturma ve veri modelinizde görüntüleme için Görünüm ve denetleyici oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![MVC denetleyicisi Ekle](generating-views/_static/image2.png)

<span data-ttu-id="a0ff6-120">Seçin **Öğrenci** model sınıfı ve seçin için **ContosoUniversityEntities** bağlamı sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="a0ff6-121">Denetleyici adı olarak tutmak **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="a0ff6-121">Keep the controller name as **StudentsController**,</span></span>

![Denetleyici belirtin](generating-views/_static/image3.png)

<span data-ttu-id="a0ff6-123">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-123">Click **Add**.</span></span>

<span data-ttu-id="a0ff6-124">Bir hata alırsanız, projeyi önceki bölümde derlenmedi olabilir.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="a0ff6-125">Bu durumda, projeyi derlemeyi deneyin ve ardından iskele kurulmuş öğe yeniden ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="a0ff6-126">Kod oluşturma işlemi tamamlandıktan sonra yeni bir denetleyici ve görünüm projenizde görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Görünümleri Göster](generating-views/_static/image4.png)

<span data-ttu-id="a0ff6-128">Aynı adımları yeniden gerçekleştirin, ancak kayıt sınıfı için bir yapı iskelesi Ekle.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="a0ff6-129">İşiniz bittiğinde, olmalıdır bir **EnrollmentsController.cs** dosya ve klasör altında **görünümleri** adlı **kayıtları** oluşturma, silme, Ayrıntılar, düzenleme ve dizin ile Görünümler.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Görünümleri Göster](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="a0ff6-131">Yeni görünümlere bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="a0ff6-131">Add links to new views</span></span>

<span data-ttu-id="a0ff6-132">Yeni görünümlerinizde gitmek kolaylaştırmak için birkaç köprüler dizin görünümlerine Öğrenciler ve kayıtlar için ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="a0ff6-133">Dosyayı açmak **Views/Home/Index.cshtml**, siteniz için giriş sayfası olduğu.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="a0ff6-134">Jumbotron altına aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="a0ff6-135">ActionLink için ilk parametre bağlantıdaki görüntülenecek metin yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="a0ff6-136">İkinci parametre eylemi ve üçüncü parametre denetleyicinin adı.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="a0ff6-137">Örneğin, ilk bağlantı StudentsController dizin eylemi işaret eder.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="a0ff6-138">Gerçek köprü, bu değerleri oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="a0ff6-139">İlk bağlantı Sonuçta götüren **Index.cshtml** içinde dosya **görünümler/Öğrenciler** klasör.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="a0ff6-140">Öğrenci görünümleri görüntüler</span><span class="sxs-lookup"><span data-stu-id="a0ff6-140">Display student views</span></span>

<span data-ttu-id="a0ff6-141">Doğru projenize eklediğiniz kodun Öğrenciler listesini görüntüler ve kullanıcıların düzenleme, oluşturma veya veritabanında Öğrenci kayıt silme sağlayan doğrular.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="a0ff6-142">Sağ **Views/Home/Index.cshtml** dosya ve seçin **tarayıcıda görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="a0ff6-143">Bu sayfada, Öğrenciler listesi için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="a0ff6-144">Bu sayfada Öğrenciler ve bağlantıları bu verileri değiştirmek için listesini dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-144">On this page, notice the list of the students and links to modify this data.</span></span>

![Öğrenciler listesi](generating-views/_static/image7.png)

<span data-ttu-id="a0ff6-146">Tıklayın **Yeni Oluştur** bağlamak ve yeni bir öğrenci için bazı değerler sağlayın.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Yeni Öğrenci oluşturma](generating-views/_static/image8.png)

<span data-ttu-id="a0ff6-148">Tıklayın **Oluştur**ve yeni Öğrenci listenize eklenir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-148">Click **Create**, and notice the new student is added to your list.</span></span>

![Yeni Öğrenci listesi](generating-views/_static/image9.png)

<span data-ttu-id="a0ff6-150">Seçin **Düzenle** bağlamak ve bazı değerler için bir öğrenci değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![öğrenciyi Düzenle](generating-views/_static/image10.png)

<span data-ttu-id="a0ff6-152">Tıklayın **Kaydet**, Öğrenci kayıt değiştirildi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="a0ff6-153">Son olarak, seçin **Sil** tıklayarak kaydı silmek istediğinizi onaylayın ve bağlama **Sil** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![Öğrenci Sil](generating-views/_static/image11.png)

<span data-ttu-id="a0ff6-155">Herhangi bir kod yazmadan Öğrenci tablodaki verileri ortak işlemleri görünümleri ekledik.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="a0ff6-156">Bir alan için bir metin etiketi veritabanı özelliği dayalı olduğunu fark etmiş olabilirsiniz (gibi **LastName**) değil gerekmeyen web sayfasında görüntülemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="a0ff6-157">Örneğin, etiketin olması tercih edebilirsiniz **Soyadı**.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="a0ff6-158">Öğreticinin ilerleyen bölümlerinde bu görüntüleme sorunu düzeltir.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="a0ff6-159">Kayıt görünümleri görüntüler</span><span class="sxs-lookup"><span data-stu-id="a0ff6-159">Display enrollment views</span></span>

<span data-ttu-id="a0ff6-160">Veritabanınızı Öğrenci ve kayıt tabloları ve kursa ve kayıt tablolar arasında bir-çok ilişki arasında bir-çok ilişkisi içerir.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="a0ff6-161">Kayıt görünümleri bu ilişkilerin düzgün bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="a0ff6-162">Sitenizi ve seçin için giriş sayfasına gidin **kayıtları listesi** bağlantısını ve ardından **Yeni Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="a0ff6-163">Görünüm, yeni bir kayıt oluşturmak için bir form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="a0ff6-164">Özellikle, formun ilgili tablolardan değerlerle doldurulur iki açılır liste içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![kayıt oluşturma](generating-views/_static/image12.png)

<span data-ttu-id="a0ff6-166">Ayrıca, sağlanan değerlerin doğrulama otomatik olarak temel alan veri türünü uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="a0ff6-167">Sınıf bir sayı, gerekli, uyumlu olmayan bir değer sağlamak denerseniz bir hata iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![doğrulama iletisi](generating-views/_static/image13.png)

<span data-ttu-id="a0ff6-169">Otomatik olarak oluşturulan görünümler veritabanındaki verilerle çalışmak kullanıcıları etkinleştirmek doğrulanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="a0ff6-170">Bu serideki sonraki öğretici, veritabanını güncellemek ve web uygulamasında karşılık gelen değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="a0ff6-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a0ff6-171">[Önceki](creating-the-web-application.md)
> [İleri](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="a0ff6-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
