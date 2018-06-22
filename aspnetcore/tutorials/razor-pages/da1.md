---
title: Bir ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştir
author: rick-anderson
description: Bir ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirmek hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 55ff98712da314e28e50a1b1b1e04530d5b3fedd
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278076"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="a9372-103">Bir ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştir</span><span class="sxs-lookup"><span data-stu-id="a9372-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="a9372-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a9372-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a9372-105">Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil.</span><span class="sxs-lookup"><span data-stu-id="a9372-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="a9372-106">Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki sözcük).</span><span class="sxs-lookup"><span data-stu-id="a9372-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Film uygulaması film verileri gösteren Chrome'da Aç](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="a9372-108">Oluşturulan kod güncelleştir</span><span class="sxs-lookup"><span data-stu-id="a9372-108">Update the generated code</span></span>

<span data-ttu-id="a9372-109">Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterildiği vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a9372-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="a9372-110">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="a9372-110">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a9372-111">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]</span><span class="sxs-lookup"><span data-stu-id="a9372-111">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]</span></span>
::: moniker-end

<span data-ttu-id="a9372-112">Bir kırmızı dalgalı satıra sağ tıklayın > **hızlı Eylemler ve yapan yeniden düzenlemeler**.</span><span class="sxs-lookup"><span data-stu-id="a9372-112">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](da1/qa.png)

<span data-ttu-id="a9372-114">seçin `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="a9372-114">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](da1/da.png)

  <span data-ttu-id="a9372-116">Visual studio ekler `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="a9372-116">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="a9372-117">[Önceki: SQL Server yerel veritabanı ile çalışma](xref:tutorials/razor-pages/sql)
> [Ekle arama](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="a9372-117">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
