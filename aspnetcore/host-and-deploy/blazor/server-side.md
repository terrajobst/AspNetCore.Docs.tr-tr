---
title: ASP.NET Core Blazor sunucu-tarafı barındırma ve dağıtma
author: guardrex
description: ASP.NET Core kullanarak Blazor sunucu tarafı uygulamasını nasıl barındırılacağını ve dağıtacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8da71faf6abc5929d6cd43d42fd896e378d99ef6
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773580"
---
# <a name="host-and-deploy-blazor-server-side"></a>Blazor sunucu tarafı barındırma ve dağıtma

, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

[Sunucu tarafı barındırma modelini](xref:blazor/hosting-models#server-side) kullanan sunucu tarafı uygulamalar [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration)kabul edebilir.

## <a name="deployment"></a>Dağıtım

[Sunucu tarafı barındırma modeliyle](xref:blazor/hosting-models#server-side), Blazor bir ASP.NET Core uygulamasının içinden sunucuda yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.

ASP.NET Core uygulaması barındırabilen bir Web sunucusu gerekiyor. Visual Studio, **Blazor Server uygulaması** proje şablonunu (`blazorserverside` [DotNet New](/dotnet/core/tools/dotnet-new) komutu kullanılırken şablon) içerir.

## <a name="connection-scale-out"></a>Bağlantı ölçeğini genişletme

Blazor sunucu tarafı uygulamalar, her kullanıcı için bir etkin SignalR bağlantısı gerektirir. Üretim Blazor sunucu tarafı dağıtımı, uygulamanın gerektirdiği sayıda eşzamanlı bağlantıyı desteklemek için bir çözüm gerektirir. [Azure SignalR hizmeti](/azure/azure-signalr/) bağlantıların ölçeklendirilmesini Işler ve Blazor sunucu tarafı uygulamaları için bir ölçeklendirme çözümü olarak önerilir. Daha fazla bilgi için bkz. <xref:signalr/publish-to-azure-web-app>.

## <a name="signalr-configuration"></a>SignalR yapılandırması

SignalR, en yaygın Blazor sunucu tarafı senaryolar için ASP.NET Core tarafından yapılandırılır. Özel ve gelişmiş senaryolar için [ek kaynaklar](#additional-resources) bölümündeki SignalR makalelerine bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/introduction>
* [Azure SignalR hizmeti belgeleri](/azure/azure-signalr/)
* [Hızlı Başlangıç: SignalR hizmetini kullanarak sohbet odası oluşturma](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Azure App Service için ASP.NET Core Preview sürümünü dağıtın](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
