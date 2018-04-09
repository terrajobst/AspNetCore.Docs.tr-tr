---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Sosyal ağ eklemek için ASP.NET Web sayfaları (Razor) siteleri | Microsoft Docs
author: tfitzmac
description: Bu bölümde, sitenizi Sosyal Ağ Hizmetleri ile tümleştirmek açıklanmaktadır. Bu bölümde, Web sitenizin yer işareti/bağlantı kişilere öğreneceksiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2d1f0074edf473c4be06adaa32598dd828a7552c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="a8e8f-104">ASP.NET Web sayfaları (Razor) siteleri sosyal ağ ekleme</span><span class="sxs-lookup"><span data-stu-id="a8e8f-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="a8e8f-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a8e8f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a8e8f-106">Bu makalede, sosyal ağ bağlantıları Facebook, Twitter, Reddit ve Digg için bir ASP.NET Web sayfaları (Razor) Web sayfalarında nasıl ekleneceğini ve Twitter akışlarını, Xbox oyuncu kartları ve Gravatar görüntüleri dahil olmak üzere nasıl açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="a8e8f-107">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="a8e8f-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="a8e8f-108">Yer işareti/sitenizi bağlantı kişilere nasıl.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="a8e8f-109">Bir Twitter akışı ekleme.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="a8e8f-110">Bir Facebook ekleme **gibi** düğmesi sayfalara.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="a8e8f-111">Gravatar.com resimleri işlemek nasıl.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="a8e8f-112">Sitenizde Xbox oyuncu kartı görüntülemek nasıl.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a8e8f-113">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="a8e8f-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a8e8f-114">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="a8e8f-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="a8e8f-115">ASP.NET Web Yardımcısı kitaplığı (NuGet paketi)</span><span class="sxs-lookup"><span data-stu-id="a8e8f-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="a8e8f-116">Bu öğreticide, ASP.NET Web Pages 3 ile ASP.NET Web Yardımcısını kitaplığını kullanan bölümleri dışında de çalışır.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="a8e8f-117">Web sitenizi sosyal ağ sitelerinde bağlama</span><span class="sxs-lookup"><span data-stu-id="a8e8f-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="a8e8f-118">Kişiler, sitenizde bir şey istiyorsanız, genellikle arkadaşlarınızla paylaşmak isterler.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="a8e8f-119">Bu kişiler Digg, Reddit, Facebook, Twitter veya benzer siteleri sayfasında paylaşmak için tıklatabilirsiniz (simgeler) karakterlerin görüntüleyerek kolayca yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="a8e8f-120">Bu karakterlerin görüntülemek için add `LinkSharecode` bir sayfaya Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="a8e8f-121">Sayfanızı ziyaret eden kişiler tek tek bir karakter tıklatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="a8e8f-122">Bunlar, sosyal ağ sitesiyle bir hesabınız varsa, bunlar sayfanıza bir bağlantı sonra bu sitede nakledebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![Resim 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="a8e8f-124">ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), kendisine - henüz eklediyseniz adlı bir sayfa oluşturma *ListLinkShare.cshtml* ve Ekle Aşağıdaki biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="a8e8f-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="a8e8f-125">Bu örnekte, zaman `LinkShare` Yardımcısı çalıştığında, sayfa başlığı, sosyal ağ sitesine sayfa başlığı sırayla geçirmeden bir parametre olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="a8e8f-126">Ancak, istediğiniz herhangi bir dize geçirebilirdiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="a8e8f-127">Bu örnek ayrıca hangi sosyal ağ sitelerine listeye dahil belirtir.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="a8e8f-128">Siteniz için uygun olan sosyal ağ sitelerine belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-128">You can specify the social networking sites that are relevant to your site.</span></span>
2. <span data-ttu-id="a8e8f-129">Çalıştırma *ListLinkShare.cshtml* sayfasını bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="a8e8f-130">(Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.)</span><span class="sxs-lookup"><span data-stu-id="a8e8f-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
3. <span data-ttu-id="a8e8f-131">Oturum açtıysanız sitelerden biri karaktere'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="a8e8f-132">Bağlantıyı sayfaya bir bağlantı burada paylaşabilirsiniz seçili sosyal ağ sitesinde alır.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="a8e8f-133">Reddit bağlantısını tıklatırsanız, örneğin, gittiğiniz `submit to reddit` Reddit Web sayfasında.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

     ![Resim 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="a8e8f-135">Bir Twitter ekleme akışı</span><span class="sxs-lookup"><span data-stu-id="a8e8f-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="a8e8f-136">Twitter API'si geçerli sürümü ile uyumlu olan bir Twitter Yardımcısı kullanma hakkında daha fazla bilgi için bkz: [Twitter Yardımcısı](../ui-layouts-and-themes/twitter-helper.md).</span><span class="sxs-lookup"><span data-stu-id="a8e8f-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="a8e8f-137">Bu örnek nasıl birden çok sayfa kodundan kolayca yeniden kullanabilirsiniz şekilde kendi yardımcı yazılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="a8e8f-138">Bir Facebook görüntüleme &quot;gibi&quot; düğmesi</span><span class="sxs-lookup"><span data-stu-id="a8e8f-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="a8e8f-139">Bazı durumlarda, sizin için en iyi seçenek Yardımcısı üzerinde güvenmek yerine sosyal ağ sağlayıcı doğrudan kod elde etmektir.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="a8e8f-140">Sosyal ağ sağlayıcısına seçeneklerini yardımcı güncelleştirilmiş daha hızlı güncelleştirmeleri olursa bu özellikle doğrudur.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="a8e8f-141">Facebook özellikleri (örneğin, LIKE düğmesi) sitenize eklemek için gelen kod parçacıkları alabilirsiniz [developers.facebook.com](https://developers.facebook.com/) site.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="a8e8f-142">Facebook sitesinde sitenize ilgili bir kod parçacığı oluşturmak için kendi araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="a8e8f-143">Aşağıdaki vurgulanmış kodu developers.facebook.com sitesinde gibi düğmesi aracından alınan kodudur.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="a8e8f-144">Kendi uygulama kimliğini sağlamanız gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="a8e8f-145">Gravatar görüntü işleme</span><span class="sxs-lookup"><span data-stu-id="a8e8f-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="a8e8f-146">A *Gravatar* (bir &quot;genel tanınan avatar&quot;) birden çok Web sitelerinde, avatar kullanılabilecek bir görüntü &#8212; diğer bir deyişle, temsil eden bir görüntü.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="a8e8f-147">Örneğin, bir Gravatar kişisel bir blog açıklamasında bir forum gönderisi olarak belirleyin ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="a8e8f-148">(Kendi Gravatar Gravatar Web sitesindeki kaydedebilirsiniz [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Kişilerin adlarını veya e-posta adreslerini yanındaki resimleri, Web sitenizde görüntülemek istiyorsanız, Gravatar yardımcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="a8e8f-149">Bu örnekte, kendiniz temsil eden tek bir Gravatar kullanıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="a8e8f-150">Bir Gravatar kullanmak için başka bir sitenizde kaydettiğinizde, Gravatar adreslerini belirtin kişilere yoludur.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="a8e8f-151">(Kaydetmek kişilere öğrenebilirsiniz [güvenlik ekleme ve bir ASP.NET Web sayfaları Site üyeliği](https://go.microsoft.com/fwlink/?LinkId=202904).) Bu kullanıcı bilgilerini görüntüleme olduğunda, daha sonra yalnızca Gravatar kullanıcının adını burada görüntülemek için ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="a8e8f-152">ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="a8e8f-153">Adlı yeni bir web sayfası oluşturun *Gravatar.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="a8e8f-154">Aşağıdaki biçimlendirmede dosyaya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a8e8f-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="a8e8f-155">`Gravatar.GetHtml` Yöntemi Gravatar görüntü sayfasında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="a8e8f-156">Görüntü boyutunu değiştirmek için ikinci parametre olarak bir sayı içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="a8e8f-157">Varsayılan boyut 80'dir.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-157">The default size is 80.</span></span> <span data-ttu-id="a8e8f-158">80'den az yapma görüntünün küçük numaralandırır.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="a8e8f-159">80'den büyük sayılar görüntü büyütün.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="a8e8f-160">İçinde `Gravatar.GetHtml` yöntemleri Değiştir `<Your Gravatar account here>` Gravatar hesabınız için kullandığınız e-posta adresine sahip.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="a8e8f-161">(Gravatar hesabınız yoksa, mu birinin e-posta adresini kullanabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="a8e8f-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="a8e8f-162">Sayfayı tarayıcınızda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-162">Run the page in your browser.</span></span> <span data-ttu-id="a8e8f-163">Sayfasında, belirttiğiniz e-posta adresi için iki Gravatar görüntü görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="a8e8f-164">İkinci görüntünün ilk küçüktür.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-164">The second image is smaller than the first.</span></span> 

    ![Resim 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="a8e8f-166">Xbox oyuncu kartı görüntüleme</span><span class="sxs-lookup"><span data-stu-id="a8e8f-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="a8e8f-167">Kişiler çevrimiçi oyunlar Microsoft Xbox yürütürken, her kullanıcı bir benzersiz kimliği vardır.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="a8e8f-168">İstatistikleri her Player kendi saygınlığı, oyuncu puanı gösterilir ve son oynanan oyunları bir oyuncu kartı biçiminde saklanır.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="a8e8f-169">Oyuncu Xbox değilseniz, size oyuncu kartınız sitenizdeki sayfalarında kullanarak gösterebilir `GamerCard` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="a8e8f-170">ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="a8e8f-171">Adlı yeni bir sayfa oluşturma *XboxGamer.cshtml* ve aşağıdaki biçimlendirmeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="a8e8f-172">Kullandığınız `GamerCard.GetHtml` özelliği görüntülenecek oyuncu kartı için diğer ad belirtin.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="a8e8f-173">Sayfayı tarayıcınızda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-173">Run the page in your browser.</span></span> <span data-ttu-id="a8e8f-174">Sayfa belirttiğiniz Xbox oyuncu kartı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a8e8f-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![Resim 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
