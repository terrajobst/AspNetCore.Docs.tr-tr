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
ms.openlocfilehash: 308fed51953b085506488c5e0dda1ced9f4d09fb
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287597"
---
<a name="signalr-connection-density-testing-with-crank"></a>Crank ile SignalR Bağlantı Yoğunluğu Testi
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Bu makalede, bir uygulama birden çok sanal istemcileri ile test etmek için mili Aracı'nı kullanmayı açıklar.


Uygulamanızı (ya da bir Azure web rolü, IIS veya Owın kullanarak şirket içinde barındırılan), barındırma ortamı içinde çalışır duruma geçtikten sonra yüksek düzeyde bağlantı yoğunluğu testi mili aracını kullanarak bir uygulamanın yanıt test edebilirsiniz. Barındırma ortamı, Internet Information Services (IIS) sunucu, bir Owın konak veya bir Azure web rolü olabilir. (Not: Öğesinden bir bağlantı yoğunluğu testi performans verilerini almanız mümkün olmayacak şekilde performans sayaçları Azure App Service Web Apps üzerinde kullanılabilir değil.)

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

- **/?** : Yardım ekranını gösterir. Kullanılabilir seçenekler de görüntülenir **Url** parametresi atlanırsa.
- **/ Url**: SignalR bağlantıları için URL. Bu parametre gereklidir. Bir SignalR uygulama için varsayılan eşlemeyi kullanarak yolun içinde sona erecek "/ signalr".
- **/ Aktarım**: Kullanılan taşıma adı. Varsayılan `auto`, en iyi kullanılabilir protokol seçer. Seçenekleriniz `WebSockets`, `ServerSentEvents`, ve `LongPolling` (`ForeverFrame` yerine Internet Explorer kullanıldığı bir seçenek mili için bu yana .NET istemci değildir). SignalR taşımalar nasıl seçer? daha fazla bilgi için bkz: [aktarım ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: Her toplu eklenen istemcilere sayısı. Varsayılan değer 50'dir.
- **/ ConnectInterval**: Bağlantılar ekleme arasındaki milisaniye cinsinden aralığı. Varsayılan değer 500'dür.
- **Bağlantı**: Uygulama yük testi için kullanılan bağlantı sayısı. Varsayılan değer 100. 000 ' dir.
- **/ ConnectTimeout**: Test çalışmasını iptal etmeden önce saniye cinsinden zaman aşımı. Varsayılan değer 300'dür.
- **MinServerMBytes**: Ulaşmak için en düşük sunucu megabayt. Varsayılan değer 500'dür.
- **SendBytes**: Boyutu bayt cinsinden sunucusuna gönderilen yük. Varsayılan değer 0'dır.
- **SendInterval**: Sunucuya iletileri arasında geçen milisaniye cinsinden gecikme. Varsayılan değer 500'dür.
- **SendTimeout**: İletileri sunucusu için milisaniye cinsinden zaman aşımı. Varsayılan değer 300'dür.
- **ControllerUrl**: Burada bir istemci bir denetleyici hub'ı barındıracak URL'si. Varsayılan olarak NULL'dur (denetleyici hub). Denetleyici hub mili oturumu başladığında başlatıldı; Daha fazla kişi denetleyicisi hub'ı ve mili arasında yapılır.
- **NumClients**: Uygulamaya bağlanmak için sanal istemci sayısı. Varsayılan biridir.
- **Günlük dosyası**: Test çalıştırması için bir günlük dosyası için dosya adı. Varsayılan, `crank.csv` değeridir.
- **SampleInterval**: Performans sayacı Örnekler arasındaki milisaniye olarak süre. Varsayılan değer 1000'dir.
- **SignalRInstance**: Sunucusunda performans sayaçları için örnek adı. Varsayılan istemci bağlantı durumu kullanmaktır.

### <a name="example"></a>Örnek

Aşağıdaki komutu adlı bir sitede sınayacak `pfsignalr` "ControllerHub" adlı bir hub ile bağlantı noktası 8080 üzerinde uygulama barındıran Azure üzerinde 100 bağlantı kullanarak.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
