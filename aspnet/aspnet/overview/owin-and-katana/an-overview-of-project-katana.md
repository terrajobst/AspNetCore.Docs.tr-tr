---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: "Proje Katana genel bakış | Microsoft Docs"
author: howarddierking
description: "ASP.NET Framework on yıldan için geçici olmuştur ve platform geliştirme sayısız Web siteleri ve Hizmetleri etkinleştirdi. Web Outlook'ta olarak..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 8f28116f88f3cf5143d3d5c9821519d62c4e5452
ms.sourcegitcommit: 6541c8b11001dd617adf5eb04c814cda165070b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
<a name="an-overview-of-project-katana"></a>Proje Katana genel bakış
====================
tarafından [Howard Dierking](https://github.com/howarddierking)

> ASP.NET Framework on yıldan için geçici olmuştur ve platform geliştirme sayısız Web siteleri ve Hizmetleri etkinleştirdi. Web uygulaması geliştirme stratejileri gelişim göstermiştir gibi framework ASP.NET MVC ve ASP.NET Web API gibi teknolojileriyle adımda gelişmesi mümkün olmuştur. Web uygulaması geliştirme bulut dünyasına sonraki Açılım adım yararlanırken proje [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) bileşenleri olmalarını esnek, taşınabilir, etkinleştirme, ASP.NET uygulamaları için temel alınan dizi sağlar Basit ve daha iyi performans sağlayan başka bir deyişle, proje [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) bulut ASP.NET uygulamalarınızın iyileştirir.


## <a name="why-katana--why-now"></a>Neden Katana – neden şimdi?

 Bir geliştirici framework ya da son kullanıcı ürün ele olup olmadığını bakılmaksızın, oluşturmak için temel alınan sözleri anlamak önemlidir ürün – ve bunun bir parçası içerir ürünü için oluşturan bilerek. ASP.NET, özgün önünde iki müşterilerle oluşturuldu.   
  
**Müşteriler ilk grubunu klasik ASP geliştiricileri oluştu.** Aynı anda ASP biçimlendirme ve sunucu tarafı komut dosyası interweaving dinamik, veri güdümlü Web siteleri ve uygulamaları oluşturmak için birincil teknolojilerden birine oluştu. ASP çalışma zamanı sunucu tarafı komut dosyası, temel alınan HTTP protokolünü ve Web sunucusu çekirdek yönlerini soyutlanır ve ek erişim gibi oturum ve uygulama Durum Yönetimi Hizmetleri sağlanan önbelleğe alma, vb. bir nesne ile sağlanan. Güçlü olsa da, klasik ASP uygulamalarını boyutu ve karmaşıklığı büyüdükçe yönetmek için bir sınama hale geldi. Bu, büyük ölçüde çoğaltma kod ve biçimlendirme Interleaving alanından elde edilen kod ile birlikte ortamları komut bulunan yapısı eksikliği nedeniyle oluştu. Klasik ASP güçlerini kendi zorluklarından bazıları adresleme sırasında büyük harfe çevirme için sunucu tarafı programlama modeli de korurken .NET Framework nesne yönelimli dilleri tarafından sağlanan kod kuruluş avantajlarından ASP.NET sürdü Geliştiriciler için hangi klasik ASP alışık.

**ASP.NET için hedef müşteri ikinci grubunun Windows iş uygulaması geliştiricileri oluştu.** HTML İşaretleme ve daha fazla HTML biçimlendirmesi oluşturmak için kod yazmaya alışkın, klasik ASP geliştiriciler, farklı WinForms geliştiriciler (gibi VB6 geliştiriciler önlerinde) bir tuval ve zengin bir kullanıcı bulunan bir tasarım zamanı deneyimi alışkın olan Arabirim denetler. ASP.NET – ilk sürümü olarak da bilinen "Web Forms" kullanıcı arabirimi bileşenleri için sunucu tarafı olay modeli ve bir dizi altyapı özellikleri (örneğin, Görünüm durumu) ile birlikte benzer bir tasarım zamanı deneyimi sorunsuz geliştirici deneyimi oluşturmak için sağlanan İstemci ve sunucu tarafı programlama arasında. Web Forms etkili bir şekilde Web'in durum bilgisiz yapısı WinForms geliştiricilerinin aşina bir durum bilgisi olan olay modeli altında HID.

### <a name="challenges-raised-by-the-historical-model"></a>Geçmiş modeli tarafından gerçekleştirilen zorluklarından

**Net sonucu olgun, zengin çalışma zamanı ve geliştirici programlama modeli oluştu.** Ancak, ile özelliği zenginliğini birkaç önemli zorluklar geldi. İlk olarak, çerçeve olan **tek yapılı**, aynı System.Web.dll bütünleştirilmiş (örneğin, Web forms framework çekirdek HTTP nesnelerle) sıkı şekilde bağlı işlevselliğin mantıksal olarak farklı birimleri ile. İkincisi, ASP.NET, bu amacı büyük .NET Framework, bir parçası olarak dahil edilen **sürümleri arasındaki süre terabayt yıl oluştu.** Tüm Web geliştirme hızla gelişen içinde gerçekleştiği değişiklikleriyle ayak tutmak ASP.NET zor hale. Son olarak, System.Web.dll kendisini belirli bir Web barındırma seçeneği için birkaç farklı şekillerde eşleşmiş: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Açılım adımlar: ASP.NET MVC ve ASP.NET Web API

Ve çok sayıda değişiklik Web geliştirme gerçekleştiği! Küçük bir dizi odaklanmış gibi büyük çerçeveleri yerine bileşenleri web uygulamaları giderek geliştirilmekte. Bileşenleri ve bunun yanı sıra, bunlar yayımlanan sıklığı her zamankinden daha hızlı bir oranda artırıldığında. Web ile ayak tutma yerine daha büyük ve daha zengin, bu nedenle daha küçük, ayrılmış ve daha odaklı almak için çerçeveler gerektirecek açıktı **ASP.NET takım sürdü ASP.NET ailesi etkinleştirme birkaç Açılım adımları tek bir çerçeve yerine takılabilir Web Bileşenleri**.

Erken değişikliklerden birini rayları sayesinde Web geliştirme çerçeveleri Ruby gibi bilinen model-view-controller (MVC) tasarım düzenini popülerliği yükselişe oluştu. Web uygulamaları oluşturma bu stili Geliştirici vermiş ASP.NET ilk satış noktaları biri, biçimlendirme ve iş mantığı ayrılması hala korurken her uygulamanın biçimlendirme üzerinde daha fazla denetim. Bu Web uygulaması geliştirme stili için talebi karşılamak üzere Microsoft kendisini konumlandırmak için Fırsat tarafından gelecek için daha iyi sürdü **bant dışı ASP.NET MVC geliştirme** (ve .NET Framework dahil değil). ASP.NET MVC bağımsız bir yükleme olarak yayımlanmıştır. Bu, önceden mümkün olan çok daha sık güncelleştirmeler sunmaya esneklik mühendislik ekibi verdi.

Web uygulaması geliştirme başka bir önemli shift istemci tarafı komut dosyası iletişim kurmasını oluşturulan sayfa dinamik bölümleri olan statik ilk biçimlendirme dinamik, sunucu tarafından üretilen Web sayfalarından kaydırma edildi **arka uç Web API'leri ile aracılığıyla AJAX istekleri**. Bu mimari shift Web API'leri, yükseklik ve ASP.NET Web API çerçevesi geliştirme propel olunmasına yardımcı oldu. ASP.NET MVC gibi söz konusu olduğunda ASP.NET daha modüler bir çerçeve olarak daha fazla gelişmesi başka bir fırsat ASP.NET Web API sürümü sağlanır. Mühendislik ekibi fırsatı avantajlarından sürdü ve **System.Web.dll içinde bulunan Çekirdek framework türlerinden herhangi birini üzerinde hiçbir bağımlılık sahip olacağı şekilde ASP.NET Web API yerleşik**. Bu iki şey etkin: ilk olarak, bu amacı ASP.NET Web API tamamen bağımsız bir şekilde gelişmesi (ve hızlı bir şekilde NuGet teslim edildiğinden yinelemek devam edebilir). İkinci olarak, System.Web.dll için dış bağımlılıkları ve bu nedenle, IIS bağımlılıkları olduğundan, ASP.NET Web API özelliği bir özel ana bilgisayar (örneğin, bir konsol uygulaması, Windows hizmeti, vb.) çalıştırmak dahil

### <a name="the-future-a-nimble-framework"></a>Gelecekte: Nimble çerçevesi

Framework bileşenleri birbirinden ayırma ve onları NuGet üzerinde serbest bırakmadan çerçeveleri şimdi verebilir **daha bağımsız olarak ve daha hızlı yineleme**. Ayrıca, güç ve esneklik Web API'nin kendi kendine barındırma yeteneğinin oluyor uygulamasına yol açıyordu isteyen geliştiriciler için çok çekici bir **küçük, basit konak** kendi Hizmetleri için. Bu nedenle çekici oluyor uygulamasına yol açıyordu, aslında, diğer çerçeveler de bu özelliği istediği ve her framework kendi temel adresini kendi ana bilgisayar işlemi içinde çalıştırılan ve (başlatıldı, durdurulmuş, vs.) yönetilmesi için gereken bu yeni bir sınama ortaya bağımsız olarak. Modern bir Web uygulaması genellikle statik dosya sunucusu, dinamik sayfa oluşturma, Web API ve daha yakın zamanda real-zamanlı/anında iletme bildirimleri destekler. Bu hizmetlerin her birini çalıştırmak ve gerekir bağımsız olarak yönetilen, bekleniyor yalnızca gerçekçi değildi.

Nelerin gerekli farklı bileşenleri ve çerçeveleri çeşitli bir uygulama oluşturmak bir geliştirici sağlayan ve ardından destekleyen bir ana bilgisayarda bu uygulamayı çalıştırmak tek bir barındırma soyutlama oluştu.

## <a name="the-open-web-interface-for-net-owin"></a>.NET (OWIN) için açık Web arabirimi

 Tarafından elde edilen yararlar tarafından neden olacak [raf](http://rack.github.io/) Söyleniş topluluğuna birkaç .NET topluluk üyeleri Web sunucuları ve framework bileşenleri arasındaki bir Özet oluşturmak için ayarlanan. OWIN soyutlama iki Tasarım hedeflerini basit ve diğer framework türlerinde en az olası bağımlılıklar sürdü emin olan. Bu iki hedeflerini yardımcı olur:

- Yeni bileşen daha kolay geliştirilen tüketilen ve yüklenemedi.
- Uygulamalar daha kolay olası tüm Platform/işletim sistemi ve ana bilgisayarlar arasında bağlantı noktası kurulmuş.

Sonuçta elde edilen soyutlama iki çekirdek öğelerden oluşur. İlk ortam sözlüğünü ' dir. Bu veri yapısını, tüm HTTP isteği ve yanıt yanı sıra, tüm ilgili sunucu durumu işlemek için gerekli durumunun depolamak için sorumludur. Ortam sözlüğünü şu şekilde tanımlanır:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Bir OWIN uyumlu Web sunucusu ortamı sözlüğü gövde akışını ve bir HTTP isteği ve yanıt üstbilgi koleksiyonlarıyla gibi verileri ile doldurmak için sorumludur. Bunun ardından doldurmak veya sözlük ek değerler ile güncelleştirin ve yanıt gövdesi akışına yazmak için uygulama veya framework bileşenleri sorumluluğundadır.

Ortam sözlüğünü türü belirtmeye ek olarak OWIN belirtimi çekirdek sözlük anahtar değer çiftlerinin listesini tanımlar. Örneğin, aşağıdaki tabloda bir HTTP isteği için gerekli sözlük anahtarları gösterilmektedir:

| Anahtar adı | Değer açıklaması |
| --- | --- |
| `"owin.RequestBody"` | İstek gövdesi varsa olan bir akış. Hiçbir istek gövdesi ise Stream.Null yer tutucu olarak kullanılabilir. Bkz: [istek gövdesinde](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Bir `IDictionary<string, string[]>` İstek üstbilgilerinin. Bkz: [üstbilgileri](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | A `string` isteğin HTTP istek yöntemi içeren (örn., `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | A `string` istek yolu içeren. "Root" uygulama temsilcisi; göreli yolu olmalıdır bkz: [yolları](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | A `string` "Kök" uygulama temsilcisi; karşılık gelen istek yolu bölümünü içeren bkz [yolları](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | A `string` protokol adı ve sürümü içeren (örn. `"HTTP/1.0"` veya `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | A `string` başında olmayan HTTP isteği URI, sorgu dizesi bileşeni içeren "?" (örn., `"foo=bar&baz=quux"`). Değer boş bir dize olabilir. |
| `"owin.RequestScheme"` | A `string` istek için kullanılan URI düzeni içeren (örn., `"http"`, `"https"`); bkz [URI düzeni](http://owin.org/html/owin.html#5-1-uri-scheme). |

İkinci anahtar OWIN uygulaması temsilci öğesidir. OWIN uygulamasının tüm bileşenler arasındaki birincil arabirim olarak hizmet veren bir işlev imzası budur. Uygulama temsilci tanımı aşağıdaki gibidir:

`Func<IDictionary<string, object>, Task>;`

Uygulama temsilci sonra yalnızca bir yere işlevi ortam sözlüğünü giriş olarak kabul eder ve bir görev döndürür Func temsilci türü uygulamasıdır. Bu tasarım, geliştiriciler için birkaç etkilere sahiptir:

- Çok az sayıda OWIN bileşenleri yazmak için gerekli tür bağımlılıkları vardır. Bu OWIN erişilebilirlik geliştiriciler için büyük ölçüde artırır.
- Zaman uyumsuz tasarım Özet bilgi işlem kaynaklarının, özellikle birden çok g/ç işlemlerinde yoğun kendi işleme ile verimli olmasını sağlar.
- Uygulama temsilci bir atomik birimidir ve ortam sözlüğünü temsilci üzerinde bir parametre olarak gerçekleştirilen çünkü OWIN bileşenleri kolayca yapılabilmesi için birlikte karmaşık HTTP ardışık düzen işleme oluşturmak için.

Bir uygulama açısından OWIN belirtimidir ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Sonraki Web çerçevesi, ancak Web çerçeveleri ve Web sunucularının nasıl etkileşime yerine bir belirtim olmaması için kendi hedeftir.

Araştırılan varsa [OWIN](http://owin.org/) veya [Katana](https://github.com/aspnet/AspNetKatana/wiki), ayrıca fark etmiş [Owın NuGet paketi](http://nuget.org/packages/Owin) ve Owin.dll. Bu kitaplık tek bir arabirim içeren [Iappbuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), testini biçimlendirir ve açıklanan başlatma sırası kod oluşturur [bölüm 4](http://owin.org/html/owin.html#4-application-startup) OWIN belirtimi. OWIN sunucuları oluşturmak için gerekli olmasa da [Iappbuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) arabirimi somut başvuru noktası sağlar ve Katana proje bileşenleri tarafından kullanılır.

## <a name="project-katana"></a>Proje Katana

Oysa hem [OWIN](http://owin.org/html/owin.html) belirtimi ve *Owin.dll* topluluk ait ve açık kaynak çalışmalarını Çalıştır topluluk [Katana](https://github.com/aspnet/AspNetKatana/wiki) proje OWIN kümesini temsil eder hala açık kaynak sırasında oluşturulan ve Microsoft tarafından yayınlanan bileşenleri. Bu bileşenler hem ana bilgisayarları ve sunucuları gibi altyapı bileşenlerini, hem de kimlik doğrulaması bileşenleri ve çerçeveleri bağlamalar gibi işlev bileşenleri gibi içeren [SignalR](../../../signalr/index.md) ve [ASP.NET Web API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Proje aşağıdaki üç üst düzey hedefi vardır: 

- **Taşınabilir** – bileşenleri kullanılabilir durumda olduklarında en yeni bileşenleri için kolayca değiştirilen mümkün. Bu ana bilgisayar ve sunucu için çerçeveden bileşenlerinin tüm türleri içerir. Olduğu çıkarımında bulunulur bu hedefin Microsoft çerçeveleri olabilecek üçüncü taraf sunucular ve ana bilgisayarlarda çalıştırabilirsiniz ancak üçüncü taraf çerçeveleri sorunsuz bir şekilde Microsoft sunucularda çalıştırabilirsiniz.
- **Modüler/esnek**– varsayılan olarak açıktır özellikleri çok sayıda dahil birçok çerçeveleri, küçük ve odaklanmış, denetim için hangi bileşenlerin belirlemede uygulama geliştiricisi vererek Katana proje bileşenleri olması gerekir her uygulamada kullanın.
- **Basit/kullanıcı/ölçeklenebilir** – bir çerçeve geleneksel kavramı küçük, kümesine yeni bir sonuç Katana uygulama, daha az sayıda bilgisayar kullanabileceğinden uygulama geliştiricisi tarafından açıkça eklenen bileşenleri odaklanmış Kaynaklar ve sonuç olarak, daha fazla yük daha sunucularının ve çerçeveleri diğer türleri tanıtıcı. Uygulamanın gereksinimlerini altyapının daha fazla özelliklerinden isteğe bağlı olarak, bunları OWIN ardışık düzenine eklenebilir, ancak uygulama geliştiricisi bölümüne açık bir karar olmalıdır. Ayrıca, alt düzey bileşenlerden substitutability kullanılabilir durumda olduklarında en yeni yüksek performanslı sunucular sorunsuz bir şekilde bu uygulamaları bozmadan OWIN uygulamaların performansını artırmak için bir şekilde uygulanması anlamına gelir.

## <a name="getting-started-with-katana-components"></a>Katana bileşenleri ile çalışmaya başlama

Bu ilk zaman sunulmuştur, tek bir yönüne [Node.js](http://nodejs.org/) hemen kişilerin dikkat u çizdiğini framework edildi Basitlik ile bir yazar ve bir Web sunucusunda çalıştırın. Katana hedefleri light, Çerçeveli varsa [Node.js](http://nodejs.org/), biri özetlemek bunları Katana birçok avantajları beraberinde getirir söyleyerek [Node.js](http://nodejs.org/) (ve bu gibi çerçeveleri) throw Geliştirici zorlamadan her şeyi aynen ASP.NET Web uygulamaları geliştirme hakkında bilir. Doğru tutmak bu deyim için Katana proje ile çalışmaya başlama yapısı eşit basit olmalıdır [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>"Hello World!" oluşturma

JavaScript ve .NET geliştirme arasındaki önemli bir fark derleyici varlığı (veya yokluğuna) ' dir. Bu nedenle, bir basit Katana sunucu için başlangıç noktası bir Visual Studio projesi ' dir. Ancak, biz en az proje türleri ile başlayabilirsiniz: boş ASP.NET Web uygulaması.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Ardından, yükleriz [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) NuGet paketini projeye. Bu paket ASP.NET istek ardışık düzeninde çalıştıran bir OWIN sunucusu sağlar. Üzerinde bulunabilir [NuGet galerisinde](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) ve aşağıdaki komutla Visual Studio Paket Yöneticisi iletişim kutusu veya Paket Yöneticisi konsolu kullanılarak yüklenebilir:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Yükleme `Microsoft.Owin.Host.SystemWeb` paket birkaç ek paketler bağımlılık olarak yüklenir. Bu bağımlılıklar, biri `Microsoft.Owin`, bir kitaplık birkaç yardımcı türleri ve OWIN uygulamaları geliştirmek için yöntemler sağlar. Hızlı bir şekilde aşağıdaki "hello world" sunucu yazmak için Biz bu türlerde kullanabilirsiniz.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Bu çok basit bir Web sunucusu artık Visual Studio'nun kullanılarak çalıştırılabilir **F5** komut ve hata ayıklama için tam destek içerir.

## <a name="switching-hosts"></a>Konaklar geçiş yapma

Varsayılan olarak, önceki "hello world" örnek System.Web IIS bağlamında kullanan ASP.NET istek kanalında çalışır. Bize esneklik ve yönetim özelliklerine sahip bir OWIN ardışık composability ve IIS'nin genel olgunluk yararlanmaya etkinleştirdiğinden bu tek başına yapabilmesinin değeri ekleyebilirsiniz. Ancak, burada IIS tarafından sağlanan avantajları gerekli değildir ve isteğini daha küçük, daha basit ana bilgisayar için durumlar olabilir. Daha sonra bizim basit bir Web sunucusu IIS ve System.Web dışında çalıştırmak için gerekenleri?

Taşınabilirlik hedef göstermek için bir Web sunucusu ana bilgisayardan bir komut satırı ana bilgisayara taşınması yalnızca yeni sunucu ve ana bilgisayar bağımlılıkları projenin çıkış klasörüne eklemek ve ardından konak başlatılmasını gerektiriyor. Bu örnekte, biz bizim Web sunucusu adı verilen bir Katana ana bilgisayar için ana bilgisayar `OwinHost.exe` ve Katana HttpListener tabanlı sunucu kullanır. Benzer şekilde diğer Katana bileşenler için bu aşağıdaki komutu kullanarak Nuget'ten kazanılması:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Komut satırından biz proje kök klasöre gidin ve çalıştırmanız yeterlidir `OwinHost.exe` (kendi ilgili NuGet paketi Araçlar klasöründeki yüklenen). Varsayılan olarak, `OwinHost.exe` HttpListener tabanlı sunucu aramak için yapılandırılmış ve ek bir yapılandırma gerekmez. Bir Web tarayıcısında gezinme `http://localhost:5000/` şimdi Konsolu aracılığıyla çalışan uygulama gösterir.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Katana mimarisi

 Aşağıda gösterildiği gibi dört mantıksal katmanı, bir uygulamaya Katana bileşen mimarisi böler: *konak, sunucu, ara yazılım,* ve *uygulama*. Bileşen mimarisi, bu katmanların uygulamaları kolayca, çoğu durumda, uygulamanın yeniden derlenmek gerek kalmadan kullanılabileceğini şekilde katılır.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Ana bilgisayar

 Ana bilgisayar sorumlu olduğu:

- Temel alınan işlem yönetme.
- Sunucu seçimi ve hangi istekleri aracılığıyla bir OWIN ardışık yapımı sonuçlanır iş akışı yönetme işleneceğini.

 Şu anda Katana tabanlı uygulamalar için 3 birincil barındırma seçenekleri vardır:  
  
**IIS/ASP.NET**: Standart HTTP ve HttpHandler türlerini kullanarak, bir ASP.NET istek akışının bir parçası olarak OWIN ardışık düzen IIS'de çalıştırabilirsiniz. Destek barındırma ASP.NET Web uygulaması projesine Microsoft.AspNet.Host.SystemWeb NuGet paketini yükleyerek etkinleştirilir. Ayrıca, IIS hem bir ana bilgisayar hem de bir sunucu olarak davrandığından OWIN sunucu/ana ayrım SystemWeb ana bilgisayar kullanıyorsanız, bir geliştirici başka sunucu uygulaması yerine, yani bu NuGet paketi conflated.  
  
**Özel ana bilgisayar**: Katana bileşen suite imkanı geliştirici kendi özel işleminde uygulamalarını barındırmak için olup bir konsol uygulaması, Windows hizmeti, vb. Bu özellik, Web API'si tarafından sağlanan kendini barındırma özellik benzer. Aşağıdaki örnek, Web API kodunda özel ana bilgisayar gösterir:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Kendini barındırma Kurulum Katana uygulama için benzer:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Web API ve Katana kendini barındırma Örnekler arasındaki önemli bir fark Web API yapılandırması kod Katana kendini barındırma örnekten eksik olmasıdır. Taşınabilirlik ve composability etkinleştirmek için sunucu istek ardışık düzen işleme yapılandırır kodundan başlar kod Katana ayırır. Web API yapılandırır ve ayrıca WebApplication.Start türü parametresi olarak belirtilen başlangıç sınıfında yer alan kodu.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

Başlangıç sınıfı makalenin sonraki bölümlerinde daha ayrıntılı açıklanmıştır. Ancak, kendi kendini barındıran işlem kendini barındırma ASP.NET Web API uygulamalarındaki bugün kullanıyor olabilirsiniz koduna benzerlik arar Katana başlatmak için kod gereklidir.

**OwinHost.exe**: bazı Katana Web uygulamalarını çalıştırmak için özel bir işlem yazmak istediğiniz, ancak çoğu yalnızca bir sunucuyu başlatır ve kullanıcıların uygulamayı çalıştırmak önceden oluşturulmuş bir yürütülebilir dosya başlatmak tercih ettiğiniz. Bu senaryo için Katana bileşen suite içerir `OwinHost.exe`. Gelen bir projenin kök dizininde çalıştırdığınızda, bu yürütülebilir dosya (varsayılan olarak HttpListener sunucunun kullandığı) bir sunucu başlatın ve bulma ve kullanıcının başlangıç sınıfı çalıştırmak için kuralları kullanın. Daha ayrıntılı bir denetim için ek komut satırı parametreleri yürütülebilir sağlar.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Sunucu

 Ana bilgisayar başlatma ve işlem içinde uygulama çalıştığında, sunucunun sorumluluğunda koruma sorumlu olsa da bir ağ yuva açın, istekleri dinlemesi ve bunları OWIN bileşenleri ardışık düzen üzerinden göndermek için (olarak, kullanıcı tarafından belirtilen zaten fark, bu ardışık düzen uygulama geliştiricinin başlangıç sınıfında belirtilir). Şu anda Katana projesi iki sunucu uygulamaları içerir: 

- **Microsoft.Owin.Host.SystemWeb**: daha önce belirtildiği gibi ASP.NET ardışık düzeni ile birlikte IIS'de bir ana bilgisayar ve bir sunucu davranır. Bu nedenle, bu seçenek barındırma seçerken, IIS hem ana bilgisayar düzeyinde sorunları işlem etkinleştirme gibi yönetir ve HTTP isteklerini dinler. ASP.NET Web uygulamaları için daha sonra ASP.NET ardışık düzenine istekler gönderir. Katana SystemWeb ana bilgisayar, bir ASP.NET HTTP ve HttpHandler HTTP ardışık düzen üzerinden akış ve kullanıcı tarafından belirtilen OWIN ardışık düzeni gönderecek şekilde isteklerin kesilmesi için kaydeder.
- **Microsoft.Owin.Host.HttpListener**: adından da anlaşılacağı gibi bu Katana sunucu yuva açın ve geliştirici tarafından belirtilen bir OWIN ardışık düzenine istekleri göndermek için .NET Framework'ün HttpListener sınıfını kullanır. Şu anda Katana kendini barındırma API ve OwinHost.exe için varsayılan sunucu seçimi budur.

## <a name="middlewareframework"></a>Ara yazılım/framework

 Sunucu bir istemciden gelen istek kabul ettiğinde daha önce belirtildiği gibi geliştiricinin başlangıç koduna göre belirtilen OWIN bileşenlerini ardışık düzeninden geçirmesi sorumludur. Bu ardışık düzeni bileşenleri ara yazılımı bilinir.  
 Çok temel düzeyde, böylece aranabilir OWIN uygulaması temsilci uygulamak bir OWIN ara yazılım bileşeni yalnızca gerekir.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Ancak, geliştirme ve ara yazılımı bileşenleri oluşumunu basitleştirmek için ara yazılımı bileşenleri için kuralları ve yardımcı türü sayıda Katana destekler. Bunlardan en yaygın olduğu `OwinMiddleware` sınıfı. Bu sınıf kullanılarak oluşturulan bir özel ara yazılım bileşeni aşağıdakine benzer görünür: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Bu sınıfın türetildiği `OwinMiddleware`, ardışık düzende sonraki ara yazılım örneği bağımsız değişkenlerinden biri olarak kabul edip temel oluşturucuya geçirmeden bir oluşturucu uygular. Ara yazılım yapılandırmak için kullanılan ek bağımsız değişkenler de sonraki ara yazılım parametresi sonra Oluşturucusu parametre olarak bildirilir.   
  
Çalışma zamanında ara yazılım yürütülür geçersiz kılınan aracılığıyla `Invoke` yöntemi. Bu yöntem tek bir bağımsız değişken türü alır `OwinContext`. Bu bağlam nesnesi tarafından sağlanan `Microsoft.Owin` NuGet paketi daha önce açıklanan ve istek, yanıt ve ortam sözlüğü, birkaç ek yardımcı türleri ile birlikte kesin türü belirtilmiş erişmenizi sağlar.   
  
Ara yazılım sınıfı kolayca uygulama başlangıç koduna OWIN ardışık gibi eklenebilir:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Katana altyapı yalnızca OWIN ara yazılımı bileşenleri bir ardışık düzen oluşturulduğundan ve bileşenleri ardışık düzeninde katılmayı uygulama temsilci desteklemek yeterlidir çünkü ara yazılımı bileşenleri karmaşıklığı basit değişebilir ASP.NET Web API gibi tüm çerçeveler için günlükçüleri veya [SignalR](../../../signalr/index.md). Örneğin, ASP.NET Web API önceki OWIN ardışık düzenine eklemek aşağıdaki başlangıç kod ekleme gerektirir:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Katana altyapı ardışık düzen yapılandırma yöntemine Iappbuilder nesnesine eklendikleri sipariş göre ara yazılım bileşenlerinin oluşturacaksınız. Bizim örneğimizde, daha sonra bu istekleri sonuçta işlenme bağımsız olarak ardışık düzeninden akış tüm istekleri LoggerMiddleware işleyebilir. Bu, burada bir ara yazılım bileşeni (örn. bir kimlik doğrulama bileşeni) çoklu bileşenleri ve çerçeveleri (örn. ASP.NET Web API SignalR ve bir statik dosya sunucusu) içeren bir ardışık düzeni için istekleri işleyebilmesi için güçlü senaryolara olanak sağlar.
 
## <a name="applications"></a>Uygulamalar

Önceki örneklerin gösterildiği üzere, OWIN ve Katana projesi, yeni bir uygulama programlama modeli, ancak uygulama programlama modelleri ve çerçeveleri sunucusuna gelen ve barındırma altyapı aynı şekilde yerine bir Özet olarak düşünülmelidir değil. Örneğin, Web API uygulamaları oluştururken, geliştirici framework desteklemediğini Katana projeden Bileşenleri'ni kullanarak bir OWIN ardışık düzenindeki uygulama çalıştıran bağımsız olarak ASP.NET Web API çerçevesi kullanmaya devam eder. Burada OWIN ilgili kod uygulama geliştiricisi için görünür olacak tek bir yerden uygulama başlangıç koduna burada Geliştirici OWIN ardışık düzenini oluşturur olacaktır. Başlangıç kodu Geliştirici UseXx deyimleri, genellikle bir gelen istekleri işleyen her ara yazılım bileşeni için bir dizi kaydeder. Bu deneyim geçerli System.Web dünyada HTTP modülleri kaydetme olarak aynı etkisi olmaz. Genellikle, bir büyük framework ara yazılım, ASP.NET Web API gibi veya [SignalR](../../../signalr/index.md) ardışık düzen sonunda kaydedilir. Böylece tüm çerçeveler ve ardışık düzeninde kayıtlı bileşenleri için istekleri işleyecek kimlik doğrulaması veya önbelleğe alma için olanlar gibi çapraz kesme ara yazılımı bileşenleri genellikle ardışık düzen başına doğru kaydedilir. Bu ayrılması ara yazılımı bileşenleri birbirinden ve temel altyapı bileşenlerini genel sistem kararlı kalmasını sağlarken farklı velocities gelişmesi bileşenleri sağlar.

## <a name="components--nuget-packages"></a>Bileşenleri – NuGet paketleri

Birçok geçerli kitaplıklarını ve çerçevelerini gibi Katana proje bileşenleri NuGet paketlerini bir dizi gönderilir. Yaklaşan sürüm 2.0, Katana paket bağımlılık grafiği aşağıdaki gibi görünür. (Daha büyük görünüm için görüntüye tıklayın.)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Neredeyse her paket Katana projesinde, doğrudan veya dolaylı olarak, Owın paketine bağlıdır. Bu bölüm OWIN belirtimi 4 açıklanan uygulama başlatma sırası somut bir uygulamasını sağlar Iappbuilder arabirimi içeren paketi olduğunu unutmayın. Ayrıca, çoğu paketlere HTTP istekleri ve yanıtları ile çalışmak için yardımcı türleri kümesi sağlar Microsoft.Owin bağlıdır. Paket geri kalanı, barındırma altyapı paketleri (sunucular veya ana bilgisayarları) veya ara yazılımı olarak sınıflandırılabilir. Paketler ve Katana projeye dış bağımlılıklar turuncu içinde görüntülenir.

Katana 2.0 barındırma alt yapısı hem SystemWeb ve HttpListener tabanlı sunucular, OwinHost.exe kullanarak OWIN uygulamalarını çalıştırmak için OwinHost paketi ve OWIN uygulamalarda kendi kendine barındırma için Microsoft.Owin.Hosting paketi içeren bir Özel ana bilgisayar (örn. Konsol uygulaması, Windows hizmeti, vb.)

Katana 2.0 için ara yazılımı bileşenleri öncelikle kimlik doğrulama farklı yollarla sağlamaya odaklanmıştır. Bir başlangıç ve hata sayfası için destek sağlayan bir ek ara yazılım bileşeni tanılama için sağlanır. OWIN gerçek barındırma soyutlama büyüdükçe, her iki olanlar Microsoft ve üçüncü taraflar tarafından geliştirilen ara yazılımı bileşenleri ekosistemi ayrıca numarasında büyüyecektir.

## <a name="conclusion"></a>Sonuç

 Kendi baştan oluşturabilir ve böylece geliştiriciler henüz başka bir Web çerçevesidir öğrenmek için zorlamak için Katana projenin hedef yedeklenmedi. Bunun yerine, hedef .NET Web uygulaması geliştiricileri daha önce mümkün olmuştur daha fazla seçenek sunmak için bir Özet oluşturmak için bırakıldı. Değiştirilebilir bileşenler kümesini tipik bir Web uygulaması yığına mantıksal katmanları yukarı bölerek Katana proje ne olursa olsun oranı bu bileşenler için mantıklı adresindeki geliştirmek için yığın boyunca bileşenleri sağlar. Tüm bileşenleri basit OWIN soyutlama geçici oluşturarak Katana çerçeveler ve çeşitli farklı sunucular ve ana bilgisayarlar arasında taşınabilmesini bunların üstüne oluşturulan uygulamaların sağlar. Yığın denetiminde Geliştirici koyarak Katana Geliştirici ultimate seçiminiz nasıl basit hale getirir sağlar ya da kendi Web yığını nasıl zengin olması gerekir.  
  

## <a name="for-more-information-about-katana"></a>Katana hakkında daha fazla bilgi için

- GitHub Katana projede: [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/).
- Video: [Katana proje - ASP.NET OWIN](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), Howard Dierking tarafından.

## <a name="acknowledgements"></a>Katkıda Bulunanlar

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick olduğundan Microsoft Azure ve MVC odaklanan için üst düzey bir programlama yazıcı.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
