---
title: "Görünümler içinde bağımlılık ekleme"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: a1258dbe2e659f6c5149d15b37451810ec7d6601
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="c3b06-102">Görünümler içinde bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="c3b06-102">Dependency injection into views</span></span>

<span data-ttu-id="c3b06-103">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c3b06-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c3b06-104">ASP.NET çekirdeği destekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) görünümleri içine.</span><span class="sxs-lookup"><span data-stu-id="c3b06-104">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="c3b06-105">Bu, yerelleştirme veya yalnızca görünüm öğeleri yerleştirmek için gereken verileri gibi görünüm özgü Hizmetleri için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c3b06-105">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="c3b06-106">Korumak denemelisiniz [sorunları ayrılması](http://deviq.com/separation-of-concerns/) görünümleri ve denetleyicilerini arasında.</span><span class="sxs-lookup"><span data-stu-id="c3b06-106">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="c3b06-107">Kendi görünümlerinizi görüntülemek verilerin çoğu denetleyicisinden geçirilmesi.</span><span class="sxs-lookup"><span data-stu-id="c3b06-107">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="c3b06-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c3b06-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="c3b06-109">Basit bir örnek</span><span class="sxs-lookup"><span data-stu-id="c3b06-109">A Simple Example</span></span>

<span data-ttu-id="c3b06-110">Bir görünümü kullanarak bir hizmet ekleyemezsiniz `@inject` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="c3b06-110">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="c3b06-111">Düşünebilirsiniz `@inject` görünümünüze özellik ekleme ve DI kullanan özellik doldurmak olarak.</span><span class="sxs-lookup"><span data-stu-id="c3b06-111">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="c3b06-112">Sözdizimi `@inject`:`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="c3b06-112">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="c3b06-113">Bir örneği `@inject` eylem:</span><span class="sxs-lookup"><span data-stu-id="c3b06-113">An example of `@inject` in action:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="c3b06-114">Bu görünüm bir listesini görüntüler `ToDoItem` genel istatistiklerini gösteren bir Özet birlikte örnekleri.</span><span class="sxs-lookup"><span data-stu-id="c3b06-114">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="c3b06-115">Özet doldurulur eklenen gelen `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="c3b06-115">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="c3b06-116">Bu hizmet bağımlılık ekleme için kayıtlı `ConfigureServices` içinde *haline*:</span><span class="sxs-lookup"><span data-stu-id="c3b06-116">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="c3b06-117">`StatisticsService` Kümesi üzerinde bazı hesaplamalar gerçekleştirir `ToDoItem` depo erişen örnekleri:</span><span class="sxs-lookup"><span data-stu-id="c3b06-117">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

<span data-ttu-id="c3b06-118">Örnek deposu, bir bellek içi koleksiyonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="c3b06-118">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="c3b06-119">Yukarıda gösterilen uygulama (tüm verilerin bellekte faaliyet) büyük, uzaktan erişilen veri kümeleri için önerilmez.</span><span class="sxs-lookup"><span data-stu-id="c3b06-119">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="c3b06-120">Örnek verileri modele görünüme bağlı ve görünüme eklenen hizmet görüntüler:</span><span class="sxs-lookup"><span data-stu-id="c3b06-120">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Toplam öğeleri listeleme görüntülemek için öğeleri, ortalama öncelik ve bunların öncelik düzeyleri ve tamamlanma gösteren Boole değerleri ile görevlerin bir listesi tamamlandı.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="c3b06-122">Arama veri doldurma</span><span class="sxs-lookup"><span data-stu-id="c3b06-122">Populating Lookup Data</span></span>

<span data-ttu-id="c3b06-123">Görünüm ekleme açılır listeleri gibi kullanıcı Arabirimi öğeleri seçeneklerinde doldurmak yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c3b06-123">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="c3b06-124">Cinsiyetiniz, durumu ve diğer tercihlerinizi belirlemek için seçenekleri içeren bir kullanıcı profili form göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="c3b06-124">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="c3b06-125">Standart bir MVC yaklaşım kullanarak formu işlemeye veri erişim Hizmetleri, bu seçenekleri her istemek ve bir modeli doldurmak için denetleyici duyar veya `ViewBag` her bağlanması için seçenekler kümesi.</span><span class="sxs-lookup"><span data-stu-id="c3b06-125">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="c3b06-126">Alternatif bir yaklaşım Hizmetleri seçenekleri elde etmek için doğrudan görünümüne yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="c3b06-126">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="c3b06-127">Bu görünüm öğesi yapım mantığı görünüme taşıma denetleyicisi tarafından gerekli kod miktarını azaltır.</span><span class="sxs-lookup"><span data-stu-id="c3b06-127">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="c3b06-128">Profil örneği form geçirmek bir profil düzenleme formu görüntülemek için denetleyici eylemi yalnızca gerekir:</span><span class="sxs-lookup"><span data-stu-id="c3b06-128">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="c3b06-129">Bu tercihler güncelleştirmek için kullanılan HTML formu açılır listeleri üç özellikleri içerir:</span><span class="sxs-lookup"><span data-stu-id="c3b06-129">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Profil görünümü adını, cinsiyetiniz, durumu ve sık kullanılan renk girişi sağlayan bir formla güncelleştirin.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="c3b06-131">Bu listeler görünüme eklenen hizmet tarafından doldurulur:</span><span class="sxs-lookup"><span data-stu-id="c3b06-131">These lists are populated by a service that has been injected into the view:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="c3b06-132">`ProfileOptionsService` Yalnızca bu form için gereken verileri sağlamak üzere tasarlanmış bir kullanıcı Arabirimi düzeyi hizmeti:</span><span class="sxs-lookup"><span data-stu-id="c3b06-132">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> <span data-ttu-id="c3b06-133">Bağımlılık ekleme aracılığıyla istek türlerinin kaydetmeyi unutmayın `ConfigureServices` yönteminde *haline*.</span><span class="sxs-lookup"><span data-stu-id="c3b06-133">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="c3b06-134">Hizmetleri geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="c3b06-134">Overriding Services</span></span>

<span data-ttu-id="c3b06-135">Yeni hizmetler injecting yanı sıra bu tekniği de daha önce eklenen Hizmetleri sayfasında geçersiz kılmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c3b06-135">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="c3b06-136">Aşağıdaki şekilde tüm kullanılabilir alanlar ilk örnekte kullanılan sayfasında gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="c3b06-136">The figure below shows all of the fields available on the page used in the first example:</span></span>

![IntelliSense bağlam menüsünde yazılmış bir @ simgesinden Html, bileşen, StatsService ve Url alanlarını listeleme](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="c3b06-138">Gördüğünüz gibi varsayılan alanlar `Html`, `Component`, ve `Url` (yanı sıra `StatsService` biz hatalara).</span><span class="sxs-lookup"><span data-stu-id="c3b06-138">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="c3b06-139">Örneği için varsayılan HTML Yardımcıları kendi ile değiştirmek istiyorsanız, kolayca bunu kullanarak yapabilirsiniz, `@inject`:</span><span class="sxs-lookup"><span data-stu-id="c3b06-139">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="c3b06-140">Var olan hizmetleri genişletmek istiyorsanız, içinden devralma veya var olan bir uygulama ile kendi kaydırma sırasında yalnızca bu tekniği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c3b06-140">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="c3b06-141">Ayrıca Bkz.</span><span class="sxs-lookup"><span data-stu-id="c3b06-141">See Also</span></span>

* <span data-ttu-id="c3b06-142">Simon Timms Blog: [, görünüme arama verilerini alma](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="c3b06-142">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
