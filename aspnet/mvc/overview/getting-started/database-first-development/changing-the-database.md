---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'İlk ASP.NET MVC ile EF veritabanında: veritabanını değiştirme | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 0176274694426a527ed0862b5138919d4cf5c319
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840948"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="dc03c-104">İlk ASP.NET MVC ile EF veritabanında: veritabanını değiştirme</span><span class="sxs-lookup"><span data-stu-id="dc03c-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="dc03c-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dc03c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="dc03c-106">MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc03c-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="dc03c-107">Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="dc03c-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="dc03c-108">Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="dc03c-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="dc03c-109">Bu serinin veritabanı yapısında bir güncelleştirme yapmak ve web uygulama boyunca bu değişiklik yayma odaklanır.</span><span class="sxs-lookup"><span data-stu-id="dc03c-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="dc03c-110">Sütun ekleme</span><span class="sxs-lookup"><span data-stu-id="dc03c-110">Add a column</span></span>

<span data-ttu-id="dc03c-111">Bir tablonun yapısını veritabanınızda güncelleştirirseniz, değişikliğiniz veri modeli, görünümler ve denetleyici yayıldığından emin olmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="dc03c-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="dc03c-112">Bu öğreticide, Öğrenci ikinci adını kaydetmek için Öğrenci tabloya yeni bir sütun ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="dc03c-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="dc03c-113">Bu sütunu eklemek için veritabanı projesini açın ve Student.sql dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="dc03c-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="dc03c-114">Tasarımcı veya T-SQL kodu adlı bir sütun eklemek **MiddleName** , bir NVARCHAR(50) ve NULL değerlere izin verir.</span><span class="sxs-lookup"><span data-stu-id="dc03c-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![İkinci ad ekleyin](changing-the-database/_static/image1.png)

<span data-ttu-id="dc03c-116">Bu değişiklik, veritabanı projesi (veya F5) başlatarak yerel veritabanınıza dağıtın.</span><span class="sxs-lookup"><span data-stu-id="dc03c-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="dc03c-117">Yeni alan tablosuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="dc03c-117">The new field is added to the table.</span></span> <span data-ttu-id="dc03c-118">SQL Server nesne Gezgini'nde, görmüyorsanız bölmeyi yenile düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="dc03c-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Yeni bir sütun Göster](changing-the-database/_static/image2.png)

<span data-ttu-id="dc03c-120">Veritabanı tablosunda yeni bir sütun var, ancak veri modeli sınıfında şu anda yok.</span><span class="sxs-lookup"><span data-stu-id="dc03c-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="dc03c-121">Model, yeni bir sütun içerecek şekilde güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="dc03c-121">You must update the model to include your new column.</span></span> <span data-ttu-id="dc03c-122">İçinde **modelleri** açık klasör **ContosoModel.edmx** modeli diyagramını görüntülemek için dosya.</span><span class="sxs-lookup"><span data-stu-id="dc03c-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="dc03c-123">Öğrenci model MiddleName özelliği içermiyor dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="dc03c-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="dc03c-124">Tasarım yüzeyinde herhangi bir yere sağ tıklatıp seçin **veritabanından bir güncelleştirme modeli**.</span><span class="sxs-lookup"><span data-stu-id="dc03c-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![bir güncelleştirme modeli](changing-the-database/_static/image3.png)

<span data-ttu-id="dc03c-126">Güncelleştirme Sihirbazı'nda seçin **Yenile** sekmesi ve **Öğrenci** tablo.</span><span class="sxs-lookup"><span data-stu-id="dc03c-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Güncelleştirme Sihirbazı](changing-the-database/_static/image4.png)

<span data-ttu-id="dc03c-128">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="dc03c-128">Click **Finish**.</span></span>

<span data-ttu-id="dc03c-129">Güncelleştirme işlemi tamamlandıktan sonra veritabanı diyagramı yeni içerir **MiddleName** özelliği.</span><span class="sxs-lookup"><span data-stu-id="dc03c-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="dc03c-130">Kaydet **ContosoModel.edmx** dosya.</span><span class="sxs-lookup"><span data-stu-id="dc03c-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="dc03c-131">Yeni bir özellik için yayılması için bu dosyayı kaydetmeniz gerekir **Student.cs** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="dc03c-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="dc03c-132">Şimdi, veritabanı ve modeli de güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="dc03c-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="dc03c-133">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="dc03c-133">Build the solution.</span></span>

<span data-ttu-id="dc03c-134">Ne yazık ki, görünümler, yeni özellik hala içermez.</span><span class="sxs-lookup"><span data-stu-id="dc03c-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="dc03c-135">Görünümlerin güncelleştirilmesi için iki seçeneğiniz vardır - ya da görünümleri yapı iskelesi Öğrenci sınıfı için bir kez yeniden ekleyerek yeniden oluşturabilirsiniz veya yeni özellik mevcut görünümlerinizde el ile ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc03c-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="dc03c-136">Bu öğreticide, yapı iskelesi ekleyeceksiniz yeniden otomatik olarak oluşturulan görünümler için özelleştirilmiş tüm değişiklikleri yapmasanız olduğundan.</span><span class="sxs-lookup"><span data-stu-id="dc03c-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="dc03c-137">Değişiklikleri görünümlerine yaptığınızda ve değişiklikleri kaybetmek istiyor musunuz özelliği el ile eklemeyi düşünebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc03c-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="dc03c-138">Görünümleri yeniden oluşturulmuş olduğundan emin olmak için silme **Öğrenciler** klasörü altında **görünümleri**ve silme **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="dc03c-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="dc03c-139">Ardından, sağ tıklayarak **denetleyicileri** klasörü ve için yapı iskelesi Ekle **Öğrenci** modeli.</span><span class="sxs-lookup"><span data-stu-id="dc03c-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="dc03c-140">Denetleyici yeniden adlandırın **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="dc03c-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="dc03c-141">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="dc03c-141">Select **OK**.</span></span>

<span data-ttu-id="dc03c-142">Görünümler, artık MiddleName özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="dc03c-142">The views now contain the MiddleName property.</span></span>

![İkinci Ad Göster](changing-the-database/_static/image5.png)

<span data-ttu-id="dc03c-144">Sonraki bölümde, bir öğrenci kayıt hakkındaki ayrıntıları göstermek için bu görünümü özelleştirmek için kod ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="dc03c-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dc03c-145">[Önceki](generating-views.md)
> [İleri](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="dc03c-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
