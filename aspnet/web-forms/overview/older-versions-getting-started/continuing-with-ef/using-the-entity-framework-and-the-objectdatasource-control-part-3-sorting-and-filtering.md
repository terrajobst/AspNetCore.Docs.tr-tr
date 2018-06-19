---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, Bölüm 3: sıralama ve filtreleme | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri ile çalışmaya başlama Entity Framework 4.0 öğretici serisi tarafından oluşturulan Contoso University web uygulaması üzerinde oluşturur. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887661"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="8e89d-104">Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, Bölüm 3: sıralama ve filtreleme</span><span class="sxs-lookup"><span data-stu-id="8e89d-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="8e89d-105">by [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8e89d-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="8e89d-106">Bu öğretici seri tarafından oluşturulan Contoso University web uygulaması üzerinde derlemeler [Entity Framework 4.0 ile çalışmaya başlama](https://asp.net/entity-framework/tutorials#Getting%20Started) öğretici serisi.</span><span class="sxs-lookup"><span data-stu-id="8e89d-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="8e89d-107">Önceki öğreticileri tamamlanmadı, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturduğunuz.</span><span class="sxs-lookup"><span data-stu-id="8e89d-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="8e89d-108">Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici seri tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8e89d-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="8e89d-109">Öğreticiler hakkında sorularınız varsa, bunları nakledebilirsiniz [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="8e89d-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="8e89d-110">Önceki öğreticide Entity Framework kullanan bir n katmanlı web uygulama havuzu desende uygulanan ve `ObjectDataSource` denetim.</span><span class="sxs-lookup"><span data-stu-id="8e89d-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="8e89d-111">Bu öğretici, sıralama ve filtreleme yapabilir ve ana-ayrıntı senaryolarını ele gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8e89d-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="8e89d-112">Aşağıdaki geliştirmeleri ekleyeceksiniz *Departments.aspx* sayfa:</span><span class="sxs-lookup"><span data-stu-id="8e89d-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="8e89d-113">Departmanlar adına göre seçmek yapmalarına izin vermek için bir metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="8e89d-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="8e89d-114">Kılavuzda gösterilen her bölüm için kurslar listesi.</span><span class="sxs-lookup"><span data-stu-id="8e89d-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="8e89d-115">Sütun başlıklarını tıklatarak sıralama olanağı.</span><span class="sxs-lookup"><span data-stu-id="8e89d-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="8e89d-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8e89d-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="8e89d-117">GridView sütunları sıralama özelliği ekleme</span><span class="sxs-lookup"><span data-stu-id="8e89d-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="8e89d-118">Açık *Departments.aspx* sayfasında ve ekleme bir `SortParameterName="sortExpression"` özniteliğini `ObjectDataSource` adlı Denetim `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="8e89d-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="8e89d-119">(Daha sonra oluşturacağınız bir `GetDepartments` yönteminin adlı bir parametre alan `sortExpression`.) Denetimin açılış etiketi için biçimlendirme şimdi aşağıdaki örneğe benzer.</span><span class="sxs-lookup"><span data-stu-id="8e89d-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="8e89d-120">Ekleme `AllowSorting="true"` özniteliği için açılış etiketinde `GridView` denetim.</span><span class="sxs-lookup"><span data-stu-id="8e89d-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="8e89d-121">Denetimin açılış etiketi için biçimlendirme şimdi aşağıdaki örneğe benzer.</span><span class="sxs-lookup"><span data-stu-id="8e89d-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="8e89d-122">İçinde *Departments.aspx.cs*, varsayılan sıralama düzeni aranarak `GridView` denetimin `Sort` yönteminden `Page_Load` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8e89d-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="8e89d-123">İş mantığı sınıfı veya depo sınıfını sıralar veya filtreler kod ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8e89d-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="8e89d-124">İş mantığı sınıfında bunu yaparsanız, verileri veritabanından alındıktan sonra iş mantığı sınıfı ile çalıştığından sıralama veya filtreleme iş yapılır bir `IEnumerable` depo tarafından döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="8e89d-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="8e89d-125">Sıralama ve filtreleme depo sınıfını kodda ekler ve LINQ ifadesi önce yapın ya da sorgu nesnesi için dönüştürülmüş bir `IEnumerable` nesnesi, komutlarınızı geçirilecektir aracılığıyla, genellikle daha verimli bir işlem için veritabanı.</span><span class="sxs-lookup"><span data-stu-id="8e89d-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="8e89d-126">Bu öğreticide, sıralama ve veritabanı tarafından yapılması işleme neden olan bir şekilde filtreleme uygulamadan — diğer bir deyişle, depodaki.</span><span class="sxs-lookup"><span data-stu-id="8e89d-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="8e89d-127">Sıralama özelliği eklemek için depo arabirimi ve depo sınıflarını da iş mantığı sınıfı için yeni bir yöntem eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8e89d-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="8e89d-128">İçinde *ISchoolRepository.cs* dosya, yeni bir ekleme `GetDepartments` yönteminin alan bir `sortExpression` döndürülen Departmanlar sıralamak için kullanılacak parametre:</span><span class="sxs-lookup"><span data-stu-id="8e89d-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="8e89d-129">`sortExpression` Parametresi sıralanacak sütunu ve sıralama yönünü belirtin.</span><span class="sxs-lookup"><span data-stu-id="8e89d-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="8e89d-130">Yeni bir yönteme için kodu ekleyin *SchoolRepository.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8e89d-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="8e89d-131">Parametresiz varolan değiştirmek `GetDepartments` yeni yöntemini çağırmak için yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8e89d-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="8e89d-132">Test projesinde aşağıdaki yeni yöntemi ekleyin *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e89d-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="8e89d-133">Sıralanmış bir döndürme bu yöntem bağımlı olduğu tüm birim testleri oluşturmak için olmaya ederse döndürmeden önce sıralamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="8e89d-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="8e89d-134">Yöntemi yalnızca bölümlerin Sıralanmamış liste dönebilmeniz testleri gibi Bu öğreticide oluşturduğunuz olmaz.</span><span class="sxs-lookup"><span data-stu-id="8e89d-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="8e89d-135">İçinde *SchoolBL.cs* dosya, iş mantığı sınıfına aşağıdaki yeni yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8e89d-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="8e89d-136">Bu kod sıralama parametresi deposu yönteme geçirir.</span><span class="sxs-lookup"><span data-stu-id="8e89d-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="8e89d-137">Çalıştırma *Departments.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="8e89d-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="8e89d-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8e89d-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="8e89d-139">Artık herhangi bir sütuna göre sıralamak için sütun başlığını tıklatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8e89d-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="8e89d-140">Sütun zaten sıraladıysanız başlığını tıklatarak sıralama yönünü tersine çevirir.</span><span class="sxs-lookup"><span data-stu-id="8e89d-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="8e89d-141">Bir arama kutusu ekleme</span><span class="sxs-lookup"><span data-stu-id="8e89d-141">Adding a Search Box</span></span>

<span data-ttu-id="8e89d-142">Bu bölümde, arama metin kutusuna eklemek için bağlantının `ObjectDataSource` denetim parametresini kullanarak denetlemek ve bir yöntem Filtrelemeyi desteklemek için iş mantığı sınıfına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8e89d-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="8e89d-143">Açık *Departments.aspx* sayfasında ve başlık ve ilk arasında aşağıdaki biçimlendirmeleri eklemek `ObjectDataSource` denetimi:</span><span class="sxs-lookup"><span data-stu-id="8e89d-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="8e89d-144">İçinde `ObjectDataSource` adlı Denetim `DepartmentsObjectDataSource`, aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="8e89d-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="8e89d-145">Ekleme bir `SelectParameters` öğesi adlı bir parametre için `nameSearchString` girilen değerin alır `SearchTextBox` denetim.</span><span class="sxs-lookup"><span data-stu-id="8e89d-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="8e89d-146">Değişiklik `SelectMethod` öznitelik değeri için `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="8e89d-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="8e89d-147">(Daha sonra bu yöntem oluşturacaksınız.)</span><span class="sxs-lookup"><span data-stu-id="8e89d-147">(You'll create this method later.)</span></span>

<span data-ttu-id="8e89d-148">İşaretleme için `ObjectDataSource` denetim şimdi aşağıdaki örnekte benzer:</span><span class="sxs-lookup"><span data-stu-id="8e89d-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="8e89d-149">İçinde *ISchoolRepository.cs*, ekleme bir `GetDepartmentsByName` yönteminin hem de alan `sortExpression` ve `nameSearchString` Parametreler:</span><span class="sxs-lookup"><span data-stu-id="8e89d-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="8e89d-150">İçinde *SchoolRepository.cs*, aşağıdaki yeni yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8e89d-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="8e89d-151">Bu kodu kullanan bir `Where` yöntemi arama dizesini içeren öğeleri seçin.</span><span class="sxs-lookup"><span data-stu-id="8e89d-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="8e89d-152">Arama dizesi boş ise, tüm kayıtlar seçilir.</span><span class="sxs-lookup"><span data-stu-id="8e89d-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="8e89d-153">Yöntem çağrıları birlikte şöyle bir deyim belirttiğinizde unutmayın (`Include`, ardından `OrderBy`, ardından `Where`), `Where` yöntemi her zaman son olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8e89d-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="8e89d-154">Varolan değiştirme `GetDepartments` yönteminin alan bir `sortExpression` yeni yöntemini çağırmak için parametre:</span><span class="sxs-lookup"><span data-stu-id="8e89d-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="8e89d-155">İçinde *MockSchoolRepository.cs* test projesinde aşağıdaki yeni yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8e89d-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="8e89d-156">İçinde *SchoolBL.cs*, aşağıdaki yeni yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8e89d-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="8e89d-157">Çalıştırma *Departments.aspx* sayfasında ve seçim mantığı çalıştığından emin olmak için bir arama dizesi girin.</span><span class="sxs-lookup"><span data-stu-id="8e89d-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="8e89d-158">Metin kutusunu boş bırakın ve tüm kayıtların döndürüldüğünden emin olmak için bir arama deneyin.</span><span class="sxs-lookup"><span data-stu-id="8e89d-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="8e89d-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="8e89d-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="8e89d-160">Her kılavuz satır için ayrıntıları sütun ekleme</span><span class="sxs-lookup"><span data-stu-id="8e89d-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="8e89d-161">Ardından, tüm kılavuz sağ hücrede görüntülenen her bölüm için kurslar görmek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="8e89d-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="8e89d-162">Bunu yapmak için iç içe bir kullanacağınız `GridView` denetimi ve databind verilerden kendisine `Courses` Gezinti özelliğinin `Department` varlık.</span><span class="sxs-lookup"><span data-stu-id="8e89d-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="8e89d-163">Açık *Departments.aspx* ve için biçimlendirme `GridView` denetlemek, bir işleyici belirtmek için `RowDataBound` olay.</span><span class="sxs-lookup"><span data-stu-id="8e89d-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="8e89d-164">Denetimin açılış etiketi için biçimlendirme şimdi aşağıdaki örneğe benzer.</span><span class="sxs-lookup"><span data-stu-id="8e89d-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="8e89d-165">Yeni bir ekleme `TemplateField` öğeden sonra `Administrator` şablonu alan:</span><span class="sxs-lookup"><span data-stu-id="8e89d-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="8e89d-166">Bu biçimlendirme iç içe bir oluşturur `GridView` denetim indirmelere numarası ve başlığı kurslar listesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="8e89d-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="8e89d-167">Databind gerekir çünkü bir veri kaynağı belirtmiyor kodda içinde `RowDataBound` işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="8e89d-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="8e89d-168">Açık *Departments.aspx.cs* ve aşağıdaki işleyicisi ekleme `RowDataBound` olay:</span><span class="sxs-lookup"><span data-stu-id="8e89d-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="8e89d-169">Bu kod alır `Department` olay bağımsız varlıktan dönüştürür `Courses` gezinti özelliği için bir `List` toplama ve iç içe databinds `GridView` koleksiyonuna.</span><span class="sxs-lookup"><span data-stu-id="8e89d-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="8e89d-170">Açık *SchoolRepository.cs* dosya ve istekli yükleme için belirtmek `Courses` çağırarak gezinti özelliği `Include` oluşturduğunuz sorgu nesnesi yönteminde `GetDepartmentsByName` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8e89d-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="8e89d-171">`return` Deyiminde `GetDepartmentsByName` yöntemi şimdi aşağıdaki örnekte benzer.</span><span class="sxs-lookup"><span data-stu-id="8e89d-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="8e89d-172">Sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8e89d-172">Run the page.</span></span> <span data-ttu-id="8e89d-173">Sıralama ve filtreleme daha önce eklediğiniz yetenek ek olarak, GridView denetiminin şimdi her bölüm için iç içe geçmiş indirmelere ayrıntıları gösterir.</span><span class="sxs-lookup"><span data-stu-id="8e89d-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="8e89d-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="8e89d-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="8e89d-175">Bu, sıralama, filtreleme ve ana-ayrıntı senaryoları giriş tamamlar.</span><span class="sxs-lookup"><span data-stu-id="8e89d-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="8e89d-176">Sonraki öğreticide eşzamanlılık nasıl ele alınacağını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8e89d-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8e89d-177">[Önceki](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [sonraki](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="8e89d-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
