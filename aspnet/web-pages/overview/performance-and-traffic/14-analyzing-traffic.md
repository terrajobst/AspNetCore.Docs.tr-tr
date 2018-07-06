---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: ASP.NET Web sayfaları (Razor) sitesinde ziyaretçi bilgi (analiz) izleme | Microsoft Docs
author: tfitzmac
description: Sitenizi giderek yönettiniz sonra Web sitesi trafiğini analiz etmek isteyebilirsiniz.
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 4e065e5223d2f996779ab47de4823962a9aa852e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831102"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="4309b-103">İzleme için bir ASP.NET Web sayfaları (Razor) sitesinde ziyaretçi bilgileri (analiz)</span><span class="sxs-lookup"><span data-stu-id="4309b-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="4309b-104">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4309b-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4309b-105">Bu makalede, bir ASP.NET Web sayfaları (Razor) Web Pages'da Web sitesi analizi eklemek için bir yardımcı kullanmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="4309b-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="4309b-106">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="4309b-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="4309b-107">Nasıl Web sitesi trafiğiniz hakkında bilgi bir analiz sağlayıcısına gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4309b-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="4309b-108">ASP.NET programlama makalesinde sunulan özellikler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="4309b-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="4309b-109">`Analytics` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="4309b-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4309b-110">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="4309b-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4309b-111">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="4309b-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="4309b-112">ASP.NET Web Yardımcıları kitaplığı (NuGet paketi)</span><span class="sxs-lookup"><span data-stu-id="4309b-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="4309b-113">Analytics sitesini nasıl kullandığını anlayın, böylece Web sitenize trafiği ölçer teknolojisi için genel bir terimdir.</span><span class="sxs-lookup"><span data-stu-id="4309b-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="4309b-114">Birçok Analiz Hizmetleri, Google, Yahoo, StatCounter ve diğer hizmetleri dahil olmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4309b-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="4309b-115">İzlemek istediğiniz bir hesap için analiz sağlayıcısı ile site kaydetme yeri, kaydolduktan olduğunu çalıştığını şekilde analiz. Sağlayıcı bir kimliği ya da izleme kodu hesabınız için JavaScript kod parçacığını gönderir.</span><span class="sxs-lookup"><span data-stu-id="4309b-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="4309b-116">İzlemek istediğiniz sitenin web sayfalarında JavaScript kod parçacığını ekleyin. (Genellikle analytics kod parçacığı bir alt bilgi veya düzen sayfası veya sitenizde her sayfada görünen diğer HTML biçimlendirmesi eklediğiniz.) Kullanıcılar bu JavaScript kod parçacıklarının birini içeren bir sayfa istediğinde, kod parçacığı sayfası ile ilgili çeşitli ayrıntılar hakkında bilgi kayıtları analiz sağlayıcısı geçerli sayfa hakkında bilgi gönderir.</span><span class="sxs-lookup"><span data-stu-id="4309b-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="4309b-117">Göz atın, site istatistikleri istediğinizde, analytics sağlayıcının Web sitesine oturum.</span><span class="sxs-lookup"><span data-stu-id="4309b-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="4309b-118">Ardından siteniz hakkında raporlar her türlü gibi görüntüleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4309b-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="4309b-119">Tek tek sayfaları için sayfa görüntüleme sayısı.</span><span class="sxs-lookup"><span data-stu-id="4309b-119">The number of page views for individual pages.</span></span> <span data-ttu-id="4309b-120">Bu, kaç (yaklaşık) kişiler siteyi ziyaret edin ve hangi sayfaların sitenizde en popüler olduğunu bildirir.</span><span class="sxs-lookup"><span data-stu-id="4309b-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="4309b-121">Ne kadar süreyle kişiler, belirli bir sayfa ayırın.</span><span class="sxs-lookup"><span data-stu-id="4309b-121">How long people spend on specific pages.</span></span> <span data-ttu-id="4309b-122">Bu, giriş sayfanızın Halk ilgi olup tutma gibi şeyler söyleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4309b-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="4309b-123">Sitenizi ziyaret önce hangi siteleri kişiler üzerinde yoktu.</span><span class="sxs-lookup"><span data-stu-id="4309b-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="4309b-124">Bu bağlantıları, arama vb. trafiğiniz geldiği olup olmadığını anlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="4309b-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="4309b-125">Kişiler, sitenizi ziyaret ettiğinizde ve ne kadar uzun kalır.</span><span class="sxs-lookup"><span data-stu-id="4309b-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="4309b-126">Hangi ülkelerde ziyaretçilerinizin arasındadır.</span><span class="sxs-lookup"><span data-stu-id="4309b-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="4309b-127">Hangi tarayıcılar ve işletim sistemleri ziyaretçilerinizin kullanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4309b-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="4309b-129">Bir analiz eklemek için bir yardımcı kullanma</span><span class="sxs-lookup"><span data-stu-id="4309b-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="4309b-130">ASP.NET Web sayfaları içeren birkaç analytics Yardımcıları (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, ve `Analytics.GetStatCounterHtml`) kolaylaştıran analizler için kullanılan JavaScript kod parçacıkları yönetmek.</span><span class="sxs-lookup"><span data-stu-id="4309b-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="4309b-131">Nasıl ve nerede çözülmesi yerine JavaScript kodu, tüm yapmanız gereken eklemektir yardımcı bir sayfaya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4309b-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="4309b-132">Sağlamanız gereken yalnızca hesap adınız, kimliği ya da izleme kodu bilgilerdir.</span><span class="sxs-lookup"><span data-stu-id="4309b-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="4309b-133">(StatCounter için Ayrıca bazı ek değerler sağlamanız gerekir.)</span><span class="sxs-lookup"><span data-stu-id="4309b-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="4309b-134">Bu yordamda, kullanan bir düzen sayfası oluşturacaksınız `GetGoogleHtml` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="4309b-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="4309b-135">Diğer analiz sağlayıcılardan birini zaten bir hesabınız varsa, bunun yerine bu hesabı kullanın ve gerektiğinde hafif ayarlamalar yapın.</span><span class="sxs-lookup"><span data-stu-id="4309b-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="4309b-136">Analytics hesabı oluşturmak için izleme yapmak istediğiniz sitenin URL'sini kaydedersiniz.</span><span class="sxs-lookup"><span data-stu-id="4309b-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="4309b-137">Her şeyin yerel bilgisayarınızda test ediyorsanız, (yalnızca trafik şey) gerçek trafik izleme olmaz kaydetmenizi ve görüntülemenizi site istatistikleri mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="4309b-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="4309b-138">Ancak bu yordamı nasıl analiz yardımcı bir sayfaya ekleyin gösterir.</span><span class="sxs-lookup"><span data-stu-id="4309b-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="4309b-139">Sitenizi yayımladığınızda, Canlı site bilgileri, analiz sağlayıcısına gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4309b-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="4309b-140">ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), bunu zaten eklemediniz.</span><span class="sxs-lookup"><span data-stu-id="4309b-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="4309b-141">Bir hesap ile Google Analytics'e oluşturun ve hesap adını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4309b-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="4309b-142">Adlı bir düzen sayfası oluşturma *Analytics.cshtml* ve aşağıdaki işaretlemeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4309b-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="4309b-143">Çağrı yerleştirmelisiniz `Analytics` Yardımcısı, web sayfasının gövdesindeki (önce `</body>` etiketi).</span><span class="sxs-lookup"><span data-stu-id="4309b-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="4309b-144">Aksi takdirde, tarayıcının betiği çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="4309b-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="4309b-145">Farklı bir analiz sağlayıcısı kullanıyorsanız, bunun yerine aşağıdaki Yardımcıları birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="4309b-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="4309b-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="4309b-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="4309b-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="4309b-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="4309b-148">Değiştirin `myaccount` hesabı, kimliği veya 1. adımda oluşturduğunuz izleme kod adı.</span><span class="sxs-lookup"><span data-stu-id="4309b-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="4309b-149">Sayfanın tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4309b-149">Run the page in the browser.</span></span> <span data-ttu-id="4309b-150">(Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.)</span><span class="sxs-lookup"><span data-stu-id="4309b-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="4309b-151">Tarayıcıda, sayfa kaynağı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="4309b-151">In the browser, view the page source.</span></span> <span data-ttu-id="4309b-152">İşlenmiş analytics kodu görmek mümkün olacaktır:</span><span class="sxs-lookup"><span data-stu-id="4309b-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="4309b-153">Google Analytics sitesinde oturum ve istatistikleri sitenizi inceleyin.</span><span class="sxs-lookup"><span data-stu-id="4309b-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="4309b-154">Sayfa Canlı site üzerinde çalıştırıyorsanız, sayfanızı ziyaret günlüğe kaydeden bir girdi görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="4309b-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="4309b-155">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4309b-155">Additional Resources</span></span>

- [<span data-ttu-id="4309b-156">Google Analytics site</span><span class="sxs-lookup"><span data-stu-id="4309b-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="4309b-157">Yahoo! Web sitesi analizi</span><span class="sxs-lookup"><span data-stu-id="4309b-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="4309b-158">StatCounter site</span><span class="sxs-lookup"><span data-stu-id="4309b-158">StatCounter site</span></span>](http://statcounter.com/)
