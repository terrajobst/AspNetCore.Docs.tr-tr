---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: Geliştirme ve (C#) üretim arasındaki yaygın yapılandırma farklılıkları | Microsoft Docs
author: rick-anderson
description: Önceki öğreticilerde, tüm ilgili dosyalar geliştirme ortamından üretim ortamına kopyalamak size sitemizin dağıtıldı. Ancak, ben...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 47060b45cad0e2f15a3c314d608b88c7c1d30809
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752792"
---
<a name="common-configuration-differences-between-development-and-production-c"></a>Geliştirme ve (C#) üretim arasındaki yaygın yapılandırma farklılıkları
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF'yi indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> Önceki öğreticilerde, tüm ilgili dosyalar geliştirme ortamından üretim ortamına kopyalamak size sitemizin dağıtıldı. Bununla birlikte, burada her ortama sahip benzersiz bir Web.config dosyası, BIOS'ta ortamlar arasında farklar yapılandırma olmak için sık karşılaşılan bir durum değil. Bu öğretici, tipik yapılandırma farklılıkları inceler ve ayrı yapılandırma bilgilerini koruma stratejileri bakar.


## <a name="introduction"></a>Giriş


Basit bir web uygulaması dağıtma işlemi, son iki öğreticiler öğrendiniz. [ *Sitenizi FTP istemcisi kullanarak dağıtma* ](deploying-your-site-using-an-ftp-client-cs.md) öğretici tek başına bir FTP istemci geliştirme ortamı üretim kadar gerekli dosyaları kopyalamak için nasıl kullanılacağını gösterdi. Önceki öğreticide [ *dağıtma bilgisayarınızı Site kullanarak Visual Studio*](deploying-your-site-using-visual-studio-cs.md), Visual Studio'nun Web sitesini kopyalama aracı ve yayımlama seçeneğini kullanarak dağıtım görünüyordu. Her iki öğreticilerde üretim ortamında her dosya, geliştirme ortamında bir dosyanın bir kopyasını oluştu. Ancak, üretim ortamında geliştirme ortamında farklı yapılandırma dosyaları için sık karşılaşılan bir durum değildir. Bir web uygulamanın yapılandırma depolanan `Web.config` dosya ve genellikle veritabanı, web ve e-posta sunucuları gibi dış kaynaklar hakkında bilgi içerir. Bu ayrıca işlenmeyen bir özel durum oluştuğunda yapılacak eylem boyunca gibi bazı durumlarda, uygulama davranışını kullanıma harfe dönüştüren.

Bir web uygulamasına dağıtırken doğru yapılandırma bilgilerini üretim ortamında düştüğünden emin önemlidir. Çoğu durumda `Web.config` dosya geliştirme ortamında, üretim ortamına kopyalanamıyor-olduğu. Bunun yerine, özelleştirilmiş bir sürümünü `Web.config` üretime yüklenmesi gerekir. Bu öğreticide daha yaygın yapılandırma farklılıkları bazıları kısaca inceler; Ayrıca, ortamlar arasında farklı yapılandırma bilgilerini korumak için bazı teknikler özetlenmektedir.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Geliştirme ve üretim ortamları arasında tipik yapılandırma farklılıkları

`Web.config` Dosya kaynaklardan bir ASP.NET uygulaması için yapılandırma bilgilerini içerir. Bu yapılandırma bilgileri bazıları olan ortamı aynı. Örneğin, URL yetkilendirme kuralları ve kimlik doğrulama ayarları, İl `Web.config` dosyanın `<authentication>` ve `<authorization>` öğeler, genellikle ortamın bağımsız olarak aynı. Ancak diğer yapılandırma bilgilerini - dış kaynaklar - hakkındaki bilgiler gibi genellikle ortamına bağlı olarak farklılık gösterir.

Veritabanı bağlantı dizelerini yapılandırma bilgilerinin farklı prime bir örnek ortamda dayalıdır. Ne zaman bir web uygulaması iletişim kuran bir veritabanı sunucusu ile önce bir bağlantı oluşturmanız gerekir ve aracılığıyla elde bir [bağlantı dizesi](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Kod sabit veritabanı bağlantı dizesinde doğrudan web sayfaları veya veritabanına bağlar kodu mümkün olsa da, yerleştirmek en iyi `Web.config`'s [ `<connectionStrings>` öğesi](https://msdn.microsoft.com/library/bf7sd233.aspx) böylece bağlantı dizesi tek, merkezi bir konumda bilgilerdir. Önerilmesine farklı bir veritabanı üretimde kullanılan geliştirme sırasında kullanılır. Sonuç olarak, bağlantı dizesi bilgilerini, her ortam için benzersiz olmalıdır.

> [!NOTE]
> Bu noktada biz içine veritabanı bağlantı dizelerini yapılandırma dosyasında nasıl depolandığını, özellikleri ele alacağız, veri odaklı uygulamaları dağıtma gelecekteki öğreticilerini keşfedin.


Geliştirme ve üretim ortamları amaçlanan bir davranış önemli ölçüde farklılık gösterir. Geliştirme ortamında bir web uygulaması, küçük bir grup geliştiriciler tarafından test edilmiş ve hata ayıklaması oluşturuluyor. Aynı uygulamanın farklı birden çok eşzamanlı kullanıcı tarafından ziyaret edilen, üretim ortamında. ASP.NET, geliştiriciler, test etme ve bir uygulamanın hatalarını ayıklamaya yardımcı olan özellikler içerir, ancak bu özellikler için performans ve güvenlik nedenleriyle, üretim ortamında devre dışı bırakılması gerekir. Böyle birkaç yapılandırma ayarları göz atalım.

### <a name="configuration-settings-that-impact-performance"></a>Performansı etkileyen yapılandırma ayarları

Bir ASP.NET sayfasını ziyaret edildiğinde ilk kez (veya ilk kez, değiştirildikten sonra) için bildirim temelli, biçimlendirme bir sınıf içinde dönüştürülmesi gerekir ve bu sınıf derlenmiş olmalıdır. Otomatik derleme web uygulamasını kullanıyorsa, sayfa arka plan kod sınıfı, çok derlenmesi gerekir. Derleme seçenekleri kaynaklardan yapılandırabileceğiniz `Web.config` dosyanın [ `<compilation>` öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx).

Hata ayıklama öznitelik en önemli öznitelikleri biridir `<compilation>` öğesi. Varsa `debug` "Visual Studio'da bir uygulamada hata ayıklaması yaparken gerekli hata ayıklama sembolleri, derlenen bütünleştirilmiş kodları eklemeniz true olarak" özniteliğini ayarlayın. Ancak, hata ayıklama sembolleri derleme boyutunu artırın ve ek bellek gereksinimlerini kodu çalışırken büyük oranda yansıtmaktadır. Ayrıca, `debug` "tarafından döndürülen herhangi bir içerik true olarak" özniteliği ayarlanmış `WebResource.axd` , yani her bir kullanıcı tarafından döndürülen statik içeriği yeniden yüklemek için ihtiyaç bir sayfayı ziyaret önbelleğe alınmaz `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` Yerleşik bir HTTP işleyicisini ASP.NET 2.0 ile komut dosyaları, görüntüler, CSS dosyaları ve diğer içerikleri gibi ekli kaynaklar almak için sunucu denetimleri kullanan sunulmuştur. Hakkında daha fazla bilgi için `WebResource.axd` çalışır ve özel sunucu denetimleri, katıştırılmış kaynaklara erişmek için nasıl kullanabileceğinizi görmek [erişme katıştırılmış kaynaklar aracılığıyla bir URL kullanarak `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


`<compilation>` Öğenin `debug` özniteliği ayarlanmış genellikle "true" geliştirme ortamında. Aslında, bu öznitelik "bir web uygulaması; hata ayıklamak için true olarak" olarak ayarlanmalıdır bir ASP.NET uygulamasını Visual Studio'dan hata ayıklama denerseniz ve `debug` özniteliği, "false" olarak ayarlandığında, Visual Studio, uygulama kadar ayıklanamıyor açıklayan bir ileti görüntülenir `debug` özniteliği "true" ve ayarlanır Bu değişikliği sizin yerinize yapmasını sağlar.

Yapmanız gerekenler **hiçbir zaman** sahip `debug` kümesi üzerindeki performans etkisini nedeniyle bir üretim ortamında "true" özniteliği. Bu konu hakkında daha kapsamlı bir açıklama için bkz [Scott Guthrie](https://weblogs.asp.net/scottgu/)'s blog gönderisi [yoksa çalıştırma üretim ASP.NET uygulamaları ile `debug="true"` etkin](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Özel hatalar ve izleme

Bir ASP.NET uygulamasında işlenmeyen bir özel durum oluştuğunda, bu noktada üç şeylerden biri olur çalışma zamanı en fazla Balonlar:

- Genel çalışma zamanı hata iletisi görüntülenir. Bu sayfa, bir çalışma zamanı hatası oluştu, ancak bu hata hakkındaki ayrıntılar sağlamaz kullanıcıya bildirir.
- Yeni oluşturulan bir özel durum hakkında bilgi içeren bir özel durum ayrıntıları iletisi görüntülenir.
- İstediğiniz herhangi bir ileti görüntüler, oluşturduğunuz bir ASP.NET sayfası olan bir özel hata sayfası görüntülenir.

İşlenmeyen bir özel durumla karşılaşıldığında ne bağlıdır `Web.config` dosyanın [ `<customErrors>` bölümü](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Geliştirme ve bir uygulamayı test ederken tarayıcıdaki herhangi bir özel durumun ayrıntılarını görmek için yardımcı olur. Ancak, bir üretim uygulamasında özel durum ayrıntıları gösteren, olası bir güvenlik riski oluşturur. Ayrıca, unflattering ile okuyucunuz arayın, Web sitesi sağlar. İdeal olarak, aynı uygulamayı üretim sırasında bir özel hata sayfası gösterilir ancak işlenmeyen bir özel durum olması durumunda bir web uygulaması geliştirme ortamında özel durumun ayrıntılarını gösterir.

> [!NOTE]
> Varsayılan `<customErrors>` bölümünde ayar gösteren özel durum ayrıntıları iletisini yalnızca sayfanın localhost ziyaret, aksi takdirde genel çalışma zamanı hata sayfası gösterilir. Bu ideal değildir, ancak varsayılan davranışı özel durum ayrıntıları yerel olmayan ziyaretçileri açığa değil bilmeniz işlemlerini. Bir sonraki öğretici inceler `<customErrors>` bölümünde daha ayrıntılı ve üretimde hata oluştuğunda gösterilen özel hata sayfası gösterir.


Geliştirme sırasında yararlı olan başka bir ASP.NET özelliğini izleme. İzleme etkinleştirilirse, her gelen istek ilgili bilgileri kaydeder ve özel bir web sayfası sağlar `Trace.axd`, son isteği ayrıntılarını görüntüleme. Açma ve izleme aracılığıyla yapılandırma [ `<trace>` öğesi](https://msdn.microsoft.com/library/6915t83k.aspx) içinde `Web.config`.

Etkinleştirirseniz emin izleme BT'nin üretimde devre dışı bırakıldı. İzleme bilgilerini tanımlama bilgileri, oturum verilerini ve diğer olası duyarlı bilgileri içerdiğinden, üretimde izleme devre dışı bırakmanız önemlidir. Varsayılan olarak, izleme devre dışı bırakıldı, güzel bir haberimiz var olduğundan ve `Trace.axd` dosyası, yalnızca localhost erişilebilir. Bu varsayılan ayarlar geliştirme değiştirirseniz geri üretimde kapatıldıysa, emin olun.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Ayrı yapılandırma bilgilerini korumak için teknikleri

Farklı yapılandırma ayarları geliştirme ve üretim ortamlarında sahip dağıtım işlemini karmaşık hale getirir. Yapılandırma bilgilerini iki ortamda aynı ise yaklaşım yalnızca çalışır ancak bu önceki iki öğreticilerdeki üretime kadar tüm gerekli dosyaları kopyalama geliştirme dağıtım işlemini söz konusu. Teknikleri değişen yapılandırma bilgileri ile bir uygulama dağıtmak için çeşitli vardır. Şimdi barındırılan web uygulamaları için bu seçeneklerden bazısı katalog.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Üretim ortamı yapılandırma dosyasını el ile dağıtma

İki sürümünü korumak için en basit yaklaşımdır `Web.config` dosyası: bir geliştirme ortamı, diğeri üretim ortamı. Bir site Üretim dağıtımı kapsar geliştirme ortamında üretim sunucusuna tüm dosyaları kopyalama *dışında* için `Web.config` dosya. Bunun yerine, üretim ortama özgü `Web.config` dosya üretime kopyalanacaksa.

Bu yaklaşım çok karmaşık değil, ancak yapılandırma bilgilerini sık değiştiğinden uygulanması kolaydır. Bu, küçük bir geliştirme ekibi, tek bir web sunucusu üzerinde barındırılan ve yapılandırma bilgilerini seyrek değiştirilen uygulamalarıyla en iyi şekilde çalışır. Bu, tek başına bir FTP istemcisi kullanarak uygulama dosyalarını el ile dağıtım yaparken en tenable. Visual Studio Web sitesini kopyalama aracı veya yayımlama seçeneğini kullanırken ilk dağıtımına özgü takas gerekecektir `Web.config` dağıtmadan önce üretim özgü bir dosya ve yeniden dağıtım tamamlandıktan sonra bunları değiştirme.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Derleme veya dağıtım işlemi sırasında yapılandırma değiştirme

Tartışmaları şimdiye kadar geçici bir derleme ve dağıtım işlemi kabul. Birçok büyük yazılım projelerinin daha açık kaynaklı, ev yapımı kullanımını veya üçüncü taraf araçları işlemleri şekillendirir. Bu gibi projeler için büyük olasılıkla üretime atıldı önce yapılandırma bilgilerini uygun şekilde değiştirmek için derleme veya dağıtım işlemi özelleştirebilirsiniz. Web uygulaması kullanarak derleme yaparsanız [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), veya bazı diğer derleme aracını, büyük olasılıkla değiştirmek için bir derleme adımını ekleyin `Web.config` dosyasını üretim özgü ayarlar içerir. Veya dağıtım akışınız programlı olarak kaynak denetim sunucusuna ve uygun almak `Web.config` dosya.

Üretim için uygun yapılandırma bilgilerini almak için gerçek bir yaklaşım, Araçlar ve iş akışı yaygın göre değişir. Bu nedenle, ki bu konuya daha fazla anlamak için delve değil. MSBuild veya NAnt gibi bir popüler derleme aracı kullanıyorsanız, dağıtım makalelerinin ve öğreticiler Bu araçlara web araması yaparak belirli bulabilirsiniz.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Web dağıtımı aracılığıyla yöneten yapılandırma farklılıkları proje eklentisi

2006'da Microsoft Visual Studio 2005'e yönelik Web geliştirme projesi eklentisi yayımladı. Visual Studio 2008 için bir eklenti 2008'de yayınlanmıştır. Bu eklenti yapılandırıldığında, web uygulaması projesi yanı sıra ayrı bir Web dağıtımı projesi, oluşturmak ASP.NET geliştiricilerine yönelik sağlar, açıkça web uygulaması derleme ve yerel çıktı dizinine dağıtmak için dosyaları kopyalar. Web uygulaması projesi arka planda MSBuild'ı kullanır.

Varsayılan olarak, geliştirme ortamına ait `Web.config` dosyayı çıkış dizinine kopyalanır, ancak özelleştirmek için Web dağıtımı projesi ayarlayabilirsiniz.

yapılandırma bilgileri aşağıdaki yollarla bu dizine kopyalanır:

- Aracılığıyla `Web.config` dosya bölüm değiştirme, değiştirilecek bölümü belirtin ve yerine konacak metin içeren bir XML dosyası.
- Dış yapılandırma kaynak dosyasına bir yol sağlayarak. Bu seçenek ile belirli bir Web dağıtımı projesi kopyalar `Web.config` dosyayı çıkış dizinine (yerine `Web.config` geliştirme ortamında kullanılan dosya).
- Web dağıtımı projesi tarafından kullanılan MSBuild dosyası için özel kurallar ekleyerek.

Web dağıtımı için uygulama, Web dağıtımı projesi oluşturun ve sonra üretim ortamına projenin çıkış klasöründeki dosyaları kopyalayın.

Bilgi edinmek için Web dağıtımı projesi kullanma hakkında daha fazla kullanıma [Web dağıtımı projeleri makalede](https://msdn.microsoft.com/magazine/cc163448.aspx) Nisan 2007 çıkıştan [MSDN dergisi](https://msdn.microsoft.com/magazine/default.aspx), ya da daha fazla bilgi kısmında bağlantılara başvurun Bu öğreticinin sonunda.

> [!NOTE]
> Web dağıtımı projesi bir Visual Studio eklentisi olarak uygulanır ve Visual Studio Express (Visual Web Developer dahil) sürümleri eklentileri desteklemez çünkü Visual Web Developer ile Web dağıtımı projesi kullanamazsınız.


## <a name="summary"></a>Özet

Bir web uygulaması geliştirme davranışını ve dış kaynaklara genellikle aynı uygulamayı üretimde olduğunda değerinden farklı. Örneğin, veritabanı bağlantı dizeleri, derleme seçeneklerini ve genelde işlenmeyen bir özel durum oluştuğunda davranışı ortamlar arasında farklılık gösterir. Dağıtım işlemi bu uyum sağlamak gerekir. Bu öğreticide ele aldığımız gibi basit bir alternatif yapılandırma dosyasını el ile üretim ortamına kopyalamak yaklaşımdır. Daha zarif çözümleri olası Web dağıtımı projesi eklentisi kullanırken veya bu tür özelleştirmeleri uyum daha şekillendirilmiş bir derleme veya dağıtım işlemi.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Bağlantı dizeleri açıklaması](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [@ ConnectionStrings.com veritabanı bağlantı dizeleri](http://www.connectionstrings.com/)
- [Üretim ASP.NET uygulamalarını çalıştırma `debug="true"` etkin](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Kolay bir hata sayfalarını görüntüleme işlenmeyen özel durumlar için - düzgün bir şekilde yanıt](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Nasıl Yaparım Visual Studio 2008 Web dağıtımı projesi kullanma?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Bir veritabanı dağıtırken anahtar yapılandırma ayarları](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 Web dağıtımı projeleri indirme](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 Web dağıtımı projeleri indir](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [VS 2008 Web dağıtımı projeleri](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [yayımlanan VS 2008 Web dağıtımı proje desteği](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Web Dağıtımı Projeleri](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Önceki](deploying-your-site-using-visual-studio-cs.md)
> [İleri](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
