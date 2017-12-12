---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: "Razor sözdizimini (C#) kullanarak ASP.NET Web programlamaya giriş | Microsoft Docs"
author: tfitzmac
description: "Bu bölümde Razor sözdizimini kullanarak ASP.NET Web sayfaları ile programlama genel bir bakış sağlar. ASP.NET dinamik web pa çalıştırmak için Microsoft'un teknolojidir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: f054d574026ab6444cc59a126ef9dcdc323f7bff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Razor sözdizimini (C#) kullanarak ASP.NET Web programlamaya giriş
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede Razor sözdizimini kullanarak ASP.NET Web sayfaları ile programlama genel bir bakış sağlar. ASP.NET, dinamik web sayfaları web sunucuları üzerinde çalışan Microsoft teknolojisidir. C# programlama dilini kullanarak bu makaleler odaklar.
> 
> **Öğrenecekleriniz**:
> 
> - Programlama ipuçları Razor sözdizimini kullanan ASP.NET Web sayfaları programlama ile çalışmaya başlama için üst 8.
> - Temel programlama kavramları, gerekir.
> - ASP.NET sunucu kodu nedir ve Razor sözdizimi olan tüm hakkında.
>   
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="the-top-8-programming-tips"></a>8 programlama ipuçları

Bu bölümde kesinlikle Razor sözdizimini kullanan ASP.NET sunucusu kod yazmaya başladığınızda bilmeniz gereken birkaç ipucu listelenir.

> [!NOTE]
> Razor sözdizimi C# programlama dilini üzerinde temel alır ve ASP.NET Web sayfaları ile en sık kullanılan dil olmasıdır. Ancak, Razor sözdizimini Visual Basic dili ve Visual Basic'te da yapabilirsiniz gördüğünüz her şeyi destekler. Ek ayrıntılar için bkz [Visual Basic dili ve sözdizimi](https://go.microsoft.com/fwlink/?LinkId=202908).


Bu programlama tekniklerinin çoğunu hakkında daha fazla bilgi makalenin sonraki bölümlerinde bulabilirsiniz.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Kod kullanarak bir sayfa ekleyin @ karakteri

`@` Karakter satır içi ifadeler, tek bir deyimde blokları ve birden fazla deyim blokları başlatır:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Bu sayfa bir tarayıcıda çalıştığında bu deyimleri nasıl göründüğünü oluşur:

![Razor Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **HTML kodlaması**
> 
> Kullanarak bir sayfa içeriği görüntülediğinizde `@` karakter, önceki örneklerde olduğu gibi çıktı ASP.NET HTML olarak kodlar. Bu ayrılmış HTML karakterlerini değiştirir (gibi `<` ve `>` ve `&`) HTML etiketleri veya varlıklar olarak yorumlanmak yerine bir web sayfasında karakter olarak görüntülenecek karakterleri etkinleştirmek kodlarıyla. HTML kodlaması olmadan sunucu kodunuzu çıktısını düzgün görüntülenmeyebilir ve sayfa güvenlik risklerine maruz bırakabileceğinden.
> 
> Amacınız biçimlendirme olarak etiketlerini işler HTML biçimlendirmesi çıkış olup olmadığını (örneğin `<p></p>` paragrafı için veya `<em></em>` metin vurgulamak için), bölümüne bakın [birleştirme metin, biçimlendirme ve kod blokları kodda](#BM_CombiningTextMarkupAndCode) bu makalenin ilerisinde yer.
> 
> Daha fazla bilgiyi, HTML kodlaması hakkında [formlarla çalışmak](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Kod blokları içinde köşeli parantez içine

A *kod bloğunu* ayraç içine ve bir veya daha fazla kod deyimleri içerir.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. Bir blok içinde her kod açıklaması noktalı virgül ile bitmelidir

Kod bloğu içinde her tam kod deyimi noktalı virgül ile bitmelidir. Satır içi ifadeler noktalı virgül ile sona ermez.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Değerlerini depolamak için değişkenleri kullanma

Değerleri depolayabilir bir *değişkeni*dizeler, sayılar ve tarihleri, vb. dahil olmak üzere. Kullanarak yeni bir değişken oluşturmak `var` anahtar sözcüğü. Doğrudan sayfasını kullanarak bir değişken değerleri ekleyebilirsiniz `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Değişmez değer dize değerleri çift tırnak işaretleri içine alın

A *dize* metin olarak kabul edilir karakter sırasıdır. Bir dize belirtmek için çift tırnak işaretleri içine alın:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Görüntülemek istediğiniz dizesi bir ters bölü karakteri içeriyorsa ( `\` ) veya çift tırnak işareti ( `"` ), kullanan bir *verbatim dize sabit değeri* ile önek `@` işleci. (C# ' ta, \ verbatim dize sabit değeri kullanmadığınız sürece özel bir anlamı olan karakter.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Çift tırnak işaretleri eklemek için verbatim dize sabit değeri kullanın ve tırnak işaretleri yineleyin:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Bu örneklerin her ikisinin bir sayfasında kullanmanın sonucu şöyledir:

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Dikkat `@` karakter verbatim dize değişmez değerleri C# işaretlemek ve ASP.NET sayfaları kodda işaretlemek için kullanılır.


### <a name="6-code-is-case-sensitive"></a>6. Kodu büyük küçük harfe duyarlı.

C#, anahtar sözcükler (gibi `var`, `true`, ve `if`) ve değişken adları büyük küçük harfe duyarlı. Aşağıdaki kod satırlarını iki farklı değişkenleri oluşturma `lastName` ve`LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Bir değişken olarak bildirirseniz `var lastName = "Smith";` ve sayfanız olarak bu değişken başvuru çalışırsanız `@LastName`, bir hata nedeniyle oluşur `LastName` tanınmaz.

> [!NOTE]
> Visual Basic anahtar sözcükleri ve değişkenleri olan *değil* büyük küçük harfe duyarlı.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Kodlama çoğunu nesneleri içerir

Bir *nesne* ile program bir şey &#8212; temsil eden bir sayfa, bir metin kutusu, bir dosya, görüntü, bir web isteği, bir e-posta iletisi, bir müşteri kaydı (veritabanı satır) vb. Nesnelerin özelliklerini açıklayan özellikleri vardır ve, okuyabilirsiniz &#8212; veya Değiştir bir metin kutusu nesnesi olan bir `Text` istek nesnesi özelliği (diğerlerinin yanı sıra) sahip bir `Url` özelliği, e-posta iletisine sahip bir `From` özelliğine ve bir müşteri nesnesi sahiptir bir `FirstName` özelliği. Nesneleri olan yöntemlerini de &quot;fiiller&quot; yapabilirler. Örnekler bir dosya nesnesinin `Save` yöntemi, bir görüntü nesnenin `Rotate` yöntemi ve e-posta nesnenin `Send` yöntemi.

Genellikle ile karşılaşmayacağınızı `Request` , metin kutuları (form alanları) değerleri gibi bilgiler sağlayan tarayıcının ne tür, sayfa, kullanıcı kimliği, vb. URL'sini istekte sayfasında, nesne. Aşağıdaki örnek özelliklerine erişmek nasıl gösterir `Request` nesne ve nasıl çağrılacağını `MapPath` yöntemi `Request` sayfasının mutlak yolu sunucuda verir nesnesi:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Kararları kod yazma

Bir anahtar dinamik web sayfaları koşullara dayanarak ne yapacağınıza belirlemek özelliğidir. Bunu yapmak için en yaygın yolu `if` deyimi (ve isteğe bağlı `else` deyimi).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

Deyim `if(IsPost)` kestirme yol yazma, `if(IsPost == true)`. İle birlikte `if` deyimleri, çeşitli yollarla koşullarda, kod, yineleme bloklarını test etmek için vardır ve vb., bu makalenin sonraki bölümlerinde açıklanmıştır.

Bir tarayıcıda görüntülenen sonuç (tıkladıktan sonra **gönderme**):

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET ve POST yöntemleri ve IsPost özelliği
> 
> Web sayfaları (HTTP) için kullanılan protokol çok sınırlı sayıda sunucuya isteği yapmak için kullanılan yöntemleri (fiiller) destekler. İki en yaygın bir sayfa okumak için kullanılan GET ve sayfa göndermek için kullanılan POST olanlardır. Genel olarak, bir kullanıcı bir sayfa istekleri ilk kez sayfa GET kullanarak istendi. Kullanıcı bir formda doldurur ve Gönder düğmesine tıkladığında, tarayıcı sunucuya bir POST isteği yapar.
> 
> Programlama web sayfasını işlemek nasıl bilmesi bir sayfa bir GET veya POST olarak istenen olup olmadığını öğrenmek yararlıdır. ASP.NET Web Pages'da kullandığınız `IsPost` bir isteğin bir GET veya POST olup olmadığını görmek için özellik. Bir POST isteğiyse `IsPost` özelliği true döndürür ve metin kutuları bir form üzerinde değerlerini okuma gibi şeyler yapabilirsiniz. Birçok örnek göreceksiniz sayfanın farklı değerine bağlı olarak işlemek nasıl Göster `IsPost`.


## <a name="a-simple-code-example"></a>Basit bir kod örneğidir

Bu yordam temel programlama tekniklerinin gösteren bir sayfa oluşturulacağını gösterir. Örnekte, kullanıcıların bunları ekler ve sonucu görüntüler sonra iki sayıyı girin olanak sağlayan bir sayfa oluşturun.

1. Düzenleyicinizde, yeni bir dosya oluşturun ve adlandırın *AddNumbers.cshtml*.
2. Aşağıdaki kod ve biçimlendirme zaten sayfasındaki herhangi bir şey değiştirme sayfanın kopyalayın.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Dikkat edilecek bazı öğeleri şunlardır:

    - `@` Karakter sayfasında ilk kod bloğunu başlar ve kendisinden `totalMessage` sayfanın altı katıştırılmış değişkeni.
    - Sayfanın üstündeki blok kaşlı ayraçlar içinde yer.
    - Üst bloğunda noktalı virgül tüm satırların bitiş.
    - Değişkenleri `total`, `num1`, `num2`, ve `totalMessage` birkaç numarası ve bir dize depolar.
    - Atanan değişmez dize değeri `totalMessage` çift tırnak işaretleri değişkenidir.
    - Kod zaman büyük küçük harfe duyarlı, olduğundan `totalMessage` değişkeni sayfanın altı kullanıldığında, adını üst değişken tam olarak eşleşmelidir.
    - İfade `num1.AsInt() + num2.AsInt()` nesneleri ve yöntemleri ile nasıl çalışılacağı gösterir. `AsInt` Yöntemi her değişken, aritmetik üzerinde gerçekleştirebilmeleri için bir sayı (tamsayı) bir kullanıcı tarafından girilen dize dönüştürür.
    - `<form>` Etiketi de içeren bir `method="post"` özniteliği. Bu belirtir kullanıcı tıklattığında **Ekle**, sayfa HTTP POST yöntemini kullanarak sunucuya gönderilir. Sayfa gönderildiğinde `if(IsPost)` test değerlendirir true ve koşullu kod sayıları ekleme sonucu görüntülenirken çalıştırır.
3. Sayfayı kaydedin ve tarayıcıda çalıştırın. (Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.) İki tam sayılar girin ve ardından **Ekle** düğmesi. 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Temel programlama kavramları

Bu makalede, ASP.NET web programlama bir bakış sağlar. Yalnızca hızlı bir tur en sık kullanacağınız programlama kavramları aracılığıyla ayrıntılı bir incelemesi değil. Buna rağmen neredeyse ASP.NET Web sayfaları ile çalışmaya başlamak için gereken her şeyi kapsar.

Ancak ilk olarak, küçük bir teknik arka plan.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Razor sözdizimi, sunucu kodu ve ASP.NET

Razor sözdizimi kod sunucu tabanlı bir web sayfasında katıştırmak için basit bir programlama sözdizimi şeklindedir. Razor sözdizimini kullanan bir web sayfası içeriği iki tür vardır: istemci içeriği ve sunucu kodu. İstemci içeriktir için kullandığınız web sayfalarında şeyler: HTML biçimlendirmesi (öğeleri), stil CSS gibi bilgileri belki JavaScript ve düz metin gibi bazı istemci komut dosyası.

Razor sözdizimi, bu istemci içerik için sunucu kodu eklemenizi sağlar. Sayfa tarayıcıya göndermeden önce sunucunun, bu kod sayfasında sunucu kodu varsa, ilk olarak, çalışır. Sunucu üzerinde çalışan tarafından kodu sunucu tabanlı veritabanlarına erişme gibi tek başına, istemci içeriği kullanarak yapmak için çok daha karmaşık olabilir görevleri gerçekleştirebilirsiniz. En önemlisi, sunucu kodu dinamik olarak istemci içeriği &#8212;oluşturabilirsiniz; Bu HTML biçimlendirmesi veya diğer içerik kolay bir şekilde oluşturmak ve sayfa içerebilecek herhangi bir statik HTML birlikte tarayıcı gönderin. Tarayıcının açısından bakıldığında, sunucu kodunuz tarafından oluşturulan istemci içeriği herhangi bir istemci içerik farklı değildir. Önceden gördüğünüz gibi gerekli olan sunucu kodu oldukça basittir.

Razor sözdizimi içeren ASP.NET web sayfaları bir özel dosya uzantısına sahip (*.cshtml* veya *.vbhtml*). Sunucu bu uzantıları tanır, Razor sözdizimi ile işaretlenmiş ve ardından sayfanın tarayıcıya gönderir. kodu çalıştırır.

### <a name="where-does-aspnet-fit-in"></a>ASP.NET nerelerde uygundur?

Razor sözdizimi sırayla Microsoft .NET Framework temel alınarak ASP.NET adlı Microsoft teknolojisini temel alır. Neredeyse her türlü bilgisayarı uygulama geliştirmek için büyük, kapsamlı bir programlama çerçevesi Microsoft.NET Framework var. ASP.NET web uygulamaları oluşturmak için özel olarak tasarlanmıştır .NET Framework'ün parçasıdır. Geliştiriciler dünyanın en büyük ve en yüksek trafik Web siteleri çoğunu oluşturmak için ASP.NET kullanmış. (Her zaman dosya adı uzantısı bkz *.aspx* URL bir sitedeki bir parçası olarak, site ASP.NET kullanılarak yazılmıştır anlarsınız.)

Razor sözdizimi, ASP.NET, ancak bir başlangıç iseniz ve size daha üretken yapıyorsa uzmanı olup olmadığınızı öğrenmek daha kolay bir Basitleştirilmiş söz dizimini kullanarak tüm güç sağlar. Bu sözdiziminin kullanmak basit olsa bile, ASP.NET ve .NET Framework ailesi ilişkisini sitelerinizi daha karmaşık hale geldikçe kullanabileceğiniz daha büyük çerçeveleri güç olduğu anlamına gelir.

![Razor Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Sınıfları ve örnekleri**
> 
> ASP.NET sunucusu kod sınıfları hakkında fikir üzerinde sırayla yerleşik nesneleri kullanır. Sınıf tanımı ve şablonu bir nesne için ' dir. Örneğin, bir uygulama içerebilir bir `Customer` sınıfı özellikleri ve herhangi bir müşteri nesnesi gereken yöntemleri tanımlar.
> 
> Uygulamayı gerçek müşteri bilgileri ile çalışmak gerektiğinde bir örneğini oluşturur (veya *başlatır*) bir müşteri nesnesi. Her müşterinin ayrı bir örneği olan `Customer` sınıfı. Her örnek aynı özellikleri ve yöntemleri destekler, ancak her müşteri nesnesi benzersiz olduğundan her örneği için özellik değerlerini genellikle farklı. Bir müşteri nesnesindeki `LastName` özelliği "Smith" olabilir; başka bir müşteri nesnesindeki `LastName` özelliği "Can" olabilir
> 
> Benzer şekilde, tüm tek tek web sitenizde sayfasıdır bir `Page` örneği nesne `Page` sınıfı. Düğme sayfasında bir `Button` örneği nesne `Button` sınıfı ve benzeri. Her örneğinin kendi özellikleri vardır, ancak bunlar tüm nesnenin sınıf tanımında belirtilen üzerinde temel alır.


## <a name="basic-syntax"></a>Temel sözdizimi

Daha önce bir ASP.NET Web Pages sayfası oluşturma ve sunucu kodunu HTML biçimlendirmesi nasıl ekleyebileceğiniz temel bir örneği gördünüz. Burada Razor sözdizimi &#8212;kullanarak ASP.NET sunucusu kod yazmaya temel bilgileri öğreneceksiniz; diğer bir deyişle, programlama dili kuralları.

(Özellikle, C, C++, C#, Visual Basic veya JavaScript kullandıysanız) programlama ile deneyimli değilseniz, ne burada okuma çoğunu tanıdık gelecektir. Büyük olasılıkla yalnızca nasıl sunucu kodu biçimlendirmede eklenen ile öğrenmeniz gerekir *.cshtml* dosyaları.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Metin, biçimlendirme ve kod blokları içinde kodu birleştirme

Sunucu kod bloğu, genellikle çıkış metni biçimlendirme (veya her ikisi de) sayfasına istersiniz. Sunucu kod bloğu kodu değil ve, bunun yerine olarak işleneceğini metin içeriyorsa, ASP.NET metnin kodunu ayırt etmek gerekir. Bunu yapmanın birkaç yolu vardır.

- Gibi bir HTML öğesindeki metni içine `<p></p>` veya `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    HTML öğesi, metin, ek HTML öğeleri ve sunucu kodu ifadeleri içerebilir. ASP.NET, HTML etiketi açılış gördüğünde (örneğin, `<p>`), öğe dahil her şeyi işler ve bunun içeriği olarak gidiyor gibi sunucu kodu ifadeleri çözme tarayıcıya.
- Kullanım `@:` işleci veya `<text>` öğesi. `@:` Tek satırlık bir düz metin veya eşleşmeyen HTML etiketleri; içeren içerik çıkarır `<text>` öğesi çıktısını almak için birden fazla satır alır. Bu seçenekler, çıkışı bir parçası olarak bir HTML öğesini işlemek istemediğiniz zaman yararlıdır.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Çok satırlı metin ya da eşleşmeyen HTML etiketleri çıktı istiyorsanız, her satırın önünde `@:`, veya satırda içine bir `<text>` öğesi. Gibi `@:` işleci,`<text>` etiketleri metin içeriği tanımlamak için ASP.NET tarafından kullanılan ve sayfa çıktıda hiçbir zaman işlenir.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    İlk örnek önceki örnek yineler ancak tek bir çift kullanır `<text>` etiketleri oluşturmak için metni alın. İkinci örnekteki `<text>` ve `</text>` etiketlerini içine üç satır, bunların tümü sahip bazı uncontained metin ve eşleşmeyen HTML etiketleri (`<br />`), sunucu kodu ve eşleşen HTML etiketleri yanı sıra. Yine de her satırın tek tek koyun `@:` işleci; her iki şekilde çalışır.

    > [!NOTE]
    > Ne zaman bu bölümde &#8212;gösterildiği gibi metin çıktısını; bir HTML öğesi kullanarak `@:` işleci veya `<text>` öğesi &#8212; ASP.NET çıktı HTML olarak kodlanacak değil. (Daha önce belirtildiği gibi ASP.NET sunucu kodu ifadeleri ve tarafından öncesinde sunucu kod blokları çıktısını kodlamak `@`, bu bölümde belirtildiği özel durumlar hariç.)

### <a name="whitespace"></a>Boşluk

Ek boşluk deyimi içinde (ve bir değişmez dize değeri dışında) deyim etkilemez:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Satır sonu deyimi içinde deyim üzerinde hiçbir etkisi olmaz ve Okunabilirlik için deyimleri kayabilir. Aşağıdaki deyimleri aynıdır:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Ancak, bir değişmez dize değeri ortasında bir satır kaydırılamıyor. Aşağıdaki örnek çalışmıyor:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Yukarıdaki kod gibi birden çok satır sarmalar uzun bir dize birleştirmek için iki seçenek vardır. Birleştirme işleci kullanabilirsiniz (`+`), hangi bu makalenin sonraki bölümlerinde görürsünüz. Aynı zamanda `@` bu makalede anlatıldığı gibi aynen dize değişmez değeri oluşturmak için karakter. Harfi harfine dize değişmez değerleri satırlara bölünebilir:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Kod (ve biçimlendirme) açıklamaları

Açıklamalar, kendiniz veya başkaları için Notlar bırakın olanak tanır. Devre dışı bırakmak de sağlarlar (*çıkışı açıklama*) bir bölümünü kodu veya çalıştırmak istediğiniz yoktur, ancak şimdilik sayfanıza tutmak istediğiniz biçimlendirme.

Sözdizimi HTML biçimlendirmesi ve Razor kod için yorum oluşturma farklı yoktur. Tüm Razor kod gibi ile Razor açıklamalar işlenen (ve sonra kaldırılan) sayfa tarayıcıya gönderilmeden önce sunucuda. Bu nedenle, Razor yorum sözdizimi, dosyayı düzenlemeniz ancak kullanıcıların görmüyorum, hatta sayfa kaynağında görebilirsiniz açıklamaları kodu (veya hatta işaretleme) yerleştirmenizi sağlar.

ASP.NET Razor açıklamaları için açıklama ile başlangıç `@*` ve ile bitmelidir `*@`. Açıklama, tek bir çizgi veya birden çok satır olabilir:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Kod bloğu içinde bir yorum şöyledir:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Böylece çalışmaz burada kod, aynı blok kod satırı ile kılınmıştır:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

Kod bloğunun içine Razor açıklama sözdizimi kullanılarak alternatif olarak, C# gibi kullandığınız programlama dili yorum söz dizimini kullanabilirsiniz:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

C# ' ta tek satırlı yorumlar öncesinde `//` karakterler ve çok satırlı açıklamaları ile başlar `/*` ve sonunda `*/`. (Razor yorumlarla gibi C# açıklamaları tarayıcıya işlenmez.)

İşaretleme için bildiğiniz gibi bir HTML açıklaması oluşturabilirsiniz:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

HTML açıklamaları başlayarak `<!--` karakterler ve ile biten `-->`. Yalnızca metin ancak sayfasında tutmak isteyebilirsiniz, ancak işlemek istemediğiniz ayrıca herhangi bir HTML biçimlendirmesi surround için HTML açıklamaları kullanabilirsiniz. Bu HTML açıklama, etiketler ve içerdikleri metin tüm içeriğini gizlenir:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Razor açıklama, HTML açıklamaları aksine *olan* sayfasına işlenmiş ve kullanıcı onları sayfa kaynağı görüntüleyerek görebilirsiniz.

Razor iç içe geçmiş bloklarını C# üzerinde sınırlamalara sahiptir. Daha fazla bilgi için bkz: [adlı C# değişkenleri ve iç içe geçmiş blokları bozuk kodu oluştur](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Değişkenler

Bir değişken verilerini depolamak için kullandığınız adlandırılmış bir nesnedir. Herhangi bir şey değişkenleri adı verebilirsiniz ancak adı alfabetik bir karakter ile başlamalı ve boşluk ya da ayrılmış karakterler içeremez.

### <a name="variables-and-data-types"></a>Değişkenleri ve veri türleri

Bir değişken ne tür veriler değişkende depolanır gösteren bir özel veri türü olabilir. Dize değerlerini depolayan dize değişkenleri olabilir (gibi &quot;Merhaba Dünya&quot;), tam sayı değerleri (örneğin, 3 veya 79) depolamak tamsayı değişkenleri ve biçimleri (4/12/2012 veya Mart 2009 gibi çeşitli tarih değerlerini depolama tarih değişkenleri ). Ve kullanabileceğiniz birçok diğer veri türleri vardır.

Ancak, genellikle bir değişken için bir tür belirtmeniz gerekmez. Çoğu zaman, ASP.NET değişkeni verileri nasıl kullanıldığını temel türü tahmin. (Bazen bir türü belirtmeniz gerekir; bu doğru olduğu örnekler görürsünüz.)

Bir değişken kullanarak bildirme `var` (bir tür belirtmek istemiyorsanız) anahtar sözcüğü veya türünün adını kullanarak:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

Aşağıdaki örnek, bir web sayfasında bazı tipik kullanımları değişkenlerin gösterir:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Bir sayfa önceki örneklerde birleştiriyorsanız, bu bir tarayıcıda görüntülenen bakın:

![Razor Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Dönüştürme ve veri türleri test etme

Bazen ASP.NET genellikle bir veri türü otomatik olarak belirlemek de, işlem gerçekleştirilemiyor. Bu nedenle, açık bir dönüştürme gerçekleştirerek ASP.NET yardımcı olması gerekebilir. Bazen türleri dönüştürme gerekmez olsa bile, veri türünü, birlikte çalışabilir görmek için test etmek yardımcı olur.

En yaygın olarak bir tam sayı veya tarih gibi bir dizeyi başka bir türüne dönüştürmek sahip olur. Aşağıdaki örnek, burada, bir dizeyi sayıya dönüştürme gerekir tipik bir durumu gösterir.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Bir kural olarak, kullanıcı girişi için dize olarak gelir. Bir sayı girin kullanıcılardan olsa bile ve bir rakam girmiş olduğunuz da kullanıcı girişi gönderildiğinde ve kodda okuma olsa bile, veri dize biçiminde sağlanır. Bu nedenle, dize bir sayıya dönüştürmeniz gerekir. Bunları, dönüştürmeden değerlerine aritmetik gerçekleştirmeye çalışırsanız ASP.NET iki dizeyi eklediğinden örnekte, aşağıdaki hata, sonuçları:

*Örtük olarak 'int' için ' string' türü dönüştürülemiyor.*

Tamsayılara değerleri dönüştürmek için arama `AsInt` yöntemi. Dönüştürme başarılı olursa, sayıları daha sonra ekleyebilirsiniz.

Aşağıdaki tabloda bazı yaygın dönüştürme ve test yöntemleri değişkenleri listeler.

| **Yöntemi** | **Açıklama** | **Örnek** |
| --- | --- | --- |
| `AsInt(), IsInt()` | Bir tam sayı (örneğin, "593") bir tamsayı olarak temsil eden bir dize dönüştürür. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)] |
| `AsBool(), IsBool()` | Gibi bir dizeyi dönüştürür &quot;true&quot; veya &quot;false&quot; bir Boolean türü. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)] |
| `AsFloat(), IsFloat()` | Gibi ondalık bir değeri olan bir dize dönüştürür &quot;1.3&quot; veya &quot;7.439&quot; bir kayan noktalı sayı. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)] |
| `AsDecimal(), IsDecimal()` | Gibi ondalık bir değeri olan bir dize dönüştürür &quot;1.3&quot; veya &quot;7.439&quot; ondalık sayıya. (ASP.NET, ondalık sayı bir kayan nokta numarasından daha kesin.) | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)] |
| `AsDateTime(), IsDateTime()` | ASP.NET için bir tarih ve saat değerini temsil eden bir dize dönüştürür `DateTime` türü. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)] |
| `ToString()` | Başka bir veri türü bir dizeye dönüştürür. | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)] |

## <a name="operators"></a>İşleçler

Bir işleç bir anahtar sözcük veya ASP ne tür bir ifadede gerçekleştirmek için komutu bir karakter değil. C# dili (ve bunu temel alan Razor sözdizimi) birçok işleçleri destekler, ancak yalnızca başlamak için birkaç tanıması gerekir. Aşağıdaki tabloda, en yaygın işleçleri özetler.

| **İşleci** | **Açıklama** | **Örnekler** |
| --- | --- | --- |
| `+``-``*``/` | Matematik işleçleri sayısal ifadelerde kullanılır. | [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)] |
| `=` | Atama. Sol tarafındaki nesnesine sağ tarafında deyiminin değeri atar. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)] |
| `==` | Eşitlik. Döndürür `true` değerleri aynıysa. (Arasında ayrım fark `=` işleci ve `==` işleci.) | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)] |
| `!=` | Eşitsizlik. Döndürür `true` değerler eşit değilse. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)] |
| `< > <= >=` | Daha az-büyük daha-daha, daha az-daha-veya-eşittir ve büyük-daha-veya-eşittir. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)] |
| `+` | Birleştirme dizeleri eklemek için kullanılır. ASP.NET bu işleci ifade veri türüne göre toplama işleci arasındaki fark bilir. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)] |
| `+=``-=` | Ekleme ve 1 (sırasıyla) bir değişkeninden artırma ve azaltma işleçleri. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)] |
| `.` | Nokta. Nesneleri ve özellikleri ve yöntemleri ayırt etmek için kullanılır. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)] |
| `()` | Parantez. Grup ifadeleri ve parametreleri yöntemlere geçirmek için kullanılır. | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)] |
| `[]` | Köşeli ayraçlar. Diziler veya koleksiyonlar değerleri erişmek için kullanılır. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)] |
| `!` | Değil. Tersine çevirir bir `true` değeri `false` tersi. Sınamak için bir toplu şekilde genelde kullanılan `false` (diğer bir deyişle, için değil `true`). | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)] |
| `&&`<code>&#124;&#124;</code> | Mantıksal AND ve OR koşulları birlikte hangi bağlamak için kullanılır. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)] |

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Dosya ve klasör yollarında kodu ile çalışma

Genellikle, kodunuzda iş ile dosya ve klasör yolları. Burada, geliştirme bilgisayarınızda görünebilir gibi bir Web sitesi için fiziksel klasör yapısı örneği verilmiştir:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

URL'ler ve yollar hakkında bazı temel ayrıntıları aşağıdadır:

- Bir URL ile ya da bir etki alanı adı başlar (`http://www.example.com`) veya bir sunucu adı (`http://localhost`, `http://mycomputer`).
- Bir URL bir ana bilgisayardaki fiziksel bir yola karşılık gelir. Örneğin, `http://myserver` klasörüne karşılık gelebilir *C:\websites\mywebsite* sunucusunda.
- Sanal yol tam yolunu belirtmek zorunda kalmadan kod yolları temsil etmek için toplu özelliktir. Etki alanı veya sunucu adı bir URL bölümünü içerir. Sanal yol kullandığınızda, yolları güncelleştirmek zorunda kalmadan farklı etki alanı veya sunucu için kodunuzu taşıyabilirsiniz.

Farkları anlamanıza yardımcı olması için örnek aşağıda verilmiştir:

| Tam URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Sunucu adı | *mycompanyserver* |
| Sanal yol | */HumanResources/CompanyPolicy.htm* |
| Fiziksel yol | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Sanal kök / yalnızca C: aynı kök gibi sürücüdür \. (Sanal klasör yolları her zaman eğik kullanın.) Sanal bir klasör yolu fiziksel klasör olarak aynı ada sahip gerekmez; bir diğer ad olabilir. (Üretim sunucularında, sanal yolu nadiren tam fiziksel yolunu eşleşir.)

Bazen kodu bulunan dosya ve klasörleri ile çalışırken, fiziksel yol ve bazen çalıştığınız hangi nesneleri bağlı olarak sanal bir yol başvuru gerekir. ASP.NET size bu araçları kodu dosya ve klasör yollarında ile çalışmak için: `Server.MapPath` yöntemi ve `~` işleci ve `Href` yöntemi.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Fiziksel için sanal yol dönüştürme: Server.MapPath yöntemi

`Server.MapPath` Yöntemi dönüştürür sanal bir yol (gibi */default.cshtml*) mutlak fiziksel yola (gibi *C:\WebSites\MyWebSiteFolder\default.cshtml*). Tam fiziksel yolunu gerek duyduğunuz her zaman bu yöntemi kullanın. Okunurken ya da bir metin dosyası veya web sunucusunda görüntü dosyası yazılırken tipik bir örnektir.

Mutlak fiziksel yol barındırma site sunucusunda, sitenizin genellikle bilmiyorsanız, bu yöntem yolu dönüştürebilirsiniz şekilde bildiğiniz — sanal yolu —, sunucuda karşılık gelen yolu. Sanal yol bir dosya veya klasör yöntemine geçirdiğiniz ve fiziksel yolu döndürür:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Sanal kök başvuran: ~ işleci ve Href yöntemi

İçinde bir *.cshtml* veya *.vbhtml* dosyası, sanal kök yolu kullanarak başvuru `~` işleci. Bu bir sitede sayfaları taşıyabilirsiniz ve diğer sayfalara içerdikleri tüm bağlantılar bozuk olmayacaktır çünkü çok kullanışlıdır. Ayrıca, her zamankinden Web sitenizi farklı bir konuma taşımanız durumunda kullanışlıdır. Bazı örnekler şunlardır:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Web sitesi ise `http://myserver/myapp`, işte sayfa çalıştığında ASP.NET bu yollar nasıl kabul eder:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Aslında bu yollar değişkeni değerleri olarak görmez, ancak, ne oldukları ise ASP.NET yollar kabul eder.)

Kullanabileceğiniz `~` işleci (yukarıdaki gibi) sunucu kodu hem de bu gibi biçimlendirme:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Biçimlendirme içinde kullandığınız `~` görüntü dosyaları, diğer web sayfalarını ve CSS dosyaları gibi kaynaklarına yollar oluşturmak için işleç. Sayfa çalıştığında, ASP.NET (kod ve biçimlendirme) sayfadan arar ve tüm çözümler `~` uygun yolu başvuruları.

## <a name="conditional-logic-and-loops"></a>Koşullu mantık ve döngüler

ASP.NET sunucusu kod koşullara göre görevlerini gerçekleştirmek ve belirli bir sayıda deyimleri yineler kod yazmayı sağlar (diğer bir deyişle, bir döngü çalıştırır kodu).

### <a name="testing-conditions"></a>Koşullar test etme

Kullandığınız basit bir koşulu test etmek için `if` true veya false değerini döndürür, belirttiğiniz bir testi temel alan deyimi:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

`if` Anahtar sözcüğü bir blok başlatır. Gerçek test (koşul) parantez içinde olduğundan ve true veya false döndürür. Test true ise çalıştırılan deyimleri ayraç içine alınır. Bir `if` deyimi dahil edebileceğiniz bir `else` koşul yanlış ise, çalıştırılacak deyimleri belirtir engelle:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Kullanarak birden çok koşul ekleyebileceğiniz bir `else if` engelle:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

Eğer ilk koşul, bu örnekte, blok true değil `else if` koşul denetlenir. Bu koşul karşılandığında deyimlerinde `else if` blok çalıştırılır. Hiç bir koşul karşılanıyorsa deyimlerinde `else` blok çalıştırılır. Else IF herhangi bir sayıda ekleyebilirsiniz engeller ve ile kapatın bir `else` olarak engellemek &quot;şey&quot; koşulu.

Çok sayıda koşullar sınamak için kullanın bir `switch` engelle:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Test etmek için parantez içinde değerdir (örnekte `weekday` değişkeni). Tek tek her test kullanan bir `case` iki nokta (:) ile biter deyimi. Varsa değerini bir `case` deyimi eşleşen test değeri, bu örneği bloğundaki kod yürütülür. Her case deyimiyle kapatmak bir `break` deyimi. (Her sonu eklenecek unutursanız `case` engellemek, sonraki koddan `case` deyimi de çalışır.) A `switch` blok genellikle sahip bir `default` deyimi için son durum olarak bir &quot;şey&quot; diğer durumlarda hiçbiri doğruysa, çalıştırılan seçeneği.

Bir tarayıcıda görüntülenen son iki koşullu blokları sonucu:

![Razor Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Döngü kodu

Genellikle aynı deyimleri tekrar tekrar çalıştırmanız gerekir. Bunun için döngüyle. Örneğin, genellikle aynı deyimlerini her öğe için bir veri koleksiyonunda çalıştırın. Tam döngü istediğiniz kaç kez biliyorsanız, kullanabileceğiniz bir `for` döngü. Bu tür bir döngü sayım veya sayım için özellikle yararlı olur:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Döngü ile başlayan `for` ayraç içinde üç deyimleri ve ardından bir anahtar sözcük, her sonlandırıldı noktalı virgül.

- İlk ifade parantez içinde (`var i=10;`) bir sayaç oluşturur ve 10 başlatır. Sayaç adı gerekmez `i` &#8212; herhangi bir değişkeni kullanabilirsiniz. Zaman `for` döngüsü çalıştırıldığında, sayaç otomatik olarak artırılır.
- İkinci ifade (`i < 21;`) saymak istediğiniz kadar koşulu ayarlar. Bu durumda, en fazla 20 için Git istediğiniz (sayaç değerinden 21 durumdayken başka bir deyişle, gidiyor).
- Üçüncü ifade (`i++` ) yalnızca sayacı döngünün her çalıştığında eklenene 1 olmalıdır belirten bir artış işleci kullanır.

Köşeli parantez her döngü için çalışacak kodudur. Yeni bir paragraf biçimlendirme oluşturur (`<p>` öğesi) her zaman ve değerini görüntüleme çıktısı için bir satır ekler `i` (sayaç). Bu sayfayı çalıştırdığınızda, örnek öğe sayısını gösteren her satırın metinle çıkış görüntüleme 11 satırları oluşturur.

![Razor Img11](introducing-razor-syntax-c/_static/image11.jpg)

Bir koleksiyonu veya dizisi ile çalışıyorsanız, sık kullandığınız bir `foreach` döngü. Bir koleksiyon benzer nesneleri oluşan bir gruptur ve `foreach` döngü koleksiyondaki her öğe bir görevi yerine getirmek sağlar. Bu tür bir döngü koleksiyonlar için uygun olan çünkü aksine bir `for` döngüsü, sınırı sayaç artırın veya zorunda değilsiniz. Bunun yerine, `foreach` döngü kodunu işlemi tamamlanana kadar toplulukta yalnızca geçer.

Örneğin, aşağıdaki kod öğeleri döndürür `Request.ServerVariables` web sunucunuz hakkında bilgi içeren bir nesne koleksiyonu. Kullandığı bir `foreac` yeni oluşturarak her öğenin adını görüntülemek için h döngü `<li>` öğesi bir HTML madde işaretli listede.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

`foreach` Anahtar sözcüğü ardından parantez tarafından nerede koleksiyonunda tek bir öğeyi temsil eden bir değişken bildirme (örnekte `var item`), ardından `in` döngü istediğiniz koleksiyonu ve ardından bir anahtar sözcük,. Gövdesinde `foreach` döngü, daha önce bildirilen değişkeni kullanarak geçerli öğesi erişebilir.

![Razor Img12](introducing-razor-syntax-c/_static/image12.jpg)

Daha fazla genel amaçlı bir döngü oluşturmak üzere kullanmanız `while` deyimi:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A `while` döngü başlıyorsa `while` parantez tarafından ne kadar süreyle döngünün devam belirlediğiniz anahtar sözcüğünü, (burada için sürece `countNum` değerinden 50'dir), yinelenecek sonra bloğu. Döngüler genellikle Artır (Ekle) veya azaltma (Çıkart) bir değişken veya sayım için kullanılan nesne. Örnekte, `+=` işleci ekler 1 `countNum` döngünün her çalıştığında. (Bir değişken sayıları aşağı Döngüdeki düşürmek için azaltma işleci kullanırsınız `-=`).

## <a name="objects-and-collections"></a>Nesneleri ve koleksiyonları

ASP.NET Web sitesi, neredeyse her şeyi web sayfası dahil olmak üzere bir nesnesidir. Bu bölümde, ile sık kodunuzda karşılaşmayacağınızı bazı önemli nesneleri anlatılmaktadır.

### <a name="page-objects"></a>Sayfa nesneleri

En temel ASP.NET sayfası nesnedir. Hiçbir belirleme nesnesi olmadan doğrudan sayfa nesnesinin özellikleri erişebilir. Aşağıdaki kod sayfanın dosya yolunu alır kullanarak `Request` sayfasının nesnesi:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Bunu yapmak için Temizle özellikleri ve yöntemleri geçerli sayfa nesnesinde başvuran, isteğe bağlı olarak anahtar sözcüğünü kullanabilirsiniz `this` kodunuzda sayfa nesnesini temsil eden. İle önceki kod örneğinde, işte `this` sayfa temsil etmek için eklendi:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Özelliklerini kullanabilirsiniz `Page` nesne gibi bir çok sayıda bilgi, almak için:

- `Request`. Bu önceden gördüğünüz gibi tarayıcının ne tür, sayfa, kullanıcı kimliği, vb. URL'sini istekte dahil olmak üzere geçerli istek hakkındaki bilgiler koleksiyonudur.
- `Response`. Bu, sunucu kodu çalışmasını bitirdikten sonra tarayıcıya gönderilen yanıt (sayfa) hakkında bilgi koleksiyonudur. Örneğin, bilgi yanıtına yazmak için bu özelliği kullanabilirsiniz. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Koleksiyon nesnesi (diziler ve sözlükler)

A *koleksiyonu* aynı türde bir koleksiyonu gibi nesneleri oluşan bir gruptur `Customer` nesnelerini bir veritabanından. ASP.NET içeren birçok yerleşik koleksiyonlar gibi `Request.Files` koleksiyonu.

Koleksiyonlarda verileri genellikle çalışmak. İki ortak koleksiyon türü *dizi* ve *sözlük*. Bir dizi benzer öğeleri koleksiyonu depolamak istiyorsanız, ancak her bir öğeyi tutmak için ayrı bir değişken oluşturmak istemediğiniz durumlarda yararlıdır:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Dizilerle, bir özel veri türü gibi bildirdiğiniz `string`, `int`, veya `DateTime`. Bir dizi değişkeni içerebilir, köşeli bildirimine eklemek belirtmek için (gibi `string[]` veya `int[]`). Öğeleri konumlarını (dizin) kullanarak bir dizi veya kullanarak erişebilirsiniz `foreach` deyimi. Dizinin sıfır tabanlı olan &#8212;dizinler; diğer bir deyişle, ilk öğe adresindeki konumlandırın 0, ikinci öğesi konum 1 ve benzeri.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Alarak dizideki öğelerin sayısını belirleyebilirsiniz kendi `Length` özelliği. Belirli bir öğenin (array aramak için) dizideki konumunu almak için `Array.IndexOf` yöntemi. Bir dizinin içeriğini da ters gibi şeyler yapabilirsiniz ( `Array.Reverse` yöntemi) veya içeriği sıralama ( `Array.Sort` yöntemi).

Bir tarayıcıda görüntülenen dize dizisi kodu çıktı:

![Razor Img13](introducing-razor-syntax-c/_static/image13.jpg)

Bir anahtar (veya ad) karşılık gelen değeri ayarlayamaz veya Burada sağladığınız anahtar/değer çiftleri koleksiyonu dictionary'si:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Bir sözlük oluşturmak için kullandığınız `new` yeni bir sözlük nesnesi oluşturduğunuzu göstermek için anahtar sözcüğü. Bir değişken kullanarak bir sözlük atayabilirsiniz `var` anahtar sözcüğü. Köşeli kullanarak sözlükteki öğe veri türlerini gösterir ( `< >` ). Bu yeni bir sözlük oluşturur yöntemi gerçekte olduğundan bildirimi sonunda parantez çifti eklemeniz gerekir.

Sözlüğe öğeler eklemek için çağırabilirsiniz `Add` sözlük değişkenin yöntemi (`myScores` bu durumda) ve ardından bir anahtar ve bir değer belirtin. Alternatif olarak, anahtarı belirtmek ve aşağıdaki örnekteki gibi basit bir atama yapmak için köşeli ayraç kullanın:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Sözlükten bir değer almak için köşeli anahtarı belirtin:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Parametrelerle yöntemleri çağırma

Bu makalenin önceki bölümlerinde okuma gibi ile program nesneleri yöntemleri olabilir. Örneğin, bir `Database` nesnesi olabilir bir `Database.Connect` yöntemi. Birçok yöntem de bir veya daha fazla parametre vardır. A *parametresi* bir yönteme geçirin bir değer, görevini tamamlamak üzere yöntemini etkinleştirmek için. Örneğin, bir bildirimi bakın `Request.MapPath` üç parametre alır yöntemi:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(Satır daha okunabilir yapmak için sarmalanmış. İç dizeleri dışında yerleştirin neredeyse tüm satır sonlarını koyabilirsiniz unutmayın tırnak işaretleri içine.)

Bu yöntem, belirtilen sanal yolu sunucuda karşılık gelen fiziksel yolu döndürür. Yöntem için üç parametreleri `virtualPath`, `baseVirtualDir`, ve `allowCrossAppMapping`. (Kabul verileri veri türleriyle parametreleri bildiriminde listelenen dikkat edin.) Bu yöntemi çağırdığınızda, tüm üç parametreleri için değerler girmeniz gerekir.

Razor sözdizimi için bir yöntem parametreleri geçirme için iki seçenek sunar: *konumsal parametreler* ve *adlandırılmış parametreleri*. Konumsal parametreler kullanarak bir yöntemi çağırmak için yöntem bildiriminde belirtilen katı bir sırada parametreleri geçirin. (Genellikle bu sırada yöntemi belgelerine okuyarak bilmeniz.) Sırayı izler ve parametrelerinden herhangi biri &#8212;atlayamazsınız; Gerekirse, boş bir dize geçirdiğiniz (`""`) veya `null` için bir değer yoksa konumsal bir parametre için.

Aşağıdaki örnek adlı bir klasör olduğunu varsayar *betikleri* Web sitenizde. Kod çağrıları `Request.MapPath` üç parametre doğru sırada yöntemi ve geçişleri değerlerini. Ardından, elde edilen eşlenen yolun görüntüler.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Bir yöntem birçok parametreleri sahip olduğunda, kodunuzu daha okunabilir adlandırılmış parametreler kullanılarak kullanmaya devam edebilir. Adlandırılmış parametreler kullanılarak bir yöntemi çağırmak için iki nokta üst üste (:), ardından değeri tarafından ardından parametre adı belirtin. Adlandırılmış parametreler avantajı, bunları istediğiniz herhangi bir sırada geçirebilirsiniz sağlamasıdır. (Bir dezavantajı yöntem çağrısının olarak compact olmamasıdır.)

Aşağıdaki örnek yukarıdaki gibi aynı yöntemini çağırır, ancak adlı parametre değerlerini sağlamak için kullanır:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Gördüğünüz gibi parametreleri farklı bir sırayla geçirilir. Ancak, önceki örnekte ve bu örnek çalıştırırsanız, aynı değere döneceksiniz.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Hataları işleme

### <a name="try-catch-statements"></a>Try-Catch deyimleri

Genellikle, denetimi dışında kalan nedeniyle başarısız olabilir, kodunuzda deyimleri sahip olacaksınız. Örneğin:

- Kodunuzu oluşturmak veya bir dosyaya erişmek çalışırsa, her türlü hataları oluşabilir. İstediğiniz dosya var olmayabilir, kilitli olabilir, kodu değil izinlere sahip ve benzeri.
- Benzer şekilde, kodunuzu veritabanındaki kayıtları güncelleştirme çalışırsa, izin sorunları olabilir, veritabanı bağlantısı bırakılabilir, kaydetmek için verileri geçersiz vb. olabilir.

Bu durumlarda programlama dilinde denir *özel durumları*. Kodunuzu bir özel durum karşılaşırsa (atar) oluşturan bir hata iletisi o, en iyi kullanıcılara sinir bozucu:

![Razor Img14](introducing-razor-syntax-c/_static/image14.jpg)

Burada kodunuzu karşılaşabileceğiniz özel durumlar durumlarda ve hata iletileri bu tür önlemek için kullanabileceğiniz `try/catch` deyimleri. İçinde `try` deyimi, denetimi kod çalıştırma. Bir veya daha `catch` deyimleri için özel konum (belirli tür özel durumlar) hatalar oluşmuş olabilir. Kadar içerebilir `catch` deyimleri yazarken gereken bekleme hataları aramak.

> [!NOTE]
> Kullanmaktan kaçının öneririz `Response.Redirect` yönteminde `try/catch` deyimleri, çünkü bu bir özel durum sayfanızda neden olabilir.


Aşağıdaki örnek ilk istek üzerinde bir metin dosyası oluşturur ve kullanıcının dosyayı açma olanak sağlayan bir düğme görüntüleyen bir sayfada görüntülenir. Örnek kasıtlı olarak hatalı dosya adı kullanır, böylece bir özel durumuna neden olur. Kod içeren `catch` deyimleri iki olası özel durumlar için: `FileNotFoundException`, dosya adı hatalı olması durumunda gerçekleşir ve `DirectoryNotFoundException`, ASP.NET bile klasörünü bulamazsanız oluşur. (, Örnek bir deyimde her şeyin doğru şekilde çalışırken, nasıl çalıştığı görmek için açıklamadan çıkarın.)

Kodunuzu özel durum işleme alamadık, önceki ekran görüntüsü gibi hata sayfasını görürsünüz. Ancak, `try/catch` bölüm, kullanıcı bu tür hataların görmemesi yardımcı olur.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

**Visual Basic ile programlama**


[Ek: Visual Basic dili ve sözdizimi](https://go.microsoft.com/fwlink/?LinkId=202908)


**Başvuru belgeleri**


[ASP.NET](https://msdn.microsoft.com/en-us/library/ee532866.aspx)

[C# dili](https://msdn.microsoft.com/en-us/library/kx37x362.aspx)
