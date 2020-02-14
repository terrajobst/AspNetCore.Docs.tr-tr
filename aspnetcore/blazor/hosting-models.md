---
title: Blazor barındırma modellerini ASP.NET Core
author: guardrex
description: Blazor WebAssembly ve Blazor sunucusu barındırma modellerini anlayın.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 2314ba39e67fbf734807b96de6c54bc94283a67d
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213320"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a>Blazor barındırma modellerini ASP.NET Core

[Daniel Roth](https://github.com/danroth27) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor, bir [Webassembly](https://webassembly.org/)tabanlı .NET çalışma zamanı ( *Blazor webassembly*) veya ASP.NET Core ( *Blazor Server*) sunucu tarafında tarayıcıda istemci tarafı çalıştırmak için tasarlanan bir Web çerçevesidir. Barındırma modelinden bağımsız olarak, uygulama ve bileşen modelleri *aynıdır*.

Bu makalede açıklanan barındırma modelleriyle ilgili bir proje oluşturmak için, bkz. <xref:blazor/get-started>.

Gelişmiş yapılandırma için bkz. <xref:blazor/hosting-model-configuration>.

## <a name="opno-locblazor-webassembly"></a>Blazor WebAssembly

Blazor için sorumlu barındırma modeli, WebAssembly üzerinde tarayıcıda istemci tarafında çalışmaktadır. Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir. Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür. UI güncelleştirmeleri ve olay işleme aynı işlem içinde oluşur. Uygulamanın varlıkları, istemcilere statik içerik sunan bir Web sunucusuna veya hizmete statik dosyalar olarak dağıtılır.

![[! Üs. NO-LOC (Blazor)] WebAssembly: [! Üs. NO-LOC (Blazor)] uygulaması, tarayıcı içindeki bir kullanıcı arabirimi iş parçacığında çalışır.](hosting-models/_static/blazor-webassembly.png)

İstemci tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için, **Blazor WebAssembly uygulama** şablonunu ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)) kullanın.

**Blazor WebAssembly uygulama** şablonunu seçtikten sonra, **ASP.NET Core barındırılan** onay kutusunu ([DotNet New blazorwasm--hosted](/dotnet/core/tools/dotnet-new)) seçerek uygulamayı ASP.NET Core arka ucunu kullanacak şekilde yapılandırma seçeneğiniz vardır. ASP.NET Core uygulaması, Blazor uygulamasına istemcilere hizmet verir. Blazor WebAssembly uygulaması, Web API çağrılarını veya [SignalR](xref:signalr/introduction) (<xref:tutorials/signalr-blazor-webassembly>) kullanarak ağ üzerinden sunucu ile etkileşime geçebilir.

Şablonlar aşağıdakileri işleyecek `blazor.webassembly.js` betiği içerir:

* .NET çalışma zamanını, uygulamayı ve uygulamanın bağımlılıklarını indirme.
* Uygulamayı çalıştırmak için çalışma zamanının başlatılması.

Blazor WebAssembly barındırma modeli çeşitli avantajlar sunar:

* .NET sunucu tarafı bağımlılığı yoktur. Uygulama, istemciye indirildikten sonra tamamen çalışır.
* İstemci kaynakları ve yetenekleri tamamen yararlanılabilir.
* İş sunucudan istemciye boşaltılır.
* Uygulamayı barındırmak için bir ASP.NET Core Web sunucusu gerekli değildir. Sunucusuz dağıtım senaryoları mümkündür (örneğin, bir CDN 'den uygulama sunma).

WebAssembly barındırma Blazor için aşağı yanlar vardır:

* Uygulama tarayıcının özelliklerine kısıtlıdır.
* Uyumlu istemci donanımı ve yazılımı (örneğin, WebAssembly desteği) gereklidir.
* İndirme boyutu daha büyüktür ve uygulamaların yüklenmesi daha uzun sürer.
* .NET çalışma zamanı ve araç desteği daha az olgun. Örneğin, [.NET Standard](/dotnet/standard/net-standard) desteğinin ve hata ayıklamada sınırlamalar mevcuttur.

## <a name="opno-locblazor-server"></a>Blazor sunucusu

Blazor sunucusu barındırma modeliyle uygulama, sunucuda bir ASP.NET Core uygulamasının içinden yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları [SignalR](xref:signalr/introduction) bir bağlantı üzerinden işlenir.

![Tarayıcı, sunucuda bir [! üzerinde barındırılan uygulamayla (bir ASP.NET Core uygulamasının içinde barındırılır) etkileşime girer. Üs. NO-LOC (SignalR)] bağlantısı.](hosting-models/_static/blazor-server.png)

Blazor sunucusu barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core **Blazor Server uygulama** şablonunu ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)) kullanın. ASP.NET Core uygulaması Blazor sunucusu uygulamasını barındırır ve istemcilerin bağlanacağı SignalR uç noktasını oluşturur.

ASP.NET Core uygulama, şu ekleme için uygulamanın `Startup` sınıfına başvurur:

* Sunucu tarafı hizmetler.
* İstek işleme işlem hattının uygulaması.

`blazor.server.js` betiği&dagger; istemci bağlantısını kurar. Uygulamanın, uygulama durumunu (örneğin, kayıp ağ bağlantısı durumunda) kalıcı hale getirmek ve geri yüklemek, uygulamanın sorumluluğundadır.

Blazor sunucusu barındırma modeli çeşitli avantajlar sunar:

* İndirme boyutu, Blazor WebAssembly uygulamasından önemli ölçüde küçüktür ve uygulama çok daha hızlı yüklenir.
* Uygulama, .NET Core ile uyumlu API 'lerin kullanımı dahil olmak üzere sunucu olanaklarından tam olarak yararlanır.
* Sunucuda .NET Core, uygulamayı çalıştırmak için kullanılır, bu nedenle hata ayıklama gibi mevcut .NET araçları beklendiği gibi çalışır.
* Ölçülü istemciler desteklenir. Örneğin, Blazor Server Apps, WebAssembly ve kaynak kısıtlı cihazlarda bulunan tarayıcılarla çalışır.
* Uygulamanın bileşen kodu da dahilC# olmak üzere, uygulamanın .NET/kod tabanı istemcilere sunulmuyor.

Sunucu barındırma Blazor için aşağı yanlar vardır:

* Daha yüksek gecikme süresi genellikle vardır. Her Kullanıcı etkileşimi bir ağ atmasını içerir.
* Çevrimdışı destek yoktur. İstemci bağlantısı başarısız olursa, uygulama çalışmayı durduruyor.
* Ölçeklenebilirlik, çok sayıda kullanıcısı olan uygulamalar için zorlayıcı bir uygulamalardır. Sunucunun birden çok istemci bağlantısını yönetmesi ve istemci durumunu işlemesi gerekir.
* Uygulamayı çalıştırmak için bir ASP.NET Core sunucusu gerekir. Sunucusuz dağıtım senaryoları mümkün değildir (örneğin, bir CDN 'den uygulama sunma).

&dagger;`blazor.server.js` betiği ASP.NET Core paylaşılan çerçevede eklenmiş bir kaynaktan sunulur.

### <a name="comparison-to-server-rendered-ui"></a>Sunucu tarafından işlenmiş Kullanıcı arabirimine karşılaştırma

Blazor Server uygulamalarını anlamanın bir yolu, Razor görünümlerini veya Razor Pages kullanarak ASP.NET Core uygulamalarda Kullanıcı arabirimini işlemek için geleneksel modellerden nasıl farklılık gösterir. Her iki model de, HTML içeriğini anlatmak için Razor dilini kullanır, ancak biçimlendirmenin nasıl işlendiği konusunda önemli ölçüde farklılık gösterir.

Bir Razor sayfası veya görünüm işlendiğinde, her Razor kodu satırı metin biçiminde HTML yayar. Oluşturulduktan sonra sunucu, üretilen herhangi bir durum da dahil olmak üzere sayfayı veya görünüm örneğini ortadan kaldırır. Sayfa için başka bir istek gerçekleştiğinde, örneğin sunucu doğrulaması başarısız olduğunda ve doğrulama özeti görüntülendiğinde:

* Sayfanın tamamı HTML metnine yeniden eklenir.
* Sayfa istemciye gönderilir.

Blazor bir uygulama, *Bileşenler*adlı Kullanıcı arabiriminin yeniden kullanılabilir öğelerinden oluşur. Bir bileşen kod C# , biçimlendirme ve diğer bileşenleri içerir. Bir bileşen işlendiğinde Blazor, bir HTML veya XML Belge Nesne Modeli (DOM) gibi dahil edilen bileşenlerin bir grafiğini üretir. Bu grafik, özelliklerde ve alanlarında tutulan bileşen durumunu içerir. Blazor, biçimlendirme ikili gösterimini üretmek için bileşen grafiğini değerlendirir. İkili biçimi şu şekilde olabilir:

* HTML metnine açıldı (prerendering&dagger;sırasında).
* Düzenli işleme sırasında biçimlendirmeyi verimli bir şekilde güncelleştirmek için kullanılır.

&dagger;*Prerendering* &ndash; istenen Razor bileşeni, sunucuda statik HTML 'ye derlenmekte ve istemciye gönderildiğinde, kullanıcıya işlendiğinde. İstemci ve sunucu arasında bağlantı kurulduktan sonra, bileşenin statik ön işlenmiş öğeleri etkileşimli öğelerle değiştirilmiştir. Prerendering, uygulamanın kullanıcıya daha fazla yanıt vermesini sağlar.

Blazor bir kullanıcı arabirimi güncelleştirmesi tarafından tetiklenir:

* Düğme seçme gibi kullanıcı etkileşimi.
* Zamanlayıcı gibi uygulama Tetikleyicileri.

Grafik yeniden tanımlanır ve bir UI *farkı* (fark) hesaplanır. Bu fark, istemcideki Kullanıcı arabirimini güncelleştirmek için gereken en küçük DOM düzenlemelerinin kümesidir. Fark istemciye bir ikili biçimde gönderilir ve tarayıcı tarafından uygulanır.

Kullanıcı, istemci üzerinde bundan uzaklaştığında bir bileşen atılmış olur. Bir Kullanıcı bir bileşenle etkileşim kurarken, bileşenin durumu (hizmetler, kaynaklar) sunucunun belleğinde tutulmalıdır. Birçok bileşenin durumu sunucu tarafından eşzamanlı olarak Korunabileceğinden, bellek tükenmesi sorunu ele alınmalıdır. Sunucu belleğinin en iyi şekilde kullanılmasını sağlamak üzere Blazor sunucu uygulamasının nasıl yazılacağı hakkında yönergeler için bkz. <xref:security/blazor/server>.

### <a name="circuits"></a>Uygulanıp

Blazor sunucusu uygulaması [ASP.NET Core SignalR](xref:signalr/introduction)üzerine kurulmuştur. Her istemci, bir *devre*olarak adlandırılan bir veya daha fazla SignalR bağlantı üzerinden sunucu ile iletişim kurar. Devre, geçici ağ kesintilerine tolerans sağlayan SignalR bağlantıları üzerinden Blazorsoyutlamasıdır. Blazor istemci SignalR bağlantısının kesileceğini gördüğünde, yeni bir SignalR bağlantısı kullanarak sunucuya yeniden bağlanmaya çalışır.

Bir Blazor sunucusu uygulamasına bağlı her tarayıcı ekranı (tarayıcı sekmesi veya IFRAME) SignalR bir bağlantı kullanır. Bu, tipik sunucu tarafından işlenmiş uygulamalarla karşılaştırıldığında daha önemli bir ayırım ifade etmiştir. Sunucu tarafından işlenen bir uygulamada, aynı uygulamayı birden çok tarayıcı ekranında açmak genellikle sunucuda ek kaynak taleplerine çevirilmez. Blazor sunucusu uygulamasında, her tarayıcı ekranı, sunucu tarafından yönetilecek ayrı bir devre ve bileşen durumunun ayrı örneklerini gerektirir.

Blazor bir tarayıcı sekmesini kapatmayı *veya bir dış* URL 'ye gidilmesini göz önünde bulundurur. Düzgün sonlandırma durumunda, devre ve ilişkili kaynaklar hemen serbest bırakılır. Bir istemci, örneğin bir ağ kesintisi nedeniyle düzgün şekilde kesilmeyen bir şekilde kesilebilir. Blazor sunucusu, istemcinin yeniden bağlanmasına izin vermek için, yapılandırılabilir bir Aralık için bağlantısı kesilen devreleri depolar. Daha fazla bilgi için bkz. [aynı sunucuya yeniden bağlanma](xref:blazor/hosting-model-configuration#reconnection-to-the-same-server).

### <a name="ui-latency"></a>UI gecikmesi

UI gecikme süresi, başlatılan bir eylemden Kullanıcı arabiriminin güncelleştirildiği zamana kadar geçen süredir. Bir uygulamanın kullanıcıya yanıt vermesi için kullanıcı ARABIRIMI gecikmesi için daha küçük değerler zorunludur. Blazor sunucu uygulamasında her bir eylem sunucusuna gönderilir, işlenir ve bir UI farkı geri gönderilir. Sonuç olarak, UI gecikmesi ağ gecikme süresinin toplamı ve eylemi işlerken sunucu gecikmesi sayısıdır.

Özel bir kurumsal ağla sınırlı bir iş kolu uygulaması için, ağ gecikmesi nedeniyle kullanıcı gecikmesi algılarını üzerindeki etki, genellikle çok sayıda CEPSİZ olur. Internet üzerinden dağıtılan bir uygulama için, özellikle de kullanıcılar coğrafi olarak coğrafi olarak dağıtılmışsa gecikme süresi kullanıcılara karşı farklılık gösterebilir.

Bellek kullanımı ayrıca uygulama gecikme süresine de katkıda bulunabilir. Daha fazla bellek kullanımı, her ikisi de uygulama performansının düşmesine neden olan ve bu nedenle kullanıcı arabirimi gecikmesini arttığı diskte sık görülen çöp toplama veya disk belleği belleği Daha fazla bilgi için bkz. <xref:security/blazor/server>.

Blazor sunucu uygulamaları, ağ gecikmesini ve bellek kullanımını azaltarak UI gecikmesini en aza indirmek için iyileştirilmelidir. Ağ gecikmesini ölçmeye yönelik bir yaklaşım için bkz. <xref:host-and-deploy/blazor/server#measure-network-latency>. SignalR ve Blazorhakkında daha fazla bilgi için bkz.

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a>Sunucuyla bağlantı

Blazor Server uygulamaları sunucuya etkin bir SignalR bağlantısı gerektirir. Bağlantı kaybolursa, uygulama sunucuya yeniden bağlanmaya çalışır. İstemcinin durumu hala bellekte olduğu sürece, istemci oturumu durum kaybı olmadan devam eder.

Sunucu üzerinde kullanıcı arabirimi durumunu ayarlayan ilk istemci isteğine yanıt olarak önceden bir Blazor sunucusu uygulaması ön ekler. İstemci bir SignalR bağlantısı oluşturmayı denediğinde, istemci aynı sunucuya yeniden bağlanmalıdır. birden fazla arka uç sunucusu kullanan Blazor Server uygulamalarının SignalR bağlantıları için *yapışkan oturumlar* uygulaması gerekir.

Blazor Server uygulamaları için [Azure SignalR hizmetini](/azure/azure-signalr) kullanmanızı öneririz. Hizmet, Blazor sunucu uygulamasının ölçeğini çok sayıda eşzamanlı SignalR bağlantı ile ölçeklendirmeye olanak tanır. Azure SignalR hizmeti için, hizmetin `ServerStickyMode` seçeneği veya yapılandırma değeri `Required`olarak ayarlanarak yapışkan oturumlar etkinleştirilir. Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/server#signalr-configuration>.

IIS kullanırken, yapışkan oturumlar uygulama Isteği yönlendirme ile etkinleştirilir. Daha fazla bilgi için bkz. [uygulama Isteği yönlendirme kullanarak HTTP yük dengelemesi](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:blazor/get-started>
* <xref:signalr/introduction>
* <xref:blazor/hosting-model-configuration>
* <xref:tutorials/signalr-blazor-webassembly>
