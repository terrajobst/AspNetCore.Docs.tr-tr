---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Paketleme ve küçültme varlıklar bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs
author: microsoft
description: Paketleme ve küçültme sitenizi hızlandırmak için yollarıdır. Paketleme sağlar, birden çok JavaScript (.js) dosyaları veya birden çok geçişli stil sayfası (... birleştirin
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572871"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="7691b-104">Paketleme ve ASP.NET Web sayfaları (Razor) sitesi varlıklar küçültme</span><span class="sxs-lookup"><span data-stu-id="7691b-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="7691b-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7691b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7691b-106">Paketleme ve küçültme sitenizi hızlandırmak için yollarıdır.</span><span class="sxs-lookup"><span data-stu-id="7691b-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="7691b-107">Paketleme birden çok JavaScript birleştirmek sağlar (*.js*) dosyaları veya birden çok geçişli stil sayfası (*.css*) bir birim yerine bir kerede olarak indirilebilir böylece dosyaları.</span><span class="sxs-lookup"><span data-stu-id="7691b-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="7691b-108">Küçültme boşluk sıkıştırır ve diğer olası küçük olarak indirilen dosyaları yapmak için sıkıştırma türleri gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="7691b-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="7691b-109">Gerekli öğeler içeren paketi henüz Microsoft WebMatrix içinde kullanılabilir olmadığından ASP.NET Web Pages 2 RC sürümünü paketleme ve küçültme desteklemez.</span><span class="sxs-lookup"><span data-stu-id="7691b-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="7691b-110">Bu rahatsızlıktan dolayı özür dileriz.</span><span class="sxs-lookup"><span data-stu-id="7691b-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="7691b-111">Paket, ASP.NET Web Pages 2 ve WebMatrix 2 son sürümde kullanılabilir olması beklenir.</span><span class="sxs-lookup"><span data-stu-id="7691b-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
