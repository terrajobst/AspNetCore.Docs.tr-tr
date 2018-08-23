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
# <a name="optimize-build-performance-for-solution"></a>Çözüm yapı performansını iyileştirme
Visual Studio 2017 15,8 ve daha sonra yeni bir menü öğesini eklenen **Yapı > ASP.NET derleme > çözüm için derleme performansı en iyi duruma getirme**.

![Yeni menü öğesinin ekran görüntüsü](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET, ASP.NET projesi ile derleyici bir kopyasını taşır anlamına gelir. çalışma zamanında görünümlerini derler. Visual Studio'nun kopyalama, kopyalama derleyicinin eşleşmediğinde ancak, bir geliştirici makinesinde derleme performansınızı Artımlı derleme başına 1-3 saniye bazında etkilenir. Bu özellik, projenizin kopyasını derleyici Visual, artımlı derlemeleri hızlanacaktır Studio'nun eşleşecek şekilde güncelleştirir.

Bu yalnızca ASP.NET Framework projeleri için geçerlidir, ASP.NET Core için geçerli değildir.
