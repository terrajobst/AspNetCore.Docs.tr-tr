---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sıralama, sayfalama ve model bağlama ve web forms ile verileri filtreleme | Microsoft Docs
author: tfitzmac
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 86ddedb68b8d18057cd2f7d68e795efda33734b1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754035"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="0a80d-104">Sıralama, sayfalama ve model bağlama ve web forms ile verileri filtreleme</span><span class="sxs-lookup"><span data-stu-id="0a80d-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="0a80d-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0a80d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0a80d-106">Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="0a80d-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="0a80d-107">Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar.</span><span class="sxs-lookup"><span data-stu-id="0a80d-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="0a80d-108">Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.</span><span class="sxs-lookup"><span data-stu-id="0a80d-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="0a80d-109">Bu öğreticide, sıralama, sayfalama ve model bağlama aracılığıyla veri filtreleme nasıl ekleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="0a80d-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="0a80d-110">Bu öğreticide oluşturduğunuz ilk proje geliştirir [bölümü](retrieving-data.md) dizi.</span><span class="sxs-lookup"><span data-stu-id="0a80d-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="0a80d-111">Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projeyi C# veya vb</span><span class="sxs-lookup"><span data-stu-id="0a80d-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="0a80d-112">İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="0a80d-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="0a80d-113">Bu öğreticide gösterilen Visual Studio 2013 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="0a80d-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="0a80d-114">Derleme</span><span class="sxs-lookup"><span data-stu-id="0a80d-114">What you'll build</span></span>

<span data-ttu-id="0a80d-115">Bu öğreticide, gerekir:</span><span class="sxs-lookup"><span data-stu-id="0a80d-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="0a80d-116">Verileri sayfalama ve sıralamayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="0a80d-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="0a80d-117">Kullanıcı tarafından bir seçimine göre veri filtreleme etkinleştir</span><span class="sxs-lookup"><span data-stu-id="0a80d-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="0a80d-118">Sıralama Ekle</span><span class="sxs-lookup"><span data-stu-id="0a80d-118">Add sorting</span></span>

<span data-ttu-id="0a80d-119">GridView ' ' sıralama etkinleştirmek çok kolaydır.</span><span class="sxs-lookup"><span data-stu-id="0a80d-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="0a80d-120">Student.aspx dosyasında ayarlamanız yeterlidir **AllowSorting** için **true** GridView içinde.</span><span class="sxs-lookup"><span data-stu-id="0a80d-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="0a80d-121">Ayarlanacak gerekmez bir **SortExpression** DataField otomatik olarak kullanılan her sütun için değer.</span><span class="sxs-lookup"><span data-stu-id="0a80d-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="0a80d-122">GridView sorguyu verileri seçilen değere göre sıralama içerecek şekilde değiştirir.</span><span class="sxs-lookup"><span data-stu-id="0a80d-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="0a80d-123">Aşağıdaki vurgulanmış kodu sıralamayı etkinleştirmek yapmanız gereken ek gösterir.</span><span class="sxs-lookup"><span data-stu-id="0a80d-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="0a80d-124">Web uygulamasını çalıştırmak ve sıralama Öğrenci kayıtları farklı sütundaki değerleri sınayın.</span><span class="sxs-lookup"><span data-stu-id="0a80d-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Sıralama öğrenciler](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="0a80d-126">Sayfalama ekleme</span><span class="sxs-lookup"><span data-stu-id="0a80d-126">Add paging</span></span>

<span data-ttu-id="0a80d-127">Disk belleği etkinleştirme de çok kolaydır.</span><span class="sxs-lookup"><span data-stu-id="0a80d-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="0a80d-128">GridView ' ayarlayın **AllowPaging** özelliğini **true** ayarlayıp **PageSize** özelliğini istediğiniz her sayfada görüntülenecek kayıt sayısı.</span><span class="sxs-lookup"><span data-stu-id="0a80d-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="0a80d-129">Bu öğreticide, 4'e ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0a80d-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="0a80d-130">Web uygulamasını çalıştırın ve kayıtları üzerinde birden çok sayfada tek bir sayfada görüntülenen en fazla 4 kayıtlarla ayrılır artık dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="0a80d-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Sayfalama ekleme](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="0a80d-132">Ertelenen sorgu yürütme uygulama verimliliğini artırır.</span><span class="sxs-lookup"><span data-stu-id="0a80d-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="0a80d-133">Tüm veri kümesini almak yerine, yalnızca geçerli sayfa için kayıtları almak için sorgu GridView değiştirir.</span><span class="sxs-lookup"><span data-stu-id="0a80d-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="0a80d-134">Kayıtları kullanıcı seçime göre filtre uygula</span><span class="sxs-lookup"><span data-stu-id="0a80d-134">Filter records by user selection</span></span>

<span data-ttu-id="0a80d-135">Model bağlama bir model bağlama yöntemin bir parametresinin değeri ayarlamak nasıl atamak sağlayan birkaç öznitelik ekler.</span><span class="sxs-lookup"><span data-stu-id="0a80d-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="0a80d-136">Bu öznitelikler bulunan **System.Web.ModelBinding** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="0a80d-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="0a80d-137">Bunlara aşağıdakiler dahildir:</span><span class="sxs-lookup"><span data-stu-id="0a80d-137">They include:</span></span>

- <span data-ttu-id="0a80d-138">Denetim</span><span class="sxs-lookup"><span data-stu-id="0a80d-138">Control</span></span>
- <span data-ttu-id="0a80d-139">Tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="0a80d-139">Cookie</span></span>
- <span data-ttu-id="0a80d-140">Form</span><span class="sxs-lookup"><span data-stu-id="0a80d-140">Form</span></span>
- <span data-ttu-id="0a80d-141">Profil</span><span class="sxs-lookup"><span data-stu-id="0a80d-141">Profile</span></span>
- <span data-ttu-id="0a80d-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="0a80d-142">QueryString</span></span>
- <span data-ttu-id="0a80d-143">Routedata öğesi</span><span class="sxs-lookup"><span data-stu-id="0a80d-143">RouteData</span></span>
- <span data-ttu-id="0a80d-144">Oturum</span><span class="sxs-lookup"><span data-stu-id="0a80d-144">Session</span></span>
- <span data-ttu-id="0a80d-145">Kullanıcı profili</span><span class="sxs-lookup"><span data-stu-id="0a80d-145">UserProfile</span></span>
- <span data-ttu-id="0a80d-146">Görünüm durumu</span><span class="sxs-lookup"><span data-stu-id="0a80d-146">ViewState</span></span>

<span data-ttu-id="0a80d-147">Bu öğreticide, bir denetimin değeri içinde GridView kayıtları filtrelemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="0a80d-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="0a80d-148">Ekleyeceksiniz **denetimi** sorgu yöntemine daha önce oluşturduğunuz özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0a80d-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="0a80d-149">İçinde bir [daha sonra](using-query-string-values-to-retrieve-data.md) öğreticide uygulanacaktır **QueryString** parametre değeri bir sorgu dizesi değerinden geldiğini belirtmek üzere bir parametre özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0a80d-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="0a80d-150">İlk olarak, ValidationSummary bir açılan listeden filtreleme için hangi Öğrenciler gösterilen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0a80d-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="0a80d-151">Arka plan kod dosyasında, bir değer denetiminden almak için select yöntemi değiştirin ve parametre adı değeri sağlayan denetimin adına ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="0a80d-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="0a80d-152">Eklemelisiniz bir **kullanarak** bildirimi **System.Web.ModelBinding** denetim özniteliği çözmek için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="0a80d-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="0a80d-153">Aşağıdaki kod, aşağı açılan listeden değere göre döndürülen verileri filtrelemek için yeniden çalışılan select yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="0a80d-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="0a80d-154">Bu parametre için değer aynı ada sahip bir denetimden gelen bir parametre belirtir. önce bir denetim özniteliği ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="0a80d-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="0a80d-155">Web uygulamasını çalıştırın ve aşağı açılan listeden Öğrenciler listesine filtre uygulamak için farklı değerler seçin.</span><span class="sxs-lookup"><span data-stu-id="0a80d-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Filtre Öğrenciler](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="0a80d-157">Sonuç</span><span class="sxs-lookup"><span data-stu-id="0a80d-157">Conclusion</span></span>

<span data-ttu-id="0a80d-158">Bu öğreticide, sıralama ve veri disk belleği etkin.</span><span class="sxs-lookup"><span data-stu-id="0a80d-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="0a80d-159">Ayrıca, bir denetimin değeri tarafından verileri filtreleme etkin.</span><span class="sxs-lookup"><span data-stu-id="0a80d-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="0a80d-160">Sonraki [öğretici](integrating-jquery-ui.md) kullanıcı arabirimini dinamik veri şablona JQuery kullanıcı Arabirimi pencere öğesi tümleştirerek ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="0a80d-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0a80d-161">[Önceki](updating-deleting-and-creating-data.md)
> [İleri](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="0a80d-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
