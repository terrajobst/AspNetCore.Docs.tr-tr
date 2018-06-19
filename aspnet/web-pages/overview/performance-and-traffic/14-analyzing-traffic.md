---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: (Razor) Site ASP.NET Web sayfaları için izleme ziyaretçi bilgileri (Analytics) | Microsoft Docs
author: tfitzmac
description: Giderek Web sitenizin kabulünüzü sonra Web sitesi trafiğini analiz etmek isteyebilirsiniz.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572934"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="ac00f-103">Ziyaretçi bilgi (Analytics) bir ASP.NET Web sayfaları (Razor) sitesi için izleme</span><span class="sxs-lookup"><span data-stu-id="ac00f-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="ac00f-104">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ac00f-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ac00f-105">Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sayfaları Web sitesi analytics eklemek için bir yardımcı kullanmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="ac00f-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="ac00f-106">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="ac00f-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ac00f-107">Bir analiz sağlayıcısı, Web sitesi trafiğini hakkında bilgi göndermek nasıl.</span><span class="sxs-lookup"><span data-stu-id="ac00f-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="ac00f-108">Bu makalede sunulan özellikler programlama ASP.NET şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ac00f-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="ac00f-109">`Analytics` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="ac00f-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ac00f-110">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="ac00f-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ac00f-111">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="ac00f-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="ac00f-112">ASP.NET Web Yardımcıları kitaplığı (NuGet paketi)</span><span class="sxs-lookup"><span data-stu-id="ac00f-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="ac00f-113">Analytics kişiler site kullanımını anlamak için Web trafiği ölçer teknolojisini için genel bir terimdir.</span><span class="sxs-lookup"><span data-stu-id="ac00f-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="ac00f-114">Birçok Analiz Hizmetleri Hizmetleri'nden Google, Yahoo, StatCounter ve diğerleri de dahil olmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ac00f-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="ac00f-115">Bir hesap için siteyi kaydettirmeniz burada analiz sağlayıcısı ile kayıt olduğunu Works olduğu şekilde analytics izlemek istiyorsunuz. Sağlayıcı bir kimlik veya izleme hesabınız için kodu içeren JavaScript kod parçacığını gönderir.</span><span class="sxs-lookup"><span data-stu-id="ac00f-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="ac00f-116">İzlemek istediğiniz sitenin web sayfalarında JavaScript kod parçacığı ekleyin. (Genellikle analytics kod parçacığını bir alt bilgi veya düzen sayfası veya sitenizdeki her sayfada görünen diğer HTML biçimlendirmesi eklediğiniz.) Kullanıcılar bu JavaScript parçacıkları birini içeren bir sayfa istediğinde, kod parçacığında sayfa çeşitli ayrıntılarını kaydeder analiz sağlayıcısı geçerli sayfa hakkında bilgi gönderir.</span><span class="sxs-lookup"><span data-stu-id="ac00f-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="ac00f-117">Site İstatistikler bakın istediğinizde, analytics sağlayıcının Web sitesinde oturum açmak.</span><span class="sxs-lookup"><span data-stu-id="ac00f-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="ac00f-118">Ardından, gibi siteniz hakkında raporlar her türlü görüntüleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ac00f-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="ac00f-119">Tek tek sayfaları için sayfa görünümleri sayısı.</span><span class="sxs-lookup"><span data-stu-id="ac00f-119">The number of page views for individual pages.</span></span> <span data-ttu-id="ac00f-120">Bu, kaç (kabaca) kişinin siteyi ziyaret ediyorsanız ve hangi sitenizde en popüler sayfalarıdır bildirir.</span><span class="sxs-lookup"><span data-stu-id="ac00f-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="ac00f-121">Ne kadar süreyle kişiler belirli sayfalara ayırın.</span><span class="sxs-lookup"><span data-stu-id="ac00f-121">How long people spend on specific pages.</span></span> <span data-ttu-id="ac00f-122">Bu giriş sayfanız kişilerin ilgi olup tutma gibi şeyler anlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ac00f-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="ac00f-123">Sitenizi ziyaret önce üzerinde hangi siteleri kişiler yoktu.</span><span class="sxs-lookup"><span data-stu-id="ac00f-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="ac00f-124">Bu trafiğinizi arar ve benzeri bağlantılardan gelen olup olmadığını anlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="ac00f-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="ac00f-125">Kişiler, sitenizi ziyaret ettiğinizde ve ne kadar süreyle kalırlar.</span><span class="sxs-lookup"><span data-stu-id="ac00f-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="ac00f-126">Hangi ülkelerde, ziyaretçileri arasındadır.</span><span class="sxs-lookup"><span data-stu-id="ac00f-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="ac00f-127">Hangi tarayıcılar ve işletim sistemleri, ziyaretçileri kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="ac00f-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="ac00f-129">Bir sayfaya Analytics eklemek için bir yardımcı kullanma</span><span class="sxs-lookup"><span data-stu-id="ac00f-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="ac00f-130">ASP.NET Web sayfaları içeren birkaç analytics Yardımcıları (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, ve `Analytics.GetStatCounterHtml`) kolaylaştıran analizi için kullanılan JavaScript parçacıkları yönetmek.</span><span class="sxs-lookup"><span data-stu-id="ac00f-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="ac00f-131">Nasıl ve nerede çözülmesi yerine JavaScript kodu koymak için tüm yapmanız gereken olduğu yardımcı bir sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ac00f-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="ac00f-132">Sağlamanız gereken tek bilgi hesap adı, kimliği veya izleme kodunu ' dir.</span><span class="sxs-lookup"><span data-stu-id="ac00f-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="ac00f-133">(StatCounter için Ayrıca birkaç ek değerler sağlamanız gerekir.)</span><span class="sxs-lookup"><span data-stu-id="ac00f-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="ac00f-134">Bu yordamda kullanan bir düzen sayfasını oluşturacaksınız `GetGoogleHtml` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="ac00f-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="ac00f-135">Diğer analytics sağlayıcılardan biri ile zaten bir hesabınız varsa, bu hesabı kullanın ve gerektiğinde hafif ayarlamaları yapın.</span><span class="sxs-lookup"><span data-stu-id="ac00f-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="ac00f-136">Bir analytics hesabı oluşturmak için izleme olmasını istediğiniz site URL'sini kaydedersiniz.</span><span class="sxs-lookup"><span data-stu-id="ac00f-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="ac00f-137">Her şeyin yerel bilgisayarınızda test ediyorsanız, (yalnızca trafiğini şey) gerçek trafiği izleme olmaz kaydı ve görünüm site İstatistikler mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="ac00f-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="ac00f-138">Ancak bu yordam analytics yardımcı bir sayfaya nasıl eklediğiniz gösterir.</span><span class="sxs-lookup"><span data-stu-id="ac00f-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="ac00f-139">Sitenizi yayımladığınızda, canlı sitede analytics sağlayıcınız bilgi gönderin.</span><span class="sxs-lookup"><span data-stu-id="ac00f-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="ac00f-140">ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), onu zaten eklemediniz.</span><span class="sxs-lookup"><span data-stu-id="ac00f-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="ac00f-141">Google Analytics ile bir hesap oluşturun ve hesap adını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ac00f-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="ac00f-142">Adlı bir düzen sayfası oluşturun *Analytics.cshtml* ve aşağıdaki biçimlendirmeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ac00f-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="ac00f-143">Çağrı yerleştirmelisiniz `Analytics` Yardımcısı, web sayfasının gövdesindeki (önce `</body>` etiketi).</span><span class="sxs-lookup"><span data-stu-id="ac00f-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="ac00f-144">Aksi durumda, tarayıcının betiği çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="ac00f-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="ac00f-145">Farklı analiz sağlayıcısı kullanıyorsanız, bunun yerine aşağıdaki Yardımcılar birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="ac00f-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="ac00f-146">(Yahoo)`@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="ac00f-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="ac00f-147">(StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="ac00f-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="ac00f-148">Değiştir `myaccount` hesabı, kimliği veya 1. adımda oluşturduğunuz izleme kodu adı.</span><span class="sxs-lookup"><span data-stu-id="ac00f-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="ac00f-149">Sayfayı tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ac00f-149">Run the page in the browser.</span></span> <span data-ttu-id="ac00f-150">(Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.)</span><span class="sxs-lookup"><span data-stu-id="ac00f-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="ac00f-151">Tarayıcıda, sayfa kaynağı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="ac00f-151">In the browser, view the page source.</span></span> <span data-ttu-id="ac00f-152">İşlenmiş analytics kod kullanabileceksiniz:</span><span class="sxs-lookup"><span data-stu-id="ac00f-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="ac00f-153">Google Analytics sitesinde oturum ve sitenizi istatistikleri inceleyin.</span><span class="sxs-lookup"><span data-stu-id="ac00f-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="ac00f-154">Canlı bir sitede sayfa çalıştırıyorsanız, sayfanızı ziyaret günlüklerini bir girişine bakın.</span><span class="sxs-lookup"><span data-stu-id="ac00f-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="ac00f-155">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ac00f-155">Additional Resources</span></span>

- [<span data-ttu-id="ac00f-156">Google Analytics site</span><span class="sxs-lookup"><span data-stu-id="ac00f-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="ac00f-157">Yahoo! Web sitesi analizi</span><span class="sxs-lookup"><span data-stu-id="ac00f-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="ac00f-158">StatCounter site</span><span class="sxs-lookup"><span data-stu-id="ac00f-158">StatCounter site</span></span>](http://statcounter.com/)
