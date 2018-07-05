---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
title: Sitenizi Visual Studio (C#) kullanarak dağıtma | Microsoft Docs
author: rick-anderson
description: Visual Studio, bir Web sitesine dağıtmaya yönelik araçlar içerir. Bu araçlar hakkında daha fazla, bu öğreticide öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: cde4ee53-a5d0-4937-a54b-67877e8266c3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
msc.type: authoredcontent
ms.openlocfilehash: d549c615ea822d58ae71876ff3acd28c5f773d8a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399766"
---
<a name="deploying-your-site-using-visual-studio-c"></a>Sitenizi Visual Studio (C#) kullanarak dağıtma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_cs.pdf)

> Visual Studio, bir Web sitesine dağıtmaya yönelik araçlar içerir. Bu araçlar hakkında daha fazla, bu öğreticide öğrenin.


## <a name="introduction"></a>Giriş

Önceki öğreticide, bir web ana bilgisayar sağlayıcısı için basit bir ASP.NET web uygulaması dağıtma attınız. Özellikle, FileZilla gibi FTP istemcisi gerekli dosyaları geliştirme ortamından üretim ortamına aktarmak için nasıl kullanılacağını gösterdi öğretici. Visual Studio yerleşik bir web ana bilgisayar sağlayıcısına dağıtımına etmeye de sunar. Bu öğreticide iki araçlarının inceler: yere taşıyabilirsiniz dosyaları uzak web sunucusuna FTP veya FrontPage Server Extensions; kullanarak gelen ve giden Web sitesini kopyalama aracı ve tüm Web sitesi belirtilen konuma kopyalar Yayımla aracı.


> [!NOTE]
> Visual Studio tarafından sunulan diğer dağıtımıyla ilgili araçlar içerir [Web Kurulum projeleri](https://msdn.microsoft.com/library/wx3b589t.aspx) ve [Web dağıtımı projeleri](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) eklenti. Web Kurulum projeleri, bir Web sitesinin içeriği ve yapılandırma bilgilerini tek bir MSI dosyasına paketleyin. Bu seçenek, bir intranet içinde dağıtılan Web siteleri için veya satış müşterilerin kendi web sunucularına yükleyin bir önceden paketlenmiş web uygulaması şirketler için en yararlı olur. Web dağıtımı projeleri geliştirme ortamları ve üretim ortamları için bir Visual Studio belirten yapılandırma farklardan kolaylaştıran Eklenti derlemeleri eklentidir. Bu öğretici serisinde Web Kurulum projeleri ele alınmamaktadır; Web dağıtımı projeleri özetlenir [ *yaygın yapılandırma farklılıkları arasında geliştirme ve üretim* ](common-configuration-differences-between-development-and-production-cs.md) öğretici.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Sitenizi kopyalama Web sitesi Aracı'nı kullanarak dağıtma

Visual Studio Web sitesini kopyalama aracı, tek başına bir FTP istemcisi için işlevselliği benzerdir. Buna koysalar Web sitesini kopyalama aracı Uzak web sitesi FTP veya FrontPage Server Extensions bağlanmanıza olanak sağlar. Benzer şekilde FileZilla'nın kullanıcı arabirimi, Web sitesini kopyalama kullanıcı arabirimi iki bölmeden oluşur: Bu dosyalar hedef sunucuda sağ bölmede listeler sırada sol bölmede yerel dosyaları listeler.

> [!NOTE]
> Web sitesini kopyalama aracı, yalnızca Web sitesi projeleri için kullanılabilir. Visual Studio, bir Web uygulaması projesi ile çalışırken bu aracı sağlar.

Üretim için kitap gözden geçirme uygulamayı yayımlamak için Web sitesini kopyalama Aracı'nı kullanarak bir göz atalım. Web sitesini kopyalama aracı yalnızca Web sitesi proje modeli kullandığınız projeleriyle çalışır çünkü biz yalnızca bu araçla BookReviewsWSP projeyle inceleyebilirsiniz. Bu projeyi açın.

Çözüm Gezgini'nde (Bu simgeyi Şekil 1'de daire içine alınmıştır) Web sitesine Kopyala simgesine tıklayarak Web sitesini kopyalama aracı proje başlatın; Alternatif olarak, Web sitesini kopyalama seçeneği Web sitesi menüsünden seçebilirsiniz. Her iki yöntemle Şekil 1'de gösterilen kopyalama Web sitesi kullanıcı arabirimi başlatır; Şekil 1 yalnızca sol bölmede, uzak bir sunucuya bağlanmak henüz çünkü doldurulur.


[![Kopyalama Web sitesi Aracı'nın kullanıcı arabirimidir iki bölme halinde ayrılmış](deploying-your-site-using-visual-studio-cs/_static/image2.png)](deploying-your-site-using-visual-studio-cs/_static/image1.png)

**Şekil 1**: kopyalama Web sitesi Aracı'nın kullanıcı arabirimidir iki bölme halinde ayrılmış ([tam boyutlu görüntüyü görmek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image3.png))


Sitemizi dağıtmak için öncelikle web ana bilgisayar sağlayıcıya bağlanmak ihtiyacımız var. Web sitesini kopyalama kullanıcı arabiriminin üst Bağlan düğmesine tıklayın. Bu, Şekil 2'de gösterilen Web sitesini aç iletişim kutusunu görüntüler.

Hedef Web sitesine soldan dört seçenekten birini belirleyerek bağlanabilirsiniz:

- **Dosya sistemi** -sitenizi bir klasör veya ağ paylaşımına erişilebilir bilgisayarınızdan dağıtmak için bu seçeneği belirleyin.
- **Yerel IIS** -site bilgisayarınızda yüklü IIS web sunucusuna dağıtmak için bu seçeneği kullanın.
- **FTP sitesi** -Uzak web sitesine FTP kullanarak bağlanın.
- **Uzak Site** -FrontPage Server Extensions'ı kullanarak uzak bir Web sitesine bağlayın.

Çoğu web ana bilgisayar sağlayıcıları FTP desteklese de, daha az FrontPage Server Extensions destek sunar. Bu nedenle, ı FTP sitesi seçeneğini seçtiyseniz, ardından bağlantı bilgilerini Şekil 2'de gösterildiği gibi girilmelidir.


[![Hedef Web sitesi belirtin](deploying-your-site-using-visual-studio-cs/_static/image5.png)](deploying-your-site-using-visual-studio-cs/_static/image4.png)

**Şekil 2**: hedef siteyi belirtin ([tam boyutlu görüntüyü görmek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image6.png))


Web sitesini kopyalama aracı bağlandıktan sonra sağ bölmede uzak sitede dosyalarını yükler ve her dosyanın durumunu gösterir: yeni, Deleted, değiştirilen veya Unchanged. Uzak site veya tersi bir yerel siteden bir dosyaya kopyalayabilirsiniz.

Yeni bir ekleyelim sayfasında BookReviewsWSP projeye ve ardından dağıtın Web sitesini kopyalama Aracı'nda eylem görebiliriz. Yeni bir ASP.NET sayfası adlı kök dizininde Visual Studio'da oluşturma `Privacy.aspx`. Ana sayfayı kullanmak sayfası `Site.master` ve bu sayfaya sitenizin gizlilik ilkesi ekleyin. Bu sayfa oluşturulduktan sonra Şekil 3, Visual Studio gösterir.


[![Adlı yeni bir sayfa ekleyin &lt;kod&gt;Privacy.aspx&lt;/code&gt; Web sitesinin kök klasör](deploying-your-site-using-visual-studio-cs/_static/image8.png)](deploying-your-site-using-visual-studio-cs/_static/image7.png)

**Şekil 3**: adlı yeni bir sayfa ekleyin `Privacy.aspx` Web sitesinin kök klasöre ([tam boyutlu görüntüyü görmek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image9.png))


Ardından, kopyalama Web sitesi kullanıcı arabirimine döndürür. Şekil 4'te gösterildiği gibi sol bölmede şimdi yeni dosyalar - içerir `Policy.aspx` ve `Policy.aspx.cs`. Bunun da ötesinde, bu dosyaları bir ok simgesi ve durumu, yerel site ancak uzak site bulunduğunu belirten yeni bir işaretlenir.


[![Yeni kopya Web sitesi Aracı'nı içeren &lt;kod&gt;Privacy.aspx&lt;/code&gt; sayfasında, sol bölmedeki](deploying-your-site-using-visual-studio-cs/_static/image11.png)](deploying-your-site-using-visual-studio-cs/_static/image10.png)

**Şekil 4**: yeni kopyalama Web sitesi Aracı'nı içeren `Privacy.aspx` sayfasında, sol bölmedeki ([tam boyutlu görüntüyü görmek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image12.png))


Yeni dağıtmak için dosyaları, bunları seçin ve ardından uzak siteye aktarmak ok simgesine tıklayın. Transfer tamamlandıktan sonra `Policy.aspx` ve `Policy.aspx.cs` dosyaları mevcut durumu Unchanged yerel ve uzak sitelerle üzerinde.

Yeni dosyaları listeleme yanı sıra Web sitesini kopyalama aracı yerel ve uzak siteler arasında farklı dosyalar vurgular. Bu uygulamada görmek için dönüş `Privacy.aspx` sayfasında ve birkaç daha fazla sözcük gizlilik ilkesine ekleyin. Sayfayı kaydedin ve ardından Web sitesini kopyalama aracı için döndürür. Şekil 5 gösterildiği gibi `Privacy.aspx` sayfası sol bölmede, değiştirilen uzak site ile eşitlenmemiş olduğunu belirten bir durumu içerir.


[![Kopyalama Web sitesi Aracı'nı gösterir &lt;kod&gt;Privacy.aspx&lt;/code&gt; sayfa değiştirildi](deploying-your-site-using-visual-studio-cs/_static/image14.png)](deploying-your-site-using-visual-studio-cs/_static/image13.png)

**Şekil 5**: kopyalama Web sitesi Aracı'nı gösterir `Privacy.aspx` sayfa değiştirildi ([tam boyutlu görüntüyü görmek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image15.png))


Web sitesini kopyalama aracı, ayrıca bir dosyanın son kopyalama işleminden beri silinip silinmediğini gösterir. Silme `Privacy.aspx` yerel proje ve yenileme Web sitesini kopyalama aracı. `Privacy.aspx` Ve `Privacy.aspx.cs` dosyaları sol bölmede listelenen kalır, ancak son kopyalama işleminden beri kaldırılan olduğunu belirten silindi durumunda.

## <a name="publishing-a-web-application"></a>Web uygulaması yayımlama

Web uygulamanızı Visual Studio'dan dağıtmak için başka bir yolu yapı menüsü aracılığıyla erişilebilir Yayımla seçeneğini kullanmaktır. Yayımla seçeneği açıkça uygulamayı derler ve tüm gerekli dosyaları belirtilen uzak site kadar kopyalar. Kısa bir süre içinde anlatıldığı gibi Yayımla Web sitesini kopyalama aracı daha fazla Küt uçlu bir seçenektir. Web sitesini kopyalama aracı yerel ve uzak siteleri dosyalarda incelemenize olanak sağlar ve karşıya yükleme veya gerektiği gibi tek tek dosyaları indirme izin verir ancak Yayımla seçeneği tüm web uygulaması dağıtır.


Tüm gerekli dosyaları belirtilen uzak siteye kopyalamaya ek olarak Yayımla seçeneğinin de açıkça uygulamayı derler. Web Uygulama projeleri olmasına gerek koşuluyla, açıkça derlenmiş, Web uygulaması projelerinde yayımlama seçeneği kullanılabilir süpriz olarak gelmelidir. Ne biraz şaşırtıcı olabilir Yayımla seçeneğini de Web sitesi projeleri için kullanılabiliyor. Belirtilen [ *belirleme dosyaları gerekenler dağıtılabilir için* ](determining-what-files-need-to-be-deployed-cs.md) Öğreticisi, Web sitesi projeleri açıkça derlenmesi olarak adlandırılan bir işlem aracılığıyla *önceden derleme*. Bu öğreticide, Web Uygulama projeleri ile yayımlama seçeneğini kullanmaya odaklanmıştır; bir sonraki öğretici, önceden derleme, bu noktada Web sitesi projeleri ile yayımlama seçeneğini kullanarak aramak için getireceğiz inceleyeceksiniz.

> [!NOTE]
> Yayımla seçeneği varken Web sitesi projeleri hem de Web Uygulama projeleri için Visual Studio'da Visual Web Developer yalnızca Web Uygulama projeleri için yayımla seçeneğini sunar.


Yayımla seçeneğini kullanarak Kitap incelemeleri uygulamayı dağıtmayı bakalım. Visual Studio'da BookReviewsWAP (Web uygulaması projesi) açarak işleme başlayın. Yayımla Menüsü'nden yapı BookReviewsWAP projesini seçin. Bu hedef konumu (bkz. Şekil 6) diğer yapılandırma seçenekleri arasından için soran bir iletişim kutusu getirir. Çok gibi Web sitesini kopyalama aracı ile bir yerel klasör, yerel bir Web sitesi IIS'de, FrontPage Server Extensions veya FTP sunucu adresi destekleyen uzak bir Web sitesi işaret eden bir konuma girebilirsiniz. Dosyaları uzak web sunucusu üzerinde dağıtılan dosyalarla değiştirin veya tüm yayımlamadan önce uzak sitede içeriği silmek için seçebilirsiniz. Ayrıca, kopyalanıp kopyalanmayacağını da belirtebilirsiniz:

- Yalnızca gereksiz kaynak kodu ve proje ile ilgili dosyaları atlayarak uygulamasını çalıştırmak için gerekli proje dosyaları.
- Çözüm dosyası gibi tüm kaynak kodu dosyaları içeren proje dosyalarını ve Visual Studio proje dosyaları.
- Bunlar projeye dahil bağımsız olarak kaynak proje klasöründeki tüm dosyaları kopyalar kaynak proje klasöründeki tüm dosyalar.

İçeriği karşıya yüklemek için bir seçenek de mevcuttur `App_Data` klasör.


[![Hedef Web sitesi belirtin](deploying-your-site-using-visual-studio-cs/_static/image17.png)](deploying-your-site-using-visual-studio-cs/_static/image16.png)

**Şekil 6**: hedef siteyi belirtin ([tam boyutlu görüntüyü görmek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image18.png))


Kitap İnceleme uygulama için Uzak site BookReviewsWSP projenin Web sitesini kopyalama aracı aracılığıyla kopyalarken dağıtılan dosyaları içerir. Bu nedenle, şimdi tüm varolan içeriğin silerek Başlat yayınlama seçeneğiniz vardır. Ayrıca, şimdi yalnızca üretim ortamını gereksiz kaynak kodu ve proje dosyaları ile yığılmak yerine gerekli dosyaları kopyalayın. Bu seçenekleri belirttikten sonra Yayımla düğmesine tıklayın. Sonraki birkaç saniye içinde Visual Studio gerekli dosyaları hedef sitenin, çıktı penceresinde, ilerleme durumunu görüntüleme dağıtır.

Şekil 7, yayımlama işlemi tamamlandıktan sonra FTP sitesindeki dosyaları gösterir. Yalnızca işaretleme sayfaları ve gereken sunucu tarafı ve istemci tarafı destek dosyalarını yüklenmiş unutmayın.


[![Yalnızca gerekli dosyalar üretim ortamına yayımlandı](deploying-your-site-using-visual-studio-cs/_static/image20.png)](deploying-your-site-using-visual-studio-cs/_static/image19.png)

**Şekil 7**: yalnızca gerekli dosyalar yayımlandı üretim ortamına ([tam boyutlu görüntüyü görmek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image21.png))


Yayımla seçeneği, Web sitesini kopyalama aracı daha az incelikli bir araçtır. Yayımla seçeneği, Web sitesini kopyalama aracı yerel ve uzak sitelerdeki dosyalarını inceleme ve bunların nasıl değişiklik gösterdiği görmenizi sağlar ancak böyle bir arabirim sağlar. Ayrıca, Web sitesini kopyalama Aracı'nı yükleyerek veya tek tek dosyaları silme tek seferlik değişiklikler yapmanızı sağlar. Yayımla seçeneği gibi hassas bir denetim olanak tanımaz; Bunun yerine, yayımlar *tüm* uygulama. Bu davranış, kendi Artıları ve eksileri vardır. Artı tarafında önemli bir dosyayı karşıya yüklemek için unutarak olmaz Yayımla seçeneğini kullanırken bildirin. Ancak, çok büyük bir Web sitesine - küçük bir değişiklik yaptıysanız yayımlama seçeneğiyle birlikte bu sayfayı veya değiştirilmiş olan iki güncelleştirilemiyor ancak bunun yerine Visual Studio tüm site dağıtır sırada beklemelisiniz ne olacağını düşünün.

İçerikleri üretim ve geliştirme ortamları arasında farklılık gösterir. bazı dosyalar burada sık sık değil. Bir uygulamanın yapılandırma dosyası, anahtar örnektir `Web.config`. Yayımla seçeneği körüne web uygulama dosyaları kopyaladığı için sürüm geliştirme ortamında üretim ortamının özelleştirilmiş yapılandırma dosyalarını yazar. Sonraki öğretici, bu konuda daha fazla inceler ve böyle farklılıklar da mevcut bir web uygulaması dağıtımı için ipuçları sunar.

## <a name="summary"></a>Özet

Bir Web sitesi dağıtımı için gerekli dosyaları geliştirme ortamından üretim ortamına kopyalanmasıdır. Önceki öğreticide FileZilla gibi FTP istemcisi kullanarak dosyaları aktarma nasıl oluşturulacağını gösterir. Bu öğreticide iki dağıtım araçları Visual Studio'da inceledi: Web sitesini kopyalama Aracı'nı ve yayımlama seçeneği. Web sitesini kopyalama aracı, dosyaları yerel bilgisayarda ve karşıya yükleme veya indirme iki bilgisayar arasında dosyaları kolaylaştırır belirtilen bir uzak bilgisayar listeleme iki paned bir arabirimi vardır, bir FTP istemcisi için benzerdir. Yayımla seçeneği, açıkça projeyi derler ve sonra tüm uygulama belirtilen hedefe dağıtır daha Küt uçlu bir araçtır.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Web sitesi kopyalama Web sitesi Aracı'nı kullanarak kopyalama](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Ne ı: dağıtma kopyalama Web sitesi Aracı'nı kullanarak bir Web sitesi](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (Video)
- [Nasıl yapılır: Web Uygulama projeleri yayımlama](https://msdn.microsoft.com/library/aa983453.aspx)
- [Nasıl yapılır: Web siteleri yayımlama](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Kurulum ve dağıtım projeleri Visual Studio'da](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Önceki](deploying-your-site-using-an-ftp-client-cs.md)
> [İleri](common-configuration-differences-between-development-and-production-cs.md)
