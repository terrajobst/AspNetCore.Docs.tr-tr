---
title: Bir ASP.NET Core MVC uygulama arama ekleme
author: rick-anderson
description: Basit ASP.NET Core MVC uygulaması için arama eklemeyi gösterir
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 4175d4dfd03d173f7025aff3b51d255bb1c213ee
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274488"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

Not: "Hayalet" ve değil "hayalet" için arama gerekir böylece SQLlite büyük/küçük harfe duyarlı.

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

Değişiklik `<form>` içinde etiketi *Views\movie\Index.cshtml* belirtmek için Razor Görünüm `method="get"`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Önceki - denetleyici yöntemlerine ve görünümleri](controller-methods-views.md)
> [sonraki - alan ekleme](new-field.md)
