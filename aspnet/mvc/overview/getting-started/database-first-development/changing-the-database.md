---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Öğretici: Veritabanı ile ASP.NET MVC uygulaması için EF veritabanı ilk değiştirme'
description: Bu öğreticide bir güncelleştirme yapmak için veritabanı yapısı ve web uygulama boyunca bu değişiklik yayma odaklanır.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667641"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="20e7e-103">Öğretici: Veritabanı ile ASP.NET MVC uygulaması için EF veritabanı ilk değiştirme</span><span class="sxs-lookup"><span data-stu-id="20e7e-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="20e7e-104">MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20e7e-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="20e7e-105">Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="20e7e-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="20e7e-106">Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="20e7e-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="20e7e-107">Bu öğreticide bir güncelleştirme yapmak için veritabanı yapısı ve web uygulama boyunca bu değişiklik yayma odaklanır.</span><span class="sxs-lookup"><span data-stu-id="20e7e-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="20e7e-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="20e7e-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="20e7e-109">Sütun ekleme</span><span class="sxs-lookup"><span data-stu-id="20e7e-109">Add a column</span></span>
> * <span data-ttu-id="20e7e-110">Görünümlere özelliği ekleme</span><span class="sxs-lookup"><span data-stu-id="20e7e-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20e7e-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="20e7e-111">Prerequisites</span></span>

* [<span data-ttu-id="20e7e-112">Görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="20e7e-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="20e7e-113">Sütun ekleme</span><span class="sxs-lookup"><span data-stu-id="20e7e-113">Add a column</span></span>

<span data-ttu-id="20e7e-114">Bir tablonun yapısını veritabanınızda güncelleştirirseniz, değişikliğiniz veri modeli, görünümler ve denetleyici yayıldığından emin olmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="20e7e-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="20e7e-115">Bu öğreticide, Öğrenci ikinci adını kaydetmek için Öğrenci tabloya yeni bir sütun ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="20e7e-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="20e7e-116">Bu sütunu eklemek için veritabanı projesini açın ve Student.sql dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="20e7e-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="20e7e-117">Tasarımcı veya T-SQL kodu adlı bir sütun eklemek **MiddleName** , bir NVARCHAR(50) ve NULL değerlere izin verir.</span><span class="sxs-lookup"><span data-stu-id="20e7e-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="20e7e-118">Bu değişiklik, veritabanı projesi (veya F5) başlatarak yerel veritabanınıza dağıtın.</span><span class="sxs-lookup"><span data-stu-id="20e7e-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="20e7e-119">Yeni alan tablosuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="20e7e-119">The new field is added to the table.</span></span> <span data-ttu-id="20e7e-120">SQL Server nesne Gezgini'nde, görmüyorsanız bölmeyi yenile düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="20e7e-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Yeni bir sütun Göster](changing-the-database/_static/image2.png)

<span data-ttu-id="20e7e-122">Veritabanı tablosunda yeni bir sütun var, ancak veri modeli sınıfında şu anda yok.</span><span class="sxs-lookup"><span data-stu-id="20e7e-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="20e7e-123">Model, yeni bir sütun içerecek şekilde güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="20e7e-123">You must update the model to include your new column.</span></span> <span data-ttu-id="20e7e-124">İçinde **modelleri** açık klasör **ContosoModel.edmx** modeli diyagramını görüntülemek için dosya.</span><span class="sxs-lookup"><span data-stu-id="20e7e-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="20e7e-125">Öğrenci model MiddleName özelliği içermiyor dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="20e7e-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="20e7e-126">Tasarım yüzeyinde herhangi bir yere sağ tıklatıp seçin **veritabanından bir güncelleştirme modeli**.</span><span class="sxs-lookup"><span data-stu-id="20e7e-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="20e7e-127">Güncelleştirme Sihirbazı'nda seçin **Yenile** sekmesini seçip **tabloları** > **dbo** > **Öğrenci**.</span><span class="sxs-lookup"><span data-stu-id="20e7e-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="20e7e-128">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="20e7e-128">Click **Finish**.</span></span>

<span data-ttu-id="20e7e-129">Güncelleştirme işlemi tamamlandıktan sonra veritabanı diyagramı yeni içerir **MiddleName** özelliği.</span><span class="sxs-lookup"><span data-stu-id="20e7e-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="20e7e-130">Kaydet **ContosoModel.edmx** dosya.</span><span class="sxs-lookup"><span data-stu-id="20e7e-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="20e7e-131">Yeni bir özellik için yayılması için bu dosyayı kaydetmeniz gerekir **Student.cs** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="20e7e-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="20e7e-132">Şimdi, veritabanı ve modeli de güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="20e7e-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="20e7e-133">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="20e7e-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="20e7e-134">Görünümlere özelliği ekleme</span><span class="sxs-lookup"><span data-stu-id="20e7e-134">Add the property to the views</span></span>

<span data-ttu-id="20e7e-135">Ne yazık ki, görünümler, yeni özellik hala içermez.</span><span class="sxs-lookup"><span data-stu-id="20e7e-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="20e7e-136">Görünümlerin güncelleştirilmesi için iki seçeneğiniz vardır - ya da görünümleri yapı iskelesi Öğrenci sınıfı için bir kez yeniden ekleyerek yeniden oluşturabilirsiniz veya yeni özellik mevcut görünümlerinizde el ile ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20e7e-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="20e7e-137">Bu öğreticide, yapı iskelesi ekleyeceksiniz yeniden otomatik olarak oluşturulan görünümler için özelleştirilmiş tüm değişiklikleri yapmasanız olduğundan.</span><span class="sxs-lookup"><span data-stu-id="20e7e-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="20e7e-138">Değişiklikleri görünümlerine yaptığınızda ve değişiklikleri kaybetmek istiyor musunuz özelliği el ile eklemeyi düşünebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20e7e-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="20e7e-139">Görünümleri yeniden oluşturulmuş olduğundan emin olmak için silme **Öğrenciler** klasörü altında **görünümleri**ve silme **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="20e7e-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="20e7e-140">Ardından, sağ tıklayarak **denetleyicileri** klasörü ve için yapı iskelesi Ekle **Öğrenci** modeli.</span><span class="sxs-lookup"><span data-stu-id="20e7e-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="20e7e-141">Denetleyici yeniden adlandırın **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="20e7e-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="20e7e-142">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="20e7e-142">Select **Add**.</span></span>

<span data-ttu-id="20e7e-143">Çözümü yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="20e7e-143">Build the solution again.</span></span> <span data-ttu-id="20e7e-144">Görünümler, artık MiddleName özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="20e7e-144">The views now contain the MiddleName property.</span></span>

![İkinci Ad Göster](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="20e7e-146">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="20e7e-146">Next steps</span></span>

<span data-ttu-id="20e7e-147">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="20e7e-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="20e7e-148">Bir sütun eklendi</span><span class="sxs-lookup"><span data-stu-id="20e7e-148">Added a column</span></span>
> * <span data-ttu-id="20e7e-149">Görünümlere özelliği eklendi</span><span class="sxs-lookup"><span data-stu-id="20e7e-149">Added the property to the views</span></span>

<span data-ttu-id="20e7e-150">Bir öğrenci kayıt hakkındaki ayrıntıları göstermek için görünümün nasıl özelleştirileceğini öğrenmek için sonraki öğreticiye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="20e7e-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="20e7e-151">Bir görünümü özelleştirme</span><span class="sxs-lookup"><span data-stu-id="20e7e-151">Customize a view</span></span>](customizing-a-view.md)