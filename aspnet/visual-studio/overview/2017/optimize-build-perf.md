---
uid: visual-studio/overview/2017/optimize-build-perf
title: Çözüm yapı performansını iyileştirme
author: AngelosP
description: Çözüm yapı performansını iyileştirme
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312147"
---
# <a name="optimize-build-performance-for-solution"></a>Çözüm yapı performansını iyileştirme

Visual Studio 2017 15,8 veya daha sonra bir menü öğesi ekleme: **derleme** > **ASP.NET derleme** > **çözümü için derleme performansı en iyi duruma getirme**.

![Yeni menü öğesinin ekran görüntüsü](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET, ASP.NET projesi ile bir kopyasını derleyici taşıyan anlamına gelir. çalışma zamanında görünümlerini derler. Visual Studio'nun kopyalama, kopyalama derleyicinin eşleşmediğinde ancak bir geliştirici makinesinde yapı performansını Artımlı derleme başına 1-3 saniye bazında etkilenir. Bu özellik, yalnızca artımlı derlemeleri genellikle hızlandıran projenizin kopyasını Visual Studio'nun eşleştirmek için derleyicinin güncelleştirir.

**ASP.NET Framework 4.7.1 uygulanabilir veya daha sonra yalnızca projeleri, ASP.NET Core için geçerli değildir.**
