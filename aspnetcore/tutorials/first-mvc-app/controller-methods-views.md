---
title: "Denetleyici yöntemlerine ve görünümler"
author: rick-anderson
description: "Denetleyici yöntemlerine, görünümleri ve DataAnnotations ile çalışma"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: cfe1838371226334d368dca13bba37c5b1f6fc39
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="04e63-103">Denetleyici yöntemlerine ve görünümler</span><span class="sxs-lookup"><span data-stu-id="04e63-103">Controller methods and views</span></span>

<span data-ttu-id="04e63-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="04e63-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="04e63-105">Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil.</span><span class="sxs-lookup"><span data-stu-id="04e63-105">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="04e63-106">Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** iki sözcük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="04e63-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Dizin görünümü: yayın tarihi bir sözcük (boşluksuz) ve her film yayın tarihi 00: 00 süresini gösterir](working-with-sql/_static/m55.png)

<span data-ttu-id="04e63-108">Açık *Models/Movie.cs* dosya ve aşağıda gösterilen vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="04e63-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="04e63-109">Bir kırmızı dalgalı satıra sağ tıklayın **> Hızlı Eylemler ve yapan yeniden düzenlemeler**.</span><span class="sxs-lookup"><span data-stu-id="04e63-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](controller-methods-views/_static/qa.png)


<span data-ttu-id="04e63-111">' A dokunun`using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="04e63-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](controller-methods-views/_static/da.png)

  <span data-ttu-id="04e63-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="04e63-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="04e63-114">Şimdi kaldırmak `using` gerekmeyen deyimleri.</span><span class="sxs-lookup"><span data-stu-id="04e63-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="04e63-115">Bunlar varsayılan olarak açık bir gri yazı tipi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="04e63-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="04e63-116">Sağ herhangi bir yeri tıklatın *Movie.cs* dosya **> Kaldır ve kullanımları sıralama**.</span><span class="sxs-lookup"><span data-stu-id="04e63-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Kaldır ve kullanımları sıralama](controller-methods-views/_static/rm.png)

<span data-ttu-id="04e63-118">Güncelleştirilmiş kod:</span><span class="sxs-lookup"><span data-stu-id="04e63-118">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="04e63-119">[Önceki](working-with-sql.md)
[sonraki](search.md)</span><span class="sxs-lookup"><span data-stu-id="04e63-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
