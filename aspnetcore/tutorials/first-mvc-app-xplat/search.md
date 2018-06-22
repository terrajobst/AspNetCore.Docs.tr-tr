---
title: Arama ekleme
author: rick-anderson
description: Basit ASP.NET Core MVC uygulaması için arama eklemeyi gösterir
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: dc84eb38c0487d90451979ec9572bf1641571357
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276012"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="e7c0f-103">Not: "Hayalet" ve değil "hayalet" için arama gerekir böylece SQLlite büyük/küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="e7c0f-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="e7c0f-104">Değişiklik `<form>` içinde etiketi *Views\movie\Index.cshtml* belirtmek için Razor Görünüm `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="e7c0f-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="e7c0f-105">[Önceki - denetleyici yöntemlerine ve görünümleri](controller-methods-views.md)
> [sonraki - alan ekleme](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="e7c0f-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>  
