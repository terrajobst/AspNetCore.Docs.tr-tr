---
title: Denetleyici yöntemlerine ve ASP.NET Core görünümler
author: rick-anderson
description: Denetleyici yöntemlerine, görünümler ve ASP.NET Core DataAnnotations ile çalışmayı öğrenin.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 6fe0a0e71079bebcbd3a76abee0f2917f562e766
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>Denetleyici yöntemlerine ve ASP.NET Core görünümler

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil. Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** iki sözcük olmalıdır.

![Dizin görünümü: yayın tarihi bir sözcük (boşluksuz) ve her film yayın tarihi 00: 00 süresini gösterir](working-with-sql/_static/m55.png)

Açık *Models/Movie.cs* dosya ve aşağıda gösterilen vurgulanan satırları ekleyin:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Bir kırmızı dalgalı satıra sağ tıklayın **> Hızlı Eylemler ve yapan yeniden düzenlemeler**.

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](controller-methods-views/_static/qa.png)


' A dokunun `using System.ComponentModel.DataAnnotations;`

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](controller-methods-views/_static/da.png)

  Visual studio adds `using System.ComponentModel.DataAnnotations;`.

Şimdi kaldırmak `using` gerekmeyen deyimleri. Bunlar varsayılan olarak açık bir gri yazı tipi gösterilir. Sağ herhangi bir yeri tıklatın *Movie.cs* dosya **> Kaldır ve kullanımları sıralama**.

![Kaldır ve kullanımları sıralama](controller-methods-views/_static/rm.png)

Güncelleştirilmiş kod:

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Önceki](working-with-sql.md)
> [sonraki](search.md)  
