---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: ASP.NET MVC denetleyicisine genel bakış (VB) | Microsoft Docs
author: StephenWalther
description: Bu öğreticide, Stephen Walther için ASP.NET MVC denetleyicileri sunar. Yeni denetleyicileri oluşturun ve eylem res farklı türde döndürmek öğrenin...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 604bf4af2a46e56d9445de141fae1a1651acf47f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754688"
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="d8a5a-104">ASP.NET MVC denetleyicisine genel bakış (VB)</span><span class="sxs-lookup"><span data-stu-id="d8a5a-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="d8a5a-105">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="d8a5a-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="d8a5a-106">Bu öğreticide, Stephen Walther için ASP.NET MVC denetleyicileri sunar.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="d8a5a-107">Yeni denetleyicileri oluşturun ve farklı türde eylem sonuçlarını döndürmek öğrenin.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="d8a5a-108">Bu öğretici, ASP.NET MVC denetleyicileri, denetleyici eylemlerini ve eylem sonuçlarını konu açıklar.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="d8a5a-109">Bu öğreticiyi tamamladıktan sonra denetleyicileri ziyaretçi bir ASP.NET MVC Web sitesi ile etkileşim şeklini denetlemek için nasıl kullanılacağını anlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="d8a5a-110">Denetleyicileri anlama</span><span class="sxs-lookup"><span data-stu-id="d8a5a-110">Understanding Controllers</span></span>

<span data-ttu-id="d8a5a-111">MVC denetleyicileri karşı bir ASP.NET MVC Web sitesine yapılan isteklerini yanıtlamadan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="d8a5a-112">Her bir tarayıcı isteğini belirli bir denetleyiciye eşlenir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="d8a5a-113">Örneğin, tarayıcınızın adres çubuğuna aşağıdaki URL'yi girin düşünün:</span><span class="sxs-lookup"><span data-stu-id="d8a5a-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="d8a5a-114">Bu durumda, ProductController adlı bir denetleyici çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="d8a5a-115">ProductController tarayıcı isteğin yanıtını oluşturmaktan sorumlu.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="d8a5a-116">Örneğin, denetleyici, belirli bir görünüm tarayıcıya geri döndürebilir veya denetleyici kullanıcı başka bir denetleyiciye yönlendirebilir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="d8a5a-117">1 listeleme ProductController adlı basit bir denetleyici içerir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="d8a5a-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="d8a5a-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="d8a5a-119">Listeleme 1'den görebileceğiniz gibi yalnızca bir sınıf (Visual Basic .NET veya C# sınıfı) bir denetleyicisi değil.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="d8a5a-120">Temel System.Web.Mvc.Controller sınıfından türetilen bir sınıf denetleyicisidir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="d8a5a-121">Bir denetleyici bu temel sınıftan devraldığı için bir denetleyici birçok kullanışlı yöntem ücretsiz devralır (Bu yöntemleri birazdan ele alır).</span><span class="sxs-lookup"><span data-stu-id="d8a5a-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="d8a5a-122">Denetleyici eylemlerini anlama</span><span class="sxs-lookup"><span data-stu-id="d8a5a-122">Understanding Controller Actions</span></span>

<span data-ttu-id="d8a5a-123">Bir denetleyici denetleyici eylemleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-123">A controller exposes controller actions.</span></span> <span data-ttu-id="d8a5a-124">Bir eylem, belirli bir URL'sini tarayıcınızın adres çubuğuna girdiğinizde çağrılan denetleyicisinde bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="d8a5a-125">Örneğin, aşağıdaki URL için bir istekte bulunmak düşünün:</span><span class="sxs-lookup"><span data-stu-id="d8a5a-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="d8a5a-126">Bu durumda, İNDİS() yöntem ProductController sınıf üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="d8a5a-127">İNDİS() yöntemi, denetleyici eylem örneğidir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="d8a5a-128">Bir denetleyici eylemi bir denetleyici sınıfının genel bir yöntemi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="d8a5a-129">Varsayılan olarak, Visual Basic.NET yöntemler, genel yöntemlerdir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="d8a5a-130">Bir denetleyici sınıfına ekleyin herhangi bir genel yöntemini bir denetleyici eylem olarak otomatik olarak kullanıma sunulduğunu unutmayın (bir denetleyici eylemi evreni içinde hiç kimse tarafından yalnızca bir tarayıcı adres çubuğuna sağ URL yazarak çağrılabilir olduğundan bu hakkında dikkatli olmanız gerekir).</span><span class="sxs-lookup"><span data-stu-id="d8a5a-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="d8a5a-131">Bir denetleyici eylemi tarafından karşılanması gereken bazı ek gereksinimleri vardır.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="d8a5a-132">Bir denetleyici eylemi kullanılan bir yöntem aşırı yüklenemez.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="d8a5a-133">Ayrıca, bir denetleyici eylemi bir statik yöntem olamaz.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="d8a5a-134">Bir denetleyici eylemi neredeyse tüm yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="d8a5a-135">Eylem sonuçlarını anlama</span><span class="sxs-lookup"><span data-stu-id="d8a5a-135">Understanding Action Results</span></span>

<span data-ttu-id="d8a5a-136">Bir denetleyici eylemi olarak adlandırılan bir şey döndürür bir *eylem sonucu*.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="d8a5a-137">Eylem sonucu bir denetleyici eylemi bir tarayıcı isteğine yanıt olarak döndürür ' dir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="d8a5a-138">ASP.NET MVC çerçevesi, eylem sonuçlarını da dahil olmak üzere çeşitli türlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="d8a5a-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="d8a5a-139">ViewResult - temsil HTML ve biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="d8a5a-140">EmptyResult - hiçbir sonucu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="d8a5a-141">RedirectResult - yeni bir URL yeniden yönlendirme temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="d8a5a-142">JsonResult - AJAX uygulamada kullanılabilecek bir JavaScript nesne gösterimi sonucu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="d8a5a-143">JavaScriptResult - JavaScript komut dosyasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="d8a5a-144">ContentResult - metin sonucu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="d8a5a-145">FileContentResult - (ikili içerikle) indirilebilir bir dosyayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="d8a5a-146">FilePathResult - (yoluyla) indirilebilir bir dosyayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="d8a5a-147">FileStreamResult - (ile bir dosya akışı) indirilebilir bir dosyayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="d8a5a-148">Tüm bu eylem sonuçlarını temel ActionResult sınıfından devralır.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="d8a5a-149">Çoğu durumda, bir denetleyici eylemi bir ViewResult döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="d8a5a-150">Örneğin, 2 listeleme dizin denetleyici eylemi bir ViewResult döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="d8a5a-151">**2 - Controllers\BookController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="d8a5a-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="d8a5a-152">Bir eylem bir ViewResult geri döndüğünde, tarayıcıya HTML döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="d8a5a-153">Listeleme 2 İNDİS() yöntemi tarayıcıya dizin adlı bir görünüm verir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="d8a5a-154">Listeleme 2 İNDİS() eylemi bir ViewResult() döndürmeyen dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="d8a5a-155">Bunun yerine, denetleyici temel sınıfın View() yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="d8a5a-156">Normalde, bir eylem sonucu doğrudan gitmez.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="d8a5a-157">Bunun yerine, denetleyici temel sınıf aşağıdaki yöntemlerden birini arayın:</span><span class="sxs-lookup"><span data-stu-id="d8a5a-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="d8a5a-158">Görüntüleme - ViewResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="d8a5a-159">Yeniden yönlendirme - RedirectResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="d8a5a-160">RedirectToAction - RedirectToRouteResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="d8a5a-161">RedirectToRoute - RedirectToRouteResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="d8a5a-162">JSON - JsonResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="d8a5a-163">JavaScriptResult - bir JavaScriptResult döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="d8a5a-164">İçerik - ContentResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="d8a5a-165">Dosya - yönteme bir FileContentResult, FilePathResult veya FileStreamResult parametreleri bağlı olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="d8a5a-166">Bu nedenle, tarayıcıya bir görünüme dönmek istiyorsanız, View() yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="d8a5a-167">Bir denetleyici eylem kullanıcıya yönlendirmek istiyorsanız, RedirectToAction() yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="d8a5a-168">Örneğin, Details() eylem listeleme 3'te bir görünüm görüntüler veya kullanıcı ID parametresine bir değer olup olmadığına bağlı olarak İNDİS() eyleme yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="d8a5a-169">**3 - CustomerController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="d8a5a-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="d8a5a-170">ContentResult eylem sonucu özeldir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-170">The ContentResult action result is special.</span></span> <span data-ttu-id="d8a5a-171">Eylem sonucu düz metin olarak döndürülecek ContentResult eylem sonucunu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="d8a5a-172">Örneğin, 4 listeleme İNDİS() yöntemi HTML değil de, düz metin olarak bir ileti döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="d8a5a-173">**4 - Controllers\StatusController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="d8a5a-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="d8a5a-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="d8a5a-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="d8a5a-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="d8a5a-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="d8a5a-176">StatusController.Index() eylem çağrıldığında, görünümü döndürülmez.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="d8a5a-177">Bunun yerine, ham metni "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d8a5a-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="d8a5a-178">tarayıcıya döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-178">is returned to the browser.</span></span>

<span data-ttu-id="d8a5a-179">Ardından bir denetleyici eylemi eylem sonucunu değil - Örneğin, bir sonuç bir tarih veya bir tamsayı - döndürürse, sonuç bir ContentResult içinde otomatik olarak paketlenir.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="d8a5a-180">Örneğin, 5 listeleme WorkController İNDİS() eylemi çağrıldığında tarih bir ContentResult otomatik olarak döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="d8a5a-181">**5 - WorkController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="d8a5a-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="d8a5a-182">Listeleme 5 İNDİS() eylemi, bir DateTime nesnesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="d8a5a-183">ASP.NET MVC çerçevesi, DateTime nesnesi bir dizeye dönüştürür ve tarih saat değeri bir ContentResult otomatik olarak sarmalar.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="d8a5a-184">Tarayıcı, tarih ve saat düz metin olarak alır.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="d8a5a-185">Özet</span><span class="sxs-lookup"><span data-stu-id="d8a5a-185">Summary</span></span>

<span data-ttu-id="d8a5a-186">Bu öğreticide, ASP.NET MVC denetleyicileri, denetleyici eylemlerini ve denetleyici eylem sonuçlarını kavramlara tanıtmak için oluştu.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="d8a5a-187">Bu bölümde, ASP.NET MVC projesinde yeni denetleyicileri ekleme öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="d8a5a-188">Ardından, etki alanı denetleyicisinin nasıl genel yöntemleri öğrendiğiniz evrenimize Hoş denetleyici eylemleri sunulur.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="d8a5a-189">Son olarak, farklı türde bir denetleyici eylemi döndürülen eylem sonuçlarını ele almıştık.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="d8a5a-190">Özellikle, bir ViewResult RedirectToActionResult ve ContentResult bir denetleyici eylemi dönüş nasıl ele almıştık.</span><span class="sxs-lookup"><span data-stu-id="d8a5a-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d8a5a-191">[Önceki](creating-a-custom-route-constraint-cs.md)
> [İleri](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d8a5a-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
