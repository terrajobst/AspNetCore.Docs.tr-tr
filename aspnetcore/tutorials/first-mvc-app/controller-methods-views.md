---
title: Denetleyici yöntemlerine ve ASP.NET Core görünümler
author: rick-anderson
description: Denetleyici yöntemlerine, görünümler ve ASP.NET Core DataAnnotations ile çalışmayı öğrenin.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: e94cb877576a68540a565225b2b3d79f9be53327
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274820"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="90c45-103">Denetleyici yöntemlerine ve ASP.NET Core görünümler</span><span class="sxs-lookup"><span data-stu-id="90c45-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="90c45-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="90c45-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="90c45-105">Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil.</span><span class="sxs-lookup"><span data-stu-id="90c45-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="90c45-106">Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** iki sözcük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="90c45-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Dizin görünümü: yayın tarihi bir sözcük (boşluksuz) ve her film yayın tarihi 00: 00 süresini gösterir](working-with-sql/_static/m55.png)

<span data-ttu-id="90c45-108">Açık *Models/Movie.cs* dosya ve aşağıda gösterilen vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="90c45-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="90c45-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span><span class="sxs-lookup"><span data-stu-id="90c45-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="90c45-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="90c45-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="90c45-111">[Önceki](working-with-sql.md)
> [sonraki](search.md)</span><span class="sxs-lookup"><span data-stu-id="90c45-111">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
