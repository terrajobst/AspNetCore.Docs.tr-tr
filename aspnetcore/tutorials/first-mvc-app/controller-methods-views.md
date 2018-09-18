---
title: Denetleyici metotları ve görünümleri ASP.NET Core
author: rick-anderson
description: Denetleyici yöntemlerinde, görünümler ve ASP.NET core'da DataAnnotations ile çalışmayı öğrenin.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 42a63044cd14873ff334a728c6c8304214ee8575
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011345"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="42ca4-103">Denetleyici metotları ve görünümleri ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42ca4-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="42ca4-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="42ca4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="42ca4-105">Film uygulaması için iyi bir başlangıç sahibiz ancak sunu ideal değildir.</span><span class="sxs-lookup"><span data-stu-id="42ca4-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="42ca4-106">Saat (12:00: 00'da aşağıdaki görüntüde) görmesini istemediğiniz ve **ReleaseDate** iki kelimeye olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="42ca4-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Dizini görüntüle: yayın tarihi bir sözcük (boşluk) ve her film yayın tarihi 12: 00 süresini gösterir.](working-with-sql/_static/m55.png)

<span data-ttu-id="42ca4-108">Açık *Models/Movie.cs* dosya ve aşağıda vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="42ca4-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="42ca4-109">[Önceki](working-with-sql.md)
> [İleri](search.md)</span><span class="sxs-lookup"><span data-stu-id="42ca4-109">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
