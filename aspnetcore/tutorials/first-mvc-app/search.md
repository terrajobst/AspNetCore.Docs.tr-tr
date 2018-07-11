---
title: Bir ASP.NET Core MVC uygulaması için arama Ekle
author: rick-anderson
description: Arama için basit bir ASP.NET Core MVC uygulaması ekleme işlemi açıklanır
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216202"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

Hızlı bir şekilde adlandırabilirsiniz `searchString` parametresi `id` ile **Yeniden Adlandır** komutu. Sağ tıklayın `searchString` **> Yeniden Adlandır**.

![Bağlamsal menü](search/_static/rename.png)

Yeniden adlandırma hedefleri vurgulanır.

![Dizin ActionResult yöntemi vurgulanmış değişkeni gösteren kod Düzenleyicisi](search/_static/rename2.png)

Parametre değiştirme `id` ve tüm oluşumlarını `searchString` değiştirmek `id`.

![Kod Düzenleyicisi'ni değişkeni gösteren kimliğine değiştirildi.](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

IntelliSense bize biçimlendirme güncelleştirme nasıl yardımcı olduğuna dikkat edin.

![Form öğesi için öznitelikler listesinde seçili yöntemiyle IntelliSense bağlamsal menü](search/_static/int_m.png)

![IntelliSense bağlam menüsü ile yöntemi öznitelik değerleri listesinde seçili](search/_static/int_get.png)

Farklı yazı tipini fark `<form>` etiketi. Etiket tarafından desteklenen farklı yazı gösterir [etiket Yardımcıları](~/mvc/views/tag-helpers/intro.md).

![Mor metinle form etiketi](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Önceki](controller-methods-views.md)
> [İleri](new-field.md)  
