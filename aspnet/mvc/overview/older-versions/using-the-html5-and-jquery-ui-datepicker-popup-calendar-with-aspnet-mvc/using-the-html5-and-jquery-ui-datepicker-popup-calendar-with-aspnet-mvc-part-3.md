---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: HTML5 ve jQuery UI Datepicker Popup Calendar ile ASP.NET MVC - 3. Kısım kullanarak | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar, ASP.NET MV ile çalışmaya ilişkin temel bilgileri sağlanır...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: baaca74d584a6d5028824e877c561494cdff044e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577229"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="61900-103">HTML5 ve jQuery UI Datepicker Popup Calendar ile ASP.NET MVC - 3. Kısım kullanma</span><span class="sxs-lookup"><span data-stu-id="61900-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="61900-104">Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="61900-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="61900-105">Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar'içinde bir ASP.NET MVC Web uygulaması ile çalışmaya ilişkin temel bilgileri sağlanır.</span><span class="sxs-lookup"><span data-stu-id="61900-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="61900-106">Karmaşık türleri ile çalışma</span><span class="sxs-lookup"><span data-stu-id="61900-106">Working with Complex Types</span></span>

<span data-ttu-id="61900-107">Bu bölümde bir adres sınıfı oluşturmak ve görüntülemek için bir şablon oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="61900-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="61900-108">İçinde *modelleri* klasöründe adlı yeni bir sınıf dosyası oluşturma *Person.cs* nerede yerleştirdiğiniz iki tür: bir `Person` sınıfı ve bir `Address` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="61900-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="61900-109">`Person` Sınıfı olarak belirlenmiş bir özellik içerecek `Address`.</span><span class="sxs-lookup"><span data-stu-id="61900-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="61900-110">`Address` Türü olup olmadığı gibi yerleşik türlerden birinde yani bir karmaşık tür `int`, `string`, veya `double`.</span><span class="sxs-lookup"><span data-stu-id="61900-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="61900-111">Bunun yerine, birçok özelliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="61900-111">Instead, it has several properties.</span></span> <span data-ttu-id="61900-112">Yeni sınıflar için kod şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="61900-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="61900-113">İçinde `Movie` denetleyici ekleyin aşağıdaki `PersonDetail` eylemi bir kişi örneği görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="61900-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="61900-114">Ardından aşağıdaki kodu ekleyin `Movie` doldurmak için denetleyici `Person` model bazı örnek verilerle:</span><span class="sxs-lookup"><span data-stu-id="61900-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="61900-115">Açık *Views\Movies\PersonDetail.cshtml* dosya ve eklemek için aşağıdaki biçimlendirme `PersonDetail` görünümü.</span><span class="sxs-lookup"><span data-stu-id="61900-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="61900-116">Uygulamayı çalıştırmak ve gitmek için CTRL + F5 tuşlarına basın *filmler/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="61900-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="61900-117">`PersonDetail` Görünüm içermiyor `Address` karmaşık tür olarak bu ekran görüntüsünde görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="61900-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="61900-118">(Adres gösterilen.)</span><span class="sxs-lookup"><span data-stu-id="61900-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="61900-119">`Address` Karmaşık bir tür olduğundan model verileri görüntülenmez.</span><span class="sxs-lookup"><span data-stu-id="61900-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="61900-120">Adres bilgileri görüntülemek için Aç *Views\Movies\PersonDetail.cshtml* yeniden dosyasını açıp aşağıdaki işaretlemeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61900-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="61900-121">İçin tam biçimlendirme `PersonDetail` artık görünümü şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="61900-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="61900-122">Uygulamayı yeniden çalıştırın ve görüntüleme `PersonDetail` görünümü.</span><span class="sxs-lookup"><span data-stu-id="61900-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="61900-123">Adres bilgileri artık görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="61900-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="61900-124">Karmaşık tür için bir şablon oluşturma</span><span class="sxs-lookup"><span data-stu-id="61900-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="61900-125">Bu bölümde, işlemek için kullanılan bir şablon oluşturursunuz `Address` karmaşık tür.</span><span class="sxs-lookup"><span data-stu-id="61900-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="61900-126">Bir şablon oluştururken `Address` türü, ASP.NET MVC otomatik olarak kullanabilir, uygulama başka bir yerindeki bir adresi modeli biçimlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="61900-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="61900-127">Bu, işlenmesi denetlemek için bir yol sağlar `Address` uygulamada tek yerden türü.</span><span class="sxs-lookup"><span data-stu-id="61900-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="61900-128">İçinde *views\shared\displaytemplates konumunda* klasöründe adlı kesin türü belirtilmiş bir kısmi görünüm oluşturma **adresi**:</span><span class="sxs-lookup"><span data-stu-id="61900-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="61900-129">Tıklayın **Ekle**ve ardından yeni açın *Views\Shared\DisplayTemplates\Address.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="61900-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="61900-130">Yeni Görünüm aşağıdaki oluşturulan biçimlendirme içeriyor:</span><span class="sxs-lookup"><span data-stu-id="61900-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="61900-131">Uygulamayı çalıştırmak ve görüntüleme `PersonDetail` görünümü.</span><span class="sxs-lookup"><span data-stu-id="61900-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="61900-132">Bu kez, `Address` yeni oluşturduğunuz şablonu görüntülemek için kullanılan `Address` karmaşık tür ekran aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="61900-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="61900-133">Özet: yolları modeli görüntüleme biçimi ve şablonu belirtin</span><span class="sxs-lookup"><span data-stu-id="61900-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="61900-134">Biçimi veya şablon için bir model özelliğine aşağıdaki yaklaşımlardan kullanarak belirtebilirsiniz, öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="61900-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="61900-135">Uygulama `DisplayFormat` modelinde bir özellik için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="61900-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="61900-136">Örneğin, aşağıdaki kod, tarih saat görüntülenecek neden olur:</span><span class="sxs-lookup"><span data-stu-id="61900-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="61900-137">Uygulama bir [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) modeli ve veri türünü belirten bir özelliği özniteliği.</span><span class="sxs-lookup"><span data-stu-id="61900-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="61900-138">Örneğin, aşağıdaki kod, tarih saat görüntülenecek neden olur.</span><span class="sxs-lookup"><span data-stu-id="61900-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="61900-139">Uygulama içeriyorsa, bir *date.cshtml* şablonunda *views\shared\displaytemplates konumunda* klasör veya *Views\Movies\DisplayTemplates* klasör, bu şablonu işlemek için kullanılan `DateTime` özelliği.</span><span class="sxs-lookup"><span data-stu-id="61900-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="61900-140">Aksi takdirde yerleşik ASP.NET şablon oluşturma sistem özelliği olarak bir tarih görüntüler.</span><span class="sxs-lookup"><span data-stu-id="61900-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="61900-141">Bir ekran şablonu oluşturma *views\shared\displaytemplates konumunda* klasör veya *Views\Movies\DisplayTemplates* klasör adıyla eşleşen biçimlendirmek istediğiniz veri türü.</span><span class="sxs-lookup"><span data-stu-id="61900-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="61900-142">Gördüğünüz gibi *Views\Shared\DisplayTemplates\DateTime.cshtml* işlemek için kullanılan `DateTime` modeline bir öznitelik ekleyerek ve tüm biçimlendirme görünümlere ekleme olmadan bir model özellikleri.</span><span class="sxs-lookup"><span data-stu-id="61900-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="61900-143">Kullanarak [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) modelin model özelliği görüntülemek için şablon belirtmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="61900-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="61900-144">Açıkça görünen şablon adının ekleme [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) Görünümü'nde arayın.</span><span class="sxs-lookup"><span data-stu-id="61900-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="61900-145">Kullandığınız yaklaşım, uygulamanızda yapmanız gerekenler üzerinde bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="61900-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="61900-146">İhtiyacınız olan biçimlendirme tam olarak türünü almak için bu yaklaşımları karıştırmak durumdur.</span><span class="sxs-lookup"><span data-stu-id="61900-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="61900-147">Sonraki bölümde, geçiş biraz gears ve verilerin nasıl girildiğini özelleştirme için nasıl görüntüleneceğini özelleştirmesini taşıyın.</span><span class="sxs-lookup"><span data-stu-id="61900-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="61900-148">JQuery datepicker tarihlerini belirtmek için bahsettiniz bir yol sağlamak üzere uygulamayı düzenleme görünümle denetime.</span><span class="sxs-lookup"><span data-stu-id="61900-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="61900-149">[Önceki](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [İleri](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="61900-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
