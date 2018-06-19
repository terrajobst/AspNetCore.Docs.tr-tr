---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: ASP.NET Web sayfaları sunarak - temel programlama | Microsoft Docs
author: tfitzmac
description: "Bu öğretici, Razor sözdizimi ile ASP.NET Web sayfalarını program hakkında genel bakış sağlar. Öğrenecekleriniz: pr için kullandığınız temel 'Razor' sözdizimi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/17/2015
ms.topic: article
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 60115dd06a27bf856427953de29e993194afb991
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33839292"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>ASP.NET Web sayfalarını - Programlama temelleri tanıtma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici, Razor sözdizimi ile ASP.NET Web sayfalarını program hakkında genel bakış sağlar.
> 
> Öğrenecekleriniz:
> 
> - Programlama, ASP.NET Web sayfaları için kullandığınız temel "Razor" sözdizimini.
> - Bazı temel C kullanacağınız programlama dili olan #.
> - Bazı temel programlama kavramları Web sayfaları için.
> - Siteniz ile kullanmak için (önceden oluşturulmuş kodu içeren bileşenleri) paketleri yükleme.
> - Nasıl kullanılacağını *Yardımcıları* genel programlama görevleri gerçekleştirmek için.
>   
> 
> Özellikler/teknolojilerini ele alınan:
> 
> - NuGet ve Paket Yöneticisi.
> - `Gravatar` Yardımcısı.


Bu öğreticide, ASP.NET Web sayfaları için kullanacağınız programlama sözdizimi için piyasaya, öncelikle bir uygulamadır. Hakkında bilgi edineceksiniz *Razor sözdizimi* ve C# dilinde yazılan kod programlama dili. Önceki öğreticide bir bakışta ve bu sözdiziminin aldığınız; Bu öğreticide daha fazla sözdizimi açıklayacağız.

Bu öğreticide, tek bir öğreticide görürsünüz ve olduğundan yalnızca öğretici olduğunu programlama çoğu içerdiğini etmeyeceğiz *yalnızca* programlama hakkında. Bu kümesindeki kalan eğitimlerine ilginç şeyler sayfaları gerçekten oluşturacaksınız.

Hakkında bilgi edineceksiniz *Yardımcıları*. Bir yardımcı bir bileşen olan — paketlenmiş yukarı paylaştırılabilen bir kod —, bir sayfasına ekleyebilirsiniz. Yardımcı çalışma sıkıcı veya el ile yapmak için karmaşık Aksi durumda olabilecek sizin için gerçekleştirir.

## <a name="creating-a-page-to-play-with-razor"></a>Razor ile yürütmek için bir sayfa oluşturma

Temel sözdizimi duygusu alabilmek için bu bölümde, bir bit Razor ile yürütmek.

Zaten çalışıyorsa, WebMatrix başlatın. Önceki öğreticide oluşturduğunuz Web sitesi kullanacağınız ([Web sayfaları ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkId=251578)). Yeniden açmak için tıklatın **Sitelerim** ve **WebPageMovies**:

![WebMatrix Başlangıç ekranına Sitelerim vurgulanmış ve açık Site seçenekleri gösterme](intro-to-web-pages-programming/_static/image1.png)

Seçin **dosyaları** çalışma.

Şeritte tıklatın **yeni** bir sayfa oluşturmak için. Seçin **CSHTML** ve yeni sayfa adı *TestRazor.cshtml*.

**Tamam**'ı tıklatın.

Aşağıdaki tamamen zaten değiştirmek dosyasına kopyalayın.

> [!NOTE]
> Bir sayfaya örneklerinden kod veya işaretleme kopyaladığınızda, girinti ve hizalama öğretici ile aynı olmayabilir. Girinti ve hizalama kodu, ancak çalışma şeklini etkilemez.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Örnek sayfası inceleniyor

Gördüğünüz çoğu sıradan HTML olur. Ancak, en üstte Bu kod bloğu vardır:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Bu kod bloğunu aşağıdaki çalışmalarıdır dikkat edin:

- Aşağıda Razor kodu, HTML değil. @ karakteri ASP.NET söyler. ASP.NET sonra her şeyi kabul @ karakteri bazı HTML'e yeniden çalışana kadar kod olarak. (Bu durumda, o &lt;! DOCTYPE&gt; öğesi.
- Küme ayraçları ({ve}) kodu birden fazla satır varsa bir Razor kod bloğunun içine alın. Küme ayraçları burada kod bloğu için başlar ve biter ASP.
- / / Karakter işaretlemek yorum — diğer bir deyişle, bir kod parçası, yürütülen olmaz.
- Noktalı virgül (;) sonlandırmak her Ekstre içerir. (, Ancak yorumu değil.)
- Değerleri depolayabilir *değişkenleri*, oluşturduğunuz (*bildirme*) anahtar sözcüğü örneklemeyi ile Bir değişkeni oluşturduğunuzda, onu bir ad harf, rakam ve alt çizgi içerebilir size (\_). Değişken adları bir rakamla başlayamaz ve programlama bir anahtar sözcüğü (gibi var) adı kullanamazsınız.
- Karakter dizeleri (örneğin, "ASP.NET" ve "Web sayfaları") tırnak işaretleri içine alın. (Bunların çift tırnak işaretleri olması gerekir.) Sayı tırnak işaretleri içine değil.
- Tırnak işaretleri dışında boşluk önemli değildir. Satır sonları çoğunlukla önemi yoktur; satıra bir dize tırnak işaretleri içindeki bölünemez istisnadır. Girinti ve hizalama önemi yoktur.

Bu örnekte belirgin olmayan tüm kod büyük küçük harfe duyarlı olduğunu şeydir. Bu değişken TheSum theSum veya thesum adlandırılabilir değişkenleri daha farklı bir değişken anlamına gelir. Benzer şekilde, bir anahtar sözcük var olan, ancak Var değil.

### <a name="objects-and-properties-and-methods"></a>Nesneleri ve özellikleri ve yöntemleri

Ardından DateTime.Now ifade yoktur. DateTime basitçe olan bir *nesne*. Bir nesne ile program şeydir — bir sayfa, metin kutusuna, dosya, bir görüntü, web isteği, bir e-posta iletisi, bir müşteri kaydı vb. Nesneler sahip bir veya daha fazla *özellikleri* özelliklerini açıklar. Bir metin kutusu nesnesi (diğerlerinin yanı sıra) metin özelliğine sahiptir, bir istek nesnesi, bir Url özelliği (ve diğerleri) sahiptir, e-posta iletisine bir From özelliğine ve bir Kime özelliğine sahiptir ve benzeri. Nesneler de *yöntemleri* "bunların fiiller" olan. Nesneleriyle çok çalışıyor.

Örnekte görebildiğiniz gibi DateTime program tarihler ve saatler olanak sağlayan bir nesnedir. Geçerli tarih ve saati döndürür artık adlı bir özellik vardır.

### <a name="using-code-to-render-markup-in-the-page"></a>Kod sayfasındaki biçimlendirmesi oluşturmak için kullanma

Sayfasının gövdesinde aşağıdakilere dikkat edin:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Yeniden @ karakteri, aşağıda ASP kodu, HTML değil. Biçimlendirme @ bir kod ifadesi tarafından izlenen ekleyebileceğiniz ve ASP.NET ifade hak değerini bu noktada oluşturmaz. Örnekte, @a adlı değişkeninin değeri ne olursa olsun oluşturmaz @product değişken adlandırılan ürün ve benzeri ne olursa olsun işler.

Ancak değişkenleri için sınırlı değildir. Burada, bazı durumlarda bir ifade @ karakteri önündeki:

- @(bir\*b) değişkenlerde ne olursa olsun, ürün işleyen bir ve b. ( \* İşleci çarpma anlamına gelir.)
- @(teknoloji + "" + Ürün) arasında bir boşluk ekleyerek ve bunları birleştirme sonra değişkenleri teknoloji ve ürün değerleri işler. Dizeleri birleştirme işleci (+) işleci numaraları eklemek için aynıdır. ASP.NET dize veya sayı ile çalışıyorsanız ve ile doğru şeyi yapar olup olmadığını genellikle öğrenebilirsiniz + işleci.
- @Request.Url İstek nesnesi Url özelliğinin işler. Request nesnesi tarayıcıdan geçerli istek hakkındaki bilgiler içerir ve Elbette URL'si özelliği, geçerli istek URL'sini içerir.

Örnek ayrıca şunları yapabilirsiniz, farklı şekilde çalıştığını göstermek için tasarlanmıştır. Üst kod bloğundaki hesaplamalar yapmak, sonuçları bir değişkene koyabilir ve biçimlendirme değişkeninde işleme. Veya bir ifade sağ biçimlendirmede hesaplamalar yapabilirsiniz. Kullandığınız yaklaşım gerçekleştirmekte olduğunuz ve, kendi tercih üzerinde bazı ölçüde bağlıdır.

### <a name="seeing-the-code-in-action"></a>Eylem kodda görme

Dosya adına sağ tıklayın ve ardından **başlatma tarayıcıda**. Tüm değerleri ve sayfanın çözülmüş ifadeleri tarayıcıda sayfasına bakın.

![Tarayıcıda çalışan 'TestRazor' sayfası](intro-to-web-pages-programming/_static/image2.png)

Tarayıcıda kaynağı bakın.

![Sayfa kaynağında 'Razor Test' tarayıcı](intro-to-web-pages-programming/_static/image3.png)

Deneyiminizi önceki öğreticideki gelen beklediğiniz gibi Razor kodunun sayfasında Yok'tur. Tüm gördüğünüz gerçek görüntü değerlerdir. Bir sayfa çalıştırdığınızda, Webmatrix'e yerleşik web sunucusuna gerçekte bir istek değişiklik yapıyorsunuz. İstek alındığında, ASP.NET tüm değerler ve ifadeler çözümler ve sayfaya değerlerini işler. Daha sonra sayfa tarayıcıya gönderir.

> [!TIP] 
> 
> **Razor ve C#**
> 
> Şimdiye kadar Razor sözdizimi ile çalışıyorsanız belirttiniz. True ise, ancak tam Öykü değil. Kullanmakta olduğunuz gerçek programlama dili olarak adlandırılan *C#*. C# on önce Microsoft tarafından oluşturulmuş ve Windows uygulamaları oluşturmak için birincil programlama dillerini birini haline gelmiştir. Bir değişken adı ve deyimleri vb. oluşturmak hakkında gördüğünüz tüm kurallar aslında tüm C# dilinin kurallardır.
> 
> Razor daha belirgin olarak bu kodu nasıl bir sayfaya katıştırmak için kuralları küçük kümesini ifade eder. Örneğin, kod sayfasında işaretlemek için @ kullanarak ve kullanarak kuralı @ kod bloğu katıştırmak için {} olan bir sayfanın Razor en boy. Ayrıca, Yardımcıları Razor parçası olarak kabul edilir. Razor sözdizimi, yalnızca ASP.NET Web Pages'de fazla yerde kullanılır. (Örneğin, bu da ASP.NET MVC görünümlerinde kullanılır.)
> 
> Programlama ASP.NET Web sayfaları hakkında bilgi için bakarsanız, Razor başvuruları sayıda bulacaksınız çünkü Biz bu Bahsediyor. Ancak, bu başvuruları çok uygulanmaz ne, bunun olduğunuz ve bu nedenle kafa karıştırıcı olabilir. Ve aslında, birçok programlama sorularınızı gerçekten C# ile çalışma veya ASP.NET ile çalışma hakkında olacak. Bu nedenle Razor hakkında bilgi için özellikle bakarsanız, gereksinim duyduğunuz yanıtları bulamayabilir.


## <a name="adding-some-conditional-logic"></a>Bazı koşullu mantık ekleme

Kod kullanarak bir sayfa hakkında harika özellikler üzerinde çeşitli koşullar neler tabanlı değiştirebilirsiniz biridir. Öğreticinin bu bölümünde, sayfasında görüntülenme şeklini değiştirmek için bazı yollar oynamak.

Örneğin, basit ve biraz böylece biz üzerindeki koşullu mantık yoğunlaşabilirsiniz contrived olacaktır. Oluşturacağınız sayfa bunu yapın:

- Farklı bir metin sayfasında ilk kez olmasından bağlı olarak sayfası gösterilir veya sayfanın göndermek için bir düğmeye tıklandığında gösterir. İlk koşullu test olacaktır.
- Yalnızca belirli bir değere (http://...?show=true) URL sorgu dizesinde geçirilen olursa bir ileti görüntüler. İkinci koşullu test olacaktır.

WebMatrix içinde bir sayfa oluşturun ve adlandırın *TestRazorPart2.cshtml*. (Şeritte tıklatın **yeni**, seçin **CSHTML**, dosyayı adlandırın ve ardından **Tamam**.)

Bu sayfa içeriğini aşağıdakiyle değiştirin:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Üst kod bloğu bazı metinleri iletisiyle adlı bir değişken başlatır. İçinde görüntülenen sayfasının gövdesinde ileti değişkenini içeriğini bir &lt;p&gt; öğesi. İşaretleme de içeren bir &lt;giriş&gt; öğesi oluşturmak için bir **gönderme** düğmesi.

Şimdi nasıl çalıştığını görmek için sayfayı çalıştırın. Tıklattığınız şu an için temel olarak statik bir sayfa olsa dahi **gönderme** düğmesi.

WebMatrix için geri dönün. Kod bloğunun içine aşağıdaki vurgulanmış kodu ekleyin *sonra* ileti başlatır satır:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Eğer {} bloğu

Bir if edildi ne eklemiş koşulu. Kodda, koşulu bir yapı şöyle varsa:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

Test etmek için parantez içinde bir durumdur. Bu, bir değer veya true veya false döndüren bir ifade olması gerekir. Koşul doğru ise, ASP.NET deyimi veya ayraçların içindeki ifadeleri çalıştırır. (Aşağıdakiler *sonra* parçası *IF then* mantığı.) Koşul yanlış ise, kod bloğunu atlanır.

İşte birkaç örnekler bir if test edebilirsiniz koşulları deyimi:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Kullanarak değişkenleri değerlerle veya ifadeleri karşı sınayabilirsiniz bir <em>mantıksal işleç</em> veya <em>karşılaştırma işleci</em>: eşittir (==), büyüktür (&gt;), küçüktür (&lt;), büyüktür veya eşittir (&gt;=) ve küçük veya eşit (&lt;=). ! = Eşit değil işleci anlamına — Örneğin, varsa (bir! = 0) anlamına gelir <em>varsa</em> <em>bir</em><em>0'a eşit değil</em>.

> [!NOTE]
> (==) Eşit karşılaştırma işleci = aynı olmadığına dikkat edin emin olun. = İşleci yalnızca değerleri atamak için kullanılır (var bir = 2). Bu işleçlere karışımı varsa, bir hata iletisi alırsınız veya bazı garip sonuçları elde edersiniz.


Bir şey doğru olup olmadığını sınamak için tam sözdizimi şöyledir if(IsDone == true). Ancak, kısayol if(IsDone) de kullanabilirsiniz. Hiçbir karşılaştırma işleci ise, ASP.NET için true test ettiğiniz varsayar.

! tek başına işleci mantıksal değil anlamına gelir. Örneğin, koşul IF (! IsPost) anlamına gelir *IsPost doğru değilse*.

Mantıksal AND kullanarak koşulları birleştirebilirsiniz (&amp; &amp; işleci) veya mantıksal OR (|| işleci). Örneğin, eğer son önceki örnekler anlamına gelir koşulları *FileProcessingIsDone ayarlanmamışsa true ve displayMessage false olarak ayarlandığında*.

### <a name="the-else-block"></a>Else bloğu

Eğer hakkında son bir şey blokları: bir blok tarafından başka bir blok izlenebilir durumunda. Başka bir blok yararlıdır koşul yanlış olduğunda farklı kod yürütmek sahip olabilir. Basit bir örnek aşağıda verilmiştir:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Başka bir blok kullanma yararlı olduğu bu serideki sonraki öğreticileri, bazı örnekler görürsünüz.

### <a name="testing-whether-the-request-is-a-submit-post"></a>İstek Gönder (post) olup olmadığını test etme

Çok daha fazlası vardır, ancak şimdi koşulu if(IsPost) {...} sahip örneği geri alın. IsPost gerçekten geçerli sayfa bir özelliğidir. İlk kez sayfa istenen IsPost false değerini döndürür. Ancak, bir düğmeye veya aksi halde gönderme sayfası — diğer bir deyişle, post — IsPost true döndürür. Bu nedenle IsPost form gönderme ile ilgilenen olup olmadığını belirlemenize olanak sağlar. (İsteği alma işlemi, HTTP fiilleri bakımından IsPost false döndürür. POST işlemine isteğiyse IsPost true değerini döndürür.) Sonraki öğreticide burada bu test özellikle yararlı olur giriş formları ile çalışması.

Sayfayı çalıştırın. Bu ilk kez olduğundan istenen sayfasına "Budur sayfa istenen ilk kez" bakın. Bu dize ileti değişkenine başlatılan değerdir. Bir if(IsPost) test yoktur, ancak, şu anda false döndürdüğü içindeki IF kodu engelleyecek şekilde çalışmaz.

Tıklatın **gönderme** düğmesi. Sayfa yeniden istedi. Önce ileti değişkenini "Bu ilk kez... olduğu için" ayarlanır. Ancak içindeki IF kodu engelleyecek şekilde çalışmalarını bu süre, test if(IsPost) true döndürür. Kod biçimlendirme işlenen olan farklı bir değere, ileti değişkenin değerini değiştirir.

Şimdi bir if ekleyin işaretleme koşulu. Aşağıda &lt;p&gt; içeren öğeyi **gönderme** düğme, aşağıdaki biçimlendirmeyi ekleyin:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

İle başlamak alacak şekilde biçimlendirmesi içinde kod eklediğiniz @. Bir if sonra test benzer bir eklediğiniz önceki kod bloğunda. Küme ayraçları içinde rağmen olağan HTML eklediğiniz — için ulaşana kadar en azından, normal @DateTime.Now. Başka bir küçük boyutta bir Razor kod olduğundan, bu nedenle yeniden @, önündeki eklemeniz gerekir.

Burada koşullar hem de en üstte ve biçimlendirme kodu engellerseniz ekleyebileceğiniz noktasıdır. Bir if kullanırsanız işaretleme veya kod bloğunun satırları sayfasının gövdesindeki koşul olabilir. Bu durumda, ve biçimlendirme ve kodun karışık zaman true olarak, kod olduğu temizleyin ASP.NET ile yapmak için @ kullanmak zorunda.

Sayfayı çalıştırın ve tıklatın **gönderme**. Bu süre, yalnızca farklı bir ileti ("gönderdiğiniz artık..."), gönderme, ancak tarih ve saat listeler yeni bir ileti görür görürsünüz.

![Gönderme sonra gösteren zaman damgalı tarayıcıda çalışan 'Razor 2 test' sayfası](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Bir sorgu dizesi değerini test etme

Daha fazla test. Bu süre, eğer ekleyeceksiniz bir değeri test blok adlandırılmış sorgu dizesinde geçirilen göster. (Şöyle: `http://localhost:43097/TestRazorPart2.cshtml?show=true`), görüntüleme ileti böylece sayfa değiştireceğiz ("Bu ilk kez...", vb.) göster değeri true ise yalnızca görüntülenir.

Alt (ancak iç) sayfanın üstündeki kod bloğu aşağıdakileri ekleyin:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Tam kod bloğu Şimdi Ara aşağıdaki örneğe benzer. (Sayfanıza kodu kopyaladığınızda, girinti farklı görünebilir olduğunu unutmayın. Ancak, kodu nasıl çalışacağını etkilemez.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Yeni kod bloğundaki false showMessage adlı bir değişken başlatır. Ardından bir if mu sorgu dizesi değeri aramak için test. Sayfa ilk kez istediğinde bunun gibi bir URL'ye sahip:

`http://localhost:43097/TestRazorPart2.cshtml`

Kod URL sorgu dizesinde URL bu sürümü gibi show adlı bir değişken içerip içermediğini belirler:

`http://localhost:43097/TestRazorPart2.cshtml`? Göster = true

Test isteği nesnesinin QueryString özellik arar. Sorgu dizesi adlı bir öğe Göster içeriyorsa ve bu öğe true olarak if ayarlanmışsa bloğu çalıştırır ve showMessage değişkeni true olarak ayarlanır.

Gördüğünüz gibi el buraya yoktur. Adı diyor gibi sorgu dizesi bir dizedir. Ancak, test ettiğiniz değer bir Boole (true/false) değeri ise yalnızca true ve false sınayabilirsiniz. Göster değişkeninin değeri sorgu dizesinde test etmeden önce bir Boolean değeri dönüştürmeniz gerekir. AsBool yöntemi yaptığı olan — giriş olarak bir dize alır ve bir Boolean değerine dönüştürür. Açıkçası, dize "true" ise, AsBool yöntemi bu değeri true değerine dönüştürür. Dize değeri başka bir şey olması durumunda AsBool false döndürür.

> [!TIP] 
> 
> **Veri türleri ve As() yöntemleri**
> 
> Biz yalnızca o ana kadarki bir değişkeni oluşturduğunuzda, anahtar sözcüğü örneklemeyi kullanmak belirttiniz Tüm öykü değildir. Değerlerini değiştirmek için — numaraları, ekleme veya dizeyi, birleştirme veya tarihleri karşılaştırmak ya da true/false için test etmek için — C# sahip bir uygun iç temsili değeri ile çalışmak. C# için *genellikle* bu gösterimi olması gerektiğini öğrenmek şekil (diğer bir deyişle, ne *türü* veri) değerlerle ne yaptığınız üzerinde temel. Şimdi yapıp yine de, bunu olamaz. Yoksa, nasıl C# verileri temsil etmelidir açıkça belirterek yardımcı olması vardır. AsBool yöntemi yapan —, C# bir dize değerini "true" veya "false" Bir Boole değeri olarak ele alınması gerektiğini bildirir. Dizeleri de diğer türleri olarak AsInt (bir tamsayı olarak değerlendir), AsDateTime (bir tarih/saat olarak değerlendir), AsFloat (kayan noktalı sayı olarak değerlendir) ve benzerleri gibi göstermek için benzer yöntemler mevcut. C# dize değeri istenen şekilde temsil edilemez ediyorsa () yöntemleri olarak kullandığınızda, bir hata görürsünüz.


Sayfa Biçimlendirme kaldırın veya bu öğe Açıklama (burada, Açıklamalı giden gösterilir):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Burada kaldırılan veya metni, geçersiz kılınan sağ aşağıdakileri ekleyin:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Test showMessage değişken true ise işlemek diyorsa bir &lt;p&gt; ileti değişkenin değerini bir öğesiyle.

### <a name="summary-of-your-conditional-logic"></a>Koşullu mantık özeti

Durumunda ne yalnızca yaptığınızı tamamen emin değilseniz, bir özeti aşağıda verilmiştir.

- İleti değişkeni varsayılan dizeye başlatılır ("... ilk kez budur").
- Sayfa isteği gönder (post) sonucunu ise, "gönderdiğiniz artık..." iletisi değerini değiştirilir
- ShowMessage değişkeni false olarak başlatılır.
- Sorgu dizesi içeriyorsa? Göster = true showMessage değişkeni ayarlanır true.
- Biçimlendirmede showMessage true ise, bir &lt;p&gt; öğesi ileti değerini gösteren işlenir. (ShowMessage false ise, hiçbir şey bu noktada biçimlendirmede çizilmez.)
- Bir post isteğiyse biçimlendirmede bir &lt;p&gt; öğesi, tarih ve saati görüntüler işlenir.

Sayfayı çalıştırın. Biçimlendirme if(showMessage) test false döndürecek şekilde showMessage yanlış olduğundan ileti yok, yoktur.

Tıklatın **gönderme**. Tarih ve saat, ancak hala bir ileti görürsünüz.

Tarayıcınızda URL kutusuna gidin ve aşağıdaki URL'yi sonuna ekleyin:? Göster = true ve ardından Enter tuşuna basın.

![Sorgu dizesi gösteren tarayıcısında sayfa 'test Razor 2'](intro-to-web-pages-programming/_static/image5.png)

Sayfa yeniden görüntülenir. (URL değiştiğinden bir gönderme yeni bir istek budur.) Tıklatın **gönderme** yeniden. İleti yeniden, tarih ve saat olarak görüntülenir.

![Bir sorgu dizesi olduğunda gönderme sonra 'Razor 2 test' sayfası](intro-to-web-pages-programming/_static/image6.png)

URL'de değiştirme? göstermek için girintili =? Göster = false ve Enter tuşuna basın. Sayfa yeniden gönderin. Geri nasıl başlattığınız için sayfasıdır — ileti yok.

Daha önce not ettiğiniz gibi bu örnek mantığını contrived biraz gereklidir. Bununla birlikte, çoğu, sayfalarınızı gündeme yayınlanıyorsa ve onu bir veya daha fazla görülen forms burada sürer.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>(Gravatar görüntü görüntüleme) Yardımcısı yükleniyor

Kişiler genellikle web sayfalarında yapmak istediğiniz bazı görevleri çok fazla kod zorunlu veya ek bilgi gerektirir. Örnekler: veri için bir grafik görüntüleme; bir Facebook "Gibi" düğmesine bir sayfada; koyma gönderen e-posta, Web sitesinden; Kırpma veya görüntüleri yeniden boyutlandırma; PayPal siteniz için kullanma. Bu tür bir şeyler yapın kolaylaştırmak için ASP.NET Web sayfaları kullanmanıza olanak sağlayan *Yardımcıları*. Bir site için yüklediğiniz ve birkaç satırlık bir Razor kod kullanarak genel görevleri gerçekleştirmenize imkan sağlayan bileşenleri yardımcılardır.

ASP.NET Web sayfaları birkaç Yardımcıları yerleşik olarak sahiptir. Bununla birlikte, birçok Yardımcıları NuGet Paket Yöneticisi'ni kullanarak sağlanan paketlerinde (eklentiler) kullanılabilir. NuGet yüklemek için bir paket seçmenize olanak sağlar ve ardından bunu yükleme tüm ayrıntılarını mvc'deki.

Öğreticinin bu bölümünde, bir Gravatar ("Genel tanınan avatar") görüntüsü görüntülemenize olanak sağlayan bir yardımcı yükleyeceksiniz. İki şey öğreneceksiniz. Bulma ve bir yardımcı yükleme biridir. Ayrıca, nasıl bir yardımcı, aksi takdirde kodu kendiniz yazmak zorunda çok kullanarak bunu gerekir işlemlerinizi kolaylaştırır öğreneceksiniz.

Kendi Gravatar Gravatar Web sitesindeki kaydedebilirsiniz [ http://www.gravatar.com/ ](http://www.gravatar.com/), ancak öğreticinin bu bölümü gerçekleştirmek için bir Gravatar hesabı oluşturmak için gerekli değildir.

Webmatrix'te tıklatın **NuGet** düğmesi.

![WebMatrix, NuGet Galerisi'nin iletişim kutusu](intro-to-web-pages-programming/_static/image7.png)

Bu, NuGet Paket Yöneticisi'ni başlatır ve kullanılabilir paketler görüntüler. (Tüm paketler Yardımcıları; bazı işlevler eklemek için WebMatrix kendisi, bazı ek şablonlar ve benzeri.) Sürüm uyumsuzluğu ile ilgili bir hata iletisi alabilirsiniz. Tıklayarak bu hata iletisini yoksayabilirsiniz **Tamam** ve bu öğreticinin işlemine devam etmeden.

![WebMatrix, NuGet Galerisi'nin iletişim kutusu](intro-to-web-pages-programming/_static/image8.png)

Arama kutusuna "asp.net Yardımcıları" girin. NuGet arama terimleriyle eşleşen paketleri gösterir.

![WebMatrix paketleri gösteren NuGet galerisinde](intro-to-web-pages-programming/_static/image9.png)

ASP.NET Web Yardımcıları kitaplığı Gravatar görüntüleri kullanımı gibi birçok ortak görevlerini basitleştirmek için kod içerir. Seçin **ASP.NET Web Yardımcıları Kitaplığı** paketini ve ardından **yükleme** yükleyici başlatmak için. Seçin **Evet** paketini yükleyin ve yüklemeyi tamamlamak için koşulları kabul etmek isteyip istemediğiniz sorulduğunda.

İşte bu kadar. NuGet indirir ve gerekli olabilecek herhangi bir ek bileşenini dahil her şeyi yükler (*bağımlılıkları*).

Bazı nedenlerden dolayı bir yardımcı kaldırmanız gerekirse, çok benzer bir işlemdir. Tıklatın **NuGet** düğmesini tıklatın, **yüklü** sekmesini tıklatın ve kaldırmak istediğiniz paketi seçin.

## <a name="using-a-helper-in-a-page"></a>Bir yardımcı bir sayfasında kullanma

Artık yalnızca yüklü yardımcı kullanacaksınız. Bir yardımcı bir sayfaya ekleme işlemi için çoğu Yardımcıları benzer.

WebMatrix içinde bir sayfa oluşturun ve adlandırın *GravatarTest.cshml*. (Yardımcı test etmek için özel bir sayfa oluşturuyorsanız, ancak, sitenizde herhangi bir sayfayı Yardımcıları kullanabilirsiniz.)

İçinde &lt;gövde&gt; öğesi ekleme bir &lt;div&gt; öğesi. İçinde &lt;div&gt; öğesi, bunu yazın:

@Gravatar.

@ Karakteri, kullanmakta Razor kodunun işaretlemek için aynı karakterdir. **Gravatar** çalıştığınız yardımcı nesnesi.

WebMatrix nokta (.) yazmanızın hemen ardından listesini görüntüler *yöntemleri* (Gravatar yardımcı kullanımına işlevleri):

![Gravatar yardımcı IntelliSense aşağı açılan liste](intro-to-web-pages-programming/_static/image10.png)

Bu özellik olarak bilinen *IntelliSense*. Bu, size kod bağlam uygun seçimler sağlayarak yardımcı olur. IntelliSense, HTML, CSS, ASP.NET kodu, JavaScript ve diğer Webmatrix'te desteklenen dilleri ile çalışır. Bu, WebMatrix web sayfalarında geliştirmek kolaylaştırır başka bir özelliktir.

Makinesinde G klavye ve bakın IntelliSense GetHtml yöntemi bulur. Sekme tuşuna basın. IntelliSense seçilen yöntemi (GetHtml) ekler. Bir açma ayracı yazın ve kapatma parantezi otomatik olarak eklenir dikkat edin. Tırnak işaretleri arasına iki e-posta adresinizi yazın. Profil resminizi Gravatar hesabınız varsa, döndürülür. Bir Gravatar hesabı yoksa, varsayılan görüntü döndürülür. İşiniz bittiğinde satırı şuna benzer:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Şimdi sayfasını bir tarayıcıda görüntülemek. Bir Gravatar hesabına sahip olup bağlı olarak, resim veya varsayılan görüntü görüntülenir.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![Varsayılan görüntü](intro-to-web-pages-programming/_static/image12.png)

Ne hakkında bir fikir edinmek için yardımcıyı sizin için yapıyor, sayfa kaynağını tarayıcıda görüntülemek. Sayfanızda olan HTML yanı sıra, bir tanımlayıcı içeren bir görüntü öğesi bakın. Bu yardımcı sayfasına sahip olduğu yerde işlenen koddur @Gravatar.GetHtml. Yardımcı sağladı ve sağlanan hesap için doğru görüntüyü geri dönmek için doğrudan Gravatar ettiği kod oluşturulan bilgileri sürdü.

GetHtml yöntemi diğer parametreleri sağlayarak görüntü özelleştirmenizi sağlar. Aşağıdaki kod, görüntü genişliği ve yüksekliği 40 piksel varsa ve adlı belirtilen varsayılan görüntü kullanıyorsa istemek gösterilmiştir **wavatar** belirtilen hesap mevcut değilse.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Bu kod (varsayılan görüntü rastgele değişir) aşağıdaki sonucu şöyle üretir.

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Sıradaki gelen

Bu öğretici kısa tutun, biz yalnızca birkaç temel özellikler üzerine odaklanmıştır gerekiyordu. Doğal olarak, var olan bir *çok* Razor ve C# daha fazla. Daha yazarken bu öğreticileri Git öğreneceksiniz. Razor ve C# programlama yönlerini hakkında daha fazla şimdi öğrenme düşünüyorsanız, daha kapsamlı girişi buraya okuyabilirsiniz: [ASP.NET Web programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkID=202890).

Sonraki öğretici bir veritabanı ile çalışmaya tanıtır. Bu öğreticide, en sevdiğiniz film listesi sağlayan örnek uygulama oluşturmaya başlarsınız.

## <a name="complete-listing-for-testrazor-page"></a>Tam listesi için TestRazor sayfası

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Tam listesi için TestRazorPart2 sayfası

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Tam listesi için GravatarTest sayfası

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [Razor sözdizimini kullanan ASP.NET Web programlamaya giriş](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Twitter Yardımcısı](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Önceki](getting-started.md)
> [sonraki](displaying-data.md)
