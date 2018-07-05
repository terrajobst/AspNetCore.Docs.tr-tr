---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Project Katana'ya genel bakış | Microsoft Docs
author: howarddierking
description: ASP.NET Framework etrafında on yılı aşkın süredir ve sayısız Web siteleri ve Hizmetleri geliştirme platformu etkinleştirdi. Web uygulaması olarak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 212594b503bfc17d3faa6df03e01402d2862522f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390194"
---
<a name="an-overview-of-project-katana"></a>Project Katana'ya genel bakış
====================
tarafından [Howard Dierking](https://github.com/howarddierking)

> ASP.NET Framework etrafında on yılı aşkın süredir ve sayısız Web siteleri ve Hizmetleri geliştirme platformu etkinleştirdi. Web uygulaması geliştirme stratejileri gelişim göstermiştir gibi framework adım ASP.NET MVC ve ASP.NET Web API gibi teknolojilerle evrim Geçiren oluşturabildi. Web uygulaması geliştirme gelişime sonraki adım, bulut bilgi işlem dünyasına yararlanırken, proje [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) bileşenleri olmalarını esnek ve taşınabilir, etkinleştirme, ASP.NET uygulamaları için temel alınan dizi sağlar Hafif ve daha iyi performans sağlayan başka bir deyişle, proje [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) bulut ASP.NET uygulamalarınızın iyileştirir.


## <a name="why-katana--why-now"></a>Neden Katana – neden şimdi?

 Bir geliştirici framework veya son kullanıcı ürün görüştükten olmadığını fark etmeksizin, oluşturmak için temel alınan motivasyonlardan anlamak önemlidir, bir parçası ve ürün – içeren ürünü için oluşturan bilerek. ASP.NET, başlangıçta göz önünde iki müşterilerle oluşturuldu.   
  
**Müşteriler ilk grubunu klasik ASP geliştiriciler oluştu.** Sırada, ASP işaretleme ve sunucu tarafı betik interweaving tarafından veri odaklı dinamik Web siteleri ve uygulamaları oluşturmak için birincil teknolojileri biriydi. ASP çalışma, sunucu tarafı komut çekirdek yönlerini temel alınan HTTP protokolünü ve Web sunucusu soyutlanır ve böyle oturum ve uygulama Durum Yönetimi Hizmetleri ek erişim sağlanan önbelleğe alma, vb. bir nesne ile sağlanan. Güçlü karşın, klasik ASP uygulamalarının boyutu ve karmaşıklığı büyüdükçe yönetmek zor hale geldi. Bu, büyük ölçüde çoğaltma işaretleme ve kod ve Interleaving alanından kaynaklanan kod ile birlikte ortamları komut bulunamadı yapısı olmaması nedeniyle oluştu. ASP.NET üzerinde klasik ASP gücü kendi zorlukları giderilirken büyük harfe çevirme için sunucu tarafı programlama modeli korurken .NET Framework nesne odaklı dildeki tarafından sağlanan kodu kuruluşu avantajlarından sürdü Geliştiriciler için hangi klasik ASP alışık.

**ASP.NET için hedef müşteriler ikinci grup, Windows iş uygulaması geliştiricilerin oluştu.** HTML İşaretleme ve daha fazla HTML biçimlendirmesi oluşturmak için kod yazmaya alışkın, klasik ASP geliştiriciler, farklı WinForms geliştiriciler (gibi bunları önce VB6 geliştiriciler) bir tuval ve zengin bir kullanıcı bir tasarım zamanı deneyimi alışkın olduğunuz arabirimi denetimleri. ASP.NET – ilk sürümü olarak da bilinen "Web Forms" yanı sıra kullanıcı arabirimi bileşenleri için bir sunucu tarafı olay modeli ve bir dizi altyapı özelliklerinden (örneğin, Görünüm durumu) benzer bir tasarım zamanı deneyimi sorunsuz bir geliştirici deneyimi oluşturmak için sağlanan İstemci ve sunucu tarafı programlama arasında. Web Forms etkili WinForms geliştiricilerinin aşina olduğu bir durum bilgisi olan olay modeli altında durum bilgisi olmayan yapısı Web gizlenmiş.

### <a name="challenges-raised-by-the-historical-model"></a>Geçmiş modeli tarafından ortaya zorlukları

**Olgun ve zengin özelliklere sahip bir çalışma zamanı ve geliştirici programlama modeli sonucunda oluştu.** Ancak, ile özellik zenginlik birkaç önemli zorlukları geldi. İlk olarak, çerçeve olan **tek parça**, işlevselliği aynı System.Web.dll derlemedeki (örneğin, Web formları framework ile çekirdek HTTP nesneler) sıkı şekilde bağlı mantıksal olarak birbirinden farklı birimleri ile. İkincisi, ASP.NET, amacı olan daha büyük .NET Framework, bir parçası olarak dahil edilen **sürümler arasında süresi yıl bazında oldu.** Bunun için ASP.NET ile Web geliştirme hızla gelişen içinde gerçekleştiği tüm değişiklikleri uydurmanıza zorlaştırıyordu. Son olarak, System.Web.dll kendisini belirli bir Web barındırma seçeneği için birkaç farklı şekilde bağlı: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Gelişime adımlar: ASP.NET MVC ve ASP.NET Web API

Ve değişiklik çok sayıda Web geliştirme oluşmasını! Bir dizi küçük, odaklanmış gibi büyük çerçeveleri yerine bileşenleri web uygulamaları giderek geliştirilmekte. Bileşenleri ve bunun yanı sıra, bunlar yayımlanan sıklığı sayısı, her zamankinden daha hızlı bir oranda artırma. Adım tutma ile Web çerçeveleri yerine daha büyük ve daha zengin, bu nedenle daha küçük, ayrılmış ve daha fazla odaklanmış almak için gerektirecek açıktı **ASP.NET takımı ailesi ASP.NET sağlamak üzere birkaç gelişime adımları sürdü tek bir çerçeve yerine takılabilir Web Bileşenleri**.

Erken değişiklikleri Rails üzerinde Ruby gibi Web geliştirme çerçeveleri sayesinde iyi bilinen model-view-controller (MVC) tasarım modelinin popülerliği artış biriydi. Bu stil, Web uygulamaları oluşturma, geliştirici verdiğiniz her uygulamanın biçimlendirme bir ASP.NET başlangıç satış noktası, biçimlendirme ve iş mantığı ayrımı hala korurken üzerinde daha fazla denetim. Bu tür bir Web uygulaması geliştirme için talebi karşılamak için Microsoft kendisine konumlandırmak için Fırsat tarafından gelecek için daha iyi sürdü **ASP.NET MVC bant dışında geliştirme** (ve .NET Framework'teki dahil değil). ASP.NET MVC, bağımsız bir indirme olarak yayımlanmıştır. Bu, daha önce mümkün olan çok daha sık güncelleştirmeler sunabilecek esnekliğe mühendislik ekibine getirdi.

Başka bir ana Web uygulama geliştirme ile istemci tarafı komut dosyası iletişim kurmasını oluşturulan sayfayı dinamik bölümlerini statik ilk biçimlendirme dinamik, sunucu tarafından oluşturulan Web sayfalarından kaydırma dönüşümdü **ile arka uç Web API'leri aracılığıyla AJAX isteği**. Bu mimari shift Web API'leri Yükselişi ve ASP.NET Web API çerçevesi geliştirilmesini propel yardımcı olmuştur. ASP.NET MVC gibi söz konusu olduğunda ASP.NET Web API sürümü ASP.NET daha modüler bir çerçeve olarak daha da geliştirmek için bir fırsat olarak sağlanır. Mühendislik ekibine fırsatı avantajlarından sürdü ve **System.Web.dll içinde bulunan Çekirdek framework türlerinden birinin üzerinde hiçbir bağımlılık olduğu gibi ASP.NET Web API yerleşik**. Bu iki şey etkin: ilk olarak, geliyordu ASP.NET Web API tamamen bağımsız bir şekilde evrim Geçiren (ve NuGet aracılığıyla teslim edildiğinden hızlıca yinelemek devam edebilir). İkinci olarak, ASP.NET Web API System.Web.dll için dış bağımlılıkları yoktur ve bu nedenle, IIS için bağımlılık olduğundan, özel bir ana (örneğin, bir konsol uygulaması, Windows hizmeti, vb.) çalıştırma özelliği dahil

### <a name="the-future-a-nimble-framework"></a>Gelecek: Hızlı çerçeve

Framework bileşenleri birbirinden ayırma ve sonra bunları NuGet üzerinde bırakarak çerçeveler artık verebilir **daha fazla Bağımsızlık ve daha hızlı yineleme**. Ayrıca, güç ve esneklik Web API'SİNİN kendi kendine barındırma yeteneğinin kanıtlandı istediği geliştiriciler için çok cazip bir **küçük, basit ana** hizmetlerini için. Bu nedenle çekici kanıtlandı, aslında, diğer çerçeveler de bu özelliği istediği ve her framework üzerinde kendi temel adresini, kendi ana işlemde çalışan ve yönetilecek (başlatıldı, durduruldu, vb.) gerekli bu yeni bir sınama ortaya bağımsız olarak. Modern bir Web uygulaması genel olarak statik dosya sunucusu, dinamik sayfa oluşturma, Web API ve daha kısa bir süre önce gerçek-zamanlı/anında iletme bildirimleri destekler. Bu hizmetlerin her biri verilecek çalıştırın ve bağımsız olarak yönetilen, bekleniyor yalnızca gerçekçi değildi.

Nelerin gerekli, çeşitli farklı bileşenlerini ve çerçeveleri dosyasından bir uygulama oluşturmak bir geliştirici etkinleştirmek, ve ardından uygulama destekleyen bir konak üzerinde çalıştırın tek bir barındırma soyutlama oluştu.

## <a name="the-open-web-interface-for-net-owin"></a>.NET (OWIN) için açık Web arabirimi

 Tarafından elde edilen yararlar ilham [raf](http://rack.github.io/) Web sunucuları ve framework bileşenleri arasında bir Özet oluşturmak için birkaç .NET topluluk üyelerinin Ruby topluluğuna belirlenmiş. Basit ve bunu diğer framework türleri üzerinde en az sayıda olası bağımlılıklar geçen iki tasarım hedefleri için OWIN özeti için olan. Bu iki hedeflerini yardımcı olur:

- Yeni bileşenleri daha kolayca geliştirilen ve tüketilecek.
- Uygulamaları daha kolay potansiyel olarak tüm Platform/işletim sistemi konaklar arasında unity'nin.

Sonuçta elde edilen soyutlama iki çekirdek öğelerden oluşur. Ortam sözlüğünü davranıştır. Tüm HTTP isteği ve yanıt yanı sıra, herhangi bir ilgili sunucu durumu işlenmesi için gereken durumları depolamak için bu veri yapısını sorumludur. Ortam sözlüğünü şu şekilde tanımlanır:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Bir OWIN uyumlu Web sunucusu ortamı sözlük gövdesi akışları ve bir HTTP isteği ve yanıt üstbilgi koleksiyonlarıyla gibi verilerle doldurmak için sorumludur. Ardından sözlük ek değerlerle güncelleştirmektir doldurmak veya yanıt gövdesi akışına yazma uygulama veya framework bileşenlerinin sorumluluğundadır.

Ortam sözlüğünü türünü belirtmenin yanı sıra, OWIN belirtimi çekirdek sözlük anahtar-değer çiftlerinin listesini tanımlar. Örneğin, bir HTTP isteği için gerekli sözlük anahtarları aşağıdaki tabloda gösterilmektedir:

| Anahtar adı | Değer Açıklama |
| --- | --- |
| `"owin.RequestBody"` | İstek gövdesi, varsa ile bir Stream. Hiçbir istek gövdesi varsa Stream.Null yer tutucu olarak kullanılabilir. Bkz: [istek gövdesi](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Bir `IDictionary<string, string[]>` istek üst bilgilerinin. Bkz: [üstbilgileri](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | A `string` isteğin HTTP istek yöntemini içeren (örn., `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | A `string` içeren istek yolu. "Kök" uygulama temsilcisi; göreli yol olmalıdır bkz: [yolları](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | A `string` "Kök" uygulama temsilcinin; karşılık gelen istek yolunun bir bölümü içeren bkz [yolları](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | A `string` protokol adı ve sürümü içeren (örn `"HTTP/1.0"` veya `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | A `string` olmadan önde gelen HTTP istek URI'si, sorgu dizesi bileşeni içeren "?" (örneğin, `"foo=bar&baz=quux"`). Değer boş bir dize olabilir. |
| `"owin.RequestScheme"` | A `string` istek için kullanılan URI düzeni içeren (örn., `"http"`, `"https"`); bkz [URI şeması](http://owin.org/html/owin.html#5-1-uri-scheme). |

İkinci anahtar öğesi, OWIN uygulama temsilcisidir. OWIN uygulamasının tüm bileşenler arasında birincil arabirim olarak hizmet veren bir işlev imzası budur. Uygulama temsilci tanımı aşağıdaki gibidir:

`Func<IDictionary<string, object>, Task>;`

Uygulama temsilcisi sonra yalnızca bir yere işlev ortam sözlüğünü girdi olarak kabul eder ve bir görev döndürür işlev temsilci türü uygulamasıdır. Bu tasarım, geliştiriciler için birçok olası etkileri vardır:

- Çok az sayıda OWIN bileşenleri yazmak için gerekli tür bağımlılıkları vardır. Bu, OWIN erişilebilirliğini geliştiricilere önemli ölçüde artırır.
- Zaman uyumsuz tasarım Özet bilgi işlem kaynaklarının, özellikle de daha fazla g/ç kullanımı yoğun işlemleri, işleme ile daha verimli olmasını sağlar.
- Uygulama temsilcisi bir atomik birim yürütme olduğundan ve ortam sözlüğünü temsilcinin parametre olarak yürütülür çünkü OWIN bileşenleri kolayca zincirlenebilir birlikte karmaşık HTTP işleme komut zincirlerini oluşturma.

Bir uygulama açısından bakıldığında, OWIN belirtimidir ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Sonraki Web çerçevesi, ancak Web çerçeveleri ve Web sunucuları nasıl etkileşim için bunun yerine bir belirtimi olmaması hedefi sağlamaktır.

Sizin araştırılması durumunda [OWIN](http://owin.org/) veya [Katana](https://github.com/aspnet/AspNetKatana/wiki), siz de etmişsinizdir [Owın NuGet paketini](http://nuget.org/packages/Owin) ve Owin.dll. Bu kitaplık içeren tek bir arabirim [Iappbuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), şekillendirir ve açıklanan başlatma sırası'ı kodlar [bölüm 4](http://owin.org/html/owin.html#4-application-startup) OWIN belirtimi. Yapı OWIN sunucusu için gerekli olmasa [Iappbuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) arabirimi somut bir başvuru noktası sağlar ve Katana proje bileşenleri tarafından kullanılır.

## <a name="project-katana"></a>Project Katana'ya

İse hem [OWIN](http://owin.org/html/owin.html) belirtimi ve *Owin.dll* topluluk ait ve açık kaynak çalışmalarını çalıştırma topluluk [Katana](https://github.com/aspnet/AspNetKatana/wiki) projeyi OWIN kümesini temsil eder yine de açık kaynak sırasında oluşturulan ve Microsoft tarafından yayınlanan bileşenleri. Bu bileşenler hem ana bilgisayar ve sunucuları gibi altyapı bileşenlerini, hem de kimlik doğrulaması bileşenleri ve çerçeveleri bağlama gibi işlevsel bileşenleri gibi dahil [SignalR](../../../signalr/index.md) ve [ASP.NET Web API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Proje, aşağıdaki üç üst düzey hedeflere sahiptir: 

- **Taşınabilir** – bileşenleri kullanılabilir oldukça yönelik yeni bileşenler kolayca yerine mümkün. Bu, sunucu ve ana bilgisayar için Framework bileşenlerinin tüm türleri içerir. Bu hedefin olduğu çıkarımında Microsoft çerçeveleri olabilecek üçüncü taraf sunucular ve ana bilgisayarlarda çalıştırabilirsiniz ancak üçüncü taraf çerçeve sorunsuz bir şekilde Microsoft sunucularında çalıştırabilirsiniz ' dir.
- **Modüler/esnek**– varsayılan olarak açık özellikleri sayıda dahil birçok çerçeveleri, küçük ve odaklı, denetim için hangi bileşenlerin belirlemede uygulama geliştiricisinin vererek Katana proje bileşenleri olması gerekir her uygulamada kullanın.
- **Basit ve yüksek performanslı/ölçeklenebilir** – geleneksel bir çerçeve kavramımız küçük bir kümesi olarak bölme odaklanmış bir sonuç Katana uygulama, daha az bilgi işlem kullanabilir uygulama geliştiricisi tarafından açıkça eklenen bileşenleri Kaynaklar ve sonuç olarak, daha fazla yük daha diğer türleri sunucularının ve çerçeveleri ile işler. Uygulamanın gereksinimlerini daha fazla temel alınan altyapı özelliklerinden isteğe bağlı olarak, bunları OWIN ardışık düzenine eklenebilir, ancak, uygulama geliştiricisi yoluna açık bir karar olmalıdır. Ayrıca, alt düzey bileşenlerden substitutability kullanılabilir oldukça, yeni yüksek performanslı sunucular sorunsuz bir şekilde bu uygulamaları bozmadan OWIN uygulamalarının performansını artırmak için sunulma olduğunu anlamına gelir.

## <a name="getting-started-with-katana-components"></a>Katana bileşenleri ile çalışmaya başlama

Bu ilk zaman sunulmuştur, tek bir yönüne [Node.js](http://nodejs.org/) hemen insanların dikkatini u çizdiğini framework olan basitliğini ile yazar ve bir Web sunucusunda çalıştırın. Katana hedefleri light, Çerçeveli, [Node.js](http://nodejs.org/), biri özetlemek bunları Katana birçok avantajları getirir diyerek [Node.js](http://nodejs.org/) (ve bu gibi çerçeveleri) atmak için geliştirici zorlamadan her şeyi Filiz ASP.NET Web uygulamaları geliştirme hakkında bilir. Bu ifade true tutacak Katana projesi ile çalışmaya başlama yapısı eşit basit olmalıdır [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>"Hello World!" oluşturma

JavaScript ve .NET geliştirme arasındaki bir önemli fark, bir derleyici varlığı (veya yokluğu) olmasıdır. Bu nedenle, basit bir Katana sunucusu için başlangıç noktası, bir Visual Studio projedir. Ancak, size en az proje türleri ile başlayabilirsiniz: boş bir ASP.NET Web uygulaması.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Ardından, yüklenir [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) NuGet paketini projeye. Bu paket, ASP.NET isteği işlem hattında çalıştırılan bir OWIN sunucusu sağlar. Üzerinde bulunabilir [NuGet galerisinde](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) ve aşağıdaki komutu kullanarak Visual Studio Paket Yöneticisi iletişim kutusu veya Paket Yöneticisi konsolu kullanılarak yüklenebilir:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Yükleme `Microsoft.Owin.Host.SystemWeb` paket bağımlılıkları olarak bazı ek paketleri yüklenir. Bu bağımlılıkların bir `Microsoft.Owin`, çeşitli yardımcı türleri ve yöntemleri OWIN uygulamalarını geliştirmek için sağlayan bir kitaplık. Aşağıdaki "hello world" sunucu hızla yazılacak türlerine kullanabiliriz.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Bu çok basit bir Web sunucusu artık Visual Studio kullanılarak çalıştırılabilir **F5** komut ve tam hata ayıklama desteği içerir.

## <a name="switching-hosts"></a>Ana bilgisayar geçişi

Varsayılan olarak, IIS bağlamında System.Web kullanan ASP.NET istek ardışık düzeninde önceki "hello world" örnek çalışır. Esneklik ve yönetim özelliklerine sahip bir OWIN Ardışık düzenin composability ve IIS genel olgunluk yararlanmak bize olanak tanıdığından bu tek başına İnanılmaz derecede bir değer ekleyebilirsiniz. Bununla birlikte, burada IIS tarafından sağlanan avantajlar gerekli değildir ve arzusu daha küçük, daha basit ana bilgisayar için durumlar olabilir. Ne, ardından, bizim basit bir Web sunucusu IIS ve System.Web dışında çalıştırmak için gerekli?

Taşınabilirlik hedef göstermek için bir komut satırı barındırmak için Web sunucusu konaktan geçiş yalnızca projenin çıkış klasörüne yeni sunucu ve ana makine bağımlılıkları ekleme ve ardından konağı başlatmadan gerektirir. Bu örnekte, biz Web Sunucumuz adlı Katana konaktaki barındırılacak `OwinHost.exe` ve Katana HttpListener tabanlı sunucu kullanır. Benzer şekilde diğer Katana bileşenler için bunlar aşağıdaki komutu kullanarak Nuget'ten gerekilir:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Komut satırından biz ardından Proje kök klasörüne gezinebilir ve çalıştırmanız yeterlidir `OwinHost.exe` (ilgili NuGet paketi Araçlar klasöründe yüklenen). Varsayılan olarak, `OwinHost.exe` HttpListener tabanlı sunucu aramak için yapılandırılmış ve ek yapılandırma gerekmez. Bir Web tarayıcısına gezinme `http://localhost:5000/` şimdi konsol üzerinden çalışan uygulama gösterir.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Katana mimarisi

 Aşağıda gösterildiği şekilde bir uygulamaya dört mantıksal katmanlara Katana bileşeni mimarisi böler: *konak sunucusu, ara yazılım,* ve *uygulama*. Bileşen mimarisi bu katmanların uygulamaları kolayca, çoğu durumda, yeniden derleme uygulamanın gerek kalmadan kullanılabileceğini şekilde ayrılır.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Ana bilgisayar

 Ana bilgisayar sorumlu olduğu:

- Temel alınan işlem yönetme.
- Bir sunucu seçimi ve hangi istekleri aracılığıyla bir OWIN ardışık oluşumu sonuçlanır iş akışı işlemlerini işleneceğini.

  Şu anda Katana tabanlı uygulamalar için 3 birincil barındırma seçeneği vardır:  
  
**IIS/ASP.NET**: standart HttpModule ve HttpHandler türlerini kullanarak bir ASP.NET istek akışının bir parçası olarak OWIN ardışık düzen IIS üzerinde çalıştırabilirsiniz. Destek barındıran ASP.NET Web uygulaması projesine Microsoft.AspNet.Host.SystemWeb NuGet paketini yükleyerek etkinleştirilir. Ayrıca, IIS bir ana bilgisayar ve bir sunucu görev yapar. çünkü OWIN sunucusu/ana fark SystemWeb konak kullanırken, bir geliştirici alternatif sunucu uygulaması geçemez anlamına gelir. Bu NuGet paketi conflated.  
  
**Özel ana bilgisayar**: Katana bileşen paketi bir geliştirici imkanı sunar kendi özel işleminde uygulamalarını barındırmak için olup olmadığını bir konsol uygulaması, Windows hizmeti, vs. Bu özellik, Web API'si tarafından sağlanan barındırma özellik benzer. Aşağıdaki örnek, Web API kodunda özel bir ana bilgisayar gösterir:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Katana uygulamanın barındırma Kurulum benzer:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Web API ve Katana barındırılan örnekler arasında bir önemli fark, Web API configuration kod barındırma Katana örnekten eksik olmasıdır. Taşınabilirlik hem composability etkinleştirmek için sunucu istek işleme ardışık düzen yapılandırır koddan başlatılır kod Katana ayırır. Web API yapılandırır ve ayrıca WebApplication.Start tür parametresi olarak belirtilen başlangıç sınıfında bulunan kodu.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

Başlangıç sınıfı makalenin ilerleyen bölümlerinde daha ayrıntılı açıklanmıştır. Ancak, barındırma işlemi şu anda barındırılan ASP.NET Web API uygulamalarındaki kullanmakta olduğunuz kod benzerlik görünür bir Katana başlatmak için kod gereklidir.

**OwinHost.exe**: bazı Katana Web uygulamaları çalıştırmak için özel bir işleminiz yazmak istersiniz, ancak çoğu yalnızca bir sunucu başlatabilir ve bunların uygulamayı çalıştırın, bir önceden oluşturulmuş bir yürütülebilir dosyayı başlatmak tercih ettiğiniz. Bu senaryo için Katana bileşen suite içerir `OwinHost.exe`. Bir projenin kök dizin içinde çalıştırın, bu yürütülebilir dosya (varsayılan olarak HttpListener sunucusunu kullanması) bir sunucuyu başlatın ve bulup kullanıcının başlangıç sınıfı çalıştırmak için kuralları kullanın. Daha ayrıntılı bir denetim için ek komut satırı parametreleri yürütülebilir sağlar.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Sunucu

 Konak başlatılıyor ve bunların bakımını işlem içinde uygulama çalıştığında, sunucunun sorumluluğu sorumlu olmakla ağ yuva açın, isteklerini dinleme ve bunları OWIN bileşenleri işlem hattı göndermek için (aynı kullanıcı tarafından belirtilen zaten fark, bu işlem hattı uygulama geliştiricinin başlangıç sınıfında belirtilir). Şu anda iki sunucu uygulamaları Katana proje içerir: 

- **Microsoft.Owin.Host.SystemWeb**: daha önceden belirtildiği gibi ASP.NET ardışık düzenini birlikte IIS bir ana bilgisayar ve bir sunucu görev yapar. Bu nedenle, bu barındırma seçeneği seçildiğinde, IIS hem işlem etkinleştirme gibi ana bilgisayar düzeyindeki konuları yönetir ve HTTP isteklerini dinler. ASP.NET Web uygulamaları için ardından ASP.NET ardışık düzende istekler gönderir. Bir ASP.NET HttpModule ve HttpHandler HTTP ardışık düzen üzerinden akan ve kullanıcı tarafından belirtilen OWIN ardışık düzeni aracılığıyla gönderecek şekilde isteklerin kesilmesi için Katana SystemWeb konağa kaydeder.
- **Microsoft.Owin.Host.HttpListener**: kendi adından da anlaşılacağı gibi bu Katana sunucu yuvası açın ve geliştirici tarafından belirtilen bir OWIN ardışık düzenine istekleri göndermek için .NET Framework'ün HttpListener sınıfını kullanır. Şu anda varsayılan sunucu seçimi Katana barındırma API'si ve OwinHost.exe budur.

## <a name="middlewareframework"></a>Ara yazılım/framework

 Sunucu, istemciden gelen bir istek kabul ettiğinde daha önce belirtildiği gibi bir işlem hattı tarafından geliştiricinin başlatma kodunu belirtilen OWIN bileşenlerinin aracılığıyla geçirmek için sorumludur. Bu işlem hattı bileşenler ara yazılımı bilinir.  
 Bir çok temel düzeyde bir OWIN ara yazılım bileşeni çağrılabilir olması, OWIN uygulama temsilci uygulamak basitçe gerekir.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Ancak, geliştirme ve ara yazılım bileşenleri oluşumunu basitleştirmek için ara yazılım bileşenleri için kuralları ve yardımcı türü tamamen Katana destekler. Bunların en yaygın `OwinMiddleware` sınıfı. Bu sınıf kullanılarak oluşturulan bir özel bir ara yazılım bileşeni aşağıdakine benzer olacaktır: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Bu sınıfın türetildiği `OwinMiddleware`, ardışık düzende sonraki ara yazılım örneği bağımsız değişkenlerinden biri kabul eder ve temel oluşturucu geçtikten sonra bir oluşturucu uygular. Ara yazılım yapılandırmak için kullanılan ek bağımsız değişkenler, ayrıca sonraki ara yazılım parametresi sonra Oluşturucu parametresi olarak bildirilir.   
  
Ara yazılım çalışma zamanında yürütülür geçersiz kılınan aracılığıyla `Invoke` yöntemi. Bu yöntem tek bir bağımsız değişken türü alır `OwinContext`. Bu bağlam nesnesi tarafından sağlanan `Microsoft.Owin` NuGet paketini daha önce açıklanan ve kesin tür belirtilmiş istek, yanıt ve ortam sözlüğü, birkaç ek yardımcı türleri ile birlikte erişim sağlar.   
  
Ara yazılım sınıfı kolayca uygulama başlatma kodunda OWIN ardışık düzenine gibi eklenebilir:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Katana altyapı yalnızca OWIN ara yazılımı bileşenleri bir işlem hattı oluşturur çünkü ve bileşenleri işlem hattında katılmayı uygulama temsilcisi desteklemek yalnızca gerektiğinden, ara yazılım bileşenleri karmaşıklığı basit değişebilir günlükçüler için tüm çerçeveleri gibi ASP.NET ve Web API'sini veya [SignalR](../../../signalr/index.md). Örneğin, ASP.NET Web API önceki OWIN ardışık düzenine eklemek için aşağıdaki başlatma kodunun eklenmesini gerektirir:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Katana altyapı, ara yazılım bileşenlerinin yapılandırma yöntemine Iappbuilder nesneye eklendikleri sırayla göre işlem hattı oluşturacaksınız. Bizim örneğimizde, daha sonra bu istekleri nihai olarak nasıl işleneceğini bağımsız olarak işlem hattı üzerinden akan tüm istekleri LoggerMiddleware başa çıkabilir. Bu, istekleri birden çok bileşenleri ve çerçeveleri (örn: ASP.NET Web API, SignalR ve bir statik dosya sunucusu) içeren işlem hattı için bir ara yazılım bileşeni (örneğin bir kimlik doğrulama bileşeni) burada işleyebilir güçlü senaryolara olanak sağlar.
 
## <a name="applications"></a>Uygulamalar

Önceki örneklerde gösterildiği gibi OWIN ve Katana proje, yeni bir uygulama programlama modeli, ancak uygulama programlama modellerini ve çerçeveleri sunucusuna gelen ve barındırma altyapısını ayırmak için bunun yerine bir Özet olarak düşünülmelidir değil. Örneğin, Web API uygulamaları oluştururken, geliştirici framework olup olmadığını Katana proje bileşenlerini kullanarak bir OWIN ardışık düzenindeki uygulamayı çalıştıran bağımsız olarak ASP.NET Web API çerçevesi, kullanmaya devam eder. OWIN ilgili kod burada uygulama geliştiricisine görünür olacak tek bir yerde nerede Geliştirici OWIN ardışık düzenini ölçeklemesini uygulama başlangıç koduna olacaktır. Başlangıç kod içinde bir dizi UseXx deyim, gelen istekleri işleyen her ara yazılım bileşeni için genellikle bir geliştirici kaydeder. Bu deneyimine geçerli System.Web dünyada HTTP modüller kaydediliyor aynı etkiye sahip olursunuz. Genellikle, bir büyük framework ara yazılım, ASP.NET Web API gibi veya [SignalR](../../../signalr/index.md) ardışık düzen sonunda kaydedilir. Böylece bunlar için tüm çerçeveleri ve daha sonra işlem hattı, kayıtlı bileşenleri isteklerini işleyecek kimlik doğrulaması veya önbelleğe alma için olanlar gibi çapraz kesme ara yazılımı bileşenleri genellikle işlem hattının başlangıç doğrultusunda kaydedilir. Bu ayrımı ara yazılımı bileşenleri birbirinden ve temel alınan altyapı bileşenlerini genel sistem kararlı kalmasını sağlarken farklı hızlarda geliştirilebilen bileşenleri sağlar.

## <a name="components--nuget-packages"></a>Bileşenleri NuGet paketleri

Çok sayıda geçerli kitaplıkları ve çerçeveleri gibi Katana proje bileşenleri NuGet paketlerini bir dizi dağıtılır. Yaklaşan sürüm 2.0, Katana paket bağımlılık grafiği şu şekilde görünür. (Daha büyük bir görünüm için görüntüye tıklayın.)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Katana projedeki neredeyse her paketi, doğrudan veya dolaylı olarak Owın paketine bağlıdır. OWIN belirtiminin 4 bölümde açıklanan uygulama başlatma sırası somut bir uygulamasını sağlar Iappbuilder arabirimi içeren paketin budur hatırlıyor olabilirsiniz. Ayrıca, çok sayıda paketi HTTP isteklerini ve yanıtlarını ile çalışmak için yardımcı türleri kümesi sağlayan Microsoft.Owin bağlıdır. Paket geri kalanında, barındırma altyapısını paketleri (sunucular ve konakları) veya ara yazılımı olarak sınıflandırılabilir. Paketler ve Katana projeye dış bağımlılıklar turuncu renkte görüntülenir.

Hem SystemWeb ve HttpListener tabanlı sunucular, OwinHost paketin OwinHost.exe kullanarak OWIN uygulamalarını çalıştırmak ve OWIN uygulamaları kendi kendine barındırma Microsoft.Owin.Hosting paketin Katana 2.0 barındırma altyapısını içerir bir Özel ana bilgisayar (örn: konsol uygulaması, Windows hizmeti, vb.)

Katana 2.0 için ara yazılımı bileşenleri öncelikle kimlik doğrulama farklı yollardan sağlamaya odaklanmıştır. Bir başlangıç ve hata sayfası için destek sağlayan bir ek ara yazılım bileşeni tanılama için sağlanır. OWIN pratikte barındırma soyutlama büyüdükçe, ara yazılım bileşenleri, hem bu Microsoft ve üçüncü taraflar tarafından geliştirilen ekosistemi numarasını da çıkarılır.

## <a name="conclusion"></a>Sonuç

 Kendi baştan oluşturmak ve böylece başka bir Web çerçevesi öğrenmek için geliştiricilere zorlamak için Katana projenin hedef ayarlanmamış. Bunun yerine, daha önce mümkün olmuştur daha fazla seçenek .NET Web uygulaması geliştiricileri sunmak için bir Özet oluşturmak için hedefi olmuştur. Değiştirilebilir bileşenler kümesi tipik bir Web uygulaması yığına mantıksal katmanları bölme tarafından Katana proje boyunca hangi hızı için bu bileşenlerin anlamlı en iyileştirmek için yığın bileşenleri sağlar. Tüm bileşenler için basit bir OWIN özeti etrafında oluşturarak Katana çerçeveleri ve bunların üstüne oluşturulan uygulamalar çeşitli farklı sunucular ve konaklar arasında taşınabilmesini sağlar. Yığın denetiminde Geliştirici koyarak Katana Geliştirici ultimate seçiminiz nasıl basit hale sağlar veya kendi Web yığını nasıl zengin özelliklere sahip olmalıdır.  
  

## <a name="for-more-information-about-katana"></a>Katana hakkında daha fazla bilgi için

- Github'da Katana projesini: [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Video: [Katana proje - ASP.NET için OWIN](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), Howard Dierking tarafından.

## <a name="acknowledgements"></a>Onayları

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick olduğu için Microsoft Azure ve MVC üzerinde odaklanarak bir üst düzey programlama yazıcı.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
