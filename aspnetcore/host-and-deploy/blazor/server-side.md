---
title: Ana bilgisayar ve sunucu tarafı Blazor dağıtma
author: guardrex
description: Ana bilgisayar ve ASP.NET Core kullanarak Blazor sunucu-tarafı uygulamasını dağıtma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 39e4f0634d40e8c720fa5a6a76bc0843487a4e3c
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982594"
---
# <a name="host-and-deploy-blazor-server-side"></a>Ana bilgisayar ve sunucu tarafı Blazor dağıtma

Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

Kullanan bir sunucu tarafı uygulamalar [sunucu tarafı barındırma modeli](xref:blazor/hosting-models#server-side) kabul edebilir [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Dağıtım

İle [sunucu tarafı barındırma modeli](xref:blazor/hosting-models#server-side), Blazor sunucusunda bir ASP.NET Core uygulaması içinde yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.

Yayımlanan çıktıda ASP.NET Core uygulaması ile birlikte bir uygulamadır ve iki uygulamanın birlikte dağıtılır. ASP.NET Core uygulaması barındırma yeteneğine sahip bir web sunucusu gereklidir. Sunucu tarafı dağıtımı için Visual Studio içerir **Blazor (sunucu tarafı)** proje şablonu (`blazorserverside` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

ASP.NET Core uygulaması barındırma ve dağıtma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/index>.

Azure App Service'e dağıtma hakkında daha fazla bilgi için bkz. <xref:tutorials/publish-to-azure-webapp-using-vs>.
