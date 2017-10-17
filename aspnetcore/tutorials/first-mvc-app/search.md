---
title: Arama ekleme
author: rick-anderson
description: "Basit ASP.NET Core MVC uygulaması için arama eklemeyi gösterir"
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-8ef6-4628-855d-200206d962b9
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: f811cd0063404872b0e993a99e8a92cc354064ce
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

Hızlı bir şekilde adlandırabilirsiniz `searchString` parametresi `id` ile **yeniden adlandırma** komutu. Sağ tıklayın `searchString` **> yeniden adlandırma**.

![Bağlam menüsü](search/_static/rename.png)

Yeniden Adlandır hedefleri vurgulanır.

![Dizin ActionResult yöntemi vurgulanmış değişkeni gösteren kod Düzenleyicisi](search/_static/rename2.png)

Parametre değiştirme `id` ve tüm oluşumlarını `searchString` değiştirmek `id`.

![Kod Düzenleyicisi değişkeni gösteren kimliğine değiştirildi](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

IntelliSense bize işaretleme güncelleştirme nasıl yardımcı olduğunu dikkat edin.

![Form öğesi için öznitelikler listesinde seçili yöntemiyle IntelliSense bağlamsal menüsü](search/_static/int_m.png)

![IntelliSense bağlamsal menüsüyle yöntemi öznitelik değerleri listesinde seçili](search/_static/int_get.png)

Ayırt edici yazı tipini fark `<form>` etiketi. Etiket tarafından desteklenen farklı yazı gösterir [etiket Yardımcıları](../../mvc/views/tag-helpers/intro.md).

![Form etiketi mor metin ile](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Önceki](controller-methods-views.md)
[sonraki](new-field.md)  
