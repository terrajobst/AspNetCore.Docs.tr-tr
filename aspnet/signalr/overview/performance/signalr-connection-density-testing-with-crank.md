---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Crank ile SignalR bağlantı yoğunluğu | Microsoft Docs
author: Rick-Anderson
description: Crank ile SignalR Bağlantı Yoğunluğu Testi
ms.author: riande
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 556accb1bcc18e9e4d1f813a87fc6f4b67bda088
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021488"
---
<a name="signalr-connection-density-testing-with-crank"></a>Crank ile SignalR Bağlantı Yoğunluğu Testi
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir uygulama birden çok sanal istemcileri ile test etmek için mili Aracı'nı kullanmayı açıklar.


Uygulamanızı (ya da bir Azure web rolü, IIS veya Owın kullanarak şirket içinde barındırılan), barındırma ortamı içinde çalışır duruma geçtikten sonra yüksek düzeyde bağlantı yoğunluğu testi mili aracını kullanarak bir uygulamanın yanıt test edebilirsiniz. Barındırma ortamı, Internet Information Services (IIS) sunucu, bir Owın konak veya bir Azure web rolü olabilir. (Not: performans sayaçları kullanılabilir değil Azure App Service Web Apps üzerinde performans verilerini öğesinden bir bağlantı yoğunluğu testi almanız mümkün olmayacak şekilde.)

Bağlantı yoğunluğu testi bir sunucuda kurulabilecek eş zamanlı TCP bağlantılarını sayısını ifade eder. Her TCP bağlantısı kendi ek yüke neden olur ve çok sayıda boşta kalan bağlantıların açma sonunda bir bellek sorunu oluşturacaksınız.

[SignalR codebase](https://github.com/signalr/signalr) adında bir yük testi araç içerir **Crank**. En son sürümünü mili bulunabilir [geliştirme dalını](https://github.com/SignalR/signalr/tree/dev) GitHub üzerinde. Bir Zip arşivi signalr geliştirme dalı codebase indirebileceğiniz [burada](https://github.com/SignalR/SignalR/archive/dev.zip).

Mili tam olarak sunucunun bellek saturate için sunucu donanımına olası boşta kalan bağlantıların toplam sayısını hesaplamak için kullanılabilir. Alternatif olarak, aynı zamanda mili yük testi için sunucunun belirli bir miktarda bellek baskısı altında belirli bir sayısı veya belirli bellek eşiğini ulaşılana kadar bağlantıları ramping kullanabilirsiniz.

Test ederken, uzak istemci herhangi bir yarışmaya kaynakları (yani TCP bağlantıları ve bellek gibi) önlemek önemlidir. Bunlar sunucunun tam kapasitesi (bellek veya CPU) erişmesini engelleyebilir. tüm performans sorunlarını vpn'den emin olmak için istemci izleme. Tam sunucu iş yükü için istemcilerin sayısını artırmanız gerekebilir.

### <a name="running-a-connection-density-test"></a>Bir bağlantı yoğunluğu testi çalıştırma

Bu bölümde bir SignalR uygulama bağlantı yoğunluğu testi çalıştırmak için gereken adımlar açıklanmaktadır.

1. İndirin ve derleyin [geliştirme dalını signalr codebase](https://github.com/SignalR/SignalR/archive/dev.zip). Bir komut istemi'nde gidin &lt;proje dizini&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Uygulamanızı, hedeflenen barındırma ortamına dağıtın. Uygulamanızın kullandığı uç noktasını not edin; Örneğin, oluşturulan uygulamadaki [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md), uç nokta `http://<yourhost>:8080/signalr`.
3. Yükleme [SignalR performans sayaçları](signalr-performance.md#perfcounters) sunucusunda. Uygulamanız Azure üzerinde çalışıyorsa, bkz. [bir Azure Web rolünde SignalR performans sayaçları kullanarak](using-signalr-performance-counters-in-an-azure-web-role.md).

İçinde indirilen bir kod temelinde oluşturulan ve performans sayaçları, konakta yüklü sonra mili komut satırı aracı bulunabilir `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` klasör.

Mili aracı için kullanılabilir seçenekler şunlardır:

- **/?** : Yardım ekranı gösterilir. Kullanılabilir seçenekler de görüntülenir **Url** parametresi atlanırsa.
- **/ Url**: SignalR bağlantıları için URL. Bu parametre gereklidir. Bir SignalR uygulama için varsayılan eşlemeyi kullanarak yolun içinde sona erecek "/ signalr".
- **/ Aktarım**: kullanılan taşıma adı. Varsayılan `auto`, en iyi kullanılabilir protokol seçer. Seçenekleriniz `WebSockets`, `ServerSentEvents`, ve `LongPolling` (`ForeverFrame` yerine Internet Explorer kullanıldığı bir seçenek mili için bu yana .NET istemci değildir). SignalR taşımalar nasıl seçer? daha fazla bilgi için bkz: [aktarım ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: her toplu eklenen istemci sayısı. Varsayılan değer 50'dir.
- **/ ConnectInterval**: bağlantıları eklenmesi arasındaki milisaniye cinsinden aralığı. Varsayılan değer 500'dür.
- **Bağlantı**: yük testi uygulama için kullanılan bağlantı sayısı. Varsayılan değer 100. 000 ' dir.
- **/ ConnectTimeout**: test çalışmasını iptal etmeden önce saniye cinsinden zaman aşımı. Varsayılan değer 300'dür.
- **MinServerMBytes**: ulaşmak için en düşük sunucu megabayt. Varsayılan değer 500'dür.
- **SendBytes**: boyutu bayt cinsinden sunucuya gönderilen yük. Varsayılan değer 0'dır.
- **SendInterval**: sunucuya iletileri arasında geçen milisaniye cinsinden gecikme. Varsayılan değer 500'dür.
- **SendTimeout**: sunucuya ileti için milisaniye cinsinden zaman aşımı. Varsayılan değer 300'dür.
- **ControllerUrl**: bir istemci ana bilgisayar denetleyicisi hub Url. Varsayılan olarak NULL'dur (denetleyici hub). Denetleyici hub mili oturumu başladığında başlatıldı; Daha fazla kişi denetleyicisi hub'ı ve mili arasında yapılır.
- **NumClients**: uygulamaya bağlanmak için sanal istemci sayısı. Varsayılan biridir.
- **Günlük dosyası**: test çalıştırması için bir günlük dosyası için dosya adı. Varsayılan, `crank.csv` değeridir.
- **SampleInterval**: performans sayacı Örnekler arasındaki milisaniye olarak süre. Varsayılan değer 1000'dir.
- **SignalRInstance**: sunucu üzerindeki performans sayaçları için örnek adı. Varsayılan istemci bağlantı durumu kullanmaktır.

### <a name="example"></a>Örnek

Aşağıdaki komutu adlı bir sitede sınayacak `pfsignalr` "ControllerHub" adlı bir hub ile bağlantı noktası 8080 üzerinde uygulama barındıran Azure üzerinde 100 bağlantı kullanarak.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
