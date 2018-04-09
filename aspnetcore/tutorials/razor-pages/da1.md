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
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Bir ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştir

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil. Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki sözcük).

![Film uygulaması film verileri gösteren Chrome'da Aç](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Oluşturulan kod güncelleştir

Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterildiği vurgulanan satırları ekleyin:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Bir kırmızı dalgalı satıra sağ tıklayın > ** Hızlı Eylemler ve yapan yeniden düzenlemeler **.

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](da1/qa.png)

seçin `using System.ComponentModel.DataAnnotations;`

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](da1/da.png)

  Visual studio adds `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Önceki: SQL Server yerel veritabanı ile çalışma](xref:tutorials/razor-pages/sql)
> [Ekle arama](xref:tutorials/razor-pages/search)
