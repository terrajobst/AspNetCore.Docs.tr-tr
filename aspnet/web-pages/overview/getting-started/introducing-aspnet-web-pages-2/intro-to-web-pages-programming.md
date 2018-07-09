---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Temel programlama - ASP.NET Web sayfaları ile tanışın | Microsoft Docs
author: tfitzmac
description: "Bu öğretici, Razor sözdizimi olan ASP.NET Web Pages'de program hakkında genel bakış sağlar. Öğrenecekleriniz: çekme isteği için kullandığınız temel \"Razor\" söz dizimi..."
ms.author: aspnetcontent
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 56268943b09d366b15d3a11e641d6c6b6c95aa16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839755"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>ASP.NET Web sayfaları - Programlama temelleri ile tanışın
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğretici, Razor sözdizimi olan ASP.NET Web Pages'de program hakkında genel bakış sağlar.
> 
> Öğrenecekleriniz:
> 
> - ASP.NET Web Pages'de programlamayı için kullandığınız temel "Razor" sözdizimi.
> - Bazı temel C kullanacağınız programlama dili olan #.
> - Bazı temel programlama kavramlarını Web sayfaları için.
> - Sitenizin kullanılacak (önceden oluşturulmuş kodu içeren bileşenleri) paketleri yükleme.
> - Nasıl kullanılacağını *Yardımcıları* ortak programlama görevlerinin yerine getirilmesi.
>   
> 
> Ele alınan özelliklerin/teknolojiler:
> 
> - Ve NuGet Paket Yöneticisi.
> - `Gravatar` Yardımcısı.


Bu öğretici, ASP.NET Web sayfaları için kullanacağınız için programlama sözdizimi ile tanışın, öncelikle bir alıştırma bölümüdür. Hakkında bilgi edineceksiniz *Razor sözdizimi* ve C# dilinde yazılmış kod programlama dili. Önceki öğreticide Bu sözdizimi bir bakışta aldığınız; Bu öğreticide daha fazla söz dizimi açıklayacağız.

Bu öğreticide, tek bir öğreticide görürsünüz ve olan yalnızca bir öğretici olduğu programlama en içerdiğini etmeyeceğiz *yalnızca* programlama hakkında. Bu kümedeki kalan öğreticilerde, ilgi çekici şeyler sayfaları gerçekten oluşturacaksınız.

Hakkında bilgi edineceksiniz *Yardımcıları*. Yardımcı bir bileşenidir; kod parçasını paketlenmiş yukarı — bir sayfaya ekleyebileceğiniz. Yardımcı, aksi takdirde tedious veya el ile yapmak için karmaşık olabilir, çalışma gerçekleştirir.

## <a name="creating-a-page-to-play-with-razor"></a>Razor ile yürütmek için bir sayfa oluşturma

Temel söz dizimi bir fikir alabilmeniz için bu bölümde, biraz Razor ile oynatın.

WebMatrix, zaten çalışıyorsa başlatın. Önceki öğreticide oluşturulan Web sitesi kullanmanız gerekir ([Web sayfaları ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkId=251578)). Yeniden açmak için tıklayın **Sitelerim** ve **WebPageMovies**:

![WebMatrix Başlat Sitelerim vurgulanmış ve açık Site seçenekleri gösteren ekran](intro-to-web-pages-programming/_static/image1.png)

Seçin **dosyaları** çalışma.

Şeritte tıklayın **yeni** bir sayfa oluşturun. Seçin **CSHTML** ve yeni sayfa adı *TestRazor.cshtml*.

**Tamam**'ı tıklatın.

Aşağıdaki tamamen zaten değiştirmek dosyasına kopyalayın.

> [!NOTE]
> Bir sayfaya örneklerden başladığınız kodun veya kopyaladığınızda, hizalama ve girinti öğretici ile aynı olmayabilir. Girinti ve hizalama kodu, ancak çalışma şeklini etkilemez.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Örnek sayfasında İnceleme

Gördüğünüz çoğunu sıradan HTML olur. Ancak, en üstünde bu kod bloğu vardır:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Bu kod bloğu hakkında aşağıdaki noktaya dikkat edin:

- @ Karakteri ASP.NET aşağıda olmayan HTML Razor kodu olduğunu söyler. ASP.NET işlemek sonra her şeyin @ karakteri olarak bazı HTML'e yeniden çalışana kadar kodu. (Bu durumda, bu &lt;! DOCTYPE&gt; öğesi.
- Küme ayraçları ({ve}) birden fazla satırı koduna sahip bir Razor kod bloğu içine alın. Ayraçlar, kod bloğu için burada başlar ve biter ASP.NET söyleyin.
- / / Mark yorum karakterleri — diğer bir deyişle, bir kod parçası, yürütülen olmaz.
- Her deyimin noktalı virgül (;) ile sona erdirmek vardır. (, Ancak yorum yok.)
- Değerleri depolayabilir *değişkenleri*, oluşturduğunuz (*bildirmek*) ile anahtar sözcük var. Bir değişkeni oluşturduğunuzda, buna harf, rakam ve alt çizgi içerebilir bir ad vermeniz (\_). Değişken adları bir sayı ile başlayamaz ve (var gibi) programlama bir anahtar adı kullanamazsınız.
- (Örneğin, "ASP.NET" ve "Web sayfaları") karakter dizeleri tırnak işaretleri içine alın. (Bunlar, çift tırnak işareti olmalıdır.) Sayı, tırnak işaretleri içine değil.
- Tırnak işaretleri dışında boşluk önemli değildir. Satır sonları çoğunlukla önemli değildir; özel durum, satırlar arasında bir dize tırnak işaretleri içindeki bölünemez kullanılır. Girinti ve hizalama önemli değildir.

Bu örnekte belirgin değil tüm kodun büyük küçük harfe duyarlı olduğunu bir şeydir. Bu değişken TheSum theSum veya thesum adlandırılabilir değişkenleri değerinden farklı bir değişken olduğu anlamına gelir. Benzer şekilde, bir anahtar sözcüğü var, ancak Var değildir.

### <a name="objects-and-properties-and-methods"></a>Nesneleri ve özellikleri ve yöntemleri

' % S'ifadesi DateTime.Now kaldırılır. Basit bir deyişle, tarih saat olan bir *nesne*. Bir nesne ile programlayabileceğiniz şeydir — bir sayfa, bir metin kutusu, bir dosya, görüntü, bir web isteği, bir e-posta iletisi, bir müşteri kaydı vb. Nesneleri olan bir veya daha fazla *özellikleri* özelliklerini açıklar. Bir metin özelliği (diğerlerinin arasında) bir metin kutusu nesnesine sahip, bir istek nesnesi URL'si özelliği (ve diğerleri) sahip, e-posta iletisine bir From özelliğine ve bir To özelliğini sahiptir ve benzeri. Nesneleri de *yöntemleri* "bunlar gerçekleştirebilir fiilleri" olan. Nesnelerle çok yoğun şekilde kullanacağınız.

Örnekte görebileceğiniz gibi DateTime program tarihler ve saatler olanak sağlayan bir nesnedir. Bu, geçerli tarih ve saati döndürür artık adlı bir özelliğe sahiptir.

### <a name="using-code-to-render-markup-in-the-page"></a>Sayfasında biçimlendirmesi oluşturmak için kod kullanma

Sayfanın gövdesi içinde aşağıdakilere dikkat edin:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Yeniden @ karakteri, ne izleyen ASP kodu, HTML değil. İşaretlemede @ bir kod ifadesi tarafından izlenen ekleyebilir ve ASP.NET ifade hakkı değerini bu noktada işlenir. Örnekte, @a adlı değişken değeri ne olursa olsun işlenir @product ne olursa olsun değişken adlandırılmış ürünü ve benzeri işler.

Yine de değişkenlere, sınırlı değilsiniz. Burada, bazı durumlarda bir ifade @ karakteri önündeki:

- @(bir\*b) değişkenlerinde ne olursa olsun, ürün işleyen bir ve b. ( \* İşleci çarpma anlamına gelir.)
- @(teknoloji + "" + Ürün) arasında bir alan ekleme ve bunları birleştirerek sonra değişkenleri teknoloji ve ürünü değerleri işler. Dizeleri birleştirme için işlecine (+) işleci numaralarını eklemek için aynıdır. ASP.NET sayılar veya dizeler ile çalışıyorsanız ve ile doğru şeyi yapar olmadığını genellikle öğrenebilirsiniz + işleci.
- @Request.Url İstek URL'si özelliğini işler. İstek nesnesi tarayıcıdan geçerli istek hakkındaki bilgiler, ve Elbette URL'si özelliği, geçerli istek URL'sini içerir.

Örneğin, oluşturabilir, farklı şekilde çalıştığını göstermek için de tasarlanmıştır. Üst kod bloğundaki hesaplamalar yapmak, sonuçları bir değişken içine yerleştirin ve ardından biçimlendirme değişkeninde işleme. Veya bir ifade sağ biçimlendirmede hesaplamalar gerçekleştirebilirsiniz. Kullandığınız yaklaşım ne yaptığınızı üzerinde ve kendi tercihinize göre bir ölçüde bağlıdır.

### <a name="seeing-the-code-in-action"></a>Uygulamada kod görme

Dosya adına sağ tıklayın ve ardından **tarayıcıda Başlat**. Tüm değerleri ve ifadeleri sayfasında çözümlenen tarayıcıda sayfayı görürsünüz.

!['TestRazor' sayfasını tarayıcıda çalışıyor](intro-to-web-pages-programming/_static/image2.png)

Kaynak tarayıcı bakın.

![İçin tarayıcıda 'Razor Test' sayfa kaynağı](intro-to-web-pages-programming/_static/image3.png)

Önceki öğreticide, deneyiminden beklediğiniz gibi Razor kodunun sayfasında, Yok'tur. Gerçek görüntüleme değerleri olan tüm görürsünüz. Bir sayfa çalıştırdığınızda, gerçekten Webmatrix'e yerleşik web sunucusu isteği yapılıyor. İstek alındığında, ASP.NET tüm değerleri ve ifadeleri giderir ve bunların değerlerini sayfasına işler. Daha sonra sayfa tarayıcıya gönderir.

> [!TIP] 
> 
> **Razor ve C#**
> 
> Şimdiye kadar Razor sözdizimi olan çalıştığınız belirttiniz. True olan ancak bütünüyle değil. Kullanmakta olduğunuz gerçek programlama dili olarak adlandırılır *C#*. C# on yıl önce Microsoft tarafından oluşturulduğunda ve Windows uygulamaları oluşturmak için birincil programlama dillerinden biri haline gelmiştir. Bir değişken adını ve tablolar vb. oluşturmak hakkında gördüğünüz tüm kurallar aslında tüm C# dilinin kurallardır.
> 
> Razor daha açık belirtmek gerekirse bu kod nasıl bir sayfasına eklemek için kuralları küçük kümesini ifade eder. Örneğin, @, kod sayfasında işaretlemek için kullanarak ve kullanarak kuralı @ bir kod bloğu eklemek için {} olan bir sayfa Razor yönüyle. Ayrıca, Yardımcıları Razor parçası olarak kabul edilir. Razor sözdizimi, yalnızca ASP.NET Web Pages'de fazla yerde kullanılır. (Örneğin, bu da ASP.NET MVC görünümleri kullanılır.)
> 
> Programlama ASP.NET Web sayfaları hakkında bilgi için bakarsanız, Razor başvuruları çok sayıda bulabilirsiniz çünkü biz bunu bahsedebilirsiniz. Ancak, bu başvuruları birçok uygulanmaz ne, bunun yapılması olduğunuz ve bu nedenle kafa karıştırıcı. Ve hatta, birçok programlama sorularınızı gerçekten C# ile çalışan ya da ASP.NET ile çalışma hakkında olacak. Bu nedenle Razor hakkında bilgi için özel olarak bakarsanız, ihtiyacınız olan yanıtları alarak gelmeyebilir.


## <a name="adding-some-conditional-logic"></a>Bazı koşullu mantık ekleme

Kod kullanarak bir sayfa hakkında harika özellikler çeşitli koşullara göre ne göre değiştirebilirsiniz biridir. Öğreticinin bu bölümünde, sayfanın görüntülenme şeklini değiştirmek için bazı yollar ile oynayabilirsiniz.

Örneğin, basit ve biraz biz koşullu mantığı üzerinde yoğunlaşabilmeniz contrived olacaktır. Oluşturacağınız sayfasında bunu:

- Farklı metin sayfasında ilk kez olmasına göre bağlı olarak, sayfa görüntülenir veya sayfa göndermek için bir düğmeye tıkladı gösterir. İlk koşul testi olacak.
- Yalnızca belirli bir değere (http://...?show=true) URL sorgu dizesinde yer iletilmezse iletisini görüntüler. İkinci koşul testi olacak.

WebMatrix, bir sayfa oluşturun ve adlandırın *TestRazorPart2.cshtml*. (Şeritte tıklayın **yeni**, seçin **CSHTML**, dosyayı adlandırın ve ardından **Tamam**.)

Bu sayfanın içeriğini aşağıdakiyle değiştirin:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Üst kod bloğu bazı metin iletisi adlı bir değişken başlatır. Sayfanın gövdesi içinde ileti değişken içeriğini görüntülenen bir &lt;p&gt; öğesi. Biçimlendirme da içeren bir &lt;giriş&gt; öğesi oluşturmak için bir **Gönder** düğmesi.

Nasıl çalıştığını görmek için sayfayı çalıştırın. ' E tıklarsanız şu an için temel olarak statik bir sayfa olduğu **Gönder** düğmesi.

WebMatrix için geri dönün. Kod bloğunun içine aşağıdaki vurgulanmış kodu ekleyin *sonra* satırın ileti başlatır:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>If {} Bloğu

Eğer ne yeni eklediğiniz olan koşul. Kodda, koşulu şöyle bir yapısı varsa:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

Test etmek için parantez içinde bir durumdur. Bu, bir değer veya true veya false değeri döndüren bir ifade olması gerekir. Koşul true ise, ASP.NET deyim ya da ayraçların içindeki deyimler çalışır. (Bu *ardından* parçası *if-then* mantığı.) Koşul false ise, kod bloğunun atlanır.

İşte birkaç örnek Eğer test koşulları deyimi:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Değişkenleri değerlerle veya karşı ifadeleri kullanarak test edebilirsiniz bir <em>mantıksal işleç</em> veya <em>karşılaştırma işleci</em>: eşittir (==) büyüktür (&gt;), küçüktür (&lt;), büyüktür veya eşittir (&gt;=) küçük veya eşittir (&lt;=). ! = Eşit değil işleci anlamına gelir — Örneğin, varsa (bir! = 0) anlamına gelir <em>varsa</em> <em>bir</em><em>0 değerine eşit değil</em>.

> [!NOTE]
> Karşılaştırma işleci için eşittir (==) için = aynı olmadığını fark emin olun. = İşleci, yalnızca değer atamak için kullanılır (değişken bir = 2). Bu işleçler karışımı varsa, bir hata iletisi alırsınız veya bazı ilginç sonuçlar elde edersiniz.


Bir şey doğru olup olmadığını test etmek için tam söz dizimi şu şekildedir if(IsDone == true). Ancak kısayol if(IsDone) de kullanabilirsiniz. Karşılaştırma işleci yok ise, ASP.NET true test ettiğiniz varsayılır.

! tek başına işleci, bir mantıksal değil anlamına gelir. Örneğin, koşul if (! IsPost) anlamına gelir *IsPost true değilse*.

Mantıksal AND kullanarak koşullar birleştirebilir (&amp; &amp; işleci) veya mantıksal OR (|| işleci). Örneğin, önceki örnek anlamına gelir ise son koşulları *FileProcessingIsDone ayarlı değil, doğru ve displayMessage false olarak ayarlandığında*.

### <a name="the-else-block"></a>Başka bloğu

Eğer hakkında bir son şey blokları: bir blok başka bir blok tarafından izlenebilir durumunda. Başka bir bloğu koşul false olduğunda, farklı kod yürütmek sahip olabilir. Basit bir örnek aşağıda verilmiştir:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Sonraki öğreticilerde bu serideki başka bir blok kullanarak yararlı olduğu bazı örnekler göreceksiniz.

### <a name="testing-whether-the-request-is-a-submit-post"></a>İstek bir gönderme (post) olup olmadığını test etme

Daha fazla ancak koşul if(IsPost) {...} olan örneğe geri geçelim. IsPost aslında geçerli sayfa bir özelliğidir. İlk kez sayfa istenen IsPost false döndürür. Ancak, bir düğmeye tıklayın veya aksi halde gönderme sayfası — diğer bir deyişle, gönderi — IsPost true döndürür. Bu nedenle IsPost bir form gönderme ile ilgilenen olup olmadığını belirlemenize olanak tanır. (Bir alma işlemi isteğiyse HTTP fiilleri açısından IsPost false döndürür. İstek gönderme işlemi ise IsPost true değerini döndürür.) Bir sonraki öğreticide burada bu test özellikle yararlı olur girdi biçimleri ile çalışırsınız.

Sayfayı çalıştırın. Bu ilk kez olduğundan istenen sayfasında "Bu bir sayfayı istenen ilk kez" görürsünüz. Bu dize ileti değişkenine başlatılan değerdir. İf(IsPost) test yoktur, ancak Eğer içinde kod engellemek için şu anda false döndüren çalışmaz.

Tıklayın **Gönder** düğmesi. Sayfa yeniden istenir. Daha önce ileti değişkeni "Bu... ilk kez bir için" ayarlanır. Ancak Eğer içinde kod engellemek için çalıştırmaları bu kez, test if(IsPost) true döndürür. Kod biçimlendirme içinde işlenen olan farklı bir değere ileti değişkenin değerini değiştirir.

Eğer şimdi ekleyin biçimlendirmede koşul. Aşağıda &lt;p&gt; öğesini içeren **Gönder** düğme, aşağıdaki işaretlemeyi ekleyin:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Başlamak alacak şekilde biçimlendirme içinde kod ekliyoruz @. Eğer daha sonra test benzer bir eklediğiniz önceki kod bloğu. Küme ayraçları içinde sıradan bir HTML ekliyorsunuz — için ulaşana kadar en az bu sıradan @DateTime.Now. Bu, başka bir küçük boyutta bir Razor kod olduğundan, bu nedenle yeniden @, önündeki eklemeniz gerekir.

Burada hem de koşullar, üst ve biçimlendirme içinde kod engellerseniz ekleyebileceğiniz noktasıdır. Eğer kullanırsanız işaretleme veya kod bloğu içinde satırları sayfasının gövdesindeki koşul olabilir. Bu durumda, ve işaretleme ve kod karıştırmak herhangi bir zamanda doğru olduğundan, kod olduğu Temizle ASP.NET ile yapmak için @ kullanın.

Sayfayı çalıştırın ve tıklayın **Gönder**. Bu süre değil yalnızca farklı bir ileti ("gönderdiğiniz artık...") gönderdiğiniz ancak tarih ve saat listeleyen yeni bir ileti görürsünüz.

!['Test Razor 2' sayfası sonra gösteren zaman damgasına sahip tarayıcıda çalışan gönderme](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Bir sorgu dizesi değerini test etme

Bir daha fazla test. Bu kez, eğer ekleyeceksiniz bir değeri test blok adlı sorgu dizesinde geçirilen göster. (Aşağıdaki gibi: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) sayfası, görüntüleme ileti değiştireceğiz ("Bu bir ilk kez...", vb.) show değeri true ise yalnızca görüntülenir.

Alt (ancak iç) kod bloğu sayfanın üst kısmındaki aşağıdakileri ekleyin:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Aşağıdaki örnekte olduğu gibi tam bir kod bloğu şimdi bakın. (Kod sayfanıza kopyaladığınızda, girinti farklı görünebilir olduğunu unutmayın. Ancak, kod nasıl çalışır etkilemez.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Yeni kod bloğundaki showMessage false adlı bir değişken başlatır. Eğer sonra mevcut bir sorgu dizesi değeri aramak için test. İlk sayfa istediğinde, bunun gibi bir URL vardır:

`http://localhost:43097/TestRazorPart2.cshtml`

Kod, URL, sorgu dizesinde URL bu sürümü gibi show adlı bir değişken içerip içermediğini belirler:

`http://localhost:43097/TestRazorPart2.cshtml`? Göster = true

Test isteği nesnenin QueryString özellik arar. Adlı bir öğe bir Göster sorgu dizesini içeriyorsa ve true olarak, eğer bu öğe ayarlarsanız bloğu çalıştırır ve showMessage değişkeni true olarak ayarlanır.

Gördüğünüz gibi burada el yoktur. Ad'ın belirttiği gibi sorgu dizesi bir dizedir. Ancak, test ettiğiniz değer bir Boole (true/false) değeri ise yalnızca true ve false sınayabilirsiniz. Show değişkenin değerini sorgu dizesinde test etmeden önce bir Boolean değerine dönüştürmek zorunda. Ne yaptığını AsBool yöntemi olan — giriş olarak bir dize alır ve bir Boolean değerine dönüştürür. Açıkça görülebileceği gibi dize "true" ise, AsBool yöntemi bu değeri true olarak dönüştürür. Dize değeri için başka bir şey ise AsBool false döndürür.

> [!TIP] 
> 
> **Veri türleri ve As() yöntemleri**
> 
> Biz yalnızca şu ana kadar bir değişken oluşturun, anahtar sözcük var. kullanın belirttiğin Bu Yazının tamamını ancak değil. Değerlerini değiştirmek için — numaralarını ekleyin veya dizeleri birleştirebilir veya tarihleri karşılaştırmak veya doğru/yanlış test — C# sahip bir uygun iç değerin gösterimi ile çalışmak. C# için *genellikle* bu gösterimi olması gerektiğini öğrenmek şekil (diğer bir deyişle, ne *türü* verilerdir) ne değerlerle yaptığınızı üzerinde temel. Artık ve sonra yine de bunu, olamaz. Aksi durumda, nasıl C# verileri temsil etmelidir açıkça belirterek yardımcı olması vardır. AsBool yöntemi yapan — bunu C# bir dize değerini "true" veya "false" Boole bir değer olarak değerlendirilmesi gerektiğini söyler. Dizeleri AsInt (bir tamsayı olarak davranma), AsDateTime (tarih/saat olarak davranma), AsFloat (bir kayan noktalı sayı olarak davranma) ve benzeri gibi diğer türleri olarak da temsil etmek için benzer yöntemler mevcut. C# dize değeri istenen şekilde gösteremez, () yöntemleri olarak kullandığınızda, bir hata görürsünüz.


Sayfanın biçimlendirmesine kaldırın veya bu öğe Açıklama (burada, derleme dışı bırakılan çıkış gösterilir):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Burada kaldırılan veya metni, Açıklamalı sağ aşağıdakileri ekleyin:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Test showMessage değişken true ise işleme diyorsa bir &lt;p&gt; ileti değişkenini değere sahip öğe.

### <a name="summary-of-your-conditional-logic"></a>Koşullu mantığınızı özeti

Ne yalnızca yaptığınızı tamamen emin olmadığınız durumlarda bir özeti aşağıda verilmiştir.

- İleti değişken varsayılan dize olarak başlatılır ("Bu... ilk kez bir").
- Sonucu bir gönderme (post), sayfa istek ise "gönderdiğiniz artık..." ileti değeri olarak değiştirilir
- ShowMessage değişken false olarak başlatılır.
- Sorgu dizesini içeriyorsa? Göster = true showMessage değişkeni ayarlanır true.
- Biçimlendirme showMessage true ise bir &lt;p&gt; öğesi ileti değerini gösteren işlenir. (ShowMessage false ise, hiçbir şey bu noktada biçimlendirme içinde işlenir.)
- Bir post isteğiyse biçimlendirmede bir &lt;p&gt; öğesi tarihi ve saati gösteren işlenir.

Sayfayı çalıştırın. İşaretlemede if(showMessage) test false döndürecek şekilde showMessage false olduğu için ileti yok, yok.

Tıklayın **gönderme**. Tarih ve saat, ancak yine de hiçbir ileti görürsünüz.

Tarayıcınızda URL kutusuna gidin ve aşağıdaki URL'nin sonuna ekleyin:? Göster = true ayarına ve ardından Enter tuşuna basın.

![Sorgu dizesi gösteren tarayıcı sayfada 'test Razor 2'](intro-to-web-pages-programming/_static/image5.png)

Sayfa yeniden görüntülenir. (URL değiştiğinden, yeni bir istek, bir gönderme budur.) Tıklayın **Gönder** yeniden. İleti, tarih ve saat olarak yeniden görüntülenir.

!['Test Razor 2' sayfası sonra bir sorgu dizesi olduğunda gönderme](intro-to-web-pages-programming/_static/image6.png)

URL'yi değiştirmeniz? Göster true =? Göster = false ve Enter tuşuna basın. Sayfa yeniden gönderin. Sayfa geri nasıl başlangıç için — iletisi yok.

Daha önce belirtildiği gibi bu örnekte mantığını contrived biraz olduğu. Ancak, sayfa, birçoğu görünmesi yayınlanıyorsa ve bir veya daha fazla görülen forms burada sürer.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>(Bir Gravatar görüntüsü görüntüleme) bir yardımcıyı yükleme

Kişiler genellikle web sayfalarında yapmak istediğiniz bazı görevler bir sürü kod gerektiren veya ek bilgi gerektirir. Örnekler: veriler için bir grafik görüntüleme; bir Facebook "Gibi" düğmesine bir sayfada; koyma gönderen e-posta, Web sitesinden; Kırpma veya görüntüleri yeniden boyutlandırmayı; PayPal, siteniz için kullanma. Bu tür bir şeyler yapmasını kolaylaştırır, ASP.NET Web Pages kullanmanıza olanak sağlar. *Yardımcıları*. Bir site yüklediğiniz ve Razor kodunun yalnızca birkaç satır kod kullanarak genel görevleri gerçekleştirmenize izin veren bileşenleri yardımcılardır.

ASP.NET Web Pages birkaç Yardımcıları yerleşik olarak sahiptir. Ancak, birçok Yardımcıları NuGet Paket Yöneticisi'ni kullanarak sağlanan paketleri (add-INS) kullanılabilir. NuGet paketini yüklemek için seçmenize olanak sağlar ve ardından bu yükleme tüm ayrıntılarını üstlenir.

Öğreticinin bu bölümünde Gravatar ("genel olarak tanınan avatar") görüntüyü olanak sağlayan bir yardımcı yükleyeceksiniz. İki şey öğreneceksiniz. Bulma ve yardımcıyı yükleme biridir. Ayrıca, nasıl bir yardımcı kod kendiniz yazmak zorunda kullanarak yapmak için gerekli bir şey kolaylaştırır öğreneceksiniz.

Gravatar Web sitesinden kendi Gravatar kaydedebilirsiniz [ http://www.gravatar.com/ ](http://www.gravatar.com/), ancak öğreticinin bu bölümünde gerçekleştirmek için Gravatar hesabı oluşturmak için gerekli değildir.

Webmatrix'te, tıklayın **NuGet** düğmesi.

![WebMatrix, NuGet galerisindeki iletişim kutusu](intro-to-web-pages-programming/_static/image7.png)

Bu, NuGet Paket Yöneticisi'ni başlatır ve kullanılabilir paketler görüntüler. (Tüm paketler yardımcılardır; bazı işlevler eklemek için WebMatrix kendisi, bazı ek şablonlar ve benzeri.) Sürüm uyumsuzluğu ile ilgili bir hata iletisi alabilirsiniz. Tıklayarak, bu hata iletisini yoksayabilirsiniz **Tamam** ve Bu öğretici ile devam etmeden.

![WebMatrix, NuGet galerisindeki iletişim kutusu](intro-to-web-pages-programming/_static/image8.png)

Arama kutusuna "asp.net Yardımcıları" girin. NuGet paketleri arama terimleriyle eşleşen gösterir.

![NuGet galerisinde WebMatrix paket gösteriliyor](intro-to-web-pages-programming/_static/image9.png)

ASP.NET Web Yardımcıları kitaplığı Gravatar görüntülerini kullanımı gibi birçok yaygın görevleri basitleştirir kodunu içerir. Seçin **ASP.NET Web Yardımcıları Kitaplığı** paketini ve ardından **yükleme** yükleyiciyi başlatın. Seçin **Evet** paketi yükleyin ve yüklemeyi tamamlamak için koşulları kabul etmek isteyip istemediğiniz sorulduğunda.

İşte bu kadar. NuGet indirir ve gerekli olabilecek ek bileşenleri dahil her şeyi yükler (*bağımlılıkları*).

Herhangi bir nedenden dolayı bir yardımcı kaldırmanız gerekirse, çok benzer bir işlemidir. Tıklayın **NuGet** düğmesini tıklatın, **yüklü** sekmesini ve kaldırmak istediğiniz paketi seçin.

## <a name="using-a-helper-in-a-page"></a>Bir sayfada bir Yardımcısını kullanarak

Artık yalnızca yüklü Yardımcısı kullanacaksınız. Yardımcı bir sayfaya ekleme işlemi için çoğu Yardımcıları benzerdir.

WebMatrix, bir sayfa oluşturun ve adlandırın *GravatarTest.cshml*. (Yardımcı test etmek için özel bir sayfa oluşturmakta olduğunuz, ancak herhangi bir sayfa sitenizde Yardımcıları kullanabilirsiniz.)

İçinde &lt;gövdesi&gt; öğe, Ekle bir &lt;div&gt; öğesi. İçinde &lt;div&gt; öğesi, bunu yazın:

@Gravatar.

@ Karakteri, kullanmakta olduğunuz Razor kod işaretlemek için aynı karakterdir. **Gravatar** çalıştığınız yardımcı nesnesi.

Nokta (.) yazmanızın hemen ardından, WebMatrix listesini görüntüler. *yöntemleri* (Gravatar Yardımcısı kullanımınıza sunduğu işlevleri):

![Gravatar Yardımcısı IntelliSense aşağı açılan listesi](intro-to-web-pages-programming/_static/image10.png)

Bu özellik olarak da bilinen *IntelliSense*. Bu, size kod bağlamı uygun seçenekleri sağlayarak yardımcı olur. IntelliSense, HTML, CSS, ASP.NET kodu, JavaScript ve diğer Webmatrix'te desteklenen dilleri ile çalışır. Bu, WebMatrix web sayfalarında geliştirmeyi daha kolay hale getirir başka bir özelliktir.

Klavye ve G tuşuna bakın IntelliSense GetHtml yöntemi bulur. Sekme tuşuna basın. IntelliSense seçilen yöntemi (GetHtml) ekler. Bir açma parantezi ve kapatma parantezinden otomatik olarak eklenir dikkat edin. Tırnak işaretleri arasına iki e-posta adresinizi yazın. Profil resminizi bir Gravatar hesabınız varsa, döndürülür. Varsayılan görüntü Gravatar hesabı yoksa döndürülür. İşiniz bittiğinde, satırı şuna benzer:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Artık bir tarayıcıda sayfasını görüntüleyin. Gravatar hesabı olmasına bağlı olarak, resim ya da varsayılan görüntü görüntülenir.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![Varsayılan görüntü](intro-to-web-pages-programming/_static/image12.png)

Ne hakkında bir fikir edinmek için yardımcı sizin yerinize yapıyor, kaynak sayfanın tarayıcıda görüntüleme. Sayfanızın olan HTML yanı sıra, bir tanımlayıcı içeren bir görüntü öğesi görürsünüz. Bu yardımcı sayfasına sahip olduğu yerde işlenen kodudur @Gravatar.GetHtml. Yardımcısı, sağlanan ve doğru görüntü için sağlanan hesabı geri almak için doğrudan için Gravatar konuşuyor kod oluşturulan bilgilerin sürdü.

GetHtml yöntemi ayrıca diğer parametreler sağlayarak görüntü özelleştirmenize olanak sağlar. Aşağıdaki kod, görüntü genişliği ve yüksekliği 40 piksel olan ve adlı, belirtilen varsayılan bir görüntü kullanır istemek gösterilmektedir **wavatar** belirtilen hesabın mevcut değilse.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Bu kod aşağıdaki gibi (varsayılan görüntü rastgele değişir) aşağıdaki sonucu verir.

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Sıradaki gelen

Bu öğretici kısa tutmak için yalnızca birkaç temel öğeler üzerinde odaklanmak vardı. Doğal olarak, var olan bir *çok* Razor ve C# için daha fazla. Daha fazla şekilde bu öğreticileri öğreneceksiniz. Razor ve C# programlama yönleri hakkında daha fazla şu anda öğrenmekle ilgileniyorsanız, daha kapsamlı bir giriş burada okuyabilirsiniz: [giriş kullanımına ASP.NET Web programlama Razor söz dizimi](https://go.microsoft.com/fwlink/?LinkID=202890).

Sonraki öğretici, bir veritabanı ile çalışmaya tanıtır. Bu öğreticide, en sevdiğiniz film listesinde olanak sağlayan örnek uygulama oluşturmaya başlarsınız.

## <a name="complete-listing-for-testrazor-page"></a>Tam listesi için TestRazor sayfası

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Tam listesi için TestRazorPart2 sayfası

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Tam listesi için GravatarTest sayfası

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Web programlama Razor söz dizimini kullanarak giriş](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Twitter Yardımcısı](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Önceki](getting-started.md)
> [İleri](displaying-data.md)
