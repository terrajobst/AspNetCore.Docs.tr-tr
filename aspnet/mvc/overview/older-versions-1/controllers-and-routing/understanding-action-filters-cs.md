---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Eylem filtreleri (C#) Anlama | Microsoft Docs
author: microsoft
description: "Bu öğretici eylem filtrelerini açıklamak için hedefidir. Bir eylem filtresi bir denetleyici eylemi--ya da tüm bir denetleyiciye uygulanan bir özniteliktir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: 86d5d429d9900d4c04391804598626705e6c88b4
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
<a name="understanding-action-filters-c"></a><span data-ttu-id="97fb3-104">Eylem filtreleri (C#) Anlama</span><span class="sxs-lookup"><span data-stu-id="97fb3-104">Understanding Action Filters (C#)</span></span>
====================
<span data-ttu-id="97fb3-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="97fb3-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="97fb3-106">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="97fb3-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> <span data-ttu-id="97fb3-107">Bu öğretici eylem filtrelerini açıklamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="97fb3-107">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="97fb3-108">Bir eylem filtresi bir denetleyici eylemi--veya eylem yürütüldüğü biçimini değiştirir tüm controller--uygulayabileceğiniz bir özniteliktir.</span><span class="sxs-lookup"><span data-stu-id="97fb3-108">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span>


## <a name="understanding-action-filters"></a><span data-ttu-id="97fb3-109">Eylem filtreleri anlama</span><span class="sxs-lookup"><span data-stu-id="97fb3-109">Understanding Action Filters</span></span>

<span data-ttu-id="97fb3-110">Bu öğretici eylem filtrelerini açıklamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="97fb3-110">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="97fb3-111">Bir eylem filtresi bir denetleyici eylemi--veya eylem yürütüldüğü biçimini değiştirir tüm controller--uygulayabileceğiniz bir özniteliktir.</span><span class="sxs-lookup"><span data-stu-id="97fb3-111">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span> <span data-ttu-id="97fb3-112">ASP.NET MVC çerçevesi birkaç eylem filtrelerini içerir:</span><span class="sxs-lookup"><span data-stu-id="97fb3-112">The ASP.NET MVC framework includes several action filters:</span></span>

- <span data-ttu-id="97fb3-113">OutputCache – Bu eylem filtresi belirtilen bir süre için bir denetleyici eylemi çıktısını önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="97fb3-113">OutputCache – This action filter caches the output of a controller action for a specified amount of time.</span></span>
- <span data-ttu-id="97fb3-114">HandleError – Bu eylem filtresi bir denetleyici eylemi yürüten tetiklenir hatalarını ele alır.</span><span class="sxs-lookup"><span data-stu-id="97fb3-114">HandleError – This action filter handles errors raised when a controller action executes.</span></span>
- <span data-ttu-id="97fb3-115">Yetki – belirli bir kullanıcı veya rol için erişimi kısıtlamak Bu eylem filtresi sağlar.</span><span class="sxs-lookup"><span data-stu-id="97fb3-115">Authorize – This action filter enables you to restrict access to a particular user or role.</span></span>

<span data-ttu-id="97fb3-116">Kendi özel eylem filtrelerini de oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-116">You also can create your own custom action filters.</span></span> <span data-ttu-id="97fb3-117">Örneğin, bir özel kimlik doğrulama sistemi uygulamak için bir özel eylem filtresi oluşturmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-117">For example, you might want to create a custom action filter in order to implement a custom authentication system.</span></span> <span data-ttu-id="97fb3-118">Ya da bir denetleyici eylemi tarafından döndürülen görünüm verileri değiştiren bir eylem filtresi oluşturmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-118">Or, you might want to create an action filter that modifies the view data returned by a controller action.</span></span>

<span data-ttu-id="97fb3-119">Bu öğreticide, sıfırdan bir eylem filtresi oluşturmak öğrenin.</span><span class="sxs-lookup"><span data-stu-id="97fb3-119">In this tutorial, you learn how to build an action filter from the ground up.</span></span> <span data-ttu-id="97fb3-120">Visual Studio çıkış penceresine farklı bir eylem işleme aşamalarını günlüklerini günlük eylem filtresi oluşturuyoruz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-120">We create a Log action filter that logs different stages of the processing of an action to the Visual Studio Output window.</span></span>

### <a name="using-an-action-filter"></a><span data-ttu-id="97fb3-121">Bir eylem filtresini kullanma</span><span class="sxs-lookup"><span data-stu-id="97fb3-121">Using an Action Filter</span></span>

<span data-ttu-id="97fb3-122">Bir eylem filtresi bir özniteliktir.</span><span class="sxs-lookup"><span data-stu-id="97fb3-122">An action filter is an attribute.</span></span> <span data-ttu-id="97fb3-123">Tek tek denetleyici eylem veya tüm denetleyicisi için çoğu eylemi filtre uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-123">You can apply most action filters to either an individual controller action or an entire controller.</span></span>

<span data-ttu-id="97fb3-124">Örneğin, veri denetleyici listeleme 1 adlı bir eylem sunan `Index()` geçerli saati döndürür.</span><span class="sxs-lookup"><span data-stu-id="97fb3-124">For example, the Data controller in Listing 1 exposes an action named `Index()` that returns the current time.</span></span> <span data-ttu-id="97fb3-125">Bu eylem ile donatılmış `OutputCache` eylem filtresi.</span><span class="sxs-lookup"><span data-stu-id="97fb3-125">This action is decorated with the `OutputCache` action filter.</span></span> <span data-ttu-id="97fb3-126">Bu filtre 10 saniye için önbelleğe alınacak eylemi tarafından döndürülen değer neden olur.</span><span class="sxs-lookup"><span data-stu-id="97fb3-126">This filter causes the value returned by the action to be cached for 10 seconds.</span></span>

<span data-ttu-id="97fb3-127">**Kod 1 –`Controllers\DataController.cs`**</span><span class="sxs-lookup"><span data-stu-id="97fb3-127">**Listing 1 – `Controllers\DataController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

<span data-ttu-id="97fb3-128">Art arda çağırma varsa `Index()` URL/Data/dizin tarayıcınızın adres çubuğuna girerek ve yenileme basarsa eylem düğmesi birden çok kez aynı anda 10 saniye görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-128">If you repeatedly invoke the `Index()` action by entering the URL /Data/Index into the address bar of your browser and hitting the Refresh button multiple times, then you will see the same time for 10 seconds.</span></span> <span data-ttu-id="97fb3-129">Çıktısını `Index()` eylem 10 (bkz. Şekil 1) saniye için önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="97fb3-129">The output of the `Index()` action is cached for 10 seconds (see Figure 1).</span></span>


<span data-ttu-id="97fb3-130">[![Önbelleğe alınan zaman](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="97fb3-130">[![Cached time](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span></span>

<span data-ttu-id="97fb3-131">**Şekil 01**: zaman önbelleğe ([tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-action-filters-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="97fb3-131">**Figure 01**: Cached time ([Click to view full-size image](understanding-action-filters-cs/_static/image3.png))</span></span>


<span data-ttu-id="97fb3-132">Bir tek eylem filtresi – listeleme 1'deki `OutputCache` eylem filtresi – uygulanan `Index()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="97fb3-132">In Listing 1, a single action filter – the `OutputCache` action filter – is applied to the `Index()` method.</span></span> <span data-ttu-id="97fb3-133">Gerekirse, birden çok eylem filtrelerini aynı eylem uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-133">If you need, you can apply multiple action filters to the same action.</span></span> <span data-ttu-id="97fb3-134">Örneğin, her ikisini de uygulamak isteyebilirsiniz `OutputCache` ve `HandleError` aynı eylem için eylem filtreleri.</span><span class="sxs-lookup"><span data-stu-id="97fb3-134">For example, you might want to apply both the `OutputCache` and `HandleError` action filters to the same action.</span></span>

<span data-ttu-id="97fb3-135">1. listeleme `OutputCache` eylem filtresi uygulanan `Index()` eylem.</span><span class="sxs-lookup"><span data-stu-id="97fb3-135">In Listing 1, the `OutputCache` action filter is applied to the `Index()` action.</span></span> <span data-ttu-id="97fb3-136">Bu öznitelik için geçerli olabilir `DataController` sınıfının kendisini.</span><span class="sxs-lookup"><span data-stu-id="97fb3-136">You also could apply this attribute to the `DataController` class itself.</span></span> <span data-ttu-id="97fb3-137">Bu durumda, denetleyici tarafından kullanıma sunulan herhangi bir eylem tarafından döndürülen sonuç 10 saniye için önbelleğe.</span><span class="sxs-lookup"><span data-stu-id="97fb3-137">In that case, the result returned by any action exposed by the controller would be cached for 10 seconds.</span></span>

### <a name="the-different-types-of-filters"></a><span data-ttu-id="97fb3-138">Farklı türde filtreler</span><span class="sxs-lookup"><span data-stu-id="97fb3-138">The Different Types of Filters</span></span>

<span data-ttu-id="97fb3-139">ASP.NET MVC çerçevesi filtre dört farklı türlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="97fb3-139">The ASP.NET MVC framework supports four different types of filters:</span></span>

1. <span data-ttu-id="97fb3-140">Yetkilendirme filtreleri – uygulayan `IAuthorizationFilter` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="97fb3-140">Authorization filters – Implements the `IAuthorizationFilter` attribute.</span></span>
2. <span data-ttu-id="97fb3-141">Eylem filtreleri – uygulayan `IActionFilter` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="97fb3-141">Action filters – Implements the `IActionFilter` attribute.</span></span>
3. <span data-ttu-id="97fb3-142">Sonuç filtreleri – uygulayan `IResultFilter` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="97fb3-142">Result filters – Implements the `IResultFilter` attribute.</span></span>
4. <span data-ttu-id="97fb3-143">Özel durum filtreleri – uygulayan `IExceptionFilter` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="97fb3-143">Exception filters – Implements the `IExceptionFilter` attribute.</span></span>

<span data-ttu-id="97fb3-144">Filtreler, yukarıda listelenen sırada yürütülür.</span><span class="sxs-lookup"><span data-stu-id="97fb3-144">Filters are executed in the order listed above.</span></span> <span data-ttu-id="97fb3-145">Örneğin, yetkilendirme filtreleri her zaman eylem filtrelerden önce yürütülür ve özel durum filtreleri filtre diğer her tür sonra her zaman yürütülür.</span><span class="sxs-lookup"><span data-stu-id="97fb3-145">For example, authorization filters are always executed before action filters and exception filters are always executed after every other type of filter.</span></span>

<span data-ttu-id="97fb3-146">Yetkilendirme filtreleri, kimlik doğrulama ve yetkilendirme denetleyici eylemleri uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="97fb3-146">Authorization filters are used to implement authentication and authorization for controller actions.</span></span> <span data-ttu-id="97fb3-147">Örneğin, yetkilendirme filtresi bir yetkilendirme filtresi örneğidir.</span><span class="sxs-lookup"><span data-stu-id="97fb3-147">For example, the Authorize filter is an example of an Authorization filter.</span></span>

<span data-ttu-id="97fb3-148">Eylem filtreleri önce ve denetleyici Eylem yürütüldükten sonra yürütülür mantık içerir.</span><span class="sxs-lookup"><span data-stu-id="97fb3-148">Action filters contain logic that is executed before and after a controller action executes.</span></span> <span data-ttu-id="97fb3-149">Bir denetleyici eylemi döndürür görünüm verileri değiştirmek için bir eylem filtresi örneği için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-149">You can use an action filter, for instance, to modify the view data that a controller action returns.</span></span>

<span data-ttu-id="97fb3-150">Sonuç filtreleri önce ve bir görünüm sonucu yürütüldükten sonra yürütülür mantık içerir.</span><span class="sxs-lookup"><span data-stu-id="97fb3-150">Result filters contain logic that is executed before and after a view result is executed.</span></span> <span data-ttu-id="97fb3-151">Örneğin, bir görünüm sonucu değiştirmek isteyebilirsiniz görünümü tarayıcıya işlenmeden önce sağ.</span><span class="sxs-lookup"><span data-stu-id="97fb3-151">For example, you might want to modify a view result right before the view is rendered to the browser.</span></span>

<span data-ttu-id="97fb3-152">Özel durum filtreleri çalıştırmak için filtre son türüdür.</span><span class="sxs-lookup"><span data-stu-id="97fb3-152">Exception filters are the last type of filter to run.</span></span> <span data-ttu-id="97fb3-153">Denetleyici eylem veya denetleyici eylem sonuçlarını tarafından oluşturulan hataları işlemek için bir özel durum filtresini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-153">You can use an exception filter to handle errors raised by either your controller actions or controller action results.</span></span> <span data-ttu-id="97fb3-154">Özel durum filtreleri, hataları günlüğe kaydetmek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-154">You also can use exception filters to log errors.</span></span>

<span data-ttu-id="97fb3-155">Farklı her filtre türü belirli bir sırada yürütülür.</span><span class="sxs-lookup"><span data-stu-id="97fb3-155">Each different type of filter is executed in a particular order.</span></span> <span data-ttu-id="97fb3-156">Aynı türde filtrelerinin yürütüldüğü sırayı denetlemek istiyorsanız, filtrenin sırası özelliği ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-156">If you want to control the order in which filters of the same type are executed then you can set a filter's Order property.</span></span>

<span data-ttu-id="97fb3-157">Tüm eylem filtrelerini temel sınıfı olan `System.Web.Mvc.FilterAttribute` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="97fb3-157">The base class for all action filters is the `System.Web.Mvc.FilterAttribute` class.</span></span> <span data-ttu-id="97fb3-158">Belirli bir tür filtre uygulamak istediğiniz sonra temel filtre sınıfından devralan ve bir veya daha fazla uygulayan bir sınıf oluşturmak gereken `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, veya `IExceptionFilter` arabirimleri.</span><span class="sxs-lookup"><span data-stu-id="97fb3-158">If you want to implement a particular type of filter, then you need to create a class that inherits from the base Filter class and implements one or more of the `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, or `IExceptionFilter` interfaces.</span></span>

### <a name="the-base-actionfilterattribute-class"></a><span data-ttu-id="97fb3-159">Temel ActionFilterAttribute sınıfı</span><span class="sxs-lookup"><span data-stu-id="97fb3-159">The Base ActionFilterAttribute Class</span></span>

<span data-ttu-id="97fb3-160">Bir özel eylem filtresi uygulamak kolaylaştırmak için bir taban ASP.NET MVC çerçevesi içeren `ActionFilterAttribute` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="97fb3-160">In order to make it easier for you to implement a custom action filter, the ASP.NET MVC framework includes a base `ActionFilterAttribute` class.</span></span> <span data-ttu-id="97fb3-161">Bu sınıfın her ikisi de uygulayan `IActionFilter` ve `IResultFilter` arabirimleri ve devraldığı `Filter` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="97fb3-161">This class implements both the `IActionFilter` and `IResultFilter` interfaces and inherits from the `Filter` class.</span></span>

<span data-ttu-id="97fb3-162">Burada terminoloji tamamen tutarlı değil.</span><span class="sxs-lookup"><span data-stu-id="97fb3-162">The terminology here is not entirely consistent.</span></span> <span data-ttu-id="97fb3-163">Teknik olarak, ActionFilterAttribute sınıfından devralan bir sınıf bir eylem filtresi ve bir sonuç Filtresi ' dir.</span><span class="sxs-lookup"><span data-stu-id="97fb3-163">Technically, a class that inherits from the ActionFilterAttribute class is both an action filter and a result filter.</span></span> <span data-ttu-id="97fb3-164">Ancak, gevşek anlamda word eylem filtresi herhangi türde bir ASP.NET MVC çerçevesi filtrede başvurmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="97fb3-164">However, in the loose sense, the word action filter is used to refer to any type of filter in the ASP.NET MVC framework.</span></span>

<span data-ttu-id="97fb3-165">Temel `ActionFilterAttribute` sınıfı geçersiz kılabilirsiniz aşağıdaki yöntemleri vardır:</span><span class="sxs-lookup"><span data-stu-id="97fb3-165">The base `ActionFilterAttribute` class has the following methods that you can override:</span></span>

- <span data-ttu-id="97fb3-166">Bir denetleyici eylemi yürütülmeden önce OnActionExecuting – bu yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="97fb3-166">OnActionExecuting – This method is called before a controller action is executed.</span></span>
- <span data-ttu-id="97fb3-167">Denetleyici Eylem yürütüldükten sonra OnActionExecuted – bu yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="97fb3-167">OnActionExecuted – This method is called after a controller action is executed.</span></span>
- <span data-ttu-id="97fb3-168">Denetleyici eylem sonucu yürütülmeden önce OnResultExecuting – bu yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="97fb3-168">OnResultExecuting – This method is called before a controller action result is executed.</span></span>
- <span data-ttu-id="97fb3-169">Denetleyici eylem sonucu yürütüldükten sonra OnResultExecuted – bu yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="97fb3-169">OnResultExecuted – This method is called after a controller action result is executed.</span></span>

<span data-ttu-id="97fb3-170">Sonraki bölümde, farklı bu yöntemlerin her biri uygulamak nasıl göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-170">In the next section, we'll see how you can implement each of these different methods.</span></span>

### <a name="creating-a-log-action-filter"></a><span data-ttu-id="97fb3-171">Günlük eylem filtresi oluşturma</span><span class="sxs-lookup"><span data-stu-id="97fb3-171">Creating a Log Action Filter</span></span>

<span data-ttu-id="97fb3-172">Özel eylem filtresinin nasıl oluşturulacağına göstermek için bir denetleyici eylemi Visual Studio çıkış penceresi için işlem aşamaları günlüklerini bir özel eylem filtresi oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="97fb3-172">In order to illustrate how you can build a custom action filter, we'll create a custom action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span> <span data-ttu-id="97fb3-173">Bizim `LogActionFilter` listeleme 2'de yer alır.</span><span class="sxs-lookup"><span data-stu-id="97fb3-173">Our `LogActionFilter` is contained in Listing 2.</span></span>

<span data-ttu-id="97fb3-174">**Kod 2 –`ActionFilters\LogActionFilter.cs`**</span><span class="sxs-lookup"><span data-stu-id="97fb3-174">**Listing 2 – `ActionFilters\LogActionFilter.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

<span data-ttu-id="97fb3-175">Listeleme 2'de, `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, ve `OnResultExecuted()` tüm yöntemleri çağırmak `Log()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="97fb3-175">In Listing 2, the `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, and `OnResultExecuted()` methods all call the `Log()` method.</span></span> <span data-ttu-id="97fb3-176">Yöntemin adı ve geçerli rota verilerini geçirilir `Log()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="97fb3-176">The name of the method and the current route data is passed to the `Log()` method.</span></span> <span data-ttu-id="97fb3-177">`Log()` Yöntemi için Visual Studio çıktı penceresinde bir ileti yazar (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="97fb3-177">The `Log()` method writes a message to the Visual Studio Output window (see Figure 2).</span></span>


<span data-ttu-id="97fb3-178">[![Visual Studio çıkış penceresine yazma](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="97fb3-178">[![Writing to the Visual Studio Output window](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span></span>

<span data-ttu-id="97fb3-179">**Şekil 02**: Visual Studio çıkış penceresine yazma ([tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-action-filters-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="97fb3-179">**Figure 02**: Writing to the Visual Studio Output window ([Click to view full-size image](understanding-action-filters-cs/_static/image6.png))</span></span>


<span data-ttu-id="97fb3-180">Giriş denetleyicisi listeleme 3'te, nasıl bir tüm denetleyicinin sınıf günlük eylem filtresi uygulayabilirsiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="97fb3-180">The Home controller in Listing 3 illustrates how you can apply the Log action filter to an entire controller class.</span></span> <span data-ttu-id="97fb3-181">Her giriş denetleyici tarafından kullanıma sunulan eylemlerden herhangi birini çağrılır – ya da `Index()` yöntemi veya `About()` yöntemi – eylem için Visual Studio çıkış penceresi günlüğe kaydedilen işleme aşamalarını.</span><span class="sxs-lookup"><span data-stu-id="97fb3-181">Whenever any of the actions exposed by the Home controller are invoked – either the `Index()` method or the `About()` method – the stages of processing the action are logged to the Visual Studio Output window.</span></span>

<span data-ttu-id="97fb3-182">**Kod 3 –`Controllers\HomeController.cs`**</span><span class="sxs-lookup"><span data-stu-id="97fb3-182">**Listing 3 – `Controllers\HomeController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a><span data-ttu-id="97fb3-183">Özet</span><span class="sxs-lookup"><span data-stu-id="97fb3-183">Summary</span></span>

<span data-ttu-id="97fb3-184">Bu öğreticide, ASP.NET MVC eylem filtrelerini tanıtıldı.</span><span class="sxs-lookup"><span data-stu-id="97fb3-184">In this tutorial, you were introduced to ASP.NET MVC action filters.</span></span> <span data-ttu-id="97fb3-185">Dört farklı türde filtreler hakkında öğrenilen: Yetkilendirme filtreleri, eylem filtreleri, sonuç filtreleri ve özel durum filtreleri.</span><span class="sxs-lookup"><span data-stu-id="97fb3-185">You learned about the four different types of filters: authorization filters, action filters, result filters, and exception filters.</span></span> <span data-ttu-id="97fb3-186">Base hakkında da öğrenilen `ActionFilterAttribute` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="97fb3-186">You also learned about the base `ActionFilterAttribute` class.</span></span>

<span data-ttu-id="97fb3-187">Son olarak, bir basit eylem filtresi uygulamak nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="97fb3-187">Finally, you learned how to implement a simple action filter.</span></span> <span data-ttu-id="97fb3-188">Visual Studio çıkış penceresi için bir denetleyici eylemi işleme aşamalarını günlüklerini günlük eylem filtresi oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="97fb3-188">We created a Log action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="97fb3-189">[Önceki](asp-net-mvc-routing-overview-cs.md)
[sonraki](improving-performance-with-output-caching-cs.md)</span><span class="sxs-lookup"><span data-stu-id="97fb3-189">[Previous](asp-net-mvc-routing-overview-cs.md)
[Next](improving-performance-with-output-caching-cs.md)</span></span>
