---
title: Barındırma ve Razor bileşenleri dağıtma
author: guardrex
description: Ana bilgisayar ve ASP.NET Core kullanarak Razor bileşenleri uygulamasını dağıtma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/razor-components
ms.openlocfilehash: 18b09dfc80e2f517c7750ce0fe1510641624aea7
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59069776"
---
# <a name="host-and-deploy-razor-components"></a>Barındırma ve Razor bileşenleri dağıtma

Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

Razor bileşenleri kullanan uygulamalar [sunucu tarafı barındırma modeli](xref:razor-components/hosting-models#server-side-hosting-model) kabul edebilir [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Dağıtım

İle [sunucu tarafı barındırma modeli](xref:razor-components/hosting-models#server-side-hosting-model), Razor bileşenleri sunucusunda bir ASP.NET Core uygulaması içinde yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.

Yayımlanan çıktıda ASP.NET Core uygulaması ile birlikte bir uygulamadır ve iki uygulamanın birlikte dağıtılır. ASP.NET Core uygulaması barındırma yeteneğine sahip bir web sunucusu gereklidir. Sunucu tarafı dağıtımı için Visual Studio içerir **Razor bileşenleri** proje şablonu (`razorcomponents` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

ASP.NET Core uygulaması barındırma ve dağıtma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/index>.

Azure App Service'e dağıtma hakkında daha fazla bilgi için bkz. <xref:tutorials/publish-to-azure-webapp-using-vs>.
