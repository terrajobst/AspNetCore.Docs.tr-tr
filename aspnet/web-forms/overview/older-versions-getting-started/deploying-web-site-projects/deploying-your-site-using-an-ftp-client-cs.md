---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: "Bir FTP istemcisi (C#) kullanarak sitenizi dağıtma | Microsoft Docs"
author: rick-anderson
description: "Bir ASP.NET uygulamasını dağıtmak için en basit yolu el ile gerekli dosyaları geliştirme ortamından üretim ortamına kopyalamaktır. Düzeltmeyi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e4af20fa1fecd1f363e979023b41203096d64ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="deploying-your-site-using-an-ftp-client-c"></a>Bir FTP istemcisi (C#) kullanarak sitenizi dağıtma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> Bir ASP.NET uygulamasını dağıtmak için en basit yolu el ile gerekli dosyaları geliştirme ortamından üretim ortamına kopyalamaktır. Bu öğretici için web ana makine sağlayıcısı masaüstünüzden dosyaları almak için bir FTP istemcisi kullanmayı gösterir.


## <a name="introduction"></a>Giriş

Önceki öğretici ASP.NET sayfaları, bir ana sayfa, özel bir taban sayıda oluşan basit bir kitap gözden geçirme ASP.NET web uygulaması sunulan `Page` , görüntüleri bir dizi sınıf ve üç CSS stil sayfaları. Biz şimdi bu noktada uygulamanın herkes için Internet bağlantısı olan erişilebilir bir web ana makine sağlayıcısı bu uygulamayı dağıtmak hazırsınız!


Bizim tartışmalara gelen [ *belirleme dosyaları gerekenler dağıtılacağını için* ](determining-what-files-need-to-be-deployed-cs.md) Öğreticisi, biliyoruz dosyaları için web ana makine sağlayıcısı kopyalanacak gerekir. (Uygulamanızın açıkça veya otomatik olarak derlenmiş üzerinde hangi dosyalar kopyalanır geri çağırma bağlıdır.) Ancak nasıl biz dosyaları geliştirme ortamı (Masaüstü bizim) üretim ortamına (web ana bilgisayar sağlayıcısı tarafından yönetilen web sunucusu) kadar almak? [ **F** il **T** aktarma **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) dosyaları bir makineden diğerine bir ağ üzerinden kopyalamak için yaygın olarak kullanılan bir protokoldür. FrontPage Server Extensions (FPSE) başka bir seçenektir. Bu öğretici geliştirme ortamından üretim ortamına gerekli dosyaları dağıtmak için tek başına FTP istemci yazılımı kullanımı üzerine odaklanmıştır.

> [!NOTE]
> Visual Studio Araçları aracılığıyla FTP Yayımlama Web siteleri için içerir; sonraki öğreticide FPSE, kullanan araçları göz yanı sıra, bu araçları ele alınmıştır.


İhtiyacımız FTP kullanarak dosyaları kopyalamak için bir *FTP istemcisi* geliştirme ortamı. Bir FTP istemcisi çalıştıran bir bilgisayara yüklü olduğu bilgisayardan dosyaları kopyalamak için tasarlanmış bir uygulamadır bir *FTP sunucusu*. (Çoğu gibi web ana bilgisayar sağlayıcınız FTP ile dosya aktarımı destekliyorsa, ardından var. kendi web sunucuları üzerinde çalışan bir FTP sunucusu) FTP istemci uygulamaları birkaç kullanılabilir. Web tarayıcınız bir FTP istemcisi bile çift olabilir. My sık kullandığınız FTP istemcisinden ve ı kullanacağınız Bu öğreticide, bir [FileZilla](http://filezilla-project.org/), Windows, Linux ve Mac bilgisayarları için kullanılabilir bir ücretsiz, açık kaynaklı FTP istemcisi. Herhangi bir FTP istemcisi, ancak bunu kullanımında en uygun olanını ne olursa olsun istemci kullanmak boş çalışır.

İzliyorsanız, önce web ana makine sağlayıcısı ile bir hesabı oluşturmak için Bu öğreticide ya da sonraki olanları tamamlayabilirsiniz. Önceki öğreticide belirtildiği gibi bir web ana bilgisayar sağlayıcısı şirketler gaggle Fiyatlar, özellikleri ve hizmet kalitesi geniş bir yelpazesini ile vardır. Bu öğretici seri için ı kullanacağınız [indirim ASP.NET](http://discountasp.net) my web ana bilgisayarı olarak sağlayıcısı izleyin, ancak, birlikte herhangi bir web ana bilgisayar sağlayıcısına sitenizi geliştirilen içinde ASP.NET sürüm destekledikleri sürece. (Bu öğreticileri ASP.NET 3.5 kullanılarak oluşturulan.) Biz dosyaları için web ana makine sağlayıcısı kopyalayarak Ayrıca, Bu öğretici ve olanları gelecekte FTP kullanarak web ana makine sağlayıcısı web sunucularına FTP erişimini desteklediğinden emin kesinlik temelli demektir. Neredeyse tüm web ana bilgisayar sağlayıcıları bu özelliği sunar, ancak kaydolmadan önce yeniden denetlemeniz gerekir.

## <a name="deploying-the-book-review-web-application-project"></a>Kitap gözden geçirme Web uygulama projesi dağıtma

Geri çağırma kitap gözden geçirme web uygulamasının iki sürümü vardır: biri Web uygulama projesi modeli (BookReviewsWAP) ve diğer Web sitesi projesini modelini (BookReviewsWSP) kullanarak kullanılarak uygulanan. Proje türü sitenin otomatik olarak veya açıkça derlenir ve bu derleme modeli dosyaları dağıtılması için gerekenler belirler olup olmadığını etkiler. Sonuç olarak, BookReviewsWAP ve BookReviewsWSP projeleri ayrı ayrı dağıtma BookReviewsWAP ile başlayarak inceleyeceğiz. Siz bunu zaten yapmadıysanız, bu iki ASP.NET uygulamaları indirmek için bir dakikanızı ayırın.

Giderek BookReviewsWAP proje başlatma `BookReviewsWAP` klasörü ve çift `BookReviewsWAP.sln` dosya. Proje dağıtmadan önce bu değişiklikler kaynak koduna derlenmiş derlemesinde bulunan emin olmak için yapı önemlidir. Projeyi derlemek için derleme menüsüne gidin ve yapı BookReviewsWAP menü seçeneğini belirleyin. Bu projenin kaynak kodda tek bir derleme derler `BookReviewsWAP.dll`, içinde yerleştirilen `Bin` klasör.

Biz artık gerekli dosyaları dağıtmak hazırsınız! FTP istemcisi başlatın ve web ana bilgisayar Sağlayıcınızdaki web sunucusuna bağlanın. (Bir web sitesi barındırma şirketi ile oturum açtığınızda, FTP sunucusuna bağlanma hakkında bilgi e; bu FTP sunucusu olarak bir kullanıcı adı ve parola adresini içerir.)

Aşağıdaki dosyaları masaüstünüzden web ana bilgisayar Sağlayıcınızdaki kök Web sitesi klasörüne kopyalayın. Web sunucusuna FTP web adresindeki sağlayıcı barındırıyorsanız, kök Web sitesi dizininde olasıdır. Ancak, bazı web ana bilgisayar sağlayıcıları adlı bir alt olan `www` veya `wwwroot` , Web sitesi dosyalarınızı için kök klasör olarak görev yapar. Son olarak, dosyaların FTPing olduğunda, üretim ortamına - karşılık gelen klasör yapısını oluşturmak gerekebilir `Bin` klasörünü `Fiction` klasörü `Images` klasörü ve benzeri.

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

Şekil 1, gerekli dosyaları kopyaladıktan sonra FileZilla gösterir. FileZilla dosyaları yerel bilgisayarda sol ve sağ taraftaki uzak bilgisayarda dosyalarını görüntüler. Şekil 1, ASP.NET kaynak kodu dosyaları gibi gösterildiği gibi `About.aspx.cs`, yerel bilgisayardaki (geliştirme ortamı) ancak kod dosyaları kullanırken dağıtılması gerekmez çünkü web ana makine sağlayıcısı (üretim ortamı) kopyalanamadı açık derleme.

> [!NOTE]
> Göz ardı edilir olarak üretim sunucusu üzerinde kaynak kodu dosyaları sahip içinde hiçbir zarar yoktur. Kaynak kodu dosyaları üretim sunucusunda mevcut olsa bile, Web sitenizin ziyaretçileri için erişilemez; böylece ASP.NET HTTP isteklerini kaynak kodu dosyaları için varsayılan olarak engelliyor. (Diğer bir deyişle, bir kullanıcı ziyaret etmek çalışırsa `http://www.yoursite.com/Default.aspx.cs` bu tür dosyaları -, açıklayan bir hata sayfası alırsınız `.cs` dosyalar - Yasak.)


[![Web ana makine sağlayıcısı Web sunucusuna masaüstünüzden gerekli dosyaları kopyalamak için bir FTP istemcisi kullanın](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**Şekil 1**: Web ana makine sağlayıcısı Web sunucusuna bilgisayarınızı masaüstünden gerekli dosyaları kopyalamak için bir FTP istemcisi kullanın ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))


Sitenizi dağıttıktan sonra site test etmek için bir dakikanızı ayırın. Bir etki alanı adı satın alınan ve DNS ayarlarını yapılandırılmış varsa düzgün bir şekilde, etki alanı adınızı girerek, sitenizi ziyaret edebilirsiniz. Alternatif olarak, web ana makine sağlayıcısı, Web sitenize bir URL ile sağladığınız hangi görünür aşağıdakine benzer *accountname*. *webhostprovider*.com veya *webhostprovider*.com /*accountname*. Örneğin, indirim ASP.NET hesabımdaki URL'sidir: `http://httpruntime.web703.discountasp.net`.

Şekil 2 dağıtılan Kitap incelemeleri site gösterir. I indirim ASP görüntülemesini olduğunu unutmayın. NET'in sunucuları adresindeki `http://httpruntime.web703.discountasp.net`. Bu anda Internet bağlantısı olan herhangi bir kişi benim Web sitesi görüntüleyebilir! Biz beklediğiniz gibi site arar ve yalnızca geliştirme ortamında test edilirken yaptığınız gibi davranır.

> [!NOTE]
> Uygulamanızı görüntüleme zaman biraz emin olmak için bir hata alırsanız, dosyaların doğru kümesi dağıtıldı. Ardından, tüm ipuçları sorun konusunda ortaya görmek için hata iletisine bakın. Web ana bilgisayar şirketinizin Yardım Masası için kapatma veya sorunuzu uygun Forumu post [ASP.NET forumları](https://forums.asp.net/).


[![Kitap incelemeleri Site şimdi bir Internet bağlantısı olan bir kişiye erişilebilen](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**Şekil 2**: Kitap incelemeleri sitesidir şimdi erişilebilir bir Internet bağlantısı olan bir kişiye ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Kitap gözden geçirme Web sitesi projesini dağıtma

BookReviewsWSP Web sitesi projesini gibi otomatik derleme kullanan bir ASP.NET Uygulama dağıtırken, derlenmiş derlemesi bulunmuyor `Bin` klasör. Sonuç olarak, web uygulamasının kaynak kodu dosyaları üretim ortamına dağıtılması gerekir. Şimdi bu işlemde size yol.

Web uygulaması projesi ile ilk yapı dağıtmadan önce uygulama akıllıca olduğu gibi. Bir Web sitesi proje derleme bütünleştirilmiş oluşturmaz, ancak sayfasında derleme zamanı hataları denetleyin. Şimdi bu hataları bulmak için daha iyi yerine ziyaretçisi sitenize sahip Bul bunları sizin için!

Proje başarıyla oluşturuncaya sonra aşağıdaki dosyaları web ana bilgisayar Sağlayıcınızdaki kök Web sitesi klasörüne kopyalamak için FTP istemcisi kullanın. Üretim ortamına karşılık gelen klasör yapısını oluşturmanız gerekebilir.

> [!NOTE]
> Proje ancak yine istediğiniz BookReviewsWSP proje dağıtmayı deneyin BookReviewsWAP zaten dağıttıysanız, web sunucusu BookReviewsWAP dağıtırken yüklenen dosyaların tümünü silmeniz ve ardından dosyaları için BookReviewsWSP dağıtın.


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- Tam içeriğini `Styles` klasörü
- Tam içeriğini `Images` klasörü (ve alt klasörlerinde `BookCovers`)
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

Şekil 3, gerekli dosyaları kopyaladıktan sonra FileZilla gösterir. Gördüğünüz gibi ASP.NET kaynak kodu dosyaları gibi `About.aspx.cs`, kod dosyaları otomatik kullanırken dağıtılması gerekiyor olduğundan hem yerel bilgisayar (geliştirme ortamı) hem de web ana makine sağlayıcısı (üretim ortamı) mevcut değil derleme.


[![Web ana makine sağlayıcısı Web sunucusuna masaüstünüzden gerekli dosyaları kopyalamak için bir FTP istemcisi kullanın](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**Şekil 3**: Web ana makine sağlayıcısı Web sunucusuna bilgisayarınızı masaüstünden gerekli dosyaları kopyalamak için bir FTP istemcisi kullanın ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))


Kullanıcı deneyimi uygulamanın derleme modeli tarafından etkilenmez. Aynı ASP.NET sayfaları erişilebilir ve bunlar arayın ve Web sitesi Web uygulama projesi modeli veya Web sitesi projesini modeli kullanılarak oluşturulmuş olup olmadığını aynı şekilde davranır.

### <a name="updating-a-web-application-on-production"></a>Üretim üzerinde Web uygulaması güncelleştiriliyor

Web uygulaması geliştirme ve dağıtım tek seferlik bir işlem değildir. Örneğin, kitap gözden geçirme Web sitesi oluştururken ı çeşitli sayfalar yerleşik ve eşlik eden kod kişisel bilgisayarımda (geliştirme ortamı) yazıldı. Böylece diğerleri sitesini ziyaret edin ve my değerlendirmeleri bir belirli kararlı bir duruma eriştikten sonra ı Uygulamam dağıtıldı. Ancak dağıtım bu sitede my geliştirme sonuna işaretlemez. I daha fazla Kitap incelemeleri ekleyebilir veya oranı books my ziyaretçileri izin verme gibi yeni özellikler, uygulama veya kendi yorum bırakın. Bu tür geliştirmeler geliştirme ortamında geliştirilen ve tamamlandığında dağıtılması gerekir. Bu nedenle, geliştirme ve dağıtım, döngüsel. Uygulama geliştirme ve ardından dağıtın. Site Canlı olsa da hem de üretim yeni özellikler eklenir ve uygulamayı yeniden dağıtılması gerekir zamanla düzeltilen hatalar. Vb. ve benzeri.

Yalnızca yeni ve değiştirilen dosyaları kopyalamak için gereken bir web uygulaması yeniden dağıtırken bekleyebilirsiniz gibi. Değişmeden sayfaları yeniden dağıtmak için gerek yoktur veya (böylece hiçbir zarar olmasa) sunucu veya istemci tarafı dosyalarını destekler.

> [!NOTE]
> Açık derlemesini kullanarak yeni bir ASP.NET sayfası projeye ekleyin ya da kod ilgili değişiklik zaman derlemede güncelleştirmeleri projenizi yeniden derleyin gerektiğini olduğunda göz önünde bulundurmanız gereken bir şey `Bin` klasör. Sonuç olarak, üretim (yanı sıra diğer yeni ve güncelleştirilmiş içerik) üzerinde web uygulaması güncelleştirilirken bu güncelleştirilmiş derlemeyi üretim kopyalamak gerekir.


Ayrıca değişiklikler için olduğunu anlamak `Web.config` veya dosyalarında `Bin` dizin durdurur ve Web sitesinin uygulama havuzu yeniden başlatır. Oturum durumu kullanarak depolanıyorsa `InProc` modunda (varsayılan) sonra sitenizin ziyaretçileri kaybolmasına neden olacak, oturum durumu bu anahtar dosyaları değiştirdiğinizde. Bu durumu önlemek için oturumu kullanarak depolamayı düşünün `StateServer` veya `SQLServer` modları. Bu konu hakkında daha fazla bilgi için okumaya devam [oturum durumu modu](https://msdn.microsoft.com/en-us/library/ms178586.aspx).

Son olarak, uygulamanın yeniden dağıtma herhangi bir yere birkaç saniye sayısını ve üretim ortamına kopyalanması gereken dosyaların boyutu bağlı olarak birkaç dakika sürebilir aklınızda bulundurun. Bu süre boyunca, hataları veya garip davranış sitenizi ziyaret eden kullanıcıların karşılaşabilirsiniz. "Tüm uygulamanızı adlı bir sayfaya ekleyerek kapatabilirsiniz" `App_Offline.htm` kullanıcılarınıza açıklar, uygulamanızın kök dizinine site Bakım (veya ne olursa olsun) çalışmıyor ve olacak olması yedekleme kısa süre içinde. Zaman `App_Offline.htm` dosya varsa, ASP.NET çalışma zamanı gelen tüm istekleri için bu sayfayı yeniden yönlendirir.

## <a name="summary"></a>Özet

Bir web uygulaması dağıtma, gerekli dosyaları geliştirme ortamından üretim ortamına kopyalama kapsar. Dosya Aktarım Protokolü (FTP) dosyaları bir ağ üzerinden aktarılır en yaygın anlamına gelir ve çoğu web ana bilgisayar sağlayıcıları web sunucularına FTP erişimi destekler. Bu öğreticide gerekli dosyaların web sunucusuna dağıtmak için bir FTP istemcisi kullanmayı gördünüz. Uygulama dağıtıldıktan sonra Web sitesi herkes tarafından bir bağlantıyla Internet'e ziyaret edilebilir!

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Uygulama\_Offline.htm ve "IE kolay hatalar" özelliği geçici çalışma](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Oturum durumu modu](https://msdn.microsoft.com/en-us/library/ms178586.aspx)

>[!div class="step-by-step"]
[Önceki](determining-what-files-need-to-be-deployed-cs.md)
[sonraki](deploying-your-site-using-visual-studio-cs.md)
