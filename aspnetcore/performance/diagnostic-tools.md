---
title: Performans Tanılama araçları
author: mjrousos
description: ASP.NET Core uygulamalarında performans sorunlarını tanılamak için faydalı araçlar.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 0b1de069e7892fff451617f2c6570fa789808c4f
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099058"
---
# <a name="performance-diagnostic-tools"></a>Performans Tanılama araçları

Tarafından [Mike Rousos](https://github.com/mjrousos)

Bu makalede, ASP.NET Core performans sorunları tanılamaya yönelik araçları listeler.

## <a name="visual-studio-diagnostic-tools"></a>Visual Studio tanılama araçları

[Profil oluşturma ve tanılama araçları](/visualstudio/profiling) Visual Studio'da yerleşik olarak bulunan performans sorunlarını araştırma başlatmak için iyi bir yerdir. Bu araçlar, güçlü ve Visual Studio geliştirme ortamından kullanmak uygun. Araç, ASP.NET Core uygulamalarında CPU kullanımı, bellek kullanımı ve performans olayları analiz sağlar. Yerleşik olan kolay geliştirme zaman profil oluşturmayı kolaylaştırır.

Daha fazla bilgi kullanılabilir [Visual Studio belgeleri](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) uygulamanız için ayrıntılı performans verileri sağlar. Application Insights bilgileri otomatik olarak toplar yanıt hızlarını, hata oranları, bağımlılık yanıt süreleri ve daha fazlası. Application Insights, uygulamanızın özel olayları ve ölçümleri belirli oturum destekler.

Azure Application Insights ınsights izlenen uygulamaları sağlamak için birden çok yol sağlar:

- [Uygulama Haritası](/azure/application-insights/app-insights-app-map) – nokta performans sorunlarını veya sık erişimli hatası-nokta dağıtılmış uygulamaların tüm bileşenler genelinde yardımcı olur.
- [Application Insights portalında ölçümler dikey penceresine](/azure/application-insights/app-insights-metrics-explorer?toc=/azure/azure-monitor/toc.json) ölçülen değerleri gösterir ve olay sayar.
- [Application Insights portalında performans dikey penceresini](/azure/application-insights/app-insights-tutorial-performance):

  - İzlenen uygulamada farklı işlemlerin performans ayrıntılarını gösterir.
  - Tüm bölümleri/uzun bir süre için katkıda bulunan bağımlılıkları kontrol etmek için tek bir işlem araştırıp bulma sağlar.
  - Profiler performans izlemeleri üzerine toplamak için buradan çağrılabilir.

- [Azure Application Insights Profiler](/azure/azure-monitor/app/profiler) normal ve isteğe bağlı .NET uygulamaları profil oluşturma sağlar.  Azure portal gösterir performans izlemeleri çağrı yığınları ve etkin yolları ile yakalanır. İzleme dosyaları da PerfView kullanma daha ayrıntılı analiz için indirilebilir.

Application Insights, çeşitli ortamlarda kullanılabilir:

* Azure'da çalışması için en iyi duruma getirilmiş.
* Üretim, geliştirme ve hazırlama çalışır.
* Öğesinden yerel olarak çalıştığı [Visual Studio](/azure/application-insights/app-insights-visual-studio) veya diğer barındırma ortamlarında.

Daha fazla bilgi için [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) .NET Performans sorunlarını tanılamak için özel olarak .NET ekibi tarafından oluşturulan bir Performans Analizi aracıdır. PerfView CPU kullanımı, bellek ve GC davranışı, performans olayları ve duvar saati süresi analizini sağlar.

PerfView ve kullanmaya başlama hakkında daha fazla bilgi [PerfView video öğreticiler](http://channel9.msdn.com/Series/PerfView-Tutorial) veya Kullanıcı Kılavuzu Aracı'nda kullanılabilir okuyarak veya [github'da](https://github.com/Microsoft/perfview).

## <a name="windows-performance-toolkit"></a>Windows Performans Araç Seti

[Windows Performans Araç Seti](/windows-hardware/test/wpt/) (WPT) iki bileşenden oluşur: Windows Performans Kaydedicisi (WPR) ve Windows Performans Çözümleyicisi'ni (WPA). Windows işletim sistemlerinin ve uygulamalarının ayrıntılı performans Profil Araçları üretir. WPT veri görselleştirmenin daha kapsamlı yollar var, ancak veri toplanırken PerfView ait daha güçlü.

## <a name="perfcollect"></a>PerfCollect

PerfView bir .NET senaryoları için faydalı performans çözümleme aracı olsa da, Linux ortamlarında çalışan ASP.NET Core uygulamaları izlemeleri toplamak için kullanamazsınız yalnızca Windows üzerinde çalışır.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) profil oluşturma araçları, yerel Linux kullanan bir bash komut dosyası ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) ve [LTTng](https://lttng.org/)) tarafından PerfView çözümlenebilir Linux'ta izlemeleri toplamak için. PerfCollect, PerfView burada doğrudan kullanılamaz Linux ortamlarında performans sorunlarını gösterilmesi yararlıdır. Bunun yerine, PerfCollect izlemeleri ardından analiz .NET Core uygulamaları PerfView kullanan bir Windows bilgisayara toplayabilirsiniz.

Yükleme ve PerfCollect ile çalışmaya başlama hakkında daha fazla bilgi edinilebilir [github'da](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).

## <a name="other-third-party-performance-tools"></a>Diğer üçüncü taraf performans araçları

.NET Core uygulamaları performans araştırmada yararlı olan bazı üçüncü taraf performans araçları listeler.

- [MiniProfiler](https://miniprofiler.com/)
- dotTrace ve dotMemory JetBrains gelen
- Intel'in VTune
