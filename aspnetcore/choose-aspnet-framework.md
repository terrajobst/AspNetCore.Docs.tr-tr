---
title: ASP.NET ve ASP.NET Core arasında seçim yapma
author: rick-anderson
description: ASP.NET ve ASP.NET Core arasında seçim yapma hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 05/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 0c6924d40b7327d2032a0278c56a0b4fa41d15a1
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a>ASP.NET ve ASP.NET Core arasında seçim yapma

ASP.NET web uygulaması olsun, oluşturduğunuz bir çözümü sizin için vardır: Windows Server hedefleme Kurumsal web uygulamaları ' küçük mikro Linux kapsayıcıları ve her şeyi arasında hedefleme.

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core Windows, macOS ya da Linux modern, bulut tabanlı web uygulamaları oluşturmak için açık kaynak, platformlar arası bir çerçevedir.

## <a name="aspnet"></a>ASP.NET

ASP.NET, kurumsal düzeyde oluşturmak için gereken tüm hizmetleri Windows server tabanlı web uygulamalarında sağlayan olgun bir çerçevedir.

## <a name="framework-selection"></a>Framework seçimi

Hangi framework gereksinimleriniz için en uygun olduğunu belirlemek için aşağıdaki tabloyu gözden geçirin.

| ASP.NET Core | ASP.NET |
|---|---|
|Windows, macOS ya da Linux derleme|Windows için derleme|
|[Razor sayfalarının](xref:mvc/razor-pages/index) ASP.NET Core itibariyle bir Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır 2.x. Ayrıca bkz. [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), ve [SignalR](xref:signalr/introduction).|Kullanım [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [Kancalarını](/aspnet/webhooks/), veya [Web sayfaları](/aspnet/web-pages)|
|Makine başına birden çok sürüm|Makine başına bir sürüm|
|Visual Studio ile geliştirme [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/), veya [Visual Studio Code](https://code.visualstudio.com/) C# veya F # kullanma|C#, VB ve F # kullanarak Visual Studio ile geliştirme|
|ASP.NET daha yüksek performans|İyi bir performans|
|[.NET Framework veya .NET çekirdeği çalışma zamanı seçin](/dotnet/articles/standard/choosing-core-framework-server)|.NET Framework çalışma zamanı kullanın|

## <a name="aspnet-core-scenarios"></a>ASP.NET Core senaryoları

* [Razor sayfalarının](xref:mvc/razor-pages/index) ASP.NET Core itibariyle bir Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır 2.x.
* [Web siteleri](xref:tutorials/first-mvc-app/index)
* [API'leri](xref:tutorials/first-web-api)
* [Gerçek zamanlı](xref:signalr/index)

## <a name="aspnet-scenarios"></a>ASP.NET senaryoları

* [Web siteleri](/aspnet/mvc)
* [API'leri](/aspnet/web-api)
* [Gerçek zamanlı](/aspnet/signalr)

## <a name="resources"></a>Kaynaklar

* [ASP.NET giriş](/aspnet/overview)
* [ASP.NET Core giriş](xref:index)
