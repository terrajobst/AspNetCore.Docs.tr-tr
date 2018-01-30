---
title: Arama ekleme
author: rick-anderson
description: "Basit ASP.NET Core MVC uygulaması için arama eklemeyi gösterir"
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: e237b432e411faf6e8a1fe8c907c5daaf6eeef9e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="2f409-103">Not: "Hayalet" ve değil "hayalet" için arama gerekir böylece SQLlite büyük/küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="2f409-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="2f409-104">Değişiklik `<form>` içinde etiketi *Views\movie\Index.cshtml* belirtmek için Razor Görünüm `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="2f409-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="2f409-105">[Önceki - denetleyici yöntemlerine ve görünümleri](controller-methods-views.md)
[sonraki - alan ekleme](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="2f409-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>  
