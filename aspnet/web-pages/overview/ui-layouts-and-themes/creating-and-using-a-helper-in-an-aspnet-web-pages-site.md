---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: "Oluşturma ve bir Yardımcısını kullanarak bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs"
author: tfitzmac
description: "Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı oluşturulacağını açıklar. Bir yardımcı kod ve performans için biçimlendirme içeren yeniden kullanılabilir bir bileşenidir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="685e2-104">Oluşturma ve bir ASP.NET Web sayfaları (Razor) sitesinde bir Yardımcısını kullanma</span><span class="sxs-lookup"><span data-stu-id="685e2-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="685e2-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="685e2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="685e2-106">Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı oluşturulacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="685e2-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="685e2-107">A *yardımcı* kod ve sıkıcı veya karmaşık olabilir bir görevi gerçekleştirmek için biçimlendirme içeren yeniden kullanılabilir bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="685e2-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="685e2-108">**Öğrenecekleriniz:**</span><span class="sxs-lookup"><span data-stu-id="685e2-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="685e2-109">Nasıl oluşturmak ve basit bir Yardımcısı'nı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="685e2-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="685e2-110">Bu makalede sunulan ASP.NET özellikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="685e2-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="685e2-111">`@helper` Sözdizimi.</span><span class="sxs-lookup"><span data-stu-id="685e2-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="685e2-112">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="685e2-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="685e2-113">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="685e2-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="685e2-114">Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="685e2-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="685e2-115">Yardımcıları genel bakış</span><span class="sxs-lookup"><span data-stu-id="685e2-115">Overview of Helpers</span></span>

<span data-ttu-id="685e2-116">Siteniz farklı sayfalarında aynı görevleri gerçekleştirmeniz gerekirse, yardımcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="685e2-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="685e2-117">ASP.NET Web sayfaları Yardımcıları çeşitli içerir ve indirmek ve yüklemek çok daha fazlasını vardır.</span><span class="sxs-lookup"><span data-stu-id="685e2-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="685e2-118">(Yerleşik ASP.NET Web Pages'de Yardımcıları listesini listelenen [ASP.NET API hızlı başvuru](https://go.microsoft.com/fwlink/?LinkId=202907).) Varolan Yardımcıları hiçbiri gereksinimlerinizi karşılıyorsa, kendi yardımcı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="685e2-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="685e2-119">Yardımcıyı, ortak bir kod bloğunu birden çok sayfada kullanmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="685e2-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="685e2-120">Sayfanızda genellikle normal paragraflar dışında ayarlanmış bir not öğesi oluşturmak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="685e2-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="685e2-121">Not belki de oluşturulur bir `<div>` öğesi, bir sınır içeren bir kutu olarak biçimlendirilmiş.</span><span class="sxs-lookup"><span data-stu-id="685e2-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="685e2-122">Not görüntülemek istediğiniz her zaman bir sayfaya bu aynı biçimlendirme eklemek yerine, bir yardımcı işaretleme paketleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="685e2-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="685e2-123">Not tek satırlık bir kod ile birlikte daha sonra ekleyebileceğiniz ihtiyacınız herhangi bir yere.</span><span class="sxs-lookup"><span data-stu-id="685e2-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="685e2-124">Böyle bir Yardımcısını kullanarak kodu her sayfalarınızın daha basit ve okuması daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="685e2-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="685e2-125">Notlar nasıl göründüğünü değiştirmeniz gerekiyorsa, tek bir yerde biçimlendirme değişebildiğinden, ayrıca, sitenizin korumak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="685e2-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="685e2-126">Yardımcıyı oluşturma</span><span class="sxs-lookup"><span data-stu-id="685e2-126">Creating a Helper</span></span>

<span data-ttu-id="685e2-127">Bu yordam yalnızca tanımlandığı gibi not oluşturur yardımcı oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="685e2-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="685e2-128">Bu basit bir örnektir, ancak özel yardımcı tüm biçimlendirme ve ihtiyacınız ASP.NET kodu içerebilir.</span><span class="sxs-lookup"><span data-stu-id="685e2-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="685e2-129">Web sitesinin kök klasöründe adlı bir klasör oluşturun *uygulama\_kod*.</span><span class="sxs-lookup"><span data-stu-id="685e2-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="685e2-130">Bu ayrılmış bir klasör ASP.NET kodu yardımcıları gibi bileşenleri için burada koyabilirsiniz adıdır.</span><span class="sxs-lookup"><span data-stu-id="685e2-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="685e2-131">İçinde *uygulama\_kod* yeni bir klasör oluşturun *.cshtml* dosya ve adlandırın *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="685e2-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="685e2-132">Varolan içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="685e2-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="685e2-133">Kod kullanan `@helper` adlı yeni bir Yardımcısı bildirmek için sözdizimi `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="685e2-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="685e2-134">Bu belirli yardımcı adlı bir parametre geçirmenize olanak verir `content` metin ve biçimlendirme bileşimini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="685e2-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="685e2-135">Not gövde kullanarak içine yardımcı bir dize ekler `@content` değişkeni.</span><span class="sxs-lookup"><span data-stu-id="685e2-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="685e2-136">Dosya adlandırılır fark *MyHelpers.cshtml*, ancak yardımcı adlı `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="685e2-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="685e2-137">Tek bir dosyaya birden çok özel Yardımcıları koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="685e2-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="685e2-138">Dosyayı kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="685e2-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="685e2-139">Bir sayfa Yardımcısını kullanma</span><span class="sxs-lookup"><span data-stu-id="685e2-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="685e2-140">Kök klasöründe adlı yeni boş bir dosya oluşturun *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="685e2-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="685e2-141">Dosyasına aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="685e2-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="685e2-142">Oluşturduğunuz Yardımcısı çağırmak için kullanırsınız `@` yardımcı olduğu bir nokta dosya adı ve ardından yardımcı adı gelir.</span><span class="sxs-lookup"><span data-stu-id="685e2-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="685e2-143">(Birden çok klasörleri olsaydı *uygulama\_kod* klasörü, sözdizimini kullanabilirsiniz `@FolderName.FileName.HelperName` yardımcınız herhangi içinde çağırmak için klasör düzeyinde iç içe).</span><span class="sxs-lookup"><span data-stu-id="685e2-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="685e2-144">Parantez içinde tırnak işaretleri içindeki eklediğiniz metin yardımcı Not web sayfasında bir parçası olarak görüntülenecek metindir.</span><span class="sxs-lookup"><span data-stu-id="685e2-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="685e2-145">Sayfayı kaydedin ve tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="685e2-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="685e2-146">Adlı burada yardımcı Not öğesi sağ yardımcı oluşturur: iki paragraf arasında.</span><span class="sxs-lookup"><span data-stu-id="685e2-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Tarayıcı ve Yardımcısı, belirtilen metnin etrafında bir kutu koyan biçimlendirme nasıl oluşturulacağını sayfasını gösteren ekran görüntüsü.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="685e2-148">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="685e2-148">Additional Resources</span></span>


<span data-ttu-id="685e2-149">[Yatay menü Razor yardımcı olarak](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="685e2-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="685e2-150">Bu blog girişi CAN Pope tarafından bir yatay menüsünün biçimlendirme, CSS ve kod kullanarak bir yardımcı nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="685e2-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="685e2-151">[ASP.NET Web HTML5 yararlanarak sayfaları Yardımcıları WebMatrix ve ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="685e2-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="685e2-152">Bu blog girişi Sam Abraham tarafından bir HTML5 işleyen yardımcıyı gösterir `Canvas` öğesi.</span><span class="sxs-lookup"><span data-stu-id="685e2-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="685e2-153">[Arasındaki farkı @Helpers ve @Functions webmatrix'te](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="685e2-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="685e2-154">Bu blog girişi CAN Brind tarafından açıklar `@helper` sözdizimi ve `@function` sözdizimi ve ne zaman her kullanılır.</span><span class="sxs-lookup"><span data-stu-id="685e2-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
