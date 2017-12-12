---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: "EF veritabanıyla ilk ASP.NET MVC: veritabanı değiştirme | Microsoft Docs"
author: tfitzmac
description: "ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 1ffe753812e5eef817f03ab488a28ae5fcefd41e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="7a510-104">EF veritabanıyla ilk ASP.NET MVC: veritabanı değiştirme</span><span class="sxs-lookup"><span data-stu-id="7a510-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="7a510-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7a510-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7a510-106">ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7a510-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="7a510-107">Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7a510-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="7a510-108">Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="7a510-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="7a510-109">Veritabanı yapısında bir güncelleştirme yapmadan ve web uygulama boyunca bu değişikliği yayılıyor dizisinin bu bölümü odaklanır.</span><span class="sxs-lookup"><span data-stu-id="7a510-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="7a510-110">Bir sütun ekleyin</span><span class="sxs-lookup"><span data-stu-id="7a510-110">Add a column</span></span>

<span data-ttu-id="7a510-111">Bir tablo yapısı veritabanınızda güncelleştirirseniz, değişikliğinizin veri model, Görünüm ve denetleyici yayılır emin olmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="7a510-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="7a510-112">Bu öğretici için yeni bir sütun Öğrenci Orta adını kaydetmek için Öğrenci tabloya ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="7a510-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="7a510-113">Bu sütun eklemek için veritabanı projesi açın ve Student.sql dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="7a510-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="7a510-114">Tasarımcı veya T-SQL kodunu adlı bir sütun ekler **MiddleName** , bir NVARCHAR(50) ve NULL değerlere izin verir.</span><span class="sxs-lookup"><span data-stu-id="7a510-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![İkinci adı ekleyin](changing-the-database/_static/image1.png)

<span data-ttu-id="7a510-116">Bu değişiklik, veritabanı projesi (veya F5) başlatarak yerel veritabanınızı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="7a510-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="7a510-117">Yeni alanın tablosuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="7a510-117">The new field is added to the table.</span></span> <span data-ttu-id="7a510-118">SQL Server nesne Gezgini'nde onu görmüyorsanız bölmesindeki Yenile düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="7a510-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Yeni bir sütun Göster](changing-the-database/_static/image2.png)

<span data-ttu-id="7a510-120">Veritabanı tablosunda yeni bir sütun var, ancak veri modeli sınıfında şu anda yok.</span><span class="sxs-lookup"><span data-stu-id="7a510-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="7a510-121">Model, yeni bir sütun eklemek için güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="7a510-121">You must update the model to include your new column.</span></span> <span data-ttu-id="7a510-122">İçinde **modelleri** klasörü, açık **ContosoModel.edmx** modeli diyagramını görüntülemek için dosya.</span><span class="sxs-lookup"><span data-stu-id="7a510-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="7a510-123">Öğrenci model MiddleName özelliği içermiyor dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="7a510-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="7a510-124">Herhangi bir yere tasarım yüzeyine sağ tıklatın ve seçin **güncelleştirme modeli veritabanından**.</span><span class="sxs-lookup"><span data-stu-id="7a510-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![Bir güncelleştirme modeli](changing-the-database/_static/image3.png)

<span data-ttu-id="7a510-126">Güncelleştirme Sihirbazı'nda seçin **yenileme** sekmesi ve **Öğrenci** tablo.</span><span class="sxs-lookup"><span data-stu-id="7a510-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Güncelleştirme Sihirbazı](changing-the-database/_static/image4.png)

<span data-ttu-id="7a510-128">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7a510-128">Click **Finish**.</span></span>

<span data-ttu-id="7a510-129">Güncelleştirme işlemi tamamlandıktan sonra veritabanı şemasını yeni içermektedir **MiddleName** özelliği.</span><span class="sxs-lookup"><span data-stu-id="7a510-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="7a510-130">Kaydet **ContosoModel.edmx** dosya.</span><span class="sxs-lookup"><span data-stu-id="7a510-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="7a510-131">Bu dosya için yayılması yeni bir özellik için kaydetmelisiniz **Student.cs** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7a510-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="7a510-132">Şimdi, veritabanı ve modeli güncelleştirdiniz.</span><span class="sxs-lookup"><span data-stu-id="7a510-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="7a510-133">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7a510-133">Build the solution.</span></span>

<span data-ttu-id="7a510-134">Ne yazık ki, görünümleri yeni özellik hala içermez.</span><span class="sxs-lookup"><span data-stu-id="7a510-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="7a510-135">Görünümlerini güncelleştirmek için iki seçeneğiniz vardır - ya da görünümleri Öğrenci sınıfı için askılamayı yeniden ekleyerek yeniden oluşturabileceğiniz veya varolan görünümlerinize yeni özellik el ile ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7a510-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="7a510-136">Bu öğreticide, yapı iskelesi ekleyeceksiniz yeniden özelleştirilmiş değişiklikleri otomatik olarak oluşturulan görünümlerine yaptığınız değil çünkü.</span><span class="sxs-lookup"><span data-stu-id="7a510-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="7a510-137">Görünümlere değişiklikler yaptığınızda ve bu değişiklikleri kaybetmek istiyor musunuz özelliğini el ile eklemeyi düşünebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7a510-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="7a510-138">Görünümleri yeniden oluşturulan emin olmak için silme **Öğrenciler** klasörü altında **görünümleri**ve silme **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="7a510-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="7a510-139">Sonra sağ tıklatın **denetleyicileri** klasörü ve eklemek için askılamayı **Öğrenci** modeli.</span><span class="sxs-lookup"><span data-stu-id="7a510-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="7a510-140">Yeniden, denetleyici adı **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="7a510-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="7a510-141">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7a510-141">Select **OK**.</span></span>

<span data-ttu-id="7a510-142">Görünümleri artık MiddleName özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="7a510-142">The views now contain the MiddleName property.</span></span>

![Orta adını göster](changing-the-database/_static/image5.png)

<span data-ttu-id="7a510-144">Sonraki bölümde Öğrenci kayıtla ilgili ayrıntıları gösteren görünümü özelleştirmek için kod ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="7a510-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7a510-145">[Önceki](generating-views.md)
[sonraki](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="7a510-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
