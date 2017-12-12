---
uid: signalr/overview/getting-started/supported-platforms
title: Desteklenen platformlar | Microsoft Docs
author: pfletcher
description: "Bu makalede, hangi istemcilerin ve sunucuların SignalR tarafından desteklenen açıklanmaktadır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 7f41017a2a8c058c01fe6f89a2503eb5fa77048e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="supported-platforms"></a>Desteklenen Platformlar
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

> Bu makalede, hangi istemcilerin ve sunucuların SignalR tarafından desteklenen açıklanmaktadır. 
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


SignalR çeşitli sunucu ve istemci yapılandırmaları altında desteklenir. Ayrıca, bir dizi gereksinim kendi her aktarım seçeneği vardır; bir taşıma için sistem gereksinimleri kullanılabilir değilse, SignalR olacak düzgün biçimde diğer taşımaları için yük devretme. SignalR destekleyen aktarımları hakkında daha fazla bilgi için bkz: [aktarımları ve geri dönüşler](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Sunucu sistem gereksinimleri

SignalR sunucu bileşeni sunucu yapılandırmaları çeşitli barındırılabilir. Bu bölümde işletim sistemleri, .NET framework, Internet Information Server ve diğer bileşenleri desteklenen sürümlerini açıklar.

### <a name="supported-server-operating-systems"></a>Desteklenen sunucu işletim sistemleri

SignalR sunucu bileşeni, aşağıdaki sunucu veya istemci işletim sistemlerinde barındırılabilir. Windows Server 2012 veya Windows 8 WebSockets kullanmak SignalR için gerekli olduğunu unutmayın (WebSocket kullanılabilir Windows Azure Web sitelerinde sitenin .NET framework sürüm 4.5 ayarlanır ve Web yuvalarını sitenin yapılandırma sayfasında etkin olduğu sürece).

- Windows Server 2012
- Windows Server 2008 r2
- Windows 8
- Windows 7
- Microsoft Azure

### <a name="supported-server-net-framework-version"></a>Desteklenen sunucu .NET Framework sürümü

SignalR 2, yalnızca .NET Framework 4. 5'üzerinde desteklenir. Bkz: [önerilen güncelleştirmeleri](#updates) güvenilirlik, uyumluluk, kararlılığı ve performansı geliştirmek güncelleştirmeleri bölümü.

### <a name="supported-server-iis-versions"></a>Desteklenen sunucu IIS sürümleri

SignalR IIS içinde barındırıldığında, aşağıdaki sürümleri desteklenir. Bir istemci işletim sistemi kullanılıyorsa, bu yana bağlantı sınırına çok hızlı bir şekilde uygulanan, 10 eşzamanlı bağlantıları sınırının olacağından gibi geliştirme için (Windows 8 veya Windows 7), tam IIS veya sürümlerini Cassini, kullanılmamalıdır olduğunu unutmayın sık sık yeniden kurdu, geçici ve bu hemen artık kullanılmayan bağlı elden değil. IIS Express, istemci işletim sistemlerinde kullanılmalıdır.

Ayrıca SignalR WebSocket, kullanmak için IIS 8 veya 8 IIS Express kullanılması gerekir, sunucunun Windows 8, Windows Server 2012 veya daha yeni sürümü kullanmanız gerekir ve WebSocket IIS'de etkinleştirilmelidir unutmayın. IIS'de WebSocket etkinleştirme hakkında daha fazla bilgi için bkz: [IIS 8.0 WebSocket protokolü desteği](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 veya 8 IIS Express.
- IIS 7 ve 7.5. Desteği [uzantısız URL'leri](https://support.microsoft.com/kb/980368) gereklidir.
- IIS tümleşik modunda çalışıyor olması gerekir; Klasik modu desteklenmiyor. IIS Server-Sent olayları aktarım kullanarak Klasik modda çalıştırırsanız 30 saniye kadar ileti gecikmelerini karşılaşmış.
- Barındırma uygulama tam güven modunda çalışıyor olmalıdır.

## <a name="client-system-requirements"></a>İstemci sistem gereksinimleri

SignalR istemci platformları çeşitli içinde kullanılabilir. Bu bölümde web tarayıcıları, Windows Masaüstü uygulamaları, Silverlight uygulamaları ve mobil cihazları SignalR kullanmak için sistem gereksinimleri açıklanmaktadır.

### <a name="web-browsers"></a>Web tarayıcıları

SignalR çeşitli web tarayıcılarıyla kullanılabilir, ancak genellikle, yalnızca son iki sürümleri desteklenir.

SignalR tarayıcılarda kullanan uygulamalar, jQuery sürüm 1.6.4 veya ana sonraki sürümleri (örneğin, 1.7.2, 1.8.2 veya 1.9.1) kullanmanız gerekir.

SignalR aşağıdaki tarayıcılarda kullanılabilir:

- Microsoft Internet Explorer sürümleri 8, 9, 10 ve 11. Modern, masaüstü ve mobil sürümleri desteklenir.
- Mozilla Firefox: geçerli sürümü - 1, hem Windows hem de Mac sürümleri.
- Google Chrome: geçerli sürümü - 1, hem Windows hem de Mac sürümleri.
- Safari: geçerli sürümü - 1, Mac ve iOS sürümleri.
- Opera: geçerli sürümü - 1, yalnızca Windows.
- Android tarayıcı

Bazı tarayıcılar istemeden yanı sıra, SignalR kullanan çeşitli taşımalar kendi gereksinimlerine sahiptir. Aşağıdaki taşımalar altında aşağıdaki yapılandırmalar desteklenir:

<a id="browser"></a>

**Web tarayıcısı aktarım gereksinimleri**

| Taşıma | Internet Explorer | Chrome (Windows veya iOS) | Firefox | Safari (OSX veya iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | Geçerli - 1 | Geçerli - 1 | Geçerli - 1 | Yok |
| Sunucu tarafından gönderilen olaylar | Yok | Geçerli - 1 | Geçerli - 1 | Geçerli - 1 | Yok |
| ForeverFrame | 8+ | Yok | Yok | Yok | 4.1 |
| Uzun yoklama | 8+ | Geçerli - 1 | Geçerli - 1 | Geçerli - 1 | 4.1 |

\*: 6 + tam işlevsellik için gerekli.

#### <a name="unsupported-browsers"></a>Desteklenmeyen tarayıcılar

While SignalR *olabilir* eski tarayıcı sürümlerindeki önemli sorunları olmadan çalıştırmak, biz etkin olarak SignalR bunları test ve bunları görünebilir hatalar genellikle düzeltme değil.

### <a name="windows-desktop-and-silverlight-applications"></a>Windows Masaüstü ve Silverlight uygulamaları

Bir web tarayıcısında çalıştırmanın yanı sıra, tek başına Windows İstemcisi veya Silverlight uygulamaları SignalR barındırılabilir. Windows Masaüstü ve Silverlight SignalR uygulamalarını aşağıdaki sistem gereksinimleri vardır.

- .NET 4 kullanan uygulamalar, Windows XP SP3 veya sonraki sürümlerde desteklenir.
- .NET Framework 4.5 kullanan uygulamalar, Windows Vista veya sonraki sürümlerde desteklenir.

İşletim sistemi ve .NET framework gereksinimleri ek olarak, SignalR için kullanılabilir taşımalar kendi gereksinimleri vardır. Aşağıdaki taşımalar altında aşağıdaki yapılandırmalar desteklenir:

**Windows Masaüstü ve Silverlight aktarım gereksinimleri**

| Taşıma | .NET uygulaması | Silverlight |
| --- | --- | --- |
| Web yuvaları | Windows 8 + ve .NET 4.5 + | Yok |
| Devamlı çerçeve | Yok | Yok |
| Sunucu tarafından gönderilen olaylar | .NET 4 + | 5+ |
| Uzun yoklama | .NET 4 + | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows mağazası ve Windows Phone uygulamaları

SignalR, Windows mağazası uygulamaları ve Windows Phone 8 uygulamalarında kullanılabilir. Aşağıdaki taşımalar altında aşağıdaki yapılandırmalar desteklenir:

**Windows mağazası ve Windows Phone aktarım gereksinimleri**

| Taşıma | Windows mağazası / .NET | Windows mağazası / JavaScript | Windows Phone / IE | Windows Phone / .NET |
| --- | --- | --- | --- | --- |
| WebSockets | Yok | Olduğu Win8 + | 8+ | Yok |
| Devamlı çerçeve | Yok | Olduğu Win8 + | 7.5+ | Yok |
| Sunucu tarafından gönderilen olaylar | Olduğu Win8 + | Yok | Yok | 8+ |
| Uzun yoklama | Olduğu Win8 + | Olduğu Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Önerilen güncelleştirmeleri

Aşağıdaki güncelleştirmeler SignalR sunucuları için önerilir:

- .NET Framework 4.5 için bir güncelleştirme kullanıma hazır [burada](https://support.microsoft.com/kb/2750149).
- Microsoft ASP.NET için QFE düzenli olarak serbest bırakır. Bunlar kullanılabilir olarak uygulanmalıdır.
