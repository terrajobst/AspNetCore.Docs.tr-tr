---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Sosyal ağ ekleme için ASP.NET Web sayfaları (Razor) siteler | Microsoft Docs
author: Rick-Anderson
description: Bu bölümde, sitenize sosyal ağ hizmetleriyle tümleştirmeye yönelik açıklanmaktadır. Bu bölümde, Web sitenizi yer işareti/bağlantı kişilere öğreneceksiniz...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: d2b970e7b80e4129d0a912f648f9c4a54df531b2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021163"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="b36ef-104">ASP.NET Web sayfaları (Razor) siteleri sosyal ağ ekleme</span><span class="sxs-lookup"><span data-stu-id="b36ef-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="b36ef-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b36ef-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b36ef-106">Bu makalede, sosyal ağ bağlantıları Facebook, Twitter, Reddit ve Digg için bir ASP.NET Web sayfaları (Razor) Web sitesi sayfalarına ekleme ve Twitter feed'lerine ve Xbox oyuncu kartları Gravatar görüntülerini ekleme açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b36ef-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="b36ef-107">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="b36ef-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b36ef-108">Yer işareti/sitenizi bağlantı kişilere nasıl.</span><span class="sxs-lookup"><span data-stu-id="b36ef-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="b36ef-109">Bir Twitter akışı ekleme.</span><span class="sxs-lookup"><span data-stu-id="b36ef-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="b36ef-110">Bir Facebook ekleme **gibi** sayfalara düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b36ef-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="b36ef-111">Gravatar.com görüntülerin nasıl.</span><span class="sxs-lookup"><span data-stu-id="b36ef-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="b36ef-112">Sitenizde bir Xbox oyuncu kartını görüntülemek nasıl.</span><span class="sxs-lookup"><span data-stu-id="b36ef-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b36ef-113">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="b36ef-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b36ef-114">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="b36ef-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="b36ef-115">ASP.NET Web yardımcı kitaplık (NuGet paketi)</span><span class="sxs-lookup"><span data-stu-id="b36ef-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="b36ef-116">Bu öğretici ile ASP.NET Web Pages 3, ASP.NET Web Yardımcısı kitaplığını kullanan bölümleri dışında da çalışır.</span><span class="sxs-lookup"><span data-stu-id="b36ef-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="b36ef-117">Sosyal ağ sitelerine sitenizde bağlama</span><span class="sxs-lookup"><span data-stu-id="b36ef-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="b36ef-118">Kişiler, sitenizdeki bir şey istiyorsanız, bunlar genellikle arkadaşlarınızla paylaşmak istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="b36ef-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="b36ef-119">Bu kişiler, Digg, Reddit, Facebook, Twitter veya benzer siteleri sayfada paylaşmak için tıklayabileceği karakterleri (simge) görüntüleyerek kolayca yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b36ef-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="b36ef-120">Bu karakterleri görüntülenecek ekleme `LinkSharecode` yardımcı olacak bir sayfa.</span><span class="sxs-lookup"><span data-stu-id="b36ef-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="b36ef-121">Sayfanızı ziyaret eden kişiler, bireysel bir karakter tıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b36ef-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="b36ef-122">Bunlar, sosyal ağ sitesine bir hesabınız varsa, bunlar sitede ardından sayfanıza bir bağlantı gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b36ef-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![Resim 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="b36ef-124">ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), zaten halen - eklemediyseniz adlı bir sayfa oluşturun *ListLinkShare.cshtml* ekleyin Aşağıdaki biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="b36ef-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="b36ef-125">Bu örnekte, zaman `LinkShare` Yardımcısını çalıştırmaları, sayfa başlığı, sırayla sosyal ağ sitesine sayfa başlığının geçen bir parametre olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="b36ef-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="b36ef-126">Ancak, istediğiniz herhangi bir dize içinde geçirebiliriz.</span><span class="sxs-lookup"><span data-stu-id="b36ef-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="b36ef-127">Bu örnek ayrıca listesinde içermek için hangi sosyal ağ sitelerine belirtir.</span><span class="sxs-lookup"><span data-stu-id="b36ef-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="b36ef-128">İlgili sitenize sosyal ağ sitelerine belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b36ef-128">You can specify the social networking sites that are relevant to your site.</span></span>
2. <span data-ttu-id="b36ef-129">Çalıştırma *ListLinkShare.cshtml* sayfasını bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="b36ef-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="b36ef-130">(Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.)</span><span class="sxs-lookup"><span data-stu-id="b36ef-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
3. <span data-ttu-id="b36ef-131">Bir yazıtipi için Kayıtlısınız sitelerden birine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b36ef-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="b36ef-132">Bağlantı sayfasına bağlantı paylaşabileceğiniz seçili sosyal ağ sitesinde götürür.</span><span class="sxs-lookup"><span data-stu-id="b36ef-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="b36ef-133">Reddit bağlantıya tıklarsanız, örneğin, adresine yönlendirilirsiniz `submit to reddit` Reddit sitesinde sayfasında.</span><span class="sxs-lookup"><span data-stu-id="b36ef-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

     ![Resim 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="b36ef-135">Bir Twitter ekleme akışı</span><span class="sxs-lookup"><span data-stu-id="b36ef-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="b36ef-136">Twitter API'si geçerli sürümü ile uyumlu bir Twitter Yardımcısı'nı kullanma hakkında daha fazla bilgi için bkz: [Twitter Yardımcısı](../ui-layouts-and-themes/twitter-helper.md).</span><span class="sxs-lookup"><span data-stu-id="b36ef-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="b36ef-137">Bu örnekte, kolayca birçok sayfalarından kodunu yeniden kullanabilmesi kendi Yardımcısı yazma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b36ef-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="b36ef-138">Bir Facebook görüntüleme &quot;gibi&quot; düğmesi</span><span class="sxs-lookup"><span data-stu-id="b36ef-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="b36ef-139">Bazı durumlarda, en iyi seçenek, üzerinde bir yardımcı güvenmek yerine sosyal ağ sağlayıcısına doğrudan kod elde etmektir.</span><span class="sxs-lookup"><span data-stu-id="b36ef-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="b36ef-140">Sosyal ağ sağlayıcısına seçeneklerini yardımcı güncelleştirilir daha hızlı güncelleştiriyorsa bu özellikle doğrudur.</span><span class="sxs-lookup"><span data-stu-id="b36ef-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="b36ef-141">Facebook özellikleri (örneğin, sıra Beğen düğmesi) sitenize eklemek için kod parçacıkları'nden alabilirsiniz [developers.facebook.com](https://developers.facebook.com/) site.</span><span class="sxs-lookup"><span data-stu-id="b36ef-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="b36ef-142">Facebook sitede, site için ilgili kod parçacığı oluşturmak için kendi araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="b36ef-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="b36ef-143">Aşağıdaki vurgulanmış kodu developers.facebook.com sitesinde düğmesi gibi aracından alınan kodudur.</span><span class="sxs-lookup"><span data-stu-id="b36ef-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="b36ef-144">Kendi uygulama kimliğini sağlamanız gerekir</span><span class="sxs-lookup"><span data-stu-id="b36ef-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="b36ef-145">Gravatar görüntü işleme</span><span class="sxs-lookup"><span data-stu-id="b36ef-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="b36ef-146">A *Gravatar* (bir &quot;genel tanınan avatar&quot;) birden çok Web sitelerinde avatarınız kullanılabilecek bir görüntü &#8212; diğer bir deyişle, gösteren görüntü.</span><span class="sxs-lookup"><span data-stu-id="b36ef-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="b36ef-147">Örneğin, bir Gravatar bir kişi bir forum gönderisi, blog açıklamasında tanımlamak ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="b36ef-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="b36ef-148">(Gravatar Web sitesinden kendi Gravatar kaydedebilirsiniz [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Web sitenizde kişilerin adları veya e-posta adreslerinin yanındaki görüntüleri görüntülemek istiyorsanız, Gravatar yardımcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b36ef-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="b36ef-149">Bu örnekte, kendiniz temsil eden tek bir Gravatar kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="b36ef-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="b36ef-150">Bir Gravatar kullanmak için başka bir sitenizde'na kaydederken Gravatar adresini belirtin kişilere yoludur.</span><span class="sxs-lookup"><span data-stu-id="b36ef-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="b36ef-151">(Kaydetmek kişilere öğrenebilirsiniz [güvenlik ekleme ve bir ASP.NET Web sayfaları sitesinde üyelik](https://go.microsoft.com/fwlink/?LinkId=202904).) Kullanıcı bilgilerini görüntüleme olduğunda, ardından yalnızca Gravatar kullanıcının adını burada görüntülemek için ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b36ef-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="b36ef-152">ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.</span><span class="sxs-lookup"><span data-stu-id="b36ef-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="b36ef-153">Adlı yeni bir web sayfası oluşturma *Gravatar.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b36ef-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="b36ef-154">Aşağıdaki biçimlendirmede dosyaya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b36ef-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="b36ef-155">`Gravatar.GetHtml` Yöntemi Gravatar görüntü sayfasında görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b36ef-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="b36ef-156">Görüntü boyutunu değiştirmek için ikinci parametre olarak bir sayı içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b36ef-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="b36ef-157">Varsayılan boyutu 80'dir.</span><span class="sxs-lookup"><span data-stu-id="b36ef-157">The default size is 80.</span></span> <span data-ttu-id="b36ef-158">80'den az yapma görüntüyü daha küçük sayılar.</span><span class="sxs-lookup"><span data-stu-id="b36ef-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="b36ef-159">Görüntü 80 büyük sayılar büyütün.</span><span class="sxs-lookup"><span data-stu-id="b36ef-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="b36ef-160">İçinde `Gravatar.GetHtml` yöntemlerini değiştirin `<Your Gravatar account here>` Gravatar hesabınız için kullandığınız e-posta adresi.</span><span class="sxs-lookup"><span data-stu-id="b36ef-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="b36ef-161">(Bir Gravatar hesabınız yoksa, mu kişinin e-posta adresini kullanabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="b36ef-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="b36ef-162">Sayfayı tarayıcınızda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b36ef-162">Run the page in your browser.</span></span> <span data-ttu-id="b36ef-163">Sayfa, belirttiğiniz e-posta adresi için iki Gravatar görüntülerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b36ef-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="b36ef-164">İkinci görüntü, daha küçük.</span><span class="sxs-lookup"><span data-stu-id="b36ef-164">The second image is smaller than the first.</span></span> 

    ![Resim 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="b36ef-166">Xbox oyuncu kart görüntüleme</span><span class="sxs-lookup"><span data-stu-id="b36ef-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="b36ef-167">Kişiler Microsoft Xbox çevrimiçi oyun, her kullanıcının benzersiz bir kimliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b36ef-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="b36ef-168">İstatistikleri kendi saygınlığı, oyuncu puanı gösterir ve oyunlar oynatılan bir oyuncu kart biçiminde her oyuncu için tutulur.</span><span class="sxs-lookup"><span data-stu-id="b36ef-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="b36ef-169">Xbox oyuncu kullanıyorsanız, oyuncu kartınız sitenizde sayfalarında kullanarak gösterebilirsiniz `GamerCard` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="b36ef-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="b36ef-170">ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.</span><span class="sxs-lookup"><span data-stu-id="b36ef-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="b36ef-171">Adlı yeni bir sayfa oluşturun *XboxGamer.cshtml* ve aşağıdaki işaretlemeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b36ef-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="b36ef-172">Kullandığınız `GamerCard.GetHtml` özelliği görüntülenecek oyuncu kart için bir diğer ad belirtin.</span><span class="sxs-lookup"><span data-stu-id="b36ef-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="b36ef-173">Sayfayı tarayıcınızda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b36ef-173">Run the page in your browser.</span></span> <span data-ttu-id="b36ef-174">Sayfa belirttiğiniz Xbox oyuncu kartı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b36ef-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![Resim 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
