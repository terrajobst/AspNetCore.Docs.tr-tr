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
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="85f98-104">Denetleyici yöntemlerine ve görünümler</span><span class="sxs-lookup"><span data-stu-id="85f98-104">Controller methods and views</span></span>

<span data-ttu-id="85f98-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="85f98-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="85f98-106">Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil.</span><span class="sxs-lookup"><span data-stu-id="85f98-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="85f98-107">Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** iki sözcük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="85f98-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Dizin görünümü: yayın tarihi bir sözcük (boşluksuz) ve her film yayın tarihi 00: 00 süresini gösterir](working-with-sql/_static/m55.png)

<span data-ttu-id="85f98-109">Açık *Models/Movie.cs* dosya ve aşağıda gösterilen vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="85f98-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

<span data-ttu-id="85f98-110">[!code-csharp[Ana](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="85f98-110">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span></span>

<span data-ttu-id="85f98-111">Bir kırmızı dalgalı satıra sağ tıklayın **> Hızlı Eylemler ve yapan yeniden düzenlemeler**.</span><span class="sxs-lookup"><span data-stu-id="85f98-111">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](controller-methods-views/_static/qa.png)


<span data-ttu-id="85f98-113">' A dokunun`using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="85f98-113">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](controller-methods-views/_static/da.png)

  <span data-ttu-id="85f98-115">Visual studio ekler `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="85f98-115">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="85f98-116">Şimdi kaldırmak `using` gerekmeyen deyimleri.</span><span class="sxs-lookup"><span data-stu-id="85f98-116">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="85f98-117">Bunlar varsayılan olarak açık bir gri yazı tipi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="85f98-117">They show up by default in a light grey font.</span></span> <span data-ttu-id="85f98-118">Sağ herhangi bir yeri tıklatın *Movie.cs* dosya **> Kaldır ve kullanımları sıralama**.</span><span class="sxs-lookup"><span data-stu-id="85f98-118">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Kaldır ve kullanımları sıralama](controller-methods-views/_static/rm.png)

<span data-ttu-id="85f98-120">Güncelleştirilmiş kod:</span><span class="sxs-lookup"><span data-stu-id="85f98-120">The updated code:</span></span>

<span data-ttu-id="85f98-121">[!code-csharp[Ana](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="85f98-121">[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]</span></span>

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="85f98-122">[Önceki](working-with-sql.md)
[sonraki](search.md)</span><span class="sxs-lookup"><span data-stu-id="85f98-122">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
