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

<span data-ttu-id="02139-103">Hızlı bir şekilde adlandırabilirsiniz `searchString` parametresi `id` ile **Yeniden Adlandır** komutu.</span><span class="sxs-lookup"><span data-stu-id="02139-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="02139-104">Sağ tıklayın `searchString` **> Yeniden Adlandır**.</span><span class="sxs-lookup"><span data-stu-id="02139-104">Right click on `searchString` **> Rename**.</span></span>

![Bağlamsal menü](search/_static/rename.png)

<span data-ttu-id="02139-106">Yeniden adlandırma hedefleri vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="02139-106">The rename targets are highlighted.</span></span>

![Dizin ActionResult yöntemi vurgulanmış değişkeni gösteren kod Düzenleyicisi](search/_static/rename2.png)

<span data-ttu-id="02139-108">Parametre değiştirme `id` ve tüm oluşumlarını `searchString` değiştirmek `id`.</span><span class="sxs-lookup"><span data-stu-id="02139-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Kod Düzenleyicisi'ni değişkeni gösteren kimliğine değiştirildi.](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="02139-110">IntelliSense bize biçimlendirme güncelleştirme nasıl yardımcı olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="02139-110">Notice how intelliSense helps us update the markup.</span></span>

![Form öğesi için öznitelikler listesinde seçili yöntemiyle IntelliSense bağlamsal menü](search/_static/int_m.png)

![IntelliSense bağlam menüsü ile yöntemi öznitelik değerleri listesinde seçili](search/_static/int_get.png)

<span data-ttu-id="02139-113">Farklı yazı tipini fark `<form>` etiketi.</span><span class="sxs-lookup"><span data-stu-id="02139-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="02139-114">Etiket tarafından desteklenen farklı yazı gösterir [etiket Yardımcıları](~/mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="02139-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![Mor metinle form etiketi](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="02139-116">[Önceki](controller-methods-views.md)
> [İleri](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="02139-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
