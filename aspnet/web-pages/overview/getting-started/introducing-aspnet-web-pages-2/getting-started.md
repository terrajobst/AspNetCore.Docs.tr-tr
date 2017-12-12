---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: "ASP.NET Web sayfaları Başlarken - giriş | Microsoft Docs"
author: tfitzmac
description: "WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir. Visual Studio veya Visual Studio kodunu kullanın. Bu kılavuz bir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 615ddc31d0d857e5bf9a7f372b7efcf67d185905
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---getting-started"></a>ASP.NET Web sayfalarını - Başlarken tanıtma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir. Kullanım [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) veya [Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Bu yönerge ve uygulama size genel bir bakış, ASP.NET Web sayfaları (sürüm 2 veya sonrası) ve Razor sözdizimi, dinamik Web siteleri oluşturmak için basit bir çerçeve. Aynı zamanda, WebMatrix, sayfaları ve siteleri oluşturmak için bir araç sunar.
> 
> **Düzey**: ASP.NET Web sayfaları için yeni.  
> **Kabul becerileri**: HTML, temel geçişli stil sayfaları (CSS).
> 
> Kümenin ilk öğreticide öğreneceksiniz:
> 
> - İçin nedir ve ne ASP.NET Web sayfaları teknolojidir.
> - WebMatrix nedir.
> - Her şeyi yükleme.
> - WebMatrix kullanarak Web sitesi oluşturma
>   
> 
> Özellikler/teknolojilerini ele alınan:
> 
> - Microsoft Web Platformu yükleyicisi.
> - WebMatrix.
> - *.cshtml* sayfaları
>   
> 
> Can Pope ilk olarak Bu öğretici yazıldı. Zel FitzMacken için Microsoft WebMatrix 3 güncelleştirildi.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>Bilmeniz gerekenler?

Alışık olduğunuz varsayılarak:

- **HTML**. Hiçbir ayrıntılı uzmanlık gereklidir. HTML açıklayan çalışmaz, ancak biz de karmaşık bir şey kullanmayın. Burada yararlı oldukları düşünüyoruz HTML öğreticileri bağlantılar sağlarız.
- **Geçişli stil sayfaları (CSS)**. Aynı HTML ile.
- **Temel veritabanı fikirleri**. Artık veriler için bir elektronik tablo kullanılan ve sıralanmış ve uzmanlık düzeyini verileri, filtre, biz Bu öğretici kümesi için genellikle varsayılarak.

Biz de temel programlama öğrenme ilgilenen varsayılarak. ASP.NET Web sayfaları adlı C# programlama diliyle kullanın. Programlama, yalnızca bu ilgi içindeki tüm arka plan olması gerekmez. Bir web sayfasında önce tüm JavaScript hiç yazdıysanız arka plan Eskinin açıyor.

Biz yeni programcıları hızlıca Getir sırada ile programlama hakkında bilginiz varsa, bu öğreticiyi tamamlarken belirlenir fark edebilirsiniz Not yavaş taşır. Biz ilk birkaç öğreticileri aldıkça, yine de olacaktır açıklamak için daha az temel programlama ve şeyler bir daha hızlı klibi taşınır.

## <a name="what-do-you-need"></a>Ne ihtiyacınız var?

İşte gerekenler:

- Windows 8, Windows 7, Windows Server 2008 veya Windows Server 2012 çalıştıran bir bilgisayar.
- Canlı bir Internet bağlantısı.
- Yönetici ayrıcalıkları (yükleme işlemi için gereklidir).

## <a name="what-is-aspnet-web-pages"></a>ASP.NET Web sayfaları nedir?

ASP.NET Web sayfaları, dinamik web sayfaları oluşturmak için kullanabileceğiniz bir çerçevedir. Basit bir HTML web sayfası statik; içeriği sayfa sabit HTML biçimlendirmesi tarafından belirlenir. ASP.NET Web sayfaları ile oluşturduğunuz gelenler dinamik sayfalar kod kullanarak hızla, sayfa içeriği oluşturmanızı sağlar.

Dinamik sayfalar, her türlü şey yapmanıza olanak tanır. Bir kullanıcı girişi için form kullanarak isteyin ve sonra ne sayfasını görüntüler veya nasıl göründüğünü değiştirin. Bir kullanıcıdan bilgi alın, bir veritabanına kaydetme ve daha sonra listesi. Sitenizden e-posta gönderebilirsiniz. (Örneğin, bir eşleme hizmeti) Web'de diğer hizmetlerle etkileşim ve bu kaynaklardan bilgi tümleştirmek sayfaları üretir.

## <a name="what-is-webmatrix"></a>WebMatrix nedir?

WebMatrix tümleşen bir web sayfası Düzenleyicisi, veritabanı yardımcı programı, sayfalar ve Internet'e Web sitenizi yayımlama özellikleri test etmek için bir web sunucusu bir araçtır. WebMatrix ücretsiz ve yüklemek kolay ve kullanımı kolay olur. (Ayrıca PHP gibi diğer teknolojileri yanı sıra, yalnızca düz HTML sayfaları için çalışır.)

Gerçekte yok *sahip* WebMatrix ASP.NET Web sayfaları ile çalışmak için kullanılacak. Sayfaları bir metin kullanarak Düzenleyicisi oluşturabilir örneğin ve erişimi olan bir web sunucusu kullanarak sayfaları test. Bu öğreticileri boyunca WebMatrix kullanacak şekilde ancak, WebMatrix tümünü çok, kolaylaştırır.

## <a name="about-these-tutorials"></a>Bu öğreticiler hakkında

Bu öğretici ASP.NET Web sayfalarının nasıl kullanılacağını giriş kümesidir. Bu Tanıtım öğretici kümesinde toplam 9 öğreticileri vardır. Gerçek, profesyonel görünümlü Web siteleri oluşturmak için ASP.NET Web sayfaları yeni başlayan alan bir dizi öğretici kümeleri parçası kullanıcının.

Bu ilk öğreticide, ASP.NET Web sayfaları ile çalışma konusunda temelleri gösteren concentrates ayarlayın. İşiniz bittiğinde, burada bunu sona erer ve daha ayrıntılı olarak, Web sayfaları keşfedin almak ek öğretici kümeleri ile çalışabilirsiniz.

Biz kasıtlı olarak hakkında ayrıntılı açıklamalar kolay gidin. Ve bir şey gösteriyoruz olduğunda, Bu öğretici kümesi için her zaman düşünüyoruz şekilde anlaşılması kolay seçeneğini belirledik. Sonraki öğretici kümeleri daha kapsamlı gidin ve daha verimli ya da daha esnek yaklaşımlar (aynı zamanda daha eğlenceli) gösterir. Ancak bu öğreticiler öncelikle temel kavramları anlamanız gerekir.

Bu özellikler, ASP.NET Web sayfaları yalnızca başladıktan öğretici kümesi kapsar:

- Giriş ve yüklü her şeyi alınıyor. (Makaleyi okuduğunuz öğreticide olmasıdır.)
- ASP.NET Web sayfaları Programlama temelleri.
- Veritabanı oluşturma.
- Oluşturma ve bir kullanıcı giriş formunu işleme.
- Ekleme, güncelleştirme ve veritabanındaki verileri silme.

## <a name="what-will-you-create"></a>Ne oluşturacaksınız?

Bu öğretici ayarlayın ve sonraki olanları istediğiniz filmler listelediğiniz yerde bir Web sitesi Uzayda Döndür. Film girin, bunları düzenleyin ve bunları listesinde görebilirsiniz. Bu öğretici kümesinde oluşturacaksınız sayfaları birkaç şunlardır. Birinci oluşturacağınız sayfa listeleme film gösterir:

![Film listesini gösteren tamamladı film uygulaması](getting-started/_static/image1.png)

Ve yeni film bilgileri sitenize eklemenize olanak sağlayan sayfa şöyledir:

![Film ekleme sayfasını gösteren tamamlanmış film uygulaması](getting-started/_static/image2.png)

Bu sonraki öğretici kümeleri yapı ayarlayın ve resimleri karşıya yükleme, oturum açma kişiler izin vererek, e-posta göndermek ve sosyal medya ile tümleştirme gibi daha fazla işlevsellik ekleyin.

## <a name="see-this-app-running-on-azure"></a>Azure üzerinde çalışan bu uygulamayı bakın

Canlı web uygulaması çalışan tamamlanmış site görmek ister misiniz? Aşağıdaki düğmeyi tıklatarak, uygulama tam sürümü Azure hesabınızda dağıtabilirsiniz.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Bu çözüm Azure'a dağıtmak için bir Azure hesabınız olmalıdır. Bir hesap zaten yoksa, aşağıdaki seçenekler vardır:

- [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

## <a name="installing-everything"></a>Her şeyi yükleme

Her şeyi, Microsoft Web Platformu Yükleyicisi'ni kullanarak yükleyebilirsiniz. Aslında, yükleyici yükleyin ve şey yüklemek için kullanın.

Web sayfaları kullanmak için olması en az olması yüklü SP3 ile Windows XP veya Windows Server 2008 veya üstü.

Üzerinde [Web Pages sayfası](../../../index.md) ASP.NET Web sitesi tıklatın **yükleme**.

![ASP.NET Web sitesi gösteren &quot;Webmatrix'i yükleyin&quot; düğmesi](getting-started/_static/image3.png)

Lisans koşullarını ve WebMatrix yüklemeden önce gizlilik bildirimini kabul istenir.

![yüklemeye başlamak için koşulu kabul](getting-started/_static/image4.png)

Tıklatın **çalıştırmak** yükleme başlatılamadı. (Yükleyici kaydetmek istiyorsanız, tıklatın **kaydetmek** ve kaydettiğiniz bu klasörden yükleyiciyi çalıştırın.)

![](getting-started/_static/image5.png)

Web Platformu yükleyicisi görünür, WebMatrix yüklemek için hazır. **Yükle**'ye tıklatın.

![](getting-started/_static/image6.png)

Yükleme işleminin ne onu bilgisayarınıza yüklemek üzere olduğunu rakamları ve işlemi başlatır. Ne tam olarak yüklü olması gereken bağlı olarak, işlem herhangi bir yere birkaç dakika sonra birkaç dakika sürebilir. Seçin **kabul ediyorum** lisans koşullarını kabul etmek için.

## <a name="hello-webmatrix"></a>Merhaba, WebMatrix

Yükleme işlemi tamamlandığında, WebMatrix otomatik olarak başlatabilirsiniz. Windows, gelen içermiyorsa **Başlat** menüsü, başlatma **Microsoft WebMatrix**.

WebMatrix ilk kez başlattığınızda, Microsoft Azure için Microsoft hesabınızla oturum açmak için bir fırsat verilir. Oturum açma tarafından 10 ücretsiz web uygulamaları Azure aracılığıyla alırsınız. Bu ücretsiz web uygulamaları uygulamalarınızı test etmek için kolay bir yol sağlamak. Zaten bir Azure hesabınız yoksa, ancak bir MSDN aboneliğiniz varsa, şunları yapabilirsiniz [MSDN abonelik Avantajlarınızı etkinleştirebilir](https://www.windowsazure.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Aksi takdirde, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Bu öğretici ile devam etmek şu anda oturum gerekmez. Şimdi oturum değil, daha sonra oturum açmak için seçeneği hala gerekir. Son [konu](publishing.md) Bu öğreticide serisi için Azure Web sitenizi dağıtmak alınmaktadır; bu nedenle, bu konuda tamamlamak oturum açmanız.

Bu noktada, ya da, Microsoft hesabı veya Seç oturum açmanız **şimdi değil** alt köşedeki.

![Oturum Aç](getting-started/_static/image7.png)

Başlamak için boş bir Web sitesi oluşturmak ve bir sayfa ekleyin. Sonraki Öğreticide bu kümesindeki yerleşik Web şablonlarından birini yürütmek.

Başlangıç penceresinde **yeni**.

![WebMatrix başlangıç ekranı](getting-started/_static/image8.png)

Önceden oluşturulmuş dosyalar ve farklı türde Web siteleri için sayfaları şablonlarıdır. Tüm varsayılan olarak kullanılabilir şablonları görmek için Şablon Galerisi seçeneğini belirleyin.

![Select Şablon Galerisi](getting-started/_static/image9.png)

İçinde **Hızlı Başlangıç** penceresinde, seçin **boş Site** gelen **ASP.NET** grup ve "WebPagesMovies" Yeni sitesini adlandırın.

![WebMatrix hızlı başlangıç penceresinde seçili boş Site şablonuyla](getting-started/_static/image10.png)


              **İleri**'ye tıklayın.

Microsoft hesabınızda oturum açmanızdan, Azure'da site oluşturma fırsatınız olur. Varsayılan adı sitenizin adını temel alarak **WebPagesMovies.azurewebsites.net** önerilir; ancak, bu adı Windows Azure üzerinde kullanılabilir değil ünlem gösterir. Kolaylık olması için seçin **atla** Azure üzerinde şu anda web sitesi oluşturma atlamak için. Bu seri biz site Azure'a yayımlayacak.

![Azure sitesi oluştur](getting-started/_static/image11.png)

WebMatrix oluşturur ve site açar:

![Yeni WebPagesMovies sitesini Webmatrix'te açın](getting-started/_static/image12.png)

En üstte bir hızlı erişim araç çubuğu ve Şerit yoktur. Altındaki sol çalışma alanı seçicisinde görevleri arasında geçiş Burada gördüğünüz (**Site**, **dosyaları**, **veritabanları**, **raporları**). Sağ tarafta içerik bölmesinde Düzenleyicisi ve raporları içindir. Ve alt arasında bazen iletiler için bir bildirim çubuğu görürsünüz.

Hakkında daha fazla WebMatrix ve özelliklerini bu öğreticileri ilerledikçe öğreneceksiniz.

## <a name="creating-a-web-page"></a>Web sayfası oluşturma

WebMatrix ve ASP.NET Web sayfaları konusunda bilgi sahibi olmak için basit bir sayfa oluşturacaksınız.

Çalışma alanı seçicisinde seçin **dosyaları** çalışma. Bu çalışma alanı, dosyalar ve klasörlerle çalışmanıza olanak tanır. Sol bölmede, sitenizin dosya yapısı gösterir. Dosya ile ilgili görevleri göstermek için Şerit değişir.

![WebMatrix çalışma dosyası](getting-started/_static/image13.png)

Şeritte altında oku **yeni** ve ardından **yeni dosya**.

![Kullanarak &quot;yeni&quot; yeni bir dosya oluşturmak için Şerit'te komutu](getting-started/_static/image14.png)

WebMatrix dosya türlerinin bir listesini görüntüler. Seçin **CSHTML**hem de **adı** "HelloWorld" yazın. Bir ASP.NET Web Pages sayfası CSHTML sayfasıdır.

![HelloWorld.cshtml adlı yeni bir CSHTML sayfa oluşturma](getting-started/_static/image15.png)

**Tamam**'ı tıklatın.

WebMatrix, sayfa oluşturur ve Düzenleyicisi'nde açar.

![WebMatrix Düzenleyicisi'nde yeni HelloWorld sayfası](getting-started/_static/image16.png)

Gördüğünüz gibi sayfa şöyle üst blok dışında sıradan HTML biçimlendirmesi çoğunlukla içerir:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Bu kod, ekleme için kısa bir süre içinde anlatıldığı gibi.

Dikkat sayfasının farklı bölümleri &mdash; öğe adları, öznitelikleri ve metin artı üst blok — farklı renkler tüm yazılımında. Bu adlı *sözdizimi vurgulama*, ve her şeyi açık tutmak kolaylaştırır. WebMatrix web sayfalarında çalışmak kolaylaştırır özelliklerden biridir.

İçerik için ekleme `<head>` ve `<body>` aşağıdaki örnekte gibi öğeler. (İstediğiniz yaparsanız, yalnızca aşağıdaki bloğu kopyalayın ve tüm mevcut sayfa bu kod ile değiştirin.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Hızlı Erişim Araç veya **dosya** menüsünde tıklatın **kaydetmek**.

![WebMatrix hızlı erişim araç çubuğunda Kaydet düğmesi](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Sayfa test etme

İçinde **dosyaları** çalışma alanında, sağ *HelloWorld.cshtml* sayfasında ve ardından **başlatma tarayıcıda**.

![WebMatrix Şeritte Çalıştır düğmesini kullanarak bir sayfa çalıştırma](getting-started/_static/image18.png)

WebMatrix, bilgisayarınızdaki sayfaları test etmek için kullanabileceğiniz bir yerleşik web sunucusu (IIS Express) başlatır. (WebMatrix, IIS Express, test edebilirsiniz önce sayfanızı bir web sunucusuna herhangi bir yerde yayımlamanız gerekir.) Sayfa varsayılan tarayıcınızda görüntülenir.

![&quot;Merhaba Dünya&quot; tarayıcıda çalışan sayfası](getting-started/_static/image19.png)

Webmatrix'te bir sayfayı test ettiğinizde, tarayıcıda URL şöyle olduğuna dikkat edin `http://localhost:33651/HelloWorld.cshtml.` adı *localhost* sayfa kendi bilgisayarınızda bir web sunucusu tarafından hizmet verilen anlamına gelir, yerel bir sunucuya başvuruyor. WebMatrix belirtildiği gibi bir sayfa başlattığında çalıştırılan IIS Express adlı bir web sunucusu programı içerir.

Sonra sayı *localhost* (örneğin, *localhost:33651*) başvurduğu bir *bağlantı noktası numarası* bilgisayarınızda. "Bu belirli Web sitesi için IIS Express kullanan kanal" sayısıdır. Bağlantı noktası numarası rastgele 1024 ile 65536 aralığından sitenizi oluşturmak ve oluşturduğunuz her site için farklı seçilir. (Kendi site test ettiğinizde, bağlantı noktası numarasını neredeyse kesinlikle 33561 daha farklı bir numara olacaktır.) Her Web sitesi için farklı bir bağlantı noktası kullanarak, IIS Express için Konuşmayı sitelerinizi hangisinin düz kullanmaya devam edebilir.

Artık görmek sitenizi bir ortak web sunucusuna yayımladığınızda sonraki *localhost* URL. Bu noktada, daha genel bir URL gibi görürsünüz `http://myhostingsite/mywebsite/HelloWorld.cshtml` veya ne olursa olsun sayfasıdır. Bu öğretici serisinde daha sonra bir site yayımlama hakkında daha fazla bilgi edineceksiniz.

## <a name="adding-some-server-side-code"></a>Bazı sunucu tarafı kod ekleme

Tarayıcıyı kapatın ve WebMatrix sayfasına geri dönün.

Aşağıdaki kod gibi görünüyor. böylece bir satır kod bloğunu ekleyin:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Biraz Razor kodunun budur. Geçerli tarih ve saati alır ve bu değeri içine yerleştirir büyük olasılıkla boş olduğundan bir *değişkeni* adlı `currentDateTime`. Daha fazla okuyacaksınız sonraki öğretici Razor söz dizimi hakkında.

Sayfasının gövdesindeki sonra `<p>Hello World!</p>` öğesi, aşağıdakileri ekleyin:

[!code-html[Main](getting-started/samples/sample4.html)]

Bu kod, yerleştirin değerini alır `currentDateTime` üst değişken ve sayfanın biçimlendirmesine ekler. `@` Karakteri sayfasında ASP.NET Web Pages kodunu işaretler.

(Sayfanın çalıştırılmadan önce WebMatrix değişiklikleri sizin için kaydeder) sayfasını tekrar çalıştırın. Bu süre, tarih ve saat sayfasında görürsünüz.

![&quot;Merhaba Dünya&quot; tarayıcı dinamik olarak üretilen zaman görüntü ile çalışan sayfası](getting-started/_static/image20.png)

Birkaç dakika bekleyin ve sonra tarayıcıda sayfayı yenileyin. Tarih ve saat gösterimi güncelleştirilir.

Tarayıcıda, sayfa kaynağında arayın. Aşağıdaki biçimlendirmede gibi görünüyor:

[!code-html[Main](getting-started/samples/sample5.html)]

Dikkat `@{ }` üst blok değil vardır. Ayrıca tarih ve saat gösterimi gerçek bir karakter dizesi gösterdiğine dikkat edin (`1/18/2012 2:49:50 PM` veya herhangi) değil `@currentDateTime` vardı gibi *.cshtml* sayfası. İşte, sayfa çalıştırdığınızda, ASP.NET ile işaretlenmiş tüm kodu (çok az bu durumda) işlenen ne `@`. Kod çıkışı üretir ve bu çıkışı sayfasına eklenmiştir.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>ASP.NET Web sayfaları üzeresiniz budur

ASP.NET Web sayfaları dinamik web içeriği üretir okurken ne Burada gördüğünüz olur. Yeni oluşturduğunuz sayfa önce gördünüz aynı HTML biçimlendirmesi içerir. Ayrıca, her türlü görevleri gerçekleştirebilir kod de içerebilir. Bu örnekte, geçerli tarih ve saati alma Önemsiz görevini yaptınız. Gördüğünüz gibi sayfasında bir çıktı oluşturmak için HTML kod intersperse. Birisi isteğinde bulunduğunda bir *.cshtml* sayfasını tarayıcıda, ASP.NET web sunucusu elinizde hala durumdayken sayfasını işler. ASP.NET kodunun çıkış (varsa) sayfasına HTML olarak ekler. Kod işlem tamamlandığında, ASP.NET ortaya çıkacak sayfasında tarayıcıya gönderir. Tüm tarayıcı hiç alır HTML olabilir. Bir diyagram şöyledir:

![Kavramsal akışı nasıl ASP.NET HTML dinamik olarak oluşturur](getting-started/_static/image21.png)

Düşünce basittir ancak kodu gerçekleştirebileceğiniz birçok ilginç görevleri vardır ve hangi dinamik olarak HTML içeriğini sayfaya ekleyebileceğiniz ilginç birçok yolu vardır. Ve ASP.NET *.cshtml* herhangi bir HTML sayfası gibi sayfaları da tarayıcının kendi (JavaScript ve jQuery kodu) çalışan bir kod içerir. Tüm bunlar Bu öğretici kümesi ve sonraki olanları ele alacağız.

## <a name="coming-up-next"></a>Sıradaki gelen

Bu serideki sonraki öğretici ASP.NET Web sayfaları biraz daha programlama keşfedin.

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web sitesi sıfırdan oluşturma](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Bu, özellikle bir öğreticidir WebMatrix (ASP.NET Web sayfaları değil) kullanma hakkında. Bu biraz bazı ek özelliklerini Bu öğretici kümesinde kapak olmaz WebMatrix hakkında daha fazla ayrıntı girmeyeceğini.

>[!div class="step-by-step"]
[Sonraki](intro-to-web-pages-programming.md)
