---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: "EF veritabanıyla ilk ASP.NET MVC: bir görünümü özelleştirme | Microsoft Docs"
author: tfitzmac
description: "ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: af9609396cff18b08824732731ddb9c5cca578fa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="27c9d-104">EF veritabanıyla ilk ASP.NET MVC: bir görünümü özelleştirme</span><span class="sxs-lookup"><span data-stu-id="27c9d-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="27c9d-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="27c9d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="27c9d-106">ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27c9d-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="27c9d-107">Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="27c9d-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="27c9d-108">Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="27c9d-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="27c9d-109">Bu serinin parçası sunu geliştirmek için otomatik olarak oluşturulan görünümleri değiştirme odaklanır.</span><span class="sxs-lookup"><span data-stu-id="27c9d-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="27c9d-110">Kayıtlı kurslar Öğrenci Ayrıntıları Ekle</span><span class="sxs-lookup"><span data-stu-id="27c9d-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="27c9d-111">Oluşturulan kod, uygulamanız için iyi bir başlangıç noktası sunar, ancak bu mutlaka uygulamanızda gereken işlevselliğin tamamını sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="27c9d-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="27c9d-112">Uygulamanızı belirli gereksinimlerini karşılamak için kodu özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27c9d-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="27c9d-113">Şu anda, uygulamanız için seçilen Öğrenci kayıtlı kurslar görüntülemez.</span><span class="sxs-lookup"><span data-stu-id="27c9d-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="27c9d-114">Bu bölümde, her Öğrenci için kayıtlı kurslar ekleyecek **ayrıntıları** Öğrenci için görünümü.</span><span class="sxs-lookup"><span data-stu-id="27c9d-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="27c9d-115">Açık **Students/Details.cshtml**ve son aşağıda &lt;/dl&gt; sekmesi, ancak kapatmadan önce &lt;/div&gt; etiketi, aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="27c9d-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="27c9d-116">Bu kod kayıt tabloda seçilen Öğrenci için her kayıt için bir satır görüntüleyen bir tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27c9d-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="27c9d-117">**Görüntü** yöntemi HTML ifadeyi temsil eden nesne için (ModelItem) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27c9d-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="27c9d-118">Görüntüleme yöntemi kullanın (yalnızca özellik değeri kodda katıştırma) yerine değeri, türü ve şablon türü için doğru temelinde biçimlendirildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="27c9d-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="27c9d-119">Bu örnekte, her ifade döngüsünde geçerli kayıttaki tek bir özellik döndürür ve metin olarak işlenir ilkel türler değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="27c9d-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="27c9d-120">Öğrenciler/dizin görünümüne yeniden göz atın ve seçim **ayrıntıları** Öğrenciler biri için.</span><span class="sxs-lookup"><span data-stu-id="27c9d-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="27c9d-121">Kayıtlı kurslar görünüme dahil görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="27c9d-121">You will see the enrolled courses have been included in the view.</span></span>

![kayıt ile Öğrenci](customizing-a-view/_static/image1.png)

>[!div class="step-by-step"]
<span data-ttu-id="27c9d-123">[Önceki](changing-the-database.md)
[sonraki](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="27c9d-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
