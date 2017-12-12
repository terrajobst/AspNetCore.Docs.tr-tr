---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: "Geliştirme ve üretim (C#) ortak yapılandırma farklarını | Microsoft Docs"
author: rick-anderson
description: "Önceki öğreticilerde, tüm ilgili dosyaları geliştirme ortamından üretim ortamına kopyalayarak biz bizim Web sitesi dağıtıldı. Ancak, ı..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 25299d1f047542ac4f2d61f9d5fe55813517f76b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="common-configuration-differences-between-development-and-production-c"></a>Geliştirme ve üretim (C#) ortak yapılandırma farklılıkları
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> Önceki öğreticilerde, tüm ilgili dosyaları geliştirme ortamından üretim ortamına kopyalayarak biz bizim Web sitesi dağıtıldı. Bununla birlikte, her ortamına sahip benzersiz bir Web.config dosyası, gerekir ortamları yapılandırma farklarını olmasını orada seyrek değil. Bu öğretici, normal yapılandırma farkları inceler ve ayrı yapılandırma bilgilerini korumak için stratejileri bakar.


## <a name="introduction"></a>Giriş


Son iki öğreticileri, basit bir web uygulaması dağıtma aracılığıyla gitti. [ *, Sitenizi bir FTP istemcisi kullanarak dağıtma* ](deploying-your-site-using-an-ftp-client-cs.md) öğretici tek başına bir FTP istemcisi üretim kadar geliştirme ortamı gerekli dosyaları kopyalamak için nasıl kullanılacağı gösterilmiştir. Önceki öğretici [ *dağıtma bilgisayarınızı Site kullanarak Visual Studio*](deploying-your-site-using-visual-studio-cs.md), aranan Visual Studio'nun Web sitesini kopyalama aracı ve yayımlama seçeneğini kullanarak dağıtım sırasında. Her iki eğitimlerine üretim ortamında her dosyayı geliştirme ortamında bir dosyanın bir kopyasını oluştu. Ancak, üretim ortamında geliştirme ortamında farklılık yapılandırma dosyaları için seyrek değil. Bir web uygulamasının yapılandırma depolanan `Web.config` dosya ve genellikle veritabanı, web ve e-posta sunucuları gibi dış kaynaklar hakkında bilgi içerir. Bu ayrıca işlenmeyen bir özel durum oluştuğunda yapılacak eylem seyri gibi bazı durumlarda, uygulamanın davranışını çıkışı harfe dönüştüren.

Bir web uygulaması dağıtırken doğru yapılandırma bilgilerini üretim ortamında sona önemlidir. Çoğu durumda `Web.config` dosya geliştirme ortamında üretim ortamı olarak kopyalanamıyor-değil. Bunun yerine, özelleştirilmiş bir sürümünü `Web.config` üretime karşıya gerekiyor. Bu öğretici kısaca daha yaygın yapılandırma farklılıklar inceler; Ayrıca, ortamlar arasında farklı yapılandırma bilgilerini korumak için bazı teknikleri özetler.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Geliştirme ve üretim ortamlarını normal yapılandırma farklılıkları

`Web.config` Dosyası, bir ASP.NET uygulaması için yapılandırma bilgilerini bir listesini içerir. Bu yapılandırma bilgilerin bazıları olduğu ortam bakılmaksızın aynı. Örneğin, URL yetkilendirme kuralları ve kimlik doğrulama ayarları içinde yazıyla `Web.config` dosyanın `<authentication>` ve `<authorization>` öğeleridir genellikle ortamı bakılmaksızın aynı. Ancak diğer yapılandırma bilgilerini - dış kaynaklara - hakkındaki bilgiler gibi genellikle ortamına bağlı olarak farklılık gösterir.

Veritabanı bağlantı dizeleri farklı yapılandırma bilgilerini prime örneği ortamda dayalıdır. Ne zaman bir web uygulaması iletişim kuran bir veritabanı sunucusu ile önce bir bağlantı kurması gerekir ve aracılığıyla elde bir [bağlantı dizesi](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Kod sabit veritabanı bağlantı dizesinde doğrudan web sayfaları veya veritabanına bağlanan kodu mümkün olsa da, yerleştirmek en iyisidir `Web.config`'s [ `<connectionStrings>` öğesi](https://msdn.microsoft.com/en-us/library/bf7sd233.aspx) böylece bağlantı dizesi tek ve merkezi bir konumda bilgilerdir. Görmemeleri farklı bir veritabanı üretimde halinden geliştirme sırasında kullanılır; Sonuç olarak, bağlantı dizesi bilgilerini her ortam için benzersiz olmalıdır.

> [!NOTE]
> Bu noktada biz veritabanı bağlantı dizelerini yapılandırma dosyasında nasıl depolandığını özelliklerini ıntune'un, veri güdümlü uygulamaları dağıtma gelecekteki öğreticileri keşfedin.


Geliştirme ve üretim ortamlarını hedeflenen davranışını önemli ölçüde farklılık gösterir. Bir web uygulaması geliştirme ortamında, küçük bir Geliştiriciler grubu tarafından test edilmiş ve hata ayıklaması oluşturuldu. Aynı uygulama farklı birden çok eşzamanlı kullanıcı tarafından ziyaret edildiğinde, üretim ortamında. ASP.NET çok sayıda test etme ve uygulama hata ayıklama, geliştiricilerin özelliği içerir, ancak bu özellikler performans ve güvenlik nedenleriyle, üretim ortamında devre dışı bırakılmalıdır. Bu tür birkaç yapılandırma ayarları bakalım.

### <a name="configuration-settings-that-impact-performance"></a>Performansı etkileyen yapılandırma ayarları

Bir ASP.NET sayfasını ziyaret ilk kez (veya onu değiştikten sonra ilk kez) için bildirim temelli biçimlendirme bir sınıfına dönüştürülmesi gerekir ve bu sınıf derlenmiş gerekir. Otomatik derleme web uygulamasını kullanıyorsa, sayfanın arka plandaki kod sınıfı, çok derlenmesi gerekiyor. Derleme seçenekleri bir sınıflama yapılandırabilirsiniz `Web.config` dosyanın [ `<compilation>` öğesi](https://msdn.microsoft.com/en-us/library/s10awwz0.aspx).

Hata ayıklama özniteliği en önemli öznitelikleri biri `<compilation>` öğesi. Varsa `debug` özniteliği ayarlanmış derlenmiş derlemeleri Visual Studio uygulamada hata ayıklama sırasında gerekli hata ayıklama simgeleri eklemek sonra "true". Ancak hata ayıklama simgeleri derleme boyutunu artırın ve kod çalıştırırken ek bellek gereksinimleri. Ayrıca, `debug` özniteliği ayarlanmış "tarafından döndürülen herhangi bir içerik true" `WebResource.axd` , her bir kullanıcı tarafından döndürülen statik içeriği yeniden yüklemek için gerekir, bir sayfasını ziyaret anlamı önbelleğe alınmaz `WebResource.axd`.

> [!NOTE]
> `WebResource.axd`Yerleşik bir HTTP işleyicisini ASP.NET 2.0 ile komut dosyaları, görüntüler, CSS dosyaları ve diğer içerik gibi katıştırılmış kaynakları almak için sunucu denetimleri kullanan sunulmuştur. Hakkında daha fazla bilgi için `WebResource.axd` çalışır ve özel sunucu denetimleri, katıştırılmış kaynaklara erişmek için nasıl kullanabileceğinizi görmek [erişme katıştırılmış kaynakları aracılığıyla bir URL'yi kullanarak `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


`<compilation>` Öğenin `debug` özniteliği ayarlanmış genellikle geliştirme ortamında "true". Aslında, bu öznitelik bir web uygulaması; hatalarının ayıklanabilmesi için "true" ayarlanmalıdır Visual Studio'dan ASP.NET uygulaması hata ayıklamak deneyin ve `debug` özniteliği, "false" olarak ayarlanmışsa, Visual Studio, uygulama kadar hata ayıklaması yapılabilir edilemez olduğunu belirten bir ileti görüntülenir `debug` özniteliği "true" ve ayarlanır Bu değişiklik yapmasını sağlar.

Yapmanız gerekenler **hiçbir zaman** sahip `debug` kümesi performans üzerindeki etkisini nedeniyle bir üretim ortamında "true" özniteliği. Bu konu hakkında daha kapsamlı bir açıklama için bkz [Scott Guthrie](https://weblogs.asp.net/scottgu/)ın blog gönderisi [yok çalıştırmak üretim ASP.NET uygulamaları ile `debug="true"` etkin](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Özel hatalar ve izleme

Bir ASP.NET uygulamasındaki işlenmeyen bir özel durum oluştuğunda, bu noktada işlemlerden birini olur çalışma zamanı kadar köpürür:

- Bir genel çalışma zamanı hata iletisi görüntülenir. Bu sayfa, bir çalışma zamanı hatası oluştu, ancak hata ile ilgili herhangi bir ayrıntıyı sağlamaz kullanıcıya bildirir.
- Yeni oluşturulan özel durum hakkında bilgi içeren bir özel durum ayrıntıları iletisi görüntülenir.
- İstediğiniz herhangi bir iletisi görüntüleyen, oluşturduğunuz bir ASP.NET sayfası olduğu bir özel hata sayfası görüntülenir.

İşlenmeyen bir özel durum karşısında olanlar bağlıdır `Web.config` dosyanın [ `<customErrors>` bölüm](https://msdn.microsoft.com/en-us/library/h0hfz6fc.aspx).

Geliştirme ve bir uygulamayı test ederken tarayıcıda herhangi bir özel durum ayrıntılarını görmek için yardımcı olur. Ancak, özel durum ayrıntıları ürün uygulamada gösteren olası bir güvenlik riski oluşturur. Ayrıca, unflattering ve profesyonel Ara Web sitenizi yapar. İdeal olarak, üretim aynı uygulamada bir özel hata sayfası gösterilir sırasında işlenmeyen bir özel durum durumunda bir web uygulaması geliştirme ortamında özel durumun Ayrıntılar gösterilir.

> [!NOTE]
> Varsayılan `<customErrors>` bölüm ayarını gösterir özel durum ayrıntıları iletisini yalnızca sayfa localhost ziyaret, aksi takdirde genel çalışma zamanı hata sayfası görüntülenir. Bu ideal değildir, ancak varsayılan davranışı yerel olmayan ziyaretçileri özel durum ayrıntıları ortaya değil bilmeniz modemlerin. Bir sonraki öğretici inceler `<customErrors>` bölümünde daha ayrıntılı ve üretimde bir hata oluştuğunda gösterilen bir özel hata sayfası gösterilmektedir.


Geliştirme sırasında yararlı olan başka bir ASP.NET özelliği izleme. İzleme etkinleştirilirse, her gelen istek ilgili bilgileri kaydeder ve özel bir web sayfası sağlar `Trace.axd`, son istek ayrıntıları görüntülemek için. ' Yı açmak ve aracılığıyla İzlemeyi Yapılandır [ `<trace>` öğesi](https://msdn.microsoft.com/en-us/library/6915t83k.aspx) içinde `Web.config`.

Etkinleştirirseniz emin olun izleme üretimde olan devre dışıdır. İzleme bilgilerini tanımlama bilgileri, oturum verilerini ve diğer olası hassas bilgileri içerdiğinden, üretim izleme devre dışı bırakmak önemlidir. Varsayılan olarak, izleme devre dışı bırakıldı, iyi haber olduğu ve `Trace.axd` dosyasıdır yalnızca localhost erişilebilir. Bu varsayılan ayarlar geliştirme değiştirirseniz geri üretimde kapalı olduğundan emin olun.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Ayrı yapılandırma bilgilerini korumak için teknikleri

Geliştirme ve üretim ortamlarında farklı yapılandırma ayarlarına sahip dağıtım işlemi karmaşık hale getirir. Yapılandırma bilgilerini iki ortamda aynıysa yaklaşım yalnızca çalışır ancak bu önceki iki eğitimlerine dağıtım işlemi, tüm gerekli dosyaların geliştirme üretime kopyalama söz konusu. Değişen yapılandırma bilgilerini içeren bir uygulama dağıtmak için teknikleri çeşitli vardır. Şimdi barındırılan web uygulamaları için bu seçeneklerden bazısı katalog.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Üretim ortamı yapılandırma dosyasını el ile dağıtma

En kolay yaklaşım iki sürümü korumaktır `Web.config` dosya: geliştirme ortamında, diğeri üretim ortamı için için. Bir site için üretim dağıtımında kapsar tüm dosyaları geliştirme ortamında üretim sunucusuna kopyalama *dışında* için `Web.config` dosya. Bunun yerine, üretim ortamı özgü `Web.config` dosyasının üretime kopyalanmış.

Bu yaklaşım çok karmaşık değil, ancak yapılandırma bilgilerini seyrek değiştiğinden uygulanması kolaydır. Küçük geliştirme ekibi ile tek bir web sunucusunda barındırılan ve yapılandırma bilgilerini seyret değiştirilir uygulamalar için en iyi çalışır. Bu, tek başına bir FTP istemcisi kullanarak uygulama dosyalarını el ile dağıtırken en tenable. Visual Studio'nun Web sitesini kopyalama aracı veya yayımlama seçeneğini kullanırken ilk dağıtım özgü takas gerekir `Web.config` dağıtmadan önce üretim özgü bir dosya ve geri dağıtım tamamlandıktan sonra bunları değiştirme.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Derleme ve dağıtım işlemi sırasında yapılandırma değiştirme

Tartışmalara bugüne kadarki geçici derleme ve dağıtım işlemini kabul. Birçok büyük yazılım projelerinin daha açık kaynaklı, ev yapımı kullanımını veya üçüncü taraf araçları işlemleri HTTP'dir. Bu tür projelerde üretime gönderilen önce uygun şekilde yapılandırma bilgilerini değiştirmek için derleme veya dağıtım işlemi büyük olasılıkla özelleştirebilirsiniz. Kullanarak web uygulamanızı yapı [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), veya bazı diğer yapı aracı, büyük olasılıkla değiştirmek için bir derleme adımı ekleyebilirsiniz `Web.config` dosya üretim özgü ayarları içerir. Veya, dağıtım iş akışı programlı olarak kaynak denetim sunucusuna bağlanmak ve uygun almak `Web.config` dosya.

Üretim için uygun yapılandırma bilgilerini almak için gerçek bir yaklaşım araçları ve iş akışı yaygın göre değişir. Bu nedenle, biz bu konuda daha fazla inceleyin değil. MSBuild veya NAnt gibi popüler yapı araç kullanıyorsanız, dağıtım makalelerinin ve öğreticiler belirli bir web araması aracılığıyla bu araçlara bulabilirsiniz.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Web dağıtımı aracılığıyla yönetme yapılandırma farklılıkları proje eklentisi

2006'Microsoft Visual Studio 2005 Web geliştirme projesi eklenti yayımladı. Visual Studio 2008 için bir eklenti 2008'de serbest bırakıldı. Bu eklenti, ASP.NET geliştiricilerin kendi web uygulama projesi yanında ayrı bir Web dağıtım projesi, yerleşik oluşturduğunuzda olanak tanır, açıkça web uygulaması derler ve yerel çıktı dizinine dağıtmak için dosyaları kopyalar. Web uygulaması projesi MSBuild arka planda kullanır.

Varsayılan olarak, geliştirme ortamı 's `Web.config` dosya çıktı dizinine kopyalanır, ancak özelleştirmek için Web dağıtım projesi ayarlayabilirsiniz

Bu dizine aşağıdaki yollarla kopyalanan yapılandırma bilgileri:

- Aracılığıyla `Web.config` dosya bölüm değiştirme, değiştirmek için bölüm belirtin ve değiştirme metnini içeren bir XML dosyası.
- Bir dış yapılandırma kaynak dosyasının yolunu sağlayarak. Bu seçenek ile belirli bir Web dağıtım projesi kopyalar `Web.config` dosyasını çıkış dizinine (yerine `Web.config` geliştirme ortamında kullanılan dosyası).
- Web dağıtım projesi tarafından kullanılan MSBuild dosyası için özel kurallar ekleyerek.

Web dağıtımı için uygulama Web dağıtımı projeyi oluşturun ve ardından dosyaları projenin çıktı klasöründen üretim ortamına kopyalayın.

Öğrenmek için Web dağıtım projesi kullanma hakkında daha fazla kullanıma [Web dağıtım projeleri makalede](https://msdn.microsoft.com/en-us/magazine/cc163448.aspx) Nisan 2007 sorunun gelen [MSDN dergisi](https://msdn.microsoft.com/en-us/magazine/default.aspx), ya da daha fazla bilgi kısmında bağlantılara bakın Bu öğreticide sonuna.

> [!NOTE]
> Web dağıtımı proje bir Visual Studio eklentisi olarak uygulanır ve Visual Studio Express (Visual Web Developer dahil) sürümleri eklentileri desteklemeyen olduğundan, Web dağıtım projesi ile Visual Web Developer kullanamazsınız.


## <a name="summary"></a>Özet

Dış Kaynaklar ve bir web uygulaması geliştirme davranışını aynı uygulama üretimde olduğunda daha genellikle farklıdır. Örneğin, veritabanı bağlantı dizeleri, derleme seçenekleri ve yaygın olarak işlenmeyen bir özel durum oluştuğunda davranışını ortamlar arasında farklılık gösterir. Dağıtım işlemi, bu farklara uyum sağlamak gerekir. Biz bu öğreticide anlatıldığı gibi basit bir alternatif yapılandırma dosyasını üretim ortamına el ile kopyalamanız yaklaşımdır. Daha fazla Zarif çözümleri olası için Web dağıtımı proje eklentisi veya kullanıldığında bu gibi özelleştirmeler uyum daha şekillendirilmiş bir yapı veya dağıtım işlemi.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Bağlantı dizeleri açıklanmıştır](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [@ ConnectionStrings.com veritabanı bağlantı dizeleri](http://www.connectionstrings.com/)
- [Üretim ASP.NET uygulamaları ile çalıştırma `debug="true"` etkin](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Kullanıcı dostu hata sayfalarını görüntüleme işlenmeyen özel durumlar için - düzgün bir şekilde yanıt](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Nasıl Yaparım Visual Studio 2008 Web dağıtım projesi kullanım?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Bir veritabanı dağıtırken anahtar yapılandırma ayarları](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 Web dağıtım projeleri indirme](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 Web dağıtım projeleri indirme](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [VS 2008 Web dağıtım projeleri](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [yayımlanan VS 2008 Web dağıtım projesi desteği](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Web dağıtım projeleri](https://msdn.microsoft.com/en-us/magazine/cc163448.aspx)

>[!div class="step-by-step"]
[Önceki](deploying-your-site-using-visual-studio-cs.md)
[sonraki](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
