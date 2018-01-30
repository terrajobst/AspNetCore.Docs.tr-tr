---
title: Bir ASP.NET Core MVC uygulama arama ekleme
author: rick-anderson
description: "Basit ASP.NET Core MVC uygulaması için arama eklemeyi gösterir"
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: d0e42242fd8c05b538e5e2e2686b77c95e9b90f4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="e7056-103">Not: "Hayalet" ve değil "hayalet" için arama gerekir böylece SQLlite büyük/küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="e7056-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="e7056-104">Değişiklik `<form>` içinde etiketi *Views\movie\Index.cshtml* belirtmek için Razor Görünüm `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="e7056-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="e7056-105">[Önceki - denetleyici yöntemlerine ve görünümleri](controller-methods-views.md)
[sonraki - alan ekleme](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="e7056-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
