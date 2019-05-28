---
title: Seçin arasında ASP.NET 4.x ve ASP.NET Core
author: rick-anderson
description: ASP.NET Core vs açıklar. ASP.NET 4.x ve bunlar arasında seçim yapma.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 05/02/2019
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: a51d9946c9e65bd1665c610153f724c6087c9f7f
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251375"
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
|Visual Studio ile geliştirin [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/), veya [Visual Studio Code](https://code.visualstudio.com/) kullanarak C# veyaF#|Visual Studio kullanarak ile geliştirme C#, VB, veyaF#|
|Daha yüksek performansa ASP.NET 4.x|İyi bir performans|
|[.NET Framework veya .NET Core çalışma zamanı seçin](/dotnet/standard/choosing-core-framework-server)|.NET Framework çalışma zamanını kullanma|

Bkz: [.NET Framework'ü hedefleyen ASP.NET Core](xref:index#target-framework) .NET Framework üzerinde ASP.NET Core 2.x desteği hakkında daha fazla bilgi için.

## <a name="aspnet-core-scenarios"></a>ASP.NET Core senaryoları

* [Web siteleri](xref:tutorials/first-mvc-app/index)
* [API'ler](xref:tutorials/first-web-api)
* [Gerçek zamanlı](xref:signalr/index)
* [Azure'da ASP.NET Core uygulaması dağıtma](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>ASP.NET 4.x senaryoları

* [Web siteleri](/aspnet/mvc)
* [API'ler](/aspnet/web-api)
* [Gerçek zamanlı](/aspnet/signalr)
* [Azure'da ASP.NET 4.x web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET'e giriş](/aspnet/overview)
* [ASP.NET Core'a giriş](xref:index)
* <xref:host-and-deploy/azure-apps/index>
