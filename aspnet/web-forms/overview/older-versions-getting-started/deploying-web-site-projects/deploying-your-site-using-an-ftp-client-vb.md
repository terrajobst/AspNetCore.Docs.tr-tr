---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: Sitenizi FTP istemcisi (VB) kullanarak dağıtma | Microsoft Docs
author: rick-anderson
description: Bir ASP.NET uygulaması dağıtmanın en basit yolu, gerekli dosyaları geliştirme ortamından üretim ortamına el ile kopyalamanız sağlamaktır. Düzeltmeyi...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: ea6d2deaaad1112f4a5ce4e4ea5534c6eab35a8d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755325"
---
<a name="deploying-your-site-using-an-ftp-client-vb"></a>Sitenizi FTP istemcisi (VB) kullanarak dağıtma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> Bir ASP.NET uygulaması dağıtmanın en basit yolu, gerekli dosyaları geliştirme ortamından üretim ortamına el ile kopyalamanız sağlamaktır. Bu öğreticide web ana bilgisayar sağlayıcıya masaüstünüzden dosyalarının alınacağı bir FTP istemcisi kullanmayı gösterir.


## <a name="introduction"></a>Giriş

Önceki öğreticide, ASP.NET sayfaları, ana sayfa, özel bir temel birkaç oluşan basit bir kitap gözden geçirme ASP.NET web uygulaması sunulan `Page` , görüntüleri bir dizi sınıf ve üç CSS stil sayfaları. Artık bu noktada uygulama herkes için Internet bağlantısı olan erişilebilir bir web ana bilgisayar sağlayıcısı, bu uygulamayı dağıtmak hazırız!


Bizim tartışmalara gelen [ *belirleme dosyaları gerekenler dağıtılabilir için* ](determining-what-files-need-to-be-deployed-vb.md) Öğreticisi, web ana bilgisayar sağlayıcıya kopyalanacak dosyaları gerekenler biliyoruz. (Geri çağırma hangi dosyalar kopyalanır, uygulamanızın açıkça ya da otomatik olarak derlenmiş üzerinde bağlıdır.) Ancak nasıl biz dosyaları (Masaüstü bizim) geliştirme ortamından üretim ortamına (web ana bilgisayar sağlayıcısı tarafından yönetilen web sunucusu) kadar elde ederim? [ **F** ile **T** aktarma **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) dosyaları bir makineden diğerine bir ağ üzerinden kopyalanması için yaygın olarak kullanılan bir protokoldür. FrontPage Sunucu Uzantıları (FPSE) başka bir seçenektir. Bu öğreticide, gerekli dosyaları geliştirme ortamından üretim ortamına dağıtmak için tek başına bir FTP istemci yazılımını kullanmaya odaklanmıştır.

> [!NOTE]
> FTP aracılığıyla Web sitelerini yayımlamak için Araçlar, Visual Studio içerir; FPSE, kullandığınız araçları göz yanı sıra, bu araçlar, sonraki öğreticide ele alınmaktadır.


İhtiyacımız FTP kullanarak dosyaları kopyalamak için bir *FTP istemcisi* geliştirme ortamı. Bir FTP istemcisi çalıştıran bir bilgisayara yüklü bir bilgisayardan dosyaları kopyalamak için tasarlanan bir uygulama olan bir *FTP sunucusu*. (Çoğu gibi web ana bilgisayar sağlayıcınız FTP aracılığıyla dosya aktarımı destekliyorsa, ardından yoktur, web sunucuları üzerinde çalışan bir FTP sunucusuna.) Bir dizi bir FTP istemci uygulamaları vardır. Web tarayıcınızda bir FTP istemcisi bile çift olabilir. My sık kullandığınız FTP istemcisinden ve ben kullanacağınız için Bu öğreticide, bir [FileZilla](http://filezilla-project.org/), Windows, Linux ve Mac bilgisayarları için kullanılabilir bir ücretsiz, açık kaynaklı FTP istemcisi. Herhangi bir FTP istemcisi, yine de bunu kullanımında en rahat kullanabileceğiniz hangi istemci ücretsiz çalışır.

İzliyorsanız, önce bir web ana bilgisayar sağlayıcısı ile hesap oluşturmak için Bu öğreticide veya sonraki olanları tamamlayabilirsiniz. Önceki öğreticide belirtildiği gibi çok geniş bir spektrumda Fiyatlar, özellikleri ve hizmet kalitesi ile web ana bilgisayar sağlayıcısı şirketlerinin bir gaggle vardır. Bu öğretici dizisinde miyim kullanacaklardır [indirim ASP.NET](http://discountasp.net) my web ana bilgisayarı sağlayıcısı, ancak izleyebilirsiniz yanı sıra herhangi bir web ana bilgisayar sağlayıcı sitenizi halinde geliştirilen verze technologie ASP.NET destekledikleri sürece. (Bu öğreticileri ASP.NET 3.5 kullanılarak oluşturulan.) Biz web ana bilgisayar sağlayıcıya dosyaları sıkıştırılabilen Ayrıca, Bu öğretici ve olanları gelecekte FTP kullanarak web ana bilgisayar sağlayıcınız web sunucularına FTP erişimi desteklediğini zorunlu olmasıdır. Neredeyse tüm web ana bilgisayar sağlayıcıları bu özelliği sunar, ancak kaydolmadan önce yeniden denetlemeniz gerekir.

## <a name="deploying-the-book-review-web-application-project"></a>Kitap gözden geçirme Web uygulama projesini dağıtma

Geri çağırma kitap gözden geçirme web uygulamasının iki sürümü vardır: bir Web uygulaması Proje Modeli (BookReviewsWAP) ve diğer Web sitesi projesi modelini (BookReviewsWSP) kullanarak kullanılarak uygulanan. Proje türü olup olmadığını açıkça veya otomatik olarak site derlenir ve hangi dosyaların dağıtılması gerekiyor, bu derleme modeli belirler etkiler. Sonuç olarak, BookReviewsWAP ve BookReviewsWSP projeleri ayrı olarak dağıtma ile BookReviewsWAP başlayarak inceleyeceğiz. Bunu zaten yapmadıysanız, bu iki ASP.NET uygulamalarını indirmek için bir dakikanızı ayırın.

Giderek BookReviewsWAP proje başlatmak `BookReviewsWAP` klasörü ve çift `BookReviewsWAP.sln` dosya. Projeyi dağıtmadan önce kaynak kodu değişiklikleri oluşturulan derlemeye dahil olduğundan emin olmak için oluşturmak önemlidir. Projeyi derlemek için derleme menüsüne gidin ve yapı BookReviewsWAP menü seçeneğini seçin. Bu projedeki kaynak kodunu tek bir derleme derler `BookReviewsWAP.dll`, içinde yerleştirilen `Bin` klasör.

Artık gerekli dosyaları dağıtmak hazırız! FTP istemci başlatın ve web ana bilgisayar Sağlayıcınızdaki web sunucusuna bağlanın. (Bir web barındırma şirketi ile oturum açtığınızda, size FTP sunucusuna bağlanma hakkında bilgi e-posta; bu FTP sunucusunun hem de bir kullanıcı adı ve parola adresini içerir.)

Aşağıdaki dosyaları masaüstünüzden web ana bilgisayar Sağlayıcınızdaki kök Web sitesi klasörüne kopyalayın. Web sunucusuna FTP, web sağlayıcısı barındırdığınızda, Web sitesinin kök dizininde olasıdır. Ancak, bazı web ana bilgisayar sağlayıcıları adlı bir alt sahip `www` veya `wwwroot` , Web sitesi dosyalarınız için kök klasör olarak görev yapar. Son olarak, dosyaları FTPing olduğunda, üretim ortamına - karşılık gelen klasör yapısını oluşturmak gerekebilir `Bin` klasöründe `Fiction` klasöründe `Images` klasör ve benzeri.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Tam içeriğini `Styles` klasörü
- Tam içeriğini `Images` klasörü (ve alt klasörlerinde `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Şekil 1, gerekli dosyaları kopyaladıktan sonra FileZilla gösterir. FileZilla dosyaları yerel bilgisayarda sol ve sağ taraftaki uzak bilgisayarda dosyaları görüntüler. Şekil 1, ASP.NET kaynak kodu dosyaları gibi gösterildiği gibi `About.aspx.vb`, yerel bilgisayardaki (geliştirme ortamı) ancak kod dosyaları kullanırken dağıtılması gerekmez çünkü web ana bilgisayar sağlayıcısı (üretim ortamı) kopyalanamadı açık derleme.

> [!NOTE]
> Bunlar yoksayılır gibi üretim sunucusu üzerinde kaynak kodu dosyaları olması, hiçbir zarar yoktur. Kaynak kodu dosyaları üretim sunucusu üzerinde mevcut olsa bile, bunlar Web sitenizi ziyaret edenler için erişilemez ve böylelikle ASP.NET HTTP istekleri kaynak kodu dosyaları için varsayılan olarak engelliyor. (Diğer bir deyişle, bir kullanıcı ziyaret etmek çalışırsa `http://www.yoursite.com/Default.aspx.vb` dosyalar: Bu tür bilgiler veren bir hata sayfası alırlar `.vb` dosyalar - Yasak.)


[![Web ana bilgisayar sağlayıcı Web sunucusunda masaüstünüzden gerekli dosyaları kopyalamak için bir FTP İstemcisi'ni kullanın.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Şekil 1**: Web ana bilgisayar sağlayıcı Web sunucusunda bilgisayarınızı masaüstünden gerekli dosyaları kopyalamak için bir FTP İstemcisi'ni kullanın ([tam boyutlu görüntüyü görmek için tıklatın](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))


Sitenizi dağıttıktan sonra siteyi sınamak için bir dakikanızı ayırın. Bir etki alanı adı satın alınan ve yapılandırılan DNS ayarları, düzgün bir şekilde, etki alanı adınızı girerek sitenizi ziyaret edebilirsiniz. Alternatif olarak, web ana bilgisayar sağlayıcınız yerine, siteniz için bir URL ile sağladığınız, görünür gibi *accountname*. *webhostprovider*.com veya *webhostprovider*.com /*accountname*. Örneğin, ASP.NET indirim hesabımdaki URL'si şudur: `http://httpruntime.web703.discountasp.net`.

Şekil 2 dağıtılan Kitap incelemeleri konumu gösterir. Miyim indirim ASP görüntüleyen olduğunu unutmayın. NET sunucuları, `http://httpruntime.web703.discountasp.net`. Bu anda bir Internet bağlantısı olan herkes sitemin görüntüleyebiliyordu! Biz beklediğiniz gibi site görünümünü ve yalnızca geliştirme ortamında test ederken yaptığınız gibi davranır.

> [!NOTE]
> Uygulamanızı görüntüleme alırken emin olmak için biraz hata alırsanız doğru dosya kümesi dağıtıldı. Ardından, her nedene sorun seçeceğine ortaya görmek için hata iletisini inceleyin. Web ana bilgisayar şirket Yardım Masası için açın veya için uygun bir foruma sorunuzu gönderin [ASP.NET forumları](https://forums.asp.net/).


[![Internet bağlantısı olan herkes Kitap incelemeleri Site artık erişilebilir durumdadır.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Şekil 2**: Kitap incelemeleri Site erişilebilir artık bir Internet bağlantısı olan herkes ([tam boyutlu görüntüyü görmek için tıklatın](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Kitap gözden geçirme Web sitesi projesi dağıtma

BookReviewsWSP Web sitesi projesi gibi otomatik derleme kullanan bir ASP.NET Uygulama dağıtırken hiçbir derlenmiş derlemede yoktur `Bin` klasör. Sonuç olarak, web uygulamasının kaynak kodu dosyaları üretim ortamına dağıtılması gerekir. Bu süreçte atalım.

Web uygulaması projesi ile ilk derleme dağıtmadan önce uygulama için doğru olduğu gibi. Bir Web sitesi projesi oluşturma bir bütünleştirilmiş kod oluşturmaz karşın, derleme zamanı hatalarına sayfasında kontrol etmez. Artık bu hataları bulmak için daha iyi yerine Web sitenize bir ziyaretçi sahip bulma bunları sizin için!

Proje başarıyla oluşturduktan sonra FTP istemcisi aşağıdaki dosyaları web ana bilgisayar Sağlayıcınızdaki kök Web sitesi klasörüne kopyalamak için kullanın. Üretim ortamına karşılık gelen klasör yapısını oluşturmanız gerekebilir.

> [!NOTE]
> Proje ancak yine de istediğiniz BookReviewsWSP proje dağıtmayı deneyin BookReviewsWAP zaten dağıttıysanız, tüm web sunucusunda BookReviewsWAP dağıtırken yüklenen dosyaları silin ve dosyalar için BookReviewsWSP dağıtabilirsiniz.


- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- Tam içeriğini `Styles` klasörü
- Tam içeriğini `Images` klasörü (ve alt klasörlerinde `BookCovers`)
- `~/App_Code/BasePage.vb`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.vb`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.vb`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.vb`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.vb`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.vb`

Şekil 3, gerekli dosyaları kopyaladıktan sonra FileZilla gösterilmektedir. Gördüğünüz gibi ASP.NET kaynak kodu dosyaları gibi `About.aspx.vb`, kod dosyaları otomatik kullanırken dağıtılması gerektiği için hem yerel bilgisayar (geliştirme ortamı) hem de web ana bilgisayar Sağlayıcısı'nı (üretim ortamı) yok derleme.


[![Web ana bilgisayar sağlayıcı Web sunucusunda masaüstünüzden gerekli dosyaları kopyalamak için bir FTP İstemcisi'ni kullanın](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Şekil 3**: Web ana bilgisayar sağlayıcı Web sunucusunda bilgisayarınızı masaüstünden gerekli dosyaları kopyalamak için bir FTP İstemcisi'ni kullanın ([tam boyutlu görüntüyü görmek için tıklatın](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))


Kullanıcı deneyimi, uygulamanın derleme modeli tarafından etkilenmez. Aynı ASP.NET sayfaları erişebilir ve Görünüm ve Web uygulaması proje modeli veya Web sitesi proje modeli kullanarak Web sitesi oluşturuldu olup aynı şekilde davranır.

## <a name="updating-a-web-application-on-production"></a>Bir üretim Web uygulaması güncelleştiriliyor

Web uygulaması geliştirme ve dağıtım tek seferlik bir işlem değildir. Örneğin, kitap gözden geçirme Web sitesi oluştururken miyim çeşitli sayfalar yerleşik ve eşlik eden kod kişisel bilgisayarımda (geliştirme ortamı) yazdığınız. Böylece diğerleri sitesini ziyaret edin ve incelemelerim okuma kararlı bir belirli duruma ulaştıktan sonra miyim Uygulamam dağıtıldı. Ancak, dağıtım bu sitede my geliştirme sonunu işaretlemek değil. Miyim daha fazla Kitap incelemeleri ekleyebilir veya my ziyaretçiler oranı Books izin verme gibi yeni özellikler, uygulama veya kendi yorum bırakabilir. Bu geliştirmeler, geliştirme ortamınızda geliştirilen ve tamamlandığında dağıtılması gerekir. Bu nedenle, geliştirme ve dağıtım, döngüsel. Bir uygulama geliştirdiğinizde ve ardından dağıtın. Site canlıyken ve üretimde yeni özellikler eklenir ve uygulamayı yeniden dağıtmadan BIOS'ta zamanla düzeltilen hatalar. Vb. ve benzeri.

Yalnızca yeni ve değiştirilen dosyaları kopyalamak için gereken bir web uygulaması yeniden dağıtırken bekleyebileceğiniz gibi. Değiştirilmemiş sayfaları yeniden dağıtmanız gerekmez veya sunucu veya istemci tarafı destek dosyaları (olmasına karşın bir zararı böylece).

> [!NOTE]
> Açık derlemesini projeye yeni bir ASP.NET sayfasına ekleyin veya kodla ilgili değişiklik zaman derlemesinde güncelleştirmeleri projenizi yeniden derleyin gerektiğini kullanırken akılda tutulması gereken bir şey `Bin` klasör. Sonuç olarak, üretim (yanı sıra diğer yeni ve güncelleştirilmiş içerik) üzerinde bir web uygulaması güncelleştirilirken, üretim bu güncelleştirilmiş derlemeyi kopyalamak gerekir.


Ayrıca değişiklikleri için olduğunu anlamanız `Web.config` veya dosyaları `Bin` dizin durdurur ve Web sitesinin uygulama havuzu yeniden başlatır. Oturum durumunu kullanarak depolanıyorsa `InProc` modu (varsayılan) sonra sitenizin ziyaretçileri kaybolmasına neden olacak, oturum durumu bu anahtar dosyaları değiştirdiğinizde. Bu durumu önlemek için oturumu kullanarak depolamanız `StateServer` veya `SQLServer` modları. Bu konu hakkında daha fazla bilgi için okumaya devam edin [oturum durumu modu](https://msdn.microsoft.com/library/ms178586.aspx).

Son olarak, uygulamanın yeniden dağıtıma herhangi bir yere birkaç saniyeden sayısı ve üretim ortamına kopyalanması gereken dosyaların boyutuna bağlı olarak birkaç dakika sürebilir aklınızda bulundurun. Bu süre boyunca, hataları veya garip davranış sitenizi ziyaret eden kullanıcılara karşılaşabilirsiniz. "Tüm uygulamanızın adlı sayfanın ekleyerek kapatabilirsiniz" `App_Offline.htm` kullanıcılarınıza açıklayan, uygulamanızın kök dizinine site Bakım (veya ne olursa olsun) çalışmıyor ve olacak olması yedekleme kısa bir süre. Zaman `App_Offline.htm` dosya varsa, ASP.NET çalışma zamanı gelen tüm istekleri için bu sayfayı yeniden yönlendirir.

## <a name="summary"></a>Özet

Bir web uygulamasını dağıtma, gerekli dosyaları geliştirme ortamından üretim ortamına kopyalamayı kapsar. Tarafından dosyaları ağ üzerinden aktarılan en yaygın Dosya Aktarım Protokolü (FTP) yöntemdir ve çoğu web ana bilgisayar sağlayıcıları web sunucularından FTP erişimi destekler. Bu öğreticide web sunucusuna, gerekli dosyaları dağıtmak için FTP istemcisi kullanmayı gördük. Dağıtıldıktan sonra Web sitesi herkes tarafından bir bağlantıyla Internet'e ziyaret edilebilir!

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Uygulama\_Offline.htm ve "IE kolay hatalar" özelliği etrafında çalışma](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Oturum durumu modu](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Önceki](determining-what-files-need-to-be-deployed-vb.md)
> [İleri](deploying-your-site-using-visual-studio-vb.md)
