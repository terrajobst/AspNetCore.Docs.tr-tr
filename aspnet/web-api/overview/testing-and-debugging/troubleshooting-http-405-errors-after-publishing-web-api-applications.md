---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Sorun giderme HTTP 405 hatalarıyla yayımlamadan sonra Web API uygulamaları | Microsoft Docs
author: rmcmurray
description: Bu öğreticide, bir üretim web sunucusu için bir Web API'sini uygulama yayımlandıktan sonra HTTP 405 hatalarıyla ilgili sorunları giderme açıklar.
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: ce5b617cc1032d190cc2450aa554b462ea6f6156
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667420"
---
# <a name="troubleshooting-http-405-errors-after-publishing-web-api-applications"></a>Web API uygulamaları yayımlandıktan sonra HTTP 405 hatalarında sorun giderme

> Bu öğreticide, bir üretim web sunucusu için bir Web API'sini uygulama yayımlandıktan sonra HTTP 405 hatalarıyla ilgili sorunları giderme açıklar.
> 
> ## <a name="software-used-in-this-tutorial"></a>Bu öğreticide kullanılan yazılım
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (sürüm 7 veya üzeri)
> - [Web API](../../index.md) 


Web API uygulamaları, genellikle birden çok genel HTTP fiilleri kullanın: GET, POST, PUT, DELETE ve bazen düzeltme eki. Başka bir deyişle, geliştiriciler burada bu fiilleri tarafından uygulanır Visual Studio'da veya bir geliştirme sunucusunda düzgün çalıştığını bir Web API denetleyicisi döndürür burada bir durum için müşteri adayları, üretim sunucusu üzerinde başka bir IIS modül durumları içine çalıştırılabilir bir HTTP 405 bir üretim sunucusuna dağıtıldığında hata. Neyse ki bu sorunu kolayca çözümlenir, ancak çözümleme sorunu neden oluştuğunu bir açıklama gerektirir.

## <a name="what-causes-http-405-errors"></a>Hangi HTTP 405 hatalarına neden olur

HTTP 405 hatalarını sorun öğrenirken ilk adımı, bir HTTP 405 hata gerçekten anlamı öğrenmektir. Birincil yöneten belge HTTP için [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), HTTP 405 durum kodu olarak tanımlayan ***yönteme izin verilmiyor***ve bir durum olarak bu durum kodu daha ayrıntılı açıklanmıştır burada &quot;yöntemi Belirtilen istek satırı Request-URI tarafından tanımlanan kaynak için izin verilmiyor.&quot; Diğer bir deyişle, bir HTTP istemci istedi belirli URL için HTTP fiili izin verilmez.

Kısa bir inceleme İşte birkaç en çok kullanılan HTTP yöntemleri RFC 2616 ', RFC 4918 ve RFC 5789 tanımlandığı şekilde:

| HTTP yöntemi | Açıklama |
| --- | --- |
| **GET** | Bu yöntem, verileri bir URI ve onu büyük olasılıkla en çok kullanılan HTTP yöntemi almak için kullanılır. |
| **HEAD** | Bu yöntem alma yöntemini benzer olduğunu aslında istek URI'SİNDE veri almıyorsa - yalnızca HTTP durumunu alır dışında. |
| **POST** | Bu yöntem, genellikle yeni bir veri URI'si göndermek için kullanılır; POST, genellikle form verileri göndermek için kullanılır. |
| **PUT** | Bu yöntem, genellikle URI ham veri göndermek için kullanılır; PUT genellikle JSON veya XML verisi Web API uygulamalarına göndermek için kullanılır. |
| **DELETE** | Bu yöntem, verileri bir URI'den kaldırmak için kullanılır. |
| **SEÇENEKLER** | Bu yöntem, genellikle bir URI için desteklenen HTTP yöntemleri listesini almak için kullanılır. |
| **KOPYALAMA TAŞIMA** | WebDAV ile kullanılan bu iki yöntem ve bunların amacı açıklayıcıdır. |
| **MKCOL** | Bu yöntem ile WebDAV kullanılır ve belirtilen URI'de bir koleksiyon (örneğin bir dizin) oluşturmak için kullanılır. |
| **PROPFIND PROPPATCH** | Bu iki yöntem WebDAV ile kullanılır ve bunlar sorgulamak ya da bir URI için özellikleri ayarlamak için kullanılır. |
| **KİLİTLEME KİLİDİNİ AÇMA** | WebDAV ile kullanılan bu iki yöntem ve yazarken istek URI'si tarafından tanımlanan kaynağa Kilitle/kilidini açmak için kullanılır. |
| **DÜZELTME EKİ** | Bu yöntem, var olan bir HTTP kaynağı değiştirmek için kullanılır. |

Bu HTTP yöntemlerinden biri olan sunucuda kullanmak için yapılandırıldığında, sunucu, HTTP durumunu ve istek için uygun olan diğer verilerle yanıt verir. (Örneğin, bir GET yöntemi bir HTTP 200 alabilirsiniz ***Tamam*** yanıt ve PUT yöntemi bir HTTP 201 alma ***oluşturulan*** yanıt.)

HTTP yöntemi, sunucu üzerinde kullanılmak üzere yapılandırılmamışsa sunucu ile bir HTTP 501 yanıtlar ***uygulanmadı*** hata.

Ancak, bir HTTP yöntemini sunucuda kullanılmak üzere yapılandırılmış, ancak belirli bir URI için devre dışı olduğunda, sunucu bir HTTP 405 ile yanıt verir ***yöntemi izin*** hata.

## <a name="example-http-405-error"></a>Örnek HTTP 405 hata

Aşağıdaki örnek HTTP istek ve yanıt burada değeri bir web sunucusunda bir Web API'sini uygulamaya KOYMAK bir HTTP istemci çalışıyor ve sunucu PUT yöntemini değil durumları izin verilen bir HTTP hatası döndürür bir durum gösterilmektedir:


HTTP isteği:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


HTTP yanıtı:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


Bu örnekte, geçerli bir JSON isteği HTTP istemcisi bir web sunucusunda bir Web API uygulaması URL'sini gönderilen, ancak sunucu PUT yöntemini URL'sini izin verilmedi olduğunu belirten bir HTTP 405 hata iletisi döndürdü. Buna karşılık, istek URI'si, Web API uygulaması için bir rota ile eşleşmedi, sunucunun bir HTTP 404 döndürecekti ***bulunamadı*** hata.

## <a name="resolve-http-405-errors"></a>HTTP 405 hataları çözün

Neden belirli bir HTTP fiiline izin, ancak baştaki IIS'de bu hatanın nedenini olan birincil senaryo yoktur birkaç nedeni vardır: birden fazla işleyici aynı fiili/yöntem için tanımlanan ve işleyiciler birini beklenen işleyicisinden engelleme isteği işleniyor. Açıklama yoluyla, sipariş işleyici girişleri nerede yolu, fiil, kaynak, vs. ilk eşleşen birleşimi isteği işlemek için kullanılacak applicationHost.config ve web.config dosyalarında son dayalı olarak ilk işleyicilerindeki IIS işler.

Aşağıdaki örnek, bir Web API uygulaması için veri göndermek için PUT yöntemini kullanırken, bir HTTP 405 hata döndüren bir IIS sunucusu için applicationHost.config dosyasındaki bir alıntı. Bu alıntı birkaç HTTP işleyici tanımlanır ve her işleyici HTTP yöntemleri için yapılandırıldığı farklı bir dizi vardır - son listesindeki diğer işleyicilerin bir chanc verdikten, kullanılan varsayılan işleyici statik içerik işleyici girdidir e-istek inceleyin:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

Yukarıdaki örnekte, WebDAV işleyicisi ve uzantısız URL işleyicisi (Web API'si için kullanılır) ASP.NET için açıkça HTTP yöntemleri ayrı bir listesi için tanımlanır. Bu yapılandırma mutlaka hataya neden olmaz ancak ISAPI DLL işleyicisi tüm HTTP yöntemleri için yapılandırıldığını unutmayın. Ancak, HTTP 405 hatalarında sorun giderme göz önünde bulundurulması gerek gibi yapılandırma ayarları.

Yukarıdaki örnekte, ISAPI DLL işleyicisi sorun değildi; Aslında, sorun IIS sunucusu için applicationHost.config dosyasında tanımlanmadı - Visual Studio'da Web API'si uygulama oluşturulduğunda, web.config dosyasında yapılan bir giriş soruna neden. Uygulamanın web.config dosyasında alınan aşağıdaki alıntıda sorun konumunu gösterir:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

Bu alıntıda uzantısız URL işleyicisi ASP.NET Web API'sini uygulama ile kullanılacak ek HTTP yöntemleri içerecek şekilde tanımlandı. Ancak, benzer bir HTTP yöntemleri kümesini WebDAV işleyicisi tanımlamış bir çakışma oluşur. Bu belirli durumda WebDAV işleyici tanımlanır ve WebDAV Web API uygulamasını içeren bir Web sitesi için devre dışı olsa bile, IIS tarafından yüklendi. PUT fiil için tanımlamış bir HTTP PUT İsteği işleme sırasında IIS WebDAV modülünü çağırır. WebDAV modülü çağrıldığı zaman yapılandırmasını denetler ve bir HTTP 405 döndüreceği için devre dışı olduğunu, görür ***yöntemi izin*** WebDAV isteği benzeyen herhangi bir istek için hata. Bu sorunu çözmek için Web API uygulamanızı burada tanımlanan Web sitesi için HTTP modülleri listesinden WebDAV kaldırmanız gerekir. Aşağıdaki örnek görünüm beğenebileceğiniz ne gösterir:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Bu senaryo genellikle bir uygulama geliştirme ortamından üretim ortamına yayınlandıktan sonra işleyicileri/modüllerin listesini, geliştirme ve üretim ortamları arasında farklı olduğu için bu gerçekleşir karşılaşıldı. Örneğin, Visual Studio 2012 kullanıyorsanız veya daha sonra bir Web API uygulaması geliştirmek için IIS Express test etmek için varsayılan web sunucusu olabilir. Bu geliştirme web sunucusu bir sunucu üründe gönderilen tam IIS işlevselliğini ölçeği aşağı sürümüdür ve bu geliştirme web sunucusu geliştirme senaryoları için eklenmiş olan bazı değişiklikler içerir. Örneğin, gerçek kullanımda olmayabilir rağmen WebDAV modülü genellikle tam IIS sürümünü çalıştıran bir üretim web sunucusuna yüklenir. IIS, (IIS Express) geliştirme sürümünü WebDAV modülünü yükler, ancak özellikle, IIS Express yapılandırması yapmadığınız sürece WebDAV modülü hiçbir zaman IIS Express'te yüklenmesi için girişler WebDAV modülü için kasıtlı olarak, yorum eklenmiş IIS Express yüklemenizi WebDAV işlevselliği eklemek için Ayarlar'ı tıklatın. Sonuç olarak, web uygulamanızı geliştirme bilgisayarınızda doğru çalışmayabilir, ancak üretim web sunucunuza Web API uygulamanızı yayımladığınızda, HTTP 405 hatalarla karşılaşabilirsiniz.

## <a name="summary"></a>Özet

HTTP 405 hatalarına neden olduğunda, bir HTTP yöntemini bir web sunucusu tarafından istenen bir URL için izin verilmiyor. Bu durum, genellikle belirli bir işleyici için belirli bir fiil tanımlanır ve bu işleyici isteği işlemek için beklediğiniz işleyicisi geçersiz kılma olduğunda görülür.

Belirli işlevleri sunucuda uygulanmadı anlamına gelir, bir HTTP 501 hata iletisini aldığınız bir durumla karşılaşırsanız bu genellikle IIS Ayarları'nda tanımlanan hiçbir HTTP isteği, eşleşen işleyici anlamına gelir, büyük olasılıkla bir şey sisteminizde doğru şekilde yüklenmedi veya, böylece bu destek belirli HTTP yöntemi hiçbir işleyicileri tanımlanan bir şey, IIS ayarlarını değiştirdi gösterir. Bu sorunu çözmek için hiçbir karşılık gelen modül veya işleyici tanımlarına sahip olduğu bir HTTP yöntemini kullanmayı deniyor herhangi bir uygulamayı yeniden gerekir.
