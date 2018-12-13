---
title: Performans Tanılama araçları
author: mjrousos
description: ASP.NET Core uygulamalarında performans sorunlarını tanılamak için faydalı araçlar.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329215"
---
# <a name="performance-diagnostic-tools"></a>Performans Tanılama araçları

Tarafından [Mike Rousos](https://github.com/mjrousos)

Bu makalede, ASP.NET Core performans sorunları tanılamaya yönelik araçları listeler.

## <a name="visual-studio-diagnostic-tools"></a>Visual Studio tanılama araçları

[Profil oluşturma ve tanılama araçları](/visualstudio/profiling) Visual Studio'da yerleşik olarak bulunan performans sorunlarını araştırma başlatmak için iyi bir yerdir. Bu araçlar, güçlü ve Visual Studio geliştirme ortamından kullanmak uygun. Araç, ASP.NET Core uygulamalarında CPU kullanımı, bellek kullanımı ve performans olayları analiz sağlar.

Daha fazla bilgi kullanılabilir [Visual Studio belgeleri](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) uygulamanız için ayrıntılı performans verileri sağlar. Application Insights bilgileri otomatik olarak toplar yanıt hızlarını, hata oranları, bağımlılık yanıt süreleri ve daha fazlası. Application Insights, uygulamanızın özel olayları ve ölçümleri belirli oturum destekler.

Application Insights, çeşitli ortamlarda kullanılabilir:

* Azure'da çalışması için en iyi duruma getirilmiş.
* Üretim, geliştirme ve hazırlama çalışır.
* Öğesinden yerel olarak çalıştığı [Visual Studio](/azure/application-insights/app-insights-visual-studio) veya diğer barındırma ortamlarında.

Daha fazla bilgi için [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) .NET Performans sorunlarını tanılamak için özel olarak .NET ekibi tarafından oluşturulan bir Performans Analizi aracıdır. PerfView CPU kullanımı, bellek ve GC davranışı, performans olayları ve duvar saati süresi analizini sağlar.

PerfView ve kullanmaya başlama hakkında daha fazla bilgi [PerfView video öğreticiler](http://channel9.msdn.com/Series/PerfView-Tutorial) veya Kullanıcı Kılavuzu Aracı'nda kullanılabilir okuyarak veya [github'da](https://github.com/Microsoft/perfview).

## <a name="perfcollect"></a>PerfCollect

PerfView bir .NET senaryoları için faydalı performans çözümleme aracı olsa da, Linux ortamlarında çalışan ASP.NET Core uygulamaları izlemeleri toplamak için kullanamazsınız yalnızca Windows üzerinde çalışır.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) profil oluşturma araçları, yerel Linux kullanan bir bash komut dosyası ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) ve [LTTng](https://lttng.org/)) tarafından PerfView çözümlenebilir Linux'ta izlemeleri toplamak için. PerfCollect, PerfView burada doğrudan kullanılamaz Linux ortamlarında performans sorunlarını gösterilmesi yararlıdır. Bunun yerine, PerfCollect izlemeleri ardından analiz .NET Core uygulamaları PerfView kullanan bir Windows bilgisayara toplayabilirsiniz.

Yükleme ve PerfCollect ile çalışmaya başlama hakkında daha fazla bilgi edinilebilir [github'da](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).
