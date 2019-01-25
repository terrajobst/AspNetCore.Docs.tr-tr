---
uid: signalr/overview/getting-started/supported-platforms
title: Desteklenen platformlar | Microsoft Docs
author: bradygaster
description: Bu makalede, hangi istemcilere ve sunuculara SignalR tarafından desteklenen açıklanır.
ms.author: bradyg
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 60fa74b54797efbe14ba525160b2f750a4f5a451
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836057"
---
<a name="supported-platforms"></a>Desteklenen Platformlar
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Bu makalede, hangi istemcilere ve sunuculara SignalR tarafından desteklenen açıklanır. 
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).

SignalR çeşitli altında sunucu ve istemci yapılandırmaları desteklenir. Ayrıca, her bir aktarım seçeneğin kendi gereksinimler kümesi vardır; Aktarım için sistem gereksinimleri, kullanılabilir durumda değilse, SignalR devreder düzgün bir şekilde diğer taşımalar için. SignalR destekleyen aktarımları hakkında daha fazla bilgi için bkz. [aktarım ve geri dönüşler](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Sunucu sistem gereksinimleri

SignalR sunucu bileşeni, çeşitli sunucu yapılandırmaları üzerinde barındırılabilir. Bu bölümde işletim sistemleri, .NET framework, Internet Information Server ve diğer bileşenleri desteklenen sürümlerini açıklar.

### <a name="supported-server-operating-systems"></a>Desteklenen sunucu işletim sistemleri

SignalR sunucu bileşeni, aşağıdaki sunucu veya istemci işletim sistemlerinde barındırılabilir. Windows Server 2012, Windows Server 2016 veya Windows 8 WebSockets kullanmak SignalR için gerekli olduğunu unutmayın (WebSocket kullanılabilir Windows Azure Web sitelerinde sitenin .NET framework sürüm 4.5 olarak ayarlanır ve Web yuvaları sitenin içinde etkin olduğu sürece Yapılandırma sayfası).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Microsoft Azure

### <a name="supported-server-net-framework-version"></a>Desteklenen sunucu .NET Framework sürümü

SignalR 2 yalnızca .NET Framework 4.5 üzerinde desteklenir. Bkz: [önerilen güncelleştirmeleri](#updates) güvenilirlik, uyumluluk, kararlılık ve performans artırmak güncelleştirmeleri bölümü.

### <a name="supported-server-iis-versions"></a>Desteklenen sunucu IIS sürümleri

SignalR IIS içinde barındırıldığında, aşağıdaki sürümleri destekler. Bir istemci işletim sistemi kullanılıyorsa, bu yana bağlantı sınırına çok hızlı bir şekilde uygulanan, 10 eşzamanlı bağlantı sınırı olacağından gibi geliştirme için IIS veya Cassini'ye tam sürümleri (Windows 8 veya Windows 7), kullanılmamalıdır olduğunu unutmayın. Geçici, sık sık yeniden oluşturulmuş ve bu hemen artık kullanılmayan üzerine elden değil. IIS Express, istemci işletim sistemlerinde kullanılmalıdır.

Ayrıca SignalR, WebSocket kullanmak için IIS 8 veya 8 IIS Express kullanılmalıdır, sunucunun Windows 8, Windows Server 2012 veya sonraki sürümü kullanıyor olmanız gerekir ve WebSocket IIS'de etkinleştirilmelidir unutmayın. IIS'de WebSocket etkinleştirme hakkında daha fazla bilgi için bkz: [IIS 8.0 WebSocket protokolü desteği](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 veya 8 IIS Express.
- IIS 7 ve 7.5. Destek [uzantısız URL'ler](https://support.microsoft.com/kb/980368) gereklidir.
- IIS tümleşik modunda çalışıyor olması gerekir; Klasik modu desteklenmiyor. IIS Server-Sent olayları taşıma kullanarak Klasik modda çalıştırırsanız, 30 saniyeye kadar ileti gecikmeleri karşılaşılabilir.
- Konak uygulama tam güven modunda çalıştırılması gerekir.

## <a name="client-system-requirements"></a>İstemci sistem gereksinimleri

SignalR istemci platformları çeşitli kullanılabilir. Bu bölümde, web tarayıcıları, Windows Masaüstü uygulamaları, Silverlight uygulamaları ve mobil cihazları SignalR kullanarak için sistem gereksinimleri açıklanmaktadır.

### <a name="web-browsers"></a>Web tarayıcıları

SignalR çeşitli web tarayıcılarıyla kullanılabilir, ancak genellikle, yalnızca en son iki sürümler desteklenir.

SignalR tarayıcılarda kullanan uygulamalar, jQuery sürüm 1.6.4 veya ana sonraki sürümleri (örneğin, 1.7.2, 1.8.2 veya 1.9.1) kullanmanız gerekir.

SignalR aşağıdaki tarayıcılarda kullanılabilir:

- Microsoft Internet Explorer sürüm 8, 9, 10 ve 11. Modern, masaüstü ve mobil sürümlerinde desteklenir.
- Mozilla Firefox: geçerli sürümü - 1, hem Windows hem de Mac sürümleri.
- Google Chrome: geçerli sürümü - 1, hem Windows hem de Mac sürümleri.
- Safari: geçerli sürümü - 1, Mac ve iOS sürümleri.
- Opera: geçerli sürümü - 1, yalnızca Windows.
- Android tarayıcı

Bazı tarayıcılar gerektiren ek olarak, SignalR kullanan çeşitli taşımalar, kendi gereksinimlerine sahiptir. Aşağıdaki taşımalar altında aşağıdaki yapılandırmalar desteklenir:

<a id="browser"></a>

**Web tarayıcısı aktarım gereksinimleri**

| Taşıma | Internet Explorer | Chrome (Windows ve iOS) | Firefox | Safari (OSX veya iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | Geçerli - 1 | Geçerli - 1 | Geçerli - 1 | Yok |
| Sunucu tarafından gönderilen olayları | Yok | Geçerli - 1 | Geçerli - 1 | Geçerli - 1 | Yok |
| ForeverFrame | 8+ | Yok | Yok | Yok | 4.1 |
| Uzun yoklama | 8+ | Geçerli - 1 | Geçerli - 1 | Geçerli - 1 | 4.1 |

\*: 6 tam işlevsellik için + gereklidir.

#### <a name="unsupported-browsers"></a>Desteklenmeyen tarayıcılar

While SignalR *olabilir* eski tarayıcı sürümlerinde önemli sorunları olmadan çalıştırın, biz etkin bir şekilde SignalR bunları test etme genellikle bunları oluşabilecek hataları ve değil.

### <a name="windows-desktop-and-silverlight-applications"></a>Windows Masaüstü ve Silverlight uygulamaları

Bir web tarayıcısında çalıştırmanın yanı sıra, tek başına Windows istemci veya Silverlight uygulamalarında SignalR barındırılabilir. Windows Masaüstü ve Silverlight SignalR uygulamaların aşağıdaki sistem gereksinimleri vardır.

- .NET 4 kullanan uygulamalar, Windows XP SP3'ü veya sonraki sürümlerde desteklenir.
- .NET Framework 4.5 kullanan uygulamalar, Windows Vista veya sonraki sürümlerde desteklenir.

İşletim sistemi ve .NET framework gereksinimleri ek olarak, SignalR için kullanılabilir taşımalar, kendi gereksinimleri vardır. Aşağıdaki taşımalar altında aşağıdaki yapılandırmalar desteklenir:

**Windows Masaüstü ve Silverlight aktarım gereksinimleri**

| Taşıma | .NET uygulaması | Silverlight |
| --- | --- | --- |
| Web yuvaları | Windows 8 + ve .NET 4.5 + | Yok |
| Her zaman çerçevesi | Yok | Yok |
| Sunucu tarafından gönderilen olayları | .NET 4+ | 5+ |
| Uzun yoklama | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows Store ve Windows Phone uygulamaları

SignalR, Windows Store uygulamaları ve Windows Phone 8 uygulamalarında kullanılabilir. Aşağıdaki taşımalar altında aşağıdaki yapılandırmalar desteklenir:

**Windows Store ve Windows Phone aktarım gereksinimleri**

| Taşıma | Windows Store / .NET | Windows Store / JavaScript | Windows Phone / IE | Windows Phone / .NET |
| --- | --- | --- | --- | --- |
| WebSockets | Yok | Win8 + | 8+ | Yok |
| Her zaman çerçevesi | Yok | Win8 + | 7.5+ | Yok |
| Sunucu tarafından gönderilen olayları | Win8 + | Yok | Yok | 8+ |
| Uzun yoklama | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Önerilen güncelleştirmeleri

Aşağıdaki güncelleştirmeleri SignalR sunucuları için önerilir:

- .NET Framework 4.5 için bir güncelleştirme kullanılabilir [burada](https://support.microsoft.com/kb/2750149).
- Microsoft ASP.NET için QFE'ler düzenli olarak serbest bırakır. Bunlar kullanılabilir olarak uygulanmalıdır.
