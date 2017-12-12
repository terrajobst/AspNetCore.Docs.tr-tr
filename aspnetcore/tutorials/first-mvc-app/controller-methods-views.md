---
title: "Denetleyici yöntemlerine ve görünümler"
author: rick-anderson
description: "Denetleyici yöntemlerine, görünümleri ve DataAnnotations ile çalışma"
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views"></a>Denetleyici yöntemlerine ve görünümler

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil. Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** iki sözcük olmalıdır.

![Dizin görünümü: yayın tarihi bir sözcük (boşluksuz) ve her film yayın tarihi 00: 00 süresini gösterir](working-with-sql/_static/m55.png)

Açık *Models/Movie.cs* dosya ve aşağıda gösterilen vurgulanan satırları ekleyin:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Bir kırmızı dalgalı satıra sağ tıklayın **> Hızlı Eylemler ve yapan yeniden düzenlemeler**.

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](controller-methods-views/_static/qa.png)


' A dokunun`using System.ComponentModel.DataAnnotations;`

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](controller-methods-views/_static/da.png)

  Visual studio ekler `using System.ComponentModel.DataAnnotations;`.

Şimdi kaldırmak `using` gerekmeyen deyimleri. Bunlar varsayılan olarak açık bir gri yazı tipi gösterilir. Sağ herhangi bir yeri tıklatın *Movie.cs* dosya **> Kaldır ve kullanımları sıralama**.

![Kaldır ve kullanımları sıralama](controller-methods-views/_static/rm.png)

Güncelleştirilmiş kod:

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Önceki](working-with-sql.md)
[sonraki](search.md)  
