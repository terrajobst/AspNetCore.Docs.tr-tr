---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: "Dinamik içerik önbelleğe alınmış bir sayfa için (VB) ekleme | Microsoft Docs"
author: microsoft
description: "Dinamik ve önbelleğe alınmış içeriği aynı sayfada karışık öğrenin. Sonrası önbellek değiştirme başlık reklamları o gibi dinamik içerik görüntülemenize olanak sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f07f4ecec36e71679dbc471b65f26d260349a07e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="f1f30-104">Dinamik içerik önbelleğe alınmış bir sayfa için (VB) ekleme</span><span class="sxs-lookup"><span data-stu-id="f1f30-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="f1f30-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f1f30-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f1f30-106">Dinamik ve önbelleğe alınmış içeriği aynı sayfada karışık öğrenin.</span><span class="sxs-lookup"><span data-stu-id="f1f30-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="f1f30-107">Sonrası önbellek değiştirme başlık reklamları veya önbelleğe alınmış çıktı bir sayfa içinde haber öğeleri gibi dinamik içerik görüntülemenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="f1f30-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="f1f30-108">Çıkış önbelleğe alma yararlanarak, bir ASP.NET MVC uygulamanızın performansını önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="f1f30-109">Sayfa istenen her zaman bir sayfa üretmek yerine sayfa kez oluşturulabilir ve birden çok kullanıcı için bellekte önbelleğe alınmış.</span><span class="sxs-lookup"><span data-stu-id="f1f30-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="f1f30-110">Ancak bir sorun oluştu.</span><span class="sxs-lookup"><span data-stu-id="f1f30-110">But there is a problem.</span></span> <span data-ttu-id="f1f30-111">Ne dinamik içerik sayfasında görüntülenecek gerekiyor?</span><span class="sxs-lookup"><span data-stu-id="f1f30-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="f1f30-112">Örneğin, bir reklam sayfasında görüntülemek istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="f1f30-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="f1f30-113">Her kullanıcının çok aynı tanıtım görebilmesi için önbelleğe alınacak reklam istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="f1f30-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="f1f30-114">Bu şekilde paranın olun olmayacaktır!</span><span class="sxs-lookup"><span data-stu-id="f1f30-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="f1f30-115">Neyse ki, kolay bir çözüm yoktur.</span><span class="sxs-lookup"><span data-stu-id="f1f30-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="f1f30-116">Bir özellik olarak adlandırılan ASP.NET framework'ün yararlanabilir *değiştirme'sonrası önbelleğe*.</span><span class="sxs-lookup"><span data-stu-id="f1f30-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="f1f30-117">Sonrası önbellek değiştirme bellekte önbelleğe alınmış bir sayfasında dinamik içerik yerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="f1f30-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="f1f30-118">Çıktı alırken normalde, bir sayfayı kullanarak önbelleğe &lt;OutputCache&gt; hem sunucu hem de istemci (web tarayıcısı) özniteliği, sayfanın önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="f1f30-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="f1f30-119">Bir sayfa sonrası önbellek değiştirme kullandığınızda, yalnızca sunucuda önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="f1f30-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="f1f30-120">Sonrası önbellek değiştirme kullanma</span><span class="sxs-lookup"><span data-stu-id="f1f30-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="f1f30-121">Sonrası önbellek değiştirme kullanarak iki adımı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="f1f30-122">İlk olarak, önbelleğe alınan sayfasında görüntülenmesini istediğiniz dinamik içerik temsil eden bir dize döndüren bir yöntem tanımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="f1f30-123">Ardından, dinamik içerik sayfaya eklemesine HttpResponse.WriteSubstitution() yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="f1f30-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="f1f30-124">Örneğin, rastgele bir önbelleğe alınan sayfasında farklı haber öğeleri görüntülemek istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="f1f30-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="f1f30-125">Listeleme 1 sınıfında rastgele üç haber öğe listesinden bir haber öğesi döndüren RenderNews() adlı tek bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="f1f30-126">**1 – Models\News.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="f1f30-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="f1f30-127">Sonrası önbellek değiştirme yararlanmak için HttpResponse.WriteSubstitution() yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="f1f30-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="f1f30-128">Önbelleğe alınan sayfasının bir bölge ile dinamik içerik değiştirmek için kodu WriteSubstitution() yöntemini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f1f30-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="f1f30-129">WriteSubstitution() yöntemi listeleme 2 görünümünde rastgele haber öğesini görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f1f30-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="f1f30-130">**2 – Views\Home\Index.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="f1f30-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="f1f30-131">RenderNews yöntemi WriteSubstitution() yöntemine iletilir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="f1f30-132">RenderNews yöntemi çağrılmazsa dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f1f30-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="f1f30-133">Bunun yerine yöntemi başvuru için WriteSubstitution() AddressOf işleci yardımıyla geçirilir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="f1f30-134">Dizin görünümünün önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="f1f30-134">The Index view is cached.</span></span> <span data-ttu-id="f1f30-135">Görünüm listeleme 3'te denetleyicisi tarafından döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f1f30-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="f1f30-136">İNDİS() eylem ile donatılmış bildirimi bir &lt;OutputCache&gt; dizin görünümünün 60 saniye boyunca önbelleğe alınmasına neden olan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f1f30-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="f1f30-137">**3 – Controllers\HomeController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="f1f30-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="f1f30-138">Dizin görünümünün önbelleğe olsa da, dizin sayfası istediğinde farklı rastgele haber öğeleri görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="f1f30-139">Dizin Sayfası istediğinde, sayfa tarafından görüntülenen zaman 60 saniye (bkz: Şekil 1) değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="f1f30-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="f1f30-140">Süre değişmez olgu sayfası önbelleğe alınır kanıtlar.</span><span class="sxs-lookup"><span data-stu-id="f1f30-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="f1f30-141">Ancak, içerik eklenen her istek ile WriteSubstitution() yöntemi – rastgele haber öğesi – değişikliklerden.</span><span class="sxs-lookup"><span data-stu-id="f1f30-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="f1f30-142">**Şekil 1 – önbelleğe alınmış bir sayfa dinamik haber öğeleri Injecting**</span><span class="sxs-lookup"><span data-stu-id="f1f30-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="f1f30-144">Sonrası önbellek değiştirme yardımcı yöntemleri kullanma</span><span class="sxs-lookup"><span data-stu-id="f1f30-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="f1f30-145">Bir özel yardımcı yöntem içinde WriteSubstitution() yöntemine yapılan çağrı kapsülleyen sonrası önbellek değiştirme yararlanmak için daha kolay bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="f1f30-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="f1f30-146">Bu yaklaşım listeleme 4 yardımcı yöntemi tarafından gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="f1f30-147">**4 – Helpers\AdHelper.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="f1f30-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="f1f30-148">4 listeleme içeren bir Visual Basic modülü iki yöntem sunar: RenderBanner() ve RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="f1f30-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="f1f30-149">RenderBanner() yöntemi gerçek yardımcı yöntemi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f1f30-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="f1f30-150">Böylece bir görünümde herhangi bir yardımcı yöntemi gibi Html.RenderBanner() çağırabilirsiniz bu yöntem standart ASP.NET MVC HtmlHelper sınıfını genişletir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="f1f30-151">RenderBanner() yöntemi RenderBannerInternal() yöntemi WriteSubsitution() yönteme geçirme HttpResponse.WriteSubstitution() yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="f1f30-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="f1f30-152">RenderBannerInternal() yöntemi özel bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="f1f30-153">Bu yöntem, bir yardımcı yöntem olarak açık olmaz.</span><span class="sxs-lookup"><span data-stu-id="f1f30-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="f1f30-154">RenderBannerInternal() yöntemi, üç başlık tanıtım görüntüleri listesinden rastgele bir başlık tanıtım görüntüsünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="f1f30-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="f1f30-155">Listeleme 5 değiştirilmiş dizin görünümünde RenderBanner() yardımcı yönteminin nasıl kullanabileceğinizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="f1f30-156">Dikkat ek bir &lt;% @ alma %&gt; yönergesi MvcApplication1.Helpers ad alanını almak için görünümü üstünde bulunur.</span><span class="sxs-lookup"><span data-stu-id="f1f30-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="f1f30-157">Bu ad alanı içe atamazsanız RenderBanner() yöntemi Html özelliği bir yöntem olarak görünmez.</span><span class="sxs-lookup"><span data-stu-id="f1f30-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="f1f30-158">**5 – Views\Home\Index.aspx (RenderBanner() yöntemiyle) listeleme**</span><span class="sxs-lookup"><span data-stu-id="f1f30-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="f1f30-159">Listeleme 5 görünüm tarafından işlenen sayfanın istediğinde, farklı bir reklam her istek ile gösterilir (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="f1f30-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="f1f30-160">Sayfanın önbelleğe alınır, ancak reklam RenderBanner() yardımcı yöntemi tarafından dinamik olarak eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="f1f30-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="f1f30-161">**Şekil 2 – rastgele bir reklam görüntüleme dizini görünümü**</span><span class="sxs-lookup"><span data-stu-id="f1f30-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="f1f30-163">Özet</span><span class="sxs-lookup"><span data-stu-id="f1f30-163">Summary</span></span>

<span data-ttu-id="f1f30-164">Bu öğretici, önbelleğe alınmış bir sayfa içeriği nasıl dinamik olarak güncelleştirebilirsiniz açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="f1f30-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="f1f30-165">Önbelleğe alınmış bir sayfa eklenemeyebilir dinamik içeriği etkinleştirmek için HttpResponse.WriteSubstitution() yöntemini kullanmak üzere öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="f1f30-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="f1f30-166">Ayrıca bir HTML yardımcı yöntemi içinde WriteSubstitution() yöntemine yapılan çağrı kapsülleyen öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="f1f30-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="f1f30-167">Mümkün olduğunda – web uygulamalarınızın performansını önemli bir etkisi olması önbelleğe alma yararlanabilir.</span><span class="sxs-lookup"><span data-stu-id="f1f30-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="f1f30-168">Bu öğreticide açıklandığı gibi hatta sayfalarınızda dinamik içeriği görüntülemek gerektiğinde önbelleğe alma yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f1f30-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f1f30-169">[Önceki](improving-performance-with-output-caching-vb.md)
[sonraki](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f1f30-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>
