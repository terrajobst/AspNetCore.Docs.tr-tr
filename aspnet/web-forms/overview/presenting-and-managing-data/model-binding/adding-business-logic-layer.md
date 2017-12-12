---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: "Model bağlama ve web forms kullanan projesine ekleniyor iş mantığı katmanı | Microsoft Docs"
author: tfitzmac
description: "Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: ca50690052cca73a718342a9725c8096a72f1187
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="ad5cf-104">Model bağlama ve web forms kullanan projesine ekleniyor iş mantığı katmanı</span><span class="sxs-lookup"><span data-stu-id="ad5cf-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="ad5cf-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ad5cf-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ad5cf-106">Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="ad5cf-107">Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="ad5cf-108">Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="ad5cf-109">Bu öğretici, bir iş mantığı katmanı ile model bağlama kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="ad5cf-110">Geçerli sayfa dışındaki bir nesnenin veri yöntemleri çağırmak için kullanıldığını belirtmek için OnCallingDataMethods üye kümesi.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="ad5cf-111">Bu öğreticide oluşturduğunuz proje inşa edilmiştir [önceki](retrieving-data.md) seri bölümlerini.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="ad5cf-112">Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede</span><span class="sxs-lookup"><span data-stu-id="ad5cf-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="ad5cf-113">İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="ad5cf-114">Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="ad5cf-115">Yapı</span><span class="sxs-lookup"><span data-stu-id="ad5cf-115">What you'll build</span></span>

<span data-ttu-id="ad5cf-116">Model bağlama, veri etkileşim kodunuzu bir web sayfası için arka plan kod dosyasına veya ayrı iş mantığı sınıfında yerleştirilmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="ad5cf-117">Önceki öğreticileri için veri etkileşim kodu arka plan kod dosyaları kullanmayı göstermiştir.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="ad5cf-118">Küçük siteler için bu yaklaşım çalışır ancak büyük site bakımı yapılırken kod yineleme ve büyük zorluk açabilir.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="ad5cf-119">Ayrıca program aracılığıyla bir soyutlama katmanı olduğundan, dosyaları arkasındaki kodda bulunan kodu test etmek oldukça zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="ad5cf-120">Veri etkileşim kodu merkezileştirmek için tüm veriler ile etkileşim için mantığı içeren bir iş mantığı katmanı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="ad5cf-121">Ardından, web sayfalarından iş mantığı katmanı çağırın.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="ad5cf-122">Bu öğretici, tüm önceki öğreticiler bir iş mantığı katmanı içine yazılmış Code taşıyın ve sonra bu kodun sayfalarından gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="ad5cf-123">Bu öğreticide, gerekir:</span><span class="sxs-lookup"><span data-stu-id="ad5cf-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="ad5cf-124">Kodu arka plan kodu dosyalarından bir iş mantığı katmanı taşıyın.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="ad5cf-125">Değişiklik iş mantığı katmanı yöntemleri çağırmak için veri bağlama denetimleri</span><span class="sxs-lookup"><span data-stu-id="ad5cf-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="ad5cf-126">İş mantığı katmanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="ad5cf-126">Create business logic layer</span></span>

<span data-ttu-id="ad5cf-127">Şimdi, web sayfaları adlı sınıfı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="ad5cf-128">Bu sınıftaki yöntemlerin önceki eğitimlerine kullanılan yöntemlere benzer ve değer sağlayıcı öznitelikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="ad5cf-129">İlk olarak, adlı yeni bir klasör ekleyin **BLL**.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-129">First, add a new folder called **BLL**.</span></span>

![klasör ekleme](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="ad5cf-131">BLL klasöründe adlı yeni bir sınıf oluşturun **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="ad5cf-132">İlk olarak arka plan kod dosyalarında belgeler veri işlemlerin içerecektir.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="ad5cf-133">Yöntemleri neredeyse arka plan kod dosyasına yöntemleri aynıdır, ancak bazı değişiklikler dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="ad5cf-134">Not etmek için en önemli koddan örneği içinde artık yürütüldüğünü değişikliktir **sayfa** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="ad5cf-135">Sayfa sınıfı içeren **TryUpdateModel** yöntemi ve **ModelState** özelliği.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="ad5cf-136">Bu kod bir iş mantığı katmanı taşındığında, bu üyeler çağırmak için sayfa sınıfının bir örneği artık yok.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="ad5cf-137">Bu sorunu çözmek almak için eklemelisiniz bir **ModelMethodContext** TryUpdateModel veya ModelState erişen herhangi bir yönteme parametre.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="ad5cf-138">TryUpdateModel arayın veya ModelState almak için bu ModelMethodContext parametresini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="ad5cf-139">Bu yeni parametre için hesap için web sayfasındaki değişikliği gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="ad5cf-140">SchoolBL.cs kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="ad5cf-141">İş mantığı katmanından verileri almak için var olan sayfaları gözden geçirin</span><span class="sxs-lookup"><span data-stu-id="ad5cf-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="ad5cf-142">Son olarak, iş mantığı katmanı kullanarak için arka plan kod dosyasına sorgularını kullanarak Students.aspx, AddStudent.aspx ve Courses.aspx sayfaları dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="ad5cf-143">Öğrenciler, AddStudent ve kurslar için arka plan kod dosyalarında silin veya aşağıdaki sorgu metotları Açıklama:</span><span class="sxs-lookup"><span data-stu-id="ad5cf-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="ad5cf-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="ad5cf-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="ad5cf-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="ad5cf-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="ad5cf-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="ad5cf-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="ad5cf-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="ad5cf-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="ad5cf-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="ad5cf-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="ad5cf-149">Veri işlemleriyle ilgili arka plan kod dosyasına hiçbir kod artık olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="ad5cf-150">**OnCallingDataMethods** olay işleyicisi için veri yöntemleri kullanmak için bir nesne belirtmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="ad5cf-151">Students.aspx, bu olay işleyicisi için bir değer ekleyin ve veri yöntemlerin adlarını iş mantığı sınıftaki yöntemlerin adlarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="ad5cf-152">Students.aspx arka plan kod dosyasına CallingDataMethods olay için olay işleyicisini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="ad5cf-153">Bu olay işleyicisi veri işlemleri için iş mantığı sınıf belirtin.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="ad5cf-154">AddStudent.aspx içinde benzer değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="ad5cf-155">Courses.aspx içinde benzer değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="ad5cf-156">Uygulamayı çalıştırın ve daha önce sahip oldukları gibi tüm sayfaları işlev dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="ad5cf-157">Doğrulama mantığını de düzgün çalışır.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ad5cf-158">Sonuç</span><span class="sxs-lookup"><span data-stu-id="ad5cf-158">Conclusion</span></span>

<span data-ttu-id="ad5cf-159">Bu öğreticide, bir veri erişim katmanı ve iş mantığı katmanı kullanmak için uygulamanızı yeniden yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="ad5cf-160">Veri denetimleri veri işlemleri için geçerli sayfa değil bir nesne kullanmak belirttiniz.</span><span class="sxs-lookup"><span data-stu-id="ad5cf-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ad5cf-161">Önceki</span><span class="sxs-lookup"><span data-stu-id="ad5cf-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
