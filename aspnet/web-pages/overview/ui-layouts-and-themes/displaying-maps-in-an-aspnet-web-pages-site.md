---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: MAPS görüntüleyen bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs
author: tfitzmac
description: Bu makalede, Bing, Google, Ma tarafından sağlanan hizmetlerin eşleme dayalı bir ASP.NET Web sayfaları (Razor) Web sayfalarında etkileşimli eşlemeleri görüntülemek açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 608dab8760bad7b877ab6fd4f89b21e980f5b1db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30893868"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="76bac-103">Bir ASP.NET Web sayfaları (Razor) sitesinde eşlemeleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="76bac-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="76bac-104">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="76bac-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="76bac-105">Bu makalede, Bing, Google, MapQuest ve Yahoo tarafından sağlanan hizmetlerin eşleme dayalı bir ASP.NET Web sayfaları (Razor) Web sayfalarında etkileşimli eşlemeleri görüntülemek açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="76bac-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="76bac-106">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="76bac-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="76bac-107">Bir adresini temel alan bir harita oluşturmak nasıl.</span><span class="sxs-lookup"><span data-stu-id="76bac-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="76bac-108">Enlem ve boylam koordinatları temelinde bir harita oluşturmak nasıl.</span><span class="sxs-lookup"><span data-stu-id="76bac-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="76bac-109">Nasıl bir Bing Haritalar Geliştirici hesabını kaydetmek ve Bing Haritalar ile kullanmak için bir anahtar alın.</span><span class="sxs-lookup"><span data-stu-id="76bac-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="76bac-110">Bu makalede sunulan ASP.NET özelliğidir:</span><span class="sxs-lookup"><span data-stu-id="76bac-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="76bac-111">`Maps` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="76bac-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="76bac-112">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="76bac-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="76bac-113">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="76bac-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="76bac-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="76bac-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="76bac-115">Bu öğretici, WebMatrix 3 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="76bac-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="76bac-116">Web sayfalarında, maps bir sayfada kullanarak görüntüleyebileceğiniz `Maps` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="76bac-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="76bac-117">Bir adres veya boylam ve enlem koordinatları kümesi üzerinde alan eşlemeleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="76bac-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="76bac-118">`Maps` Sınıfı Bing, Google, MapQuest ve Yahoo gibi popüler harita motorları çağrı sağlar.</span><span class="sxs-lookup"><span data-stu-id="76bac-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="76bac-119">Bir sayfaya eşleme ekleme adımları hangi harita motorları bağımsız olarak çağrı aynıdır.</span><span class="sxs-lookup"><span data-stu-id="76bac-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="76bac-120">Eşlemeyi görüntülemek için kullanılabilir yöntemleri sağlayan JavaScript dosyası başvuru eklemeniz yeterlidir, ardından yöntemlerini çağıran `Maps` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="76bac-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="76bac-121">Temel bir harita hizmet seçin `Maps` yardımcı yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="76bac-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="76bac-122">Aşağıdakilerden herhangi birini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="76bac-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="76bac-123">Gereksinim duyduğunuz parça yükleme</span><span class="sxs-lookup"><span data-stu-id="76bac-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="76bac-124">MAPS görüntülemek için bu parça gerekir:</span><span class="sxs-lookup"><span data-stu-id="76bac-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="76bac-125">`Maps` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="76bac-125">The `Maps` helper.</span></span> <span data-ttu-id="76bac-126">Bu yardımcı sürüm ASP.NET Web Yardımcıları kitaplığı 2 dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="76bac-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="76bac-127">Kitaplık zaten eklemediyseniz, bir NuGet paketi olarak sitenizdeki yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="76bac-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="76bac-128">Ayrıntılar için bkz [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="76bac-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="76bac-129">(Galerisi'nde arama `microsoft-web-helpers` paket.)</span><span class="sxs-lookup"><span data-stu-id="76bac-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="76bac-130">JQuery kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="76bac-130">The jQuery library.</span></span> <span data-ttu-id="76bac-131">WebMatrix site şablonları çeşitli zaten jQuery kitaplıklarında dahil kendi *betik* klasörler.</span><span class="sxs-lookup"><span data-stu-id="76bac-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="76bac-132">Bu kitaplıklar yoksa, en son jQuery kitaplıktan doğrudan indirebilirsiniz [jQuery.org](http://jQuery.org) site.</span><span class="sxs-lookup"><span data-stu-id="76bac-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="76bac-133">Veya bir şablon kullanarak yeni bir site oluşturun (örneğin, **başlangıç sitesi** şablonu) ve jQuery dosyaları siteden geçerli sitenize kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="76bac-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="76bac-134">Son olarak, Bing Haritalar kullanmak istiyorsanız, öncelikle (ücretsiz) bir hesap oluşturun ve bir anahtar alın.</span><span class="sxs-lookup"><span data-stu-id="76bac-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="76bac-135">Bir anahtar almak için şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="76bac-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="76bac-136">Bir hesap oluşturmak [Bing Haritalar Geliştirici hesabını](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="76bac-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="76bac-137">Bir Microsoft hesabı (Windows Live ID) de olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="76bac-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="76bac-138">Anahtar için kullanmak istediğiniz belirtebilirsiniz **değerlendirme ve Test**.</span><span class="sxs-lookup"><span data-stu-id="76bac-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="76bac-139">WebMatrix ve IIS Express kullanarak kendi bilgisayarınızda eşleme işlevi test ediyorsanız, Git **Site** çalışma ve Not sitenizin URL'sini (örneğin, `http://localhost:50408`, bağlantı noktası numarası muhtemelen farklı olacaktır rağmen).</span><span class="sxs-lookup"><span data-stu-id="76bac-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="76bac-140">Bu kullanabilirsiniz *localhost* kaydettiğinizde site adres.</span><span class="sxs-lookup"><span data-stu-id="76bac-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="76bac-141">İçin bir hesap kaydettikten sonra Bing Haritalar hesap Merkezi'ne gidin ve tıklatın **anahtarlar oluşturma veya Görünüm**:</span><span class="sxs-lookup"><span data-stu-id="76bac-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![eşleme 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="76bac-143">Bing oluşturur anahtar kaydedin.</span><span class="sxs-lookup"><span data-stu-id="76bac-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="76bac-144">(Google kullanarak) bir adresini temel alan bir eşleme oluşturma</span><span class="sxs-lookup"><span data-stu-id="76bac-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="76bac-145">Aşağıdaki örnekte bir adresini temel alan bir harita işleyen bir sayfa oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="76bac-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="76bac-146">Bu örnek, Google haritalar kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="76bac-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="76bac-147">Adlı bir dosya oluşturun *MapAddress.cshtml* sitenin kök.</span><span class="sxs-lookup"><span data-stu-id="76bac-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="76bac-148">Bu sayfa için geçirdiğiniz bir adresi göre bir harita oluşturur.</span><span class="sxs-lookup"><span data-stu-id="76bac-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="76bac-149">Aşağıdaki kod, var olan içeriğin üzerine dosyasına kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="76bac-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="76bac-150">Sayfanın aşağıdaki özelliklere dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="76bac-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="76bac-151">`<script>` Öğesinde `<head>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="76bac-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="76bac-152">Örnekte, `<script>` öğesi başvuruları *jquery 1.6.4.min.js* jQuery kitaplığı, sürüm 1.6.4 küçültülmüş (sıkıştırılmış) sürümü dosya.</span><span class="sxs-lookup"><span data-stu-id="76bac-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="76bac-153">Başvuru varsaydığını unutmayın *.js* dosyası *betikleri* sitenizin klasörüne.</span><span class="sxs-lookup"><span data-stu-id="76bac-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="76bac-154">JQuery kitaplığı farklı bir sürümünü kullanıyorsanız, yalnızca, bu sürüme doğru işaret ettiğinden olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="76bac-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="76bac-155">Çağrı `@Maps.GetGoogleHtml` sayfasının gövdesindeki.</span><span class="sxs-lookup"><span data-stu-id="76bac-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="76bac-156">Bir adresi eşlemek için bir adres dizesinin geçmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="76bac-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="76bac-157">Bir harita altyapıları için yöntemler benzer şekilde çalışır (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="76bac-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="76bac-158">Sayfayı çalıştırın ve bir adres girin.</span><span class="sxs-lookup"><span data-stu-id="76bac-158">Run the page and enter an address.</span></span> <span data-ttu-id="76bac-159">Sayfasında, belirttiğiniz konuma gösterir Google haritalar üzerinde temel bir harita görüntüler.</span><span class="sxs-lookup"><span data-stu-id="76bac-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![eşleme-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="76bac-161">Enlem ve boylam dayalı bir harita oluşturmak (Bing kullanarak) koordine eder</span><span class="sxs-lookup"><span data-stu-id="76bac-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="76bac-162">Bu örnek bir harita koordinatlarına göre oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="76bac-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="76bac-163">Bu örnek, Bing Haritalar kullanmayı ve Bing anahtarınızı içerecek şekilde nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="76bac-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="76bac-164">(Bir harita motorları ayrıca Bing anahtar kullanmadan koordinatları dayalı bir harita oluşturabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="76bac-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="76bac-165">Adlı bir dosya oluşturun *MapCoordinates.cshtml* kök site ve var olan içerik aşağıdaki kodu ve İşaretleme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="76bac-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="76bac-166">Değiştir `your-key-here` daha önce oluşturulan Bing Haritalar anahtara sahip.</span><span class="sxs-lookup"><span data-stu-id="76bac-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="76bac-167">Çalıştırma *MapCoordinates.cshtml* sayfasında, enlem ve boylam koordinatları girin ve ardından **Map It!**</span><span class="sxs-lookup"><span data-stu-id="76bac-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="76bac-168">düğme.</span><span class="sxs-lookup"><span data-stu-id="76bac-168">button.</span></span> <span data-ttu-id="76bac-169">(Tüm koordinatları bilmiyorsanız, aşağıdaki deneyin.</span><span class="sxs-lookup"><span data-stu-id="76bac-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="76bac-170">Microsoft Redmond kampüs konumunda budur.)</span><span class="sxs-lookup"><span data-stu-id="76bac-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="76bac-171">Latitude: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="76bac-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="76bac-172">Boylam:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="76bac-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="76bac-173">Belirttiğiniz koordinatları kullanarak sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="76bac-173">The page is displayed using the coordinates that you specified.</span></span>

     ![eşleme-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="76bac-175">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="76bac-175">Additional Resources</span></span>


[<span data-ttu-id="76bac-176">Microsoft.Maps API Başvurusu</span><span class="sxs-lookup"><span data-stu-id="76bac-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
