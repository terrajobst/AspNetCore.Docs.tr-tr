---
uid: web-forms/what-is-web-forms
title: Web Forms nedir | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: e135cfa2945b9e7e5269eb436ff0c1dff20aacdf
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055718"
---
<a name="what-is-web-forms"></a>Web Forms nedir
====================
ASP.NET Web Forms, ASP.NET web uygulama çerçevesi bir parçasıdır ve içerdiği [Visual Studio](https://www.asp.net/downloads). ASP.NET web uygulamaları oluşturmak için kullanabileceğiniz dört programlama modelinden biridir, diğerleri ASP.NET MVC, ASP.NET Web sayfaları ve ASP.NET tek sayfalık uygulamalar.

Web Forms, kullanıcılarınızın tarayıcıları kullanarak istek sayfalarıdır. Bu sayfalar, HTML, bir birleşimi kullanılarak yazılabilir istemci-komut dosyası, sunucu denetimleri ve sunucu kodu. Kullanıcılar bir sayfa istediğinde, derlenmiş ve framework tarafından sunucu üzerinde yürütülen ve ardından tarayıcı işleyebilen HTML biçimlendirmesi framework oluşturur. Bir ASP.NET Web Forms sayfası, herhangi bir tarayıcıyı veya istemci cihaz kullanıcıya bilgi sunar.

Visual Studio kullanarak ASP.NET Web formları oluşturabilirsiniz. Visual Studio tümleşik geliştirme ortamı (IDE), Web Forms sayfanızı düzenlemek için sunucu denetimleri sürükleyip olanak tanır. Daha sonra kolayca özellikleri, yöntemleri ve olayları veya sayfa için sayfadaki denetimleri ayarlayabilirsiniz. Bu özellikleri, yöntemleri ve olayları, web sayfanızın davranışı, Görünüm ve benzeri tanımlamak için kullanılır. Sayfa için mantığı işlemek için sunucu kodu yazmak için bir .NET dil Visual Basic veya C# gibi kullanabilirsiniz.

> [!NOTE] 
> 
> ASP.NET ve Visual Studio belgeleri çeşitli sürümlerinden yayılır. Önceki sürümlerden özellikleri vurgulayan konuları geçerli görevler ve en son sürümleri kullanan senaryolar için yararlı olabilir.


**ASP.NET Web Forms şunlardır:** 

- Sunucu üzerinde dinamik olarak çalışan kodu tarayıcı veya istemci cihaza Web sayfası çıkış oluşturduğu Microsoft ASP.NET teknolojisini temel alır.
- Herhangi bir tarayıcı veya mobil cihaz uyumludur. Bir ASP.NET Web sayfası doğru tarayıcı uyumlu HTML stillerini, Düzen ve benzeri gibi özellikleri için otomatik olarak işler.
- .NET ortak dil çalışma zamanı, Microsoft Visual Basic ve Microsoft Visual C# gibi tarafından desteklenen herhangi bir dil ile uyumludur.
- Microsoft .NET Framework üzerine inşa edilmiş. Bu, bir yönetilen ortamı, tür güvenliği ve devralma gibi framework, tüm avantajlarını sağlar.
- Kullanıcı tarafından oluşturulmuş ekleyebilir ve bunları için üçüncü taraf denetimleri için esnek.

**ASP.NET Web Forms sunar:** 

- Uygulama mantığı gelen HTML ve diğer UI kodunu ayrılması.
- Zengin bir veri erişim dahil olmak üzere, ortak görevler için sunucu denetimleri paketi.
- Güçlü veri bağlama, harika bir araç desteği.
- Tarayıcıda yürüten istemci tarafı betik oluşturma desteği.
- Çeşitli yönlendirme, güvenlik, performans, uluslararası duruma getirme, test etme, hata ayıklama, hata işleme ve durum yönetimi gibi diğer özellikleri için destek.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.NET Web Forms zorlukların yardımcı olur

Web uygulaması programlama genellikle geleneksel istemci tabanlı uygulamaları programlama ortaya çıkan değil zorluklar teşkil etmektedir. Arasında sorunlardır:

- **Bir zengin Web kullanıcı arabirimini uygulayan** - zor olabilir ve tasarlayıp uygulamasına yorucu bir kullanıcı arabirimi temel HTML özellikleri, özellikle karmaşık bir düzen, dinamik içeriğin büyük bir miktarını sayfa varsa, kullanma ve tam özellikli Etkileşimli kullanıcı nesneleri.
- **İstemci ve sunucu ayrımı** -içinde bir Web uygulaması, istemci (tarayıcı) ve sunucu programlardır çalıştırılmasında çoğunlukla farklı bilgisayarlarda (ve hatta farklı işletim sistemlerinde) farklı. Sonuç olarak, uygulamanın iki yarısı çok az bilgi paylaşımı; Bunlar iletişim ancak genellikle yalnızca küçük öbekler halinde basit bilgi alışverişi.
- **Durum bilgisi olmayan yürütme** - bir Web sunucusu bir sayfa için bir istek aldığında, sayfanın bulur, işler, tarayıcıya gönderir ve sonra tüm sayfa bilgileri atar. Kullanıcı aynı sayfayı yeniden isterse, sunucunun sayfanın sıfırdan yeniden işlemeyerek dizinin tamamıyla tekrarlar. Başka bir deyişle, bir sunucu işlenmemiş olan sayfaların belleğe sahip değil; sayfa durum bilgisi olmayan. Bu nedenle, bir uygulama bir sayfa hakkında bilgileri tutması gerekiyorsa, durum bilgisi olmayan doğası bir sorun olabilir.
- **Bilinmeyen istemci yeteneklerini** -çoğu durumda, Web uygulamaları birçok kullanıcılara farklı tarayıcılar kullanarak erişilebilir. Tarayıcılar çalıştırılacak bir uygulama oluşturmak zorlaştıran farklı özelliklere sahip tüm bunların yanı sıra eşit.
- **Veri erişimi ile zorluklar** -okuma ve yazma veri kaynağı geleneksel Web uygulamaları için karmaşık ve kaynak kullanımı yoğun olabilir.
- **Ölçeklenebilirlik ile zorluklar** - Web uygulamaları mevcut yöntemleriyle tasarlanmış başarısız çeşitli uygulama bileşenleri arasındaki uyumluluk olmaması nedeniyle ölçeklenebilirlik amaçlarını karşılamak için çoğu durumda. Ağır büyüme döngüsü altında uygulamaları için ortak bir hata noktası genellikle budur.

Web uygulamaları için bu zorlukların aşılması, önemli miktarda zaman ve çaba gerektirebilir. Bu zorluklar, ASP.NET Web Forms ve ASP.NET framework aşağıdaki yollarla ele:

- **Sezgisel, tutarlı bir nesne modeli** -ASP.NET sayfası framework formlarınızı ayrı istemci ve sunucu parçaları olarak değil bir birim olarak düşünün olanak sağlayan bir nesne modeli sunar. Bu modelde, geleneksel Web uygulamaları, sayfa öğeleri için özellikleri ayarlamak ve olaylarına yanıt verme olanağı dahil olmak üzere daha sezgisel bir şekilde sayfa programlayabilirsiniz. Ayrıca, ASP.NET sunucu denetimleri fiziksel bir HTML sayfası içeriğini ve tarayıcı ve sunucu arasında doğrudan etkileşim bir Özet ' dir. Genel olarak, bir istemci uygulaması denetimlerinde çalışmak ve sunmak ve denetimleri ve bunların içeriğini işlemek için HTML oluşturmak nasıl depolayacağınız üzerine düşünmeniz gerekmez şekilde sunucu denetimleri kullanabilirsiniz.
- **Olay kaynaklı programlama modeli** -ASP.NET Web Forms Web uygulamaları için istemci veya sunucu üzerinde meydana gelen olayları için olay işleyicileri yazma bilinen modelini getirin. ASP.NET sayfası framework bu modeli temel mekanizması istemcide bir olay yakalama, sunucuya aktarmadan ve uygun yöntemi çağırma tüm otomatik ve sizin için görünmez olduğunu şekilde soyutlar. Olay temelli geliştirme destekleyen bir temiz, kolay yazılmış kod yapısı sonucudur.
- **Sezgisel durum yönetimi** - ASP.NET sayfası framework sayfanız ve denetimlerinin durumunu bakım görevini otomatik olarak işler ve uygulamaya özgü bilgilerin durumunu korumak üzere açık yol sağlar. Bu, sunucu kaynaklarını aşırı kullanmadan gerçekleştirilir ve ile veya tarayıcı tanımlama bilgileri göndermeden uygulanabilir.
- **Tarayıcı bağımsız uygulamalar** -ASP.NET sayfası framework tarayıcılar farklılıkları açıkça kod gereğini ortadan kaldırır sunucusunda, tüm uygulama mantığı oluşturmanıza olanak sağlar. Ancak, yine de Gelişmiş performans ve daha zengin bir istemci deneyimini sağlamak için istemci tarafı kod yazarak tarayıcı özgü özelliklerinden yararlanmanıza olanak tanır.
- **.NET framework ortak dil çalışma zamanı desteği** -tüm framework herhangi bir ASP.NET uygulamasında kullanılabilir olması, ASP.NET sayfası framework .NET Framework üzerinde oluşturulur. Uygulamalarınızı, çalışma zamanı ile uyumlu herhangi bir dilde yazılabilir. Ayrıca, veri erişimi ADO.NET dahil olan .NET Framework tarafından sağlanan veri erişim altyapısını kullanarak basitleştirilmiştir.
- **.NET framework ölçeklenebilir sunucu performansını** -ASP.NET sayfası framework temiz ve uygulamanın karmaşık değişiklik yapmadan Web uygulamanızı çoklu bilgisayar Web grubu için tek bir işlemciye sahip bir bilgisayar ölçeklendirmenize olanak sağlar mantığı.

## <a name="features-of-aspnet-web-forms"></a>ASP.NET Web Forms özellikleri

- **Sunucu denetimleri**-ASP.NET Web sunucusu denetimleri olan sayfa istendiğinde çalıştırılan ASP.NET Web sayfaları ve bu işleme biçimlendirme tarayıcıya nesneler. Birçok Web sunucusu denetimleri için düğme ve metin kutuları gibi tanıdık HTML öğeleri benzerdir. Karmaşık bir davranış, bir takvim gibi diğer denetimleri kapsar denetimleri ve veri kaynaklarına bağlanmak ve verileri görüntülemek için kullanabileceğiniz.
- **Ana sayfalar**-ASP.NET ana sayfaları, uygulamanızda sayfalar için tutarlı bir düzen oluşturmak izin verin. Tek bir ana sayfa uygulamanızda görünüme ve tüm sayfaları (veya sayfaları bir grup) için istediğiniz standart davranışını tanımlar. Ardından görüntülemek istediğiniz içeriği içeren tek içerik sayfaları oluşturabilirsiniz. Kullanıcıların içerik sayfalarını istediğinde, içerik içerik sayfasından ana sayfa düzenini birleştirir çıktı oluşturmak için ana sayfa ile birleştirin.
- **Verilerle çalışma**-ASP.NET verileri görüntüleme depolamak ve almak için birçok seçenek sunar. Bir ASP.NET Web Forms uygulaması'nda, sunum ya da giriş veri tabloları ve metin kutusu ve aşağı açılan listeler gibi web sayfası kullanıcı Arabirimi öğelerinde otomatik hale getirmek için verilere bağlı denetimleri kullanın.
- **Üyelik**-ASP.NET Identity kullanıcılarınızın kimlik bilgileri uygulama tarafından oluşturulan bir veritabanında depolar. Kullanıcılarınız oturum açtığında, uygulama veritabanını okuyarak, kimlik bilgilerini doğrular. Projenizin *hesabı* klasörü üyelik çeşitli bölümlerini uygulama dosyalarını içerir: kaydetme, oturum açma, parola değiştirme ve erişimi yetkilendirme. Ayrıca, ASP.NET Web Forms, OAuth ve Openıd destekler. Bu kimlik doğrulama geliştirmeler, sitenizi Facebook, Twitter, Windows Live ve Google gibi hesaplarından var olan kimlik bilgilerini kullanarak oturum açmasına imkan tanıyın. Varsayılan olarak, şablon, varsayılan veritabanı adı bir SQL Server Express LocalDB, Visual Studio Express 2013 Web ile birlikte gelen geliştirme veritabanı sunucusunu örneğini kullanarak bir üyelik veritabanı oluşturur.
- **İstemci betiğini ve istemci çerçevelerini**-ASP.NET Web formu sayfalarında istemci betiği işlevleri dahil olmak üzere ASP.NET sunucu tabanlı özellikleri geliştirebilirsiniz. Daha zengin ve daha duyarlı kullanıcı arabirimi kullanıcılara sunmak için istemci komut dosyası kullanabilirsiniz. İstemci betiği, bir sayfa tarayıcıya çalışırken, Web sunucusu zaman uyumsuz çağrı yapmak için de kullanabilirsiniz.
- **Yönlendirme**-URL yönlendirme kabul etmek için bir uygulama yapılandırmanıza olanak tanır fiziksel dosyaları eşlemeyin URL'leri isteyin. Yalnızca web sitenizde bir sayfayı bulmak için kendi tarayıcıya kullanıcının girdiği URL bir istek URL'sidir. Anlamsal olarak kullanıcılar için anlamlı olan ve arama motoru iyileştirmesi (SEO) yardımcı olan URL'ler tanımlamak için yönlendirme kullanır.
- **Durum Yönetimi**-ASP.NET Web Forms içeren birkaç yardımcı olan seçeneklerdir hem sayfa başına hem de birçok farklı uygulama temeli veri koruma.
- **Güvenlik**-tehditleri anlamak için daha güvenli bir uygulama geliştirmenin önemli bir bölümünü oluşturur. Microsoft tehditleri sınıflandırmak için bir yol geliştirilen: yanıltma, kurcalama, ret, bilgilerin açıklanması, hizmet, ayrıcalıkların yükseltilmesine (STRIDE) engelleme. ASP.NET Web formları içindeki genişletilebilirlik noktaları ve ASP.NET Web Forms çeşitli güvenlik davranışları özelleştirmenize olanak sağlayan yapılandırma seçenekleri ekleyebilirsiniz.
- **Performans**-bir Web sitesi başarılı ya da proje performans önemli bir etken olabilir. ASP.NET Web Forms sayfası ve sunucu denetim işleme, durum yönetimi, veri erişimi, uygulama yapılandırma ve yükleme için ilgili ve verimli kodlama uygulamaları performans değiştirmenize olanak sağlar.
- **Uluslararası duruma getirme**- ASP.NET Web Forms web sayfalarını edinebilirsiniz içerik oluşturmanıza olanak sağlar ve diğer verileri tarayıcı için tercih edilen dil ayarına göre veya kullanıcının açık dilinin seçimine bağlı. İçerik ve diğer veri kaynakları olarak adlandırılır ve bu tür veriler kaynak dosyaları veya diğer kaynakları depolanabilir. Bir ASP.NET Web formları sayfasında kaynaklardan özellik değerlerini almak için denetimleri yapılandırın. Çalışma zamanında, kaynak ifadeleri uygun yerelleştirilmiş kaynak dosyasından kaynaklar tarafından değiştirilir.
- **Hata ayıklama ve hata işleme**-ASP.NET Web Forms uygulaması'nda doğabilecek sorunları tanılamanıza yardımcı olacak özellikler içerir. Uygulamalarınızı derleyin ve etkili bir şekilde çalıştırın böylece hata ayıklama ve hata işleme de ASP.NET Web formları içinde desteklenir.
- **Dağıtım ve barındırma**-Visual Studio, ASP.NET, Azure ve IIS Web Forms uygulamanızı barındırma ve dağıtma işlemi ile yardımcı olan araçlar sağlar.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Bir Web Forms uygulaması oluşturmak ne zaman karar verme

Bir Web uygulaması ya da kullanarak ASP.NET Web uygulamanız için model veya ASP.NET MVC çerçevesi gibi başka bir model Forms olup olmadığını dikkatlice dikkate almanız gerekir. MVC çerçevesi, Web Forms modelini değiştirmez; Web uygulamaları için ikisinden birini kullanabilirsiniz. Web Forms modeli veya MVC çerçevesi için belirli bir Web sitesini kullanmaya karar vermeden önce her iki yaklaşımın avantajlarını masaya yatırın.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Web Forms tabanlı Web uygulamasının avantajları

Web Forms tabanlı çerçeve aşağıdaki avantajları sunar:

- Bu satır iş kolu Web uygulaması geliştirmeleri için faydalı HTTP üzerinden durumu koruyan olay modelini destekler. Web Forms tabanlı uygulama onlarca, yüzlerce sunucu denetiminde içinde desteklenen olayları sağlar.
- Belirli sayfalara işlevsellik ekleyen Page Controller örüntüsünü kullanır. Daha fazla bilgi için [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") MSDN Web sitesinde.
- Görünüm durumu veya durum bilgilerinin kolaylaştırabilir sunucu tabanlı formlarda kullanır.
- Web geliştiricileri ve tasarımcıları bileşenleri hızlı uygulama geliştirme için kullanılabilir çok sayıda yararlanmak isteyen küçük takımlar için iyi çalışır.
- Genel olarak, uygulama geliştirme için daha az karmaşık olduğundan bileşenleri ( **sayfa** sınıfı, denetimleri vb.) sıkı olarak tümleştirildiği ve MVC modeline göre daha az kod genellikle gerektirir.

### <a name="advantages-of-an-mvc-based-web-application"></a>Bir MVC tabanlı Web uygulamasının avantajları

ASP.NET MVC çerçevesi aşağıdaki avantajları sunar:

- Bu karmaşıklığı, model, Görünüm ve denetleyici içinde uygulama bölerek Karışıklığın yönetilmesini kolaylaştırır.
- Görünüm durumu veya sunucu tabanlı formlar kullanmaz. Bu MVC çerçevesi bir uygulamanın davranışı üzerinde tam denetim isteyen geliştiriciler için ideal hale getirir.
- Tek bir denetleyici üzerinden Web uygulama isteklerini işleyen bir Front Controller örüntüsü kullanır. Bu, zengin yönlendirme altyapısını destekleyen bir uygulama tasarlamanızı sağlar. Daha fazla bilgi için [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") MSDN Web sitesinde.
- Teste dayalı geliştirme (TDD) için daha iyi destek sağlar.
- Geliştiriciler ve uygulama davranışı üzerinde denetim yüksek derecede ihtiyaç duyan Web tasarımcıları büyük takımlar tarafından desteklenen Web uygulamaları için çalışır.
