---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Başlarken | Microsoft Docs
author: tfitzmac
description: WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir. Visual Studio veya Visual Studio Code'u kullanın. Bu kılavuz bir...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 467239fdd2758240e589f4e1bfb40501502b83cf
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391277"
---
<a name="getting-started"></a>Başlarken
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE[](~/includes/rp.md)]

> > [!NOTE] 
> > 
> > WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir. Kullanım [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) veya [Visual Studio Code'u](https://code.visualstudio.com/).
> 
> 
> Bu kılavuzu ve uygulama size bir ASP.NET Web sayfaları (sürüm 2 veya sonraki sürümler), genel bakış ve Razor sözdizimi, dinamik Web siteleri oluşturmak için basit bir çerçeve. WebMatrix, sayfalar ve site oluşturmak için bir araç da tanıtılmaktadır.
> 
> **Düzey**: yeni ASP.NET Web sayfaları için.  
> **Kabul becerileri**: HTML, temel geçişli stil sayfaları (CSS).
> 
> Kümenin ilk öğreticide öğreneceksiniz:
> 
> - İçin nedir ve hangi ASP.NET Web Pages teknolojidir.
> - WebMatrix nedir?
> - Her şeyi yükleme.
> - WebMatrix kullanarak Web sitesi oluşturma
>   
> 
> Ele alınan özelliklerin/teknolojiler:
> 
> - Microsoft Web Platformu yükleyicisi.
> - WebMatrix.
> - *.cshtml* sayfaları
>   
> 
> Mike Pope, bu öğreticide ilk olarak yazıldı. Tom FitzMacken Microsoft WebMatrix 3'için güncelleştirildi.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>Bilmeniz gerekenler?

Biz, alışık olduğunuz varsayılır:

- **HTML**. Hiçbir kapsamlı uzmanlığını gereklidir. Biz HTML açıklayan olmaz, ancak biz de karmaşık bir şey kullanmayın. Burada yararlı oldukları düşünüyoruz HTML öğreticiler bağlantılarını sağlarız.
- **Geçişli stil sayfaları (CSS)**. Aynı HTML.
- **Temel veritabanı fikirleri**. Bu öğretici kümesi için biz genellikle, Elektronik tablolardaki verileri için kullanılan ve sıralanmış ve uzmanlık düzeyi verilere, filtre varsayılır.

Biz de, temel programlama edinmek istiyorsanız da varsayılır. ASP.NET Web Pages adlı C# programlama dilini kullanın. Programlama, yalnızca bir ilgi içindeki herhangi bir arka plana sahip gerekmez. Bir web sayfasında önce hiç olmadığı kadar herhangi bir JavaScript yazdıysanız, arka plan bolca aradığınızı bulacaksınız.

Ancak biz kısa sürede yeni programcılar Getir ile programlama hakkında bilginiz varsa, Bu öğretici başlangıçta kümesine bulabilirsiniz, Not yavaş taşır. İlk birkaç öğreticiler aldığımız gibi yine de olmayacaktır açıklayın daha az temel programlama ve şeyler bir daha hızlı klibi taşınır.

## <a name="what-do-you-need"></a>Ne gerekiyor?

İşte gerekenler:

- Windows 8, Windows 7, Windows Server 2008 veya Windows Server 2012 çalıştıran bir bilgisayar.
- Canlı bir internet bağlantısı.
- Yönetici ayrıcalıkları (yükleme işlemi için gereklidir).

## <a name="what-is-aspnet-web-pages"></a>ASP.NET Web sayfaları nedir?

ASP.NET Web sayfaları, dinamik web sayfaları oluşturmak için kullanabileceğiniz bir altyapısıdır. Basit bir HTML web sayfası statiktir; içeriği, sayfaya sabit HTML biçimlendirme tarafından belirlenir. Dinamik sayfaları, ASP.NET Web sayfaları ile oluşturduğunuz benzer kod kullanarak hızla, sayfa içeriğini oluşturmanızı sağlar.

Dinamik sayfalar çok şey yapmanıza olanak tanır. Bir kullanıcı giriş formu kullanarak isteyin ve ne sayfası görüntüler veya şöyle değiştirin. Bir kullanıcıdan bilgi al, bir veritabanına kaydetme ve daha sonra liste. Sitenizden e-posta gönderebilirsiniz. Diğer Hizmetleri Web (örneğin, bir eşleme hizmeti) ile etkileşim kurabilir ve bu kaynaklardan bilgi tümleştirme sayfaları oluşturmak.

## <a name="what-is-webmatrix"></a>WebMatrix nedir?

WebMatrix tümleşen bir web sayfası Düzenleyicisi, bir veritabanı yardımcı programı, sayfalarını ve Web sitenizi Internet'e yayımlama özelliklerini test etmek için bir web sunucusu bir araçtır. WebMatrix, ücretsiz ve kolay yükleme ve kullanımı kolay. (Ayrıca PHP gibi diğer teknolojileri yanı sıra, yalnızca düz HTML sayfaları için çalışır.)

Aslında yoksa *sahip* WebMatrix ASP.NET Web sayfaları ile çalışmak için kullanılacak. Sayfaları metin kullanarak Düzenleyicisi, oluşturabilir ve erişimi olan bir web sunucusunu kullanarak sayfaları test. Bu öğretici boyunca WebMatrix kullanabilirsiniz ancak WebMatrix tüm çok, kolaylaştırır.

## <a name="about-these-tutorials"></a>Bu öğreticileri hakkında

Bu öğretici kümesi, ASP.NET Web sayfalarının nasıl kullanılacağını giriş niteliğindedir. Bu giriş niteliğindeki öğretici kümesinde toplam 9 öğreticiler vardır. ASP.NET Web Pages Acemi kullanıcıdan gerçek ve profesyonel görünümlü bir Web siteleri oluşturmak için gereken bir dizi öğretici kümeleri gereksinimlerimizim bir parçasıdır.

Bu ilk öğreticide, ASP.NET Web sayfaları ile çalışmaya ilişkin temel bilgileri gösteren concentrates ayarlayın. İşiniz bittiğinde, burada bunu sona erer ve, Web sayfalarını daha derinlemesine keşfedin öğrenilip ek öğretici kümeleri ile çalışabilirsiniz.

Biz kasıtlı olarak üzerinde ayrıntılı açıklamalar kolay gidin. Ve bir şeyi göstereceğiz her durumda, Bu öğretici kümesi için size her zaman düşünüyoruz bir şekilde anlaşılması kolay seçin. Sonraki öğretici kümeleri, daha ayrıntılı gidin ve daha verimli ya da daha esnek yaklaşımları (Ayrıca daha eğlenceli) gösterir. Ancak bu öğreticileri öncelikle temellerini anlamanız gerekir.

Bu özellikler, ASP.NET Web sayfaları yalnızca başlangıç öğretici kümesi içerir:

- Giriş ve yüklü olan her şeyi alma. (Bu makaleyi okuduğunuz öğreticide içindir.)
- ASP.NET Web sayfaları Programlama temelleri.
- Veritabanı oluşturma.
- Oluşturma ve bir kullanıcı giriş formu işleme.
- Ekleme, güncelleştirme, veritabanındaki verileri siliniyor.

## <a name="what-will-you-create"></a>Ne oluşturacaksınız?

Bu öğreticide ayarlayabilir ve sonraki olanları çalışmalarınızı istediğiniz film burada listeleyebilirsiniz bir Web sitesi. Filmler girin, düzenleyebilir ve bunları listelemek mümkün olacaktır. Bu öğretici kümesinde oluşturacaksınız sayfaları birkaç aşağıda verilmiştir. İlki, oluşturacağınız sayfa listeleme film gösterir:

![Film listesini gösteren bitirdi film uygulaması](getting-started/_static/image1.png)

Ve İşte yeni film bilgileri sitenize eklemenize olanak sağlayan sayfası:

![Ekleme film sayfasını gösteren tamamlanmış film uygulaması](getting-started/_static/image2.png)

Bu sonraki öğretici kümeleri yapı ayarlayın ve resimleri karşıya yükleme, oturum kişilere izin vererek, e-posta göndermek ve sosyal medya ile tümleştirme gibi daha fazla işlevsellik ekler.

## <a name="see-this-app-running-on-azure"></a>Azure üzerinde çalışan bu uygulamayı bakın

Canlı web uygulaması olarak çalışan tamamlanmış site görmek ister misiniz? Aşağıdaki düğmeye tıklayarak Azure hesabınızda bir tam sürümü uygulama dağıtabilirsiniz.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Bu çözüm, Azure'a dağıtmak için bir Azure hesabına ihtiyacınız var. Bir hesap zaten yoksa, aşağıdaki seçenekleriniz:

- [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

## <a name="installing-everything"></a>Her şeyi yükleme

Her şey, Microsoft Web Platformu Yükleyicisi'ni kullanarak yükleyebilirsiniz. Aslında, yükleyici yükleme ve diğer her şey yüklemek için kullanın.

Web sayfaları kullanmak için sahip en az zorunda yüklü SP3, Windows XP veya Windows Server 2008 veya üstü.

Üzerinde [Web Pages sayfası](../../../index.md) ASP.NET Web sitesini, tıklayın **yükleme**.

![ASP.NET Web sitesini gösteren &quot;Webmatrix'i yükleyin&quot; düğmesi](getting-started/_static/image3.png)

Lisans koşullarını ve gizlilik bildirimi, WebMatrix yüklemeden önce kabul istenir.

![yüklemeye başlamak için koşulu kabul](getting-started/_static/image4.png)

Tıklayın **çalıştırma** yüklemeyi başlatmak için. (Yükleyici kaydetmek istiyorsanız, tıklayın **Kaydet** ve ardından kaydettiğiniz bu klasörden yükleyiciyi çalıştırın.)

![](getting-started/_static/image5.png)

Web Platformu yükleyicisi görüntülenir, WebMatrix yüklemek için hazır. **Yükle**'ye tıklatın.

![](getting-started/_static/image6.png)

Yükleme işlemi, ne, bilgisayarınıza yüklemek sahip rakamları ve işlemini başlatır. Hangi tam olarak yüklenmesi gerekir bağlı olarak, işlem her yerde birkaç dakika için birkaç dakika sürebilir. Seçin **kabul ediyorum** lisans koşullarını kabul etmek için.

## <a name="hello-webmatrix"></a>Merhaba, WebMatrix

İşlem tamamlandığında, yükleme işlemi otomatik olarak WebMatrix başlatabilirsiniz. Windows gelen eşleşmiyorsa **Başlat** menüsü, başlatma **Microsoft WebMatrix**.

WebMatrix için ilk kez başlattığınızda, Microsoft Azure için Microsoft hesabınızla oturum açmak için bir fırsat sunulur. Oturum açarak Azure üzerinden 10 ücretsiz web uygulaması elde edersiniz. Bu ücretsiz web apps, uygulamalarınızı test etmek için kullanışlı bir yol sağlar. Bir Azure hesabınız yoksa, ancak bir MSDN aboneliğiniz varsa [MSDN abonelik Avantajlarınızı etkinleştirin](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Aksi takdirde, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Bu öğretici ile devam etmek şu anda oturum gerekmez. Artık oturum değil ise, yine de daha sonra oturum açmayı seçeneğine sahip olursunuz. Son [konu](publishing.md) Bu öğreticide serisi Azure'da Web sitenizi dağıtmak nasıl etkinleştireceğinizi de açıklar; bu nedenle, bu konuyu tamamlamak oturum açmanız.

Bu noktada, ya da, Microsoft hesabı ile veya select oturum **şimdi değil** sağ alt köşedeki.

![Oturum Aç](getting-started/_static/image7.png)

Başlamak için boş bir Web sitesi oluşturma ve bir sayfa ekleyin. Bir sonraki Öğreticide bu yerleşik Web şablonlarından biriyle oynatın.

Başlangıç pencerede **yeni**.

![WebMatrix başlangıç ekranı](getting-started/_static/image8.png)

Önceden oluşturulmuş dosyalar ve sayfalar için Web siteleri farklı türde şablonlardır. Tüm varsayılan olarak kullanılabilir şablonları görmek için Şablon Galerisi seçeneğini seçin.

![Şablon Galerisi seçin](getting-started/_static/image9.png)

İçinde **Hızlı Başlangıç** penceresinde **boş Site** gelen **ASP.NET** grubu ve yeni site "WebPagesMovies" olarak adlandırın.

![Boş Site şablonu seçili penceresiyle WebMatrix hızlı başlangıç](getting-started/_static/image10.png)

**İleri**'ye tıklayın.

Microsoft hesabınızda oturum açmanızdan, Azure'da site oluşturma fırsatı verilir. Varsayılan adını sitenizin adına dayalı **WebPagesMovies.azurewebsites.net** önerilir; ancak, bu adı Windows Azure üzerinde kullanılabilir değil ünlem gösterir. Kolaylık olması için seçin **atla** Azure'da web sitesi oluşturma, şu anda atlamak için. Bu seri için Azure site yayımlarız.

![Azure site oluşturma](getting-started/_static/image11.png)

WebMatrix oluşturur ve bu sitenin açılır:

![Webmatrix'te yeni WebPagesMovies sitesini açın](getting-started/_static/image12.png)

En üstünde, hızlı erişim araç çubuğu ve bir Şerit yoktur. Alt sol, çalışma alanı Seçici görevler arasında geçiş Burada gördüğünüz (**Site**, **dosyaları**, **veritabanları**, **raporları**). Sağ tarafta içerik bölmesinde Düzenleyicisi ve raporları içindir. Ve alt arasında bazen iletiler için bir bildirim çubuğu göreceksiniz.

Hakkında daha fazla WebMatrix ve özelliklerini bu öğreticileri gibi öğreneceksiniz.

## <a name="creating-a-web-page"></a>Bir Web sayfası oluşturma

WebMatrix ve ASP.NET Web sayfaları ile ilgili bilgi sahibi olmak için basit bir sayfa oluşturacaksınız.

Çalışma alanı seçiciden seçin **dosyaları** çalışma. Bu çalışma alanı, dosyalar ve klasörler ile çalışmanıza olanak tanır. Sol bölmede, sitenizin dosya yapısı gösterilmektedir. Dosya ile ilgili görevleri göstermek için Şerit değişir.

![Webmatrix'te dosya çalışma](getting-started/_static/image13.png)

Şeritte altında oku **yeni** ve ardından **yeni dosya**.

![Kullanarak &quot;yeni&quot; yeni bir dosya oluşturmak için Şeritte komutu](getting-started/_static/image14.png)

WebMatrix, dosya türlerinin bir listesini görüntüler. Seçin **CSHTML**hem de **adı** "HelloWorld" yazın. Bir ASP.NET Web Pages sayfasında CSHTML sayfasıdır.

![HelloWorld.cshtml adlı yeni bir CSHTML sayfası oluşturma](getting-started/_static/image15.png)

**Tamam**'ı tıklatın.

WebMatrix sayfası oluşturur ve düzenleyicide açılır.

![WebMatrix Düzenleyicisi'nde yeni HelloWorld sayfası](getting-started/_static/image16.png)

Gördüğünüz gibi sayfa şuna benzer bir üst bloğu dışında sıradan HTML biçimlendirmesi çoğunlukla içerir:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Bu kısa bir süre içinde anlatıldığı gibi kodu eklemek için.

Dikkat sayfasının farklı bölümlerini &mdash; öğe adları, öznitelikleri ve metnin yanı sıra üst kısmındaki blok — tüm de farklı renklerde. Bu adlandırılır *söz dizimi vurgulama*, ve her şeyi NET tutmak kolaylaştırır. Webmatrix'te web sayfalarıyla çalışma kolaylaştırır özelliklerden biridir.

İçin içerik ekleme `<head>` ve `<body>` aşağıdaki örnekte gibi öğeler. (İsterseniz, yalnızca aşağıdaki bloğunu kopyalayın ve mevcut sayfanın tamamını şu kodla değiştirin.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Hızlı Erişim Araç çubuğu veya **dosya** menüsünde tıklatın **Kaydet**.

![WebMatrix hızlı erişim araç çubuğunda Kaydet düğmesi](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Sayfasını test etme

İçinde **dosyaları** çalışma alanında, sağ *HelloWorld.cshtml* sayfasında ve ardından **tarayıcıda Başlat**.

![WebMatrix Şeritte Çalıştır düğmesini kullanarak bir sayfa çalıştırma](getting-started/_static/image18.png)

WebMatrix, bilgisayarınızda sayfaları test etmek için kullanabileceğiniz bir yerleşik web sunucusu (IIS Express) başlatır. (Webmatrix'te IIS Express, test edebilirsiniz önce sayfanıza bir web sunucusuna yere yayımlamanız gerekir.) Sayfa varsayılan tarayıcınızda görüntülenir.

![&quot;Merhaba Dünya&quot; sayfasını tarayıcıda çalışıyor](getting-started/_static/image19.png)

Webmatrix'te bir sayfayı test ettiğinizde, tarayıcı URL aşağıdakine benzer olduğunu fark `http://localhost:33651/HelloWorld.cshtml.` adı *localhost* sayfa kendi bilgisayarınızda bir web sunucusu tarafından sunulan, yani, yerel bir sunucuya ifade eder. WebMatrix, belirtildiği gibi bir sayfa başlatıldığında çalıştırılan IIS Express adlı bir web sunucusu programı içerir.

Sonra sayı *localhost* (örneğin, *localhost:33651*) başvurduğu bir *bağlantı noktası numarası* bilgisayarınızda. Bu "IIS Express kullandığından bu belirli bir Web sitesi için kanal" sayısıdır. Bağlantı noktası numarasını rastgele bir aralıktan 1024 ile 65536 sitenizi oluşturmak ve oluşturduğunuz her site için farklı seçilir. (Kendi site test ettiğinizde, bağlantı noktası numarasını neredeyse kesindir 33561 değerinden farklı bir numara olacaktır.) Her Web sitesi için farklı bir bağlantı noktası kullanarak IIS Express için Bahsediyor sitelerinizi hangisinin düz tutabilirsiniz.

Artık bkz: Genel web sunucusuna, sitenizi yayımladığınızda sonraki *localhost* URL. Bu noktada, gibi daha genel bir URL göreceğiniz `http://myhostingsite/mywebsite/HelloWorld.cshtml` veya herhangi bir sayfa. Bu öğretici serisinde daha sonra bir site yayımlama hakkında daha fazla bilgi edineceksiniz.

## <a name="adding-some-server-side-code"></a>Bazı sunucu tarafındaki kod ekleme

Tarayıcıyı kapatın ve WebMatrix sayfasına geri dönün.

Şu kod gibi görünüyor. böylece kod bloğu için bir satır ekleyin:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Razor kod biraz budur. Geçerli tarih ve saati alır ve bu değeri içine yerleştirir büyük olasılıkla boş olduğundan bir *değişkeni* adlı `currentDateTime`. Daha fazla bilgi edinin sonraki öğreticide Razor söz dizimi hakkında.

Sayfanın gövdesindeki sonra `<p>Hello World!</p>` öğesi, aşağıdakileri ekleyin:

[!code-html[Main](getting-started/samples/sample4.html)]

Bu kod içine yerleştirdiğiniz değeri alır `currentDateTime` üst değişken ve sayfanın biçimlendirmesine ekler. `@` ASP.NET Web Pages kod sayfasında karakter işaretler.

(Sayfa çalıştırılmadan önce WebMatrix değişiklikleri sizin için kaydeder) sayfasını tekrar çalıştırın. Bu kez tarih ve saat sayfasında bakın.

![&quot;Merhaba Dünya&quot; tarayıcı dinamik olarak üretilen bir saati görüntüleme ile çalışan sayfası](getting-started/_static/image20.png)

Birkaç dakika bekleyin ve ardından sayfanın tarayıcıda yenileyin. Tarih ve saat görüntüleme güncelleştirilir.

Tarayıcıda, sayfa kaynağında arayın. Aşağıdaki biçimlendirme gibi görünüyor:

[!code-html[Main](getting-started/samples/sample5.html)]

Dikkat `@{ }` üst blok değil vardır. Ayrıca, tarih ve saat görüntüleme gerçek bir karakter dizesi gösterir dikkat edin (`1/18/2012 2:49:50 PM` veya) değil `@currentDateTime` olduğu gibi *.cshtml* sayfası. İşte sayfa çalıştırdığınızda, ASP.NET ile işaretlenmiş tüm kod (çok az bu durumda) işlenen ne `@`. Çıkış kodu üretir ve bu çıkışı sayfasına eklenir.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>ASP.NET Web sayfaları üzeresiniz budur

ASP.NET Web Pages dinamik web içeriği üretir okuduğunuzda ne Burada gördüğünüz olur. Yeni oluşturduğunuz sayfasında önce gördüğünüz aynı HTML biçimlendirmeyi içerir. Ayrıca, çok çeşitli görevler gerçekleştiren kod de içerebilir. Bu örnekte, geçerli tarih ve saat almaya ilişkin basit bir görev yaptım. Gördüğünüz gibi HTML sayfasındaki çıktı üretmek için kod aralarına koyabilirsiniz. Birisi istediğinde bir *.cshtml* sayfasını tarayıcıda, ASP.NET web sunucusunun hala kişiye olsa sayfa işler. ASP.NET kodun çıktısı (varsa) sayfasına HTML olarak ekler. Kod işlem tamamlandığında, ASP.NET elde edilen sayfanın tarayıcısına gönderir. Tüm tarayıcı hiç olmadığı kadar alır HTML budur. Bir diyagramda şu şekildedir:

![Kavramsal akışı nasıl ASP.NET HTML dinamik olarak oluşturur](getting-started/_static/image21.png)

Basit bir uygulamadır ancak kod gerçekleştirebileceğiniz birçok ilgi çekici görevleri vardır ve hangi dinamik olarak HTML içeriği sayfaya ekleyebileceğiniz birçok ilgi çekici yolu vardır. Ve ASP.NET *.cshtml* sayfaları, herhangi bir HTML sayfası gibi tarayıcı kendisi (JavaScript ve jQuery kodu) çalışan kod da içerebilir. Tüm bunlar, Bu öğretici kümesinde ve sonraki olanları hakkında bilgi edineceksiniz.

## <a name="coming-up-next"></a>Sıradaki gelen

Bu serideki sonraki öğretici, ASP.NET Web Pages biraz daha programlama keşfedin.

## <a name="additional-resources"></a>Ek Kaynaklar

[Sıfırdan ASP.NET Web sitesi oluşturma](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Bu, özellikle bir öğretici WebMatrix (ASP.NET Web sayfaları değil) kullanarak hakkında. Bu biraz bazı ek özellikleri Bu öğretici kümesinde ele olmaz WebMatrix hakkında daha fazla ayrıntı geçerlidir.

> [!div class="step-by-step"]
> [Next](intro-to-web-pages-programming.md)
