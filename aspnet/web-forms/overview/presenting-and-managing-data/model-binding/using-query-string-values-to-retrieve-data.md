---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Sorgu dizesi değerlerini kullanarak model bağlama ile verileri filtreleme ve web forms | Microsoft Docs
author: tfitzmac
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: fc4ec64cf583f25eca6795f7ff9215f025170054
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754221"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="53f38-104">Sorgu dizesi değerlerini verilere filtre uygulamak, model bağlama ve web forms ile kullanma</span><span class="sxs-lookup"><span data-stu-id="53f38-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="53f38-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="53f38-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="53f38-106">Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="53f38-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="53f38-107">Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar.</span><span class="sxs-lookup"><span data-stu-id="53f38-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="53f38-108">Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.</span><span class="sxs-lookup"><span data-stu-id="53f38-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="53f38-109">Bu öğreticide, sorgu dizesinde bir değer geçirmek ve model bağlama aracılığıyla veri almak için bu değeri gösterilir.</span><span class="sxs-lookup"><span data-stu-id="53f38-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="53f38-110">Bu öğreticide oluşturulan proje geliştirir [önceki](retrieving-data.md) dizisinin bölümleri.</span><span class="sxs-lookup"><span data-stu-id="53f38-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="53f38-111">Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projeyi C# veya vb</span><span class="sxs-lookup"><span data-stu-id="53f38-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="53f38-112">İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="53f38-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="53f38-113">Bu öğreticide gösterilen Visual Studio 2013 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="53f38-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="53f38-114">Derleme</span><span class="sxs-lookup"><span data-stu-id="53f38-114">What you'll build</span></span>

<span data-ttu-id="53f38-115">Bu öğreticide, gerekir:</span><span class="sxs-lookup"><span data-stu-id="53f38-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="53f38-116">Bir öğrenci için kayıtlı kursları göstermek için yeni bir sayfa ekleyin</span><span class="sxs-lookup"><span data-stu-id="53f38-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="53f38-117">Sorgu dizesinde bir değere göre seçilen Öğrenci için kayıtlı kursları Al</span><span class="sxs-lookup"><span data-stu-id="53f38-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="53f38-118">Bir sorgu dizesi değerini içeren bir köprü kılavuz görünümünden yeni sayfaya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="53f38-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="53f38-119">Bu öğreticide, yaptığınız için oldukça benzer adımlarla önceki [öğretici](sorting-paging-and-filtering-data.md) bir açılan liste kullanıcı seçimine göre görüntülenen öğrencilere filtre uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="53f38-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="53f38-120">Bu öğreticide kullandığınız **denetimi** select yöntemindeki parametre değeri bir denetimden geldiğini belirtmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="53f38-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="53f38-121">Bu öğreticide kullanacaksınız **QueryString** select yöntemindeki parametre değeri, Sorgu dizesinden geldiğini belirtmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="53f38-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="53f38-122">Bir öğrenci kursları görüntülemek için Yeni Sayfa Ekle</span><span class="sxs-lookup"><span data-stu-id="53f38-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="53f38-123">Site.master ana sayfa kullanan yeni bir web formu ekleyin ve sayfayı adlandırın **kursları**.</span><span class="sxs-lookup"><span data-stu-id="53f38-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="53f38-124">İçinde **Courses.aspx** Kurslar için seçilen Öğrenci görüntülemek için bir kılavuz görünümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="53f38-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="53f38-125">Select yöntemi tanımlayın</span><span class="sxs-lookup"><span data-stu-id="53f38-125">Define the select method</span></span>

<span data-ttu-id="53f38-126">İçinde **Courses.aspx.cs**, select yöntemi kılavuz görünümünün belirtilen adla ekleyeceksiniz **SelectMethod** özelliği.</span><span class="sxs-lookup"><span data-stu-id="53f38-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="53f38-127">Bu yöntemde, bir öğrencinin kurslar almak için sorgu tanımlama ve parametre, parametre olarak aynı ada sahip bir sorgu dizesi değerinden geldiğini belirtmek.</span><span class="sxs-lookup"><span data-stu-id="53f38-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="53f38-128">İlk olarak, aşağıdaki ekleyin **kullanarak** deyimleri.</span><span class="sxs-lookup"><span data-stu-id="53f38-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="53f38-129">Ardından, Courses.aspx.cs için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="53f38-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="53f38-130">Sorgu dizesi öznitelik StudentID adlı bir sorgu dizesi değerini bu yöntem parametresi için otomatik olarak atandığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="53f38-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="53f38-131">Sorgu dizesi değeri ile köprü ekleme</span><span class="sxs-lookup"><span data-stu-id="53f38-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="53f38-132">Students.aspx ızgara görünümünde, yeni kursları sayfasına bağlayan bir köprü alanına ekler.</span><span class="sxs-lookup"><span data-stu-id="53f38-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="53f38-133">Köprüyü öğrencinin kimliğine sahip bir sorgu dizesi değerini içerir.</span><span class="sxs-lookup"><span data-stu-id="53f38-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="53f38-134">Students.aspx içinde aşağıdaki alan ızgara görünümü sütunları hemen altındaki alanı için toplam kredi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="53f38-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="53f38-135">Uygulamayı çalıştırmak ve ızgara görünümü artık kursları bağlantı içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="53f38-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Köprü ekleme](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="53f38-137">Bağlantılardan birine tıkladığınızda, bu öğrencinin kayıtlı kursları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="53f38-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![kursları göster](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="53f38-139">Sonuç</span><span class="sxs-lookup"><span data-stu-id="53f38-139">Conclusion</span></span>

<span data-ttu-id="53f38-140">Bu öğreticide, bir sorgu dizesi değerini içeren bir bağlantı eklendi.</span><span class="sxs-lookup"><span data-stu-id="53f38-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="53f38-141">Bu sorgu dizesi değerini select yöntemi parametresinin değeri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53f38-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="53f38-142">Sonraki [öğretici](adding-business-logic-layer.md), kod ile iş mantığı katmanı ve veri erişim katmanı arka plan kod dosyaları taşınır.</span><span class="sxs-lookup"><span data-stu-id="53f38-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="53f38-143">[Önceki](integrating-jquery-ui.md)
> [İleri](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="53f38-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
