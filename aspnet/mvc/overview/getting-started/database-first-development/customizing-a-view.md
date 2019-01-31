---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Öğretici: ASP.NET MVC uygulaması ile EF veritabanı ilk için görünümünü özelleştirme'
description: Bu öğreticide, sunu artırmak için otomatik olarak oluşturulan görünümler değişen odaklanır.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236503"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="94fb6-103">Öğretici: ASP.NET MVC uygulaması ile EF veritabanı ilk için görünümünü özelleştirme</span><span class="sxs-lookup"><span data-stu-id="94fb6-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="94fb6-104">MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="94fb6-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="94fb6-105">Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="94fb6-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="94fb6-106">Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="94fb6-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="94fb6-107">Bu öğreticide, sunu artırmak için otomatik olarak oluşturulan görünümler değişen odaklanır.</span><span class="sxs-lookup"><span data-stu-id="94fb6-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="94fb6-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="94fb6-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="94fb6-109">Kursları Öğrenci ayrıntıları sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="94fb6-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="94fb6-110">Kursları sayfasına eklenen onaylayın</span><span class="sxs-lookup"><span data-stu-id="94fb6-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94fb6-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="94fb6-111">Prerequisites</span></span>

* [<span data-ttu-id="94fb6-112">Veritabanını değiştirme</span><span class="sxs-lookup"><span data-stu-id="94fb6-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="94fb6-113">Kursları Öğrenci ayrıntıları ekleyin</span><span class="sxs-lookup"><span data-stu-id="94fb6-113">Add courses to student detail</span></span>

<span data-ttu-id="94fb6-114">Oluşturulan kod, uygulamanız için iyi bir başlangıç noktası sağlar, ancak bunu mutlaka tüm uygulamanızda ihtiyacınız olan işlevleri sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="94fb6-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="94fb6-115">Kod, uygulamanızın belirli gereksinimlerini karşılayacak şekilde özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="94fb6-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="94fb6-116">Şu anda, uygulamanız için seçilen Öğrenci kayıtlı kursları görüntülemez.</span><span class="sxs-lookup"><span data-stu-id="94fb6-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="94fb6-117">Bu bölümde, her öğrencinin için için kayıtlı kursları ekleyeceksiniz **ayrıntıları** Öğrenci için görünümü.</span><span class="sxs-lookup"><span data-stu-id="94fb6-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="94fb6-118">Açık **görünümleri** > **Öğrenciler** > *Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="94fb6-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="94fb6-119">Son aşağıda &lt;/dl&gt; etiketi, ancak kapatmadan önce &lt;/div&gt; etiketi, aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="94fb6-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="94fb6-120">Bu kod, her kayıt için bir satır için seçilen Öğrenci kayıt tabloda görüntüleyen bir tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="94fb6-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="94fb6-121">**Görünen** yöntemi HTML ifadeyi temsil eden nesne için (ModelItem) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="94fb6-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="94fb6-122">Görüntüleme yöntemi kullanın (yerine yalnızca özellik değeri, kod ekleme) değeri, doğru türü ve şablon türüne göre biçimlendirildiğinde emin olmak için.</span><span class="sxs-lookup"><span data-stu-id="94fb6-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="94fb6-123">Bu örnekte, her ifade tek bir özellik döngüsünde geçerli kaydı döndürür ve metin olarak işlenir, ilkel türler değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="94fb6-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="94fb6-124">Kursları eklenen onaylayın</span><span class="sxs-lookup"><span data-stu-id="94fb6-124">Confirm courses are added</span></span>

<span data-ttu-id="94fb6-125">Çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="94fb6-125">Run the solution.</span></span> <span data-ttu-id="94fb6-126">Tıklayın **Öğrenciler listesi** seçip **ayrıntıları** Öğrenciler biri için.</span><span class="sxs-lookup"><span data-stu-id="94fb6-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="94fb6-127">Kayıtlı kursları Görünümü'nde dahil edilmiş görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="94fb6-127">You will see the enrolled courses have been included in the view.</span></span>

![Öğrenci kaydı](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="94fb6-129">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="94fb6-129">Next steps</span></span>
<span data-ttu-id="94fb6-130">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="94fb6-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="94fb6-131">Öğrenci ayrıntıları sayfasına eklenen kursları</span><span class="sxs-lookup"><span data-stu-id="94fb6-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="94fb6-132">Kursları sayfasına eklenen Onaylandı</span><span class="sxs-lookup"><span data-stu-id="94fb6-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="94fb6-133">Doğrulama gereksinimlerini belirtin ve biçimlendirme görüntülemek için veri ek açıklamaları ekleme konusunda bilgi edinmek için sonraki öğreticiye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="94fb6-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="94fb6-134">Veri doğrulama geliştirin</span><span class="sxs-lookup"><span data-stu-id="94fb6-134">Enhance data validation</span></span>](enhancing-data-validation.md)
