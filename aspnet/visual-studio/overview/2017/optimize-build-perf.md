---
uid: visual-studio/overview/2017/optimize-build-perf
title: Çözüm yapı performansını iyileştirme
author: tfitzmac
description: Çözüm yapı performansını iyileştirme
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41910033"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="177b0-103">Çözüm yapı performansını iyileştirme</span><span class="sxs-lookup"><span data-stu-id="177b0-103">Optimize build performance for solution</span></span>
<span data-ttu-id="177b0-104">Visual Studio 2017 15,8 ve daha sonra yeni bir menü öğesini eklenen **Yapı > ASP.NET derleme > çözüm için derleme performansı en iyi duruma getirme**.</span><span class="sxs-lookup"><span data-stu-id="177b0-104">Visual Studio 2017 15.8 and later added a new menu item under **Build > ASP.NET Compilation > Optimize Build Performance for Solution**.</span></span>

![Yeni menü öğesinin ekran görüntüsü](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="177b0-106">ASP.NET, ASP.NET projesi ile derleyici bir kopyasını taşır anlamına gelir. çalışma zamanında görünümlerini derler.</span><span class="sxs-lookup"><span data-stu-id="177b0-106">ASP.NET compiles its views at runtime, which means your ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="177b0-107">Visual Studio'nun kopyalama, kopyalama derleyicinin eşleşmediğinde ancak, bir geliştirici makinesinde derleme performansınızı Artımlı derleme başına 1-3 saniye bazında etkilenir.</span><span class="sxs-lookup"><span data-stu-id="177b0-107">However, on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, your build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="177b0-108">Bu özellik, projenizin kopyasını derleyici Visual, artımlı derlemeleri hızlanacaktır Studio'nun eşleşecek şekilde güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="177b0-108">This feature will update your project's copy of the compiler to match Visual Studio's which should speed up incremental builds.</span></span>

<span data-ttu-id="177b0-109">Bu yalnızca ASP.NET Framework projeleri için geçerlidir, ASP.NET Core için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="177b0-109">This is applicable to ASP.NET Framework projects only, it does not apply to ASP.NET Core.</span></span>
