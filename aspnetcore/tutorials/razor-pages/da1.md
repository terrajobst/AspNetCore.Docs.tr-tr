---
title: ASP.NET Core uygulaması oluşturulan sayfaları güncelleştirme
author: rick-anderson
description: ASP.NET Core uygulaması oluşturulan sayfaları güncelleştirme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 7633c0a40764cc18a656f0497e3280e4067cb59f
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045581"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="e1ae1-103">ASP.NET Core uygulaması oluşturulan sayfaları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="e1ae1-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="e1ae1-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e1ae1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e1ae1-105">Film uygulaması için iyi bir başlangıç sahibiz ancak sunu ideal değildir.</span><span class="sxs-lookup"><span data-stu-id="e1ae1-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="e1ae1-106">Saat (12:00: 00'da aşağıdaki görüntüde) görmesini istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki kelimeye).</span><span class="sxs-lookup"><span data-stu-id="e1ae1-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Film verileri gösteren Chrome'da açık film uygulaması](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="e1ae1-108">Oluşturulan kodu güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="e1ae1-108">Update the generated code</span></span>

<span data-ttu-id="e1ae1-109">Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterilen vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e1ae1-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="e1ae1-110">Kırmızı dalgalı çizgi üzerinde sağ tıklayın > **hızlı Eylemler ve yeniden düzenlemeler**.</span><span class="sxs-lookup"><span data-stu-id="e1ae1-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Bağlamsal menüyü gösterir \*\* > Hızlı Eylemler ve yeniden düzenlemeler \*\*.](da1/qa.png)

<span data-ttu-id="e1ae1-112">Seçin `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="e1ae1-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![listenin en üstünde System.ComponentModel.DataAnnotations kullanma](da1/da.png)

  <span data-ttu-id="e1ae1-114">Visual Studio ekler `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="e1ae1-114">Visual Studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="e1ae1-115">[Önceki: SQL Server LocalDB ile çalışma](xref:tutorials/razor-pages/sql)
> [arama Ekle](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="e1ae1-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
