---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: ASP.NET Web sayfaları sunarak - tutarlı bir düzen oluşturma | Microsoft Docs
author: tfitzmac
description: Bu öğretici düzenleri ASP.NET Web sayfaları kullanan bir site sayfalar için tutarlı bir görünüm oluşturmak için nasıl kullanılacağını gösterir. Tamamladığınızdan varsayar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899820"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="4b550-104">ASP.NET Web sayfaları sunarak - tutarlı bir düzen oluşturma</span><span class="sxs-lookup"><span data-stu-id="4b550-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="4b550-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4b550-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4b550-106">Bu öğretici nasıl kullanılacağını gösterir *düzenleri* ASP.NET Web sayfaları kullanan bir site sayfalar için tutarlı bir görünüm oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="4b550-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="4b550-107">Seri aracılığıyla tamamladığınızdan varsayar [veritabanı veri ASP.NET Web sayfalarını silme](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="4b550-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="4b550-108">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="4b550-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="4b550-109">Hangi bir düzen sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="4b550-109">What a layout page is.</span></span>
> - <span data-ttu-id="4b550-110">Düzen sayfaları ile dinamik içerik birleştirmek nasıl.</span><span class="sxs-lookup"><span data-stu-id="4b550-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="4b550-111">Bir düzen sayfası değerleri geçirmek nasıl.</span><span class="sxs-lookup"><span data-stu-id="4b550-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="4b550-112">Düzenler hakkında</span><span class="sxs-lookup"><span data-stu-id="4b550-112">About Layouts</span></span>

<span data-ttu-id="4b550-113">Şu ana kadar oluşturdum sayfaları tüm tam tek başına sayfa.</span><span class="sxs-lookup"><span data-stu-id="4b550-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="4b550-114">Bunların tümü aynı siteye ait, ancak tüm ortak öğeler veya standart bir görünüm yok.</span><span class="sxs-lookup"><span data-stu-id="4b550-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="4b550-115">Sitelerinin çoğu bir tutarlı bir görünüm ve düzeni sahip.</span><span class="sxs-lookup"><span data-stu-id="4b550-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="4b550-116">Örneğin, giderseniz [Microsoft.com/web](https://www.microsoft.com/web/) site ve araştırın, tüm sayfaları genel bir düzen ve görsel tema için uyması bakın:</span><span class="sxs-lookup"><span data-stu-id="4b550-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Üstbilgi, gezinti alanını, içerik alanının ve altbilgi düzenini gösteren Microsoft.com/web site sayfası](layouts/_static/image1.png)

<span data-ttu-id="4b550-118">Bir *verimsiz* bu düzeni oluşturmak için bir yol, bir başlığı, gezinti çubuğu ve altbilgi sayfalarınızın her biri üzerinde ayrı olarak tanımlamak için olacaktır.</span><span class="sxs-lookup"><span data-stu-id="4b550-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="4b550-119">Her zaman aynı biçimlendirme çoğaltma.</span><span class="sxs-lookup"><span data-stu-id="4b550-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="4b550-120">Bir şey değiştirmek istiyorsanız (örneğin, alt bilgi güncelleştirmesi), her sayfanın ayrı olarak değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4b550-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="4b550-121">Where olan *Düzen sayfaları* geldikçe.</span><span class="sxs-lookup"><span data-stu-id="4b550-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="4b550-122">ASP.NET Web Pages'da sitenizdeki sayfaları için genel bir kapsayıcı sağlayan düzen sayfası tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b550-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="4b550-123">Örneğin, düzen sayfası üstbilgi, gezinti alanını ve altbilgi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4b550-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="4b550-124">Düzen sayfası ana içeriğin nereye gideceğini yer tutucu içerir.</span><span class="sxs-lookup"><span data-stu-id="4b550-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="4b550-125">Ardından, biçimlendirme ve yalnızca bu sayfa için kod içeren her bir içerik sayfayı tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b550-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="4b550-126">İçerik sayfaları tam HTML sayfaları olmak zorunda değildir; için bile sahip olmayan bir `<body>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="4b550-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="4b550-127">Aynı zamanda bir içeriği görüntülemek istediğiniz hangi düzen sayfası ASP kod satırı sahiptirler.</span><span class="sxs-lookup"><span data-stu-id="4b550-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="4b550-128">Kabaca bu ilişkiyi nasıl çalıştığını gösteren resim şöyledir:</span><span class="sxs-lookup"><span data-stu-id="4b550-128">Here's a picture that shows roughly how this relationship works:</span></span>

![İki içerik sayfaları ve içine sığması düzen sayfası gösteren kavramsal diyagram](layouts/_static/image2.png)

<span data-ttu-id="4b550-130">Bu etkileşimi eylemde gördüğünüzde anlamak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="4b550-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="4b550-131">Bu öğreticide, bir düzeni kullanmak için filmler sayfalarınızı değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="4b550-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="4b550-132">Düzen sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="4b550-132">Adding a Layout Page</span></span>

<span data-ttu-id="4b550-133">Üstbilgi ve altbilgi ana içerik için bir alanı ile bir tipik sayfa düzeni tanımlayan düzen sayfası oluşturarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="4b550-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="4b550-134">Adlı CSHTML sayfa WebPagesMovies sitede ekleme  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4b550-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="4b550-135">Önde gelen alt çizgi ( `_` ) karakterdir önemli.</span><span class="sxs-lookup"><span data-stu-id="4b550-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="4b550-136">Bir sayfanın adı bir alt çizgiyle başlıyorsa, ASP.NET doğrudan bu sayfayı tarayıcıya göndermek olmaz.</span><span class="sxs-lookup"><span data-stu-id="4b550-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="4b550-137">Bu kural, kullanıcıları doğrudan isteği mümkün olmaması gerekir ancak bu, siteniz için gerekli olan sayfaları tanımlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="4b550-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="4b550-138">Sayfa içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4b550-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="4b550-139">Gördüğünüz gibi bu biçimlendirme kullanan yalnızca HTML'dir `<div>` üç bölüm sayfa artı bir daha tanımlamak için öğeleri `<div>` üç bölüm tutacak öğesi.</span><span class="sxs-lookup"><span data-stu-id="4b550-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="4b550-140">Altbilgi biraz Razor kod içerir: `@DateTime.Now.Year`, hangi sokacak geçerli yıl sayfasında bu konumda.</span><span class="sxs-lookup"><span data-stu-id="4b550-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="4b550-141">Adlı bir stil sayfası bağlantısını fark *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="4b550-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="4b550-142">Öğeleri fiziksel düzenini ayrıntılarını tanımlandığı stil sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="4b550-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="4b550-143">Bir dakika içinde oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="4b550-143">You'll create that in a moment.</span></span>

<span data-ttu-id="4b550-144">Bu yalnızca olağan dışı özelliği  *\_Layout.cshtml* sayfası `@Render.Body()` satır.</span><span class="sxs-lookup"><span data-stu-id="4b550-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="4b550-145">Bu düzen başka bir sayfaya ile birleştirildiğinde içeriği nereye yer tutucu olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="4b550-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="4b550-146">.Css dosyası ekleme</span><span class="sxs-lookup"><span data-stu-id="4b550-146">Adding a .css File</span></span>

<span data-ttu-id="4b550-147">Sayfada öğeleri gerçek düzenleme (diğer bir deyişle, Görünüm) tanımlamak için tercih edilen yol geçişli stil sayfası (CSS) kurallarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="4b550-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="4b550-148">Oluşturacağınız şekilde bir *.css* yeni düzeninizi kurallarını sahip dosya.</span><span class="sxs-lookup"><span data-stu-id="4b550-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="4b550-149">Webmatrix'te, sitenizin kök seçin.</span><span class="sxs-lookup"><span data-stu-id="4b550-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="4b550-150">Sonra **dosyaları** sekmesi altında Şeritinde oku **yeni** düğmesine tıklayın ve ardından **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="4b550-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![Şeritte 'Yeni Klasör' seçeneği yeni altında.](layouts/_static/image3.png)

<span data-ttu-id="4b550-152">Yeni bir klasör adı *stilleri*.</span><span class="sxs-lookup"><span data-stu-id="4b550-152">Name the new folder *Styles*.</span></span>

![Yeni Klasör 'Stiller' adlandırma](layouts/_static/image4.png)

<span data-ttu-id="4b550-154">Yeni içinde *stilleri* klasör adında bir dosya oluşturun *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="4b550-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Yeni bir Movies.css dosyası oluşturma](layouts/_static/image5.png)

<span data-ttu-id="4b550-156">Yeni Değiştir *.css* aşağıdaki dosyasıyla:</span><span class="sxs-lookup"><span data-stu-id="4b550-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="4b550-157">Çoğu iki şey not üzere dışında bu CSS kuralları hakkında dediğimiz olmaz.</span><span class="sxs-lookup"><span data-stu-id="4b550-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="4b550-158">Yazı tipleri ve boyutları ayarlamaya ek olarak, kuralları mutlak konumlandırma üstbilgi, altbilgi ve ana içerik alanı konumunu oluşturmak için kullandığınız paroladır.</span><span class="sxs-lookup"><span data-stu-id="4b550-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="4b550-159">CSS konumlandırma yenisiniz varsa, okuyabilirsiniz [CSS konumlandırma](http://www.w3schools.com/css/css_positioning.asp) W3Schools sitesindeki öğretici.</span><span class="sxs-lookup"><span data-stu-id="4b550-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="4b550-160">Alt kısmında, biz özgün olarak olan stil kurallarını kopyaladığınızdan emin tanımlanmış ayrı ayrı olarak Not başka bir şey *Movies.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="4b550-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="4b550-161">Bu kurallar de kullanılan [görüntüleme veri tarafından ASP.NET Web sayfalarını kullanarak giriş](https://go.microsoft.com/fwlink/?LinkId=251580) yapmak için öğretici `WebGrid` yardımcı işleme şeritler tabloya eklenen biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="4b550-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="4b550-162">(Kullanmak için kullanacaksanız bir *.css* dosya Stil tanımları için de stil kurallarını tüm sitenin içinde yerleştirdiğiniz.)</span><span class="sxs-lookup"><span data-stu-id="4b550-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="4b550-163">Düzen kullanılacak filmler dosyasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="4b550-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="4b550-164">Şimdi yeni düzeni kullanmak için sitenizdeki varolan dosyaların güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b550-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="4b550-165">Açık *Movies.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="4b550-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="4b550-166">En üstte, kod, ilk satır aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4b550-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="4b550-167">Şimdi şu şekilde başlatılır:</span><span class="sxs-lookup"><span data-stu-id="4b550-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="4b550-168">Bu bir kod satırı ASP olduğunda *filmler* sayfa çalıştırır, ile birleştirilmesini  *\_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="4b550-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="4b550-169">Bu yana *Movies.cshtml* dosya artık düzen sayfası kullanır, biçimlendirmeden kaldırabilirsiniz *Movies.cshtml* tarafından hallolduğuna sayfa  *\_Layout.cshtml*dosya.</span><span class="sxs-lookup"><span data-stu-id="4b550-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="4b550-170">Çıkın `<!DOCTYPE>`, `<html>`, ve `<body>` açma ve kapatma etiketleri.</span><span class="sxs-lookup"><span data-stu-id="4b550-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="4b550-171">Tüm alın `<head>` öğesi ve bu kuralları şimdi var ki bu yana hangi kılavuz için stil kurallarını içerir içeriği, bir *.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="4b550-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="4b550-172">Varolan adresinden iken değiştirme `<h1>` öğesine bir `<h2>` öğesi; sahip bir `<h1>` düzen sayfası zaten öğesinde.</span><span class="sxs-lookup"><span data-stu-id="4b550-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="4b550-173">Değişiklik `<h2>` "Listesi filmler" için metin.</span><span class="sxs-lookup"><span data-stu-id="4b550-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="4b550-174">Normal bir içerik sayfasında bu tür değişiklikler yapmak zorunda olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="4b550-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="4b550-175">Olan düzen sayfası başlattığınız siteniz olduğunda, tüm bu öğeler olmadan içerik sayfalarını başından itibaren oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4b550-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="4b550-176">Bu durumda, ancak, bir tek başına sayfa; böylece bir bit temizleme bir düzeni kullanan bir dönüştürdüğünüz.</span><span class="sxs-lookup"><span data-stu-id="4b550-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="4b550-177">İşiniz bittiğinde *Movies.cshtml* sayfasında, aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="4b550-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="4b550-178">Düzen test etme</span><span class="sxs-lookup"><span data-stu-id="4b550-178">Testing the Layout</span></span>

<span data-ttu-id="4b550-179">Şimdi düzenini nasıl göründüğünü görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b550-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="4b550-180">Webmatrix'te sağ *Movies.cshtml* sayfasından seçim yapıp **başlatma tarayıcıda**.</span><span class="sxs-lookup"><span data-stu-id="4b550-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="4b550-181">Tarayıcı sayfası görüntülendiğinde bu sayfayı gibi görünüyor:</span><span class="sxs-lookup"><span data-stu-id="4b550-181">When the browser displays the page, it looks like this page:</span></span>

![Bir düzen kullanılarak oluşturulması filmler sayfası](layouts/_static/image6.png)

<span data-ttu-id="4b550-183">ASP.NET Movies.cshtml sayfasının içeriği birleştirildi  *\_Layout.cshtml* sayfasında sağ nereye `RenderBody` yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="4b550-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="4b550-184">Ve tabi ki  *\_Layout.cshtml* sayfasında başvurular bir *.css* sayfa görünümünü tanımlayan dosyası.</span><span class="sxs-lookup"><span data-stu-id="4b550-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="4b550-185">AddMovie sayfa düzeni kullanmak için güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="4b550-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="4b550-186">Düzenleri gerçek faydası, onları tüm sayfalar için sitenizdeki sağlamasıdır.</span><span class="sxs-lookup"><span data-stu-id="4b550-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="4b550-187">Açık *AddMovie.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="4b550-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="4b550-188">Unutmamanız *AddMovie.cshtml* sayfa başlangıçta olan bazı CSS kuralları doğrulama hatası iletilerinin görünümünü tanımlamak için bunu.</span><span class="sxs-lookup"><span data-stu-id="4b550-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="4b550-189">Sahip olduğu bir *.css* dosya siteniz şimdi için bu kuralların taşıyabilirsiniz *.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="4b550-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="4b550-190">Bunları kaldırmak *AddMovie.cshtml* dosya ve alt kısmına ekleyin *Movies.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="4b550-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="4b550-191">Aşağıdaki kuralları taşıdığınız:</span><span class="sxs-lookup"><span data-stu-id="4b550-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="4b550-192">Şimdi değişiklikleri aynı tür olun *AddMovie.cshtml* yaptığınız *Movies.cshtml* — eklemek `Layout="~/_Layout.cshtml;` ve şimdi yabancı HTML biçimlendirme kaldırın.</span><span class="sxs-lookup"><span data-stu-id="4b550-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="4b550-193">Değişiklik `<h1>` öğesine `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="4b550-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="4b550-194">İşiniz bittiğinde, sayfa bu örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="4b550-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="4b550-195">Sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4b550-195">Run the page.</span></span> <span data-ttu-id="4b550-196">Şimdi bu çizim gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="4b550-196">Now it looks like this illustration:</span></span>

![Bir düzen kullanılarak oluşturulması 'Filmler Ekle' sayfası](layouts/_static/image7.png)

<span data-ttu-id="4b550-198">Site sayfalarına benzer değişiklikler yapmak istediğiniz — *EditMovie.cshtml* ve *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4b550-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="4b550-199">Ancak, bunu yapmadan önce biraz daha esnek hale getirir düzenini başka bir değişiklik yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b550-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="4b550-200">Düzen sayfası geçirme başlık bilgileri</span><span class="sxs-lookup"><span data-stu-id="4b550-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="4b550-201"> *\_Layout.cshtml* oluşturduğunuz sayfasına sahip bir `<title>` "Film Sitem" ayarlamak öğesi.</span><span class="sxs-lookup"><span data-stu-id="4b550-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="4b550-202">Çoğu tarayıcılar bu öğenin içeriğini bir sekmede metin olarak görüntüle:</span><span class="sxs-lookup"><span data-stu-id="4b550-202">Most browsers display the content of this element as the text on a tab:</span></span>

![Sayfanın &lt;başlık&gt; bir tarayıcı sekmesinde görüntülenen öğesi](layouts/_static/image8.png)

<span data-ttu-id="4b550-204">Bu başlık bilgileri geneldir.</span><span class="sxs-lookup"><span data-stu-id="4b550-204">This title information is generic.</span></span> <span data-ttu-id="4b550-205">Geçerli sayfa için daha belirgin olması için başlık metni istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="4b550-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="4b550-206">(Başlık metni arama motorları tarafından da sayfanızı hakkındadır belirlemek için kullanılır.) Bir içerik sayfasındaki gibi bilgileri geçirebilirsiniz *Movies.cshtml* veya *AddMovie.cshtml* Düzen sayfasını ve sonra kullanmak için Düzen sayfası özelleştirmek için bu bilgileri işler.</span><span class="sxs-lookup"><span data-stu-id="4b550-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="4b550-207">Açık *Movies.cshtml* yeniden sayfa.</span><span class="sxs-lookup"><span data-stu-id="4b550-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="4b550-208">Üst kod içinde aşağıdaki satırı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4b550-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="4b550-209">`Page` Nesnenin kullanılabilir tüm *.cshtml* sayfaları ve bu amaçla, yani olan bir sayfanın düzenini arasında bilgi paylaşmak için.</span><span class="sxs-lookup"><span data-stu-id="4b550-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="4b550-210">Açık<em>\_Layout.cshtml</em> sayfası.</span><span class="sxs-lookup"><span data-stu-id="4b550-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="4b550-211">Değişiklik `<title>` öğesi, BT'nin bu biçimlendirme gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="4b550-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="4b550-212">Bu kod içinde ne olursa olsun işler `Page.Title` özellik sayfası bu konumda sağ.</span><span class="sxs-lookup"><span data-stu-id="4b550-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="4b550-213">Çalıştırma *Movies.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="4b550-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="4b550-214">Bu zaman tarayıcı sekmesinde gösterir değeri olarak geçirilen `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="4b550-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Dinamik olarak oluşturulan başlık gösteren bir tarayıcı sekmesi](layouts/_static/image9.png)

<span data-ttu-id="4b550-216">İsterseniz, sayfa kaynağı tarayıcıda görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="4b550-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="4b550-217">Görebilirsiniz `<title>` öğesi olarak oluşturulur `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="4b550-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="4b550-218">**The Page Object**</span><span class="sxs-lookup"><span data-stu-id="4b550-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="4b550-219">Kullanışlı bir özelliği `Page` bir dinamik Nesne olmasıdır — `Title` özelliği bir sabit ya da ayrılmış adı değil.</span><span class="sxs-lookup"><span data-stu-id="4b550-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="4b550-220">Kullanabileceğiniz *herhangi* değerini adı `Page` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="4b550-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="4b550-221">Örneğin, kolayca başlığı adlı bir özellik kullanarak geçtiğiniz `Page.CurrentName` veya `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="4b550-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="4b550-222">Yalnızca kısıtlama adı hangi özelliklerin adlı normal kuralları izlemek sahip olur.</span><span class="sxs-lookup"><span data-stu-id="4b550-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="4b550-223">(Örneğin, ad boşluk içeremez.)</span><span class="sxs-lookup"><span data-stu-id="4b550-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="4b550-224">Herhangi bir sayıda değerleri kullanarak geçirebilirsiniz `Page` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="4b550-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="4b550-225">Düzen sayfası film bilgileri geçirmek istiyorsanız, aşağıdakine benzer kullanarak değerleri geçirebilirdiniz `Page.MovieTitle` ve `Page.Genre` ve `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="4b550-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="4b550-226">(Veya bilgileri depolamak için geliştirmiştir herhangi bir ad.) Tek gereksinim — büyük olasılıkla belirgin olduğu — içerik sayfasını ve düzen sayfası aynı adı kullanmak zorunda değil.</span><span class="sxs-lookup"><span data-stu-id="4b550-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="4b550-227">Geçirdiğiniz kullanarak bilgi `Page` nesne düzeni sayfasında görüntülenecek yalnızca metin ile sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="4b550-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="4b550-228">Düzen sayfası için bir değer geçirebilirsiniz ve düzen sayfası kodda sayfanın bölümünü görüntülemek karar vermek için değeri daha sonra kullanabilirsiniz ne *.css* kullanmak için dosya ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="4b550-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="4b550-229">Geçirdiğiniz değerleri `Page` nesne olan diğer değerleri gibi kullandığınız kodu.</span><span class="sxs-lookup"><span data-stu-id="4b550-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="4b550-230">Yalnızca değerleri içerik sayfasındaki kaynaklanan ve düzen sayfası geçirilen değil.</span><span class="sxs-lookup"><span data-stu-id="4b550-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="4b550-231">Açık *AddMovie.cshtml* sayfasında ve bir satır için bir başlık sağlar kod üstüne ekleyin *AddMovie.cshtml* sayfa:</span><span class="sxs-lookup"><span data-stu-id="4b550-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="4b550-232">Çalıştırma *AddMovie.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="4b550-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="4b550-233">Yeni başlık bakın:</span><span class="sxs-lookup"><span data-stu-id="4b550-233">You see the new title there:</span></span>

![Dinamik olarak oluşturulan 'Filmler Ekle' başlık gösteren bir tarayıcı sekmesi](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="4b550-235">Düzeni kullanmak için kalan sayfalarını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="4b550-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="4b550-236">Şimdi yeni düzeni kullanmasını sağlayarak sitenizdeki kalan sayfalarını bitirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b550-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="4b550-237">Açık *EditMovie.cshtml* ve *DeleteMovie.cshtml* içinde açın ve her birini aynı değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="4b550-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="4b550-238">Düzen sayfası bağlantıları kod satırını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4b550-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="4b550-239">Sayfanın başlığı ayarlamak için bir satır ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4b550-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="4b550-240">veya:</span><span class="sxs-lookup"><span data-stu-id="4b550-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="4b550-241">Tüm gereksiz HTML biçimlendirmesi Kaldır — temel olarak, yalnızca içinde olduğunda BITS bırakın `<body>` öğesi (artı üst kod bloğu).</span><span class="sxs-lookup"><span data-stu-id="4b550-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="4b550-242">Değişiklik `<h1>` olmasını öğesi bir `<h2>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="4b550-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="4b550-243">Bu değişiklikler yaptığınızda, her test edin ve düzgün bir şekilde görüntüleme ve başlık doğru olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="4b550-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="4b550-244">Herhangi düşüncelerinizi Düzen sayfaları hakkında</span><span class="sxs-lookup"><span data-stu-id="4b550-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="4b550-245">Bu öğreticide oluşturduğunuz bir  *\_Layout.cshtml* sayfasında ve kullanılan `RenderBody` içeriği başka bir sayfadan birleştirmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="4b550-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="4b550-246">Web sayfalarında düzenleri kullanmak için temel düzeni olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="4b550-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="4b550-247">Düzen sayfaları biz burada kapak kaydetmedi ek özellikler vardır.</span><span class="sxs-lookup"><span data-stu-id="4b550-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="4b550-248">Örneğin, Düzen sayfaları geçirebilmenize — bir düzen sayfası sırayla başvuru başka.</span><span class="sxs-lookup"><span data-stu-id="4b550-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="4b550-249">İç içe geçmiş düzenleri farklı düzenleri gerektiren alt site ile çalışıyorsanız yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4b550-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="4b550-250">Ek yöntemleri de kullanabilirsiniz (örneğin, `RenderSection`) ayarlamak için Düzen sayfası bölümlerde adlı.</span><span class="sxs-lookup"><span data-stu-id="4b550-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="4b550-251">Birleşimi, Düzen sayfaları ve *.css* dosyaları güçlü.</span><span class="sxs-lookup"><span data-stu-id="4b550-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="4b550-252">Webmatrix'te sonraki öğretici serisinde anlatıldığı gibi temel bir site oluşturabilirsiniz bir *şablonu*, hangi size sahip bir site önceden oluşturulmuş işlevindeki.</span><span class="sxs-lookup"><span data-stu-id="4b550-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="4b550-253">Şablonlar, Düzen sayfaları ve CSS harika arayın ve menüleri gibi özellikleri olan siteler oluşturmak için iyi kullanılmasını sağlamak.</span><span class="sxs-lookup"><span data-stu-id="4b550-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="4b550-254">Aşağıda, Düzen sayfaları ve CSS kullanan özellikler gösteren bir şablonu temel alan bir siteden giriş sayfasının ekran görüntüsü verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4b550-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Üstbilgi, gezinti alanını, içerik alanının, isteğe bağlı bir bölüm ve oturum açma bağlantıları gösteren WebMatrix site şablonu tarafından oluşturulan düzeni](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="4b550-256">Tam listesi için (bir düzen sayfasını kullanmak üzere güncelleştirilir) film sayfası</span><span class="sxs-lookup"><span data-stu-id="4b550-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="4b550-257">Tam sayfa için listeleme (düzeni için güncelleştirilmiş) film sayfasına ekleyin</span><span class="sxs-lookup"><span data-stu-id="4b550-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="4b550-258">Delete film sayfa (düzeni için güncelleştirilmiş) için tam sayfa listeleme</span><span class="sxs-lookup"><span data-stu-id="4b550-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="4b550-259">Düzen film sayfa (düzeni için güncelleştirilmiş) için tam sayfa listeleme</span><span class="sxs-lookup"><span data-stu-id="4b550-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="4b550-260">Sıradaki gelen</span><span class="sxs-lookup"><span data-stu-id="4b550-260">Coming Up Next</span></span>

<span data-ttu-id="4b550-261">Sonraki öğreticide herkes görebilmeniz için Internet'e sitenizi yayımlayın öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="4b550-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4b550-262">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4b550-262">Additional Resources</span></span>

- <span data-ttu-id="4b550-263">[Tutarlı görünecek oluşturma](https://go.microsoft.com/fwlink/?LinkID=202891) — bir makale düzenleri ile çalışma hakkında bazı daha fazla ayrıntı sağlar.</span><span class="sxs-lookup"><span data-stu-id="4b550-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="4b550-264">Ayrıca, gösterir veya gizler içeriğin bir kısmı bir düzen sayfası için bir değer geçirmek nasıl açıklanır.</span><span class="sxs-lookup"><span data-stu-id="4b550-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="4b550-265">[İç içe, Düzen sayfaları Razor ile](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — CAN Brind bloglar Düzen sayfaları yerleştirmek nasıl bir örneği.</span><span class="sxs-lookup"><span data-stu-id="4b550-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="4b550-266">(Sayfaları yüklenmesini kapsar.)</span><span class="sxs-lookup"><span data-stu-id="4b550-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4b550-267">[Önceki](deleting-data.md)
> [sonraki](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="4b550-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
