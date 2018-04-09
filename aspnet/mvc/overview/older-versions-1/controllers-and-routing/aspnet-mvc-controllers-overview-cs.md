---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: ASP.NET MVC denetleyicisi genel bakış (C#) | Microsoft Docs
author: StephenWalther
description: Bu öğreticide, Stephen Walther ASP.NET MVC denetleyicisi sunar. Yeni denetleyicileri oluşturun ve eylem res farklı türlerde iade hakkında bilgi edinin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 95e7c555a52c8c3b765a6fffab15276491cf5714
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-controller-overview-c"></a><span data-ttu-id="41e8d-104">ASP.NET MVC denetleyicisi genel bakış (C#)</span><span class="sxs-lookup"><span data-stu-id="41e8d-104">ASP.NET MVC Controller Overview (C#)</span></span>
====================
<span data-ttu-id="41e8d-105">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="41e8d-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="41e8d-106">Bu öğreticide, Stephen Walther ASP.NET MVC denetleyicisi sunar.</span><span class="sxs-lookup"><span data-stu-id="41e8d-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="41e8d-107">Yeni denetleyicileri oluşturun ve dönüş eylem sonuçlarını farklı türleri hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="41e8d-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="41e8d-108">Bu öğreticide ASP.NET MVC denetleyicileri, denetleyici eylemleri ve eylem sonuçlarını konu araştırır.</span><span class="sxs-lookup"><span data-stu-id="41e8d-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="41e8d-109">Bu öğreticiyi tamamladıktan sonra denetleyicileri ziyaretçisi bir ASP.NET MVC Web sitesi ile etkileşime giren şeklini denetlemek için kullanılan nasıl anlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="41e8d-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="41e8d-110">Denetleyicileri anlama</span><span class="sxs-lookup"><span data-stu-id="41e8d-110">Understanding Controllers</span></span>

<span data-ttu-id="41e8d-111">MVC denetleyicileri karşı bir ASP.NET MVC Web isteklerini yanıtlamak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="41e8d-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="41e8d-112">Her tarayıcı isteğini belirli bir denetleyiciye eşlenir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="41e8d-113">Örneğin, tarayıcınızın adres çubuğuna aşağıdaki URL'yi girin düşünün:</span><span class="sxs-lookup"><span data-stu-id="41e8d-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="41e8d-114">Bu durumda, ProductController adlı bir denetleyicisi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="41e8d-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="41e8d-115">ProductController tarayıcı isteğinin yanıtı oluşturmak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="41e8d-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="41e8d-116">Örneğin, denetleyici belirli bir görünüm tarayıcıya geri döndürebilir veya denetleyicisi kullanıcı başka bir denetleyiciye yönlendirebilir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="41e8d-117">Kod 1 ProductController adlı basit bir denetleyicisi içerir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="41e8d-118">**Listing1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="41e8d-118">**Listing1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

<span data-ttu-id="41e8d-119">Listeleme 1'den görebileceğiniz gibi bir sınıf yalnızca (Visual Basic .NET veya C# sınıfı) denetleyicisidir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="41e8d-120">Temel System.Web.Mvc.Controller sınıfından türeyen bir sınıf denetleyicisidir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="41e8d-121">Bir denetleyici bu taban sınıfından devralıyor çünkü bir denetleyici çeşitli kullanışlı yöntemler ücretsiz devralır (biz bu yöntemler bir dakika içinde ele alınmıştır).</span><span class="sxs-lookup"><span data-stu-id="41e8d-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="41e8d-122">Denetleyici eylemlerini anlama</span><span class="sxs-lookup"><span data-stu-id="41e8d-122">Understanding Controller Actions</span></span>

<span data-ttu-id="41e8d-123">Bir denetleyici denetleyici eylemleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-123">A controller exposes controller actions.</span></span> <span data-ttu-id="41e8d-124">Bir eylem, belirli bir URL tarayıcınızın adres çubuğuna girdiğinizde çağrılan bir denetleyicisinde bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="41e8d-125">Örneğin, aşağıdaki URL için bir istek olun düşünün:</span><span class="sxs-lookup"><span data-stu-id="41e8d-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="41e8d-126">Bu durumda, İNDİS() yöntemi ProductController sınıf üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="41e8d-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="41e8d-127">İNDİS() yöntemi, denetleyici eylem örneğidir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="41e8d-128">Bir denetleyici eylemi bir denetleyici sınıfının genel yöntem olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="41e8d-129">C#, varsayılan olarak, özel yöntemler yöntemleridir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-129">C# methods, by default, are private methods.</span></span> <span data-ttu-id="41e8d-130">Bir denetleyici sınıfına ekleyin herhangi bir genel yöntemini bir denetleyici eylemi otomatik olarak sunulur unutmayın (bir denetleyici eylemi universe herhangi biri tarafından yalnızca bir tarayıcınızın adres çubuğuna sağ URL yazarak çağrılabilir olduğundan, bu konuda dikkatli olmalıdır).</span><span class="sxs-lookup"><span data-stu-id="41e8d-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="41e8d-131">Bir denetleyici eylemi tarafından karşılanması gereken bazı ek gereksinimleri vardır.</span><span class="sxs-lookup"><span data-stu-id="41e8d-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="41e8d-132">Bir denetleyici eylemi kullanılan bir yöntem aşırı yüklenemez.</span><span class="sxs-lookup"><span data-stu-id="41e8d-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="41e8d-133">Ayrıca, bir denetleyici eylemi bir statik yöntem olamaz.</span><span class="sxs-lookup"><span data-stu-id="41e8d-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="41e8d-134">Başka bir denetleyici eylemi neredeyse tüm yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="41e8d-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="41e8d-135">Eylem sonuçlarını anlama</span><span class="sxs-lookup"><span data-stu-id="41e8d-135">Understanding Action Results</span></span>

<span data-ttu-id="41e8d-136">Bir denetleyici eylemi olarak adlandırılan bir şey döndürüyor bir *eylem sonucu*.</span><span class="sxs-lookup"><span data-stu-id="41e8d-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="41e8d-137">Bir eylem sonucu bir denetleyici eylemi bir tarayıcı isteğine yanıt olarak döndürür ' dir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="41e8d-138">ASP.NET MVC çerçevesi, eylem sonuçlarını dahil olmak üzere birkaç türlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="41e8d-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="41e8d-139">ViewResult - temsil HTML ve biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="41e8d-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="41e8d-140">EmptyResult - sonuç temsil eder.</span><span class="sxs-lookup"><span data-stu-id="41e8d-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="41e8d-141">RedirectResult - yeni bir URL yeniden yönlendirme temsil eder.</span><span class="sxs-lookup"><span data-stu-id="41e8d-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="41e8d-142">JsonResult - AJAX uygulamada kullanılan bir JavaScript nesne gösterimi sonucunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="41e8d-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="41e8d-143">JavaScriptResult - JavaScript komut temsil eder.</span><span class="sxs-lookup"><span data-stu-id="41e8d-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="41e8d-144">ContentResult - metin sonucunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="41e8d-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="41e8d-145">FileContentResult - (ikili içerikle) indirilebilir bir dosyayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="41e8d-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="41e8d-146">FilePathResult - (yoluyla) indirilebilir bir dosyayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="41e8d-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="41e8d-147">FileStreamResult - (ile bir dosya akışı) indirilebilir bir dosyayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="41e8d-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="41e8d-148">Tüm bu eylem sonuçlarını temel ActionResult sınıfından devralınır.</span><span class="sxs-lookup"><span data-stu-id="41e8d-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="41e8d-149">Çoğu durumda, bir denetleyici eylemi bir ViewResult döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="41e8d-150">Örneğin, dizin denetleyici eylemi listeleme 2'deki bir ViewResult döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="41e8d-151">**2 - Controllers\BookController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="41e8d-151">**Listing 2 - Controllers\BookController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

<span data-ttu-id="41e8d-152">Bir eylem bir ViewResult geri döndüğünde, HTML tarayıcıya döndürülür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="41e8d-153">2. listeleme İNDİS() yöntemi tarayıcıya dizin adlı bir görünümü döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="41e8d-154">Listeleme 2 İNDİS() eylemde bir ViewResult() döndürmüyor dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="41e8d-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="41e8d-155">Bunun yerine, denetleyici temel sınıfın View() yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="41e8d-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="41e8d-156">Normalde, bir eylem sonucu doğrudan döndürmeyin.</span><span class="sxs-lookup"><span data-stu-id="41e8d-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="41e8d-157">Bunun yerine, denetleyici temel sınıfın aşağıdaki yöntemlerden birini arayın:</span><span class="sxs-lookup"><span data-stu-id="41e8d-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="41e8d-158">Görüntüleme - ViewResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="41e8d-159">Yeniden yönlendirme - RedirectResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="41e8d-160">RedirectToAction - RedirectToRouteResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="41e8d-161">RedirectToRoute - RedirectToRouteResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="41e8d-162">JSON - JsonResult eylem sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="41e8d-163">JavaScriptResult - Returns a JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="41e8d-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="41e8d-164">Content - Returns a ContentResult action result.</span><span class="sxs-lookup"><span data-stu-id="41e8d-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="41e8d-165">Dosya - FileContentResult, FilePathResult veya FileStreamResult parametreleri bağlı olarak yönteme geçirilen döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="41e8d-166">Bu nedenle, tarayıcıya bir görünüme dönmek istiyorsanız, View() yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="41e8d-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="41e8d-167">Bir denetleyici eylem kullanıcıya yönlendirmek istiyorsanız, RedirectToAction() yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="41e8d-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="41e8d-168">Örneğin, listeleme 3 Details() eylemde görüntüleyen bir görünüm ya da kullanıcı ID parametresi bir değer olup bağlı olarak İNDİS() eyleme yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="41e8d-169">**3 - CustomerController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="41e8d-169">**Listing 3 - CustomerController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

<span data-ttu-id="41e8d-170">ContentResult eylem sonucu özeldir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-170">The ContentResult action result is special.</span></span> <span data-ttu-id="41e8d-171">Eylem sonucu düz metin olarak döndürmek için ContentResult eylem sonucunu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="41e8d-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="41e8d-172">Örneğin, listeleme 4 İNDİS() yönteminde bir ileti HTML değil de, düz metin olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="41e8d-173">**4 - Controllers\StatusController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="41e8d-173">**Listing 4 - Controllers\StatusController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

<span data-ttu-id="41e8d-174">StatusController.Index() eylem çağrıldığında, görünümü döndürülmez.</span><span class="sxs-lookup"><span data-stu-id="41e8d-174">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="41e8d-175">Bunun yerine, "Hello World!" ham metni</span><span class="sxs-lookup"><span data-stu-id="41e8d-175">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="41e8d-176">tarayıcıya döndürülür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-176">is returned to the browser.</span></span>

<span data-ttu-id="41e8d-177">Ardından bir denetleyici eylemi eylem sonucunu değil - Örneğin, bir sonuç bir tarih veya tamsayı - döndürürse, sonuç bir ContentResult otomatik olarak paketlenir.</span><span class="sxs-lookup"><span data-stu-id="41e8d-177">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="41e8d-178">Örneğin, 5 listeleme WorkController İNDİS() eylemini çağrıldığında, tarih ContentResult otomatik olarak döndürülür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-178">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="41e8d-179">**5 - WorkController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="41e8d-179">**Listing 5 - WorkController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

<span data-ttu-id="41e8d-180">Listeleme 5 İNDİS() eylemde bir DateTime nesnesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="41e8d-180">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="41e8d-181">ASP.NET MVC çerçevesi DateTime nesnesi bir dizeye dönüştürür ve DateTime değeri bir ContentResult otomatik olarak sarmalar.</span><span class="sxs-lookup"><span data-stu-id="41e8d-181">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="41e8d-182">Tarayıcı, tarih ve saat düz metin olarak alır.</span><span class="sxs-lookup"><span data-stu-id="41e8d-182">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="41e8d-183">Özet</span><span class="sxs-lookup"><span data-stu-id="41e8d-183">Summary</span></span>

<span data-ttu-id="41e8d-184">ASP.NET MVC denetleyicileri, denetleyici eylemleri ve denetleyici eylem sonuçlarını kavramlarını tanıtmak için bu öğreticinin amacı oluştu.</span><span class="sxs-lookup"><span data-stu-id="41e8d-184">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="41e8d-185">Bu bölümde, ASP.NET MVC projesinde yeni denetleyicileri ekleme öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="41e8d-185">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="41e8d-186">Ardından, bir denetleyicinin nasıl ortak yöntemlerini öğrenilen universe denetleyici eylemleri sunulur.</span><span class="sxs-lookup"><span data-stu-id="41e8d-186">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="41e8d-187">Son olarak, farklı türdeki bir denetleyici eylemin getirdiği eylem sonuçlarını ele.</span><span class="sxs-lookup"><span data-stu-id="41e8d-187">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="41e8d-188">Özellikle, bir ViewResult, RedirectToActionResult ve ContentResult denetleyicisi eylemden döndürmek nasıl ele.</span><span class="sxs-lookup"><span data-stu-id="41e8d-188">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="41e8d-189">[Önceki](creating-an-action-vb.md)
> [sonraki](creating-custom-routes-cs.md)</span><span class="sxs-lookup"><span data-stu-id="41e8d-189">[Previous](creating-an-action-vb.md)
[Next](creating-custom-routes-cs.md)</span></span>
