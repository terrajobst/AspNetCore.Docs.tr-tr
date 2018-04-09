---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: HTML Yardımcıları (C#) oluşturmak için TagBuilder sınıfını kullanma | Microsoft Docs
author: StephenWalther
description: Stephen Walther yararlı yardımcı program sınıfı TagBuilder sınıfı adlı ASP.NET MVC çerçevesi arasında tanıtır. TagBuilder sınıfına kolayca kullanabilirsiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c0e8e4e3a733f2cc8690dc85e3006bce6c661d2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="1c06e-104">HTML Yardımcıları (C#) oluşturmak için TagBuilder sınıfını kullanma</span><span class="sxs-lookup"><span data-stu-id="1c06e-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="1c06e-105">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="1c06e-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="1c06e-106">Stephen Walther yararlı yardımcı program sınıfı TagBuilder sınıfı adlı ASP.NET MVC çerçevesi arasında tanıtır.</span><span class="sxs-lookup"><span data-stu-id="1c06e-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="1c06e-107">TagBuilder sınıfı kolayca HTML etiketleri oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c06e-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="1c06e-108">ASP.NET MVC çerçevesi HTML Yardımcıları oluştururken kullanabileceğiniz TagBuilder sınıfı adlı bir yararlı yardımcı program sınıfı içerir.</span><span class="sxs-lookup"><span data-stu-id="1c06e-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="1c06e-109">Sınıfın adını da anlaşılacağı gibi TagBuilder sınıfı, HTML etiketleri kolayca oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1c06e-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="1c06e-110">Kısa Bu öğreticide, TagBuilder sınıfı bir bakış sağlanır ve basit bir HTML Yardımcısı oluşturmaya HTML işleyen olduğunda bu sınıfının nasıl kullanılacağını öğrenin &lt;img&gt; etiketler.</span><span class="sxs-lookup"><span data-stu-id="1c06e-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="1c06e-111">TagBuilder sınıfı genel bakış</span><span class="sxs-lookup"><span data-stu-id="1c06e-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="1c06e-112">TagBuilder sınıfı System.Web.Mvc ad alanında yer alır.</span><span class="sxs-lookup"><span data-stu-id="1c06e-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="1c06e-113">Beş yöntemi vardır:</span><span class="sxs-lookup"><span data-stu-id="1c06e-113">It has five methods:</span></span>

- <span data-ttu-id="1c06e-114">AddCssClass() - yeni bir eklemenize olanak sağlayan *class = ""* özniteliği için bir etiket.</span><span class="sxs-lookup"><span data-stu-id="1c06e-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="1c06e-115">GenerateId() - ID özniteliği için bir etiket eklemenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1c06e-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="1c06e-116">Bu yöntem otomatik olarak Kimliği'nde noktaları değiştiren (varsayılan olarak, alt çizgi tarafından nokta değiştirilir)</span><span class="sxs-lookup"><span data-stu-id="1c06e-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="1c06e-117">MergeAttribute() - bir etiket için öznitelikler eklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="1c06e-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="1c06e-118">Bu yöntemin birden çok aşırı vardır.</span><span class="sxs-lookup"><span data-stu-id="1c06e-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="1c06e-119">SetInnerText() - etiketinin iç metni ayarlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1c06e-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="1c06e-120">HTML kodlama otomatik olarak iç metindir.</span><span class="sxs-lookup"><span data-stu-id="1c06e-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="1c06e-121">ToString() - etiket oluşturmak etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="1c06e-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="1c06e-122">Normal bir etiketi, bir başlangıç etiketi, bir bitiş etiketi veya kendi kendine kapanan etiketi oluşturmak isteyip istemediğinizi belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c06e-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="1c06e-123">TagBuilder sınıfı dört önemli özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1c06e-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="1c06e-124">Öznitelikleri - tüm etiket özniteliklerini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="1c06e-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="1c06e-125">IdAttributeDotReplacement - GenerateId() yöntemi tarafından (varsayılan değer altçizgidir) nokta değiştirmek için kullanılan karakteri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="1c06e-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="1c06e-126">InnerHtml - etiketinin iç içeriğini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="1c06e-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="1c06e-127">Bu özellik bir dize atama *yok* HTML kodlama dizesi.</span><span class="sxs-lookup"><span data-stu-id="1c06e-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="1c06e-128">TagName - etiketin adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="1c06e-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="1c06e-129">Bu yöntemlere ve özelliklere tüm temel yöntemleri ve bir HTML etiketi oluşturmak için gereken özelliklerini verin.</span><span class="sxs-lookup"><span data-stu-id="1c06e-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="1c06e-130">Gerçekten TagBuilder sınıfı kullanmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="1c06e-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="1c06e-131">Bunun yerine bir StringBuilder sınıfını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c06e-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="1c06e-132">Ancak, TagBuilder sınıfı yaşamınızı biraz daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="1c06e-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="1c06e-133">Bir görüntü HTML Yardımcısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="1c06e-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="1c06e-134">TagBuilder sınıfının bir örneğini oluştururken TagBuilder oluşturucuya oluşturmak istediğiniz etiketin adını geçirin.</span><span class="sxs-lookup"><span data-stu-id="1c06e-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="1c06e-135">Ardından, etiket özniteliklerini değiştirmek için AddCssClass ve MergeAttribute() yöntemleri gibi yöntemleri çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="1c06e-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="1c06e-136">Son olarak, etiket oluşturmak için ToString() yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="1c06e-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="1c06e-137">Örneğin, 1 listeleyen bir görüntü HTML Yardımcısı içerir.</span><span class="sxs-lookup"><span data-stu-id="1c06e-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="1c06e-138">Image helper dahili bir HTML temsil eden bir TagBuilder ile uygulanan &lt;img&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="1c06e-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="1c06e-139">**1 - Helpers\ImageHelper.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="1c06e-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="1c06e-140">Listeleme 1 sınıfında görüntü adlı iki statik aşırı yüklenmiş yöntemler içerir.</span><span class="sxs-lookup"><span data-stu-id="1c06e-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="1c06e-141">Image() yöntemini çağırdığınızda, veya bir dizi HTML özniteliklerini temsil eden bir nesne geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c06e-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="1c06e-142">TagBuilder.MergeAttribute() yöntemi TagBuilder src özniteliğini gibi tek tek öznitelikler eklemek için nasıl kullanıldığını dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="1c06e-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="1c06e-143">Ayrıca, fark nasıl TagBuilder.MergeAttributes() yöntemi TagBuilder özniteliklerin bir koleksiyonu eklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1c06e-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="1c06e-144">Bir sözlük MergeAttributes() yöntemi kabul&lt;dize, nesne&gt; parametresi.</span><span class="sxs-lookup"><span data-stu-id="1c06e-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="1c06e-145">RouteValueDictionary sınıf öznitelikleri koleksiyon bir sözlük olarak temsil eden nesneyi dönüştürmek için kullanılan&lt;dize, nesne&gt;.</span><span class="sxs-lookup"><span data-stu-id="1c06e-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="1c06e-146">Image helper oluşturduktan sonra ASP.NET MVC görünümlerinizde gibi diğer standart HTML Yardımcıları hiçbirini yardımcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c06e-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="1c06e-147">Xbox aynı görüntüsü iki kez görüntülemek için Image helper listeleme 2 görünümünde kullanır (bkz: Şekil 1).</span><span class="sxs-lookup"><span data-stu-id="1c06e-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="1c06e-148">Image() Yardımcısı, hem ile hem de bir HTML öznitelik koleksiyonu olmadan adı verilir.</span><span class="sxs-lookup"><span data-stu-id="1c06e-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="1c06e-149">**Listing 2 - Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="1c06e-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="1c06e-150">[![Yeni Proje iletişim kutusu](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1c06e-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="1c06e-151">**Şekil 01**: yansıma Yardımcısını kullanarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="1c06e-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="1c06e-152">Bildirim Index.aspx görünüm üstündeki Image helper ilişkili ad alanı içeri aktarmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1c06e-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="1c06e-153">Yardımcı aşağıdaki yönergesi alınır:</span><span class="sxs-lookup"><span data-stu-id="1c06e-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> <span data-ttu-id="1c06e-154">[Önceki](creating-custom-html-helpers-cs.md)
> [sonraki](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1c06e-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
