---
title: ASP.NET Core görünümlere bağımlılık ekleme
author: ardalis
description: ASP.NET Core MVC görünümlerine bağımlılık ekleme işlemini nasıl desteklediğini öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: 6241bb8e262f64e2e30721bc5fe6f8f1be84b60d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656102"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a><span data-ttu-id="1c0fb-103">ASP.NET Core görünümlere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="1c0fb-103">Dependency injection into views in ASP.NET Core</span></span>

<span data-ttu-id="1c0fb-104">[Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="1c0fb-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1c0fb-105">ASP.NET Core, görünümlere [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini destekler.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="1c0fb-106">Bu, yalnızca görünüm öğelerini doldurmak için gereken yerelleştirme veya veriler gibi görünüme özgü hizmetler için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="1c0fb-107">Denetleyicileriniz ve görünümleriniz arasındaki [kaygıların ayrılmasını](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) korumayı denemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-107">You should try to maintain [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) between your controllers and views.</span></span> <span data-ttu-id="1c0fb-108">Görünümlerinizin görüntüleyeceği verilerin çoğu denetleyiciden geçirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="1c0fb-109">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1c0fb-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration-injection"></a><span data-ttu-id="1c0fb-110">Yapılandırma ekleme</span><span class="sxs-lookup"><span data-stu-id="1c0fb-110">Configuration injection</span></span>

<span data-ttu-id="1c0fb-111">*appSettings. JSON* değerleri doğrudan bir görünüme eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-111">*appsettings.json* values can be injected directly into a view.</span></span>

<span data-ttu-id="1c0fb-112">Bir *appSettings. JSON* dosyası örneği:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-112">Example of an *appsettings.json* file:</span></span>

```json
{
   "root": {
      "parent": {
         "child": "myvalue"
      }
   }
}
```

<span data-ttu-id="1c0fb-113">`@inject`için sözdizimi: `@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="1c0fb-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="1c0fb-114">`@inject`kullanarak bir örnek:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-114">An example using `@inject`:</span></span>

```csharp
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration
@{
   string myValue = Configuration["root:parent:child"];
   ...
}
```

## <a name="service-injection"></a><span data-ttu-id="1c0fb-115">Hizmet ekleme</span><span class="sxs-lookup"><span data-stu-id="1c0fb-115">Service injection</span></span>

<span data-ttu-id="1c0fb-116">Bir hizmet `@inject` yönergesini kullanarak bir görünüme eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-116">A service can be injected into a view using the `@inject` directive.</span></span> <span data-ttu-id="1c0fb-117">`@inject`, görünüme özellik ekleme ve dı kullanarak özelliği doldurma olarak düşünebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-117">You can think of `@inject` as adding a property to the view, and populating the property using DI.</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="1c0fb-118">Bu görünüm, genel istatistikleri gösteren bir Özet ile birlikte `ToDoItem` örneklerinin bir listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-118">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="1c0fb-119">Özet, eklenen `StatisticsService`doldurulur.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-119">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="1c0fb-120">Bu hizmet, *Startup.cs*içinde `ConfigureServices` bağımlılık ekleme için kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-120">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="1c0fb-121">`StatisticsService`, bir depo aracılığıyla eriştiği `ToDoItem` örnekleri kümesi üzerinde bazı hesaplamalar yapar:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-121">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

<span data-ttu-id="1c0fb-122">Örnek depo, bellek içi bir koleksiyon kullanır.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-122">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="1c0fb-123">Yukarıda gösterilen uygulama (bellekteki tüm veriler üzerinde çalışır), büyük ve uzaktan erişilen veri kümeleri için önerilmez.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-123">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="1c0fb-124">Örnek, görüntüleme ile bağlantılı modeldeki verileri ve görünüme eklenen hizmeti görüntüler:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-124">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Toplam öğe, tamamlanan öğe, ortalama öncelik ve tamamlanma düzeylerine sahip bir görev listesi ve tamamlanma belirten Boole değerlerini görüntülemek için.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="1c0fb-126">Arama verileri dolduruluyor</span><span class="sxs-lookup"><span data-stu-id="1c0fb-126">Populating Lookup Data</span></span>

<span data-ttu-id="1c0fb-127">Görünüm ekleme, açılan listeler gibi kullanıcı arabirimi öğelerindeki seçenekleri doldurmak için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-127">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="1c0fb-128">Cinsiyet, eyalet ve diğer tercihlerin belirtilmesine yönelik seçenekleri içeren bir kullanıcı profili formu düşünün.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-128">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="1c0fb-129">Standart bir MVC yaklaşımı kullanarak bu tür bir formu işlemek, denetleyicinin Bu seçenek kümelerinin her biri için veri erişim Hizmetleri istemesi ve ardından bir modeli veya `ViewBag`, bağlanacak her seçenek kümesiyle doldurmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-129">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="1c0fb-130">Alternatif yaklaşım, seçenekleri almak için Hizmetleri doğrudan görünüme çıkarır.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-130">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="1c0fb-131">Bu, denetleyicinin gerektirdiği kod miktarını en aza indirir ve bu görünüm öğesi oluşturma mantığını görünümün içine taşır.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-131">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="1c0fb-132">Bir profil düzenlemesi formunu görüntülemeye yönelik denetleyici eylemi yalnızca profil örneği formunu geçirmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-132">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="1c0fb-133">Bu tercihleri güncelleştirmek için kullanılan HTML formu, özelliklerin üç yanındaki açılan listeleri içerir:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-133">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Ad, cinsiyet, eyalet ve sık kullanılan renk girişine izin veren bir formla profil görünümünü güncelleştirin.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="1c0fb-135">Bu listeler, görünüme eklenen bir hizmet tarafından doldurulur:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-135">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="1c0fb-136">`ProfileOptionsService`, yalnızca bu form için gereken verileri sağlamak üzere tasarlanan bir UI düzeyi hizmettir:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-136">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> <span data-ttu-id="1c0fb-137">`Startup.ConfigureServices`, bağımlılık ekleme yoluyla istediğiniz türleri kaydetmeyi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-137">Don't forget to register types you request through dependency injection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1c0fb-138">Kaydedilmemiş bir tür, hizmet sağlayıcısı, [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice)aracılığıyla dahili olarak sorgulandığından çalışma zamanında bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-138">An unregistered type throws an exception at runtime because the service provider is internally queried via [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span></span>

## <a name="overriding-services"></a><span data-ttu-id="1c0fb-139">Hizmetleri geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="1c0fb-139">Overriding Services</span></span>

<span data-ttu-id="1c0fb-140">Bu teknik, ekleme yeni hizmetlere ek olarak, bir sayfada önceden eklenen Hizmetleri geçersiz kılmak için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-140">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="1c0fb-141">Aşağıdaki şekilde, ilk örnekte kullanılan sayfada bulunan tüm alanlar gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-141">The figure below shows all of the fields available on the page used in the first example:</span></span>

![HTML, bileşen, StatsService ve URL alanları için belirlenmiş bir @ symbol üzerinde IntelliSense bağlamsal menüsü](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="1c0fb-143">Gördüğünüz gibi, varsayılan alanlar `Html`, `Component`ve `Url` (Ayrıca eklediğimiz `StatsService`) içerir.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-143">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="1c0fb-144">Örneğin, varsayılan HTML yardımcılarını kendi kendinize değiştirmek istiyorsanız, `@inject`kullanarak kolayca bunu yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1c0fb-144">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="1c0fb-145">Mevcut hizmetleri genişletmek istiyorsanız, bu tekniği kullanarak var olan uygulamadan devralma veya kaydırma yaparken bu tekniği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-145">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="1c0fb-146">Ayrıca Bkz.</span><span class="sxs-lookup"><span data-stu-id="1c0fb-146">See Also</span></span>

* <span data-ttu-id="1c0fb-147">Simon Timms blogu: [görünümünüze arama verileri alma](https://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="1c0fb-147">Simon Timms Blog: [Getting Lookup Data Into Your View](https://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
