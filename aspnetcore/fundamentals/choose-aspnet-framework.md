---
title: Seçin arasında ASP.NET 4.x ve ASP.NET Core
author: rick-anderson
description: ASP.NET Core vs açıklar. ASP.NET 4.x ve bunlar arasında seçim yapma.
ms.author: riande
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: fa281a068dfdcd8a7c45dd5d0a0c3084c4bc8dbc
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549078"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a>Seçin arasında ASP.NET 4.x ve ASP.NET Core

ASP.NET Core, ASP.NET'in yeniden 4.x. Bu makalede, aralarındaki farklılıklar listelenmektedir.

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core Windows, macOS veya Linux'ta modern, bulut tabanlı web uygulamaları oluşturmaya yönelik açık kaynaklı, platformlar arası bir çerçevedir.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a>ASP.NET 4.x

ASP.NET 4.x, kurumsal sınıf, oluşturmak için gereken hizmetleri Windows server tabanlı web apps'de sağlayan olgun bir altyapısıdır.

## <a name="framework-selection"></a>Framework seçimi

Aşağıdaki tabloda karşılaştırılmıştır ASP.NET Core, ASP.NET 4.x.

| ASP.NET Core | ASP.NET 4.x |
|---|---|
|Windows, macOS veya Linux için derleme|Windows için derleme|
|[Razor sayfaları](xref:razor-pages/index) itibarıyla ASP.NET Core Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır 2.x. Ayrıca bkz: [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), ve [SignalR](xref:signalr/introduction).|Kullanım [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [Web kancaları](/aspnet/webhooks/), veya [Web sayfaları](/aspnet/web-pages)|
|Makine başına birden çok sürümü|Makine başına bir sürümü|
|Visual Studio ile geliştirin [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/), veya [Visual Studio Code](https://code.visualstudio.com/) kullanarak C# veyaF#|Visual Studio kullanarak ile geliştirme C#, VB, veyaF#|
|Daha yüksek performansa ASP.NET 4.x|İyi bir performans|
|[.NET Framework veya .NET Core çalışma zamanı seçin](/dotnet/articles/standard/choosing-core-framework-server)|.NET Framework çalışma zamanını kullanma|

Bkz: [.NET Framework'ü hedefleyen ASP.NET Core](xref:index#target-framework) .NET Framework üzerinde ASP.NET Core 2.x desteği hakkında daha fazla bilgi için.

## <a name="aspnet-core-scenarios"></a>ASP.NET Core senaryoları

* [Razor sayfaları](xref:razor-pages/index) itibarıyla ASP.NET Core Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır 2.x.
* [Web siteleri](xref:tutorials/first-mvc-app/index)
* [API'leri](xref:tutorials/first-web-api)
* [Gerçek zamanlı](xref:signalr/index)
* [Azure'da ASP.NET Core uygulaması dağıtma](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>ASP.NET 4.x senaryoları

* [Web siteleri](/aspnet/mvc)
* [API'leri](/aspnet/web-api)
* [Gerçek zamanlı](/aspnet/signalr)
* [Azure'da ASP.NET 4.x web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET'e giriş](/aspnet/overview)
* [ASP.NET Core'a giriş](xref:index)
* <xref:host-and-deploy/azure-apps/index>

> [!NOTE]
> ASP.NET Core içindekiler tablosuna yönelik önerilmiş olan yeni bir yapının kullanılabilirliğini test ediyoruz.  Geçerli veya önerilen içindekiler tablosunda 7 farklı konuyu bulmaya ilişkin alıştırmayı denemek için vaktiniz varsa lütfen [çalışmaya katılmak için buraya tıklayın](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).