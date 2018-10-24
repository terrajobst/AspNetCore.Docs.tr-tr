---
title: ASP.NET Core’a Giriş
author: rick-anderson
description: Modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, açık kaynak bir çerçeve olan ASP.NET Core’a giriş yapın.
ms.author: riande
ms.date: 9/28/2018
uid: index
ms.openlocfilehash: 69ab702e9d9f8d746b7bc546d4f2bbb831ff59c7
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911709"
---
# <a name="introduction-to-aspnet-core"></a>ASP.NET Core’a Giriş

[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Shaun Luttin](https://twitter.com/dicshaunary) tarafından hazırlanmıştır

ASP.NET Core, modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, [açık kaynak](https://github.com/aspnet/home) bir çerçevedir. ASP.NET Core ile şunları yapabilirsiniz:

* Web uygulamaları ve hizmetleri, [IoT](https://www.microsoft.com/internet-of-things/) uygulamaları ve mobil arka uçlar oluşturun.
* Windows, macOS ve Linux üzerinde tercih ettiğiniz geliştirme araçlarını kullanın.
* Buluta veya şirket içine dağıtın.
* [.NET Core veya .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) üzerinde çalıştırın.

## <a name="why-use-aspnet-core"></a>Neden ASP.NET Core kullanılmalı?

Milyonlarca geliştirici, web uygulamaları oluşturmak için [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) kullandı (ve kullanmaya devam ediyor). ASP.NET Core, ASP.NET 4.x sürümünün daha yalın, daha modüler bir çerçeve elde edilmesini sağlayan mimari değişikliklerle yeniden tasarlanmış halidir.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>ASP.NET Core MVC kullanarak web API'leri ve web kullanıcı arabirimi oluşturma

ASP.NET Core MVC, [web API’leri](xref:tutorials/index#build-web-apis) ve [web uygulamaları](xref:tutorials/index#build-web-apps) oluşturmaya yönelik özellikler sağlar:

* [Model-Görünüm-Denetleyici (MVC) deseni](xref:mvc/overview), web API'lerinin ve web uygulamalarının [sınanabilir](xref:test/index) olmasını sağlamanıza yardımcı olur.
* [Razor Sayfaları](xref:razor-pages/index) (ASP.NET Core 2.0 sürümünde yeni), web kullanıcı arabirimi oluşturmayı daha kolay ve üretken bir hale getiren, sayfa tabanlı bir programlama modelidir.
* [Razor işaretlemesi](xref:mvc/views/razor), [Razor Sayfaları](xref:razor-pages/index) ve [MVC görünümleri](xref:mvc/views/overview) için üretken bir söz dizimi sağlar.
* [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.
* [Birden çok veri biçimi ve içerik anlaşması](xref:web-api/advanced/formatting) için sunulan yerleşik destek, web API'lerinizin tarayıcılar ve mobil cihazlar dahil olmak üzere birçok çeşit istemciye ulaşmasına imkan tanır.
* [Model bağlama](xref:mvc/models/model-binding), HTTP isteklerinden alınan verileri otomatik olarak eylem metodu parametreleriyle eşleştirir.
* [Model doğrulama](xref:mvc/models/validation), otomatik olarak istemci ve sunucu tarafı doğrulama gerçekleştirir.

## <a name="client-side-development"></a>İstemci tarafı geliştirme

ASP.NET Core, popüler istemci tarafı çerçeveler ve kitaplıklar ([Angular](xref:spa/angular), [React](xref:spa/react) ve [Bootstrap](xref:client-side/bootstrap) dahil) ile sorunsuz bir şekilde tümleştirilir. Daha fazla bilgi için bkz. [İstemci tarafı geliştirme](xref:client-side/index).

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a>.NET Framework'ü hedefleyen ASP.NET Core

ASP.NET Core, .NET Core'u veya .NET Framework'ü hedefleyebilir. .NET Framework'ü hedefleyen ASP.NET Core uygulamaları platformlar arası çalışmaz; bunlar yalnızca Windows üzerinde çalışır. ASP.NET Core'da .NET Framework hedeflemesi desteğini kaldırmaya yönelik bir plan yoktur. Genel olarak, ASP.NET Core [.NET Standard](/dotnet/standard/net-standard) kitaplıklarından oluşturulmuştur. .NET Standard 2.0 ile yazılmış uygulamalar .NET Standard 2.0'ın desteklendiği her yerde çalıştırılır.

ASP.NET Core 2.x, .NET Standard 2.0 ile uyumlu .NET Framework sürümlerinde desteklenir:

* .NET Framework 4.7.1 ve üzeri önemle tavsiye edilir.
* .NET Framework 4.6.1 ve üzeri.

.NET Core hedeflemesinin çeşitli avantajları vardır ve bu avantajlar her yeni sürümle birlikte artmaktadır. .NET Framework'e göre .NET Core'un bazı avantajları şunlardır:

* Platformlar arası. macOS, Linux ve Windows üzerinde çalışır.
* Geliştirilmiş performans
* Yan yana sürüm oluşturma
* Yeni API'ler
* Açık kaynak

.NET Framework ile .NET Core arasındaki API açığını kapatmak için çok çalışıyoruz. [Windows Uyumluluk Paketi](/dotnet/core/porting/windows-compat-pack), .NET Core'da yalnızca Windows'a yönelik binlerce API'yi kullanıma sunmaktadır. Bu API'ler .NET Core 1.x'te sağlanmamıştı.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)
* [ASP.NET Core öğreticileri](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [ASP.NET Core temelleri](xref:fundamentals/index)
* [Haftalık ASP.NET topluluğu toplantısında](https://live.asp.net/), takımın ilerleme durumu ve planları ele alınır. Yeni bloglara ve üçüncü taraf yazılıma yer verilir.
