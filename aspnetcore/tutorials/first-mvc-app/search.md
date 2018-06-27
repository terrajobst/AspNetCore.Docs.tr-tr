---
title: Bir ASP.NET Core MVC uygulamasına arama ekleyin
author: rick-anderson
description: Basit ASP.NET Core MVC uygulaması için arama eklemeyi gösterir
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960846"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

Hızlı bir şekilde adlandırabilirsiniz `searchString` parametresi `id` ile **yeniden adlandırma** komutu. Sağ tıklayın `searchString` **> yeniden adlandırma**.

![Bağlam menüsü](search/_static/rename.png)

Yeniden Adlandır hedefleri vurgulanır.

![Dizin ActionResult yöntemi vurgulanmış değişkeni gösteren kod Düzenleyicisi](search/_static/rename2.png)

Parametre değiştirme `id` ve tüm oluşumlarını `searchString` değiştirmek `id`.

![Kod Düzenleyicisi değişkeni gösteren kimliğine değiştirildi](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

IntelliSense bize işaretleme güncelleştirme nasıl yardımcı olduğunu dikkat edin.

![Form öğesi için öznitelikler listesinde seçili yöntemiyle IntelliSense bağlamsal menüsü](search/_static/int_m.png)

![IntelliSense bağlamsal menüsüyle yöntemi öznitelik değerleri listesinde seçili](search/_static/int_get.png)

Ayırt edici yazı tipini fark `<form>` etiketi. Etiket tarafından desteklenen farklı yazı gösterir [etiket Yardımcıları](~/mvc/views/tag-helpers/intro.md).

![Form etiketi mor metin ile](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Önceki](controller-methods-views.md)
> [sonraki](new-field.md)  
