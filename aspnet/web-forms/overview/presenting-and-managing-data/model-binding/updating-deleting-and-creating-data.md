---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Güncelleştirme, silme ve verileri model bağlama ve web forms ile oluşturma | Microsoft Docs
author: tfitzmac
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 1cf9873db177b67927b579def1eedd08e3e9a762
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821638"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="e49a6-104">Güncelleştirme, silme ve verileri model bağlama ve web forms ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="e49a6-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="e49a6-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e49a6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e49a6-106">Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="e49a6-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="e49a6-107">Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar.</span><span class="sxs-lookup"><span data-stu-id="e49a6-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="e49a6-108">Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.</span><span class="sxs-lookup"><span data-stu-id="e49a6-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="e49a6-109">Bu öğreticide, oluşturmak, güncelleştirmek ve model bağlama ile verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="e49a6-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="e49a6-110">Aşağıdaki özellikleri ayarlar:</span><span class="sxs-lookup"><span data-stu-id="e49a6-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="e49a6-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="e49a6-111">DeleteMethod</span></span>
> - <span data-ttu-id="e49a6-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="e49a6-112">InsertMethod</span></span>
> - <span data-ttu-id="e49a6-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="e49a6-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="e49a6-114">Bu özellikler, karşılık gelen işlemi işleyen yöntem adını alır.</span><span class="sxs-lookup"><span data-stu-id="e49a6-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="e49a6-115">Bu yöntem içinde verilerle etkileşim kurmak için mantığı sağlar.</span><span class="sxs-lookup"><span data-stu-id="e49a6-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="e49a6-116">Bu öğreticide oluşturduğunuz ilk proje geliştirir [bölümü](retrieving-data.md) dizi.</span><span class="sxs-lookup"><span data-stu-id="e49a6-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="e49a6-117">Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projeyi C# veya vb</span><span class="sxs-lookup"><span data-stu-id="e49a6-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="e49a6-118">İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="e49a6-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="e49a6-119">Bu öğreticide gösterilen Visual Studio 2013 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="e49a6-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="e49a6-120">Derleme</span><span class="sxs-lookup"><span data-stu-id="e49a6-120">What you'll build</span></span>

<span data-ttu-id="e49a6-121">Bu öğreticide, gerekir:</span><span class="sxs-lookup"><span data-stu-id="e49a6-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="e49a6-122">Dinamik veri şablonları ekleme</span><span class="sxs-lookup"><span data-stu-id="e49a6-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="e49a6-123">Model bağlama yöntemleri güncelleştirme ve silme veri etkinleştir</span><span class="sxs-lookup"><span data-stu-id="e49a6-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="e49a6-124">Veri doğrulama kuralları uygulamak - bir veritabanında yeni bir kayıt oluşturma etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="e49a6-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="e49a6-125">Dinamik veri şablonları ekleme</span><span class="sxs-lookup"><span data-stu-id="e49a6-125">Add dynamic data templates</span></span>

<span data-ttu-id="e49a6-126">Kod tekrarından en aza indirmek ve en iyi kullanıcı deneyimi sağlamak için dinamik veri şablonları kullanır.</span><span class="sxs-lookup"><span data-stu-id="e49a6-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="e49a6-127">Bir NuGet paketini yükleyerek önceden oluşturulmuş dinamik veri şablonları mevcut sitenize kolayca tümleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e49a6-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="e49a6-128">Gelen **NuGet paketlerini Yönet**, yükleme **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="e49a6-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![dinamik veri şablonları](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="e49a6-130">Projeniz artık adlı bir klasör içeriyorsa bildirimi **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="e49a6-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="e49a6-131">Bu klasörde otomatik olarak web formlarınızı dinamik denetimlere uygulanan şablonlar bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e49a6-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![dinamik veri klasörü](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="e49a6-133">Etkinleştirme güncelleştirme ve silme</span><span class="sxs-lookup"><span data-stu-id="e49a6-133">Enable updating and deleting</span></span>

<span data-ttu-id="e49a6-134">Kullanıcıların, güncelleştirme ve silme kayıtlarını veritabanında veri alma işlemine çok benzer.</span><span class="sxs-lookup"><span data-stu-id="e49a6-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="e49a6-135">İçinde **UpdateMethod** ve **DeleteMethod** özellikleri, bu işlemleri gerçekleştiren yöntemler adlarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="e49a6-136">Bir GridView denetimi Düzenle otomatik olarak oluşturulmasını belirtin ve Sil düğmeleri.</span><span class="sxs-lookup"><span data-stu-id="e49a6-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="e49a6-137">Aşağıdaki vurgulanmış kodu GridView kod eklemeleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="e49a6-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="e49a6-138">Kullanarak bir arka plan kod dosyasında, ekleme deyimini **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="e49a6-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="e49a6-139">Ardından, şu güncelleştirmeyi ekleyin ve yöntemlerini silin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="e49a6-140">**TryUpdateModel** yöntemi web formu eşleşen verilere bağlı değerleri veri öğesi için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e49a6-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="e49a6-141">Veri öğesi dayalı kimliği parametresinin değeri olarak alınır.</span><span class="sxs-lookup"><span data-stu-id="e49a6-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="e49a6-142">Doğrulama gereksinimlerini zorunlu</span><span class="sxs-lookup"><span data-stu-id="e49a6-142">Enforce validation requirements</span></span>

<span data-ttu-id="e49a6-143">Verileri güncelleştirirken, Öğrenci sınıfında FirstName ve LastName yıl özelliklerine uygulanan doğrulama özniteliklerinin otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e49a6-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="e49a6-144">İstemci ve sunucu doğrulayıcıları doğrulama özniteliklerine dayalı DynamicField denetimleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="e49a6-145">FirstName ve LastName özelliklerinin her ikisi de olduğundan gerekli.</span><span class="sxs-lookup"><span data-stu-id="e49a6-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="e49a6-146">FirstName 20 karakterden uzun olamaz ve LastName 40 karakterden uzun olamaz.</span><span class="sxs-lookup"><span data-stu-id="e49a6-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="e49a6-147">Yıl AcademicYear sabit listesi için geçerli bir değer olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e49a6-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="e49a6-148">Kullanıcı doğrulama gereksinimlerinden biri, ihlal edilirse, güncelleştirme devam etmez.</span><span class="sxs-lookup"><span data-stu-id="e49a6-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="e49a6-149">Hata iletisini görmek için yukarıdaki GridView bir ValidationSummary denetimi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="e49a6-150">Model bağlama doğrulama hatalarından görüntülenecek kümesi **ShowModelStateErrors** özelliğini **true**.</span><span class="sxs-lookup"><span data-stu-id="e49a6-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="e49a6-151">Web uygulamasını çalıştırmak, güncelleştirme ve kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-151">Run the web application, and update and delete any of the records.</span></span>

![verileri güncelleştirme](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="e49a6-153">Düzenleme modunda yıl özelliğinin değeri otomatik olarak açılan bir liste olarak işlendiğinde dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="e49a6-154">Year özelliği bir sabit listesi değeri ve bir açılan listeden düzenlemek için bir sabit listesi değeri için dinamik veri şablonu belirtir.</span><span class="sxs-lookup"><span data-stu-id="e49a6-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="e49a6-155">Bu şablon açarak bulabilirsiniz **numaralandırma\_Edit.ascx** dosyası **DynamicData**/**FieldTemplates** klasör.</span><span class="sxs-lookup"><span data-stu-id="e49a6-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="e49a6-156">Geçerli değerler sağlarsanız, güncelleştirme başarıyla tamamlar.</span><span class="sxs-lookup"><span data-stu-id="e49a6-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="e49a6-157">Doğrulama gereksinimlerini ihlal güncelleştirme devam etmiyor ve kılavuzun üzerindeki bir hata iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e49a6-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![hata iletisi](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="e49a6-159">Yeni kayıtlar ekleme</span><span class="sxs-lookup"><span data-stu-id="e49a6-159">Add new records</span></span>

<span data-ttu-id="e49a6-160">GridView denetiminde yer almaz **InsertMethod** özelliği ve bu nedenle yeni bir kayıt ile model bağlama eklemek için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="e49a6-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="e49a6-161">InsertMethod özelliğinde bulabilirsiniz **FormView**, **DetailsView**, veya **ListView** kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="e49a6-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="e49a6-162">Bu öğreticide, yeni bir kayıt eklemek için FormView denetimi kullanır.</span><span class="sxs-lookup"><span data-stu-id="e49a6-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="e49a6-163">İlk olarak, yeni bir kayıt eklemek için oluşturacağınız yeni sayfasına bağlantı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="e49a6-164">Bir ValidationSummary ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e49a6-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="e49a6-165">Yeni bağlantı içeriklerin Öğrenciler üstünde görünür.</span><span class="sxs-lookup"><span data-stu-id="e49a6-165">The new link will appear at the top of the content for the Students page.</span></span>

![Yeni bağlantı](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="e49a6-167">Ardından, bir ana sayfa kullanan yeni bir web formu ekleyin ve adlandırın **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="e49a6-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="e49a6-168">Site.Master ana sayfa olarak seçin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="e49a6-169">Kullanarak yeni bir öğrenci eklemek için alanlar işleyecek bir **DynamicEntity** denetimi.</span><span class="sxs-lookup"><span data-stu-id="e49a6-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="e49a6-170">Düzenlenebilir, Itemtype özelliğinde belirtilen sınıf özelliklerinde DynamicEntity denetimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e49a6-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="e49a6-171">StudentID özelliği ile işaretlenmiş **[ScaffoldColumn(false)]** değil işlenebilmesi özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e49a6-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="e49a6-172">MainContent yer tutucusu AddStudent sayfasının, aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="e49a6-173">Arka plan kod dosyasında (AddStudent.aspx.cs) ekleme bir **kullanarak** bildirimi **ContosoUniversityModelBinding.Models** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="e49a6-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="e49a6-174">Ardından, yeni bir kayıt ve iptal düğmesi için bir olay işleyicisi ekleme belirtmek için aşağıdaki yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="e49a6-175">Tüm değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-175">Save all of the changes.</span></span>

<span data-ttu-id="e49a6-176">Web uygulamasını çalıştırın ve yeni bir öğrenci oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e49a6-176">Run the web application and create a new student.</span></span>

![Yeni bir öğrenci eklemek](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="e49a6-178">Tıklayın **Ekle** ve yeni Öğrenci oluşturulan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-178">Click **Insert** and notice the new student has been created.</span></span>

![Yeni Öğrenci görüntüleme](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="e49a6-180">Sonuç</span><span class="sxs-lookup"><span data-stu-id="e49a6-180">Conclusion</span></span>

<span data-ttu-id="e49a6-181">Bu öğreticide, güncelleştirme, silme ve verileri oluşturma etkin.</span><span class="sxs-lookup"><span data-stu-id="e49a6-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="e49a6-182">Doğrulama kuralları verilerle etkileşim kurulurken uygulanır olmasını sağladı.</span><span class="sxs-lookup"><span data-stu-id="e49a6-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="e49a6-183">Sonraki [öğretici](sorting-paging-and-filtering-data.md) bu dizide, sıralama, sayfalama ve verileri filtreleme sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="e49a6-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e49a6-184">[Önceki](retrieving-data.md)
> [İleri](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="e49a6-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
