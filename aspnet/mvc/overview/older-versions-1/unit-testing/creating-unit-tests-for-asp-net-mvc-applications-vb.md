---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: (VB) ASP.NET MVC uygulamaları için birim testleri oluşturma | Microsoft Docs
author: StephenWalther
description: Denetleyici eylemleri için birim testleri oluşturmayı öğrenin. Bu öğreticide, Stephen Walther bir denetleyici eylemi bir parti döndürüp döndürmediğini test gerçekleştirerek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 8edd134f6534b2a53be7f475cf0cb35ca93d3067
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384089"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a><span data-ttu-id="9d834-104">(VB) ASP.NET MVC uygulamaları için birim testleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d834-104">Creating Unit Tests for ASP.NET MVC Applications (VB)</span></span>
====================
<span data-ttu-id="9d834-105">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="9d834-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="9d834-106">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="9d834-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> <span data-ttu-id="9d834-107">Denetleyici eylemleri için birim testleri oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="9d834-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="9d834-108">Bu öğreticide, Stephen Walther bir denetleyici eylemi belirli bir görünüm verir, belirli bir veri kümesini döndürür ya da farklı türde bir eylem sonucunu döndürür test etmeyi göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="9d834-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="9d834-109">Bu öğreticinin amacı nasıl denetleyicileri için birim testleri, ASP.NET MVC uygulamaları yazabileceğiniz göstermektir.</span><span class="sxs-lookup"><span data-stu-id="9d834-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="9d834-110">Üç farklı türde birim testleri oluşturmak nasıl ele alır.</span><span class="sxs-lookup"><span data-stu-id="9d834-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="9d834-111">Denetleyici eylem tarafından döndürülen görünümü test etme, denetleyici eylem tarafından döndürülen görünüm verileri test etme ve bir denetleyici eylemi için ikinci bir denetleyici eylemi yönlendiren olup olmadığını test etme bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="9d834-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="9d834-112">Test denetleyicisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d834-112">Creating the Controller under Test</span></span>

<span data-ttu-id="9d834-113">Test etmek istediğiniz denetleyiciye oluşturarak başlayalım.</span><span class="sxs-lookup"><span data-stu-id="9d834-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="9d834-114">Adlı denetleyicisi `ProductController`, listeleme 1'de yer alır.</span><span class="sxs-lookup"><span data-stu-id="9d834-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="9d834-115">**Kod 1 – `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="9d834-115">**Listing 1 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

<span data-ttu-id="9d834-116">`ProductController` Adlı iki eylem yöntemleri içeren `Index()` ve `Details()`.</span><span class="sxs-lookup"><span data-stu-id="9d834-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="9d834-117">Her iki eylem yöntemleri bir görünüm döndürür.</span><span class="sxs-lookup"><span data-stu-id="9d834-117">Both action methods return a view.</span></span> <span data-ttu-id="9d834-118">Dikkat `Details()` Eylem Kimliği adlı bir parametre kabul eder</span><span class="sxs-lookup"><span data-stu-id="9d834-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="9d834-119">Bir denetleyici tarafından döndürülen görünümü test etme</span><span class="sxs-lookup"><span data-stu-id="9d834-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="9d834-120">Test etmek istediğimiz Imagine olup olmadığını `ProductController` sağ döndürür.</span><span class="sxs-lookup"><span data-stu-id="9d834-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="9d834-121">Emin olmak istiyoruz olduğunda `ProductController.Details()` eylem çağrılır, ayrıntı görünümü döndürülür.</span><span class="sxs-lookup"><span data-stu-id="9d834-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="9d834-122">Listeleme 2'deki test sınıfı tarafından döndürülen görünümü test etme için bir birim testini içeren `ProductController.Details()` eylem.</span><span class="sxs-lookup"><span data-stu-id="9d834-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="9d834-123">**Kod 2 – `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="9d834-123">**Listing 2 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

<span data-ttu-id="9d834-124">Adlı bir test yöntemi listeleme 2 sınıfında içerir `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="9d834-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="9d834-125">Bu yöntem, üç satır kod içerir.</span><span class="sxs-lookup"><span data-stu-id="9d834-125">This method contains three lines of code.</span></span> <span data-ttu-id="9d834-126">Kodun ilk satırını yeni bir örneğini oluşturur `ProductController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9d834-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="9d834-127">İkinci kod satırını denetleyicinin çağırır `Details()` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9d834-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="9d834-128">Son olarak, son satırının olup olmadığını görünümü tarafından döndürülen kodu denetimleri `Details()` Ayrıntılar görünümünü bir eylemdir.</span><span class="sxs-lookup"><span data-stu-id="9d834-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="9d834-129">`ViewResult.ViewName` Özelliği, denetleyici tarafından döndürülen görünümün adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="9d834-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="9d834-130">Bu özelliği test etme hakkında bir büyük uyarı.</span><span class="sxs-lookup"><span data-stu-id="9d834-130">One big warning about testing this property.</span></span> <span data-ttu-id="9d834-131">Bir denetleyici görünüm döndürebilir, iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="9d834-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="9d834-132">Bir denetleyici, böyle bir görünüm açıkça döndürebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9d834-132">A controller can explicitly return a view like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

<span data-ttu-id="9d834-133">Alternatif olarak, görünümün adını böyle denetleyici eylemini addan:</span><span class="sxs-lookup"><span data-stu-id="9d834-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

<span data-ttu-id="9d834-134">Bu denetleyici eylem ayrıca adlı bir görünüm verir `Details`.</span><span class="sxs-lookup"><span data-stu-id="9d834-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="9d834-135">Ancak, görünümün adını eylem adı algılanır.</span><span class="sxs-lookup"><span data-stu-id="9d834-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="9d834-136">Ardından Görünüm adını test etmek isterseniz, denetleyici eylem Görünüm adı açıkça döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="9d834-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="9d834-137">Her iki tuş bileşimini girerek listeleme 2'de birim testini çalıştırabilirsiniz **Ctrl-R, A** veya tıklayarak **Çözümdeki tüm Testleri Çalıştır** (bkz. Şekil 1) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="9d834-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="9d834-138">Test başarılı olursa, Şekil 2'deki Test Sonuçları penceresinde görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9d834-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="9d834-139">[![Çözümdeki tüm Testleri Çalıştır](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9d834-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span></span>

<span data-ttu-id="9d834-140">**Şekil 01**: Çözümdeki tüm testleri çalıştır ([tam boyutlu görüntüyü görmek için tıklatın](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9d834-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span></span>


<span data-ttu-id="9d834-141">[![Başarılı!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9d834-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span></span>

<span data-ttu-id="9d834-142">**Şekil 02**: başarılı!</span><span class="sxs-lookup"><span data-stu-id="9d834-142">**Figure 02**: Success!</span></span> <span data-ttu-id="9d834-143">([Tam boyutlu görüntüyü görmek için tıklatın](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9d834-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="9d834-144">Görünüm verilerini test denetleyicisi tarafından döndürülen</span><span class="sxs-lookup"><span data-stu-id="9d834-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="9d834-145">MVC denetleyicisi adlı kullanarak veri görünümüne geçirir *`View Data`*.</span><span class="sxs-lookup"><span data-stu-id="9d834-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="9d834-146">Örneğin, çağırdığınızda, belirli bir ürünün ayrıntılarını görüntülemek istediğiniz Imagine `ProductController Details()` eylem.</span><span class="sxs-lookup"><span data-stu-id="9d834-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="9d834-147">Örneği bu durumda, oluşturabileceğiniz bir `Product` sınıfı (modelinizde tanımlı) ve örneğine geçme `Details` yararlanarak görünümü `View Data`.</span><span class="sxs-lookup"><span data-stu-id="9d834-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="9d834-148">Değiştirilmiş `ProductController` listeleme 3'te güncelleştirilmiş içerir `Details()` bir ürün döndüren eylem.</span><span class="sxs-lookup"><span data-stu-id="9d834-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="9d834-149">**Kod 3 – `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="9d834-149">**Listing 3 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

<span data-ttu-id="9d834-150">İlk olarak, `Details()` eylem yeni bir örneğini oluşturur `Product` bir dizüstü bilgisayar nesnesini temsil eden sınıf.</span><span class="sxs-lookup"><span data-stu-id="9d834-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="9d834-151">Ardından, örneğini `Product` sınıfı, ikinci parametre olarak geçirilir `View()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9d834-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="9d834-152">Birim testleri yazabileceğiniz beklenen verileri olup olmadığını sınamak için görünümdeki veriler içeriyor.</span><span class="sxs-lookup"><span data-stu-id="9d834-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="9d834-153">Çağırdığınızda, bir dizüstü bilgisayar temsil eden bir ürün döndürülür olup olmadığını listeleyen 4 testlerinde birim testi `ProductController Details()` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9d834-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="9d834-154">**4 listeleme – `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="9d834-154">**Listing 4 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

<span data-ttu-id="9d834-155">Listeleme, 4'te `TestDetailsView()` yöntemi çağırarak döndürülen görünüm verileri testleri `Details()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9d834-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="9d834-156">`ViewData` Üzerinde bir özelliği olarak sunulan `ViewResult` çağırarak döndürülen `Details()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9d834-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="9d834-157">`ViewData.Model` Görünüme iletilen ürün özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="9d834-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="9d834-158">Test, yalnızca görünüm verileri ürün dizüstü bilgisayar adına sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="9d834-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="9d834-159">Bir denetleyici tarafından döndürülen eylem sonucu test etme</span><span class="sxs-lookup"><span data-stu-id="9d834-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="9d834-160">Daha karmaşık bir denetleyici eylemi, denetleyici eylem için geçirilen parametrelerin eylem sonuçlarını değerlere bağlı olarak farklı türde döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="9d834-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="9d834-161">Bir denetleyici eylemi çeşitli türleri dahil olmak üzere, eylem sonuçlarını döndürebilir bir `ViewResult`, `RedirectToRouteResult`, veya `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="9d834-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="9d834-162">Örneğin, değiştirilen `Details()` eylem listeleme 5 döndürür `Details` geçerli bir ürün kimliği eyleme başarıyla sonuçlandıktan sonra görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="9d834-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="9d834-163">1--seçeneğini yönlendirilirsiniz daha az bir geçersiz ürün kimliği--bir kimlik değeri geçirirseniz `Index()` eylem.</span><span class="sxs-lookup"><span data-stu-id="9d834-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="9d834-164">**5 listeleme – `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="9d834-164">**Listing 5 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

<span data-ttu-id="9d834-165">Davranışını sınayabilirsiniz `Details()` listeleme 6'daki birim testi ile eylem.</span><span class="sxs-lookup"><span data-stu-id="9d834-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="9d834-166">Listeleme 6'daki birim testi için yönlendirilirsiniz doğrular `Index` görüntülemek için -1 değerine sahip bir kimliği geçirildiğinde `Details()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9d834-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="9d834-167">**6 listeleme – `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="9d834-167">**Listing 6 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

<span data-ttu-id="9d834-168">Çağırdığınızda `RedirectToAction()` denetleyici eylemi bir denetleyici eylem yöntemine döndürür bir `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="9d834-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="9d834-169">Test denetimleri olmadığını `RedirectToRouteResult` kullanıcı adında bir denetleyici eylemi yönlendireceği `Index`.</span><span class="sxs-lookup"><span data-stu-id="9d834-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="9d834-170">Özet</span><span class="sxs-lookup"><span data-stu-id="9d834-170">Summary</span></span>

<span data-ttu-id="9d834-171">Bu öğreticide, MVC denetleyici eylemleri için birim testleri oluşturma öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="9d834-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="9d834-172">İlk olarak, sağ denetleyici eylem tarafından döndürülen olup olmadığını doğrulamak hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="9d834-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="9d834-173">Nasıl kullanacağınızı öğrendiniz `ViewResult.ViewName` özelliği bir görünümün adını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9d834-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="9d834-174">Ardından, içeriği nasıl test incelenirken `View Data`.</span><span class="sxs-lookup"><span data-stu-id="9d834-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="9d834-175">Doğru ürün içinde döndürülen olup olmadığını denetlemek öğrendiniz `View Data` bir denetleyici eylemi çağrıldıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="9d834-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="9d834-176">Son olarak, eylem sonuçlarını farklı türde bir denetleyici eylemi döndürülen olup olmadığını nasıl sınayabilirsiniz ele almıştık.</span><span class="sxs-lookup"><span data-stu-id="9d834-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="9d834-177">Bir denetleyici döndürüp döndürmediğini test öğrendiniz bir `ViewResult` veya `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="9d834-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9d834-178">Önceki</span><span class="sxs-lookup"><span data-stu-id="9d834-178">Previous</span></span>](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
