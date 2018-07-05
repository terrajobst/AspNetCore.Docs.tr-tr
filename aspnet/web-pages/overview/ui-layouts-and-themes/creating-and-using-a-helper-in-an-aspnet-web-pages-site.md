---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Oluşturma ve bir Yardımcısı kullanarak bir ASP.NET Web sayfaları (Razor) sitesinde | Microsoft Docs
author: tfitzmac
description: Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı oluşturulacağını açıklar. Bir yardımcı kod ve iyileştirilmiş işaretlemede içeren yeniden kullanılabilir bir bileşen olan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5e73beabf973a20335112a0d7dc5ecb6b8817744
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394618"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="73de3-104">Oluşturma ve bir ASP.NET Web sayfaları (Razor) sitesinde bir Yardımcısını kullanma</span><span class="sxs-lookup"><span data-stu-id="73de3-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="73de3-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="73de3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="73de3-106">Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı oluşturulacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="73de3-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="73de3-107">A *Yardımcısı* kod ve yorucu bir süreç veya karmaşık olabilir bir görevi gerçekleştirmek için biçimlendirme içeren yeniden kullanılabilir bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="73de3-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="73de3-108">**Öğrenecekleriniz:**</span><span class="sxs-lookup"><span data-stu-id="73de3-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="73de3-109">Nasıl oluşturmak ve basit bir Yardımcısı'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="73de3-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="73de3-110">Bu makalede sunulan ASP.NET özellikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="73de3-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="73de3-111">`@helper` Söz dizimi.</span><span class="sxs-lookup"><span data-stu-id="73de3-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="73de3-112">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="73de3-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="73de3-113">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="73de3-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="73de3-114">Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="73de3-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="73de3-115">Yardımcıları genel bakış</span><span class="sxs-lookup"><span data-stu-id="73de3-115">Overview of Helpers</span></span>

<span data-ttu-id="73de3-116">Aynı görevleri, sitedeki farklı sayfalar gerçekleştirmeniz gerekiyorsa yardımcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73de3-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="73de3-117">ASP.NET Web sayfaları içeren bir dizi yardımcıları ve indirip yükleme çok daha fazlası vardır.</span><span class="sxs-lookup"><span data-stu-id="73de3-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="73de3-118">(Bir listesi yerleşik ASP.NET Web Pages'de yardımcıların listelenen [ASP.NET API hızlı başvurusu](https://go.microsoft.com/fwlink/?LinkId=202907).) Mevcut Yardımcıları hiçbiri gereksinimlerinizi karşılamıyorsa, kendi Yardımcısı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73de3-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="73de3-119">Yardımcı, ortak bir kod bloğu birden çok sayfada kullanmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="73de3-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="73de3-120">Sayfanızın, genellikle normal paragraflar dışında ayarlanmış bir not öğesi oluşturmak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="73de3-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="73de3-121">Belki Not olarak oluşturulan bir `<div>` öğesi, bir sınır içeren bir kutu olarak biçimlendirilmiş.</span><span class="sxs-lookup"><span data-stu-id="73de3-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="73de3-122">Not görüntülemek istediğiniz her zaman bir sayfaya bu aynı biçimlendirme eklemek yerine, bir yardımcı işaretleme paketleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73de3-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="73de3-123">Daha sonra tek bir kod satırı ile Not ekleyebileceğiniz herhangi bir yere ihtiyacınız.</span><span class="sxs-lookup"><span data-stu-id="73de3-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="73de3-124">Böyle bir Yardımcısını kullanarak kodu her sayfalarınızın daha basit ve okuması daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="73de3-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="73de3-125">Notları nasıl görüneceğini değiştirmeniz gerekiyorsa, tek bir yerde işaretleme değişebildiğinden, ayrıca, sitenizin bakımı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="73de3-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="73de3-126">Bir yardımcı oluşturma</span><span class="sxs-lookup"><span data-stu-id="73de3-126">Creating a Helper</span></span>

<span data-ttu-id="73de3-127">Bu yordam yalnızca tanımlandığı gibi bir not oluşturur yardımcı oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="73de3-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="73de3-128">Bu basit bir örnektir, ancak özel yardımcı herhangi bir işaretleme ve ihtiyaç duyduğunuz ASP.NET kod içerebilir.</span><span class="sxs-lookup"><span data-stu-id="73de3-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="73de3-129">Web sitesinin kök klasöründe adlı bir klasör oluşturun *uygulama\_kod*.</span><span class="sxs-lookup"><span data-stu-id="73de3-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="73de3-130">Kod yardımcıları gibi bileşenlerin yere koyabilirsiniz ayrılmış bir klasör adı ASP.NET budur.</span><span class="sxs-lookup"><span data-stu-id="73de3-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="73de3-131">İçinde *uygulama\_kod* yeni bir klasör oluşturun *.cshtml* adlandırın ve dosya *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="73de3-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="73de3-132">Mevcut içeriğini aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="73de3-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="73de3-133">Kod `@helper` adlı yeni bir Yardımcısı bildirmek için söz dizimi `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="73de3-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="73de3-134">Bu belirli Yardımcısı adlı bir parametre olarak geçirmenize olanak tanır `content` metni ve biçimlendirmeyi bir birleşimini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="73de3-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="73de3-135">Yardımcı, Not gövdesi kullanarak bir dize ekler `@content` değişkeni.</span><span class="sxs-lookup"><span data-stu-id="73de3-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="73de3-136">Dosya olarak adlandırıldığına dikkat edin *MyHelpers.cshtml*, ancak yardımcı adlı `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="73de3-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="73de3-137">Tek bir dosyada birden çok özel Yardımcıları koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73de3-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="73de3-138">Dosyayı kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="73de3-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="73de3-139">Bir sayfa Yardımcısını kullanma</span><span class="sxs-lookup"><span data-stu-id="73de3-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="73de3-140">Adlı yeni bir boş dosyası kök klasöründe oluşturma *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="73de3-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="73de3-141">Dosyaya aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="73de3-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="73de3-142">Oluşturduğunuz Yardımcısı çağırmak için kullanın `@` yardımcı olduğu bir nokta, dosya adının ardından ve yardımcı adı.</span><span class="sxs-lookup"><span data-stu-id="73de3-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="73de3-143">(Birden çok klasör olsaydı *uygulama\_kod* klasörü, aşağıdaki söz dizimini kullanabilirsiniz `@FolderName.FileName.HelperName` yardımcınız herhangi içinde çağırmak için klasör düzeyinde iç içe geçmiş).</span><span class="sxs-lookup"><span data-stu-id="73de3-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="73de3-144">Parantez içindeki tırnak işaretleri içindeki eklediğiniz yardımcı Not web sayfasındaki bir parçası olarak görüntülenecek metni metindir.</span><span class="sxs-lookup"><span data-stu-id="73de3-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="73de3-145">Sayfayı kaydedin ve bir tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="73de3-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="73de3-146">Yardımcı adlı burada yardımcı Not öğesi hemen oluşturur: iki paragraflar arasındaki.</span><span class="sxs-lookup"><span data-stu-id="73de3-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Tarayıcı ve yardımcı belirtilen metin etrafına bir kutu koyar biçimlendirme nasıl oluşturulacağını sayfasını gösteren ekran görüntüsü.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="73de3-148">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="73de3-148">Additional Resources</span></span>


<span data-ttu-id="73de3-149">[Bir Razor yardımcı olarak yatay menü](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="73de3-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="73de3-150">Bu blog girişi Mike Pope tarafından biçimlendirme, CSS ve kod kullanarak bir yardımcı yatay menü oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="73de3-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="73de3-151">[Yararlanarak HTML5'te ASP.NET Web sayfaları Yardımcıları WebMatrix ve ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="73de3-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="73de3-152">Bu blog girişi Sam Abraham tarafından bir HTML5 işleyen bir yardımcı gösterir `Canvas` öğesi.</span><span class="sxs-lookup"><span data-stu-id="73de3-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="73de3-153">[Arasındaki fark @Helpers ve @Functions webmatrix'te](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="73de3-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="73de3-154">Bu blog girişi Mike Brind tarafından açıklar `@helper` söz dizimi ve `@function` söz dizimi ve ne zaman her kullanılır.</span><span class="sxs-lookup"><span data-stu-id="73de3-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
