---
title: ASP.NET 4. x ve ASP.NET Core arasında seçim yapın
author: rick-anderson
description: ASP.NET Core vs. ASP.NET 4. x ve aralarında nasıl seçim yapılacağını açıklar.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/12/2019
no-loc:
- SignalR
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 8b1681476f96e8613f9461c507fbb7696f888cbc
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963619"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a>ASP.NET 4. x ve ASP.NET Core arasında seçim yapın

ASP.NET Core, ASP.NET 4. x ' in bir tasarlayadır. Bu makalede aralarındaki farklar listelenir.

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core, Windows, macOS veya Linux 'ta modern, bulut tabanlı Web uygulamaları oluşturmaya yönelik açık kaynaklı, platformlar arası bir çerçevedir.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a>ASP.NET 4. x

ASP.NET 4. x, Windows 'ta kurumsal düzeyde, sunucu tabanlı Web uygulamaları oluşturmak için gereken hizmetleri sağlayan, yetişkinlere yönelik bir çerçevedir.

## <a name="framework-selection"></a>Çerçeve seçimi

Aşağıdaki tabloda ASP.NET Core ASP.NET 4. x ile karşılaştırılır.

| ASP.NET Core | ASP.NET 4. x |
|---|---|
|Windows, macOS veya Linux için derleme|Windows için derleme|
|[Razor Pages](xref:razor-pages/index) , bir Web kullanıcı arabirimini ASP.NET Core 2. x olarak oluşturmak için önerilen yaklaşımdır. Ayrıca bkz. [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api)ve [SignalR](xref:signalr/introduction).|[Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), Web [kancaları](/aspnet/webhooks/)veya [Web sayfaları](/aspnet/web-pages) kullanın|
|Makine başına birden çok sürüm|Makine başına bir sürüm|
|C# Veya kullanarak [Visual Studio](https://visualstudio.microsoft.com/vs/), [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)veya [Visual Studio Code](https://code.visualstudio.com/) geliştirinF#|, Vb veya kullanarak C# [Visual Studio](https://visualstudio.microsoft.com/vs/) ile geliştirmeF#|
|ASP.NET 4. x 'ten daha yüksek performans|İyi performans|
|[.NET Core çalışma zamanı kullan](/dotnet/standard/choosing-core-framework-server)|.NET Framework çalışma zamanını kullan|

.NET Framework üzerinde ASP.NET Core 2. x desteği hakkında bilgi için bkz. [ASP.NET Core .NET Framework Hedefleme](xref:index#target-framework) .

## <a name="aspnet-core-scenarios"></a>ASP.NET Core senaryolar

* [Siteleriniz](xref:tutorials/first-mvc-app/index)
* [GetVersionEx](xref:tutorials/first-web-api)
* [Gerçek zamanlı](xref:signalr/index)
* [Azure 'a ASP.NET Core uygulaması dağıtma](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>ASP.NET 4. x senaryoları

* [Siteleriniz](/aspnet/mvc)
* [GetVersionEx](/aspnet/web-api)
* [Gerçek zamanlı](/aspnet/signalr)
* [Azure 'da bir ASP.NET 4. x Web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET 'e giriş](/aspnet/overview)
* [ASP.NET Core giriş](xref:index)
* <xref:host-and-deploy/azure-apps/index>
