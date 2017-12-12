---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: "ASP.NET Web sayfaları sunarak - WebMatrix kullanarak bir Site yayımlama | Microsoft Docs"
author: tfitzmac
description: "Bu öğretici ASP.NET Web sayfaları ve Microsoft WebMatrix tanıtır öğretici kümesi son yüklemede ' dir. Sitenizi t yayımlamak nasıl ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 1e718c92a2f94df50fcf7af6859917746a4982ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>ASP.NET Web sayfaları sunarak - WebMatrix kullanarak bir Site yayımlama
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici ASP.NET Web sayfaları ve Microsoft WebMatrix tanıtır öğretici kümesi son yüklemede ' dir. Diğerleri ile çalışabilmeniz için Internet'e, sitenizi yayımlama açıklanır. Seri aracılığıyla tamamladığınızdan varsayar [tutarlı aramak için ASP.NET Web Pages sitelerinde oluşturma](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Site kullanarak yayımlamayı öğreneceksiniz:
> 
> - Microsoft Azure
> - Web barındırma şirketi


## <a name="about-publishing-your-site"></a>Sitenizi yayımlama hakkında

Şimdiye kadar sayfalarınızı test de dahil olmak üzere yerel bir bilgisayardaki tüm iş yaptık. Çalıştırmak için*.cshtml* sayfaları, WebMatrix, yani IIS Express yerleşik web sunucusu kullandığınız. Ancak hiç kimsenin dışında oluşturduğunuz site Elbette görebilirsiniz. Başkalarının sitenizle çalışmak için Internet'e yayımlamanız gerekir.

Bir ortak web sunucusuna erişim zaten yoksa, hesabınız zorunda yayımlama anlamına gelir bir *bulut platformu* veya *barındırma sağlayıcısına*. Microsoft Azure gibi bir bulut platformu uygulamalarınız için isteğe bağlı altyapı sağlar. Bir barındırma sağlayıcısı, genel olarak erişilebilir web sunucuları sahibi ve, kira şirket siteniz için boşluk ' dir. Barındırma planları ay birkaç dolar çalıştırılabilen (veya hatta ücretsiz) yüzlerce yüksek hacimli ticari Web siteleri için ayda bir ABD doları için küçük siteler için.

> [!NOTE]
> Internet servis evde almak için kullandığınız Internet servis sağlayıcısı (ISS) üzerinden ortak web sunucusuna erişim olabilir. Ancak, barındırma sağlayıcınız ASP.NET Web sayfaları desteklemesi gerekir. Birçok ISS yoktur, ancak bunu her zaman denetleme değerindedir.


Bu öğreticide, nasıl yayımlanacağını genel bakış sunuyoruz. İşlem her barındırma sağlayıcısı için biraz farklıdır olduğundan tam Ayrıntılar için her şeyi, sağlamak mümkün değil. Ancak işleminin nasıl çalıştığı, iyi bir fikir elde edersiniz.

Bu öğretici dört bölüm içerir:

1. [Varsayılan sayfa ayarlama](#defaultpage)
2. Yayımlama (aşağıdakilerden birini seçin)  
 a. [Siteniz için Microsoft Azure yayımlama](#azure)  
 b. [Şirket barındıran bir Web sitenizi yayımlama](#host)
3. [Canlı Site güncelleştiriliyor: yeniden yayımlama](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Varsayılan sayfa ayarlama

Bir kullanıcı web siteniz için temel adres gittiğinde, siteniz için varsayılan sayfa kullanıcıya görüntülenir. Default.htm www.contoso.com sırasında site için varsayılan sayfa olarak ayarlandığında, örneğin, ardından giderek **www.contoso.com** giderek aynı **www.contoso.com/Default.htm**.

Siteniz şu anda kullandığı **Default.cshtml** varsayılan sayfa olarak. Bu sayfa için varsayılan sayfanızı sorun yoktur, ancak boş bir sayfa görüntüleyecektir şekilde Bu öğreticide, herhangi bir içerik için bu sayfayı eklemediniz. Default.cshtml açın ve içeriğini aşağıdaki kodla değiştirin.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Artık, sitenizi yayımlama için hazırdır. İlk olarak, site Azure'a dağıtma ve bir web barındırma şirketi dağıtma görürsünüz. Web sitenizi ve için iki seçenek çalışır, yalnızca dağıtım seçeneklerinden birini gerçekleştirmeniz gerekir.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Siteniz için Microsoft Azure yayımlama

Bu öğretici ilk sitenizi Microsoft Azure'a dağıtmak nasıl yapacağınızı gösterir. Bir Microsoft hesabıyla oturum açarak, Azure üzerinde en fazla 10 ücretsiz siteleri oluşturabilirsiniz. Bu ücretsiz sitelere sitelerinizi sınamak için kolay bir yol sağlamak. Bu örnek site daha sonra tüm ücretsiz sitelerinizin kullanarak önlemek için her zaman silebilirsiniz. Yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

WebMatrix Şeritte tıklatın **Yayımla** düğmesi.

!['WebMatrix Şeritte Yayımla düğmesi'](publishing/_static/image1.png)

**Sitenizin yayımlama** iletişim kutusu görüntülenir. Microsoft hesabınızda oturum açmanızdan değil, iletişim kutusu içerecek bir **Azure ile çalışmaya başlama** bağlantı. Bu bağlantıyı tıklatın.

![Sitenizi yayımlayın](publishing/_static/image2.png)

Bir Microsoft hesabı ile oturum açmanızdan değil, oturum açmak için Fırsat yeniden verilir. Bir Microsoft hesabına sitenizi Azure yayımlama oturum gerekir.

![Oturum Aç](publishing/_static/image3.png)

Microsoft hesabınızda oturum açtıktan sonra iletişim kutusu Azure üzerinde yeni bir site oluşturmak veya var olan Azure sitelerinizde birine bağlanmak için bağlantılar içerir.

![Yeni Site oluşturma](publishing/_static/image4.png)

Seçin **yeni bir site oluştur**.

Projenizi adlandırırsanız **WebPagesMovies**, siteniz için varsayılan adı **webpagesmovies.azurewebsites.net**. Bu varsayılan adı büyük olasılıkla kullanılamıyor, kırmızı bir ünlem işareti tarafından belirtildiği şekilde.

![Varsayılan Web sitesi adı](publishing/_static/image5.png)

Kullanılabilir olan bir site adı değiştirin ve konumunuz yakın bir konum seçin.

![Değiştirilen site adı](publishing/_static/image6.png)

**Tamam**'ı tıklatın.

WebMatrix performss sunucu sitenizle uyumlu olup olmadığını belirlemek için bir test.

![Uyumluluk testi](publishing/_static/image7.png)

Seçin **devam**.

Uyumluluk test sonuçlarını görüntülenir.

![Uyumluluk sonucu](publishing/_static/image8.png)

Seçin **devam**.

WebMatrix, dosyalar ve siteye yayımlanan veritabanları görüntüler. Bu sitenin yayımlama ilk kez olduğuna göre tüm dosyalar listelenmektedir. Yayımlanmaya hazır olmayan bir dosya kutusunun işaretini kaldırın. Sonraki yayınlarda yalnızca değişen dosyaları görüntülenir. Bkz: [Canlı Site güncelleştiriliyor: yeniden yayımlanması](#update).

![Yayımlama önizlemesi](publishing/_static/image9.png)

Seçin **devam**.

Site için Azure dağıtıldıktan sonra dağıtımın tamamlandığını bir ileti görüntülenir.

![tam yayımlama](publishing/_static/image10.png)

Site ve veritabanı için Azure yayımlandı ve ortak kullanıma sunulmuştur. Yayımlama tamamladı ve dağıtılan siteniz şimdi göreceksiniz belirten iletiyi bağlantıya tıklayın. Siz veya Internet erişimi olan herkes, ekleyin veya veritabanındaki kayıtları değiştirin.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Şirket barındıran bir Web sitenizi yayımlama

İçin Azure yayımlama değil karar verirseniz, bunun yerine şirket barındıran bir web sitenizi yayımlayabilirsiniz.

Tıklatın **web barındırma Bul** bağlantı.

![Yayımlama Ayarları iletişim kutusunda 'web barındırma Bul' düğmesini](publishing/_static/image12.png)

ASP.NET'i destekleyen barındırma hizmeti sağlayıcıları listeler Microsoft sitesinde bir sayfasına gidin.

![Barındırma hizmeti sağlayıcıları listeler Microsoft sitesinde sayfası](publishing/_static/image13.png)

Belli ki, tam olarak uzun vadeli gerektirebilir barındırma hangi özelliklerin biliyorsunuz zor olabilir. Birkaç göz önünde bulundurmanız gerekenler şunlardır:

- WebPagesMovies site amaçları doğrultusunda, genellikle çok maliyetleri SQL Server için ayrı bir eklenti olması gerekmez. Sitenizdeki, kendi içinde bulunan olduğu SQL Server Compact Edition, kullanmakta olduğunuz. Ancak, bunu bazı gelecekteki Web sitesi iş için SQL Server erişimi gerekebilir. Olabilirsiniz düşünüyorsanız, SQL Server özelliği daha sonra ekleyebilirsiniz emin olun.
- Barındırma sağlayıcısı Web dağıtımı yayımlama protokolünü destekleyip desteklemediğini kontrol edin. FTP protokolünü kullanarak yayımlayabilirsiniz, ancak Web dağıtımı kullanabilmeniz daha uygundur.

Bazı siteler ücretsiz bir deneme süresi sunar. Ücretsiz deneme yayımlama denemek için iyi bir yoldur ve basılı barındırma hala denemeler WebMatrix ve ASP.NET Web sayfaları.

İstediğiniz bir tane seçin. Bu öğreticide, öğretici oluşturma olsa da, bu şirket için birkaç ay ücretsiz bir siteyi barındıracak kişilere bir yükseltme sahip olduğundan DiscountASP.NET, seçtik.

> [!NOTE]
> Bu öğretici için bir barındırma sağlayıcısına bizim seçimi o şirket bir onay diğer yorumlanacağını döndürmemelidir. Ancak biz bir çizim için çekme gerekiyordu ve yayımlama için ASP.NET Web sayfaları ve Web dağıtımı protokolünü destekleyen birçok şirket, DiscountASP.NET biridir.


Genellikle, barındırma sağlayıcı ile oturum açtığınız sonra şirket, bir kullanıcı adı ve parola, web sunucusu ve benzeri URL'sini içeren bir e-posta gönderir. Barındırma şirketi Web dağıtımı protokolünü destekliyorsa, içeren bir dosyayı yayımlama ayarları veya, bir indirmenizi sağlar gönderebilir. Yayımlama ayarları dosyası işlemi sizin için basitleştirir.

Oturum açtığınız ve yayımlamaya hazır olduğunuzda tıklatın **Yayımla** WebMatrix düğmesini. **Yayımlama ayarları** iletişim kutusu görüntülenir.

Barındırma sağlayıcısı yayımlama ayarları dosyası gönderirse tıklatın **yayımlama ayarlarını içeri aktarmak** bağlamak ve dosyayı içeri aktarın. Yayımlama ayarları dosyası yoksa, barındırma şirket e-posta ile gönderilen değerler kullanarak alanları doldurun. İşte **yayımlama ayarları** iletişim kutusu, ne zaman bitirdiniz gibi görünebilir:

!['Yayımlama Ayarları' iletişim kutusuna doldurulmuş yayımlama ayarları](publishing/_static/image14.png)

Tıklatın **bağlantısı doğrulama**. Her şeyi Tamam olup olmadığını, iletişim kutusunu raporlarını **başarıyla bağlandı**, yani barındırma sağlayıcısının sunucusu ile iletişim kurabilir.

![Başarılı olursa iletiyi yayımlama ayarları doğru](publishing/_static/image15.png)

Bir sorun varsa, WebMatrix sorunun ne olduğunu bildirmek için en iyi yapar:

![Bir sorun olduğunda hata iletisi yayımlama ayarları](publishing/_static/image16.png)

Tıklatın **kaydetmek** ayarlarınızı kaydetmek için. WebMatrix, bunu doğru şekilde barındırma site ile iletişim kurabildiğinden emin olmak için bir test gerçekleştirmek sunar:

![İleti yayımlama işlemini bir testi yapmak için sunumu](publishing/_static/image17.png)

**Evet**'i tıklayın. WebMatrix, barındırma sağlayıcısının bazı örnek dosya karşıya yükleme. Uyumluluk sınama işiniz bittiğinde, WebMatrix sonuçları raporları:

![Yayımlama test sonuçları](publishing/_static/image18.png)

Gitmek için hazır olduğunuzda, bir tane tıklatın **devam** yayımlama işlemi gerçekte başlatmak için. WebMatrix, ne dosyalar sitenizdeki ve ana bilgisayar sunucusunda (şu anda hiçbiri) zaten var ve yayımlama işlemi önizlemesini verir çıkışı rakamları:

![Yayımlama işlemi karşıya yükleyecek hangi dosyaların önizlemesini](publishing/_static/image19.png)

Yayımlamak için dosya listesini gibi oluşturduğunuz web sayfalarını içerir *Movies.cshtml*. Bu liste, dosyaları, yüklediğiniz, Yardımcıları için veritabanınızı SQL Server Compact Edition çalıştırın ve benzeri dosyaları da içerir. Sonuç olarak, ilk yayımlama işlemi olabilmektedir.


              **Devam**'a tıklayın. WebMatrix, dosyalarınızı barındırma sağlayıcısının sunucusuna kopyalar. Tamamlandığında, sonuçları durum çubuğunda bildirilen:

![Yayımlama işlemi başarıyla tamamlandığında durum çubuğu iletisi](publishing/_static/image20.png)

Canlı sitenizi görmek için durum çubuğunda bağlantıya tıklayın. Ekleme *filmler* URL'ye görürsünüz *Movies.cshtml* oluşturduğunuz dosyası:

![Film sayfasını gösteren Canlı site](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Canlı Site güncelleştiriliyor: yeniden yayımlama

(Azure veya şirket barındıran bir web için), sitenizi yayımladıktan sonra iki kopyasını vardır &mdash; bilgisayarınız ve hizmet sağlayıcı sürümünde sürümüne. Site geliştirmeye devam istersiniz (başka bir şey varsa sonraki öğretici kümesinin bir parçası olarak). Bunu yaptığınızda, değişiklikler hizmet sağlayıcısına bilgisayarınızdan kopyalamak için sitenizin yeniden yayımlamanız gerekir. WebMatrix yayımlama işleminde ne sitenizde dosyalar değiştirilmiş belirlemek ve yalnızca bu dosyaları yayımlayın.

Yeniden yayımlanması nasıl çalıştığını görmek için açın *Movies.cshtml* site, bazı küçük değişiklikler yapın ve ardından dosyayı kaydedin. Örneğin, başlık değiştirme `Movies - Updated`.

Tıklatın **Yayımla** düğmesini. WebMatrix, hangi değiştirilir ve bu yayımlayacak dosyaları önizlemesini belirler.

![Yeniden yayımlanması için hazır dosyalar değiştirilmiş gösteren 'Yayımla' iletişim kutusu](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Varsayılan olarak, WebMatrix veritabanınızı yayımlar (*.sdf* dosyası) yalnızca ilk kez site yayımlama. Sitenizi yayımlanır ve kişiler Web sitesi ile etkileşim sonra dinamik site veritabanında genellikle sitenin gerçek veri vardır. Canlı veritabanı ile üzerine çok dikkatli olmak zorunda *.sdf* genellikle yalnızca test verilerini içeren, bilgisayarınızda dosya. İşte bu nedenle uyarı gördüğünüz **yayımlama uzak tüm veritabanlarının üzerine yazılacak**, ve neden onay kutusunu *WebPagesMovies.sdf* varsayılan olarak işaretli değildir.



              **Devam**'a tıklayın. WebMatrix, değişen dosyaları yayımlar ve yayımladığınız ilk kez olduğu gibi bir başarı iletisi gösterilir.

(Tıklayabilirsiniz başarı iletisi bağlantıyı hala gösteriliyorsa) dinamik site gidin ve değişikliğinizi yayınlandığından emin olun.

> [!TIP] 
> 
> **Uzaktan dosyalarını düzenleme**
> 
> Sitenizi değiştirme ve ardından yeniden yayımlanması için alternatif olarak, WebMatrix uzak dosyalara doğrudan düzenleyebilirsiniz. Bu senaryoda, hizmet sağlayıcısında bir dosya açın ve bir kopyasını düzenleyebilmeniz için WebMatrix indirir. Dosyayı kaydedin her zaman, WebMatrix değişiklikleri siteye gönderir.
> 
> Uzak düzenleme, Canlı sitenize değişiklik yapmak için kolay bir yoludur. Ancak, bu şekilde yaptığınız değişiklikler yerel sitenizi dosyaları ile eşitlenmedi. Yerel dosyaları uzak siteyle eşitlemek için Uzak dosyaları indirebilirsiniz. Bu işlem yayımlama, benzer dışında geriye doğru çalışır.
> 
> Biz daha uzaktan düzenleme ve Uzaktan yükleme özellikleri, WebMatrix hakkında burada açıklayın olmaz. Bunlar birden çok kişi farklı bilgisayarlarda aynı sitedeki çalışmak zorunda oldukça yararlıdır. Daha fazla bilgi için bkz: [yayımlamak ve WebMatrix 2 Beta ile bir uzak Site Düzenle](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET WebMatrix ASP.NET Web sayfaları Forumu](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), post için uygun bir yerdir sorular ve yanıtlar alın.

>[!div class="step-by-step"]
[Önceki](layouts.md)
