---
title: "ASP.NET Core’a Giriş"
author: rick-anderson
description: "ASP.NET Core ile ilgili temel bilgiler sağlanır."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a>ASP.NET Core’a Giriş

[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Shaun Luttin](https://twitter.com/dicshaunary) tarafından hazırlanmıştır

ASP.NET Core, modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, [açık kaynak](https://github.com/aspnet/home) bir çerçevedir. ASP.NET Core ile şunları yapabilirsiniz:

* Web uygulamaları ve hizmetleri, [IoT](https://www.microsoft.com/en-us/internet-of-things/) uygulamaları ve mobil arka uçlar oluşturun.
* Windows, macOS ve Linux üzerinde tercih ettiğiniz geliştirme araçlarını kullanın.
* Buluta veya şirket içine dağıtın
* [.NET Core veya .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) üzerinde çalıştırın.

## <a name="why-use-aspnet-core"></a>Neden ASP.NET Core kullanılmalı?

Milyonlarca geliştirici, web uygulamaları oluşturmak için ASP.NET’i kullandı (ve kullanmaya devam ediyor). ASP.NET Core, ASP.NET’in daha yalın ve modüler bir çerçeve elde edilmesini sağlayan mimari değişikliklerle yeniden tasarlanmış sürümüdür.

ASP.NET Core aşağıdaki avantajları sağlar:

* Web kullanıcı arabirimi ve web API’leri oluşturmak için birleşik bir öykü.
* [Modern istemci tarafı çerçeveler](xref:client-side/index) ile geliştirme iş akışlarının tümleştirilmesi.
* Bulutta kullanıma hazır, ortam tabanlı bir [yapılandırma sistemi](xref:fundamentals/configuration/index).
* Yerleşik [bağımlılık ekleme](xref:fundamentals/dependency-injection).
* Basit, yüksek performanslı ve modüler bir HTTP istek işlem hattı.
* IIS’de veya kendi işleminizde barındırma olanağı.
* Gerçek yan yana uygulama sürümü oluşturmayı destekleyen [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) üzerinde çalışabilir.
* Modern web geliştirmeyi basitleştiren araçlar.
* Windows, macOS ve Linux üzerinde derleyip çalıştırma olanağı.
* Açık kaynak ve topluluk odaklı.

ASP.NET Core tamamen [NuGet](https://www.nuget.org/) paketleri olarak sunulur. Bu, uygulamanızı yalnızca gereksinim duyduğunuz NuGet paketlerini içerecek şekilde en iyi duruma getirmenize imkan sağlar. Uygulama yüzeyinin dar olmasının daha sıkı güvenlik, daha az bakım ve gelişmiş performans gibi avantajları vardır.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>ASP.NET Core MVC kullanarak web API'leri ve web kullanıcı arabirimi oluşturma

ASP.NET Core MVC, [web API'leri](xref:tutorials/index#building-web-apis) ve [web uygulamaları](xref:tutorials/index#building-web-applications) oluşturmanıza yardımcı olan özellikler sağlar:

* [Model-Görünüm-Denetleyici (MVC) deseni](xref:mvc/overview), web API'lerinin ve web uygulamalarının [sınanabilir](testing/index.md) olmasını sağlamanıza yardımcı olur.
* [Razor Sayfaları](xref:mvc/razor-pages/index) (2.0 sürümünde yeni), web kullanıcı arabirimi oluşturmayı daha kolay ve üretken bir hale getiren, sayfa tabanlı bir programlama modelidir.
* [Razor söz dizimi](xref:mvc/views/razor), [Razor Sayfaları](xref:mvc/razor-pages/index) ve [MVC Görünümleri](xref:mvc/views/overview) için üretken bir dil sağlar.
* [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.
* [Birden çok veri biçimi ve içerik anlaşması](mvc/models/formatting.md) için sunulan yerleşik destek, web API'lerinizin tarayıcılar ve mobil cihazlar dahil olmak üzere birçok çeşit istemciye ulaşmasına imkan tanır.
* [Model Bağlama](xref:mvc/models/model-binding), HTTP isteklerinden alınan verileri otomatik olarak eylem metodu parametreleriyle eşleştirir.
* [Model Doğrulama](xref:mvc/models/validation), otomatik olarak istemci ve sunucu taraflı doğrulamayı gerçekleştirir.

## <a name="client-side-development"></a>İstemci tarafı geliştirme

ASP.NET Core ürünü, [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) ve [Bootstrap](xref:client-side/bootstrap) dahil olmak üzere çeşitli istemci tarafı çerçevelerle sorunsuz olarak tümleştirilebilecek şekilde tasarlanmıştır. Daha fazla ayrıntı için bkz. [İstemci tarafı geliştirme](client-side/index.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [ASP.NET Core öğreticileri](xref:tutorials/index)
* [ASP.NET Core temelleri](xref:fundamentals/index)
* [Haftalık ASP.NET topluluğu toplantısında](https://live.asp.net/), takımın ilerleme durumu ve planlarının ele alınmasının yanı sıra yeni blog gönderileri ve üçüncü taraf yazılımlar sunulur.
