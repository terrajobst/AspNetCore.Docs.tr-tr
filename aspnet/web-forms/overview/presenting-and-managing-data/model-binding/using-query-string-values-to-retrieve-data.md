---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Model bağlama ile verileri filtrelemek ve web formları için sorgu dizesi değerlerini kullanarak | Microsoft Docs
author: tfitzmac
description: Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="73436-104">Filtre veri için sorgu dizesi değerlerini model bağlama ve web forms ile kullanma</span><span class="sxs-lookup"><span data-stu-id="73436-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="73436-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="73436-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="73436-106">Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="73436-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="73436-107">Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="73436-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="73436-108">Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.</span><span class="sxs-lookup"><span data-stu-id="73436-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="73436-109">Bu öğretici, sorgu dizesinde bir değer geçirmek ve model bağlama üzerinden veri almak için bu değeri kullanın gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="73436-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="73436-110">Bu öğreticide oluşturduğunuz proje inşa edilmiştir [önceki](retrieving-data.md) seri bölümlerini.</span><span class="sxs-lookup"><span data-stu-id="73436-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="73436-111">Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede</span><span class="sxs-lookup"><span data-stu-id="73436-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="73436-112">İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="73436-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="73436-113">Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="73436-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="73436-114">Yapı</span><span class="sxs-lookup"><span data-stu-id="73436-114">What you'll build</span></span>

<span data-ttu-id="73436-115">Bu öğreticide, gerekir:</span><span class="sxs-lookup"><span data-stu-id="73436-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="73436-116">Öğrencinin için kayıtlı kurslar göstermek için yeni bir sayfa ekleyin</span><span class="sxs-lookup"><span data-stu-id="73436-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="73436-117">Sorgu dizesindeki bir değere göre seçilen Öğrenci için kayıtlı kurslar alma</span><span class="sxs-lookup"><span data-stu-id="73436-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="73436-118">Sorgu dize değerine sahip bir köprüyü kılavuz görünümünden yeni sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="73436-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="73436-119">Bu öğreticideki adımlardan yaptıklarınızın için oldukça benzer önceki içinde [öğretici](sorting-paging-and-filtering-data.md) görüntülenen Öğrenciler açılır listeden kullanıcı seçime göre filtre uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="73436-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="73436-120">Bu öğreticide kullandığınız **denetim** parametre değeri denetimden geldiğini belirtmek için select yöntemi özniteliği.</span><span class="sxs-lookup"><span data-stu-id="73436-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="73436-121">Bu öğreticide kullanacaksınız **QueryString** parametre değeri Sorgu dizesinden geldiğini belirtmek için select yöntemi özniteliği.</span><span class="sxs-lookup"><span data-stu-id="73436-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="73436-122">Öğrencinin kurslar görüntülemek için yeni bir sayfa ekleyin</span><span class="sxs-lookup"><span data-stu-id="73436-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="73436-123">Site.master ana sayfayı kullanan yeni bir web formu ekleyin ve sayfanın adını **kurslar**.</span><span class="sxs-lookup"><span data-stu-id="73436-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="73436-124">İçinde **Courses.aspx** dosya, seçilen Öğrenci için kursları görüntülemek için bir kılavuz görünüm ekleyin.</span><span class="sxs-lookup"><span data-stu-id="73436-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="73436-125">Select yöntemi tanımlayın</span><span class="sxs-lookup"><span data-stu-id="73436-125">Define the select method</span></span>

<span data-ttu-id="73436-126">İçinde **Courses.aspx.cs**, kılavuz görünümünün belirtilen adla select yöntemi ekleyecek **SelectMethod** özelliği.</span><span class="sxs-lookup"><span data-stu-id="73436-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="73436-127">Bu yöntemde, öğrencinin kurslar almak için sorgu tanımlayın ve parametre için parametre olarak aynı ada sahip bir sorgu dizesi değerinden geldiğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="73436-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="73436-128">İlk olarak, aşağıdaki ekleyin **kullanarak** deyimleri.</span><span class="sxs-lookup"><span data-stu-id="73436-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="73436-129">Ardından, Courses.aspx.cs için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="73436-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="73436-130">Sorgu dizesi özniteliği StudentID adlı bir sorgu dizesi değeri otomatik olarak bu yöntem parametresinde atandığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="73436-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="73436-131">Sorgu dizesi değeri ile köprü ekleme</span><span class="sxs-lookup"><span data-stu-id="73436-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="73436-132">Students.aspx ızgara görünümünde yeni kurslar sayfanıza bağlanan bir köprü alanı ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="73436-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="73436-133">Köprü öğrencinin kimliğine sahip bir sorgu dizesi değerini içerir.</span><span class="sxs-lookup"><span data-stu-id="73436-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="73436-134">Students.aspx içinde aşağıdaki alan hemen altına alan ızgara görünümü sütunları için toplam iadeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="73436-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="73436-135">Uygulamayı çalıştırın ve ızgara görünümü şimdi kurslar bağlantı içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="73436-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Köprü ekleme](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="73436-137">Bağlantılardan birine tıkladığınızda, bu öğrencinin kayıtlı kurslar görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="73436-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![kursları göster](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="73436-139">Sonuç</span><span class="sxs-lookup"><span data-stu-id="73436-139">Conclusion</span></span>

<span data-ttu-id="73436-140">Bu öğreticide, bir sorgu dizesi değerini içeren bir bağlantı eklendi.</span><span class="sxs-lookup"><span data-stu-id="73436-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="73436-141">Select yöntemindeki parametre değeri, sorgu dizesi değeri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="73436-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="73436-142">Sonraki [öğretici](adding-business-logic-layer.md), kodu bir iş mantığı katmanı ve veri erişim katmanı olarak arka plan kodu dosyalarından taşır.</span><span class="sxs-lookup"><span data-stu-id="73436-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="73436-143">[Önceki](integrating-jquery-ui.md)
> [sonraki](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="73436-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
