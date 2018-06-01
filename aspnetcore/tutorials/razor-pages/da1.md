---
title: Bir ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştir
author: rick-anderson
description: Bir ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirmek hakkında bilgi edinin.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 5/30/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: e36982f2b14ec65feb6be0c7f9e942c7a3c9e9ca
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34688438"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Bir ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştir

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil. Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki sözcük).

![Film uygulaması film verileri gösteren Chrome'da Aç](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Oluşturulan kod güncelleştir

Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterildiği vurgulanan satırları ekleyin:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]
::: moniker-end

Bir kırmızı dalgalı satıra sağ tıklayın > **hızlı Eylemler ve yapan yeniden düzenlemeler**.

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](da1/qa.png)

seçin `using System.ComponentModel.DataAnnotations;`

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](da1/da.png)

  Visual studio ekler `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Önceki: SQL Server yerel veritabanı ile çalışma](xref:tutorials/razor-pages/sql)
> [Ekle arama](xref:tutorials/razor-pages/search)
