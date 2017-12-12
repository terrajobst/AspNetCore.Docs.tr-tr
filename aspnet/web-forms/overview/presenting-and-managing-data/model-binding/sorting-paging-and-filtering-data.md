---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: "Sıralama, disk belleği ve model bağlama ve web forms ile veri filtreleme | Microsoft Docs"
author: tfitzmac
description: "Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 94fc84533be5fcbcf0612fcdcabea7dee738d89b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="195ab-104">Sıralama, disk belleği ve model bağlama ve web forms ile verileri filtreleme</span><span class="sxs-lookup"><span data-stu-id="195ab-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="195ab-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="195ab-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="195ab-106">Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="195ab-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="195ab-107">Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="195ab-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="195ab-108">Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.</span><span class="sxs-lookup"><span data-stu-id="195ab-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="195ab-109">Bu öğretici, sıralama, disk belleği ve model bağlama aracılığıyla verileri filtreleme nasıl ekleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="195ab-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="195ab-110">Bu öğretici ilk oluşturulan proje inşa edilmiştir [bölümü](retrieving-data.md) dizi.</span><span class="sxs-lookup"><span data-stu-id="195ab-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="195ab-111">Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede</span><span class="sxs-lookup"><span data-stu-id="195ab-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="195ab-112">İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="195ab-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="195ab-113">Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="195ab-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="195ab-114">Yapı</span><span class="sxs-lookup"><span data-stu-id="195ab-114">What you'll build</span></span>

<span data-ttu-id="195ab-115">Bu öğreticide, gerekir:</span><span class="sxs-lookup"><span data-stu-id="195ab-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="195ab-116">Sıralama ve verilerin disk belleği etkinleştir</span><span class="sxs-lookup"><span data-stu-id="195ab-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="195ab-117">Kullanıcı tarafından seçime dayalı veri filtreleme etkinleştir</span><span class="sxs-lookup"><span data-stu-id="195ab-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="195ab-118">Sıralama Ekle</span><span class="sxs-lookup"><span data-stu-id="195ab-118">Add sorting</span></span>

<span data-ttu-id="195ab-119">GridView sıralama etkinleştirme çok kolaydır.</span><span class="sxs-lookup"><span data-stu-id="195ab-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="195ab-120">Student.aspx dosyasında sadece ayarlanan **AllowSorting** için **true** GridView içinde.</span><span class="sxs-lookup"><span data-stu-id="195ab-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="195ab-121">Ayarlanacak gerekmez bir **SortExpression** DataField otomatik olarak kullanılan her sütun için değer.</span><span class="sxs-lookup"><span data-stu-id="195ab-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="195ab-122">GridView sorguyu seçili değeriyle verileri sıralama içerecek şekilde değiştirir.</span><span class="sxs-lookup"><span data-stu-id="195ab-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="195ab-123">Aşağıda vurgulanan kod sıralamayı etkinleştirmek yapmanız gereken ek gösterir.</span><span class="sxs-lookup"><span data-stu-id="195ab-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="195ab-124">Web uygulamasını çalıştırın ve sıralama Öğrenci kayıtlarını farklı sütunlardaki değerleri sınayın.</span><span class="sxs-lookup"><span data-stu-id="195ab-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Sıralama öğrenciler](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="195ab-126">Disk belleği ekleme</span><span class="sxs-lookup"><span data-stu-id="195ab-126">Add paging</span></span>

<span data-ttu-id="195ab-127">Disk belleği etkinleştirme ayrıca çok kolaydır.</span><span class="sxs-lookup"><span data-stu-id="195ab-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="195ab-128">GridView, ayarlamak **AllowPaging** özelliğine **true** ve **PageSize** istediğiniz her bir sayfasında görüntülenecek kayıt sayısını özelliğine.</span><span class="sxs-lookup"><span data-stu-id="195ab-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="195ab-129">Bu öğreticide, 4'e ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="195ab-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="195ab-130">Web uygulamasını çalıştırmak ve kayıtları tek bir sayfada görüntülenen en fazla 4 kayıtlarla birden çok sayfa ayrılır artık dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="195ab-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![disk belleği ekleme](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="195ab-132">Ertelenmiş sorgu yürütme uygulama verimliliğini artırır.</span><span class="sxs-lookup"><span data-stu-id="195ab-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="195ab-133">GridView, tüm veri kümesinin alınıyor yerine, yalnızca geçerli sayfa için kayıtları almaya yönelik sorgu değiştirir.</span><span class="sxs-lookup"><span data-stu-id="195ab-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="195ab-134">Kullanıcı seçime göre filtre kayıtları</span><span class="sxs-lookup"><span data-stu-id="195ab-134">Filter records by user selection</span></span>

<span data-ttu-id="195ab-135">Model bağlama bir model bağlama yönteminde bir parametre için değer ayarlamak nasıl tanımlamanızı sağlayan birkaç öznitelik ekler.</span><span class="sxs-lookup"><span data-stu-id="195ab-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="195ab-136">Bu öznitelikler bulunan **System.Web.ModelBinding** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="195ab-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="195ab-137">Bunlara aşağıdakiler dahildir:</span><span class="sxs-lookup"><span data-stu-id="195ab-137">They include:</span></span>

- <span data-ttu-id="195ab-138">Denetim</span><span class="sxs-lookup"><span data-stu-id="195ab-138">Control</span></span>
- <span data-ttu-id="195ab-139">Tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="195ab-139">Cookie</span></span>
- <span data-ttu-id="195ab-140">Form</span><span class="sxs-lookup"><span data-stu-id="195ab-140">Form</span></span>
- <span data-ttu-id="195ab-141">Profil</span><span class="sxs-lookup"><span data-stu-id="195ab-141">Profile</span></span>
- <span data-ttu-id="195ab-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="195ab-142">QueryString</span></span>
- <span data-ttu-id="195ab-143">Routedata öğesi</span><span class="sxs-lookup"><span data-stu-id="195ab-143">RouteData</span></span>
- <span data-ttu-id="195ab-144">Oturum</span><span class="sxs-lookup"><span data-stu-id="195ab-144">Session</span></span>
- <span data-ttu-id="195ab-145">Kullanıcı profili</span><span class="sxs-lookup"><span data-stu-id="195ab-145">UserProfile</span></span>
- <span data-ttu-id="195ab-146">Görünüm durumu</span><span class="sxs-lookup"><span data-stu-id="195ab-146">ViewState</span></span>

<span data-ttu-id="195ab-147">Bu öğretici kapsamında, hangi kayıtların GridView görüntüleneceğini filtrelemek için bir denetimin değeri kullanır.</span><span class="sxs-lookup"><span data-stu-id="195ab-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="195ab-148">Ekleyeceksiniz **denetim** özniteliğine daha önce oluşturduğunuz sorgu yöntemi.</span><span class="sxs-lookup"><span data-stu-id="195ab-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="195ab-149">İçinde bir [daha sonra](using-query-string-values-to-retrieve-data.md) öğretici, uygulanacaktır **QueryString** öznitelik parametre değeri bir sorgu dizesi değerinden geldiğini belirtmek için bir parametre.</span><span class="sxs-lookup"><span data-stu-id="195ab-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="195ab-150">İlk olarak, bir açılır listeyi filtrelemek için hangi Öğrenciler gösterilen ValidationSummary ekleyin.</span><span class="sxs-lookup"><span data-stu-id="195ab-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="195ab-151">Arka plan kod dosyasına denetimden bir değer almasını select yöntemini değiştirin ve parametresinin adı değeri sağlayan denetimi adına ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="195ab-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="195ab-152">Eklemelisiniz bir **kullanarak** bildirimi **System.Web.ModelBinding** denetim özniteliği çözümlemek için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="195ab-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="195ab-153">Aşağıdaki kod döndürülen verileri aşağı açılan listeden değere göre filtre uygulamak için yeniden çalışılan seçme yöntemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="195ab-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="195ab-154">Bir parametre değeri bu parametre için aynı ada sahip bir denetiminden geldiğini belirten önce bir denetim özniteliği ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="195ab-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="195ab-155">Web uygulamasını çalıştırın ve açılır listeden Öğrenciler listesini filtrelemek için farklı değerler seçin.</span><span class="sxs-lookup"><span data-stu-id="195ab-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Filtre Öğrenciler](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="195ab-157">Sonuç</span><span class="sxs-lookup"><span data-stu-id="195ab-157">Conclusion</span></span>

<span data-ttu-id="195ab-158">Bu öğreticide, sıralama ve verilerin sayfalama etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="195ab-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="195ab-159">Ayrıca verileri denetim değeriyle filtreleme etkin.</span><span class="sxs-lookup"><span data-stu-id="195ab-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="195ab-160">Sonraki [öğretici](integrating-jquery-ui.md) dinamik veri şablonuna JQuery UI pencere öğesi tümleştirerek UI ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="195ab-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="195ab-161">[Önceki](updating-deleting-and-creating-data.md)
[sonraki](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="195ab-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
