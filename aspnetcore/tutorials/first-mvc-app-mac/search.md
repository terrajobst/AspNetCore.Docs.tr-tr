---
title: Bir ASP.NET Core MVC uygulamasına arama ekleyin
author: rick-anderson
description: Basit ASP.NET Core MVC uygulaması için arama eklemeyi gösterir
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: aca0835340977605cc84fad1970ac30fa1a9872a
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961548"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="8cf23-103">Not: "Hayalet" ve değil "hayalet" için arama gerekir böylece SQLlite büyük/küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="8cf23-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="8cf23-104">Değişiklik `<form>` içinde etiketi *Views\movie\Index.cshtml* belirtmek için Razor Görünüm `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="8cf23-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="8cf23-105">[Önceki - denetleyici yöntemlerine ve görünümleri](controller-methods-views.md)
> [sonraki - alan ekleme](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="8cf23-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
