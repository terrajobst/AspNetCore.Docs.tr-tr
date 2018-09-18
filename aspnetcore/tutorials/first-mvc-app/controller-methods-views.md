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
# <a name="controller-methods-and-views-in-aspnet-core"></a>Denetleyici metotları ve görünümleri ASP.NET Core

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film uygulaması için iyi bir başlangıç sahibiz ancak sunu ideal değildir. Saat (12:00: 00'da aşağıdaki görüntüde) görmesini istemediğiniz ve **ReleaseDate** iki kelimeye olmalıdır.

![Dizini görüntüle: yayın tarihi bir sözcük (boşluk) ve her film yayın tarihi 12: 00 süresini gösterir.](working-with-sql/_static/m55.png)

Açık *Models/Movie.cs* dosya ve aşağıda vurgulanan satırları ekleyin:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Önceki](working-with-sql.md)
> [İleri](search.md)  
