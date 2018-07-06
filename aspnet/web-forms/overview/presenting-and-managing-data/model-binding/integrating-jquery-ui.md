---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: JQuery UI Datepicker model bağlama ve web forms ile tümleştirme | Microsoft Docs
author: tfitzmac
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: fc278575615e63af2fc7284e51814cf318165110
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807583"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="a3b93-104">JQuery UI Datepicker model bağlama ve web forms ile tümleştirme</span><span class="sxs-lookup"><span data-stu-id="a3b93-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="a3b93-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a3b93-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a3b93-106">Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="a3b93-107">Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar.</span><span class="sxs-lookup"><span data-stu-id="a3b93-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="a3b93-108">Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.</span><span class="sxs-lookup"><span data-stu-id="a3b93-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="a3b93-109">Bu öğreticide, JQuery kullanıcı Arabirimi ekleme işlemi açıklanır [Datepicker pencere öğesi](http://jqueryui.com/datepicker/) bir Web formu ve kullanım modeli ile seçilen değeri veritabanını güncellemek için bağlama.</span><span class="sxs-lookup"><span data-stu-id="a3b93-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="a3b93-110">Bu öğreticide oluşturulan proje geliştirir [ilk](retrieving-data.md) ve [ikinci](updating-deleting-and-creating-data.md) dizisinin bölümleri.</span><span class="sxs-lookup"><span data-stu-id="a3b93-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="a3b93-111">Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projeyi C# veya vb</span><span class="sxs-lookup"><span data-stu-id="a3b93-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="a3b93-112">İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="a3b93-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="a3b93-113">Bu öğreticide gösterilen Visual Studio 2013 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="a3b93-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="a3b93-114">Derleme</span><span class="sxs-lookup"><span data-stu-id="a3b93-114">What you'll build</span></span>

<span data-ttu-id="a3b93-115">Bu öğreticide, gerekir:</span><span class="sxs-lookup"><span data-stu-id="a3b93-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="a3b93-116">Öğrenci kayıt tarihi kaydetmek için model bir özelliği Ekle</span><span class="sxs-lookup"><span data-stu-id="a3b93-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="a3b93-117">JQuery UI Datepicker pencere öğesi kullanarak kayıt tarih seçmesini etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="a3b93-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="a3b93-118">Doğrulama kuralları uygulamak için kayıt tarihi</span><span class="sxs-lookup"><span data-stu-id="a3b93-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="a3b93-119">JQuery UI Datepicker pencere öğesi, kullanıcıların kolayca kullanıcı alanı ile etkileşim kurduğunda görüntülenen iletideki bir takvimden bir tarih seçmek üzere sağlar.</span><span class="sxs-lookup"><span data-stu-id="a3b93-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="a3b93-120">Bu pencere öğesini kullanarak bir tarih el ile yazmak daha kullanıcılar için daha kullanışlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="a3b93-121">Datepicker pencere öğesi veri işlemleri için model bağlama kullanan bir sayfa ile tümleştirme, yalnızca az miktarda ek iş gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="a3b93-122">Yeni bir özellik modele eklemek</span><span class="sxs-lookup"><span data-stu-id="a3b93-122">Add a new property to the model</span></span>

<span data-ttu-id="a3b93-123">İlk olarak ekleyecek bir **Datetime** Tutkusu özelliğini model ve bu değişiklik veritabanına geçirin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="a3b93-124">Açık **UniversityModels.cs**, Öğrenci modele vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="a3b93-125">**RangeAttribute** özelliği için doğrulama kuralları uygulamak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="a3b93-126">Bu öğreticide, Contoso University 1 Ocak 2013'ün üzerinde kurulmuştur ve bu nedenle önceki kayıt tarihleri geçerli olmayan varsayacağız.</span><span class="sxs-lookup"><span data-stu-id="a3b93-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="a3b93-127">Komutunu çalıştırarak paket Yönetimi penceresinde bir geçiş ekleyin **ekleme geçiş AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="a3b93-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="a3b93-128">Geçiş kodu Öğrenci tabloya yeni bir Datetime sütunu eklediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="a3b93-129">RangeAttribute içinde belirtilen değerle eşleşecek şekilde aşağıdaki vurgulanmış kodu gösterildiği gibi yeni bir sütun için varsayılan bir değer ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="a3b93-130">Değişikliğinizi geçiş dosyaya kaydedin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-130">Save your change to the migration file.</span></span>

<span data-ttu-id="a3b93-131">Verileri yeniden dağıtılması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a3b93-131">You do not need to seed the data again.</span></span> <span data-ttu-id="a3b93-132">Bu nedenle, açık **Configuration.cs** geçişler klasöründe kaldırın ya da kod açıklama **çekirdek** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a3b93-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="a3b93-133">Dosyayı kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="a3b93-133">Save and close the file.</span></span>

<span data-ttu-id="a3b93-134">Şimdi komutu çalıştırın **veritabanını Güncelleştir**.</span><span class="sxs-lookup"><span data-stu-id="a3b93-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="a3b93-135">Sütun artık veritabanında bulunmuyor ve var olan kayıtların tümünün EnrollmentDate için varsayılan değere sahip olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="a3b93-136">Kayıt tarihi için Dinamik denetimler ekleme</span><span class="sxs-lookup"><span data-stu-id="a3b93-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="a3b93-137">Şimdi, görüntülemek ve kayıt tarihi düzenleme denetimleri de ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a3b93-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="a3b93-138">Bu noktada, değerin bir metin kutusu düzenlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="a3b93-139">Öğreticinin sonraki bölümlerinde JQuery Datepicker pencere öğesi için metin kutusu değişecektir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="a3b93-140">İlk olarak, herhangi bir değişiklik yapmanız gerekmez unutmayın **AddStudent.aspx** dosya.</span><span class="sxs-lookup"><span data-stu-id="a3b93-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="a3b93-141">DynamicEntity denetimi otomatik olarak yeni bir özellik olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="a3b93-142">Açık **Students.aspx**, aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="a3b93-143">Uygulamayı çalıştırın ve kayıt tarih değerini bir tarih yazarak ayarlayabilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="a3b93-144">Yeni bir öğrenci eklendiği zaman:</span><span class="sxs-lookup"><span data-stu-id="a3b93-144">When adding a new student:</span></span>

![tarihi ayarlama](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="a3b93-146">Veya var olan bir değer düzenleme:</span><span class="sxs-lookup"><span data-stu-id="a3b93-146">Or, editing an existing value:</span></span>

![tarihi Düzenle](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="a3b93-148">Müşteri deneyimini sağlamak istediğiniz tarih çalışır, ancak yazmayı olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="a3b93-149">Sonraki bölümde, bir takvim aracılığıyla bir tarih seçerek olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="a3b93-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="a3b93-150">JQuery kullanıcı Arabirimi ile çalışmak için NuGet paketini yükle</span><span class="sxs-lookup"><span data-stu-id="a3b93-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="a3b93-151">**Suyu UI** NuGet paketi, web uygulamanıza JQuery kullanıcı Arabirimi pencere öğeleri kolay tümleştirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="a3b93-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="a3b93-152">Bu paketi kullanmak için NuGet yükleyin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-152">To use this package, install it through NuGet.</span></span>

![suyu kullanıcı Arabirimi ekleme](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="a3b93-154">Yüklediğiniz suyu UI sürümü, uygulamanızdaki JQuery sürümüyle çakışabilir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="a3b93-155">Bu öğretici ile devam etmeden önce uygulamanızı çalıştırmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="a3b93-156">Bir JavaScript hatayla karşılaşırsanız, JQuery sürüm mutabakat sağlamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="a3b93-157">JQuery beklenen sürümü betikleri klasörünüze (Bu öğreticide yazma zamanında sürüm 1.8.2) ekleyin veya Site.master JQuery dosyanın yolunu belirtin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="a3b93-158">DatePicker pencere öğesi eklemek için DateTime şablonunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="a3b93-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="a3b93-159">Datepicker pencere öğesi, bir datetime değeri düzenlemek için dinamik veri şablona ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a3b93-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="a3b93-160">Şablona pencere öğesi ekleyerek bunu otomatik olarak yeni bir öğrenci eklemek için her iki formu ve düzenleme Öğrenciler için kılavuz görünümünde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a3b93-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="a3b93-161">Açık **DateTime\_Edit.ascx**, aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="a3b93-162">Arka plan kod dosyasında, minimum ve maksimum tarih tarih seçici için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a3b93-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="a3b93-163">Bu değerleri ayarlayarak, kullanıcıların geçersiz tarihlere gitmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="a3b93-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="a3b93-164">Minimum ve maksimum değerleri alır **RangeAttribute** sağlanmazsa, bir DateTime özellikte.</span><span class="sxs-lookup"><span data-stu-id="a3b93-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="a3b93-165">Açık **DateTime\_Edit.ascx.cs**, sayfaya aşağıdaki vurgulanmış kodu ekleyin\_yükleme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a3b93-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="a3b93-166">Web uygulamasını çalıştırmak ve AddStudent sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="a3b93-167">Alanlar için değerler sağlayın ve kayıt tarihi için metin kutusunu tıkladığınızda, Takvim görüntülendiğini dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a3b93-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Tarih Seçici](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="a3b93-169">Bir tarih seçin ve tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a3b93-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="a3b93-170">Sunucu doğrulamasını RangeAttribute zorlar.</span><span class="sxs-lookup"><span data-stu-id="a3b93-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="a3b93-171">Datepicker üzerinde minDate özelliğini ayarlayarak, ayrıca doğrulama istemcide uygulayın.</span><span class="sxs-lookup"><span data-stu-id="a3b93-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="a3b93-172">Takvime bir tarihten önce minDate değerini gidin kullanıcı izin vermez.</span><span class="sxs-lookup"><span data-stu-id="a3b93-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="a3b93-173">Izgara görünümünde bir kayıt düzenlediğinizde, Takvim de görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a3b93-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker GridView içinde](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="a3b93-175">Sonuç</span><span class="sxs-lookup"><span data-stu-id="a3b93-175">Conclusion</span></span>

<span data-ttu-id="a3b93-176">Bu öğreticide, model bağlama kullanan bir web formu JQuery pencere öğesi eklemek öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="a3b93-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="a3b93-177">Sonraki [öğretici](using-query-string-values-to-retrieve-data.md), verileri seçerken bir sorgu dizesi değerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a3b93-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3b93-178">[Önceki](sorting-paging-and-filtering-data.md)
> [İleri](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="a3b93-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
