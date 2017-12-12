---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: "Hata ayıklama yetenekleri ASP.NET AJAX anlama | Microsoft Docs"
author: scottcate
description: "Kodda hata ayıklama özelliği her geliştiricinin kullanmakta olduğunuz teknolojisi bağımsız olarak kendi arsenal sahip olması gereken bir yetenektir. Çoğu geliştiricinin durumdayken..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 426d0182978faf7fc7516203fcc84ef0152790ba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Hata ayıklama yetenekleri ASP.NET AJAX anlama
====================
tarafından [Scott göstermek](https://github.com/scottcate)

[PDF indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> Kodda hata ayıklama özelliği her geliştiricinin kullanmakta olduğunuz teknolojisi bağımsız olarak kendi arsenal sahip olması gereken bir yetenektir. Çoğu geliştiricinin VB.NET veya C# kodunu kullanan ASP.NET uygulamaları hata ayıklamak için Visual Studio .NET ya da Web Developer Express kullanmaya alışkın olsa da, bazı de JavaScript gibi istemci tarafı kodda hata ayıklama için son derece kullanışlıdır, uyumlu değil. .NET uygulamalarında hata ayıklamak için kullanılan teknikleri aynı türde AJAX etkinleştirilmiş uygulamaları ve daha açık belirtmek gerekirse ASP.NET AJAX uygulamaları için uygulanabilir.


## <a name="debugging-aspnet-ajax-applications"></a>ASP.NET AJAX uygulamalarında hata ayıklama

Dan Wahlin

Kodda hata ayıklama özelliği her geliştiricinin kullanmakta olduğunuz teknolojisi bağımsız olarak kendi arsenal sahip olması gereken bir yetenektir. Kullanılabilir farklı hata ayıklama seçeneklerini anlama inanılmaz süreyi proje ve hatta belki de birkaç zorlukların kaydedebilirsiniz olduğunu bildiren olmadan gidiyor. Çoğu geliştiricinin VB.NET veya C# kodunu kullanan ASP.NET uygulamaları hata ayıklamak için Visual Studio .NET ya da Web Developer Express kullanmaya alışkın olsa da, bazı de JavaScript gibi istemci tarafı kodda hata ayıklama için son derece kullanışlıdır, uyumlu değil. .NET uygulamalarında hata ayıklamak için kullanılan teknikleri aynı türde AJAX etkinleştirilmiş uygulamaları ve daha açık belirtmek gerekirse ASP.NET AJAX uygulamaları için uygulanabilir.

Bu makalede nasıl Visual Studio 2008 ve diğer çeşitli araçları hataları ve diğer sorunları hızla bulmak için ASP.NET AJAX uygulamalarında hata ayıklamak için kullanılabilir görürsünüz. Bu tartışma Internet Explorer 6 veya üstünü hata ayıklama, kod üzerinden adım için Visual Studio 2008 ve betik Gezgini kullanma yanı sıra Web geliştirme Yardımcısı gibi ücretsiz diğer araçları kullanarak etkinleştirme hakkında bilgi içerir. Ayrıca, bir uzantı Firebug sağlayan JavaScript kodu herhangi bir aracı olmadan doğrudan tarayıcıda adım adım adlı Firefox kullanarak ASP.NET AJAX uygulamalarında hata ayıklamasını nasıl öğreneceksiniz. Son olarak, izleme ve kod onaylama deyimleri gibi çeşitli hata ayıklama görevlerle yardımcı olabilecek ASP.NET AJAX Kitaplığı'nda sınıfları için sunulan.

Internet Explorer'da görüntülenen sayfa hata ayıklamaya çalışmadan önce hata ayıklama için etkinleştirmeyi gerçekleştirmek için gereken birkaç temel adımı vardır. Başlamak için yapılması gereken bazı temel kurulum gereksinimleri bir göz atalım.

## <a name="configuring-internet-explorer-for-debugging"></a>Hata ayıklama için Internet Explorer'ı yapılandırma

Çoğu kişi Internet Explorer ile görüntülenen bir Web sitesinde JavaScript sorunları görmeniz ilgilenen değil. Aslında, ortalama kullanıcı bile bir hata iletisi görürseniz yapmanız gerekenler bilmeniz olmayacaktır. Sonuç olarak, hata ayıklama seçenekleri varsayılan tarayıcı olarak devre dışı bırakılmıştır. Ancak, hata ayıklama açmak ve yeni AJAX uygulamaları geliştirirken kullanmak üzere yerleştirmek oldukça basittir.

Hata ayıklama işlevini etkinleştirmek için Araçlar menüsünde Internet Seçenekleri ' Internet Explorer gidin ve Gelişmiş sekmesini seçin. Gözatma bölüm içinde aşağıdaki öğeleri işaretli olduğundan emin olun:

- (Internet Explorer) ayıklamasını devre dışı bırak
- (Diğer) ayıklamasını devre dışı bırak

Gerekli değil, eğer olsa da büyük olasılıkla hemen görünür ve belirgin olması için sayfaya JavaScript hatalarını isteyeceksiniz bir uygulamada hata ayıklama deniyorsunuz. Tüm hataları içeren bir ileti kutusu "Her komut dizisi hatası için bir bildirim göster" onay kutusunu işaretleyerek gösterilecek zorlayabilirsiniz. Bu bir uygulama geliştirirken sırada açmak için harika bir seçenek olmakla birlikte, bunu hızlı bir şekilde JavaScript hatalarla şansınız oldukça iyi olduğu diğer Web siteleri yalnızca harcadığı can sıkıcı olabilir.

Hata ayıklama için düzgün şekilde yapılandırıldıktan sonra Şekil 1 gösterir hangi Internet Gelişmiş iletişim kutusu Explorer görünmelidir.


[![Hata ayıklama için Internet Explorer'ı yapılandırma.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Şekil 1**: hata ayıklama için Internet Explorer yapılandırma.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Hata ayıklama açık durumda sonra komut dosyası hata ayıklayıcısı adlı Görünüm menüsünde görünen yeni menü öğesi görürsünüz. Bu sonraki deyimde açık ve sonu dahil olmak üzere kullanılabilecek iki seçenek vardır. Açık seçildiğinde hata ayıklama sayfası Visual Studio 2008 (Visual Web Developer Express ayrıca hata ayıklama için kullanılabileceğini unutmayın) istenir. Visual Studio .NET şu anda çalışıyorsa, bu örneği kullanmak için veya yeni bir örneğini oluşturmak için seçebilirsiniz. Sonraki deyim sonu seçildiğinde JavaScript kodu çalıştırıldığında hata ayıklama sayfası istenir. JavaScript kodu sayfasının yüklendiğinde olayı yürütülürse bir hata ayıklama oturumu tetiklemek için sayfayı yenileyin. Düğme tıklatıldığında sonra JavaScript kodu çalıştırırsanız düğme tıklatıldığında hemen sonra hata ayıklayıcı çalışır.

> *>[!NOTE] Windows Vista ile kullanıcı erişim denetimi (etkin UAC) çalıştıran ve bir yönetici olarak çalıştırmak için Visual Studio 2008 varsa, Visual Studio eklemek için istendiğinde işlem ekleme başarısız olur. Bu sorunu çözmek için Visual Studio ilk başlatın ve hata ayıklamak için bu örneği kullanmak için.*


Sonraki bölümde Visual Studio 2008 içinde doğrudan bir ASP.NET AJAX sayfası hata ayıklamak nasıl gösterilmektedir olsa da Internet Explorer komut dosyası hata ayıklayıcı seçeneğini kullanarak bir sayfa zaten açık olan ve daha eksiksiz incelemek istediğiniz yararlıdır.

## <a name="debugging-with-visual-studio-2008"></a>Visual Studio 2008 ile hata ayıklama

Visual Studio 2008 hata ayıklama .NET uygulamaları için her gün tüm dünyada geliştiriciler Bel hata ayıklama işlevsellik sağlar. Yerleşik hata ayıklayıcı kod çağrı yığını ve çok daha fazlasını nesne verileri, belirli değişkenler için izleme izleme görünümü adım olanak tanır. VB.NET veya C# kodunda hata ayıklama, ek hata ayıklayıcı de ASP.NET AJAX uygulamalarında hata ayıklama için yararlı olur ve JavaScript kodu satır adım olanak sağlar. Visual Studio 2008 kullanarak uygulamalarında hata ayıklama genel işlemi üzerinde discourse sağlama yerine istemci tarafı komut dosyası hata ayıklamak için kullanılan teknikleri odak takip ayrıntıları.

Visual Studio 2008'de bir sayfa hata ayıklama işlemi birkaç farklı şekillerde başlatılabilir. İlk olarak, önceki bölümde belirtildiği Internet Explorer komut dosyası hata ayıklayıcı seçeneğini kullanabilirsiniz. Bu iyi bir sayfasını tarayıcıda zaten yüklü ve bu hata ayıklamayı başlatmak istiyor musunuz çalışır. Alternatif olarak, Çözüm Gezgini'nde .aspx sayfasındaki sağ tıklatın ve menüden başlangıç sayfası olarak ayarla seçin. ASP.NET sayfaları hata ayıklama için alışkın, ardından, büyük olasılıkla bu önce yaptık. F5 düğmesine basıldığında sayfa hata ayıklaması yapılabilir. Ancak, bir kesme noktası herhangi bir yere genellikle kümesi sonraki göreceğiniz gibi her zaman JavaScript durumuyla olmayan VB.NET veya C# kodunda, ister.

*Katıştırılmış dış betikler karşılaştırması*

Visual Studio 2008 hata ayıklayıcı dış JavaScript dosyalardan daha farklı bir sayfasındaki katıştırılmış JavaScript değerlendirir. Dış betik dosyalarıyla dosyasını açın ve seçtiğiniz herhangi bir satırında bir kesme noktası ayarlayın. Kesme noktaları kod düzenleyici penceresini solundaki gri Tepsisi alanında tıklatarak ayarlayabilirsiniz. JavaScript zaman katıştırılmış bir sayfa kullanarak doğrudan içine `<script>` gri Tepsisi alanında tıklayarak bir kesme noktası ayarlama etiketi, bir seçenek değil. Katıştırılmış komut satırında bir kesme noktası ayarlamaya çalışır "Bu bir kesme noktası için geçerli bir konum değil" bildiren bir uyarı neden olur.

Bu sorunu geçici bir dış .js dosyasına kodu taşıma ve src özniteliğini kullanarak başvuran alabilirsiniz &lt;betik&gt; etiketi:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Değer ne kodu bir dış dosyasına taşıma veya bir seçenek değil daha daha fazla iş gerektirir mi? Düzenleyicisi'ni kullanarak bir kesme noktası ayarlanamıyor olsa da, doğrudan nerede hata ayıklamayı başlatmak istediğiniz koda debugger deyimi ekleyebilirsiniz. Başlatmak için hata ayıklama zorlamak için ASP.NET AJAX Kitaplığı'nda kullanılabilir Sys.Debug sınıfı de kullanabilirsiniz. Bu makalenin sonraki bölümlerinde Sys.Debug sınıfı hakkında daha fazla bilgi edineceksiniz.

Kullanarak bir örnek `debugger` anahtar sözcük listeleme 1'de gösterilmiştir. Bu örnek, bir güncelleştirme işlevi çağrısı yapılmadan önce sağ ayırmak için hata ayıklayıcı zorlar.

**1 listesi. Hata ayıklayıcı anahtar sözcüğü ayırmak için Visual Studio .NET hata ayıklayıcı zorlamak için kullanma.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Debugger deyimi bastığınızda hata ayıklama Visual Studio .NET ile sayfası istenir ve kod atlama başlayabilirsiniz. Bunu yapmak, ASP.NET AJAX kitaplığı komut dosyaları sayfa içinde sağlandığından kullanılan erişmeyle bir sorunla karşılaşabilirsiniz sırada Visual Studio kullanarak bir göz atalım. NET'in betik Gezgini.

## <a name="using-visual-studio-net-windows-to-debug"></a>Hata ayıklamak için Visual Studio .NET Windows kullanma

Sayfa içinde kullanılan tüm komut dosyaları açık ve hata ayıklama için kullanılabilir olmadığı sürece bir hata ayıklama oturumu başlatıldı ve varsayılan F11 anahtarı kullanarak kod üzerinden taramasını başlatmak, gösterilen hata iletişim kutusu karşılaşabileceğiniz Şekil 2 bakın.


[![Hata iletişim kutusunda kaynak kod yok hata ayıklama için kullanılabilir olduğunda gösterilen.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Şekil 2**: kaynak kod yok hata ayıklama için kullanılabilir olduğunda gösterilen hata iletişim kutusu.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Visual Studio .NET bazı sayfa tarafından başvurulan komut dosyalarının kaynak kodunu alma emin olmadığı için bu iletişim kutusu gösterilir. Bu oldukça can sıkıcı olabilirken ilk başta, basit bir düzeltme yoktur. Hata ayıklama oturumu başlatıldı ve bir kesme noktası isabet sonra Visual Studio 2008 menüsünde hata ayıklama Windows betik Gezgini penceresine gidin veya Ctrl + Alt + N kısayol tuşu kullanın.

> *>[!NOTE] Listelenen betik Gezgini menü göremiyorsanız, Araçlar Git* *Özelleştir* *Visual Studio .NET menüsündeki komutlar. Kategoriler bölümünde hata ayıklama girişini bulun ve tüm kullanılabilir menü girişleri göstermek için tıklayın. Komutlar listesinde betik Gezgini aşağı kaydırın ve hata ayıklama sürükleyin* *Windows menüde daha önce bahsedilen. Bunun yapılması betik Gezgini menü girişi Visual Studio .NET her çalıştırdığınızda kullanımına sunar.*


Betik Gezgini, bir sayfa içinde kullanılan tüm betikler görüntülemek ve kod düzenleyicisinde açmak için kullanılabilir. Betik Gezgini açıldıktan sonra kod düzenleyicisinde açmak için şu anda ayıklanacak .aspx sayfasında çift tıklayın. Betik Gezgini'nde gösterilen diğer komut dosyalarının tümü için aynı eylemi gerçekleştirin. Komut dosyalarının tümü yapabileceğiniz kod penceresinde açıldıktan sonra basın F11 (ve diğer hata ayıklama kısayollarıyla) kodunuz adım için kullanabilirsiniz. Şekil 3 betik Gezgini örneği gösterilmektedir. İki özel komut dosyaları ve dinamik olarak sayfaya ASP.NET AJAX ScriptManager tarafından eklenen iki komut yanı sıra ayıklanacak geçerli dosyanın (Demo.aspx) listeler.


[![Betik Gezgini sayfa içinde kullanılan betikleri kolay erişim sağlar.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Şekil 3**. Betik Gezgini sayfa içinde kullanılan betikleri kolay erişim sağlar.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Birkaç diğerleri windows ayrıca kodlarda bir sayfasında adım gibi yararlı bilgileri sağlamak için kullanılabilir. Örneğin, yerel öğeler penceresi sayfasında, belirli değişkenler veya koşulları değerlendirmek ve çıktı görüntülemek için komut penceresi kullanılır farklı değişkenlerin değerleri görmek için kullanabilirsiniz. İzleme deyimleri (Bu makalenin sonraki bölümlerinde ele alınacak) Sys.Debug.trace işlevin veya Internet Explorer'ın Debug.writeln işlevi kullanılarak yazılan görüntülemek için çıkış penceresini de kullanabilirsiniz.

Hata ayıklayıcıyı kullanma kod üzerinden adım olarak atanmış olan değerini görüntülemek için kodu değişkenlerde üzerinden fare. Belirli bir JavaScript değişkenine fare gibi Bununla birlikte, komut dosyası hata ayıklayıcısı bazen herhangi bir şey göstermeyecektir. Değeri görmek için deyimi veya Kod Düzenleyicisi penceresinde görmek ve bunun üzerine fare çalıştığınız değişkenini vurgulayın. Bu teknik her durumda işe yaramazsa rağmen birçok kez değeri yerel öğeler penceresi gibi farklı bir hata ayıklama penceresinde Ara gerek kalmadan görmeye siz olacaksınız.

Burada tartışılan özelliklerin bazıları gösteren videosu en görüntülenebilir [http://www.xmlforasp.net](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Web geliştirme Yardımcısı ile hata ayıklama

Visual Studio 2008 (ve Visual Web Developer 2008 Express) hata ayıklama araçları çok yeteneğine sahip olsa da, daha hafif de kullanılabilecek ek seçenekleri vardır. Serbest bırakılacak için en yeni araçları Web geliştirme Yardımcısı biridir. Microsoft'un Nikhil Kothari (anahtar ASP.NET AJAX mimarları Microsoft'taki biri), basit HTTP istek ve yanıt iletileri görüntülemek için hata ayıklama gelen birçok farklı görev gerçekleştirebilen bu mükemmel araç geliştirdik. Web geliştirme Yardımcısı, adresindeki indirilebilir [http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Web geliştirme Yardımcısı doğrudan Internet kullanmak uygun hale getiren Gezgini içinde kullanılabilir. Internet Explorer menüsünden araçları Web geliştirme yardımcı seçerek başlatılır. HTTP istek ve yanıt ileti günlüğe kaydetme gibi çeşitli görevleri gerçekleştirmek için tarayıcı çıkmak zorunda değilsiniz beri iyi tarayıcısı alt kısmında bu Aracı'nı açar. Şekil 4, Web geliştirme yardımcı nasıl eylemde göründüğünü gösterir.


[![Web geliştirme Yardımcısı](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Şekil 4**: Web geliştirme Yardımcısı ([tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Web geliştirme yardımcı kullandığınız bir aracı olmayan adıma kodlarda ile Visual Studio 2008 satır olarak. Ancak, bu izleme çıktısını görüntüleyin, kolayca bir komut dosyası değişkenleri değerlendirmek veya keşfetmek için kullanılabilir veri bir JSON nesnesi içinde değildir. Bir ASP.NET AJAX sayfası gelen ve bir sunucu geçirilen verileri görüntülemek için çok faydalı olur.

Internet Explorer'da Web geliştirme Yardımcısı açıldıktan sonra komut dosyasında hata ayıklama betik etkinleştirmek komut dosyasında hata ayıklama Web geliştirme Yardımcısı menüsünden önceki Şekil 4'te gösterildiği gibi seçerek etkinleştirilmesi gerekir. Bu sayfa çalışırken oluşan ıntercept hataları araç sağlar. Ayrıca, çıkış sayfasında izleme iletilerini kolay erişim sağlar. İzleme bilgilerini görüntülemek veya farklı işlevler bir sayfa içinde test etmek için betik komutlarını çalıştırmak için komut dosyasını Göster komut konsol Web geliştirme yardımcı menüsünden seçin. Bu, bir komut penceresi ve basit bir komut penceresi erişim sağlar.

*İzleme iletileri ve JSON nesne verilerini görüntüleme*

Komut penceresi komut yürütme veya hatta yüklemek veya farklı işlevler bir sayfayı test etmek için kullanılan komut dosyaları kaydetmek için kullanılabilir. Komut penceresinde görüntülenmesini sayfası tarafından yazılan izleme ya da hata ayıklama iletilerini görüntüler. Kod 2 nasıl Internet Explorer'ın Debug.writeln işlevi kullanarak bir izleme iletisi yazılacağını gösterir.

**2 listesi. Hata ayıklama sınıfı kullanarak bir istemci-tarafı izleme iletisi yazma.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

LastName özelliği Doe değerini içeriyorsa, Web geliştirme yardımcı iletisini görüntüler "kişi adı: Doe" komut dosyası konsolun komut penceresinde (hata ayıklama etkinleştirilmiş olduğunu varsayarak). Web geliştirme yardımcı ayrıca izleme bilgilerini yazmak veya JSON nesnelerinin içeriğini görüntülemek için kullanılan sayfalarına bir üst düzey debugService nesnesi ekler. Kod 3 debugService sınıfının izleme işlevini kullanarak bir örnek gösterilmektedir.

**3 listesi. İzleme iletisi yazmak için Web geliştirme yardımcının debugService sınıfını kullanma.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

İyi debugService sınıfı hata ayıklama Web geliştirme Yardımcısı çalıştırırken izleme verileri her zaman erişim kolaylaşır Internet Explorer'da etkinleştirilmemiş olsa bile çalışacağını özelliğidir. Aracı bir sayfa hata ayıklamak için kullanılmadığından, window.debugService çağrısı false döndürecektir beri izleme deyimleri yoksayılacak.

DebugService sınıfı ayrıca JSON nesne verileri Web geliştirme yardımcının Inspector penceresini kullanarak görüntülenmesine izin verir. 4 listeleme kişi verilerini içeren basit bir JSON nesnesi oluşturur. Nesne oluşturulduktan sonra bir çağrı yapılır debugService sınıfının görsel olarak denetlenecek JSON nesnesi izin vermek için işlevi inceleyin.

**4 listesi. JSON nesnesi verilerini görüntülemek için debugService.inspect işlevini kullanma.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Sayfadaki veya komut penceresi aracılığıyla GetPerson() işlevi çağırma Şekil 5'te gösterildiği gibi görünmesini Nesne Denetleyici iletişim penceresinde neden olur. Özellikleri nesne içinde değiştirilebilir dinamik olarak bunları vurgulayarak değeri metin kutusunda gösterilen değerini değiştirmek ve güncelleştirme bağlantısını tıklatarak. Nesne Denetimi'ni kullanarak, JSON nesne verilerini görüntüleme ve özellikleri için farklı değerler uygulama ile denemeler basitleştirir.

*Hata ayıklama*

İzleme verilerini ve görüntülenecek JSON nesnelerinin izin ek olarak, Web geliştirme yardımcı ayrıca bir sayfa hataları hata ayıklamaya yardımcı olabilir. Hatayla karşılaşılırsa, kod sonraki satıra devam veya komut dosyası hata ayıklamaya istenir (bkz. Şekil 6). Tam çağrı yığın satır numaralarını yanı sıra sorunları bir komut dosyası içinden nerede kolayca tanıyacak şekilde komut dosyası hatası iletişim penceresini gösterir.


[![Bir JSON nesnesi görüntülemek için nesne Inspector penceresini kullanma.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Şekil 5**: bir JSON nesnesi görüntülemek için nesne Inspector penceresini kullanma.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Hata ayıklama seçeneği değişkenlerin değerini görüntülemek için JSON nesnelerini ayrıca daha fazla yazma doğrudan Web geliştirme yardımcının hemen penceresinde betik deyimlerini yürütmek sağlar. Hatayı tetikleyen aynı eylemi yeniden gerçekleştirilir ve Visual Studio 2008 makinede kullanılabilir ise, bir hata ayıklama oturumu başlatın önceki bölümde açıklandığı gibi satır kod aracılığıyla adım istenir.


[![Web geliştirme yardımcının komut dosyası hata iletişim kutusu](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Şekil 6**: Web geliştirme yardımcının komut dosyası hata iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*İstek ve yanıt iletileri İnceleme*

ASP.NET AJAX sayfaları hata ayıklama sırasında genellikle bir sayfa ve sunucu arasında gönderilen istek ve yanıt iletilerini görmek yararlı olacaktır. İletileri içinde içeriği görüntüleme iletilerinin boyutu hem de uygun verileri geçip geçmediğini görmenizi sağlar. Web geliştirme Yardımcısı, verileri ham metin olarak ya da daha okunabilir bir biçimde görüntülemek kolaylaştırır mükemmel bir HTTP ileti günlükçü özelliği sağlar.

ASP.NET AJAX isteği ve yanıt iletilerini görüntülemek için HTTP Günlükçü Web geliştirme Yardımcısı menüsünden HTTP HTTP günlüğü etkinleştir'i seçerek etkinleştirilmesi gerekir. Bir kez etkinleştirildikten sonra geçerli sayfadan gönderilen tüm iletiler HTTP Göster HTTP günlükleri seçerek erişilebilen HTTP Günlük Görüntüleyici görüntülenebilir.

Her istek/yanıt iletisinde gönderilen ham metin görüntüleme kesinlikle yararlı olsa da (ve Web geliştirme Yardımcısı seçeneği), genellikle fazla grafik biçiminde ileti verilerini görüntülemek kolaydır. HTTP günlüğü etkin ve iletileri günlüğe sonra ileti veri HTTP Günlük Görüntüleyici iletisinde çift tıklayarak görüntülenebilir. Bunun yapılması, gerçek ileti yanı sıra bir ileti ile ilişkili tüm üstbilgileri görüntülemek içerik sağlar. Şekil 7 bir istek iletisi ve HTTP Günlük Görüntüleyici penceresinde görüntülenen yanıt iletisi örneği gösterilmektedir.


[![İstek ve yanıt iletisi verilerini görüntülemek için HTTP günlük Görüntüleyicisi'ni kullanarak.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Şekil 7**: istek ve yanıt iletisi verilerini görüntülemek için HTTP günlük Görüntüleyicisi'ni kullanarak.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


HTTP Günlük Görüntüleyici otomatik olarak JSON nesnelerinin ayrıştırır ve nesnenin özellik verileri görüntülemek kolay ve hızlı hale ağaç görünümünü kullanarak görüntüler. Bir ASP.NET AJAX sayfasında bir UpdatePanel kullanıldığında Görüntüleyici her iletinin tek tek parçalara kısmını çıkışı Şekil 8'de gösterildiği gibi keser. Ham ileti verileri görüntüleme karşılaştırıldığında iletisi nedir anlamak ve görmek çok daha kolay hale getirir harika bir özelliktir.


[![HTTP Günlüğü Görüntüleyicisi'ni kullanarak görüntülenen bir UpdatePanel yanıt iletisi.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Şekil 8**: HTTP Günlüğü Görüntüleyicisi'ni kullanarak bir UpdatePanel yanıt iletisi görüntülenebilir.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Web geliştirme Yardımcısı ek istek ve yanıt iletileri görüntülemek için kullanılan diğer çeşitli araçlar vardır. Başka bir iyi ücretsiz adresten edinilebilir Fiddler seçenektir [http://www.fiddlertool.com](http://www.fiddlertool.com). Fiddler burada incelenecektir değil de, ileti üstbilgilerini ve verileri kapsamlı olarak incelemek gerektiğinde de iyi bir seçenek değil.

## <a name="debugging-with-firefox-and-firebug"></a>Firefox ve Firebug ile hata ayıklama

Internet Explorer en yaygın olarak kullanılan tarayıcı hala durumdayken, Firefox gibi diğer tarayıcılarda oldukça popüler hale ve daha da fazla kullanılıyor. Sonuç olarak, görüntülemek ve ASP.NET AJAX sayfalarınızda Firefox yanı sıra Internet Explorer, uygulamalarınızın düzgün çalıştığından emin olmak için hata ayıklama istersiniz. Hata ayıklama için Visual Studio 2008 doğrudan Firefox tie olamaz rağmen sayfalar hata ayıklamak için kullanılan Firebug adlı bir uzantı var. Firebug indirilebilir ücretsiz giderek [http://www.getfirebug.com](http://www.getfirebug.com).

Firebug tam özellikli ve satır kod üzerinden adım, bir sayfa içinde kullanılan tüm betikler erişim, DOM yapıları görüntülemek, CSS stilleri ve ortaya çıkan bile izleme olayları bir sayfada görüntülemek için kullanılan bir hata ayıklama ortamı sağlar. Bir kez yüklendikten sonra Firebug Firefox menüsünden araçları Firebug açık Firebug seçerek erişilebilir. Ayrıca tek başına bir uygulama olarak kullanılabilse de Web geliştirme Yardımcısı gibi doğrudan tarayıcıda Firebug kullanılır.

Firebug çalışmaya başladıktan sonra komut dosyasını bir sayfaya veya katıştırılmış olup olmadığını kesme noktaları herhangi bir JavaScript dosyası satırında ayarlayabilirsiniz. Bir kesme noktası ayarlamak için ilk Firefox'ta hata ayıklamak istediğiniz uygun sayfaya yükleyin. Sayfa yüklendikten sonra Firebug'ın betikleri aşağı açılan listeden hata ayıklamak için komut dosyasını seçin. Sayfa tarafından kullanılan tüm betikler gösterilir. Bir kesme noktası Firebug'ın gri Tepsisi alanında satır kesme Visual Studio 2008'de yaptığınız gibi gereken nereye tıklayarak ayarlanır.

Bir kesme noktası Firebug içinde ayarladıktan sonra düğmeye tıklama veya yüklendiğinde olayı tetiklemek için tarayıcıyı yenilemeyi gibi ayıklanacak gereken komut dosyasını çalıştırmak için gerekli eylem gerçekleştirebilirsiniz. Yürütme kesme içeren satırı otomatik olarak durur. Şekil 9 Firebug içinde tetiklendi bir kesme noktası örneği gösterilmektedir.


[![Kesme noktaları Firebug içinde işleme.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Şekil 9**: Firebug kesme işleme.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Bir kesme noktası isabet sonra adımla üzerinden adım veya ok düğmelerini kullanarak kodun dışına adım. Kod üzerinden adım olarak, komut dosyası değişkenleri değerlerini görmek ve ayrıntıya nesnelerine izin veren hata ayıklayıcı sağ kısmında görüntülenir. Firebug ayıklanacak geçerli satır açan komut yürütme adımları görüntülemek için bir çağrı yığını açılan listesini de içerir.

Firebug farklı bir komut dosyası deyimleri sınamak için değişkenleri değerlendirmek ve izleme çıktısını görüntülemek için kullanılan bir konsol penceresi de içerir. Firebug pencerenin üstündeki Konsolu sekmesine tıklayarak erişilir. Ayıklanacak sayfa ayrıca "DOM yapısını ve içeriğini inceleyin sekmesinde tıklayarak görmek için denetlenecek". Siz sayfasında uygun kısmı Inspector penceresinde gösterilen farklı DOM öğeler üzerinde fare öğesi sayfasında kullanıldığı görmeyi kolaylaştırır vurgulanır. Belirli bir öğesiyle ilişkili öznitelik değerlerini farklı genişlikleri, stiller, vb. bir öğe olarak uygulama ile denemek için "live" değiştirilebilir. Sürekli olarak ne kadar basit değişiklikler etkileyen bir sayfa görüntülemek için kaynak kod düzenleyicisini ve Firefox tarayıcısı arasında geçiş yapmak gereğini ortadan kaldırır iyi bir özelliktir.

Şekil 10 sayfasında txtCountry adlı bir metin kutusu bulmak için DOM Inspector'ı kullanarak bir örnek gösterilir. Firebug Inspector ayrıca CSS stilleri fare hareketleri, düğme tıklamaları yanı sıra daha fazla izleme gibi meydana gelen olayları yanı sıra bir sayfa kullanılan görüntülemek için kullanılabilir.


[![Firebug'ın DOM Inspector'ı kullanarak.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Şekil 10**: kullanarak Firebug'ın DOM denetçisi.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug hızlı bir şekilde bir sayfasında doğrudan Firefox gibi farklı öğeleri sayfa içinde incelemek için mükemmel bir aracı hata ayıklama için hafif bir yol sağlar.

## <a name="debugging-support-in-aspnet-ajax"></a>ASP.NET AJAX hata ayıklama desteği

ASP.NET AJAX Kitaplığı Web sayfalarına AJAX yetenekleri ekleme işlemini basitleştirmek için kullanılan birçok farklı sınıflar içerir. Bu sınıfların sayfasında öğeleri bulmak ve üzerlerinde değişiklik, yeni denetimler ekleme, Web hizmetlerini çağırır ve hatta olayları işlemek için kullanabilirsiniz. ASP.NET AJAX kitaplığı da hata ayıklama sayfaları işlem artırmak için kullanılan sınıfları içerir. Bu bölümdeki Sys.Debug sınıfına tanıyacak ve uygulamalarda nasıl kullanılabileceğini bakın.

*Sys.Debug sınıfını kullanma*

Sys.Debug sınıfı (Sys ad alanında bulunan bir JavaScript sınıfı), izleme çıktısı yazma, kod onaylar gerçekleştirme ve böylece hata ayıklaması başarısız kodu zorlama dahil birkaç farklı işlevleri gerçekleştirmek için kullanılabilir. Yaygın (C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 sırasında varsayılan olarak yüklenir) ASP.NET AJAX kitaplığın hata ayıklama dosyalarını koşullu testleri (gerçekleştirmek için kullanılır onaylar denir) emin olun parametreler İşlevler, nesneleri beklenen verileri içerdiğini ve izleme deyimleri yazmak için düzgün şekilde geçirilir.

Sys.Debug sınıfı izleme, kod onaylar veya Tablo 1'de gösterildiği gibi hataları işlemek için kullanılan birkaç farklı işlevler sunar.

**Tablo 1. Sys.Debug sınıfı işlevleri.**

| **İşlev adı** | **Açıklama** |
| --- | --- |
| Assert (koşul, ileti, displayCaller) | Koşul parametresi true olduğunu onaylar. Sınanan koşul yanlış ise, bir ileti kutusu ileti parametre değerini görüntülemek için kullanılır. DisplayCaller parametresi true ise, yöntem çağıran hakkında bilgi de görüntüler. |
| clearTrace() | İzleme işlemleri deyimleri çıktısını siler. |
| Fail(Message) | Program yürütme durdurmanız ve hata ayıklayıcısında araya girmektir neden olur. İleti parametresi, başarısızlığın nedenini sağlamak için kullanılabilir. |
| trace(Message) | İleti parametre İzleme çıktısı yazar. |
| traceDump (nesne, ad) | Bir nesnenin veri okunabilir bir biçimde çıkarır. Name parametresi, bir etiket için izleme dökümü sağlamak için kullanılabilir. Tüm alt nesneler yazılan nesne içinde varsayılan olarak yazılır. |

İstemci-tarafı izleme, izleme işlevselliği ASP.NET kullanılabilir kadar aynı şekilde kullanılabilir. Uygulama akışını kesintiye uğratmadan kolayca görülebilir farklı iletileri sağlar. 5 listeleme izleme günlüğüne yazmak için Sys.Debug.trace işlevini kullanarak bir örnek gösterilmektedir. Bu işlev yalnızca bir parametre olarak yazılması gereken iletiyi alır.

**5 listesi. Sys.Debug.trace işlevi kullanıyor.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Listeleme 5'te gösterilen kodu yürütüyorsa sayfasında herhangi bir izleme çıktı görmezsiniz. Görmek için tek yolu, Visual Studio .NET, Web geliştirme Yardımcısı veya Firebug kullanılabilir bir konsol penceresi kullanmaktır. İzleme çıkışı sayfasında görmek istiyorsanız, TextArea etiket ekleme ve sonraki gösterildiği gibi TraceConsole kimliği vermek gerekir:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Tüm sayfa Sys.Debug.trace deyimlerinde TraceConsole TextArea öğesine yazılır.

Bir JSON nesnesi içinde yer alan verileri görmek istediğiniz durumlarda Sys.Debug sınıfının traceDump işlevini kullanabilirsiniz. Bu işlev için izleme konsolu yazılan nesne ve İzleme çıktısı nesnesinde tanımlamak için kullanılan bir ad da dahil olmak üzere iki parametre alır. 6 listeleme traceDump işlevini kullanarak bir örnek gösterilmektedir.

**6 listesi. Sys.Debug.traceDump işlevi kullanıyor.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Şekil 11 Sys.Debug.traceDump işlevi çağırma gelen çıktısını gösterir. Kişi nesnenin verileri yazma ek olarak, bu da adres alt-nesnenin verileri yazdığını dikkat edin.

İzlemenin yanı sıra Sys.Debug sınıfı ayrıca kod onaylar gerçekleştirmek için kullanılabilir. Onaylar, bir uygulama çalışırken belirli koşulların karşılandığından test etmek için kullanılır. Hata ayıklama sürümü, ASP.NET AJAX kitaplığı komut dosyalarının içeren birkaç onay deyimleri koşullar çeşitli test etmek için.

7 listeleyen bir koşulu test etmek için Sys.Debug.assert işlevini kullanarak bir örnek gösterilmektedir. Kod bir kişi nesnesi güncelleştirmeden önce adres nesnesi null olup olmadığını sınar.


[![Sys.Debug.traceDump işlevi çıkışı.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Şekil 11**: Sys.Debug.traceDump işlevinin çıktı.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**7 listesi. Debug.assert işlevi kullanıyor.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Değerlendirmek için onaylama false ve desteklemediğini çağıran hakkında bilgi görüntüleneceğini döndürürse görüntülenecek iletiyi koşulun dahil olmak üzere üç parametre geçirildi. Üçüncü parametre true ise, burada bir onaylama başarısız durumda yanı sıra arayan bilgileri iletisi görüntülenir. Şekil 12 listeleme 7'de gösterilen onaylama başarısız olursa görüntülenen hata iletişim kutusu örneği gösterilmektedir.

Sys.Debug.fail son kapak işlevdir. Kod bir betik içinde belirli bir satırda başarısız zorlamak istediğinizde JavaScript uygulamalarda genelde kullanılan debugger deyimi yerine Sys.Debug.fail çağrısı ekleyebilirsiniz. Sys.Debug.fail işlevi sonraki gösterildiği gibi hatanın nedenini gösteren tek bir dize parametresi kabul eder:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Bir Sys.Debug.assert hatası iletisi.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Şekil 12**: A Sys.Debug.assert hata iletisi.  ([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Bir komut dosyası yürütülürken Sys.Debug.fail deyimi karşılaşıldığında, ileti parametresinin değeri, Visual Studio 2008 gibi bir hata ayıklama uygulamasının konsolunda görüntülenir ve uygulamada hata ayıklama istenir. Visual Studio 2008 ile bir kesme noktası bir satır içi betiği ayarlanamıyor ancak değişkenlerin değerini incelemek için belirli bir satırın durdurmak için kodu ister misiniz burada bu oldukça yararlı olabilir bir durumdur.

*ScriptManager denetimin ScriptMode özelliğini anlama*

ASP.NET AJAX kitaplığı içeren hata ayıklama ve yayın C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 sırasında varsayılan olarak yüklü betik sürümleri. Hata ayıklama komut dosyaları düzgün şekilde biçimlendirilmiş, kolay okunabilir ve yayın betikleri çıkarılır boşluk varsa ve bunların toplam boyutu en aza indirmek için Sys.Debug sınıfı tutumlu kullanmak sırada birkaç Sys.Debug.assert çağrıları bunları dağılmış sahip.

ASP.NET AJAX sayfalara eklenmiş ScriptManager denetimi derleme öğenin hata ayıklama özniteliği hangi yüklemeye kitaplığı komut dosyalarının sürümlerini belirlemek için web.config dosyasında okur. Bununla birlikte, varsa denetleyebilirsiniz hata ayıklama veya yayın betikleri ScriptMode özelliğini değiştirerek olan yüklenen (kitaplığı komut dosyalarının veya özel komut). ScriptMode otomatik, hata ayıklama, sürüm ve devral üyeleri dahil ScriptMode numaralandırma kabul eder.

ScriptMode ScriptManager web.config dosyasında hata ayıklama özniteliği denetleyecektir anlamı otomatik değerini varsayılan olarak ayarlanır. Hata ayıklama false olduğunda ScriptManager ASP.NET AJAX kitaplığı komut dosyalarının sürümü yükleyin. Hata ayıklama true olduğunda komut dosyası hata ayıklama sürümü yüklenir. Serbest bırakma veya hata ayıklama için ScriptMode özelliğini değiştirmek, web.config dosyasında hata ayıklama özniteliğine sahip hangi değer bağımsız olarak uygun komut dosyalarını yüklemek için ScriptManager zorlar. 8 listeleme, ASP.NET AJAX Kitaplığı'ndan hata ayıklama komut dosyaları yüklemek için ScriptManager denetimi kullanma örneği gösterir.

**8 listesi. ScriptManager kullanarak hata ayıklama betikleri yüklenirken**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Farklı sürümleri (hata ayıklama veya yayın), kendi özel komut dosyaları listeleme 9'da gösterildiği gibi ScriptReference bileşen birlikte ScriptManager'ın betikleri özelliğini kullanarak da yükleyebilirsiniz.

**9 listesi. ScriptManager kullanan özel betikler yükleniyor.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> ScriptReference bileşen kullanan özel betikler yüklemekte olduğunuz, komut dosyası komut dosyasının sonuna aşağıdaki kodu ekleyerek yükleme tamamlandığında ScriptManager bildirmeniz gerekir:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Listeleme 9'da gösterilen kodu Person.js yerine Person.debug.js için otomatik olarak görünmesi kişi komut dosyası hata ayıklama sürümü için aranacak ScriptManager söyler. Person.debug.js dosyası bir hata ortaya çıkacaktır bulunmazsa.

Bir hata ayıklama istediğiniz veya ScriptManager denetimi ayarlamak ScriptMode özelliğinin değeri yüklenmesi için özel bir komut dosyası sürümünü bağlı olduğu durumlarda devral için ScriptReference denetimin ScriptMode özelliğini ayarlayabilirsiniz. Bu listeleme 10'da gösterildiği gibi ScriptManager'ın ScriptMode özelliği göre yüklenmesi için özel bir komut dosyası uygun sürümü neden olur. ScriptManager denetimi ScriptMode özelliği Debug ayarlandığından Person.debug.js betik yüklenir ve sayfa içinde kullanılan.

**10 listesi. ScriptManager özel bir komut dosyası içinden ScriptMode devralma.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

ScriptMode özelliğini uygun şekilde kullanarak daha kolay uygulamalarda hata ayıklama ve genel işlemini basitleştirir. ASP.NET AJAX kitaplığın sürüm komut dosyaları aracılığıyla adım ve hata ayıklama betikleri özellikle hata ayıklama amacıyla biçimlendirilir sırada kod biçimlendirme kaldırıldıktan sonra okumak bunun yerine zordur.

## <a name="conclusion"></a>Sonuç

Microsoft ASP.NET AJAX teknolojisi, son kullanıcının genel deneyimini geliştirmek AJAX etkinleştirilmiş uygulamaları oluşturmak için sağlam bir temel sağlar. Ancak, olarak herhangi bir programlama teknolojisiyle hataları ve diğer uygulama sorunlarını kesinlikle ortaya çıkar. Farklı hata ayıklama seçenekleri hakkında bilmek daha kararlı bir üründe çok fazla zaman ve sonuç kaydedebilirsiniz.

Bu makalede, Visual Studio 2008, Web geliştirme Yardımcısı ve Firebug Internet Explorer dahil olmak üzere ASP.NET AJAX sayfaları hata ayıklama için birkaç farklı teknikleri için ekledik. Değişken veri erişebilir, satır kod boyunca size yol ve izleme deyimleri görüntülemek beri bu araçlar genel hata ayıklama işlemini kolaylaştırabilir. Ele alınan farklı hata ayıklama araçlarını yanı sıra, aynı zamanda ASP.NET AJAX kitaplığın Sys.Debug sınıfı bir uygulamada nasıl kullanılacağını ve nasıl ScriptManager sınıfı yüklemek için kullanılabilir hatalarını ayıklama veya komut dosyalarının sürümlerini yayınlama gördüğünüz.

## <a name="bio"></a>Biyografisi

Dan Wahlin (Microsoft en değerli profesyonel ASP.NET ve XML Web Hizmetleri için) olan arabirimi teknik eğitim sırasında bir .NET geliştirme Eğitmen ve mimari Danışman ([www.interfacett.com)](http://www.interfacett.com). Dan kurulan ASP.NET geliştiricilerinin Web sitesi için XML ([www.XMLforASP.NET](http://www.XMLforASP.NET)), üzerinde INETA Konuşmacı kuruluşu olması ve birkaç konferans konuşur. Dan birlikte Professional Windows DNA (Wrox), ASP.NET yazılan: ipuçları, öğreticiler ve kod (kendi), ASP.NET 1.1 Insider çözümleri, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0 MVP yönlendirir ve ASP.NET geliştiricilerinin (kendi) için yazılan XML. Kendisine kod, makaleler veya books değil yazarken, yazma ve müzik kaydetme ve golf ve kendi wife ve çocuklar Basketbol çalma Dan özgürlüğüne.

Tan göstermek Microsoft Web teknolojileri ile bu yana 1997 çalışma ve myKB.com Başkanı ise ([www.myKB.com](http://www.myKB.com)) kendisine ASP.NET yazılırken burada uzmanlaşmış tabanlı Bilgi Bankası yazılım çözümlerini odaklanmış uygulamaları. Tan temas kurulabileceğini doğrula e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog adresindeki [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Önceki](understanding-asp-net-ajax-web-services.md)
