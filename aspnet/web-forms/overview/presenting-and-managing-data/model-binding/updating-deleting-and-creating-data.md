---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: "Güncelleştirme, silme ve veri model bağlama ve web forms ile oluşturma | Microsoft Docs"
author: tfitzmac
description: "Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 18c065b44524e7738c048b5908fa50c592188064
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="ab429-104">Güncelleştirme, silme ve veri model bağlama ve web forms ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="ab429-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="ab429-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ab429-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ab429-106">Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ab429-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="ab429-107">Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="ab429-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="ab429-108">Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.</span><span class="sxs-lookup"><span data-stu-id="ab429-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="ab429-109">Bu öğretici, oluşturmak, güncelleştirmek ve verileri model bağlamayla silmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ab429-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="ab429-110">Aşağıdaki özellikler ayarlayacak:</span><span class="sxs-lookup"><span data-stu-id="ab429-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="ab429-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="ab429-111">DeleteMethod</span></span>
> - <span data-ttu-id="ab429-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="ab429-112">InsertMethod</span></span>
> - <span data-ttu-id="ab429-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="ab429-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="ab429-114">Bu özellikleri karşılık gelen işlemi işleyen yöntem adını alır.</span><span class="sxs-lookup"><span data-stu-id="ab429-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="ab429-115">Bu yöntem içinde veri ile etkileşmek için mantığı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab429-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="ab429-116">Bu öğretici ilk oluşturulan proje inşa edilmiştir [bölümü](retrieving-data.md) dizi.</span><span class="sxs-lookup"><span data-stu-id="ab429-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="ab429-117">Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede</span><span class="sxs-lookup"><span data-stu-id="ab429-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="ab429-118">İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="ab429-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="ab429-119">Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="ab429-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="ab429-120">Yapı</span><span class="sxs-lookup"><span data-stu-id="ab429-120">What you'll build</span></span>

<span data-ttu-id="ab429-121">Bu öğreticide, gerekir:</span><span class="sxs-lookup"><span data-stu-id="ab429-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="ab429-122">Dinamik veri şablonları Ekle</span><span class="sxs-lookup"><span data-stu-id="ab429-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="ab429-123">Model bağlama yöntemlerle güncelleştirme ve silme verilerini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="ab429-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="ab429-124">Veri doğrulama kuralları uygula - veritabanında yeni bir kayıt oluşturma etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="ab429-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="ab429-125">Dinamik veri şablonları Ekle</span><span class="sxs-lookup"><span data-stu-id="ab429-125">Add dynamic data templates</span></span>

<span data-ttu-id="ab429-126">En iyi kullanıcı deneyimi sağlamak ve kod yineleme en aza indirmek için dinamik veri şablonları kullanır.</span><span class="sxs-lookup"><span data-stu-id="ab429-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="ab429-127">NuGet paketini yükleyerek var olan sitenize önceden derlenmiş dinamik veri şablonları kolayca tümleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab429-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="ab429-128">Gelen **NuGet paketlerini Yönet**, yükleme **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="ab429-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![dinamik veri şablonları](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="ab429-130">Projenizi şimdi adlı bir klasör içerir bildirimi **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="ab429-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="ab429-131">Bu klasörde, web forms Dinamik denetimleri için otomatik olarak uygulanan şablonları bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab429-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![dinamik veri klasörü](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="ab429-133">Etkinleştirme güncelleştirme ve silme</span><span class="sxs-lookup"><span data-stu-id="ab429-133">Enable updating and deleting</span></span>

<span data-ttu-id="ab429-134">Güncelleştirme ve veritabanındaki kayıtları silme sağlama verilerini alma işlemi için çok benzer.</span><span class="sxs-lookup"><span data-stu-id="ab429-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="ab429-135">İçinde **UpdateMethod** ve **DeleteMethod** özellikleri, o işlemler gerçekleştiren yöntemleri adlarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="ab429-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="ab429-136">GridView denetimi ile düzenleme otomatik olarak oluşturulmasını belirtin ve düğmeleri silin.</span><span class="sxs-lookup"><span data-stu-id="ab429-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="ab429-137">Aşağıdaki vurgulanmış kodu GridView kod eklemeler gösterir.</span><span class="sxs-lookup"><span data-stu-id="ab429-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="ab429-138">Kullanarak bir arka plan kod dosyasına ekleyin bildirimi **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="ab429-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="ab429-139">Ardından, aşağıdaki güncelleştirmeyi ekleyin ve yöntemlerini silin.</span><span class="sxs-lookup"><span data-stu-id="ab429-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="ab429-140">**TryUpdateModel** yöntemi web formundan eşleşen veri bağlama değerleri veri öğesi için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ab429-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="ab429-141">Veri öğesi kimliği parametresinin değeri göre alınır.</span><span class="sxs-lookup"><span data-stu-id="ab429-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="ab429-142">Doğrulama gereksinimlerini zorla</span><span class="sxs-lookup"><span data-stu-id="ab429-142">Enforce validation requirements</span></span>

<span data-ttu-id="ab429-143">Öğrenci sınıfı FirstName, LastName ve yıl özelliklerinde uygulanan doğrulama özniteliklerinin verileri güncelleştirilirken otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ab429-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="ab429-144">DynamicField denetimleri doğrulama özniteliklerini temel alarak istemci ve sunucu doğrulayıcıları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ab429-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="ab429-145">Ad ve Soyadı her ikisi de özelliklerdir gerekli.</span><span class="sxs-lookup"><span data-stu-id="ab429-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="ab429-146">Ad uzunluğu 20 karakterden uzun olamaz ve LastName 40 karakteri aşamaz.</span><span class="sxs-lookup"><span data-stu-id="ab429-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="ab429-147">Yıl AcademicYear numaralandırma için geçerli bir değer olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ab429-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="ab429-148">Kullanıcı doğrulama gereksinimlerini bozup güncelleştirme devam etmez.</span><span class="sxs-lookup"><span data-stu-id="ab429-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="ab429-149">Hata iletisini görmek için ValidationSummary denetimi GridView üstüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ab429-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="ab429-150">Model bağlama doğrulama hatalarından görüntülenecek ayarlamak **ShowModelStateErrors** özelliğini **doğru**.</span><span class="sxs-lookup"><span data-stu-id="ab429-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="ab429-151">Web uygulamasını çalıştırmak, güncelleştirme ve kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="ab429-151">Run the web application, and update and delete any of the records.</span></span>

![Verileri güncelleştirme](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="ab429-153">Düzenleme modunda yıl özelliğinin değeri otomatik olarak açılır listeden olarak işlendiğinde dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ab429-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="ab429-154">Year özelliği bir numaralandırma değeridir ve açılan düzenlemek için listeden bir numaralandırma değeri için dinamik veri şablonu belirtir.</span><span class="sxs-lookup"><span data-stu-id="ab429-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="ab429-155">Bu şablonu açarak bulabileceğiniz **numaralandırma\_Edit.ascx** dosyasını **DynamicData**/**FieldTemplates** klasör.</span><span class="sxs-lookup"><span data-stu-id="ab429-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="ab429-156">Geçerli değerler sağlarsanız, güncelleştirme başarıyla tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="ab429-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="ab429-157">Doğrulama gereksinimlerini birini ihlal güncelleştirme devam etmez ve kılavuz bir hata iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ab429-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![Hata iletisi](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="ab429-159">Yeni kayıtlar ekleme</span><span class="sxs-lookup"><span data-stu-id="ab429-159">Add new records</span></span>

<span data-ttu-id="ab429-160">GridView denetiminin içermemesi **InsertMethod** özelliği ve bu nedenle model bağlama ile yeni bir kayıt eklemek için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="ab429-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="ab429-161">InsertMethod özelliğinde bulabilirsiniz **FormView**, **DetailsView**, veya **ListView** kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="ab429-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="ab429-162">Bu öğreticide, yeni bir kayıt eklemek için FormView denetimi kullanır.</span><span class="sxs-lookup"><span data-stu-id="ab429-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="ab429-163">İlk olarak, yeni bir kayıt eklemek için oluşturacağınız yeni sayfaya bir bağlantı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ab429-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="ab429-164">ValidationSummary ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ab429-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="ab429-165">Yeni bağlantı Öğrenciler sayfasının içeriğinin üstünde görünür.</span><span class="sxs-lookup"><span data-stu-id="ab429-165">The new link will appear at the top of the content for the Students page.</span></span>

![Yeni bağlantı](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="ab429-167">Ardından, bir ana sayfa kullanarak yeni bir web formu ekleyin ve adını **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="ab429-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="ab429-168">Site.Master ana sayfa olarak seçin.</span><span class="sxs-lookup"><span data-stu-id="ab429-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="ab429-169">Kullanarak yeni bir öğrenci eklemek için alanlarını kılacak bir **DynamicEntity** denetim.</span><span class="sxs-lookup"><span data-stu-id="ab429-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="ab429-170">DynamicEntity denetim ItemType özelliğinde belirtilen sınıfında düzenlenebilir bu özellikleri işler.</span><span class="sxs-lookup"><span data-stu-id="ab429-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="ab429-171">StudentID özelliği ile işaretlenmiş **[ScaffoldColumn(false)]** değil işlenen şekilde özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ab429-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="ab429-172">MainContent yer tutucu AddStudent sayfasında, aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ab429-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="ab429-173">Arka plan kod dosyasına (AddStudent.aspx.cs) ekleyin bir **kullanarak** bildirimi **ContosoUniversityModelBinding.Models** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="ab429-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="ab429-174">Ardından, yeni bir kayıt ve iptal düğmesi olay işleyicisi ekleme belirtmek için aşağıdaki yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ab429-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="ab429-175">Tüm değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ab429-175">Save all of the changes.</span></span>

<span data-ttu-id="ab429-176">Web uygulamasını çalıştırın ve yeni Öğrenci oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ab429-176">Run the web application and create a new student.</span></span>

![Yeni Öğrenci ekleme](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="ab429-178">Tıklatın **Ekle** ve yeni Öğrenci oluşturulan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ab429-178">Click **Insert** and notice the new student has been created.</span></span>

![Yeni Öğrenci görüntüleme](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="ab429-180">Sonuç</span><span class="sxs-lookup"><span data-stu-id="ab429-180">Conclusion</span></span>

<span data-ttu-id="ab429-181">Bu öğreticide, güncelleştirme, silme ve veri oluşturma etkin.</span><span class="sxs-lookup"><span data-stu-id="ab429-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="ab429-182">Doğrulama kuralları verilerle kullanılırken uygulanır güvence altına.</span><span class="sxs-lookup"><span data-stu-id="ab429-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="ab429-183">Sonraki [öğretici](sorting-paging-and-filtering-data.md) bu dizide, disk belleği, verileri sıralama ve filtreleme olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab429-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ab429-184">[Önceki](retrieving-data.md)
[sonraki](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="ab429-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
