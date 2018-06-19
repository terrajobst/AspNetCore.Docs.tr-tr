---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
title: Visual Studio (C#) kullanarak, sitenizi dağıtma | Microsoft Docs
author: rick-anderson
description: Visual Studio, bir Web sitesi dağıtmak için Araçlar içerir. Bu araçları hakkında daha fazla Bu öğreticide öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: cde4ee53-a5d0-4937-a54b-67877e8266c3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
msc.type: authoredcontent
ms.openlocfilehash: f06e2fe1fdfb03b106466a1792f6381495f76096
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888831"
---
<a name="deploying-your-site-using-visual-studio-c"></a>Visual Studio (C#) kullanarak, sitenizi dağıtma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_cs.pdf)

> Visual Studio, bir Web sitesi dağıtmak için Araçlar içerir. Bu araçları hakkında daha fazla Bu öğreticide öğrenin.


## <a name="introduction"></a>Giriş

Bir web ana makine sağlayıcısı için basit bir ASP.NET web uygulaması dağıtma sırasında önceki öğretici arama. Özellikle, bir FTP istemcisi FileZilla gibi gerekli dosyaları geliştirme ortamından üretim ortamına aktarmak için nasıl kullanılacağı gösterilmiştir Öğreticisi. Visual Studio web ana bilgisayar sağlayıcısına dağıtım kolaylaştırmak için yerleşik araçlar da sağlar. Bu öğretici iki bu araçların inceler: taşıyabileceğiniz dosyaları FTP veya FrontPage Server Extensions; kullanarak bir uzak web sunucusuna gelen ve giden kopya Web sitesi aracı ve tüm Web sitesi, belirtilen bir konuma kopyalar. yayımlama aracı.


> [!NOTE]
> Visual Studio tarafından sunulan diğer dağıtımla ilgili araçları dahil [Web Kurulum projeleri](https://msdn.microsoft.com/library/wx3b589t.aspx) ve [Web dağıtım projeleri](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) eklenti. Web Kurulum projeleri, Web sitesinin içeriğini ve tek bir MSI dosyası yapılandırma bilgilerini paketi. Bu seçenek en intranet içinde dağıtılan Web siteleri için veya müşterilerin kendi web sunucularına yükleme önceden paketlenmiş web uygulaması satmak şirketler için yararlıdır. Web dağıtım projeleri geliştirme ortamlarını ve üretim ortamları için bir Visual Studio belirterek yapılandırma farklarını kolaylaştıran eklentisini derlemeler eklentidir. Web Kurulum projeleri Bu öğretici serisinde açıklanmamaktadır; Web dağıtım projeleri özetlenir [ *ortak yapılandırma farklılıkları arasında geliştirme ve üretim* ](common-configuration-differences-between-development-and-production-cs.md) Öğreticisi.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Kopyalama Web sitesi Aracı'nı kullanarak sitenizi dağıtma

Visual Studio'nun Web sitesini kopyalama aracı, tek başına bir FTP istemcisi için işlevselliği benzer. Buna koysalar kopyalama Web sitesi aracı, FTP veya FrontPage Server Extensions üzerinden uzak bir web sitesine bağlanmak sağlar. Benzer şekilde FileZilla'nın kullanıcı arabirimi, Web sitesini kopyalama kullanıcı arabirimi iki bölmeden oluşur: Bu dosyaları hedef sunucuda sağ bölmede listeleyen sırada sol bölmede yerel dosyaları listelenmektedir.

> [!NOTE]
> Web sitesini kopyalama aracı yalnızca Web sitesi projeleri için kullanılabilir. Visual Studio, bir Web uygulaması projesi ile çalışırken bu araç sunar.

Üretim için kitap gözden geçirme uygulamayı yayımlamak için Web sitesini kopyalama aracını kullanarak bir bakalım. Web sitesini kopyalama aracı yalnızca Web sitesi projesini modelini kullanmanız projelerle çalıştığından biz yalnızca BookReviewsWSP projeyle bu aracı kullanarak inceleyebilirsiniz. Bu projeyi açın.

(Bu simgeyi Şekil 1'de daire içine alınmıştır) Çözüm Gezgini'nde Web sitesini kopyalama simgesini tıklatarak Web sitesini kopyalama aracı proje başlatın; Alternatif olarak, Web sitesi menüsünden Web sitesini kopyalama seçeneği seçebilirsiniz. Her iki yaklaşım Şekil 1'de gösterilen Web sitesini kopyalama kullanıcı arabirimi başlatır; Şekil 1 yalnızca sol bölmesinde, bir uzak sunucuya bağlanmak henüz çünkü doldurulur.


[![Kopyalama Web sitesi aracın kullanıcı arabirimidir bölünmüş içine iki bölme](deploying-your-site-using-visual-studio-cs/_static/image2.png)](deploying-your-site-using-visual-studio-cs/_static/image1.png)

**Şekil 1**: kopyasını Web sitesini aracın kullanıcı arabirimidir bölünmüş içine iki bölme ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image3.png))


İlk web ana bilgisayar sağlayıcısına bağlanmak için ihtiyacımız sitemizi dağıtmak için. Web sitesini kopyalama kullanıcı arabirimi üstündeki Bağlan düğmesini tıklatın. Bu, Şekil 2'de gösterilen açık Web sitesi iletişim kutusunu görüntüler.

Sol taraftan dört seçenekten birini seçerek hedef Web sitesine bağlanabilir:

- **Dosya sistemi** -sitenizi bir klasör veya ağ paylaşımına erişilebilir bilgisayarınızdan dağıtmak için bu seçeneği kullanın.
- **Yerel IIS** -site bilgisayarınızda yüklü IIS web sunucusu dağıtmak için bu seçeneği kullanın.
- **FTP sitesi** -Uzak web sitesine FTP kullanarak bağlanın.
- **Uzak Site** -FrontPage Server Extensions kullanarak uzak bir Web sitesine bağlanın.

Çoğu web ana bilgisayar sağlayıcıları FTP destekler, ancak daha az FrontPage Server Extensions destek sunar. Bu nedenle, g FTP sitesi seçeneğinin seçili, ardından bağlantı bilgilerini Şekil 2'de gösterildiği gibi girilmelidir.


[![Hedef Web sitesi belirtin](deploying-your-site-using-visual-studio-cs/_static/image5.png)](deploying-your-site-using-visual-studio-cs/_static/image4.png)

**Şekil 2**: Hedef Web sitesi belirtin ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image6.png))


Bağlandıktan sonra Web sitesini kopyalama Aracı'nı sağ bölmede uzak sitedeki dosyalarını yükler ve her dosyanın durumunu gösterir: yeni, Deleted, değiştirilen veya Unchanged. Uzak site veya tersi bir yerel siteden bir dosyayı kopyalayabilirsiniz.

Yeni bir ekleyelim sayfasında BookReviewsWSP projeye ve ardından dağıtmanız eylem Web sitesini kopyalama aracında görebiliriz. Visual Studio'da adlı kök dizininde yeni bir ASP.NET sayfası oluşturma `Privacy.aspx`. Ana sayfayı kullanmak sayfası `Site.master` ve bu sayfaya, sitenin gizlilik ilkesi ekleyin. Bu sayfayı oluşturulduktan sonra Şekil 3'te Visual Studio gösterir.


[![Adlı yeni bir sayfa ekleyin &lt;kod&gt;Privacy.aspx&lt;/kod&gt; Web sitesinin kök klasörü için](deploying-your-site-using-visual-studio-cs/_static/image8.png)](deploying-your-site-using-visual-studio-cs/_static/image7.png)

**Şekil 3**: adlı yeni bir sayfa ekleyin `Privacy.aspx` Web sitenizin kök klasörüne ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image9.png))


Ardından, kopyalama Web sitesi kullanıcı arabirimine döndür. Şekil 4'te gösterildiği gibi sol bölmede şimdi yeni dosyalar - içeren `Policy.aspx` ve `Policy.aspx.cs`. Daha, bu dosyaları bir ok simgesine ve durumu, yerel sitede ancak uzak site bulunduğunu belirten yeni bir işaretlenir.


[![Yeni kopya Web sitesi Aracı'nı içerir &lt;kod&gt;Privacy.aspx&lt;/kod&gt; , sol bölmedeki sayfası](deploying-your-site-using-visual-studio-cs/_static/image11.png)](deploying-your-site-using-visual-studio-cs/_static/image10.png)

**Şekil 4**: yeni kopyasını Web sitesini Aracı'nı içeren `Privacy.aspx` , sol bölmedeki sayfa ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image12.png))


Yeni dağıtmak için dosyaları bunları seçin ve ardından uzak siteye aktarmak için ok simgesine tıklayın. Transfer tamamlandıktan sonra `Policy.aspx` ve `Policy.aspx.cs` dosyaları mevcut Unchanged durum yerel ve uzak sitelerle üzerinde.

Yeni dosyaları listeleme yanı sıra yerel ve uzak siteleri arasında farklılık herhangi bir dosya kopyalama Web sitesi aracı vurgular. Bu eylem görmek için dönmek `Privacy.aspx` sayfasında ve birkaç sözcük gizlilik ilkesi ekleyin. Sayfayı kaydedin ve ardından Web sitesini kopyalama Aracı'na dönün. Şekil 5 gösterildiği gibi `Privacy.aspx` sayfası sol bölmede, değiştirilen uzak site ile eşit olduğunu belirten bir durumu içerir.


[![Kopyalama Web sitesi aracı belirten &lt;kod&gt;Privacy.aspx&lt;/kod&gt; sayfa değiştirildi](deploying-your-site-using-visual-studio-cs/_static/image14.png)](deploying-your-site-using-visual-studio-cs/_static/image13.png)

**Şekil 5**: kopyalama Web sitesi aracı belirten `Privacy.aspx` sayfa değiştirildi ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image15.png))


Web sitesini kopyalama aracı ayrıca bir dosyayı son kopyalama işlemi bu yana silinmiş olmadığını gösterir. Silme `Privacy.aspx` yerel proje ve yenileme kopyalama Web sitesi aracı. `Privacy.aspx` Ve `Privacy.aspx.cs` dosyaları sol bölmede listelenen kalır, ancak son kopyalama işlemi bu yana kaldırılmış olduğunu gösteren silinmiş durumuna sahip.

## <a name="publishing-a-web-application"></a>Bir Web uygulaması yayımlama

Visual Studio içinde web uygulamanızı dağıtmak için başka bir yapı menüsü üzerinden erişilebilen yayımlama seçeneğini kullanmak için yoludur. Yayımla seçeneği açıkça uygulama derler ve tüm gerekli dosyaları belirtilen uzak site kadar kopyalar. Kısa süre içinde anlatıldığı gibi Yayımla Web sitesini kopyalama aracı daha fazla Küt uçlu seçenektir. Web sitesini kopyalama aracı, yerel ve uzak siteleri dosyalarda incelemenize olanak tanır ve karşıya yükleme veya gerektiği gibi tek tek dosyaları indirme izin verir kullanılabilirken Yayımla seçeneği tüm web uygulamasına dağıtır.


Tüm gerekli dosyaları belirtilen uzak siteye kopyalamaya ek olarak Yayımla seçeneği de açıkça uygulama derler. Web uygulaması projelerine olmasına gerek o açıkça derlenmiş, Web uygulaması projelerinde Yayımla seçeneği kullanılabilir hiç şaşırtıcı olarak gelmelidir. Ne biraz şaşırtıcı olabilir Yayımla seçeneği de Web sitesi projeleri için kullanılabilir olmasıdır. İçinde belirtildiği gibi [ *belirleme dosyaları gerekenler dağıtılacağını için* ](determining-what-files-need-to-be-deployed-cs.md) öğretici, Web sitesi projeleri açıkça derlenmesi denen bir işlem aracılığıyla *ön derleme*. Bu öğretici, Web Uygulama projeleri ile yayımlama seçeneğini kullanarak odaklanır; bir sonraki öğretici, bu noktada Web sitesi projeleri ile yayımlama seçeneğini kullanarak aramak için getireceğiz ön derleme inceleyeceksiniz.

> [!NOTE]
> Yayımla seçeneği Web sitesi projeleri ve Web Uygulama projeleri için Visual Studio'da kullanılabilir olsa da, Visual Web Developer yalnızca Web Uygulama projeleri yayımlama seçeneği sunar.


Yayımla seçeneği kullanılarak Kitap incelemeleri uygulamasını dağıtma sırasında bakalım. Visual Studio'da BookReviewsWAP (Web uygulaması projesi) açarak başlayın. Yayımla Menüsü'nden yapı BookReviewsWAP projesini seçin. Bu hedef konumu, (bkz. Şekil 6) diğer yapılandırma seçenekleri arasında isteyen bir iletişim kutusu açar. Çok gibi Web sitesini kopyalama aracı ile bir yerel klasör, yerel bir Web sitesi IIS'de, FrontPage Server Extensions veya FTP sunucusu adresi destekleyen uzak bir Web sitesine işaret eden bir konum girebilirsiniz. Uzak web sunucusundaki dosyaları ile dağıtılan dosyaları değiştirmek mi, yoksa tüm yayımlamadan önce uzak site içeriğini silmek için seçebilirsiniz. Kopyalanıp kopyalanmayacağını belirtebilirsiniz:

- Yalnızca gereksiz kaynak kodu ve projeyle ilişkili dosyaları atlayarak uygulamayı çalıştırmak için gerekli proje dosyaları.
- Çözüm dosyası gibi tüm kaynak kodu dosyaları içeren proje dosyalarını ve Visual Studio proje dosyaları.
- Bunlar projeye dahil bağımsız olarak kaynak proje klasöründeki tüm dosyaları kopyalar kaynak proje klasöründeki tüm dosyalar.

Ayrıca içeriğini karşıya yüklemek için bir seçenek olan `App_Data` klasör.


[![Hedef Web sitesi belirtin](deploying-your-site-using-visual-studio-cs/_static/image17.png)](deploying-your-site-using-visual-studio-cs/_static/image16.png)

**Şekil 6**: Hedef Web sitesi belirtin ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image18.png))


Kitap gözden geçirme uygulama için Uzak site kopyalama Web sitesi aracı yoluyla BookReviewsWSP proje kopyalarken dağıtılan dosyalarını içerir. Bu nedenle, şimdi tüm var olan içerik silerek Başlat Yayımla seçeneğiniz vardır. Ayrıca, şirketinizdeki yalnızca üretim ortamını gereksiz kaynak kodu ve proje dosyalarını içeren alanınızda karışıklık yerine gerekli dosyaları kopyalayın. Bu seçenekleri belirttikten sonra Yayımla düğmesine tıklayın. Sonraki birkaç saniye boyunca Visual Studio gerekli dosyaları hedef siteye çıktı penceresinde ilerleme durumunu görüntüleme dağıtır.

Yayımlama işlemi tamamlandıktan sonra Şekil 7 FTP sitesinde dosyaları gösterir. Yalnızca biçimlendirme sayfalar ve gerekli sunucu tarafı ve istemci tarafı destek dosyalarını karşıya olduğunu unutmayın.


[![Yalnızca gerekli dosyalar üretim ortamına yayımlanan](deploying-your-site-using-visual-studio-cs/_static/image20.png)](deploying-your-site-using-visual-studio-cs/_static/image19.png)

**Şekil 7**: yalnızca gerekli dosyalar olan yayımlanan üretim ortamına ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-your-site-using-visual-studio-cs/_static/image21.png))


Yayımla seçeneği, kopyalama Web sitesi aracı daha az nuanced bir araçtır. Yayımla seçeneği Web sitesini kopyalama aracı yerel ve uzak siteleri dosyalarda inceleyin ve bunların nasıl farklı görmenizi sağlar ancak böyle bir arabirim sağlar. Ayrıca, Web sitesini kopyalama Aracı'nı yükleyerek veya tek tek dosyaların silinmesi bir kerelik değişiklik yapmanızı sağlar. Yayımla seçeneği gibi hassas bir denetim izin verme; Bunun yerine, yayımlar *tüm* uygulama. Bu davranış, Artıları ve eksileri vardır. Artı tarafında önemli bir dosyayı karşıya yüklemeyi unutulması olmaz Yayımla seçeneği kullanılırken bildirin. Ancak çok büyük bir Web sitesine - küçük değişiklikler yaptıysanız yayımlama seçeneğiyle birlikte bu sayfayı veya değiştirilmiş olan iki güncelleştirilemiyor, ancak tüm site Visual Studio dağıtır; bunun yerine, bekleyin gerekir ne olacağını düşünün.

Seyrek orada içerikleri geliştirme ortamlarını ve üretim arasında farklılık gösterir. bazı dosyalar için değil. Uygulamanın yapılandırma dosyasına anahtar örnektir `Web.config`. Yayımla seçeneği doğrudan web uygulama dosyaları kopyaladığı için üretim ortamı özelleştirilmiş yapılandırma dosyalarını geliştirme ortamında sürümüyle değiştirir. Sonraki öğretici, bu konuda daha fazla inceler ve bu farklara varken bir web uygulaması dağıtımı için ipuçları sunar.

## <a name="summary"></a>Özet

Bir Web sitesi dağıtımı için gerekli dosyaları geliştirme ortamından üretim ortamına kopyalama içerir. Önceki öğretici FileZilla gibi bir FTP istemcisi kullanarak dosyaları aktarmak nasıl oluşturulacağını gösterir. Bu öğreticide Visual Studio içindeki iki dağıtım araçları incelenmesi: Web sitesini kopyalama aracı ve Yayımla seçeneği. Web sitesini kopyalama aracı, yerel bilgisayarda ve karşıya yükleme veya iki bilgisayar arasında dosya indirme daha kolay hale getirir belirtilen bir uzak bilgisayar dosyaları listeleme bir iki paned arabirimine sahiptir, bir FTP istemcisi için benzerdir. Yayımla seçeneği açıkça Proje derlenir ve tüm uygulama belirtilen hedefe dağıtır daha Küt uçlu bir araçtır.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Web sitesi kopyasını Web sitesini aracı ile kopyalama](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Ne I: dağıtmak kopyalama Web sitesi Aracı'nı kullanarak bir Web sitesi](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (Video)
- [Nasıl yapılır: Web Uygulama projeleri yayımlama](https://msdn.microsoft.com/library/aa983453.aspx)
- [Nasıl yapılır: Web siteleri yayımlama](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Kurulum ve dağıtım projeleri Visual Studio'da](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Önceki](deploying-your-site-using-an-ftp-client-cs.md)
> [sonraki](common-configuration-differences-between-development-and-production-cs.md)
