---
title: "Oluşturulan sayfaları güncelleştir"
author: rick-anderson
description: "Oluşturulan sayfaları ile daha iyi görünüm güncelleştirilemiyor."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a><span data-ttu-id="3342c-103">Oluşturulan sayfaları güncelleştir</span><span class="sxs-lookup"><span data-stu-id="3342c-103">Update the generated pages</span></span>

<span data-ttu-id="3342c-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3342c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3342c-105">Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil.</span><span class="sxs-lookup"><span data-stu-id="3342c-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="3342c-106">Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki sözcük).</span><span class="sxs-lookup"><span data-stu-id="3342c-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Film uygulaması film verileri gösteren Chrome'da Aç](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="3342c-108">Oluşturulan kod güncelleştir</span><span class="sxs-lookup"><span data-stu-id="3342c-108">Update the generated code</span></span>

<span data-ttu-id="3342c-109">Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterildiği vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3342c-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="3342c-110">Bir kırmızı dalgalı satıra sağ tıklayın > ** Hızlı Eylemler ve yapan yeniden düzenlemeler **.</span><span class="sxs-lookup"><span data-stu-id="3342c-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](da1/qa.png)

<span data-ttu-id="3342c-112">Seçin`using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="3342c-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](da1/da.png)

  <span data-ttu-id="3342c-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="3342c-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
<span data-ttu-id="3342c-115">[Önceki: SQL Server yerel veritabanı ile çalışma](xref:tutorials/razor-pages/sql)
[arama ekleme](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="3342c-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
