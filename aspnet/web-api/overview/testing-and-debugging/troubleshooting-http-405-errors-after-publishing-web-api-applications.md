---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: "HTTP sorun giderme 405 hataları yayımlama sonra Web API 2 uygulamaları | Microsoft Docs"
author: rmcmurray
description: "Bu öğretici, bir üretim web sunucusuna bir Web API uygulama yayımlandıktan sonra HTTP 405 hatalarında sorun giderme açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: b87ae7420e1295030e90c30e97b1e331413ce263
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>HTTP sorun giderme 405 hataları yayımlama sonra Web API 2 uygulamaları
====================
tarafından [Robert McMurray '](https://github.com/rmcmurray)

> Bu öğretici, bir üretim web sunucusuna bir Web API uygulama yayımlandıktan sonra HTTP 405 hatalarında sorun giderme açıklar.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (sürüm 7 veya üzeri)
> - [Web API](../../index.md) (sürüm 1 veya 2)


Web API uygulamaları genellikle birkaç ortak HTTP fiilleri kullanın: GET, POST, PUT, DELETE ve bazen düzeltme eki. Burada bu fiiller tarafından uygulanır Visual Studio'da veya bir geliştirme sunucusu üzerinde doğru şekilde çalışan Web API denetleyicisi döndürür burada bir durum müşteri adayları, üretim sunucusu üzerinde başka bir IIS modül durumları içine başka bir deyişle, geliştiricilerin çalışabilir bir HTTP 405 bir üretim ortamına dağıtıldığında hata. Neyse ki bu sorunu kolayca çözümlenir, ancak çözümleme sorunu neden oluşan bir açıklama garanti eder.

## <a name="what-causes-http-405-errors"></a>Hangi nedenler HTTP 405 hataları

HTTP 405 hatalarını sorun öğrenme ilk adım, bir HTTP 405 hata gerçekte anlamı anlamaktır. Birincil yöneten belge HTTP için [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), HTTP 405 durum kodu olarak tanımlayan ***yöntemini verilmiyor***ve başka bir durum olarak bu durum kodu açıklar nerede &quot;yöntemi Belirtilen Request-Line Request-URI tarafından tanımlanan kaynak için izin verilmiyor.&quot; Diğer bir deyişle, HTTP fiili bir HTTP istemci isteğinde belirli URL için izin verilmiyor.

Kısa bir gözden geçirme İşte birkaç en sık kullanılan HTTP yöntemleri RFC 2616, RFC 4918 ve RFC 5789 tanımlanan:

| HTTP yöntemi | Açıklama |
| --- | --- |
| **AL** | Bu yöntem, verileri bir URI ve bunu büyük olasılıkla en sık kullanılan HTTP yöntemini almak için kullanılır. |
| **HEAD** | Bu yöntem GET yöntemi benzer olduğunu gerçekte istek URI'si veri almıyorsa - yalnızca HTTP durum alır dışında. |
| **YAYINLA** | Bu yöntem, genellikle URI'si yeni veri göndermek için kullanılır; POST genellikle form verileri göndermek için kullanılır. |
| **PUT** | Bu yöntem, genellikle URI ham verileri göndermek için kullanılır; PUT genellikle Web API uygulamaları JSON veya XML veri göndermek için kullanılır. |
| **SİL** | Bu yöntem, verileri bir URİ'den kaldırmak için kullanılır. |
| **SEÇENEKLER** | Bu yöntem, genellikle bir URI için desteklenen HTTP yöntemleri listesini almak için kullanılır. |
| **KOPYALAMA TAŞIMA** | Bu iki yöntem WebDAV ile kullanılır ve bunların amaçla kendinden açıklamalıdır. |
| **MKCOL** | Bu yöntem WebDAV ile kullanılır ve belirtilen URI'de koleksiyonu (örneğin bir dizin) oluşturmak için kullanılır. |
| **PROPFIND PROPPATCH** | Bu iki yöntem WebDAV ile kullanılır ve sorgu veya için URI özelliklerini ayarlamak için kullanılır. |
| **KİLİTLEME KİLİDİNİ AÇMA** | Bu iki yöntem WebDAV ile kullanılır ve yazarken, istek URI'si tarafından tanımlanan kaynağa Kilitle/kilidini açmak için kullanılır. |
| **DÜZELTME EKİ** | Bu yöntem, varolan bir HTTP kaynağı değiştirmek için kullanılır. |

Bu HTTP yöntemlerden birini sunucuda kullanmak için yapılandırıldığında, sunucu HTTP durum ve istek için uygun olan diğer veri ile yanıt verir. (Örneğin, bir GET yöntemi bir HTTP 200 alabilirsiniz ***Tamam*** yanıt ve PUT yöntemini HTTP 201 alma ***oluşturulan*** yanıt.)

HTTP yöntemi, sunucu üzerinde kullanılmak üzere yapılandırılmamışsa, sunucu ile bir HTTP 501 yanıt ***uygulanmadı*** hata.

Ancak, bir HTTP yöntemini sunucuda kullanmak için yapılandırıldı, ancak belirli bir URI için devre dışı olduğunda, sunucu bir HTTP 405 ile yanıt verir ***yöntemini verilmiyor*** hata.

## <a name="example-http-405-error"></a>Örnek HTTP 405 hatası

Aşağıdaki örnek HTTP istek ve yanıt burada bir HTTP istemcisi bir web sunucusunda bir Web API uygulaması için değer almaya çalışıyor ve sunucu PUT yöntemini değil durumları izin verilen bir HTTP hata döndürür bir durumu gösterir:


HTTP isteği:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


HTTP yanıtı:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


Bu örnekte, bir web sunucusu üzerinde bir Web API uygulamasının URL'si geçerli bir JSON İstek HTTP istemcisi gönderilen, ancak sunucu PUT yöntemini URL'sini izin verilmedi olduğunu gösteren bir HTTP 405 hata iletisi döndürdü. Buna karşılık, istek URI'si ile Web API uygulaması için bir rota eşleşmiyor, sunucunun bir HTTP 404 döndürecektir ***bulunamadı*** hata.

## <a name="resolving-http-405-errors"></a>HTTP 405 çözümleme hataları

Neden belirli bir HTTP fiili verilmiyor, ancak baştaki bu hatanın nedeni, IIS'de olan birincil senaryo yok birkaç nedeni vardır: birden çok işleyici için aynı fiil/yöntemi tanımlanır ve işleyicileri birini beklenen işleyicisinden engelleme istek işleme. Açıklama yapmamanız IIS sipariş işleyici girişleri nerede yolu, fiil, kaynak, vb., ilk eşleşen birleşimi isteği işlemek için kullanılacak applicationHost.config ve web.config dosyalarında son dayalı olarak ilk işleyicilerini işler.

Aşağıdaki örnek bir alıntı applicationHost.config dosyasındaki bir HTTP 405 hata PUT yöntemini kullanarak bir Web API uygulamasına veri göndermek için zaman döndüren bir IIS sunucusu için aynıdır. Bu alıntı birkaç HTTP işleyicileri tanımlanan ve her işleyici HTTP yöntemleri kendisi için yapılandırıldığı farklı bir dizi vardır - son listesindeki diğer işleyicilerin bir chanc verdikten kullanılan varsayılan işleyici statik içerik işleyici girdidir İstek incelemek üzere e:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

Yukarıdaki örnekte WebDAV işleyicisi ve uzantısız URL işleyicisi (hangi Web API için kullanılır) ASP.NET açıkça HTTP yöntemlerini ayrı listesi için tanımlanır. Bu yapılandırma mutlaka hataya neden olmaz ancak ISAPI DLL işleyicisi için tüm HTTP yöntemleri, yapılandırılmış olduğunu unutmayın. Ancak, HTTP 405 hatalarında sorun giderme göz önünde bulundurulması gerek bu gibi yapılandırma ayarları.

Yukarıdaki örnekte, ISAPI DLL işleyicisi sorun değil; Aslında, sorun IIS sunucusunun applicationHost.config dosyasında tanımlanmadı - sorun Visual Studio ile Web API uygulaması oluşturulduğunda, web.config dosyasında yapılan bir giriş ile neden oldu. Uygulamanın web.config dosyasından aşağıdaki alıntı sorun konumunu gösterir:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

Bu alıntı, uzantısız URL işleyicisi ASP.NET Web API uygulama ile kullanılacak ek HTTP yöntemleri dahil etmek için tanımlandı. Ancak, benzer bir HTTP yöntemleri kümesini WebDAV işleyicisi tanımlamış bir çakışma meydana gelir. Bu belirli durumda WebDAV işleyici tanımlanır ve WebDAV Web API uygulama içeren bir Web sitesi için devre dışı olsa da, IIS tarafından yüklenir. PUT fiil için tanımlamış bir HTTP PUT İsteği işleme sırasında IIS WebDAV modülü çağırır. WebDAV modülü çağrıldığında yapılandırmasını denetler ve devre dışı olduğundan, bir HTTP 405 döndürülecek şekilde görür ***yöntemini verilmiyor*** WebDAV isteği benzer herhangi bir istek için hata. Bu sorunu çözmek için Web API uygulamanızın tanımlandığı Web sitesi için HTTP modülleri listesinden WebDAV kaldırmanız gerekir. Aşağıdaki örnek ne görünüm beğenebileceğiniz gösterir:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Bu senaryo genellikle uygulama geliştirme ortamından üretim ortamına yayımlandıktan sonra işleyicileri/modüllerin listesini geliştirme ve üretim ortamlar arasında farklı olduğundan bu oluşur karşılaştı. Örneğin, bir Web API uygulaması geliştirmek için Visual Studio 2012 veya 2013 kullanıyorsanız, IIS Express 8 test etmek için varsayılan web sunucusu'dir. Bu geliştirme web sunucusunu bir sunucu üründe gönderilen tam IIS işlevselliğini ölçeklendirilmiş aşağı sürümüdür ve bu geliştirme web sunucusu geliştirme senaryoları için eklenen birkaç değişiklik içerir. Örneğin, gerçek kullanımda olmayabilir ancak WebDAV modülü genellikle IIS,'ın tam sürümünü çalıştıran bir üretim web sunucusunda yüklü. IIS, (IIS Express), geliştirme sürümünü WebDAV modülünü yükler, ancak özellikle IIS Express yapılandırmanızı yapmadığınız sürece WebDAV modülü hiçbir zaman IIS Express yüklendi şekilde WebDAV modülü girişlerinde bilerek, dışı bırakılır IIS Express yüklemenizi WebDAV işlevsellik eklemek için ayarlar. Sonuç olarak, web uygulamanızı geliştirme bilgisayarınızda doğru çalışmayabilir, ancak Web API uygulamanızı üretim web sunucunuza yayımladığınızda, HTTP 405 hatalarla karşılaşabilirsiniz.

## <a name="summary"></a>Özet

HTTP 405 hataları nedeniyle, bir HTTP yöntemini bir web sunucusu tarafından istenen URL için izin verilmiyor. Bu durum genellikle belirli bir işleyicinin belirli bir fiil için tanımlanan ve bu işleyici, isteği işlemek için beklediğiniz işleyicisi geçersiz kılma görülür.

Belirli işlevleri sunucuda uygulanmadı anlamına gelir, bir HTTP 501 hata iletisini aldığınız bir durumla karşılaşırsanız bu genellikle HTTP isteği eşleşen IIS Ayarları'nda tanımlanan hiçbir işleyici olduğu anlamına gelir, büyük olasılıkla bir şey doğru sisteminizde yüklü değil veya vardır; böylece bu destek belirli HTTP yöntem hiçbir işleyicileri tanımlı bir şey IIS ayarlarınızı değiştirdi gösterir. Bu sorunu çözmek için hiçbir karşılık gelen modül veya işleyici tanımları sahip olduğu bir HTTP yöntemini kullanmayı deniyor herhangi bir uygulama yeniden yüklemeniz gerekir.
