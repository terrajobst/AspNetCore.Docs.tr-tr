---
title: "Ayrıntıları inceleme ve silme yöntemleri"
author: rick-anderson
description: "Basit bir ASP.NET Core MVC uygulaması görünümünde ve ayrıntıları denetleyici yöntemi."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: 870192b4-8d4f-45c7-8c14-83d02bc0ad79
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 28ed7a7a56415d7eb675c06353fb9a8f65fb571f
ms.sourcegitcommit: c9658c0db446f7cb2e443f62b00cf773bed545fa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2017
---
# <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="8e184-104">Ayrıntıları inceleme ve silme yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8e184-104">Examining the Details and Delete methods</span></span>

<span data-ttu-id="8e184-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8e184-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8e184-106">Film denetleyicisi açın ve incelemek `Details` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8e184-106">Open the Movie controller and examine the `Details` method:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="8e184-107">Bu eylem yöntemine oluşturulan MVC yapı iskelesi altyapısı yöntemini çağıran bir HTTP isteği gösteren bir açıklama ekler.</span><span class="sxs-lookup"><span data-stu-id="8e184-107">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="8e184-108">Bu durumda bir GET isteği üç URL kesimine sahip olup `Movies` denetleyicisi `Details` yöntemi ve bir `id` değeri.</span><span class="sxs-lookup"><span data-stu-id="8e184-108">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="8e184-109">Bu kesimler tanımlanmış geri çağırma *haline*.</span><span class="sxs-lookup"><span data-stu-id="8e184-109">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="8e184-110">EF verileri kullanarak için arama yapmayı kolaylaştırır `SingleOrDefaultAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8e184-110">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="8e184-111">Yönteme yerleşik bir önemli güvenlik kodu şey denemeden önce search yöntemini film buldu doğrular özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="8e184-111">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="8e184-112">Örneğin, bir bilgisayar korsanının siteye bağlantılardan tarafından oluşturulan URL değiştirerek hatalara `http://localhost:xxxx/Movies/Details/1` gibi bir `http://localhost:xxxx/Movies/Details/12345` (veya gerçek bir filmi temsil etmez başka bir değer).</span><span class="sxs-lookup"><span data-stu-id="8e184-112">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="8e184-113">Null film denetlemiyordu, uygulama bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="8e184-113">If you did not check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="8e184-114">İncelemek `Delete` ve `DeleteConfirmed` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="8e184-114">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

<span data-ttu-id="8e184-115">Unutmayın `HTTP GET Delete` yöntemi belirtilen film Sil değil, bir filmi görünümünü (HttpPost) gönderebileceği silme değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="8e184-115">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="8e184-116">Bir GET'e yanıt olarak bir silme işlemi isteği (veya bir düzenleme işlemi gerçekleştirilirken Bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturmak) güvenlik boşluğu açar.</span><span class="sxs-lookup"><span data-stu-id="8e184-116">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="8e184-117">`[HttpPost]` Verileri siler yöntemi adlandırılan `DeleteConfirmed` HTTP POST yöntemi için benzersiz bir imza veya ad vermek için.</span><span class="sxs-lookup"><span data-stu-id="8e184-117">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="8e184-118">İki yöntem imzaları aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8e184-118">The two method signatures are shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="8e184-119">Ortak dil çalışma zamanı (CLR) benzersiz parametre imzası (aynı yöntem adı ancak farklı parametrelerin listesi) sağlamak için aşırı yüklenmiş yöntemler gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8e184-119">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="8e184-120">Ancak, burada iki gerekir `Delete` --biri--get ve POST için bir yöntem her ikisi de aynı parametre imzaya sahip.</span><span class="sxs-lookup"><span data-stu-id="8e184-120">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="8e184-121">(Her ikisi de tek bir tamsayı bir parametre olarak kabul etmeniz gerekir.)</span><span class="sxs-lookup"><span data-stu-id="8e184-121">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="8e184-122">Bu sorunun iki yaklaşım vardır, farklı adlar yöntemleri vermek biridir.</span><span class="sxs-lookup"><span data-stu-id="8e184-122">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="8e184-123">Önceki örnekte yapı iskelesi mekanizması ne yaptığını olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="8e184-123">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="8e184-124">Ancak, bu küçük bir sorunla sunar: ASP.NET eylem yöntemleri için bir URL kesimleri adıyla eşler ve bir yöntem yeniden adlandırırsanız, normal olarak Yönlendirme bu yöntem bulmak bağlanamayacak.</span><span class="sxs-lookup"><span data-stu-id="8e184-124">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="8e184-125">Eklemek için örnekte bkz çözümdür `ActionName("Delete")` özniteliğini `DeleteConfirmed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8e184-125">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="8e184-126">Böylece bir POST isteği için /Delete/ içeren bir URL bulur, öznitelik eşleme için yönlendirme sistem gerçekleştirir. `DeleteConfirmed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8e184-126">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="8e184-127">Aynı ad ve imza sahip yöntemleri için başka bir Ortak üstesinden gelme yapay fazladan dahil etmek için POST yöntemini imza değiştirmektir (kullanılmayan) parametre.</span><span class="sxs-lookup"><span data-stu-id="8e184-127">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="8e184-128">Biz eklendiğinde neler önceki postasına yaptığımız olan `notUsed` parametresi.</span><span class="sxs-lookup"><span data-stu-id="8e184-128">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="8e184-129">Aynı şeyi burada yapabilirsiniz `[HttpPost] Delete` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8e184-129">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="8e184-130">Azure'a yayımlama</span><span class="sxs-lookup"><span data-stu-id="8e184-130">Publish to Azure</span></span>

<span data-ttu-id="8e184-131">Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) bu uygulama için Azure yayımlama hakkında yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="8e184-131">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure.</span></span>

<span data-ttu-id="8e184-132">Bu giriş ASP.NET Core MVC tamamlamak için teşekkür ederiz.</span><span class="sxs-lookup"><span data-stu-id="8e184-132">Thanks for completing this introduction to ASP.NET Core MVC.</span></span> <span data-ttu-id="8e184-133">Çıkmadan açıklamaları veriyoruz.</span><span class="sxs-lookup"><span data-stu-id="8e184-133">We appreciate any comments you leave.</span></span> <span data-ttu-id="8e184-134">[MVC ve EF çekirdek Başlarken](xref:data/ef-mvc/intro) mükemmel bir izleme Bu öğretici kadar olan.</span><span class="sxs-lookup"><span data-stu-id="8e184-134">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="8e184-135">Önceki</span><span class="sxs-lookup"><span data-stu-id="8e184-135">Previous</span></span>](validation.md)
