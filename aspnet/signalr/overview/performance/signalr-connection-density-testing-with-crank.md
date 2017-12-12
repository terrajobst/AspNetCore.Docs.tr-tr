---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: "SignalR bağlantısı yoğunluğu mili ile test etme | Microsoft Docs"
author: tfitzmac
description: "SignalR bağlantısı yoğunluğu mili ile test etme"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: a3e8fdb47bd80d819358f9c48cd82fd51d6c0400
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2017
---
<a name="signalr-connection-density-testing-with-crank"></a>SignalR bağlantısı yoğunluğu mili ile test etme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, birden çok sanal istemcilerle bir uygulamayı test etmek için mili Aracı'nı kullanmayı açıklar.


Uygulamanızı (ya da Azure rolü, IIS, web veya Owın kullanarak kendi kendini barındıran), barındırma ortamında çalışmaya başladıktan sonra uygulamanın yanıt bağlantı yoğunluğu mili aracını kullanarak yüksek düzeyde test edebilirsiniz. Barındırma ortamı, Internet Information Services (IIS) sunucu, bir Owın ana bilgisayar veya bir Azure web rolü olabilir. (Not: performans sayaçları kullanılamaz Azure App Service Web Apps üzerinde öğesinden bir bağlantı yoğunluğu testi performans verilerini almak mümkün olmayacak şekilde.)

Bağlantı yoğunluğu bir sunucuda kurulmuş eşzamanlı TCP bağlantısı sayısını ifade eder. Her TCP bağlantısı kendi ek yükü doğurur ve çok sayıda boşta bağlantıları açılırken bir bellek performans sorunu sonunda oluşturacağız.

[SignalR codebase](https://github.com/signalr/signalr) adı verilen bir yük testi aracı içeren **Crank**. En son sürümünü mili bulunabilir [geliştirme şube](https://github.com/SignalR/signalr/tree/dev) github'da. SignalR geliştirme dalı arşivini codebase Zip indirebilirsiniz [burada](https://github.com/SignalR/SignalR/archive/dev.zip).

Mili tam olarak sunucunun bellek saturate için boşta bağlantı sunucu donanımına olası toplam sayısını hesaplamak için kullanılabilir. Alternatif olarak, aynı zamanda mili yük testi için sunucunun belirli bir miktarda bellek baskısı altında belirli bir sayısı veya belirli bellek eşiğini ulaşılana kadar bağlantıları ramping kullanabilirsiniz.

Test ederken kaynaklar (yani, TCP bağlantıları ve bellek) için tüm rekabet önlemek için Uzak istemci kullanmak önemlidir. Bunlar sunucunun tam kapasitesi (bellek veya CPU) ulaşmasına engel olabilen herhangi bir performans sorunu devreyi değil emin olmak için istemci izleyin. Tam sunucu iş yükü için istemci sayısını artırmak gerekebilir.

### <a name="running-a-connection-density-test"></a>Bir bağlantı yoğunluğu testi çalıştırma

Bu bölümde bir SignalR uygulamaya bir bağlantı yoğunluğu testi çalıştırmak için gereken adımlar açıklanmaktadır.

1. İndirin ve derleyin [SignalR geliştirme dalı codebase](https://github.com/SignalR/SignalR/archive/dev.zip). Bir komut isteminde gidin &lt;proje dizinine&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Uygulamanızı hedeflenen kendi barındırma ortamınıza dağıtın. Uygulamanızı kullanan uç nokta not edin; Örneğin, oluşturulan uygulamada [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md), uç nokta `http://<yourhost>:8080/signalr`.
3. Yükleme [SignalR performans sayaçlarını](signalr-performance.md#perfcounters) sunucusunda. Uygulamanızı Azure üzerinde çalışıyorsa, bkz: [SignalR performans sayaçlarını kullanarak bir Azure Web rolünde](using-signalr-performance-counters-in-an-azure-web-role.md).

İndirilen codebase yerleşik ve performans sayaçları, ana bilgisayar üzerine yüklü sonra mili komut satırı aracı bulunabilir `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` klasör.

Mili aracı için mevcut Seçenekler şunlardır:

- **/?** : Yardım ekranı gösterir. Kullanılabilir seçenekler ayrıca görüntülenir **Url** parametresi atlanırsa.
- **/ Url**: SignalR bağlantıları için URL. Bu parametre gereklidir. Bir SignalR uygulaması için varsayılan eşlemeyi kullanarak yolun içinde sona erecek "/ signalr".
- **/ Aktarım**: kullanılan taşıma adı. Varsayılan değer `auto`, en iyi kullanılabilir Protokolü seçebilir. Seçenekleriniz `WebSockets`, `ServerSentEvents`, ve `LongPolling` (`ForeverFrame` yerine Internet Explorer kullanıldığı bir seçenek mili için .NET istemci bu yana değil). SignalR taşımaları nasıl seçtiği hakkında daha fazla bilgi için bkz: [aktarımları ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: her toplu işlemde eklenen istemci sayısı. Varsayılan değer 50'dir.
- **/ ConnectInterval**: bağlantıları ekleme arasındaki milisaniye cinsinden aralığı. Varsayılan 500'dür.
- **/ Bağlantıları**: yük testi uygulama için kullanılan bağlantı sayısı. Varsayılan değer 100. 000 ' dir.
- **/ ConnectTimeout**: sınama durduruluyor önce saniye olarak zaman aşımı. Varsayılan değer 300 ' dir.
- **MinServerMBytes**: ulaşmak için en düşük sunucu megabayt. Varsayılan 500'dür.
- **SendBytes**: bayt sunucusuna gönderilen yükü boyutu. Varsayılan değer 0'dır.
- **SendInterval**: sunucuya iletileri arasındaki milisaniye cinsinden gecikme. Varsayılan 500'dür.
- **SendTimeout**: iletileri sunucuyu için milisaniye cinsinden zaman aşımı. Varsayılan değer 300 ' dir.
- **ControllerUrl**: bir istemci ana bilgisayar denetleyicisi hub Url. Varsayılan değer null (hiçbir denetleyici hub). Denetleyici hub mili oturum başlatıldığında başlatılır; denetleyici hub mili arasındaki iletişim başka yapılır.
- **NumClients**: uygulamaya bağlanmak için sanal istemci sayısı. Varsayılan biridir.
- **Günlük dosyası**: test çalışması için günlük dosyası için dosya adı. Varsayılan, `crank.csv` değeridir.
- **SampleInterval**: performans sayacı Örnekler arasındaki milisaniye olarak süre. Varsayılan değer 1000'dir.
- **SignalRInstance**: sunucu üzerindeki performans sayaçları için örnek adı. Varsayılan istemci bağlantı durumu kullanmaktır.

### <a name="example"></a>Örnek

Aşağıdaki komutu adlı bir sitede sınayacak `pfsignalr` "ControllerHub" adlı bir hub ile bağlantı noktası 8080 üzerinde bir uygulamayı barındıran Azure üzerinde 100 bağlantıları kullanma.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
