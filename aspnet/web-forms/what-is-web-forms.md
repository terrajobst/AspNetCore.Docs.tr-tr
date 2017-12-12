---
uid: web-forms/what-is-web-forms
title: Web Forms nedir | Microsoft Docs
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: dcaba60a8640ce83f73460a5c8ee457257b9397e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="what-is-web-forms"></a>Web Forms nedir
====================
ASP.NET Web Forms ASP.NET web uygulama çerçevesi, bir parçasıdır ve içerdiği [Visual Studio](https://www.asp.net/downloads). ASP.NET web uygulamaları oluşturmak için kullanabileceğiniz dört programlama modeli biri, diğer ASP.NET MVC, ASP.NET Web sayfaları ve ASP.NET tek sayfa uygulamaları.

Web Forms, kullanıcılarınızın kendi tarayıcı kullanarak isteği sayfalarıdır. Bu sayfa, HTML, birleşimini kullanarak yazılabilir istemci-komut dosyası, sunucu denetimleri ve sunucu kodu. Kullanıcılar bir sayfa istediğinde derlenir ve sunucu üzerinde çerçevesi tarafından yürütülen ve ardından framework tarayıcı işleyebilen HTML biçimlendirmeleri oluşturur. Bir ASP.NET Web Forms sayfası kullanıcı herhangi bir tarayıcı veya istemci aygıt için bilgileri gösterir.

Visual Studio kullanarak, ASP.NET Web Forms oluşturabilirsiniz. Visual Studio tümleşik geliştirme ortamı (IDE), Web Forms sayfası düzenlemek için sunucu denetimleri sürükleyip olanak sağlar. Daha sonra kolayca özellikleri, yöntemleri ve olayları sayfa çıktısını veya sayfadaki denetimleri için ayarlayabilirsiniz. Bu özellikler, yöntemleri ve olayları web sayfasının davranışı, Görünüm ve benzeri tanımlamak için kullanılır. Sayfa için mantığı işlemek için sunucu kodu yazmak için Visual Basic veya C# gibi .NET dil kullanabilirsiniz.

> [!NOTE] 
> 
> ASP.NET ve Visual Studio belgeleri birkaç sürümleri yayar. Önceki sürümlerden özellikleri vurgulayın konuları geçerli görevler ve en son sürümlerini kullanan senaryolar için yararlı olabilir.


**ASP.NET Web Forms şunlardır:** 

- Sunucu üzerinde dinamik olarak çalışan bir kod tarayıcı veya istemci aygıt Web sayfası çıkışı oluşturur, Microsoft ASP.NET teknolojisi temel.
- Herhangi bir tarayıcı veya mobil aygıt ile uyumludur. Bir ASP.NET Web sayfası doğru tarayıcı uyumlu HTML stil, Düzen ve benzeri gibi özellikleri için otomatik olarak oluşturur.
- .NET ortak dil çalışma zamanı, Microsoft Visual Basic ve Microsoft Visual C# gibi tarafından desteklenen herhangi bir dil ile uyumludur.
- Microsoft .NET Framework üzerine inşa edilmiştir. Bu Yönetilen ortamlarda, tür güvenliği ve devralma dahil olmak üzere framework'ün tüm avantaj sağlar.
- Esnek ekleyebileceğiniz çünkü kullanıcı tarafından oluşturulan ve üçüncü taraf onlara denetler.

**ASP.NET Web Forms sunar:** 

- Uygulama mantığının den HTML ve başka bir kullanıcı Arabirimi kodu ayrılması.
- Sunucu denetimleri veri erişimi de dahil olmak üzere, ortak görevler için zengin bir dizisi.
- Güçlü veri bağlama, iyi bir araç desteği.
- Tarayıcıda yürüten istemci tarafı komut dosyası için destek.
- Çeşitli yönlendirme, güvenlik, performans, uluslararası duruma getirme, test, hata ayıklama, hata işleme ve durum yönetimi gibi diğer özellikler için destek.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.NET Web Forms güçlüklerini yardımcı olur

Web uygulaması programlama genellikle geleneksel istemci tabanlı uygulamalar programlama olduğunda ortaya değil zorluklar gösterir. Arasında görevleridir:

- **Zengin bir Web kullanıcı arabirimini uygulayan** - zor olabilir ve tasarlamasını ve uygulamasını yorucu bir kullanıcı arabirimi özellikle sayfanın dinamik içerik, büyük bir miktarını karmaşık bir düzen varsa temel HTML tesis kullanılarak ve tam özellikli Kullanıcı etkileşimli nesneleri.
- **İstemci ve sunucu ayrımı** -içinde bir Web uygulaması, istemci (tarayıcı) ve sunucu programlardır genellikle farklı bilgisayarlarda (ve hatta farklı işletim sistemleri) çalıştıran farklı. Sonuç olarak, uygulamayı iki yarısının çok az bilgi paylaşımı; Bunlar iletişim ancak genelde yalnızca küçük parçalara basit bilgi değişimi.
- **Durum bilgisiz yürütme** - bir Web sunucusu bir sayfa için bir istek aldığında, sayfa bulur, işler, tarayıcıya gönderir ve tüm sayfa bilgilerini atar. Kullanıcı aynı sayfa yeniden isterse, sunucunun sayfanın en baştan yeniden işlemeyerek tüm dizisi tekrarlar. Başka bir deyişle, bir sunucu işlenmemiş olan sayfaların belleğe sahip değil — sayfa durum bilgisiz. Bu nedenle, bir uygulama bir sayfa hakkında bilgiler tutması gerekiyorsa, durum bilgisiz doğası bir sorun olabilir.
- **Bilinmeyen istemci yeteneklerini** -çoğu durumda, Web uygulamaları için çok sayıda kullanıcı farklı tarayıcılar kullanarak erişilebilir. Tarayıcılar çalışacak bir uygulama oluşturmak zorlaşır farklı özelliklere sahip eşit hepsinde yanı sıra.
- **Veri erişimi olan zorluklar** -veri okuma ve yazma geleneksel Web uygulamalarında veri kaynağına karmaşık ve yoğun bir kaynak olabilir.
- **Ölçeklenebilirlik ile zorluklar** - varolan yöntemleriyle tasarlanmış Web uygulamaları başarısız uyumluluk uygulamasının çeşitli bileşenler arasındaki eksikliği nedeniyle ölçeklenebilirlik amaçlarını karşılamak için birçok durumda. Bu genellikle yoğun büyüme döngüsü altında uygulamaları için ortak bir hata noktası olur.

Web uygulamaları için bu zorlukların aşılması önemli ölçüde zaman ve çaba gerektirebilir. ASP.NET Web formları ve ASP.NET framework bu aşağıdaki yollarla güçlükleri:

- **Sezgisel, tutarlı nesne modeli** -ASP.NET sayfa çerçevesi formlarınızı ayrı istemci ve sunucu parçaları olarak değil bir birim olarak düşünmek sağlayan nesne modeli sunar. Bu modelde, sayfa öğelerini özelliklerini ayarlamak ve olaylara yanıt özelliği de dahil olmak üzere, geleneksel Web uygulamalarını daha sezgisel bir yolla sayfa bildirebilirsiniz. Ayrıca, ASP.NET sunucu denetimleri HTML sayfası fiziksel içeriğini ve tarayıcı ve sunucu arasında doğrudan etkileşim için bir Özet bağlıdır. Genel olarak, bir istemci uygulaması denetimlerinde çalışmak ve sunmak ve denetimleri ve içerikleri işlemek için HTML oluşturma hakkında düşünmek olmaması şekilde sunucu denetimleri kullanabilirsiniz.
- **Olay kaynaklı programlama modeli** -ASP.NET Web Forms Web uygulamaları için istemci veya sunucu üzerinde meydana gelen olayları için olay işleyicileri yazma bilinen modelini getirin. ASP.NET sayfa çerçevesi, istemci üzerinde bir olay yakalama, sunucuya iletmeden ve uygun yöntemi çağırma temelindeki mekanizması tüm otomatik ve sizin için görünmez olduğunu şekilde bu modeli soyutlar. Olay kaynaklı geliştirme destekleyen bir açık, kolayca yazılan kod yapısı sonucudur.
- **Sezgisel durum yönetimi** - ASP.NET sayfa çerçevesi sayfanız ve denetimlerinin durumunu sürdürme görevini otomatik olarak yönetir ve uygulamaya özgü bilgileri durumunu korumak için açık yollar sağlar. Bu sunucu kaynaklarını aşırı kullanmadan gerçekleştirilir ve tarayıcı tanımlama bilgilerini göndermeden veya ile uygulanabilir.
- **Tarayıcı bağımsız uygulamalar** -ASP.NET sayfa çerçevesi tarayıcılarda farklar için açıkça kod gereğini ortadan sunucudaki tüm uygulama mantığını oluşturmanıza olanak sağlar. Ancak, yine performansı ve daha zengin istemci deneyimi sağlamak için istemci tarafı kodlar yazarak tarayıcıya özgü özelliklerden yararlanmak sağlar.
- **.NET framework ortak dil çalışma zamanı desteği** -ASP.NET sayfa çerçevesi tüm framework tüm ASP.NET uygulamaları için kullanılabilir olacak şekilde .NET Framework üzerine inşa. Uygulamalarınızı, çalışma zamanı ile uyumlu herhangi bir dilde yazılabilir. Ayrıca, veri erişimi ADO.NET dahil olan .NET Framework tarafından sağlanan veri erişim altyapısını kullanarak basitleştirilmiştir.
- **.NET framework ölçeklenebilir sunucu performansını** -ASP.NET sayfa çerçevesi düzgün bir şekilde ve uygulamanın karmaşık değişiklikleri olmadan Web uygulamanıza tek işlemcili bir bilgisayardan birden çok bilgisayar Web grubu ölçeklendirmenizi sağlar mantığı.

## <a name="features-of-aspnet-web-forms"></a>ASP.NET Web Forms özellikleri

- **Sunucu denetimleri**-ASP.NET Web sunucusu denetimleri olan nesneler sayfa istendiğinde çalıştırılan ASP.NET Web sayfaları ve tarayıcı için bu işleme biçimlendirme. Birçok Web sunucusu denetimleri, düğmeler ve metin kutuları gibi bilinen HTML öğeleri benzerdir. Diğer denetimlerin bir takvim gibi karmaşık davranışı kapsayan denetimleri ve veri kaynaklarına bağlanmak ve verileri görüntülemek için kullanabileceğiniz.
- **Ana sayfalar**-ASP.NET ana sayfalar sayfalar için tutarlı bir yerleşim, uygulamanızda oluşturmak izin verir. Tek bir ana sayfa Görünüm ve tüm sayfaları (veya bir grup sayfa) istediğiniz standart davranışı uygulamanızda tanımlar. Daha sonra görüntülemek istediğiniz içeriği içeren tek tek içerik sayfaları oluşturabilirsiniz. Kullanıcıların içerik sayfaları istediğinde, içerik sayfasından içeriği ana sayfa düzeni birleştirir bir çıktı oluşturmak için ana sayfa ile birleştirin.
- **Verilerle çalışma**-ASP.NET, depolama, alma ve verileri görüntülemek için birçok seçenek sağlar. Bir ASP.NET Web Forms uygulamasında sunu veya web sayfası kullanıcı Arabirimi öğeleri tablolar ve metin kutusu ve aşağı açılır listeler gibi veri giriş otomatikleştirmek için veri bağlama denetimleri kullanın.
- **Üyelik**-ASP.NET Identity kullanıcılarınızın kimlik bilgilerini uygulama tarafından oluşturulan bir veritabanında depolar. Kullanıcılarınız oturum açtığında uygulama veritabanı okuyarak kimlik bilgilerini doğrular. Projenizin *hesap* klasörü üyelik çeşitli kısımlarını uygulamak dosyaları içerir: kaydetme, oturum açma, parola değiştirme ve erişim yetkisi verme. Ayrıca, ASP.NET Web Forms, OAuth ve Openıd destekler. Bu kimlik doğrulaması geliştirmeleri kullanıcıların, sitenizi Facebook, Twitter, Windows Live ve Google olarak böyle hesaplarından varolan kimlik bilgilerini kullanarak oturum açın izin verin. Varsayılan olarak, SQL Server Express LocalDB, Visual Studio Express 2013 Web birlikte geliştirme veritabanı sunucusu örneği üzerinde bir varsayılan veritabanı adını kullanarak bir üyelik veritabanı şablonu oluşturur.
- **İstemci komut dosyası ve istemci çerçeveleri**-ASP.NET Web formu sayfalarında istemci-komut dosyası işlevleri dahil olmak üzere ASP.NET sunucu tabanlı özelliklerini geliştirebilirsiniz. Daha zengin, daha iyi yanıt kullanıcı arabirimi kullanıcılara sağlamak için istemci komut dosyası kullanabilirsiniz. İstemci komut dosyası, sayfa tarayıcıda çalışırken Web sunucusu zaman uyumsuz çağrı yapmak için de kullanabilirsiniz.
- **Yönlendirme**-URL yönlendirme kabul etmek için bir uygulama yapılandırmanıza olanak tanır fiziksel dosyalarına eşleme değil URL'leri isteyin. Yalnızca web sitenizde bir sayfayı bulmak için tarayıcıda kullanıcının girdiği URL isteği URL'dir. Kullanıcılara anlamsal olarak anlamlı ve arama motoru iyileştirmesi (SEO) yardımcı olabilecek URL'leri tanımlamak için yönlendirme kullanın.
- **Durum Yönetimi**-ASP.NET Web Forms içeren birkaç yardımcı olan seçeneklerdir sayfa başına ve uygulama genelinde temel verileri korumak.
- **Güvenlik**-daha güvenli bir uygulama geliştirme, önemli bir bölümünü tehditleri anlamaktır. Microsoft tehditleri kategorilere ayırmak için bir yol geliştirilen: yanıltma, oynama, geri çevirme, bilgilerin açıklanması, hizmet, ayrıcalıkların (STRIDE) reddi. ASP.NET Web Forms içinde genişletilebilirlik noktaları ve ASP.NET Web Forms çeşitli güvenlik davranışlarının özelleştirmenize olanak sağlayan yapılandırma seçenekleri ekleyebilirsiniz.
- **Performans**-başarılı Web sitesi ya da proje performans önemli bir etken olabilir. ASP.NET Web Forms performans ile ilgili sayfa ve sunucu denetim işleme, durum yönetimi, veri erişimi, uygulama yapılandırma ve yükleme ve verimli yöntemler kodlama değiştirmenize olanak sağlar.
- **Uluslararası duruma getirme**- ASP.NET Web Forms edinebilirsiniz web sayfaları içerik oluşturmanıza olanak sağlar ve diğer verileri tarayıcı için tercih edilen dil ayarı dayalı veya kullanıcının açık dilinin seçimine bağlı. İçerik ve diğer veri kaynakları olarak adlandırılır ve bu tür veriler kaynak dosyaları veya diğer kaynakları depolanabilir. Bir ASP.NET Web Forms sayfası denetimlerini kaynaklardan özellik değerlerini almak için yapılandırın. Çalışma zamanında kaynak ifadeleri uygun yerelleştirilmiş kaynak dosyasını kaynaklardan değiştirilir.
- **Hata ayıklama ve hata işleme**-ASP.NET Web Forms uygulamanızda doğabilecek sorunları tanılamanıza yardımcı olmak için özellikleri içerir. Uygulamalarınızı derlemek ve etkili bir şekilde çalıştırmak için hata ayıklama ve hata işleme iyi ASP.NET Web Forms içinde desteklenir.
- **Dağıtım ve barındırma**-Visual Studio, ASP.NET, Azure ve IIS dağıtma ve Web Forms uygulamanızı barındıran işlemine yardımcı olan araçlar sağlar.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Web Forms uygulaması oluşturmak karar verme

Bir Web uygulaması ya da kullanarak ASP.NET Web uygulamanız için model veya ASP.NET MVC çerçevesi gibi başka bir model Forms olup olmadığını dikkatlice dikkate almanız gerekir. MVC çerçevesi, Web Forms modelini değiştirmez; Web uygulamaları için ikisinden birini kullanabilirsiniz. Web Forms modeli veya MVC çerçevesi için belirli bir Web sitesini kullanmaya karar vermeden önce her iki yaklaşımın avantajları tartmanız.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Web Forms tabanlı Web uygulamasının avantajları

Web Forms tabanlı çerçeve aşağıdaki avantajları sağlar:

- İş Web uygulaması geliştirmeleri için faydalı HTTP üzerinden durumu koruyan olay modelini destekler. Web Forms tabanlı uygulama onlarca, yüzlerce sunucu denetiminde içinde desteklenen olayların sağlar.
- Belirli sayfalara işlevsellik ekleyen Page Controller örüntüsünü kullanır. Daha fazla bilgi için bkz: [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") MSDN Web sitesinde.
- Görünüm durumu veya durum bilgilerinin kolaylaştırabilir sunucu tabanlı formlar kullanır.
- Web geliştiricileri ve hızlı uygulama geliştirme için kullanılabilen bileşeni çok sayıda yararlanmak isteyen tasarımcıları küçük ekipleri için iyi çalışır.
- Genel olarak, uygulama geliştirme için en az karmaşık olduğundan bileşenleri ( **sayfa** sınıfı, denetimleri vb.) sıkı olarak tümleştirildiği ve MVC modeline göre daha az kod genellikle gerektirir.

### <a name="advantages-of-an-mvc-based-web-application"></a>MVC tabanlı Web uygulamasının avantajları

ASP.NET MVC çerçevesi aşağıdaki avantajları sağlar:

- Bu, uygulama modeli, görünüme ve denetleyiciye bölerek karmaşıklık yönetmenizi kolaylaştırır.
- Görünüm durumu veya sunucu tabanlı formlar kullanmaz. Bu MVC çerçevesi bir uygulamanın davranışı üzerinde tam denetime isteyen geliştiriciler için ideal hale getirir.
- Tek bir denetleyici üzerinden Web uygulama isteklerini işleyen bir Front Controller örüntüsü kullanır. Bu zengin yönlendirme altyapısını destekleyen bir uygulama tasarlamanızı sağlar. Daha fazla bilgi için bkz: [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") MSDN Web sitesinde.
- Teste dayalı geliştirme (TDD) için daha iyi destek sağlar.
- Geliştiriciler ve uygulama davranışı üzerinde denetime yüksek derecede gereksinim duyan Web tasarımcıları büyük ekipleri tarafından desteklenen Web uygulamaları için çalışır.
