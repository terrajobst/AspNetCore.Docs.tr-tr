---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Önbelleğe alınmış bir sayfaya (VB) için dinamik içerik ekleme | Microsoft Docs
author: microsoft
description: Aynı sayfayı dinamik ve önbelleğe alınan içeriğini karıştırmak öğrenin. Sonrası önbellek değiştirme başlığı tanıtımları o gibi dinamik içerik görüntülenecek sağlar...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 121a3a35c8255f1423d7008930315f76bbb8e8f9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756512"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="92bae-104">Önbelleğe alınmış bir sayfaya (VB) için dinamik içerik ekleme</span><span class="sxs-lookup"><span data-stu-id="92bae-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="92bae-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="92bae-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="92bae-106">Aynı sayfayı dinamik ve önbelleğe alınan içeriğini karıştırmak öğrenin.</span><span class="sxs-lookup"><span data-stu-id="92bae-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="92bae-107">Sonrası önbellek değiştirme başlık reklamları veya, önbelleğe alınan çıkış bir sayfa içinde olan haber öğelerinin gibi dinamik içerik görüntülemenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="92bae-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="92bae-108">Çıkış önbelleğe alma özelliğinden yararlanarak, bir ASP.NET MVC uygulamasının performansını önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="92bae-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="92bae-109">Sayfa istenen her zaman bir sayfa üretmek yerine, sayfanın bir kez oluşturulabilir ve birden çok kullanıcı için bellekte önbelleğe alınmış.</span><span class="sxs-lookup"><span data-stu-id="92bae-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="92bae-110">Ancak bir sorun oluştu.</span><span class="sxs-lookup"><span data-stu-id="92bae-110">But there is a problem.</span></span> <span data-ttu-id="92bae-111">Peki sayfasında dinamik içerikleri görüntülemek gerekiyor?</span><span class="sxs-lookup"><span data-stu-id="92bae-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="92bae-112">Örneğin, bir reklam sayfasında görüntülemek istediğinizi düşünün.</span><span class="sxs-lookup"><span data-stu-id="92bae-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="92bae-113">Her kullanıcı aynı tanıtım görebilmesi için önbelleğe alınacak reklam istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="92bae-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="92bae-114">Bu şekilde paranın sabitlenebilen!</span><span class="sxs-lookup"><span data-stu-id="92bae-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="92bae-115">Neyse ki, kolay bir çözüm yoktur.</span><span class="sxs-lookup"><span data-stu-id="92bae-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="92bae-116">Bir özelliğin çağırılır ASP.NET framework'ün yararlanabilirsiniz *sonrası, değiştirme önbelleğe*.</span><span class="sxs-lookup"><span data-stu-id="92bae-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="92bae-117">Sonrası önbellek değiştirme, bellekte önbelleğe alınmış bir sayfaya dinamik içerik yerine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="92bae-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="92bae-118">Çıktı alırken genellikle bir sayfasını kullanarak önbelleğe alma &lt;OutputCache&gt; hem sunucu hem de istemci (tarayıcı) özniteliği, sayfanın önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="92bae-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="92bae-119">Bir sayfa, sonrası önbellek değiştirme kullandığınızda, yalnızca sunucuda önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="92bae-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="92bae-120">Sonrası önbellek değiştirme kullanma</span><span class="sxs-lookup"><span data-stu-id="92bae-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="92bae-121">Sonrası önbellek değiştirme kullanarak iki adımı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="92bae-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="92bae-122">İlk olarak, önbelleğe alınan sayfasında görüntülenmesini istediğiniz dinamik içeriği temsil eden bir dize döndüren bir yöntemi tanımlamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="92bae-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="92bae-123">Ardından, sayfaya dinamik içerik eklemesine HttpResponse.WriteSubstitution() yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="92bae-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="92bae-124">Örneğin, rastgele önbelleğe alınmış bir sayfaya farklı haber öğelerini görüntülemek istediğinizi düşünün.</span><span class="sxs-lookup"><span data-stu-id="92bae-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="92bae-125">1 listeleme sınıfı, rastgele bir haber öğesi üç haber öğelerinin bir listeden döndüren RenderNews() adlı tek bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="92bae-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="92bae-126">**1 – Models\News.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="92bae-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="92bae-127">Sonrası önbellek değiştirme yararlanmak için HttpResponse.WriteSubstitution() yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="92bae-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="92bae-128">WriteSubstitution() yöntemi kodu bölgesi önbelleğe alınmış bir sayfaya dinamik içerik ile değiştirmek için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="92bae-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="92bae-129">WriteSubstitution() yöntemin rastgele haber öğesinin 2 liste görünümünde görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="92bae-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="92bae-130">**2 – Views\Home\Index.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="92bae-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="92bae-131">RenderNews yöntemi WriteSubstitution() yöntemine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="92bae-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="92bae-132">RenderNews yöntemi çağrılmadı dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="92bae-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="92bae-133">Bunun yerine bir yönteme başvuru için WriteSubstitution() AddressOf işleci yardımıyla geçirilir.</span><span class="sxs-lookup"><span data-stu-id="92bae-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="92bae-134">Dizin görünümünün önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="92bae-134">The Index view is cached.</span></span> <span data-ttu-id="92bae-135">Görünüm denetleyicisi listeleme 3'te tarafından döndürülür.</span><span class="sxs-lookup"><span data-stu-id="92bae-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="92bae-136">İNDİS() eylem ile donatılmış bildirimi bir &lt;OutputCache&gt; dizin görünümünün 60 saniye boyunca önbelleğe alınmasına neden olan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="92bae-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="92bae-137">**3 – Controllers\HomeController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="92bae-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="92bae-138">Dizin görünümünün önbelleğe alınmış olsa da, dizin sayfası istediğinde farklı rastgele haber öğeler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="92bae-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="92bae-139">Dizin Sayfası istediğinde 60 saniye (bkz. Şekil 1) sayfa tarafından görüntülenen zaman değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="92bae-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="92bae-140">Sayfanın önbelleğe alma süresi değişmez olgu kanıtlar.</span><span class="sxs-lookup"><span data-stu-id="92bae-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="92bae-141">Ancak, içerik eklenen her istekle WriteSubstitution() yöntemi – rastgele haber öğesinin – değişikliklerden.</span><span class="sxs-lookup"><span data-stu-id="92bae-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="92bae-142">**Şekil 1-önbelleğe alınmış bir sayfaya dinamik haber öğeleri ekleme**</span><span class="sxs-lookup"><span data-stu-id="92bae-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="92bae-144">Sonrası önbellek değiştirme yardımcı yöntemleri kullanma</span><span class="sxs-lookup"><span data-stu-id="92bae-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="92bae-145">Bir özel bir yardımcı yöntem içinde WriteSubstitution() yöntemine çağrı yalıtılacak sonrası önbellek değiştirme yararlanmak için daha kolay bir yolu var.</span><span class="sxs-lookup"><span data-stu-id="92bae-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="92bae-146">Bu yaklaşım listeleme 4'te yardımcı yöntemi gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="92bae-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="92bae-147">**4 – Helpers\AdHelper.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="92bae-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="92bae-148">4 listeleme içeren iki yöntemlerini gösteren bir Visual Basic module'u: RenderBanner() ve RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="92bae-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="92bae-149">RenderBanner() yöntemi gerçek yardımcı yöntemi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="92bae-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="92bae-150">Bu yöntem, herhangi bir yardımcı yöntemi gibi bir görünümde Html.RenderBanner() çağırabilirsiniz standart ASP.NET MVC HtmlHelper sınıfı genişletir.</span><span class="sxs-lookup"><span data-stu-id="92bae-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="92bae-151">RenderBanner() yöntem RenderBannerInternal() yöntemi WriteSubsitution() yöntemine geçirerek HttpResponse.WriteSubstitution() yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="92bae-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="92bae-152">Özel bir yöntem RenderBannerInternal() yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="92bae-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="92bae-153">Bu yöntem, bir yardımcı yöntem sunulmamasını.</span><span class="sxs-lookup"><span data-stu-id="92bae-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="92bae-154">RenderBannerInternal() yöntemi, üç başlığı reklam görüntü listesinden rastgele bir başlık tanıtım görüntüsünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="92bae-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="92bae-155">Listeleme 5'te değiştirilmiş dizin görünümünün RenderBanner() yardımcı yöntemini nasıl kullanabileceğinizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="92bae-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="92bae-156">Dikkat ek &lt;içeri aktarma % @ %&gt; yönergesi MvcApplication1.Helpers ad alanı içeri aktarmak için görünümün en üstünde bulunur.</span><span class="sxs-lookup"><span data-stu-id="92bae-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="92bae-157">Bu ad alanı içeri aktarmak yayımlarsanız, RenderBanner() yöntemi Html özelliği üzerinde bir yöntem olarak görünmez.</span><span class="sxs-lookup"><span data-stu-id="92bae-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="92bae-158">**5-Views\Home\Index.aspx (RenderBanner() yöntemiyle) listeleme**</span><span class="sxs-lookup"><span data-stu-id="92bae-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="92bae-159">Farklı bir reklam listesi 5 görünüm tarafından işlenen sayfa istediğinde, her bir istekle görüntülenir (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="92bae-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="92bae-160">Sayfanın önbelleğe alınır ancak reklam RenderBanner() yardımcı yöntemi tarafından dinamik olarak eklenmiş olur.</span><span class="sxs-lookup"><span data-stu-id="92bae-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="92bae-161">**Şekil 2 – rastgele bir reklam görüntüleme dizini görünümü**</span><span class="sxs-lookup"><span data-stu-id="92bae-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="92bae-163">Özet</span><span class="sxs-lookup"><span data-stu-id="92bae-163">Summary</span></span>

<span data-ttu-id="92bae-164">Bu öğreticide, içeriği önbelleğe alınmış bir sayfaya dinamik olarak nasıl güncelleştirebilirsiniz açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="92bae-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="92bae-165">Dinamik içeriği önbelleğe alınmış bir sayfa eklenerek üzere etkinleştirmek için HttpResponse.WriteSubstitution() yöntemi kullanmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="92bae-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="92bae-166">Ayrıca bir HTML yardımcı yöntem içinde WriteSubstitution() yöntemine çağrı kapsülleyen öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="92bae-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="92bae-167">Mümkün olduğunda – web uygulamalarınızın performansını önemli bir etkisi olması önbelleğe alma avantajından faydalanın.</span><span class="sxs-lookup"><span data-stu-id="92bae-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="92bae-168">Bu öğreticide açıklandığı şekilde, hatta sayfalarınıza dinamik içerikleri görüntülemek, ihtiyacınız olduğunda, önbelleğe alma yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="92bae-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="92bae-169">[Önceki](improving-performance-with-output-caching-vb.md)
> [İleri](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="92bae-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>
