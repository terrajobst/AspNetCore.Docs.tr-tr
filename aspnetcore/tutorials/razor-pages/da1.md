---
title: Bir ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştir
author: rick-anderson
description: Bir ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirmek hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 0207696af14305a1b549cf55da1b42fa99d01776
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="74664-103">Bir ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştir</span><span class="sxs-lookup"><span data-stu-id="74664-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="74664-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="74664-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="74664-105">Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil.</span><span class="sxs-lookup"><span data-stu-id="74664-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="74664-106">Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki sözcük).</span><span class="sxs-lookup"><span data-stu-id="74664-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Film uygulaması film verileri gösteren Chrome'da Aç](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="74664-108">Oluşturulan kod güncelleştir</span><span class="sxs-lookup"><span data-stu-id="74664-108">Update the generated code</span></span>

<span data-ttu-id="74664-109">Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterildiği vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="74664-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="74664-110">Bir kırmızı dalgalı satıra sağ tıklayın > ** Hızlı Eylemler ve yapan yeniden düzenlemeler **.</span><span class="sxs-lookup"><span data-stu-id="74664-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](da1/qa.png)

<span data-ttu-id="74664-112">seçin `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="74664-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](da1/da.png)

  <span data-ttu-id="74664-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="74664-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="74664-115">[Önceki: SQL Server yerel veritabanı ile çalışma](xref:tutorials/razor-pages/sql)
> [Ekle arama](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="74664-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
