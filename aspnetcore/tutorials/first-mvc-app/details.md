---
title: Ayrıntıları inceleyin ve silme yöntemleri bir ASP.NET Core uygulama
author: rick-anderson
description: Ayrıntılar denetleyicisi yöntem hakkında bilgi edinin ve temel bir ASP.NET Core MVC uygulama görüntüleyin.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: f7f9a369e3e612542140fcf1091b21037e530a91
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278654"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a><span data-ttu-id="343df-103">Ayrıntıları inceleyin ve silme yöntemleri bir ASP.NET Core uygulama</span><span class="sxs-lookup"><span data-stu-id="343df-103">Examine the Details and Delete methods of an ASP.NET Core app</span></span>

<span data-ttu-id="343df-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="343df-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="343df-105">Film denetleyicisi açın ve incelemek `Details` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="343df-105">Open the Movie controller and examine the `Details` method:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="343df-106">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="343df-106">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="343df-107">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="343df-107">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

::: moniker-end

<span data-ttu-id="343df-108">Bu eylem yöntemine oluşturulan MVC yapı iskelesi altyapısı yöntemini çağıran bir HTTP isteği gösteren bir açıklama ekler.</span><span class="sxs-lookup"><span data-stu-id="343df-108">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="343df-109">Bu durumda bir GET isteği üç URL kesimine sahip olup `Movies` denetleyicisi `Details` yöntemi ve bir `id` değeri.</span><span class="sxs-lookup"><span data-stu-id="343df-109">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="343df-110">Bu kesimler tanımlanmış geri çağırma *haline*.</span><span class="sxs-lookup"><span data-stu-id="343df-110">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="343df-111">EF verileri kullanarak için arama yapmayı kolaylaştırır `SingleOrDefaultAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="343df-111">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="343df-112">Yönteme yerleşik bir önemli güvenlik kodu şey denemeden önce search yöntemini film buldu doğrular özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="343df-112">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="343df-113">Örneğin, bir bilgisayar korsanının siteye bağlantılardan tarafından oluşturulan URL değiştirerek hatalara `http://localhost:xxxx/Movies/Details/1` gibi bir `http://localhost:xxxx/Movies/Details/12345` (veya gerçek bir filmi temsil etmez başka bir değer).</span><span class="sxs-lookup"><span data-stu-id="343df-113">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="343df-114">Null film işaretlerseniz alamadık, uygulama bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="343df-114">If you didn't check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="343df-115">İncelemek `Delete` ve `DeleteConfirmed` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="343df-115">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="343df-116">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="343df-116">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="343df-117">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="343df-117">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

::: moniker-end

<span data-ttu-id="343df-118">Unutmayın `HTTP GET Delete` yöntemi belirtilen film Sil değil, bir filmi görünümünü (HttpPost) gönderebileceği silme değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="343df-118">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="343df-119">Bir GET'e yanıt olarak bir silme işlemi isteği (veya bir düzenleme işlemi gerçekleştirilirken Bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturmak) güvenlik boşluğu açar.</span><span class="sxs-lookup"><span data-stu-id="343df-119">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="343df-120">`[HttpPost]` Verileri siler yöntemi adlandırılan `DeleteConfirmed` HTTP POST yöntemi için benzersiz bir imza veya ad vermek için.</span><span class="sxs-lookup"><span data-stu-id="343df-120">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="343df-121">İki yöntem imzaları aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="343df-121">The two method signatures are shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="343df-122">Ortak dil çalışma zamanı (CLR) benzersiz parametre imzası (aynı yöntem adı ancak farklı parametrelerin listesi) sağlamak için aşırı yüklenmiş yöntemler gerektirir.</span><span class="sxs-lookup"><span data-stu-id="343df-122">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="343df-123">Ancak, burada iki gerekir `Delete` --biri--get ve POST için bir yöntem her ikisi de aynı parametre imzaya sahip.</span><span class="sxs-lookup"><span data-stu-id="343df-123">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="343df-124">(Her ikisi de tek bir tamsayı bir parametre olarak kabul etmeniz gerekir.)</span><span class="sxs-lookup"><span data-stu-id="343df-124">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="343df-125">Bu sorunun iki yaklaşım vardır, farklı adlar yöntemleri vermek biridir.</span><span class="sxs-lookup"><span data-stu-id="343df-125">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="343df-126">Önceki örnekte yapı iskelesi mekanizması ne yaptığını olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="343df-126">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="343df-127">Ancak, bu küçük bir sorunla sunar: ASP.NET eylem yöntemleri için bir URL kesimleri adıyla eşler ve bir yöntem yeniden adlandırırsanız, normal olarak Yönlendirme bu yöntem bulmak bağlanamayacak.</span><span class="sxs-lookup"><span data-stu-id="343df-127">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="343df-128">Eklemek için örnekte bkz çözümdür `ActionName("Delete")` özniteliğini `DeleteConfirmed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="343df-128">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="343df-129">Böylece bir POST isteği için /Delete/ içeren bir URL bulur, öznitelik eşleme için yönlendirme sistem gerçekleştirir. `DeleteConfirmed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="343df-129">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="343df-130">Aynı ad ve imza sahip yöntemleri için başka bir Ortak üstesinden gelme yapay fazladan dahil etmek için POST yöntemini imza değiştirmektir (kullanılmayan) parametre.</span><span class="sxs-lookup"><span data-stu-id="343df-130">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="343df-131">Biz eklendiğinde neler önceki postasına yaptığımız olan `notUsed` parametresi.</span><span class="sxs-lookup"><span data-stu-id="343df-131">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="343df-132">Aynı şeyi burada yapabilirsiniz `[HttpPost] Delete` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="343df-132">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="343df-133">Azure'a yayımlama</span><span class="sxs-lookup"><span data-stu-id="343df-133">Publish to Azure</span></span>

<span data-ttu-id="343df-134">Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) bu uygulama için Visual Studio kullanarak Azure yayımlama hakkında yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="343df-134">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="343df-135">Uygulama aynı zamanda gelen yayımlanabilir [komut satırı](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="343df-135">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="343df-136">Önceki</span><span class="sxs-lookup"><span data-stu-id="343df-136">Previous</span></span>](validation.md)
