---
title: Barındırma ve ASP.NET Core Blazor sunucu tarafı dağıtma
author: guardrex
description: Ana bilgisayar ve ASP.NET Core kullanarak Blazor sunucu-tarafı uygulamasını dağıtma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8b332c2fb439e9832d604ed26f972b266eed2507
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406117"
---
# <a name="host-and-deploy-blazor-server-side"></a>Ana bilgisayar ve sunucu tarafı Blazor dağıtma

Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

Kullanan bir sunucu tarafı uygulamalar [sunucu tarafı barındırma modeli](xref:blazor/hosting-models#server-side) kabul edebilir [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Dağıtım

İle [sunucu tarafı barındırma modeli](xref:blazor/hosting-models#server-side), Blazor sunucusunda bir ASP.NET Core uygulaması içinde yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.

ASP.NET Core uygulaması barındırma uyumlu bir web sunucusu gereklidir. Visual Studio içerir **Blazor (sunucu tarafı)** proje şablonu (`blazorserverside` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).

## <a name="connection-scale-out"></a>Bağlantı ölçeği genişletme

Blazor sunucu tarafı uygulamalar, her kullanıcı için etkin bir SignalR bağlantısı gerektirir. Bir üretim Blazor sunucu tarafı dağıtım, uygulamanın gerektirdiği sayıda eş zamanlı bağlantı desteklemek için bir çözüm gerektirir. [Azure SignalR hizmeti](/azure/azure-signalr/) bağlantıları ölçeklendirme işler ve Blazor sunucu tarafı uygulamalar için ölçeklendirme çözümü olarak önerilir. Daha fazla bilgi için bkz. <xref:signalr/publish-to-azure-web-app>.

## <a name="signalr-configuration"></a>SignalR yapılandırma

SignalR en yaygın Blazor sunucu tarafı senaryolar için ASP.NET Core tarafından yapılandırılır. Özel ve Gelişmiş senaryolar için SignalR makalelerinde başvurun [ek kaynaklar](#additional-resources) bölümü.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/introduction>
* [Azure SignalR hizmeti belgeleri](/azure/azure-signalr/)
* [Hızlı Başlangıç: SignalR hizmetini kullanarak sohbet odası oluşturamadı.](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [ASP.NET Core Önizleme sürümü, Azure App Service'e dağıtma](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
