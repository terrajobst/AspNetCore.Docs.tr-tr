---
title: Seçin arasında ASP.NET 4.x ve ASP.NET Core
author: rick-anderson
description: ASP.NET Core vs. ASP.NET 4. x ve aralarında nasıl seçim yapılacağını açıklar.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/12/2020
no-loc:
- SignalR
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: a7280b59578ee1d96edeeccf9c9df0b0e4eb4eb8
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447301"
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
|[Razor Pages](xref:razor-pages/index) , bir Web kullanıcı arabirimini ASP.NET Core 2. x olarak oluşturmak için önerilen yaklaşımdır. Ayrıca bkz. [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api)ve [SignalR](xref:signalr/introduction).|[Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), Web [kancaları](/aspnet/webhooks/)veya [Web sayfaları](/aspnet/web-pages) kullanın|
|Makine başına birden çok sürümü|Makine başına bir sürümü|
|C# Veya kullanarak [Visual Studio](https://visualstudio.microsoft.com/vs/), [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)veya [Visual Studio Code](https://code.visualstudio.com/) geliştirinF#|, Vb veya kullanarak C# [Visual Studio](https://visualstudio.microsoft.com/vs/) ile geliştirmeF#|
|Daha yüksek performansa ASP.NET 4.x|İyi bir performans|
|[.NET Core çalışma zamanı kullan](/dotnet/standard/choosing-core-framework-server)|.NET Framework çalışma zamanını kullanma|

.NET Framework üzerinde ASP.NET Core 2. x desteği hakkında bilgi için bkz. [ASP.NET Core .NET Framework Hedefleme](xref:index#target-framework) .

## <a name="aspnet-core-scenarios"></a>ASP.NET Core senaryoları

* [Siteleriniz](xref:tutorials/first-mvc-app/index)
* [API'ler](xref:tutorials/first-web-api)
* [Gerçek zamanlı](xref:signalr/introduction)
* [Azure 'a ASP.NET Core uygulaması dağıtma](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>ASP.NET 4.x senaryoları

* [Siteleriniz](/aspnet/mvc)
* [API'ler](/aspnet/web-api)
* [Gerçek zamanlı](/aspnet/signalr)
* [Azure 'da bir ASP.NET 4. x Web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET 'e giriş](/aspnet/overview)
* [ASP.NET Core giriş](xref:index)
* <xref:host-and-deploy/azure-apps/index>
