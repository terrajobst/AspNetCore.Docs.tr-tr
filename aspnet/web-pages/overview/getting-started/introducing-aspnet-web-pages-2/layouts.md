---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Tutarlı bir düzen oluşturma - ASP.NET Web sayfaları ile tanışın | Microsoft Docs
author: tfitzmac
description: Bu öğreticide, ASP.NET Web sayfaları kullanan bir site sayfalar için tutarlı bir görünüm oluşturmak için düzenlerini kullanmayı gösterir. Tamamladığınızdan varsayar...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 8f3d9e8a6f6a0179224e18faf11db3dc1510a095
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755112"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="b257c-104">ASP.NET Web sayfalarına giriş - tutarlı bir düzen oluşturma</span><span class="sxs-lookup"><span data-stu-id="b257c-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="b257c-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b257c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b257c-106">Bu öğreticide nasıl kullanılacağını gösterir *düzenleri* kullanan ASP.NET Web sayfaları sitesinde sayfalar için tutarlı bir görünüm oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="b257c-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="b257c-107">Bu seriyi aracılığıyla bitirdiğinizi [veritabanı verilerini silme ASP.NET Web Pages'de](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="b257c-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="b257c-108">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="b257c-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b257c-109">Bir düzen sayfası ' dir.</span><span class="sxs-lookup"><span data-stu-id="b257c-109">What a layout page is.</span></span>
> - <span data-ttu-id="b257c-110">Yerleşim sayfaları dinamik içerik ile bir araya getirilebileceğini öğrenin.</span><span class="sxs-lookup"><span data-stu-id="b257c-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="b257c-111">Bir düzen sayfasına değerleri geçirmek nasıl.</span><span class="sxs-lookup"><span data-stu-id="b257c-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="b257c-112">Düzenler hakkında</span><span class="sxs-lookup"><span data-stu-id="b257c-112">About Layouts</span></span>

<span data-ttu-id="b257c-113">Şu ana kadar oluşturduğunuz sayfaları tüm tam tek başına sayfa.</span><span class="sxs-lookup"><span data-stu-id="b257c-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="b257c-114">Bunların tümü aynı siteye ait, ancak ortak öğeleri veya standart bir görünüm yok.</span><span class="sxs-lookup"><span data-stu-id="b257c-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="b257c-115">Çoğu siteleri bir tutarlı bir görünüm ve Düzen sahip.</span><span class="sxs-lookup"><span data-stu-id="b257c-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="b257c-116">Örneğin, Git, [Microsoft.com/web](https://www.microsoft.com/web/) site ve araştırın, tüm sayfaların genel bir düzeni ve görsel temasını uygun olacağını göreceksiniz:</span><span class="sxs-lookup"><span data-stu-id="b257c-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Üst bilgi, gezinti alanına, içerik alanı ve alt bilgi düzenini gösteren Microsoft.com/web site sayfası](layouts/_static/image1.png)

<span data-ttu-id="b257c-118">Bir *verimsiz* üst bilgi, gezinti çubuğunda ve altbilgi her sayfalarınızın ayrı olarak tanımlamak için bu düzeni oluşturmak için bir yol olur.</span><span class="sxs-lookup"><span data-stu-id="b257c-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="b257c-119">Her zaman aynı biçimlendirme çoğaltma.</span><span class="sxs-lookup"><span data-stu-id="b257c-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="b257c-120">Bir şeyle değiştirmek istiyorsanız (örneğin, alt bilgi güncelleştirme) her sayfaya ayrı olarak değiştirmeniz gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="b257c-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="b257c-121">Olduğu yer *Düzen sayfaları* vardır.</span><span class="sxs-lookup"><span data-stu-id="b257c-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="b257c-122">ASP.NET Web sayfaları'nda, sitenizdeki sayfaları için genel bir kapsayıcı sağlayan bir düzen sayfası tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b257c-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="b257c-123">Örneğin, düzen sayfası, üst bilgi, gezinti alanı ve alt bilgi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b257c-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="b257c-124">Düzen sayfası ana içeriğin nereye gideceğini yer tutucu içerir.</span><span class="sxs-lookup"><span data-stu-id="b257c-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="b257c-125">Ardından, biçimlendirme ve yalnızca o sayfanın kodu içeren her bir içerik sayfayı tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b257c-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="b257c-126">İçerik sayfaları tam HTML sayfalarını olmak zorunda değildir; Bunlar bile sahip olmak zorunda değildir bir `<body>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="b257c-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="b257c-127">Ayrıca bir ASP hangi düzen sayfası içeriği görüntülemek istediğiniz kod satırının sahiptirler.</span><span class="sxs-lookup"><span data-stu-id="b257c-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="b257c-128">Kabaca bu ilişkiyi nasıl çalıştığını gösteren resim şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="b257c-128">Here's a picture that shows roughly how this relationship works:</span></span>

![İki içerik sayfaları ve içine sığması bir düzen sayfası gösteren kavramsal diyagram](layouts/_static/image2.png)

<span data-ttu-id="b257c-130">Bu etkileşimi nasıl çalıştığını görün anlamak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="b257c-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="b257c-131">Bu öğreticide, bir düzeni kullanmak için filmler sayfalarınızı değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b257c-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="b257c-132">Bir düzen sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="b257c-132">Adding a Layout Page</span></span>

<span data-ttu-id="b257c-133">Bir üstbilgi, altbilgi ve ana içerik için bir alan tipik sayfa düzeniyle tanımlayan bir düzen sayfası oluşturarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="b257c-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="b257c-134">WebPagesMovies sitede adlı CSHTML sayfa ekleme  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b257c-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="b257c-135">Başındaki altçizgiyi ( `_` ) karakteri önemlidir.</span><span class="sxs-lookup"><span data-stu-id="b257c-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="b257c-136">Bir sayfanın adı bir alt çizgiyle başlıyorsa ASP.NET bu sayfayı doğrudan tarayıcıya göndermez.</span><span class="sxs-lookup"><span data-stu-id="b257c-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="b257c-137">Bu kural, ancak bu kullanıcıları doğrudan istek işleyememelidir siteniz için gerekli olan sayfa tanımlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="b257c-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="b257c-138">Sayfa içeriğini aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="b257c-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="b257c-139">Gördüğünüz gibi bu işaretleme kullanan yalnızca HTML'dir `<div>` sayfa ayrıca bir üç bölüm daha tanımlamak için öğeleri `<div>` üç bölüm tutmak için öğesi.</span><span class="sxs-lookup"><span data-stu-id="b257c-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="b257c-140">Altbilgi Razor kodunun bir bit içerir: `@DateTime.Now.Year`, hangi işleme geçerli yıl sayfasında bu konumdaki.</span><span class="sxs-lookup"><span data-stu-id="b257c-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="b257c-141">Adlı bir stil sayfası bağlantısı olduğunu fark *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="b257c-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="b257c-142">Öğeleri fiziksel düzenini ayrıntılarını tanımlanacağı stil sayfası alınır.</span><span class="sxs-lookup"><span data-stu-id="b257c-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="b257c-143">Bir dakika içinde oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b257c-143">You'll create that in a moment.</span></span>

<span data-ttu-id="b257c-144">Bu yalnızca olağan dışı özellik  *\_Layout.cshtml* sayfasıdır `@Render.Body()` satır.</span><span class="sxs-lookup"><span data-stu-id="b257c-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="b257c-145">Bu düzen, başka bir sayfa ile birleştirildiğinde içeriği gidecekleri yer tutucu olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="b257c-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="b257c-146">.Css dosyasını ekleme</span><span class="sxs-lookup"><span data-stu-id="b257c-146">Adding a .css File</span></span>

<span data-ttu-id="b257c-147">Sayfada öğeleri gerçek yerleşimini (diğer bir deyişle, Görünüm) tanımlamak için tercih edilen yoludur, geçişli stil sayfası (CSS) kurallarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="b257c-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="b257c-148">Oluşturacağınız şekilde bir *.css* kuralları, yeni düzene sahip bir dosya.</span><span class="sxs-lookup"><span data-stu-id="b257c-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="b257c-149">Webmatrix'te, sitenizin kök seçin.</span><span class="sxs-lookup"><span data-stu-id="b257c-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="b257c-150">Ardından **dosyaları** sekmesi altında Şerit, oku **yeni** düğmesine ve ardından **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="b257c-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![Şeritteki yeni 'Yeni Klasör' seçeneği.](layouts/_static/image3.png)

<span data-ttu-id="b257c-152">Yeni klasör adı *stilleri*.</span><span class="sxs-lookup"><span data-stu-id="b257c-152">Name the new folder *Styles*.</span></span>

![Yeni Klasör 'Stilleri' adlandırma](layouts/_static/image4.png)

<span data-ttu-id="b257c-154">İçinde yeni *stilleri* klasöründe adlı bir dosya oluşturun *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="b257c-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Yeni bir Movies.css dosyası oluşturma](layouts/_static/image5.png)

<span data-ttu-id="b257c-156">Yeni içeriklerini *.css* aşağıdaki dosya:</span><span class="sxs-lookup"><span data-stu-id="b257c-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="b257c-157">İki şey not üzere dışında bu CSS kurallarını hakkında pek fazla dediğimiz olmaz.</span><span class="sxs-lookup"><span data-stu-id="b257c-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="b257c-158">Yazı tiplerine ve boyutlarına ayarlamanın yanı sıra kuralları mutlak konumlandırma üstbilgi, altbilgi ve ana içerik alanı konumu'kurmak için kullandığınız paroladır.</span><span class="sxs-lookup"><span data-stu-id="b257c-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="b257c-159">Edinebilirsiniz, CSS konumlandırma yeni tanışıyorsanız, [CSS konumlandırma](http://www.w3schools.com/css/css_positioning.asp) W3Schools sitesindeki öğretici.</span><span class="sxs-lookup"><span data-stu-id="b257c-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="b257c-160">En altında ki özgün olarak olan Stil kurallarının kopyalamış olduğunuz tanımlanmış tek tek de dikkat edilecek diğer şey *Movies.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="b257c-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="b257c-161">Bu kurallar de kullanılan [veri görüntüleme ile ASP.NET Web sayfaları kullanarak giriş](https://go.microsoft.com/fwlink/?LinkId=251580) yapmak için öğreticiyi `WebGrid` yardımcı işleme şeritler tabloya eklenen biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="b257c-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="b257c-162">(Kullanmak için kullanacaksanız bir *.css* dosya Stil tanımları için de Stil kurallarının tüm sitenin içinde yerleştirdiğiniz.)</span><span class="sxs-lookup"><span data-stu-id="b257c-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="b257c-163">Bu düzeni kullanmak için filmler dosyası güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="b257c-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="b257c-164">Artık sitenizin yeni düzeni kullanmak için mevcut dosyaları güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b257c-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="b257c-165">Açık *Movies.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="b257c-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="b257c-166">En üstünde, kodun ilk satırı aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b257c-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="b257c-167">Sayfa artık şu şekilde başlar:</span><span class="sxs-lookup"><span data-stu-id="b257c-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="b257c-168">Bu tek satırlık bir kod ASP olduğunda *filmler* sayfa çalıştırılırsa ile birleştirilmesini  *\_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="b257c-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="b257c-169">Bu yana *Movies.cshtml* dosyası, bir yerleşim sayfası artık kullanır, biçimlendirmeden kaldırabilirsiniz *Movies.cshtml* tarafından dikkate sayfa  *\_Layout.cshtml*dosya.</span><span class="sxs-lookup"><span data-stu-id="b257c-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="b257c-170">Çıkın `<!DOCTYPE>`, `<html>`, ve `<body>` açılış ve kapanış etiketlerinin.</span><span class="sxs-lookup"><span data-stu-id="b257c-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="b257c-171">Tüm Al `<head>` öğesi ve artık bu kurallar kendinizi ComUnregisterFunction kılavuz stili kurallarını içerir, içeriğinin bir *.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="b257c-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="b257c-172">Mevcut olduğundan iken değiştirmek `<h1>` öğesine bir `<h2>` öğesi; olan bir `<h1>` düzen sayfası zaten öğesinde.</span><span class="sxs-lookup"><span data-stu-id="b257c-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="b257c-173">Değişiklik `<h2>` "Listesi filmler" metni.</span><span class="sxs-lookup"><span data-stu-id="b257c-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="b257c-174">Normalde bir içerik sayfasında bu tür değişiklikler yapmak zorunda mıydı.</span><span class="sxs-lookup"><span data-stu-id="b257c-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="b257c-175">Olan düzen sayfası başlattığınız siteniz olduğunda, bu öğeler olmadan içerik sayfalarını başlangıç olarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b257c-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="b257c-176">Bu durumda, yine de, bir tek başına sayfa biraz temizlik, bu nedenle, bir düzeni kullanan bir dönüştürüyoruz.</span><span class="sxs-lookup"><span data-stu-id="b257c-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="b257c-177">İşiniz bittiğinde, *Movies.cshtml* sayfasında, aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="b257c-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="b257c-178">Test düzeni</span><span class="sxs-lookup"><span data-stu-id="b257c-178">Testing the Layout</span></span>

<span data-ttu-id="b257c-179">Artık düzenini nasıl göründüğünü görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b257c-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="b257c-180">Webmatrix'te, sağ *Movies.cshtml* sayfasından seçim yapıp **tarayıcıda Başlat**.</span><span class="sxs-lookup"><span data-stu-id="b257c-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="b257c-181">Tarayıcı sayfası görüntülendiğinde, bu sayfada gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="b257c-181">When the browser displays the page, it looks like this page:</span></span>

![Bir düzen kullanılarak oluşturulması filmler sayfası](layouts/_static/image6.png)

<span data-ttu-id="b257c-183">ASP.NET Movies.cshtml sayfanın içeriğini birleştirildi  *\_Layout.cshtml* sayfasında doğru nerede `RenderBody` yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="b257c-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="b257c-184">Ve Elbette  *\_Layout.cshtml* sayfasında başvurular bir *.css* sayfa görünümünü tanımlayan dosya.</span><span class="sxs-lookup"><span data-stu-id="b257c-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="b257c-185">AddMovie sayfa düzeni kullanmak için güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="b257c-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="b257c-186">Düzenleri gerçek avantajı, bunları tüm sayfalar için sitenizde sağlamasıdır.</span><span class="sxs-lookup"><span data-stu-id="b257c-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="b257c-187">Açık *AddMovie.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="b257c-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="b257c-188">Unutmayın *AddMovie.cshtml* sayfası ilk olan bazı CSS kurallarını doğrulama hatası iletilerinin görünümünü tanımlamak için bunu.</span><span class="sxs-lookup"><span data-stu-id="b257c-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="b257c-189">Elinizde bu yana bir *.css* dosya sitenizin artık bu kuralları taşıyabilirsiniz *.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="b257c-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="b257c-190">Kaldırabilir *AddMovie.cshtml* dosya ve alt kısmına ekleyin *Movies.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="b257c-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="b257c-191">Aşağıdaki kurallar taşıdığınız:</span><span class="sxs-lookup"><span data-stu-id="b257c-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="b257c-192">Şimdi değişiklikleri aynı tür hale *AddMovie.cshtml* yaptığınız *Movies.cshtml* — ekleme `Layout="~/_Layout.cshtml;` ve artık fazlalık HTML biçimlendirmeyi kaldırın.</span><span class="sxs-lookup"><span data-stu-id="b257c-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="b257c-193">Değişiklik `<h1>` öğesine `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="b257c-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="b257c-194">İşiniz bittiğinde, sayfa şu örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="b257c-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="b257c-195">Sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b257c-195">Run the page.</span></span> <span data-ttu-id="b257c-196">Artık bu çizim gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="b257c-196">Now it looks like this illustration:</span></span>

![Bir düzen kullanılarak oluşturulması Sayfası 'Filmler Ekle'](layouts/_static/image7.png)

<span data-ttu-id="b257c-198">Site sayfaları benzer değişiklikler yapmak istediğiniz — *EditMovie.cshtml* ve *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b257c-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="b257c-199">Ancak, bunu yapmadan önce biraz daha esnek yapan düzene başka bir değişiklik yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b257c-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="b257c-200">Düzen Sayfası başlık bilgilerini geçirme</span><span class="sxs-lookup"><span data-stu-id="b257c-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="b257c-201">*\_Layout.cshtml* oluşturduğunuz sayfasına sahip bir `<title>` "My film sitesine" öğesi.</span><span class="sxs-lookup"><span data-stu-id="b257c-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="b257c-202">Çoğu tarayıcısı bu öğenin içeriğini bir sekme üzerindeki metin olarak görüntüle:</span><span class="sxs-lookup"><span data-stu-id="b257c-202">Most browsers display the content of this element as the text on a tab:</span></span>

![Sayfanın &lt;başlık&gt; bir tarayıcı sekmesinde görüntülenen öğe](layouts/_static/image8.png)

<span data-ttu-id="b257c-204">Bu başlık bilgilerini geneldir.</span><span class="sxs-lookup"><span data-stu-id="b257c-204">This title information is generic.</span></span> <span data-ttu-id="b257c-205">Başlık metni geçerli sayfa için ayrıntılı olmasını istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="b257c-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="b257c-206">(Başlık metni arama motorları tarafından da sayfanızı ne hakkında olduğunu belirlemek için kullanılır.) İçerik sayfasından gibi bilgileri geçirebilirsiniz *Movies.cshtml* veya *AddMovie.cshtml* Düzen sayfasını ve ardından Düzen sayfası özelleştirmek için bu bilgileri işler.</span><span class="sxs-lookup"><span data-stu-id="b257c-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="b257c-207">Açık *Movies.cshtml* yeniden sayfa.</span><span class="sxs-lookup"><span data-stu-id="b257c-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="b257c-208">Üst kod içinde aşağıdaki satırı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b257c-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="b257c-209">`Page` Nesnedir tüm kullanılabilir *.cshtml* sayfaları ve bu amaçla, yani ise bir sayfa ve powerapps'in düzen arasında bilgi paylaşımı için.</span><span class="sxs-lookup"><span data-stu-id="b257c-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="b257c-210">Açık<em>\_Layout.cshtml</em> sayfası.</span><span class="sxs-lookup"><span data-stu-id="b257c-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="b257c-211">Değişiklik `<title>` öğesi olan bu biçimlendirme gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="b257c-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="b257c-212">Bu kod içinde ne olursa olsun işler `Page.Title` özelliği sayfasında bu konumdaki sağ.</span><span class="sxs-lookup"><span data-stu-id="b257c-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="b257c-213">Çalıştırma *Movies.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="b257c-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="b257c-214">Bu süre tarayıcı sekmesini gösteren değeri olarak geçirilen `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="b257c-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Dinamik olarak oluşturulan bir başlık gösteren bir tarayıcı sekmesi](layouts/_static/image9.png)

<span data-ttu-id="b257c-216">İsterseniz, sayfa kaynağı tarayıcıda görüntüleme.</span><span class="sxs-lookup"><span data-stu-id="b257c-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="b257c-217">Gördüğünüz gibi `<title>` öğesi olarak işlenen `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="b257c-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="b257c-218">**Sayfa nesnesi**</span><span class="sxs-lookup"><span data-stu-id="b257c-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="b257c-219">Kullanışlı bir özelliği `Page` dinamik Nesne olmasıdır; `Title` özellik sabit veya ayrılmış bir ad değil.</span><span class="sxs-lookup"><span data-stu-id="b257c-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="b257c-220">Kullanabileceğiniz *herhangi* değerini adı `Page` nesne.</span><span class="sxs-lookup"><span data-stu-id="b257c-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="b257c-221">Örneğin, kolayca başlık adlı bir özellik kullanarak başarılı `Page.CurrentName` veya `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="b257c-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="b257c-222">Tek kısıtlama adı için hangi özelliklerin adlı normal kurallara uymak sahip olur.</span><span class="sxs-lookup"><span data-stu-id="b257c-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="b257c-223">(Örneğin, ad boşluk içeremez.)</span><span class="sxs-lookup"><span data-stu-id="b257c-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="b257c-224">Herhangi bir sayıda değerleri kullanarak geçirebilirsiniz `Page` nesne.</span><span class="sxs-lookup"><span data-stu-id="b257c-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="b257c-225">Düzen sayfasına film bilgi geçirmek istiyorsanız, aşağıdaki gibi kullanarak değerleri geçirebiliriz `Page.MovieTitle` ve `Page.Genre` ve `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="b257c-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="b257c-226">(Veya bilgileri depolamak için geliştirilen diğer adlar.) Tek gereksinim — büyük olasılıkla açık olduğu — içerik sayfası ve düzen sayfası aynı adları kullanılacak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="b257c-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="b257c-227">Geçirdiğiniz kullanarak bilgi `Page` nesne düzeni sayfasında görüntülenecek yalnızca metin sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="b257c-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="b257c-228">Düzen sayfası için bir değer geçirebilirsiniz ve düzen sayfası kod sayfası bir bölümünü görüntülemek karar vermek için değeri ardından kullanabilirsiniz ne *.css* kullanılacak dosya ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="b257c-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="b257c-229">Geçirdiğiniz değerleri `Page` nesne olan diğer değerleri gibi kullandığınız kod.</span><span class="sxs-lookup"><span data-stu-id="b257c-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="b257c-230">Yalnızca değerleri içerik sayfasındaki kaynaklanan ve Düzen sayfasına geçirilir olduğu.</span><span class="sxs-lookup"><span data-stu-id="b257c-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="b257c-231">Açık *AddMovie.cshtml* sayfa ve bir satır için bir başlık sağladığı kodunu en üstüne ekleyin *AddMovie.cshtml* sayfası:</span><span class="sxs-lookup"><span data-stu-id="b257c-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="b257c-232">Çalıştırma *AddMovie.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="b257c-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="b257c-233">Yeni başlık görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="b257c-233">You see the new title there:</span></span>

![Dinamik olarak oluşturulan Ekle'filmler ' başlık gösteren bir tarayıcı sekmesi](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="b257c-235">Bu düzeni kullanmak için geri kalan sayfalarını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="b257c-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="b257c-236">Şimdi yeni düzene kullanmasını sağlayarak sitenizdeki kalan sayfalarını tamamlayabilir.</span><span class="sxs-lookup"><span data-stu-id="b257c-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="b257c-237">Açık *EditMovie.cshtml* ve *DeleteMovie.cshtml* içinde açın ve her aynı değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="b257c-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="b257c-238">Düzen sayfasına bağlayan bir kod satırı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b257c-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="b257c-239">Sayfanın başlığını ayarlamak için bir satır ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b257c-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="b257c-240">veya:</span><span class="sxs-lookup"><span data-stu-id="b257c-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="b257c-241">Tüm gereksiz HTML biçimlendirmeyi kaldırmak — aslında içinde olan BITS bırakın `<body>` öğesi (Ayrıca üst kod bloğu).</span><span class="sxs-lookup"><span data-stu-id="b257c-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="b257c-242">Değişiklik `<h1>` olarak öğeyi bir `<h2>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="b257c-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="b257c-243">Bu değişiklikler yaptınız, her test edin ve düzgün şekilde görüntülenmesini ve başlık doğru olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="b257c-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="b257c-244">Yerleşim sayfaları hakkında fikirlerinizi herhangi</span><span class="sxs-lookup"><span data-stu-id="b257c-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="b257c-245">Bu öğreticide oluşturduğunuz bir  *\_Layout.cshtml* sayfasında ve kullanılan `RenderBody` içeriği başka bir sayfadan birleştirmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b257c-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="b257c-246">Düzenleri kullanarak Web sayfaları için temel düzeni olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="b257c-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="b257c-247">Yerleşim sayfaları, burada ele yaramadı ek özellikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="b257c-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="b257c-248">Örneğin, Düzen sayfaları yuvalayabilirsiniz; bir düzen sayfası sırayla başvurabilir başka.</span><span class="sxs-lookup"><span data-stu-id="b257c-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="b257c-249">İç içe geçmiş düzenleri alan farklı düzenler gerektiren bir site bölümlerini ile çalışıyorsanız yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="b257c-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="b257c-250">Ek yöntemleri de kullanabilirsiniz (örneğin, `RenderSection`) adlı düzen sayfası olarak bölümlerde ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="b257c-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="b257c-251">Birleşimi, Düzen sayfaları ve *.css* dosyaları güçlü.</span><span class="sxs-lookup"><span data-stu-id="b257c-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="b257c-252">Webmatrix'te sonraki öğretici serisinde anlatıldığı gibi temel bir site oluşturabilirsiniz bir *şablon*, size sağlayan olan bir siteyi önceden oluşturulmuş işlevindeki.</span><span class="sxs-lookup"><span data-stu-id="b257c-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="b257c-253">Şablonlar, Düzen sayfaları ve CSS, harika görünen ve menüler gibi özellikleri olan siteleri oluşturmak için iyi kullanılmasını sağlamak.</span><span class="sxs-lookup"><span data-stu-id="b257c-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="b257c-254">Düzen sayfaları ve CSS özelliklerini gösteren bir şablonu temel alan bir siteden giriş sayfasının ekran görüntüsü aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b257c-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Üst bilgi, gezinti alanına, içerik alanının, isteğe bağlı bir bölüm ve oturum açma bağlantılar gösteren WebMatrix site şablonu tarafından oluşturulan düzeni](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="b257c-256">Tam listesi için (bir düzen sayfası kullanmak için güncelleştirilmiş) film sayfası</span><span class="sxs-lookup"><span data-stu-id="b257c-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="b257c-257">Tam sayfa için listeleme (düzeni için güncelleştirilmiş) film sayfaya ekleyin</span><span class="sxs-lookup"><span data-stu-id="b257c-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="b257c-258">Tam silme film sayfası (düzeni için güncelleştirilmiş) için sayfa listesi</span><span class="sxs-lookup"><span data-stu-id="b257c-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="b257c-259">Tam sayfa listesi için düzenleme film sayfası (düzeni için güncelleştirilmiş)</span><span class="sxs-lookup"><span data-stu-id="b257c-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="b257c-260">Sıradaki gelen</span><span class="sxs-lookup"><span data-stu-id="b257c-260">Coming Up Next</span></span>

<span data-ttu-id="b257c-261">Sonraki öğreticide, herkesin görebileceği şekilde Internet'e sitenizi yayımlama öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b257c-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b257c-262">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b257c-262">Additional Resources</span></span>

- <span data-ttu-id="b257c-263">[Tutarlı bir ara oluşturma](https://go.microsoft.com/fwlink/?LinkID=202891) — düzeni ile çalışma hakkında daha fazla ayrıntı sağlayan bir makale.</span><span class="sxs-lookup"><span data-stu-id="b257c-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="b257c-264">Ayrıca, görüntüleyen veya gizleyen İçeriklerinden bazılarını bir düzen sayfası için bir değer geçirmek nasıl açıklar.</span><span class="sxs-lookup"><span data-stu-id="b257c-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="b257c-265">[Razor sayfalarıyla Düzen iç içe geçmiş](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogları Düzen sayfaları iç içe ilişkin bir örnek.</span><span class="sxs-lookup"><span data-stu-id="b257c-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="b257c-266">(Bir indirme sayfaları içerir.)</span><span class="sxs-lookup"><span data-stu-id="b257c-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b257c-267">[Önceki](deleting-data.md)
> [İleri](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="b257c-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
