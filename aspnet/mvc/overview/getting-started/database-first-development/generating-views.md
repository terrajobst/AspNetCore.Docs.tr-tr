---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: "EF veritabanıyla ilk ASP.NET MVC: görünümleri oluşturma | Microsoft Docs"
author: tfitzmac
description: "ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 5fccb3c56af0945ec448becff777a3e92dc160d7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="6cafb-104">EF veritabanıyla ilk ASP.NET MVC: görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="6cafb-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="6cafb-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6cafb-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6cafb-106">ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6cafb-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="6cafb-107">Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6cafb-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="6cafb-108">Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6cafb-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="6cafb-109">Bu serinin parçası görünümleri ve denetleyicilerini oluşturmak için ASP.NET yapı İskelesi kullanma üzerine odaklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6cafb-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="6cafb-110">İskele Ekle</span><span class="sxs-lookup"><span data-stu-id="6cafb-110">Add scaffold</span></span>

<span data-ttu-id="6cafb-111">Standart veri işlemleri modeli sınıfları için sağlayacak kodu oluşturmak hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="6cafb-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="6cafb-112">İskele öğesini ekleyerek kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6cafb-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="6cafb-113">Ekleyebilirsiniz yapı iskelesi türü için birçok seçeneğiniz vardır; Bu öğreticide, iskele bir denetleyici ve önceki bölümde oluşturduğunuz Öğrenci ve kayıt modelleri karşılık görünümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="6cafb-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="6cafb-114">Projenizi tutarlılığını korumak için yeni denetleyicisi varolan ekleyecek **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="6cafb-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="6cafb-115">Sağ **denetleyicileri** klasörü ve select **Ekle** – **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="6cafb-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![iskele Ekle](generating-views/_static/image1.png)

<span data-ttu-id="6cafb-117">Seçin **Entity Framework kullanarak MVC 5 denetleyici, görünümleri olan** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="6cafb-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="6cafb-118">Bu seçenek, denetleyici ve güncelleştirme, silme, oluşturma ve veri modelinizi görüntülemek için görünümler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6cafb-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![MVC denetleyicisi ekleme](generating-views/_static/image2.png)

<span data-ttu-id="6cafb-120">Seçin **Öğrenci** model sınıfı ve select **ContosoUniversityEntities** bağlam sınıfını için.</span><span class="sxs-lookup"><span data-stu-id="6cafb-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="6cafb-121">Denetleyici adı olarak tutun **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="6cafb-121">Keep the controller name as **StudentsController**,</span></span>

![Denetleyici belirtin](generating-views/_static/image3.png)

<span data-ttu-id="6cafb-123">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="6cafb-123">Click **Add**.</span></span>

<span data-ttu-id="6cafb-124">Bir hata alırsanız, projeyi önceki bölümde oluşturmak değil çünkü olabilir.</span><span class="sxs-lookup"><span data-stu-id="6cafb-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="6cafb-125">Öyleyse projeyi oluşturmayı deneyin ve ardından kurulmuş öğe yeniden ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6cafb-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="6cafb-126">Kod oluşturma işlemi tamamlandıktan sonra yeni bir denetleyici ve görünüm projenizde görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="6cafb-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Görünümleri Göster](generating-views/_static/image4.png)

<span data-ttu-id="6cafb-128">Aynı adımları yeniden gerçekleştirir, ancak kayıt sınıfı için iskele Ekle.</span><span class="sxs-lookup"><span data-stu-id="6cafb-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="6cafb-129">Tamamlandığında, olmalıdır bir **EnrollmentsController.cs** dosya ve klasörü altında **görünümleri** adlı **kayıtları** oluştur, Sil, ayrıntıları, düzenleme ve dizin ile Görünümler.</span><span class="sxs-lookup"><span data-stu-id="6cafb-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Görünümleri Göster](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="6cafb-131">Bağlantılar için yeni görünümler ekleme</span><span class="sxs-lookup"><span data-stu-id="6cafb-131">Add links to new views</span></span>

<span data-ttu-id="6cafb-132">Yeni görünümlerinize gitmek kolaylaştırmak için birkaç köprüler dizini görünümlerine Öğrenciler ve kayıtları için ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6cafb-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="6cafb-133">Konumundaki dosyayı açın **Views/Home/Index.cshtml**, siteniz için giriş sayfası olduğu.</span><span class="sxs-lookup"><span data-stu-id="6cafb-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="6cafb-134">Jumbotron altına aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6cafb-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="6cafb-135">ActionLink için ilk parametresi bağlantıdaki görüntülenecek metni yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="6cafb-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="6cafb-136">İkinci parametre eylemi ve üçüncü parametre denetleyicinin adı.</span><span class="sxs-lookup"><span data-stu-id="6cafb-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="6cafb-137">Örneğin, ilk bağlantı StudentsController dizin eylemde işaret eder.</span><span class="sxs-lookup"><span data-stu-id="6cafb-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="6cafb-138">Gerçek köprü bu değerleri oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6cafb-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="6cafb-139">İlk bağlantı Sonuçta götüren **Index.cshtml** içinde dosya **görünümler/Öğrenciler** klasör.</span><span class="sxs-lookup"><span data-stu-id="6cafb-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="6cafb-140">Öğrenci görünümleri görüntüler</span><span class="sxs-lookup"><span data-stu-id="6cafb-140">Display student views</span></span>

<span data-ttu-id="6cafb-141">Projenize doğru eklenen kodu Öğrenciler listesini görüntüler ve kullanıcıların Düzenle oluşturmak ya da veritabanında Öğrenci kayıtlarını sil olanak sağlayan doğrular.</span><span class="sxs-lookup"><span data-stu-id="6cafb-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="6cafb-142">Sağ **Views/Home/Index.cshtml** dosyasını bulun ve seçin **tarayıcıda görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="6cafb-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="6cafb-143">Bu sayfada Öğrenciler listesi için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6cafb-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="6cafb-144">Bu sayfada, bağlantılar ve öğrenciler bu verileri değiştirmek için listesi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6cafb-144">On this page, notice the list of the students and links to modify this data.</span></span>

![Öğrenciler listesi](generating-views/_static/image7.png)

<span data-ttu-id="6cafb-146">Tıklatın **Yeni Oluştur** bağlamak ve yeni Öğrenci için bazı değerler sağlayın.</span><span class="sxs-lookup"><span data-stu-id="6cafb-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Yeni Öğrenci oluşturma](generating-views/_static/image8.png)

<span data-ttu-id="6cafb-148">Tıklatın **oluşturma**ve yeni Öğrenci listenize eklenir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6cafb-148">Click **Create**, and notice the new student is added to your list.</span></span>

![Yeni Öğrenci listesiyle](generating-views/_static/image9.png)

<span data-ttu-id="6cafb-150">Seçin **Düzenle** bağlamak ve bazı değerleri Öğrenci için değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6cafb-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![Öğrenci Düzenle](generating-views/_static/image10.png)

<span data-ttu-id="6cafb-152">Tıklatın **kaydetmek**ve Öğrenci kayıt değiştirildi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6cafb-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="6cafb-153">Son olarak, seçin **silmek** tıklayarak kaydını silmek istediğinizi onaylamak ve bağlama **silmek** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="6cafb-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![Öğrenci Sil](generating-views/_static/image11.png)

<span data-ttu-id="6cafb-155">Herhangi bir kod yazmak zorunda kalmadan Öğrenci tablodaki verileri ortak işlemleri görünümleri eklediniz.</span><span class="sxs-lookup"><span data-stu-id="6cafb-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="6cafb-156">Bir alan için metin etiketi veritabanı özelliğine dayalı olduğunu fark etmiş olabilirsiniz (gibi **LastName**) olmayan mutlaka web sayfasında görüntülemek istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="6cafb-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="6cafb-157">Örneğin, etiket olması tercih edebilirsiniz **Soyadı**.</span><span class="sxs-lookup"><span data-stu-id="6cafb-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="6cafb-158">Öğreticide daha sonra bu görüntüleme sorunu düzeltir.</span><span class="sxs-lookup"><span data-stu-id="6cafb-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="6cafb-159">Kayıt görünümleri görüntüler</span><span class="sxs-lookup"><span data-stu-id="6cafb-159">Display enrollment views</span></span>

<span data-ttu-id="6cafb-160">Veritabanınızı Öğrenci ve kayıt tabloları indirmelere ve kayıt tabloları arasında bir-çok ilişkisi arasında bir-çok ilişkisi içerir.</span><span class="sxs-lookup"><span data-stu-id="6cafb-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="6cafb-161">Kayıt görünümleri, bu ilişkileri düzgün bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="6cafb-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="6cafb-162">Seçin ve site için giriş sayfasına gidin **kayıtları listesi** bağlantısını ve ardından **Yeni Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="6cafb-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="6cafb-163">Yeni bir kayıt oluşturmak için bir form görünümü görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6cafb-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="6cafb-164">Özellikle, formun ilişkili tabloları değerlerle doldurulan iki açılan listeleri içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6cafb-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![kaydı oluşturma](generating-views/_static/image12.png)

<span data-ttu-id="6cafb-166">Ayrıca, sağlanan değerlerin doğrulama otomatik olarak tabanlı alanın veri türünü uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6cafb-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="6cafb-167">Sınıf bir sayı gerektirir ve bu nedenle uyumlu olmayan bir değer sağlamak çalışırsanız bir hata iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6cafb-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![doğrulama iletisi](generating-views/_static/image13.png)

<span data-ttu-id="6cafb-169">Otomatik olarak oluşturulan görünümleri veritabanındaki verilerle çalışmak kullanıcıları etkinleştirme doğrulanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6cafb-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="6cafb-170">Bu serideki sonraki öğretici veritabanını güncelleştirmek ve web uygulamasında karşılık gelen değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="6cafb-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6cafb-171">[Önceki](creating-the-web-application.md)
[sonraki](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="6cafb-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
