---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter ile ASP.NET Web sayfaları Yardımcısı | Microsoft Docs
author: Rick-Anderson
description: Bu konu başlığında ve uygulama bir Twitter Yardımcısı, WebMatrix 3'ü projenize ekleme işlemini göstermektedir. Twitter Yardımcısı kodunu içerir ve yardımcı çağırma gösterilmektedir...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299436"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="ead72-104">ASP.NET Web Sayfaları ile Twitter Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="ead72-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="ead72-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ead72-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ead72-106">Twitter Yardımcıları kullanım dışıdır.</span><span class="sxs-lookup"><span data-stu-id="ead72-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="ead72-107">Twitter'ın en son engagement araçları için Web siteleri için bkz: [Twitter için Web siteleri genel bakış](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="ead72-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="ead72-108">Bu konu başlığında ve uygulama bir Twitter Yardımcısı, WebMatrix 3'ü projenize ekleme işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="ead72-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="ead72-109">Twitter Yardımcısı kodunu içerir ve yardımcı yöntemleri çağırma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ead72-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="ead72-110">Twitter.cshtml dosyası için bu kodu tarafından geliştirilmiştir **Tian Pan** Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ead72-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ead72-111">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="ead72-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ead72-112">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="ead72-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="ead72-113">Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="ead72-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="ead72-114">Giriş</span><span class="sxs-lookup"><span data-stu-id="ead72-114">Introduction</span></span>

<span data-ttu-id="ead72-115">Bu konuda, bir Twitter Yardımcısı uygulamanıza ekleyin ve yardımcı yöntemlerini çağırmak için Razor sözdizimini kullanan gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ead72-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="ead72-116">Twitter Yardımcısı, Twitter düğmeler ve uygulamanızdaki pencere öğeleri bir araya kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="ead72-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="ead72-117">Bir kullanıcının zaman çizelgesinde veya diyez etiketi için arama sonuçları gibi bir Twitter pencere öğesini kullanmak için öncelikle oluşturmanız gerekir [pencere öğesi Twitter'da](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="ead72-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="ead72-118">Pencere öğenizi oluşturduktan sonra bir pencere öğesi kimliği alırsınız. Pencere öğesi Göster yardımcı yöntemler ararken Bu pencere öğesi kimliği bir parametre olarak geçiriyoruz.</span><span class="sxs-lookup"><span data-stu-id="ead72-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="ead72-119">Bu konu başlığında, Twitter API'si 1.1 sürümü alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ead72-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="ead72-120">Projenize doğrudan Twitter Yardımcısı kod ekleyerek, Twitter API'si değişirse yardımcı kod güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ead72-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="ead72-121">Webmatrix'i yükleme hakkında daha fazla bilgi için bkz: [Karşınızda ASP.NET Web sayfaları Başlarken 2 -](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="ead72-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="ead72-122">Twitter Yardımcısı projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ead72-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="ead72-123">Twitter Yardımcısı eklemek için ilk olarak, adlı bir klasör ekleyin **uygulama\_kod** projenize.</span><span class="sxs-lookup"><span data-stu-id="ead72-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="ead72-124">Adlı bir dosya oluşturup **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="ead72-124">Then, create a file named **Twitter.cshtml**.</span></span>

![App_Code klasörü](twitter-helper/_static/image1.png)

<span data-ttu-id="ead72-126">Twitter.cshtml varsayılan kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ead72-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="ead72-127">Web sayfalarınızdan twitter yöntemlerini çağırın</span><span class="sxs-lookup"><span data-stu-id="ead72-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="ead72-128">Aşağıdaki örnek, projenizde sayfasından Twitter yardımcı yöntemler kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="ead72-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="ead72-129">Projenizde, ihtiyaçlarınıza uygun olan değerleri içeren parametre değerlerini değiştirmek istersiniz.</span><span class="sxs-lookup"><span data-stu-id="ead72-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="ead72-130">Yöntemleri nasıl keşfetmek için sağlanan pencere öğesi kimlikleri kullanabilirsiniz, ancak projeniz için kendi pencere öğelerinizi oluşturmak isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="ead72-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="ead72-131">Aşağıda gösterilen parametrelerin tümü gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ead72-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="ead72-132">İsteğe bağlı parametreler, bir düğme veya pencere öğesi nasıl görüntüleneceğini özelleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ead72-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="ead72-133">Örneğin, İzle düğmesine izlemek için kullanıcı adı yalnızca gerektiriyor, ancak örnek takipçi sayısı eklemek nasıl düğmesi ve dilin boyutunu belirlemek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="ead72-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="ead72-134">Sonuçları göster</span><span class="sxs-lookup"><span data-stu-id="ead72-134">See the results</span></span>

<span data-ttu-id="ead72-135">Yukarıdaki kod, aşağıdaki düğmeler ve pencere öğeleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ead72-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="ead72-136">Bu düğmeler ve pencere öğeleri tam işlevsel, ekran görüntüleri değil.</span><span class="sxs-lookup"><span data-stu-id="ead72-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="ead72-137">Dili için parametre olarak ayarlanmış olduğu için İspanyolca izleyin düğmesi görüntülenir **es**.</span><span class="sxs-lookup"><span data-stu-id="ead72-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="ead72-138">Düğme izleyin</span><span class="sxs-lookup"><span data-stu-id="ead72-138">Follow Button</span></span>

<span data-ttu-id="ead72-139">[İzleyin @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="ead72-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="ead72-140">Tweet düğmesi</span><span class="sxs-lookup"><span data-stu-id="ead72-140">Tweet Button</span></span>

<span data-ttu-id="ead72-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="ead72-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="ead72-142">Kullanıcı zaman çizelgesini (Profil)</span><span class="sxs-lookup"><span data-stu-id="ead72-142">User Timeline (Profile)</span></span>

<span data-ttu-id="ead72-143">[Seçtiği tweet'ler: @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="ead72-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="ead72-144">Sık Kullanılanlar</span><span class="sxs-lookup"><span data-stu-id="ead72-144">Favorites</span></span>

<span data-ttu-id="ead72-145">[Sık kullanılan seçtiği Tweet'ler: @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="ead72-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="ead72-146">List</span><span class="sxs-lookup"><span data-stu-id="ead72-146">List</span></span>

<span data-ttu-id="ead72-147">[Gelen bir tweet @Microsoft/MS \_tüketici\_bantları](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="ead72-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="ead72-148">Ara</span><span class="sxs-lookup"><span data-stu-id="ead72-148">Search</span></span>

<span data-ttu-id="ead72-149">[İlgili bir tweet &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="ead72-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
