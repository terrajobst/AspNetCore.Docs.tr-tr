---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Model bağlama ve web forms ile JQuery UI Datepicker tümleştirme | Microsoft Docs
author: tfitzmac
description: Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="7e44e-104">JQuery UI Datepicker model bağlama ve web forms ile tümleştirme</span><span class="sxs-lookup"><span data-stu-id="7e44e-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="7e44e-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7e44e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7e44e-106">Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="7e44e-107">Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="7e44e-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="7e44e-108">Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.</span><span class="sxs-lookup"><span data-stu-id="7e44e-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="7e44e-109">Bu öğretici JQuery UI eklemeyi gösterir [Datepicker pencere öğesi](http://jqueryui.com/datepicker/) bir Web formu ve seçilen değerle veritabanını güncelleştirmek için bağlama kullanım modeli.</span><span class="sxs-lookup"><span data-stu-id="7e44e-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="7e44e-110">Bu öğreticide oluşturduğunuz proje inşa edilmiştir [ilk](retrieving-data.md) ve [ikinci](updating-deleting-and-creating-data.md) seri bölümlerini.</span><span class="sxs-lookup"><span data-stu-id="7e44e-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="7e44e-111">Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede</span><span class="sxs-lookup"><span data-stu-id="7e44e-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="7e44e-112">İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="7e44e-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="7e44e-113">Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="7e44e-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="7e44e-114">Yapı</span><span class="sxs-lookup"><span data-stu-id="7e44e-114">What you'll build</span></span>

<span data-ttu-id="7e44e-115">Bu öğreticide, gerekir:</span><span class="sxs-lookup"><span data-stu-id="7e44e-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="7e44e-116">Modelinize öğrencinin kayıt tarihi kaydetmek için özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="7e44e-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="7e44e-117">JQuery UI Datepicker pencere öğesini kullanarak kayıt tarihi seçmek kullanıcı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="7e44e-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="7e44e-118">Doğrulama kurallarını zorunlu tutmak için kayıt tarihi</span><span class="sxs-lookup"><span data-stu-id="7e44e-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="7e44e-119">JQuery UI Datepicker pencere öğesi kolayca bir tarih kullanıcı alanı ile etkileşim kurarken, açılır takvimden kullanıcıların seçmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e44e-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="7e44e-120">Bu pencere öğesini kullanarak bir tarih el ile yazmak daha kullanıcılar için daha kullanışlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="7e44e-121">Model bağlama için veri işlemleri kullanan bir sayfasına Datepicker pencere öğesini tümleştirmek az miktarda ek iş gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="7e44e-122">Yeni bir özellik için model ekleme</span><span class="sxs-lookup"><span data-stu-id="7e44e-122">Add a new property to the model</span></span>

<span data-ttu-id="7e44e-123">İlk olarak, ekleyecek bir **Datetime** , Öğrenci özelliğine model ve bu değişikliği veritabanına geçirme.</span><span class="sxs-lookup"><span data-stu-id="7e44e-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="7e44e-124">Açık **UniversityModels.cs**ve Öğrenci modeline vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="7e44e-125">**RangeAttribute** özelliği için doğrulama kurallarını zorunlu tutmak için eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="7e44e-126">Bu öğreticide, Contoso University 1 Ocak 2013'ün kurulmuştur ve bu nedenle önceki kayıt tarihleri geçerli olmayan varsayacağız.</span><span class="sxs-lookup"><span data-stu-id="7e44e-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="7e44e-127">Paket Yönetimi penceresinde, bir geçiş komutunu çalıştırarak ekleyin **add-migration AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="7e44e-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="7e44e-128">Geçiş kodu Öğrenci tabloya yeni bir Datetime sütun eklediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="7e44e-129">RangeAttribute içinde belirtilen değerle eşleşecek şekilde aşağıda vurgulanan kodda gösterildiği gibi yeni bir sütun için varsayılan bir değer ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="7e44e-130">Değişikliğiniz geçiş dosyasına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-130">Save your change to the migration file.</span></span>

<span data-ttu-id="7e44e-131">Verileri yeniden çekirdek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="7e44e-131">You do not need to seed the data again.</span></span> <span data-ttu-id="7e44e-132">Bu nedenle, açık **Configuration.cs** geçişler klasöründe ve kaldırın veya açıklama kod çıkışı **çekirdek** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7e44e-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="7e44e-133">Dosyayı kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="7e44e-133">Save and close the file.</span></span>

<span data-ttu-id="7e44e-134">Şimdi, komutu çalıştırmak **update-database**.</span><span class="sxs-lookup"><span data-stu-id="7e44e-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="7e44e-135">Sütun artık veritabanında var ve tüm var olan kayıtlar için EnrollmentDate varsayılan değere sahip dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="7e44e-136">Kayıt tarihi dinamik denetimleri ekleme</span><span class="sxs-lookup"><span data-stu-id="7e44e-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="7e44e-137">Şimdi görüntülemek ve kayıt tarihi düzenleme denetimleri ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="7e44e-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="7e44e-138">Bu noktada, değeri metin kutusu düzenlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="7e44e-139">Daha sonra öğreticide JQuery Datepicker pencere öğesi için metin kutusunu değiştirir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="7e44e-140">İlk olarak, herhangi bir değişiklik yapmak gerekmez unutmamalısınız **AddStudent.aspx** dosya.</span><span class="sxs-lookup"><span data-stu-id="7e44e-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="7e44e-141">DynamicEntity denetimi otomatik olarak yeni özellik işlemez.</span><span class="sxs-lookup"><span data-stu-id="7e44e-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="7e44e-142">Açık **Students.aspx**ve aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="7e44e-143">Uygulamayı çalıştırın ve bir tarih yazarak kayıt tarih değerini ayarlayabilir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="7e44e-144">Yeni bir öğrenci eklerken:</span><span class="sxs-lookup"><span data-stu-id="7e44e-144">When adding a new student:</span></span>

![tarihi ayarlama](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="7e44e-146">Ya da var olan bir değerle düzenleme:</span><span class="sxs-lookup"><span data-stu-id="7e44e-146">Or, editing an existing value:</span></span>

![düzenleme tarihi](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="7e44e-148">Tarih çalışır, ancak yazarak sağlamak istediğiniz müşteri deneyimi olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="7e44e-149">Sonraki bölümde, bir takvim aracılığıyla bir tarih seçme olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e44e-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="7e44e-150">JQuery UI ile çalışmak için NuGet paket yüklemesi</span><span class="sxs-lookup"><span data-stu-id="7e44e-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="7e44e-151">**Suyu UI** NuGet paketi web uygulamanıza JQuery UI pencere öğeleri kolay tümleştirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e44e-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="7e44e-152">Bu paket kullanmak için NuGet yükleyin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-152">To use this package, install it through NuGet.</span></span>

![suyu UI ekleme](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="7e44e-154">Yüklediğiniz suyu UI sürümü, uygulamanızda JQuery sürümüyle çakışabilir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="7e44e-155">Bu öğretici ile devam etmeden önce uygulamayı çalıştırmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="7e44e-156">JavaScript hatayla karşılaşırsanız, JQuery sürüm mutabık kılmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="7e44e-157">Komut dosyaları klasörünüze (sürüm 1.8.2 Bu öğretici yazma zamanında) JQuery beklenen sürümü eklemek veya Site.master içinde JQuery dosyasının yolunu belirtin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="7e44e-158">DatePicker pencere öğesi eklemek için DateTime şablonunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="7e44e-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="7e44e-159">Bir tarih saat değeri düzenlemek için dinamik veri şablonu Datepicker pencere öğesi ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="7e44e-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="7e44e-160">Şablona pencere öğesi ekleyerek onu otomatik olarak yeni bir öğrenci eklemek için her iki formun ve ızgara görünümü düzenleme Öğrenciler için işlenir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="7e44e-161">Açık **DateTime\_Edit.ascx**ve aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="7e44e-162">Arka plan kod dosyasına DatePicker için minimum ve maksimum tarihleri ayarlamak.</span><span class="sxs-lookup"><span data-stu-id="7e44e-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="7e44e-163">Bu değerleri ayarlayarak, kullanıcıların geçersiz tarihleri gezinme engeller.</span><span class="sxs-lookup"><span data-stu-id="7e44e-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="7e44e-164">Minimum ve maksimum değerleri alacak **RangeAttribute** bir tane belirtilmişse DateTime özelliği üzerinde.</span><span class="sxs-lookup"><span data-stu-id="7e44e-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="7e44e-165">Açık **DateTime\_Edit.ascx.cs**ve aşağıdaki vurgulanmış kodu sayfasına ekleme\_yükleme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7e44e-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="7e44e-166">Web uygulamasını çalıştırmak ve AddStudent sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="7e44e-167">Alanlar için değerler sağlayın ve kayıt tarihini metin kutusunu tıklattığınızda, takvim görüntülenir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="7e44e-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Tarih Seçici](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="7e44e-169">Bir tarihi seçin ve'ı tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="7e44e-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="7e44e-170">RangeAttribute doğrulama sunucusundaki zorlar.</span><span class="sxs-lookup"><span data-stu-id="7e44e-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="7e44e-171">Üzerinde Datepicker minDate özelliği ayarlanarak, ayrıca doğrulama istemcide uygulayın.</span><span class="sxs-lookup"><span data-stu-id="7e44e-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="7e44e-172">Takvim tarihi minDate değerini önce gidin kullanıcı izin vermez.</span><span class="sxs-lookup"><span data-stu-id="7e44e-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="7e44e-173">Izgara görünümünde bir kaydı düzenlediğinizde, Takvim de görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7e44e-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![GridView DatePicker](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="7e44e-175">Sonuç</span><span class="sxs-lookup"><span data-stu-id="7e44e-175">Conclusion</span></span>

<span data-ttu-id="7e44e-176">Bu öğreticide, model bağlama kullanan bir web formu JQuery pencere öğesi dahil öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="7e44e-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="7e44e-177">Sonraki [öğretici](using-query-string-values-to-retrieve-data.md), verileri seçerken bir sorgu dizesi değeri kullanır.</span><span class="sxs-lookup"><span data-stu-id="7e44e-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7e44e-178">[Önceki](sorting-paging-and-filtering-data.md)
> [sonraki](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="7e44e-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
