---
title: "ASP.NET Core’a Giriş"
author: rick-anderson
description: "ASP.NET Core ile ilgili temel bilgiler sağlanır."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: 3a18ed30819a3d395e9bfb5dba0547667a4425e8
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="introduction-to-aspnet-core"></a>ASP.NET Core’a Giriş

[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Shaun Luttin](https://twitter.com/dicshaunary) tarafından hazırlanmıştır

ASP.NET Core, modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, [açık kaynak](https://github.com/aspnet/home) bir çerçevedir. ASP.NET Core ile şunları yapabilirsiniz:

* Web uygulamaları ve hizmetleri, [IoT](https://www.microsoft.com/internet-of-things/) uygulamaları ve mobil arka uçlar oluşturun.
* Windows, macOS ve Linux üzerinde tercih ettiğiniz geliştirme araçlarını kullanın.
* Buluta veya şirket içine dağıtın.
* [.NET Core veya .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) üzerinde çalıştırın.

## <a name="why-use-aspnet-core"></a>Neden ASP.NET Core kullanılmalı?

Milyonlarca geliştirici, web uygulamaları oluşturmak için [ASP.NET 4.x](https://docs.microsoft.com/en-us/aspnet/overview) kullandı (ve kullanmaya devam ediyor). ASP.NET Core, ASP.NET 4.x sürümünün daha yalın, daha modüler bir çerçeve elde edilmesini sağlayan mimari değişikliklerle yeniden tasarlanmış halidir.

ASP.NET Core aşağıdaki avantajları sağlar:

* Web kullanıcı arabirimi ve web API’leri oluşturmak için birleşik bir öykü.
* [Modern istemci tarafı çerçeveler](xref:client-side/index) ile geliştirme iş akışlarının tümleştirilmesi.
* Bulutta kullanıma hazır, ortam tabanlı bir [yapılandırma sistemi](xref:fundamentals/configuration/index).
* Yerleşik [bağımlılık ekleme](xref:fundamentals/dependency-injection).
* Basit, [yüksek performanslı](https://github.com/aspnet/benchmarks) ve modüler bir HTTP istek işlem hattı.
* [IIS](xref:publishing/iis), [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) ve [Docker](xref:publishing/docker)’da veya kendi işleminizde barındırma olanağı.
* [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) hedeflenirken yan yana uygulama sürümü oluşturma.
* Modern web geliştirmeyi basitleştiren araçlar.
* Windows, macOS ve Linux üzerinde derleyip çalıştırma olanağı.
* Açık kaynak ve [topluluk odaklı](https://live.asp.net/).

ASP.NET Core tamamen [NuGet](https://www.nuget.org/) paketleri olarak sunulur. Bu, uygulamanızı yalnızca gerekli NuGet paketlerini içerecek şekilde iyileştirmenize imkan sağlar. Aslına bakılırsa, .NET Core’u hedefleyen ASP.NET Core 2.x uygulamaları yalnızca [tek bir NuGet paketi](xref:fundamentals/metapackage) gerektirir. Uygulama yüzeyinin dar olmasının daha sıkı güvenlik, daha az bakım ve gelişmiş performans gibi avantajları vardır.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>ASP.NET Core MVC kullanarak web API'leri ve web kullanıcı arabirimi oluşturma

ASP.NET Core MVC, [web API’leri](xref:tutorials/index#building-web-apis) ve [web uygulamaları](xref:tutorials/index#building-web-applications) oluşturmaya yönelik özellikler sağlar:

* [Model-Görünüm-Denetleyici (MVC) deseni](xref:mvc/overview), web API'lerinin ve web uygulamalarının [sınanabilir](testing/index.md) olmasını sağlamanıza yardımcı olur.
* [Razor Sayfaları](xref:mvc/razor-pages/index) (ASP.NET Core 2.0 sürümünde yeni), web kullanıcı arabirimi oluşturmayı daha kolay ve üretken bir hale getiren, sayfa tabanlı bir programlama modelidir.
* [Razor işaretlemesi](xref:mvc/views/razor), [Razor Sayfaları](xref:mvc/razor-pages/index) ve [MVC görünümleri](xref:mvc/views/overview) için üretken bir söz dizimi sağlar.
* [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.
* [Birden çok veri biçimi ve içerik anlaşması](mvc/models/formatting.md) için sunulan yerleşik destek, web API'lerinizin tarayıcılar ve mobil cihazlar dahil olmak üzere birçok çeşit istemciye ulaşmasına imkan tanır.
* [Model bağlama](xref:mvc/models/model-binding), HTTP isteklerinden alınan verileri otomatik olarak eylem metodu parametreleriyle eşleştirir.
* [Model doğrulama](xref:mvc/models/validation), otomatik olarak istemci ve sunucu tarafı doğrulama gerçekleştirir.

## <a name="client-side-development"></a>İstemci tarafı geliştirme

ASP.NET Core, popüler istemci tarafı çerçeveler ve kitaplıklar ([Angular](xref:spa/angular), [React](xref:spa/react) ve [Bootstrap](xref:client-side/bootstrap) dahil) ile sorunsuz bir şekilde tümleştirilir. Daha fazla ayrıntı için bkz. [İstemci tarafı geliştirme](xref:client-side/index).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [ASP.NET Core öğreticileri](xref:tutorials/index)
* [ASP.NET Core temelleri](xref:fundamentals/index)
* [Haftalık ASP.NET topluluğu toplantısında](https://live.asp.net/), takımın ilerleme durumu ve planları ele alınır. Yeni bloglara ve üçüncü taraf yazılıma yer verilir.
