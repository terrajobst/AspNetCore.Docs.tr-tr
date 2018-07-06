---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: ASP.NET AJAX hata ayıklama özelliklerini anlama | Microsoft Docs
author: scottcate
description: Kod hata ayıklama özelliği her geliştirici, kullanmakta olduğunuz teknolojiye bakılmaksızın kendi arsenal olması gereken bir yetenektir. Geliştiricilerin çoğu çalışırken...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 9d420a41f50d06541d04a1dd3cb78a2e6beaaa9a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813834"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>ASP.NET AJAX hata ayıklama özelliklerini anlama
====================
tarafından [Scott Cate](https://github.com/scottcate)

[PDF'yi indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> Kod hata ayıklama özelliği her geliştirici, kullanmakta olduğunuz teknolojiye bakılmaksızın kendi arsenal olması gereken bir yetenektir. Birçok geliştiricinin VB.NET ya da C# kodu kullanan ASP.NET uygulamalarında hata ayıklamak için Visual Studio .NET veya Web Developer Express kullanmaya alışkın olsa da, bazı de JavaScript gibi istemci tarafı kod hata ayıklama için son derece kullanışlıdır, uyumlu değil. .NET uygulamalarında hata ayıklamak için kullanılan teknikleri aynı türde AJAX özellikli uygulamalar ve daha belirgin olarak ASP.NET AJAX uygulamalara uygulanabilir.


## <a name="debugging-aspnet-ajax-applications"></a>ASP.NET AJAX uygulamalarının hatalarını ayıklama

Dan Wahlin

Kod hata ayıklama özelliği her geliştirici, kullanmakta olduğunuz teknolojiye bakılmaksızın kendi arsenal olması gereken bir yetenektir. Bu, mevcut olan farklı hata ayıklama seçeneklerini anlama süresi İnanılmaz derecede bir proje ve hatta belki de bazı zorlukların kaydedebilirsiniz söylemeden gider. Birçok geliştiricinin VB.NET ya da C# kodu kullanan ASP.NET uygulamalarında hata ayıklamak için Visual Studio .NET veya Web Developer Express kullanmaya alışkın olsa da, bazı de JavaScript gibi istemci tarafı kod hata ayıklama için son derece kullanışlıdır, uyumlu değil. .NET uygulamalarında hata ayıklamak için kullanılan teknikleri aynı türde AJAX özellikli uygulamalar ve daha belirgin olarak ASP.NET AJAX uygulamalara uygulanabilir.

Bu makalede, nasıl Visual Studio 2008 ve çeşitli diğer araçları hataları ve diğer sorunları hızlı bir şekilde bulmak için ASP.NET AJAX uygulamalarında hata ayıklamak için kullanılabileceğini göreceksiniz. Bu tartışma Internet Explorer 6 ya da daha yüksek hata ayıklama, kodda adım adım için Visual Studio 2008 ve betik Gezgini kullanarak yanı sıra diğer ücretsiz araçlarla Web geliştirme Yardımcısı gibi etkinleştirme hakkında bilgi içerir. Ayrıca uzantı Firebug olanak sağlayan herhangi bir aracı olmadan doğrudan tarayıcıda JavaScript kodu adım adım adlı Firefox kullanarak ASP.NET AJAX uygulamalarında hata ayıklamak öğreneceksiniz. Son olarak, izleme ve kod onaylama deyimleri gibi çeşitli hata ayıklama görevlerini yardımcı ASP.NET AJAX Kitaplığı'ndaki sınıfları için izleyeceksiniz.

Internet Explorer'da görüntülenen sayfa hata ayıklama denemeden önce birkaç temel adımın hata ayıklama için etkinleştirme işlemini gerçekleştirmek için ihtiyacınız vardır. Başlamak için gerçekleştirilmesi gereken bazı temel kurulum gereksinimleri bir göz atalım.

## <a name="configuring-internet-explorer-for-debugging"></a>Hata ayıklama için Internet Explorer'ı yapılandırma

Çoğu kişi, Internet Explorer ile görüntülenen bir Web sitesinde karşılaştığı JavaScript sorunlarını görmeniz ilgilenen değildir. Aslında, ortalama kullanıcı bile ne bunlar bir hata mesajı gördüğünüz durumunda ne yapacaklarını bilmesi mıydı. Sonuç olarak, hata ayıklama seçenekleri tarayıcı varsayılan olarak kapalıdır. Ancak, hata ayıklama etkinleştirmek ve yeni AJAX uygulamalar geliştirirken kullanmak üzere yerleştirmek çok basittir.

Hata ayıklama işlevselliğini etkinleştirmek için Araçlar menüsünden Internet Seçenekleri ' Internet Explorer gidin ve Gelişmiş sekmesini seçin. Gözatma bölüm içinde aşağıdaki öğeleri işaretli olduğundan emin olun:

- (Internet Explorer) ayıklamasını devre dışı bırak
- (Diğer) ayıklamasını devre dışı bırak

Kullanılmıyorsa gereklidir, ancak büyük olasılıkla JavaScript hataları sayfasında görünür ve belirgin hemen olmasını istersiniz bir uygulamada hata ayıklamak çalışıyorsunuz. "Her komut dosyası hata hakkında bir bildirim göster" onay kutusunu işaretleyerek içeren bir ileti kutusu gösterilecek tüm hataları zorlayabilirsiniz. Bu sırada bir uygulama geliştirirken açmak için harika bir seçenek olmakla birlikte, bunu hızlı bir şekilde JavaScript hatalarla şansınız oldukça iyi çünkü diğer Web siteleri yalnızca harcadığı rahatsız edici dönüşebilir.

Şekil 1 gösterir hangi Internet iletişim Explorer Gelişmiş hata ayıklama için düzgün şekilde yapılandırıldıktan sonra görünmelidir.


[![Hata ayıklama için Internet Explorer'ı yapılandırma.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Şekil 1**: Internet Explorer hata ayıklama için yapılandırma.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Hata ayıklama açık durumda sonra betik hata ayıklayıcısı adlı Görünüm menüsünde görünen yeni bir menü öğesini görürsünüz. Bu sonraki deyimi açık ve kesme dahil olmak üzere kullanılabilecek iki seçenek vardır. Açık seçildiğinde hata ayıklama sayfası Visual Studio 2008 (Visual Web Developer Express ayrıca hata ayıklama için kullanılabileceğini unutmayın) istenir. Visual Studio .NET şu anda çalışıyorsa bu örneği kullanın veya yeni bir örneğini oluşturmak için seçebilirsiniz. Sonraki deyim sonu seçildiğinde JavaScript kod yürütüldüğünde, hata ayıklama sayfası istenir. Sayfa yüklendiğinde olayda JavaScript kodu yürütür, hata ayıklama oturumu tetiklemek için sayfayı yenileyebilirsiniz. Bir düğmeye tıkladı sonra JavaScript kodu çalıştırırsanız hemen düğmesine tıklandığında sonra hata ayıklayıcı çalışır.

> *> [!NOTE] Windows Vista ile kullanıcı erişim denetimi (UAC etkin) çalıştırıyorsanız ve yönetici olarak çalıştırmak için Visual Studio 2008, Visual Studio işleme eklemek için istendiğinde başarısız olur. Bu sorunu geçici olarak çözmek için Visual Studio'yu ilk kez başlatın ve hata ayıklama için bu örneği kullanın.*


Sonraki bölümde, bir ASP.NET AJAX sayfasına doğrudan Visual Studio 2008 içinde hata ayıklamak nasıl sürdürebileceğiniz gösterilecek olsa da, Internet Explorer komut dosyası hata ayıklayıcı seçeneğini kullanarak bir sayfa zaten açık olan ve daha ayrıntılı incelemek istediğiniz yararlı olur.

## <a name="debugging-with-visual-studio-2008"></a>Visual Studio 2008 ile hata ayıklama

Visual Studio 2008, her gün hata ayıklama .NET uygulamaları için dünyanın dört bir yanındaki geliştiricilere güvenmektedir hata ayıklama işlevlerini sağlar. Yerleşik hata ayıklayıcısı, nesne verileri, watch belirli değişkenleri için çağrı yığını ve çok daha fazlasını izleme görünümü kodda adım adım olanak tanır. VB.NET ya da C# kodu hata ayıklama, ek olarak hata ayıklayıcı da ASP.NET AJAX uygulamalarında hata ayıklama için yararlıdır ve satır JavaScript kodda adım adım olanak sağlayacak. Odak bir discourse uygulamaları Visual Studio 2008'i kullanarak hata ayıklama genel süreci hakkında sağlamaya yerine istemci tarafı komut dosyası hata ayıklama için kullanılan teknikleri izlenecek ayrıntıları.

Visual Studio 2008'de bir sayfa hata ayıklama işlemi birkaç farklı şekilde başlatılabilir. İlk olarak, önceki bölümde belirtilen Internet Explorer komut dosyası hata ayıklayıcı seçeneği kullanabilirsiniz. Bu, iyi bir sayfasını tarayıcıda zaten yüklü olan ve bunu hata ayıklamaya başlamak istediğiniz zaman çalışır. Alternatif olarak, bir .aspx sayfasında Çözüm Gezgini'nde sağ tıklayın ve menüden başlangıç sayfası olarak Ayarla'ı seçin. ASP.NET sayfaları hata ayıklama için alışkın değilseniz daha sonra büyük olasılıkla bu önce yaptık. F5'e basıldığında sayfa ayıklanabilir. Ancak, bir kesme noktası herhangi bir yerde genel olarak ayarlayabilirsiniz sonraki gördüğünüz gibi her zaman JavaScript durum değil VB.NET veya C# kod içinde istersiniz.

*Katıştırılmış ve dış betikler*

Visual Studio 2008 hata ayıklayıcı dış JavaScript dosyalardan farklı bir sayfasına gömülü JavaScript değerlendirir. Dış komut dosyalarıyla dosyasını açın ve seçtiğiniz herhangi bir satırında bir kesme noktası ayarlayın. Kesme noktaları, Kod Düzenleyicisi penceresinin sol tarafında gri Tepsisi alana tıklayarak ayarlayabilirsiniz. JavaScript zaman katıştırılmış doğrudan bir sayfa kullanarak `<script>` gri Tepsisi bölümüne tıklayarak bir kesme noktası ayarlamak, etiketi, bir seçenek değildir. Komut dosyası katıştırılmış bir satıra bir kesme noktası ayarlamak için deneme "Bu bir kesme noktası için geçerli bir konum değil" bildiren bir uyarı neden olur.

Bu sorunu geçici olarak bir dış .js dosyasına kod taşıma ve src özniteliğini kullanarak başvuru alabilirsiniz &lt;betik&gt; etiketi:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Değer ne otomatik olarak harici bir dosyaya kod taşıma veya bir seçenek değildir daha fazla çalışma gerekir mi? Düzenleyiciyi kullanarak bir kesme noktası ayarlanamıyor, ancak hata ayıklayıcı deyimi doğrudan hata ayıklamaya başlamak istediğiniz kodun içine ekleyebilirsiniz. Başlatmak için hata ayıklama zorlamak için ASP.NET AJAX Kitaplığı'nda kullanılabilir Sys.Debug sınıfı da kullanabilirsiniz. Bu makalenin devamındaki Sys.Debug sınıfı hakkında daha fazla bilgi edinin.

Kullanma örneği `debugger` anahtar sözcüğü, listeleme 1'de gösterilmiştir. Bu örnek, bir güncelleştirme işlevine bir çağrı yapılır önce doğru hata ayıklayıcının zorlar.

**1 listesi. Visual Studio .NET hata ayıklayıcının zorlamak için hata ayıklayıcı anahtar sözcüğü kullanılarak.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Debugger deyimi yaklaştığınızda hata ayıklama Visual Studio .NET kullanarak sayfası istenir ve kod içerisinde ilerlemeye başlayabilirsiniz. Bunu yaparsanız, Haydi kullanılan ASP.NET AJAX kitaplığı betik dosyalara eriştiğinde bir sorun karşılaşabilirsiniz Visual Studio kullanarak bir göz atalım. Betik Gezgini NET'in.

## <a name="using-visual-studio-net-windows-to-debug"></a>Hata ayıklamak için Visual Studio .NET Windows kullanma

Açık ve kullanılabilir hata ayıklama için kullanılan tüm komut dosyaları olmadığı sürece bir hata ayıklama oturumu başlatılır ve varsayılan F11 tuşuna kullanarak kod walking başlamak için gösterilen hata iletişim kutusu, karşılaşabileceğiniz sonra Şekil 2 bakın.


[![Kaynak kodu hata ayıklama için kullanılabilir olduğunda gösterilen hata iletisi.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Şekil 2**: kaynak kodu hata ayıklama için kullanılabilir olduğunda gösterilen hata iletişim kutusu.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Visual Studio .NET sayfa tarafından başvurulan komut dosyaları bazıları kaynak kodunu almak nasıl emin olarak Önemsiz olduğundan bu iletişim kutusu gösterilir. Bu oldukça can sıkıcı olabilir, ancak ilk olarak, basit bir düzeltme yoktur. Bir hata ayıklama oturumu başlatıldı ve bir kesme noktası isabet sonra Windows betik Gezgini hata ayıklama penceresine git Visual Studio 2008 menüsünden veya Ctrl + Alt + N kısayol tuşu kullanın.

> *> [!NOTE] Betik Gezgini menü listelenen göremiyorsanız, Araçlar* *Özelleştir* *Visual Studio .NET menüsündeki komutları. Kategori bölümünde hata ayıklama girişini bulun ve tüm kullanılabilir menü girişleri göstermek için tıklayın. Komutlar listesinde betik Gezgini aşağı kaydırın ve hata ayıklama sürükleyin* *daha önce bahsedilen Windows menüsünde. Bunun yapılması betik Gezgini menü girişi Visual Studio .NET her çalıştırdığınızda kullandıracağınız.*


Betik Gezgini sayfa içinde kullanılan tüm betikleri görüntülemek ve bunları Kod Düzenleyicisi'nde açmak için kullanılabilir. Betik Gezgini açıldıktan sonra kod düzenleyici penceresinde açmak için şu anda ayıklanan .aspx sayfasına çift tıklayın. Tüm betik Gezgini'nde gösterilen betikleriniz için aynı eylemi gerçekleştirir. Tüm betikler yapabilecekleriniz kod penceresinde açık olduğunda, kodda adım adım için F11 tuşuna (ve diğer hata ayıklama kısayol tuşlarını kullanın). Şekil 3, betik Gezgini örneği gösterilmektedir. Ayrıca iki özel komut dosyaları ve dinamik ASP.NET AJAX ScriptManager sayfasına eklenen iki betik hataları ayıklanmakta olan geçerli dosya (Demo.aspx) listeler.


[![Betik Gezgini sayfa içinde kullanılan betikleri kolayca erişmenizi sağlar.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Şekil 3**. Betik Gezgini sayfa içinde kullanılan betikleri kolayca erişmenizi sağlar.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Birkaç başka windows ayrıca bir sayfa kodda adım adım gibi yararlı bilgiler sağlayan için kullanılabilir. Örneğin, sayfa, hemen penceresinde belirli değişkenler veya koşulları değerlendirin ve çıktısını görüntülemek için kullanılan farklı değişkenlerin değerleri görmek için Yereller penceresine kullanabilirsiniz. İzleme deyimleri (Bu makalenin sonraki bölümlerinde ele alınacak) Sys.Debug.trace işlevi veya Internet Explorer'ın Debug.writeln işlevi kullanılarak yazılmış görüntülemek için çıkış penceresine de kullanabilirsiniz.

Hata ayıklayıcıyı kullanarak kodunuz içinde adım adım olarak atanmış olan değerini görüntülemek için koddaki değişkenleri üzerine gelip. Belirli bir JavaScript değişkenine fare gibi ancak komut dosyası hata ayıklayıcı bazen herhangi bir şey gösterilmez. Değeri görmek için deyim ya da kod düzenleyici penceresinde görmek ve üzerine fare çalıştığınız değişkenini vurgulayın. Bu teknik, her durumda işe yaramazsa karşın, birçok kez, yerel öğeler penceresi gibi farklı hata ayıklama pencerede gezinmeniz gerekmeden değerini görmek mümkün olmayacak.

Burada tartışılan özelliklerin bazıları gösteren bir video öğreticide görüntülenebileceği [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Web geliştirme Yardımcısı'nı kullanarak hata ayıklama

Visual Studio 2008 (ve Visual Web Developer 2008 Express) hata ayıklama araçları çok yetenekli olsa da, daha hafif olan de kullanılabilecek ek seçenek vardır. Yayımlanacak en yeni araçlara Web geliştirme yardımcı olmasıdır. Microsoft'un Nikhil Kothari (Microsoft'ta anahtar ASP.NET AJAX mimarları biri), HTTP istek ve yanıt iletileri görüntülemek için basit hata ayıklamasından çok farklı görev gerçekleştirebilen bu mükemmel araç yazıldı. Web geliştirme Yardımcısı indirilebilir [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Web geliştirme Yardımcısı doğrudan Internet kullanmak uygun hale getiren Explorer içinde kullanılabilir. Araçları Web geliştirme Yardımcısı Internet Explorer menüsünde seçerek başlatılır. HTTP istek ve yanıt iletileri günlüğe kaydetme gibi çeşitli görevleri gerçekleştirmek için tarayıcı bırakmak yoksa güzel tarayıcısı alt kısmında bu Aracı'nı açar. Şekil 4, Web geliştirme Yardımcısı uygulamada nasıl göründüğünü gösterir.


[![Web geliştirme Yardımcısı](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Şekil 4**: Web geliştirme Yardımcısı ([tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Web geliştirme yardımcı olmadığından kullanacağınız bir araç için Visual Studio 2008 içinde satır olarak kodu adımlayın. Ancak, bir komut dosyası değişkenleri değerlendirmek veya keşfedin kolayca izleme çıkışını görüntülemek için kullanılabilir bir JSON nesnesi içinde verilerdir. Ayrıca, bir ASP.NET AJAX sayfa ve bir sunucu gelen ve giden geçirilen verileri görüntülemek için çok yararlı olur.

Web geliştirme Yardımcısı, Internet Explorer'da açıldıktan sonra betik hata ayıklamasını kod etkinleştirme betik hata ayıklamasını Web geliştirme Yardımcısını daha önce Şekil 4'te gösterildiği gibi seçerek etkinleştirilmesi gerekir. Bu, bir sayfa çalışırken oluşan ıntercept hataları araç sağlar. Ayrıca sayfada çıkarılan izleme iletilerini kolayca erişmenizi sağlar. İzleme bilgilerini görüntülemek veya bir sayfa içinde farklı işlevlere test etmek için komut yürütmek için komut dosyasını Göster komut konsol Web geliştirme Yardımcısı menüsünden seçin. Bu, bir komut penceresi ve basit bir komut penceresi erişim sağlar.

*İzleme iletileri ve JSON nesne verileri görüntüleme*

Komut penceresi komutları yürütün veya bile yüklemek veya bir sayfaya farklı işlevlere test etmek için kullanılan betikleri kaydetmek için kullanılabilir. Komut penceresinde, görüntülenmekte olan sayfa tarafından yazılan izleme veya hata ayıklama iletilerini görüntüler. Kod 2 Internet Explorer'ın Debug.writeln işlevi kullanarak bir izleme iletisi yazma işlemi gösterilmektedir.

**2 listesi. Yazma hata ayıklama sınıfını kullanarak bir istemci-tarafı izleme iletisi.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Soyadı özelliği Doe değerini içeriyorsa, Web geliştirme Yardımcısı iletisini görüntüler "kişi adı: Doe" betik konsolun komut penceresinde (hata ayıklama etkinleştirilmiş olduğunu varsayarak). Web geliştirme Yardımcısı, izleme bilgilerini yazma ya da JSON nesnelerinin içeriğini görüntülemek için kullanılan sayfalarına ayrıca bir en üst düzey debugService nesnesi ekler. Kod 3 debugService sınıfın izleme işlevini kullanarak bir örnek gösterilmektedir.

**3 listesi. İzleme ileti yazmak için Web geliştirme yardımcının debugService sınıf kullanarak.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Internet Explorer Web geliştirme Yardımcısı çalışırken her zaman izleme verilerine erişmek kolaylaştıran hata ayıklama etkin değil olsa bile bu çalışır yer debugService sınıfı güzel bir özelliğidir. Bir sayfa hata ayıklamak için aracı kullanılmadığında window.debugService çağrısı false döndüreceği olduğundan izleme deyimleri yoksayılacak.

DebugService sınıfı ayrıca JSON nesne verilerini Web geliştirme yardımcının denetçisi penceresini kullanarak görüntülenmesine izin verir. 4 listeleme kişi verilerini içeren basit bir JSON nesnesi oluşturur. Nesne oluşturulduktan sonra bir çağrı yapılır debugService için görsel olarak denetlenecek JSON nesnesi işlevini sınıfın inceleyin.

**4 listesi. JSON nesne verilerini görüntülemek için debugService.inspect işlevi kullanıyor.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Şekil 5'te gösterildiği gibi görünen nesne denetçisi iletişim penceresinde sayfasında ya da komut penceresi GetPerson() işleve çağrı sonuçlanır. Özellikleri nesnesi içinde değiştirilebilir dinamik olarak bunları vurgulayarak değeri metin kutusunda gösterilen değerini değiştirmeyi ve ardından güncelleştirme bağlantısını. Nesne Inspector'ı kullanarak basit JSON nesne verilerini görüntüleme ve farklı değerler özelliklerine uygulama ile denemeler yapar.

*Hata ayıklama*

İzleme verileri ve JSON nesneleri görüntülenecek izin ek olarak, Web geliştirme yardımcı ayrıca sayfasında hata ayıklama içinde yardımcı olur. Bir hatayla karşılaştı, kodun sonraki satırına devam etmek için komut dosyası hata ayıklama istenir (bkz. Şekil 6). Tam çağrı yığın satır numaralarını yanı sıra sorunları komut dosyası içinden nerede kolayca tanımlayabilirsiniz böylece betik hatası iletişim kutusu penceresini gösterir.


[![Bir JSON nesnesi görüntülemek için nesne Inspector penceresini kullanarak.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Şekil 5**: bir JSON nesnesi görüntülemek için nesne Inspector penceresini kullanarak.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Hata ayıklama seçeneğini belirleyerek, değişkenlerin değerini görüntülemek için JSON nesneleri ayrıca daha fazla yazma doğrudan Web geliştirme yardımcının hemen penceresinde komut dosyası deyimlerini yürütmek sağlar. Hatayı tetikleyen aynı eylemi yeniden gerçekleştirilir ve Visual Studio 2008 makinede kullanılabilir, böylece kod önceki bölümde açıklandığı gibi satır boyunca adım hata ayıklama oturumu başlatmak için istenir.


[![Web geliştirme yardımcının betik hata iletişim kutusu](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Şekil 6**: Web geliştirme yardımcının betik hata iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*İstek ve yanıt iletilerinin İnceleme*

ASP.NET AJAX sayfa hata ayıklama sırasında genellikle bir sayfa ile sunucu arasında gönderilen istek ve yanıt iletilerini görmek yararlı olur. İleti içeriği görüntüleme, yanı sıra iletileri boyutunu uygun verileri geçip geçmediğini görmek sağlar. Web geliştirme Yardımcısı, verileri ham metin ya da daha okunabilir bir biçimde görüntülemek kolaylaştırır mükemmel bir HTTP ileti günlükçüsü özelliği sağlar.

ASP.NET AJAX isteği ve yanıt iletileri görüntülemek için HTTP günlüğü Web geliştirme Yardımcısı menüsünden HTTP HTTP günlüğü etkinleştir'i seçerek etkinleştirilmesi gerekir. Etkinleştirildikten sonra geçerli sayfadan gönderilen tüm iletilerin HTTP HTTP günlükleri Göster'i seçerek erişilebilen HTTP Günlük Görüntüleyici görüntülenebilir.

Her istek/yanıt iletisinde gönderilen ham metni görüntüleme kesinlikle kullanışlı olsa da (ve Web geliştirme Yardımcısı seçeneği), genellikle bir grafik biçiminde ileti verilerini görüntülemek kolaydır. HTTP günlüğü etkinleştirilmişse ve iletileri günlüğe sonra ileti veri HTTP Günlük Görüntüleyici iletisinde çift tıklayarak görüntülenebilir. Bunun yapılması, gerçek iletinin yanı sıra bir ileti ile ilişkili tüm üst bilgileri görüntülemek içerik sağlar. Şekil 7, bir istek iletisi ve HTTP Günlük Görüntüleyici penceresinde görüntülenen yanıt iletisi örneği gösterilmektedir.


[![İstek ve yanıt iletisi verilerini görüntülemek için HTTP Günlük Görüntüleyici kullanarak.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Şekil 7**: istek ve yanıt iletisi verilerini görüntülemek için HTTP Günlük Görüntüleyici kullanarak.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


HTTP Günlük Görüntüleyici, otomatik olarak JSON nesneleri ayrıştırır ve bunları kullanarak hızla ve kolayca nesnenin özellik verileri görüntülemek ağaç görünümünü görüntüler. Bir ASP.NET AJAX sayfa UpdatePanel kullanıldığında Görüntüleyicisi her iletinin tek tek parçalara kısmını kullanıma Şekil 8'de gösterildiği gibi keser. Bu çok daha kolay görmek ve iletinin ileti ham verileri görüntüleme karşılaştırıldığında ne olduğunu anlamak yaptığı harika bir özelliğidir.


[![HTTP Günlük Görüntüleyici kullanarak bir UpdatePanel yanıt iletisi.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Şekil 8**: bir UpdatePanel yanıt iletisini görüntülerse HTTP günlük Görüntüleyicisi'ni kullanma.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Web geliştirme Yardımcısı ek istek ve yanıt iletilerini görüntülemek için kullanılan diğer birçok araç vardır. Kullanılabilir ücretsiz olan Fiddler başka bir iyi seçenek, [ http://www.fiddlertool.com ](http://www.fiddlertool.com). Fiddler burada açıklanmıştır değil olsa da, ileti üstbilgileri ve verileri kapsamlı olarak incelemek, ihtiyacınız olduğunda bu da iyi bir seçenektir.

## <a name="debugging-with-firefox-and-firebug"></a>Firebug ve Firefox ile hata ayıklama

Internet Explorer en yaygın olarak kullanılan tarayıcı yine de olsa da, Firefox gibi diğer tarayıcılar oldukça popüler hale gelmiştir ve daha da fazla kullanıldığını görebilirsiniz. Sonuç olarak, ASP.NET AJAX sayfalarınıza Firefox hem de Internet Explorer, uygulamalarınızın düzgün çalışmasını sağlamak için hata ayıklama ve görüntülemek isteyebilirsiniz. Hata ayıklama için doğrudan Visual Studio 2008 ile Firefox tie olamaz ancak sayfaları hata ayıklamak için kullanılan Firebug adlı bir uzantı var. Firebug yüklenebilir ücretsiz giderek [ http://www.getfirebug.com ](http://www.getfirebug.com).

Firebug tam özellikli ve satır kodda adım adım, bir sayfa içinde kullanılan tüm betikleri erişmek, DOM yapıları görüntüleyin, CSS stilleri ve hatta meydana gelen olayları izlemek bir sayfasında görüntülenen kullanılan bir hata ayıklama ortamı sağlar. Yüklendikten sonra Firebug araçları Firebug açık Firebug Firefox menüden tarafından erişilebilir. Tek başına bir uygulama olarak ayrıca kullanılabilir ancak Web geliştirme Yardımcısı gibi doğrudan tarayıcınızda Firebug kullanılır.

Firebug çalıştırıldıktan sonra kesme noktaları, komut dosyasını bir sayfaya veya katıştırılmış olup olmadığını bir JavaScript dosyası herhangi bir satırında ayarlanabilir. Bir kesme noktası ayarlamak için uygun sayfanın Firefox'ta hata ayıklamak istediğiniz ilk yükleyin. Sayfa yüklendiğinde Firebug'ın betikleri aşağı açılan listeden hata ayıklamak için betik seçin. Sayfa tarafından kullanılan tüm betikler gösterilir. Bir kesme noktası Firebug'ın gri Tepsisi alanında satırında kesme noktası Visual Studio 2008'de yaptığınız gibi gereken nereye tıklayarak ayarlanır.

Firebug içinde bir kesme noktası ayarlandıktan sonra bir düğmeye tıklayarak veya yüklendiğinde olayı tetiklemek için tarayıcıyı yenilemeyi gibi hata ayıklama için gereken betik yürütmek için gerekli eylem gerçekleştirebilirsiniz. Yürütme kesme noktasını içeren satırdaki otomatik olarak durdurulur. Şekil 9 Firebug tetiklendiğini bir kesme noktası örneği gösterilmektedir.


[![Kesme noktaları Firebug işleme.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Şekil 9**: Firebug kesme noktaları işleme.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Bir kesme noktası yaklaştığınızda Adımlama, Atla veya ok düğmelerini kullanarak kodların dışına adım. Kodunuz içinde adım adım olarak, komut dosyası değişkenleri değerleri görür ve detaya gitme nesnelerini olanak tanıyan hata ayıklayıcı sağ kısmındaki gösterilir. Firebug hataları ayıklanmakta olan geçerli satırı açan betiğin yürütme adımları görüntülemek için bir çağrı yığını açılır listede de içerir.

Firebug farklı komut dosyası deyimlerini test değişkenleri değerlendirmek ve izleme çıkışını görüntülemek için kullanılabilir bir konsol penceresi de içerir. Firebug pencerenin üst kısmındaki Konsolu sekmesine tıklayarak erişilir. Hataları ayıklanmakta olan sayfa da "inceleyin sekmesinde tıklayarak DOM yapısını ve içeriğini görmek için denetlenecek". Siz uygun kısmı sayfa denetçisi penceresinde gösterilen farklı DOM öğeleri üzerinde fare öğenin sayfanın kullanıldığı görmek kolaylaştıran vurgulanır. Belirli bir öğeyle ilişkili öznitelik değerleri, "Canlı farklı genişlikleri, stiller vb. bir öğeye uygulanıyor ile denemek için" değiştirilebilir. Sürekli olarak nasıl basit değişiklikler etkileyen bir sayfasını görüntülemek için kaynak kod düzenleyicisi ve Firefox tarayıcısı arasında geçiş yapmak zorunda kalmaktan kurtarır güzel bir özelliktir.

Şekil 10 txtCountry sayfasında adlı bir metin kutusu bulmak için DOM Inspector'ı kullanarak bir örnek gösterilmektedir. Firebug Inspector ayrıca CSS stilleri kullanılan bir sayfaya ek olarak, fare hareketlerini, düğme tıklamaları yanı sıra daha fazla izleme gibi meydana gelen olayları görüntülemek için kullanılabilir.


[![Firebug'ın DOM denetçisini kullanma.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Şekil 10**: kullanarak Firebug'ın DOM Inspector'ı.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug hafif bir sayfaya doğrudan Firefox gibi farklı öğeler sayfada incelemek için mükemmel bir aracı hızla hata ayıklama olanağı sağlar.

## <a name="debugging-support-in-aspnet-ajax"></a>ASP.NET AJAX hata ayıklama desteği

ASP.NET AJAX kitaplığı, bir Web uygulamasına AJAX özellikleri ekleme işlemini basitleştirmek için kullanılabilecek birçok farklı sınıflar içerir. Bu sınıflar, bir sayfa içinde öğeleri bulmak ve üzerlerinde değişiklik, yeni denetimler ekleme, Web hizmetlerini çağırır ve hatta olayları işlemek için kullanabilirsiniz. ASP.NET AJAX kitaplığı ayrıca hata ayıklama sayfaların sürecini iyileştirmek için kullanılan sınıfları içerir. Bu bölümdeki Sys.Debug sınıfa izleyeceksiniz ve uygulamalarda nasıl kullanılabileceğini öğrenin.

*Sys.Debug sınıfını kullanma*

İzleme çıktısına yazma, kod onaylar gerçekleştirme ve böylece hata ayıklama gerçekleştirilebilir başarısız için kod zorlama gibi birçok farklı işlevleri gerçekleştirmek için Sys.Debug sınıfı (Sys ad alanında bulunan bir JavaScript sınıfı) kullanılabilir. Kapsamlı bir şekilde (varsayılan olarak C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 yüklü) ASP.NET AJAX kitaplığın hata ayıklama dosyalarını (koşullu testleri gerçekleştirmek için kullanılır Onaylamalar olarak adlandırılır) emin olun parametreler İşlevler, nesneler beklenen verileri içerdiğini ve izleme deyimleri yazmak için düzgün şekilde geçirilir.

Sys.Debug sınıfı, izleme, kod onaylar veya Tablo 1'de gösterildiği gibi hataları işlemek için kullanılan birkaç farklı işlevlerini gösterir.

**Tablo 1. Sys.Debug sınıf işlevleri.**

| **İşlev adı** | **Açıklama** |
| --- | --- |
| Assert (koşul, message, displayCaller) | Koşul parametresi doğru olduğunu onaylar. Test edilen koşul yanlışsa bir ileti kutusu iletisi parametre değerini görüntülemek için kullanılır. DisplayCaller parametre true ise, yöntemin arayanı hakkında bilgi de görüntüler. |
| clearTrace() | İzleme işlemleri deyimleri çıktısını siler. |
| Fail(Message) | Program yürütme durdurmak ve hata ayıklayıcıyı durdurmak neden olur. İleti parametresi, hatanın nedenini sağlamak için kullanılabilir. |
| trace(Message) | İleti parametre izleme çıktısına yazar. |
| traceDump (object, adı) | Bir nesnenin veri okunabilir bir biçimde çıkarır. Name parametresi, bir etiket için izleme dökümü sağlamak için kullanılabilir. Tüm alt nesneleri yazılan nesne içinde varsayılan olarak yazılır. |

İstemci-tarafı izleme, ASP.NET'te kullanılabilen izleme işlevi çok aynı şekilde kullanılabilir. Uygulama akışını kesintiye uğratmadan kolayca görülebilmesi farklı iletileri sağlar. 5 listeleme izleme günlüğüne yazmak için Sys.Debug.trace işlevi kullanarak bir örnek gösterilmektedir. Bu işlev, yalnızca bir parametre olarak yazılması belirten bir ileti alır.

**5 listesi. Sys.Debug.trace işlevi kullanıyor.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Listeleme 5'te gösterilen kod yürütme, sayfadaki tüm izleme çıktısına görmeyeceksiniz. Bunu görmek için tek yolu, Visual Studio .NET, Web geliştirme Yardımcısı veya Firebug kullanılabilir bir konsol penceresi kullanmaktır. Ardından, izleme çıkışını sayfada görmek istiyorsanız TextArea etiket ekleyin ve aşağıda gösterildiği TraceConsole kimliğini verin gerekecektir:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Tüm sayfa Sys.Debug.trace deyimlerinde TraceConsole TextArea öğesine yazılır.

Bir JSON nesnesinde yer alan verileri görmek için istediğiniz durumlarda Sys.Debug sınıfın traceDump işlevi kullanabilirsiniz. Bu işlev izleme konsoluna yazılan nesne ve izleme çıktısına nesnesinde tanımlamak için kullanılan adı dahil olmak üzere iki parametre alır. 6 listeleme traceDump işlevini kullanarak bir örnek gösterilmektedir.

**6 listesi. Sys.Debug.traceDump işlevi kullanıyor.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Şekil 11 Sys.Debug.traceDump işleve çağrı gelen çıktı gösterir. Kişi nesnesinin verileri yazmak ek olarak, bu da adres alt-nesnenin veri çıkışı yazdığını dikkat edin.

İzlemenin yanı sıra Sys.Debug sınıf kodunu onaylar gerçekleştirmek için de kullanılabilir. Onaylar, uygulamanın çalıştığı sırada belirli koşulların karşılandığından test etmek için kullanılır. ASP.NET AJAX kitaplığı betikler hata ayıklama sürümünü içeren bazı onay deyimleri çeşitli koşulları test etmek için.

7 listeleyen bir koşulu test Sys.Debug.assert işlevini kullanarak bir örnek gösterilmektedir. Kod, bir kişi nesnesinin güncelleştirmeden önce adresi nesnenin null olup olmadığını test eder.


[![Sys.Debug.traceDump işlev çıkışı.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Şekil 11**: Sys.Debug.traceDump işlev çıkışı.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**7 listesi. Debug.assert işlevi kullanıyor.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Koşulu değerlendirmek için false ve çağıran hakkında bilgi görüntülenen olup olmadığını onaylama döndürürse, görüntülenecek iletiyi dahil olmak üzere üç parametre geçirildi. Üçüncü parametre true ise burada bir onaylama işlemi başarısız olduğunda yanı sıra arayan bilgileri iletisi görüntülenir. Şekil 12 listeleme 7'de gösterilen onaylama başarısız olursa hata iletişim örneği gösterilmektedir.

Son işlevi kapsayacak şekilde Sys.Debug.fail ' dir. Belirli bir satıra bir betikte hata vermesine kod zorlamak istediğinizde, genellikle JavaScript uygulamalarında kullanılan hata ayıklayıcı deyimi yerine Sys.Debug.fail çağrı ekleyebilirsiniz. Sys.Debug.fail işlevi, başarısızlığın nedenini sonraki gösterildiği temsil eden tek bir dize parametresi kabul eder:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Sys.Debug.assert hata iletisi.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Şekil 12**: A Sys.Debug.assert hata iletisi.  ([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Bir betiği yürütülürken Sys.Debug.fail deyimi karşılaşıldığında, ileti parametresinin değerini, Visual Studio 2008 gibi bir hata ayıklama uygulamasının konsolunda görüntülenir ve uygulamanın hatasını ayıklama istenir. Bir satır içi betik üzerinde Visual Studio 2008 ile bir kesme noktası ayarlanamıyor, ancak değişkenlerin değerini inceleyebilirsiniz. Bu nedenle, belirli bir satırda durdurmak için kodu istediğiniz zaman burada bu oldukça faydalı olabilir bir durumdur.

*ScriptManager denetimin ScriptMode özelliği anlama*

ASP.NET AJAX kitaplığı içeren hata ayıklama ve yayın betik sürümleri, C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 varsayılan olarak yüklenir. Hata ayıklama betikleri düzgün şekilde biçimlendirilmiş, kolay okunabilir ve yayımlanan betikler çıkarılır boşluk olması ve Sys.Debug sınıfı boyutlarına genel en aza indirmek için tedbirli şekilde kullanın. çeşitli Sys.Debug.assert çağrılar bunları dağılmış sahip.

ASP.NET AJAX sayfalarına eklenmiş bir ScriptManager denetimi kitaplığı betikleri yüklemek için hangi sürümlerini belirlemek için web.config dosyasındaki derleme öğe hata ayıklama özniteliği okur. Ancak, varsa denetleyebilirsiniz hata ayıklama veya sürüm betikleri ScriptMode özelliğini değiştirerek olan yüklenen (kitaplık betikleri veya kendi özel komut dosyaları). ScriptMode otomatik, hata ayıklama, yayın ve devral üyeleri içeren bir ScriptMode numaralandırması kabul eder.

ScriptMode ScriptManager web.config dosyasında hata ayıklama öznitelik denetleyecek anlamına gelir. otomatik değerini varsayılan olarak ayarlanır. Hata ayıklama false olduğunda ScriptManager ASP.NET AJAX kitaplığı komut dosyalarının sürümü yüklenir. Hata ayıklama true olduğunda, komut dosyası hata ayıklama sürümü yüklenir. Hata ayıklama veya sürüm ScriptMode özelliğini değiştirme web.config dosyasında hata ayıklama özniteliğine sahip hangi değeri bağımsız olarak uygun komut dosyalarını yüklemek için ScriptManager zorlar. 8 hata ayıklama betikleri ASP.NET AJAX Kitaplığı'ndan yüklemeye ScriptManager denetimini kullanma örneği gösterir.

**8 listesi. ScriptManager kullanarak hata ayıklama betikler yüklüyor**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Kendi özel betikler'ün farklı sürümlerini (hata ayıklama veya sürüm) listeleme 9'da gösterildiği gibi ScriptReference bileşen birlikte ScriptManager'ın komut dosyaları özelliğini kullanarak da yükleyebilirsiniz.

**9 listesi. ScriptManager kullanan özel betikler yükleniyor.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> ScriptReference bileşenini kullanarak özel betikler yüklüyor, betik aşağıdaki kodu betik sayfanın en ekleyerek yüklenmesi sonlandığında ScriptManager bildirmeniz gerekir:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

ScriptManager Person.debug.js Person.js yerine için otomatik olarak görünmesi için kişi betik hata ayıklama sürümü aramak için listeleme 9'da gösterilen kod söyler. Bir hata oluşacaktır Person.debug.js dosyası bulunamazsa.

ScriptManager denetiminde ScriptMode özelliği değerini yüklenecek özel bir betik sürümü temel veya burada bir hata ayıklama istediğiniz durumlarda için devral ScriptReference denetimin ScriptMode özelliğini ayarlayabilirsiniz. Bu listeleme 10'da gösterildiği gibi ScriptManager'ın ScriptMode özelliği göre uygun sürümünün yüklenmesi için özel betik neden olur. ScriptManager denetimini ScriptMode özelliği için hata ayıklama ayarlandığından Person.debug.js betik yüklenir ve sayfa içinde kullanılan.

**10 listesi. ScriptMode ScriptManager özel komut dosyaları için'öğesinden devralıyor.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

ScriptMode özelliğini uygun şekilde kullanarak daha kolay uygulamalarında hata ayıklamak ve genel işlem basitleştirin. ASP.NET AJAX kitaplığın sürüm betikleri adım adım ve hata ayıklama betikleri özellikle hata ayıklama amacıyla biçimlendirilir sırasında kod biçimlendirme kaldırılmış olduğundan okumak yerine zordur.

## <a name="conclusion"></a>Sonuç

Microsoft ASP.NET AJAX teknolojisi, son kullanıcının genel deneyimini geliştirmek AJAX özellikli uygulamalar oluşturmak için sağlam bir temel sağlar. Ancak, olarak herhangi bir programlama teknolojisiyle, hataları ve diğer uygulama sorunları kesinlikle ortaya çıkar. Çok fazla zaman ve sonucu farklı hata ayıklama seçenekleri hakkında bilmek daha kararlı bir ürüne kaydedebilirsiniz.

Bu makalede, ASP.NET AJAX sayfa Internet Explorer ile Visual Studio 2008 ve Web geliştirme Yardımcısı Firebug dahil olmak üzere hata ayıklama için birkaç farklı teknik için tanışmış oldunuz. Değişken veri erişebilir, kod satırlarını açıklaması ve izleme deyimleri görüntülemek olduğundan bu araçlar genel hata ayıklama işlemini kolaylaştırabilir. Ele alınan farklı hata ayıklama araçlarını ek olarak, aynı zamanda ASP.NET AJAX Kitaplığı'nızın Sys.Debug sınıfı bir uygulamada nasıl kullanılabileceğini ve yüklenecek ScriptManager sınıfı'nın nasıl kullanılabileceğini hata ayıklama veya sürüm betikleri sürümleri gördüğünüz.

## <a name="bio"></a>Biyografi

Dan Wahlin (Microsoft en değerli Professional ASP.NET ve XML Web Hizmetleri) olan arabirimi teknik eğitim sırasında bir .NET geliştirme Eğitmen ve mimari Danışman ([www.interfacett.com)](http://www.interfacett.com). Dan kurulan ASP.NET geliştiricilerinin Web sitesi için XML ([www.XMLforASP.NET](http://www.XMLforASP.NET)) üzerinde INETA konuşmacının kuruluşu olan ve çeşitli konferanslarda konuşur. Dan Professional Windows DNA (Wrox), ASP.NET yazarlarından: ipuçları, öğreticiler ve kod (kendi), ASP.NET 1.1 Insider çözümleri, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0 MVP yönlendirir ve ASP.NET geliştiricileri (kendi) için yazılan XML. He kodu, makaleler veya books değil yazarken, yazma ve müzik kaydetme ve golf ve kendi eşim ve çocuklar Basketbol yürütme Dan ölçeklenebilme.

Scott Cate 1997'den beri Microsoft Web teknolojileri ile çalışmakta olduğu ve myKB.com Yardımcısı ([www.myKB.com](http://www.myKB.com)) tabanlı Bilgi Bankası yazılım çözümlerinizi odaklı uygulamaları burada kendisinin ASP.NET yazma konusunda uzmanlaşmış. Scott temas kurulabileceğini e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog'da [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Önceki](understanding-asp-net-ajax-web-services.md)
