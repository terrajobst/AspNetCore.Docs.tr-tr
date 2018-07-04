---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: ASP.NET Web programlama Razor söz dizimini (C#) kullanarak giriş | Microsoft Docs
author: tfitzmac
description: Bu bölümde Razor sözdizimini kullanarak ASP.NET Web sayfaları ile programlamaya genel bir bakış sağlar. ASP.NET dinamik web pa çalıştırmak için Microsoft'un teknolojisidir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 6e0af63ffab5ce1a4d582cbe1e9456da20df2b64
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363409"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>ASP.NET Web programlama Razor söz dizimini (C#) kullanarak giriş
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede Razor sözdizimini kullanarak ASP.NET Web sayfaları ile programlamaya genel bir bakış sağlar. ASP.NET dinamik web sayfaları web sunucuları üzerinde çalışan için Microsoft'un teknolojisidir. C# programlama dilini kullanarak bu makaleler odaklanılmıştır.
> 
> **Öğrenecekleriniz**:
> 
> - Programlama ipuçları, Razor sözdizimini kullanan ASP.NET Web sayfaları programlama ile Başlarken üst 8.
> - Temel programlama kavramlarını ihtiyacınız olacak.
> - ASP.NET sunucusu kod ve Razor sözdizimi olan tüm hakkında.
>   
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="the-top-8-programming-tips"></a>Üst 8 programlama ipuçları

Bu bölümde kesinlikle Razor sözdizimini kullanan ASP.NET sunucusu kod yazmaya başladığınızda bilmeniz gereken birkaç ipucu listelenir.

> [!NOTE]
> Razor sözdizimi C# programlama dilini alır ve, ASP.NET Web sayfaları ile en sık kullanılan dildir. Ancak, Visual Basic dili ve her şeyi Visual Basic'te de yapabileceğinizi görün Razor söz dizimi de destekler. Ek ayrıntılar için bkz [Visual Basic Dil ve sözdizimi](https://go.microsoft.com/fwlink/?LinkId=202908).


Makalenin sonraki bölümlerinde çoğu bu programlama teknikleri hakkında daha fazla ayrıntı bulabilirsiniz.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Kod kullanarak bir sayfa ekleyin @ karakteri

`@` Karakter satır içi ifadeler, tek bir deyim blokları ve birden fazla deyim blokları başlatır:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Bir tarayıcıda sayfa çalıştığında bu deyimler göründüğünü budur:

![Razor Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **HTML kodlama**
> 
> Kullanarak bir sayfanın içeriğini görüntülediğinizde `@` karakter, önceki örneklerde olduğu gibi çıkış ASP.NET HTML olarak kodlar. Bu ayrılmış HTML karakterlerini değiştirir (gibi `<` ve `>` ve `&`) karakterlerini HTML etiketleri veya varlıklar yorumlanmasını yerine bir web sayfasında karakter olarak görüntülenecek etkinleştirme kodları ile. Sunucu kodunuz çıktısını HTML kodlaması olmadan düzgün görüntülenmeyebilir ve bir sayfa güvenlik risklerine maruz.
> 
> Amacınız etiketlerini işler biçimlendirmesi olarak HTML biçimlendirmesi çıkış olup olmadığını (örneğin `<p></p>` paragrafı veya `<em></em>` metni vurgulamak için), bölümüne bakın [metin birleştirme, işaretleme ve kod blokları içinde kod](#BM_CombiningTextMarkupAndCode) bu makalenin ilerleyen bölümlerinde.
> 
> Daha fazla bilgi edinebilirsiniz, HTML kodlaması hakkında [formlarla çalışma](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Kod blokları ayraçlarının içine alın

A *kod bloğu* bir veya daha fazla kod deyimlerini içerir ve parantez içine alınır.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. Bir blok içinde her kod deyimi noktalı virgül ile bitmelidir

Bir kod bloğunun içine her tam kod deyimi noktalı virgül ile bitmelidir. Satır içi ifadeler, noktalı virgül ile sona ermez.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Değerleri depolamak için değişkenleri kullanma

Değerleri depolayabilir bir *değişkeni*dizeler, sayılar ve tarihler, vb. dahil olmak üzere. Bir değişken kullanarak yeni oluşturduğunuz `var` anahtar sözcüğü. Değişken değerleri doğrudan bir sayfasını kullanarak içinde ekleyebilirsiniz `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Değişmez değer dize değerleri çift tırnak içine alın

A *dize* metin olarak kabul edilir karakterden oluşan bir dizidir. Bir dizeyi belirtmek için çift tırnak içine alın:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Görüntülemek istediğiniz dize bir ters eğik çizgi karakteri içeriyorsa ( `\` ) veya çift tırnak işareti ( `"` ), kullanan bir *verbatim dize sabit değeri* ile önek `@` işleci. (C# ' ta, \ karakterin verbatim dize sabit değeri kullanmadığınız sürece özel bir anlamı vardır.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Çift tırnak işaretleri eklemek için verbatim dize sabit değeri kullanın ve tırnak işaretleri yineleyin:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Bu örneklerin her ikisi bir sayfasını kullanarak sonucu şu şekildedir:

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Dikkat `@` karakter, hem C# ' deki verbatim dizesi değişmez değerleri işaretlemek için hem de ASP.NET sayfaları kodda işaretlemek için kullanılır.


### <a name="6-code-is-case-sensitive"></a>6. Kodu büyük küçük harfe duyarlı.

C# anahtar sözcükleri (gibi `var`, `true`, ve `if`) ve değişken adları büyük küçük harfe duyarlıdır. Aşağıdaki kod satırlarını iki farklı değişkenleri oluşturma `lastName` ve `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Bir değişken olarak bildirirseniz `var lastName = "Smith";` ve sayfanız olarak bu değişkene başvurmak çalışırsanız `@LastName`, bir hata nedeniyle oluşur `LastName` tanınmaz.

> [!NOTE]
> Visual Basic anahtar sözcükleri ve değişkenleri olan *değil* büyük küçük harfe duyarlı.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Kodlamanızı çoğunu nesneleri içerir

Bir *nesne* ile programlayabileceğiniz bir şeyi temsil &#8212; bir sayfa, bir metin kutusu, bir dosya, görüntü, bir web isteği, bir e-posta iletisi, bir müşteri kaydı (veritabanı satır) vb. Nesnelerin özelliklerini tanımlayan özelliği vardır ve, okuma veya değiştirebilirsiniz, &#8212; bir metin kutusu nesnesine sahip bir `Text` özelliği (diğerlerinin arasında) bir istek nesnesi olan bir `Url` özelliğine sahip bir e-posta iletisi bir `From` özelliği ve bir müşteri nesnesi bir `FirstName` özelliği. Nesneleri yöntemlerle de &quot;fiilleri&quot; yerine getirebilirsiniz. Örnekler, bir dosya nesnesinin `Save` yöntemi, bir görüntü nesnenin `Rotate` yöntemi ve bir e-posta nesnenin `Send` yöntemi.

Genellikle ile çalışacaksınız `Request` , metin kutuları (form alanları) değerleri gibi bilgileri verir ne tür bir tarayıcı, sayfa, kullanıcı kimliği, vb. URL'sini istekte sayfasında, nesne. Aşağıdaki örnek özelliklerine erişmek nasıl gösterir `Request` nesne ve nasıl çağrılacağını `MapPath` yöntemi `Request` sayfasının mutlak yolu sunucu üzerinde size nesnesi:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Kararları kod yazabilirsiniz.

Bir anahtar dinamik web sayfaları ne koşullara göre yapılacağını belirlemek özelliğidir. Bunu yapmak için en yaygın yolu `if` deyimi (ve isteğe bağlı `else` deyimi).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

Deyim `if(IsPost)` yazma toplu yoludur `if(IsPost == true)`. İle birlikte `if` deyimleri, bir yineleme kod bloklarını koşulları test için çeşitli yollar vardır ve bu şekilde, bu makalenin sonraki bölümlerinde açıklanmıştır.

Bir tarayıcıda görüntülenen sonuç (tıkladıktan sonra **Gönder**):

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET ve POST yöntemleri ve IsPost özelliği
> 
> Web sayfaları (HTTP) için kullanılan protokol çok sınırlı sayıda sunucuya isteğinde bulunmak için kullanılan yöntemleri (fiilleri) destekler. İki en yaygın bir okumak için kullanılan GET ve POST, bir sayfa göndermek için kullanılan olanlardır. Genel olarak, ilk kez bir kullanıcı bir sayfa istediğinde, GET kullanarak sayfa istenmektedir. Kullanıcı, bir formda doldurur ve sonra Gönder düğmesine tıkladığında tarayıcı bu sunucuya bir POST isteği yapar.
> 
> Web programlama, böylece sayfa işleme bildiğiniz bir sayfaya bir GET veya POST olarak istenen olup olmadığını bilmek yararlıdır. ASP.NET Web sayfaları'nda kullanabileceğiniz `IsPost` bir GET veya POST isteği olup olmadığını görmek için özellik. Bir POST isteğiyse `IsPost` özelliği true döndürür ve bir form üzerinde metin kutuları değerlerini okuma gibi şeyler yapabilirsiniz. Birçok örnekler göreceksiniz sayfanın değerine bağlı olarak farklı şekilde işlemek nasıl Göster `IsPost`.


## <a name="a-simple-code-example"></a>Basit bir kod örneğidir

Bu yordam, temel programlama tekniklerini gösteren bir sayfa oluşturma işlemini gösterir. Örnekte, kullanıcılar bunları ekler ve sonucu görüntüler iki sayıyı girin sağlayan bir sayfa oluşturun.

1. Düzenleyicinizde, yeni bir dosya oluşturun ve adlandırın *AddNumbers.cshtml*.
2. Aşağıdaki kodu ve biçimlendirmeyi sayfasındaki herhangi bir şey değiştirerek sayfasına kopyalayın.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Not yapabileceğiniz bazı işlemler aşağıda verilmiştir:

    - `@` Karakter sayfasında ilk kod bloğunu başlar ve kendisinden `totalMessage` sayfanın altına katıştırılmış değişkeni.
    - Sayfanın üst kısmındaki blok parantez içine alınır.
    - Üst blok tüm satırları noktalı virgül ile bitmelidir.
    - Değişkenleri `total`, `num1`, `num2`, ve `totalMessage` birkaç sayı ve bir dize depolar.
    - Atanan değişmez dize değeri `totalMessage` çift tırnak işareti bulunan bir değişkendir.
    - Kod zaman büyük/küçük harfe, olduğundan `totalMessage` değişkeni sayfanın altına kullanılıyor, üst değişkeni adını tam olarak eşleşmelidir.
    - İfade `num1.AsInt() + num2.AsInt()` nesneleri ve yöntemleri ile çalışma hakkında bilgi verilmektedir. `AsInt` Yöntemi her bir değişken üzerinde aritmetik işlemleri, bir sayı (tamsayı) bir kullanıcı tarafından girilen bir dize dönüştürür.
    - `<form>` Etiket içeren bir `method="post"` özniteliği. Kullanıcı tıkladığında bu belirten **Ekle**, sayfanın HTTP POST yöntemini kullanarak sunucuya gönderilir. Sayfa gönderildiğinde `if(IsPost)` test değerlendirilen koşul true ile kod çalıştırır, sayıları eklemenin sonucunu görüntüleme.
3. Sayfayı kaydedin ve bir tarayıcıda çalıştırın. (Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.) İki tam sayılar girin ve ardından **Ekle** düğmesi. 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Temel programlama kavramları

Bu makalede, ASP.NET web programlama bir bakış sunulmaktadır. Büyük kapsamlı bir incelemesini, en sık kullanacağınız programlama kavramları ile yalnızca hızlı bir tura değildir. Bu halde bile hemen ASP.NET Web sayfaları ile kullanmaya başlamak için ihtiyacınız olacak her şeyi kapsar.

Ancak ilk olarak az teknik arka planı.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Razor sözdizimi, sunucu kodu ve ASP.NET

Razor kod sunucu tabanlı bir web sayfasına eklemek için basit bir programlama sözdizimi sözdizimidir. Razor sözdizimini kullanan bir web sayfası içeriği iki tür vardır: istemci içeriği ve sunucu kodu. İstemci içeriktir için kullandığınız web sayfaları'nda Hizmetler: HTML biçimlendirmesi (öğeleri) ve stil CSS gibi bilgileri belki de JavaScript ve düz metin gibi bazı istemci komut dosyası.

Razor sözdizimi, bu istemci içeriği için sunucu kodu eklemenizi sağlar. Tarayıcıya sayfanın göndermeden önce sunucu, kod sayfasında sunucu kodu varsa, ilk olarak çalıştırır. Sunucu üzerinde çalıştırarak kodu istemci içeriği sunucu tabanlı veritabanlarına erişme gibi tek başına kullanarak yapmak için çok daha karmaşık görevleri gerçekleştirebilirsiniz. En önemlisi, sunucu kodu istemci içeriği dinamik olarak oluşturabileceğiniz &#8212; HTML biçimlendirmeyi ya da diğer içeriğini hareket halindeyken oluşturmak ve tarayıcı sayfası içerebilecek herhangi bir statik HTML birlikte gönderin. Tarayıcının açısından bakıldığında, sunucu kodunuz tarafından oluşturulan istemci içeriği herhangi bir istemci içerik farklı değildir. Önceden gördüğünüz gibi gerekli sunucu kodu oldukça basittir.

Razor sözdizimi içeren ASP.NET web sayfaları, bir özel dosya uzantısına sahip (*.cshtml* veya *.vbhtml*). Sunucu, bu uzantıları tanır, Razor sözdizimi ile işaretlenir ve sonra sayfa tarayıcıya gönderir kodunu çalıştırır.

### <a name="where-does-aspnet-fit-in"></a>ASP.NET nerelerde uygundur?

Razor sözdizimi, bir Microsoft .NET Framework üzerinde sırayla dayalı olarak ASP.NET adlı Microsoft teknolojisini temel alır. Neredeyse her türde bilgisayar uygulaması geliştirmek için büyük ve kapsamlı bir programlama çerçevesi Microsoft.NET Framework var. ASP.NET web uygulamaları oluşturmak için özel olarak tasarlanmıştır .NET Framework'ün parçasıdır. Geliştiriciler, dünyanın en büyük ve en yüksek trafikli Web sitelerinin çoğu oluşturmak için ASP.NET kullandınız. (Her zaman dosya adı uzantısı görürsünüz *.aspx* URL bir sitedeki bir parçası olarak site ASP.NET kullanılarak yazılmış olduğundan anlarsınız.)

Razor sözdizimi, ASP.NET, ancak daha kolay, başlangıç ve daha üretken getiren Uzman olup olmadığınızı öğrenmek basitleştirilmiş bir söz dizimi kullanarak tüm güç sağlar. Bu sözdizimi kullanımı basit olsa da, ASP.NET ve .NET Framework ailesi ilişkisini sitelerinizi daha karmaşık bir HAL gibi kullanabileceğiniz daha büyük çerçeveleri gücünü olduğu anlamına gelir.

![Razor Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Sınıfları ve örnekleri**
> 
> ASP.NET sunucusu kod sınıflarının fikrini sırayla oluşturulan nesneleri kullanır. Tanımı veya bir nesne için şablon sınıfıdır. Örneğin, bir uygulama içerebilir bir `Customer` gereken herhangi bir müşteri nesnesi yöntemleri ve özellikleri tanımlayan sınıf.
> 
> Uygulamayı gerçek müşteri bilgileri ile çalışmak gerektiğinde örneği oluşturur (veya *başlatır*) bir müşteri nesnesi. Her bir müşteriye, ayrı bir örneğidir `Customer` sınıfı. Her örnek aynı özellikleri ve yöntemleri destekler, ancak her müşteri nesnesi benzersiz olduğundan her örneği için özellik değerlerini genellikle farklı. Bir müşteri nesnesi `LastName` özelliği, "Smith" olabilir; başka bir müşteri nesnesi `LastName` özelliği, "Jones." olabilir
> 
> Benzer şekilde, herhangi tek tek web sitenizde sayfasıdır bir `Page` örneği nesnesini `Page` sınıfı. Bir düğme sayfasında bir `Button` örneği nesnesini `Button` sınıfı ve benzeri. Her örnek kendi özellikleri vardır, ancak bunların tümü nesnenin sınıf tanımında belirtilen üzerinde temel alır.


## <a name="basic-syntax"></a>Temel söz dizimi

Daha önce bir ASP.NET Web Pages sayfasında nasıl oluşturulacağını ve sunucu kodunu HTML biçimlendirmesi nasıl ekleyebileceğinizi basit bir örneği gördünüz. Burada, Razor sözdizimini kullanan ASP.NET sunucusu kod yazmaya ilişkin temel bilgileri öğreneceksiniz &#8212; diğer bir deyişle, programlama dili kuralları.

(Özellikle, C, C++, C#, Visual Basic veya JavaScript kullandıysanız) programlama deneyimli değilseniz ne burada okuma çoğunu tanıdık gelecektir. Yalnızca nasıl sunucu kodu işaretlemede eklenir ile kendinizi alıştırın gerekecektir *.cshtml* dosyaları.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Metni İşaretleme ve kod blokları içinde kod birleştirme

Sunucu kod bloklarında genellikle çıkış metin biçimlendirme (veya her) sayfasına istersiniz. Sunucusu kod bloğu kod değil ve, bunun yerine olarak işleneceğini metin içeriyorsa, ASP.NET koddan metni ayırt etmek mümkün olması gerekir. Bunu yapmanın birkaç yolu vardır.

- Gibi bir HTML öğesindeki metni alın `<p></p>` veya `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    HTML öğesi, metin ve ek HTML öğelerinin sunucu kodu ifadeleri içerebilir. ASP.NET, HTML etiketi açılışı gördüğünde (örneğin, `<p>`), öğenin dahil her şeyi işler ve içeriği olarak olduğu sunucu kodu ifadeler çözümleme tarayıcıya olan.
- Kullanım `@:` işleci veya `<text>` öğesi. `@:` Tek satırlık bir düz metin veya HTML etiketleri eşleşmeyen; içeren içerik çıkarır `<text>` öğesi çıktısını almak için birden fazla satır alır. Bu seçenekler, çıkış bir parçası olarak bir HTML öğesini işlemek istemediğiniz zaman yararlıdır.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Çok satırlı metin veya HTML etiketleri eşleşmeyen çıkışı istiyorsanız, her satırın koyabilirsiniz `@:`, veya satır içine alın bir `<text>` öğesi. Gibi `@:` işleci`<text>` etiketleri metin içeriği tanımlamak için ASP.NET tarafından kullanılan ve sayfa çıktıda hiçbir zaman işlenir.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    İlk örnek, önceki örnekte yineler ancak tek bir çift kullanır `<text>` metin işlemek için etiketler. İkinci örnekte, `<text>` ve `</text>` etiketlerini bazı uncontained metin ve eşleşmeyen HTML etiketleri olması her biri üç satır içine (`<br />`), yanı sıra sunucu kodu ve eşleşen HTML etiketleri. Yine de her satırın tek tek önünde `@:` işleci; her iki şekilde çalışır.

    > [!NOTE]
    > Ne zaman, çıktı metin bu bölümde gösterildiği &#8212; bir HTML öğesini kullanarak `@:` işleci veya `<text>` öğesi &#8212; ASP.NET olmayan HTML olarak kodlanacak çıktı. (Daha önce belirtildiği gibi ASP.NET sunucusu kod ifadeleri tarafından öncelenen sunucusu kod bloğu ve çıkış kodlama `@`, bu bölümde belirtilen özel durumlar hariç.)

### <a name="whitespace"></a>Boşluk

Fazladan boşlukları deyimindeki (ve bir dize sabit değeri dışındaki) deyim etkilemez:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Satır sonu deyimindeki ifade üzerinde etkiye sahip değildir ve Okunabilirlik için deyimleri kayabilir. Aşağıdaki deyimler aynıdır:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Ancak, bir dize sabit değeri ortasında bir satır kaydırma olamaz. Aşağıdaki örnek çalışmaz:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Yukarıdaki kodu gibi çoklu satırlara sarar uzun bir dize birleştirmek için iki seçenek vardır. Birleştirme işlecini kullanabilirsiniz (`+`), bu makalenin sonraki bölümlerinde görürsünüz. Ayrıca `@` bu makalenin önceki bölümlerinde anlatıldığı gibi aynen bir dize değişmez değeri oluşturmak için kullanılan karakter. Satırlara verbatim dize değişmez değerleri bölünebilir:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Kod (ve biçimlendirme) açıklamaları

Açıklama notları kendiniz veya başkaları için terk etmenize imkan. Devre dışı bırakmak de sağlar (*açıklama*) bir bölümünü başladığınız kodun veya çalıştırmak istemiyorsanız, ancak şimdilik sayfanızın tutmak istiyor.

Farklı söz dizimi HTML biçimlendirmesi ve Razor kod için açıklama yoktur. Tüm Razor kod gibi Razor açıklama işlenen (ve sonra kaldırılır) sayfa tarayıcıya gönderilmeden önce sunucuda. Bu nedenle, Razor açıklama ekleme söz dizimi, kod (veya hatta işaretleme) dosyasını düzenleyin, ancak kullanıcıların gördüğü yoksa bile sayfa kaynağında gördüğünüz açıklamaları put olanak tanır.

ASP.NET Razor yorumlar için açıklamaya başlamadan `@*` ve ile bitmelidir `*@`. Bir satır veya çok satırlı açıklama olabilir:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Bir kod bloğu içinde bir açıklama aşağıda verilmiştir:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Böylece çalışmaz burada aynı kod bloğu kod satırı ile devre dışı bırakılmışsa:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

Bir kod bloğunun içine Razor açıklama sözdizimi kullanılarak alternatif olarak, C# gibi kullandığınız programlama diline yorum söz dizimini kullanabilirsiniz:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

C# dilinde tek satırlı yorumlar tarafından öncelenen `//` karakter ve çok satırlı yorumlar ile başlayan `/*` ve ile sona erdi `*/`. (Razor yorumlarla gibi C# açıklamaları tarayıcıya işlenmez.)

Bildiğiniz gibi biçimlendirme için bir HTML açıklaması oluşturabilirsiniz:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

HTML Yorumlarını başlayarak `<!--` karakterleri ve sonu `-->`. HTML Yorumlarını yalnızca metin aynı sayfada saklamak isteyebilirsiniz ancak işlemek istemiyorsanız da herhangi bir HTML biçimlendirmesi kapsamak için kullanabilirsiniz. Bu HTML açıklama etiketleri ve içerdikleri metin tüm içeriği gizler:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Razor açıklama, HTML yorumlarında aksine *olan* sayfasına işlenen ve kullanıcı bunları sayfa kaynağı görüntüleyerek görebilirsiniz.

Razor iç içe geçmiş bloklarını C# üzerinde sınırlamalar uygulanır. Daha fazla bilgi için [adlı C# değişkenleri ve iç içe geçmiş blokları bozuk kod oluştur](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Değişkenler

Bir değişken verilerini depolamak için kullandığınız adlandırılmış bir nesnedir. Herhangi bir şey değişkenleri ad verebilirsiniz ancak adı, alfabetik bir karakter ile başlamalı ve boşluk ya da ayrılmış karakterler içeremez.

### <a name="variables-and-data-types"></a>Değişkenleri ve veri türleri

Ne tür veriler bir değişkende depolandığını belirtir bir özel veri türü bir değişken olabilir. Dize değerleri saklayan string değişkeni olabilir (gibi &quot;Merhaba Dünya&quot;), tam sayı değerleri (örneğin, 3 veya 79) depolayan tamsayı değişkenleri ve çeşitli biçimlerde (4/12/2012 veya 2009 yılı Mart gibi tarih değerlerini depolar tarih değişkenleri ). Ve kullanabileceğiniz diğer birçok veri türleri vardır.

Ancak, genellikle bir değişken için bir tür belirtmeniz gerekmez. Çoğu zaman, ASP.NET, out değişkeni içinde verilerin nasıl kullanıldığı temel tür anlayabilir. (Bazen bir türünü belirtmeniz gerekir; bunun true olduğu örnekler göreceksiniz.)

Bir değişken kullanarak bildirdiğiniz `var` (bir tür belirtmek istemiyorsanız) anahtar sözcüğü veya türün adını kullanarak:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

Aşağıdaki örnek, bir web sayfasındaki bazı tipik kullanımları değişkenlerin gösterir:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Önceki örneklerde bir sayfa birleştirirseniz, bu bir tarayıcıda görüntülenen bakın:

![Razor Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Dönüştürme ve veri türleri test etme

ASP.NET genellikle bir veri türü otomatik olarak belirleyebilir, ancak bazen bunu dönüştürülemez. Bu nedenle, ASP.NET açık bir dönüştürme gerçekleştirerek yardımcı olacak birçok gerekebilir. Bazen türleri dönüştürmek yoksa bile, ne tür verilere, ile çalışabilir engellemediğini test etmek için yardımcı olur.

En yaygın bir tamsayı veya tarih gibi bir dizeyi başka bir türe dönüştürmek sahip olduğu. Aşağıdaki örnek, bir sayıyı bir dize burada dönüştürmeniz gerekir tipik bir servis talebi gösterir.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Bir kural olarak, kullanıcı girişini dize olarak size gelir. Bir sayı girin kullanıcılardan ve kullanıcı girişi gönderilir ve kodda okuma zaman bir basamak girdikten bile verileri dize biçiminde bile. Bu nedenle, dizeyi sayıya dönüştürme gerekir. Dönüştürmeden değerleri üzerinde aritmetik işlemleri denerseniz, iki dizeyi ASP.NET eklenemiyor çünkü örnekte, şu hata, sonuçları:

*Örtük olarak 'int', ' string' türü dönüştürülemiyor.*

Tam sayılar değerlerini dönüştürmek için çağrı `AsInt` yöntemi. Dönüştürme başarılı olursa, sayıları ekleyebilirsiniz.

Aşağıdaki tablo bazı yaygın dönüştürme ve test yöntemleri değişkenleri listeler.

::: satır:::::: sütun::: <strong>yöntemi</strong> ::: sütun uç:::::: sütun::: <strong>açıklama</strong> ::: sütun uç:::::: sütun::: <strong>örnek</strong> ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `AsInt(), IsInt()` ::: sütun uç:::::: sütun::: ("593" gibi) bir tam sayı bir tamsayı olarak temsil eden bir dize dönüştürür.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `AsBool(), IsBool()` ::: sütun uç:::::: sütun::: gibi bir dize dönüştürür &quot;true&quot; veya &quot;false&quot; Boole türü.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `AsFloat(), IsFloat()` ::: sütun uç:::::: sütun::: gibi ondalık bir değeri içeren bir dize dönüştürür &quot;1.3&quot; veya &quot;7.439&quot; bir kayan noktalı sayı.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `AsDecimal(), IsDecimal()` ::: sütun uç:::::: sütun::: gibi ondalık bir değeri içeren bir dize dönüştürür &quot;1.3&quot; veya &quot;7.439&quot; ondalık bir sayı. (ASP.NET, bir ondalık kayan noktalı sayıdan daha kesin sayıdır.) ::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `AsDateTime(), IsDateTime()` ::: sütun uç:::::: sütun::: ASP.NET için bir tarih ve saat değerini temsil eden bir dize dönüştürür `DateTime` türü.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `ToString()` ::: sütun uç:::::: sütun::: herhangi bir veri türü bir dizeye dönüştürür.
::: sütun uç:::::: sütun::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    ::: sütun uç:::::: satır sonuna:::

## <a name="operators"></a>İşleçler

Bir anahtar sözcük veya ne tür bir ifadede gerçekleştirilecek komut ASP karakter işlecidir. C# dili (ve bunu temel alan bir Razor sözdizimi) birçok işleçleri destekler, ancak yalnızca kullanmaya başlamak için birkaç tanıması gerekir. En yaygın işleçleri aşağıdaki tabloda özetlenmiştir.


::: satır:::::: sütun::: <strong>işleci</strong> ::: sütun uç:::::: sütun::: <strong>açıklama</strong> ::: sütun uç:::::: sütun::: <strong>örnekler</strong> ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `+` `-` `*` `/` ::: sütun uç:::::: sütun::: sayısal ifadeler kullanılan matematik işleçleri.
::: sütun uç:::::: sütun::: [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `=` ::: sütun uç:::::: sütun::: atama. Deyiminin sağ taraftaki değer sol tarafındaki nesnesi atar.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `==` ::: sütun uç:::::: sütun::: eşitlik. Döndürür `true` değerler eşitse. (Birbirinden dikkat edin `=` işleci ve `==` işleci)::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `!=` ::: sütun uç:::::: sütun::: eşitsizlik. Döndürür `true` değerler eşit değilse.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `< > <= >=` ::: sütun uç:::::: sütun::: daha az-daha fazla-daha az daha-veya-eşittir ve daha fazla-veya-eşittir büyüktür.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `+` ::: sütun uç:::::: sütun::: dizeleri birleştirmek için kullanılan birleştirme. ASP.NET bu operatörü ve ifade veri türüne göre toplama işleci arasındaki farkı bilir.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `+=` `-=` ::: sütun uç:::::: sütun::: ekleyin ve 1 (sırasıyla) bir değişkenden gelen çıkarma artırma ve azaltma işleçleri.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `.` ::: sütun uç:::::: sütun::: nokta. Nesneleri ve özellikleri ve yöntemleri ayırt etmek için kullanılır.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `()` ::: sütun uç:::::: sütun::: parantez. Grup ifadeleri ve yönteme parametreleri geçirmek için kullanılır.
::: sütun uç:::::: sütun::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `[]` ::: sütun uç:::::: sütun::: köşeli ayraç. Değer dizileri veya koleksiyonlardaki erişmek için kullanılır.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `!` ::: sütun uç:::::: sütun::: değil. Tersine çevirir bir `true` değerini `false` ve bunun tersi de geçerlidir. Test etmek için bir toplu şekilde genellikle kullanılan `false` (diğer bir deyişle, için değil `true`).
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    ::: sütun uç:::::: satır sonuna:::
* * *
::: satır:::::: sütun::: `&&` <code>&#124;&#124;</code> ::: sütun uç:::::: sütun::: mantıksal ve ve OR koşulları birlikte hangi bağlamak için kullanılır.
::: sütun uç:::::: sütun::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    ::: sütun uç:::::: satır sonuna:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Dosya ve klasör yollarında kod ile çalışma

Genellikle, kodunuzda dosya ve klasör yolları ile çalışırsınız. Geliştirme bilgisayarınızda görünebilir gibi bir Web sitesi için fiziksel klasör yapısı örneği aşağıdadır:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

URL'leri ve yolları hakkında bazı önemli ayrıntılar aşağıda verilmiştir:

- Bir URL ile birlikte bir etki alanı adı başlar (`http://www.example.com`) veya bir sunucu adı (`http://localhost`, `http://mycomputer`).
- Bir URL bir ana bilgisayarın fiziksel yola karşılık gelir. Örneğin, `http://myserver` klasörüne karşılık gelebilir *C:\websites\mywebsite* sunucusunda.
- Sanal yol tam yolunu belirtmeniz gerek kalmadan kod yollarında temsil etmek için toplu özelliktir. Bu, etki alanı veya sunucu adı bir URL bölümünü içerir. Sanal yol kullandığınızda, yolları güncelleştirmek zorunda kalmadan farklı bir etki alanı ya da sunucu kodunuzu taşıyabilirsiniz.

Farklar anlamanıza yardımcı olması için bir örnek aşağıda verilmiştir:

| Tam URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Sunucu adı | *mycompanyserver* |
| Sanal yol | */HumanResources/CompanyPolicy.htm* |
| Fiziksel yol | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Sanal kök / yalnızca C: aynı kök gibi sürücüdür \. (Sanal klasör yolları her zaman eğik çizgilerin kullanılması.) Sanal yolun bir klasörün, fiziksel klasör olarak aynı ada sahip gerekmez; Bu, bir diğer ad olabilir. (Üretim sunucularında sanal yolu nadiren tam bir fiziksel yol ile eşleşir.)

Bazen kod bulunan dosya ve klasörleri ile çalışırken, fiziksel yolunu ve bazen çalıştığınız hangi nesneleri bağlı olarak bir sanal yola başvurusu gerekir. ASP.NET, size bu araçları dosya ve klasör yolları kod ile çalışmak için: `Server.MapPath` yöntemi ve `~` işleci ve `Href` yöntemi.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Sanal-fiziksel yolları dönüştürme: Server.MapPath yöntemi

`Server.MapPath` Yöntemi, bir sanal yol dönüştürür (gibi */default.cshtml*) mutlak bir fiziksel yola (gibi *C:\WebSites\MyWebSiteFolder\default.cshtml*). Tam fiziksel yolunu ihtiyacınız dilediğiniz zaman bu yöntemi kullanın. Okunurken ya da bir metin dosyası veya web sunucusunda görüntü dosyası yazma tipik bir örnektir.

Genellikle sitenizi barındırma site sunucuda mutlak fiziksel yolu bilmiyorsanız, bu yöntem, yolun dönüştürebilirsiniz şekilde bildiğiniz — sanal yolu — sunucusu üzerinde karşılık gelen yolu. Sanal yol bir dosya veya klasöre yöntemine geçirmeniz ve fiziksel yolu döndürür:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Sanal kök başvuran: ~ işleci ve Href yöntemi

İçinde bir *.cshtml* veya *.vbhtml* dosyası, kök sanal yolu kullanarak başvurabilirsiniz `~` işleci. Bu sayfaları bir siteye taşıyabilirsiniz ve içerdikleri diğer sayfalara giden bağlantıları kopmuş olmayacak çünkü çok kullanışlıdır. Ayrıca, hiç olmadığı kadar Web sitenizi farklı bir konuma taşımanız durumunda kullanışlıdır. Bazı örnekler şunlardır:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Web sitesi ise `http://myserver/myapp`, işte sayfa çalıştığında ASP.NET bu yolların nasıl değerlendirir:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Bu yolları aslında değişken değerleri olarak göremezsiniz ancak alacağı ne oldukları olan ASP.NET yolları değerlendirir.)

Kullanabileceğiniz `~` işleci hem de sunucu kodu (yukarıdaki gibi) bu gibi biçimlendirme içinde:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Biçimlendirme içinde kullandığınız `~` yollara görüntü dosyaları, diğer web sayfaları ve CSS dosyaları gibi kaynakları oluşturmak için işleç. Sayfa çalıştığında, ASP.NET (kod ve biçimlendirme) sayfası görünür ve tüm çözümler `~` başvurularını uygun yolu.

## <a name="conditional-logic-and-loops"></a>Koşullu mantık ve döngüler

ASP.NET sunucusu kod koşullara göre görevleri gerçekleştirmenize ve belirli bir sayıda yineler deyimleri kod yazma sağlar (diğer bir deyişle, bir döngü çalışan kod).

### <a name="testing-conditions"></a>Test koşulları

Kullandığınız basit bir koşulunu test etmek için `if` true veya false değerini döndürür, belirttiğiniz bir test temel deyimi:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

`if` Anahtar sözcüğü, bir blok başlatır. Gerçek testi (durum) parantez içine ve true veya false döndürür. Sınama true olduğunda çalıştırılan deyimleri ayraç içine alınır. Bir `if` deyimi içerebilir bir `else` koşul false ise çalıştırılacak deyimleri belirtir engelle:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Kullanarak birden çok koşulu ekleyebileceğiniz bir `else if` engelle:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

İlk koşul ise, bu örnekte, blok true değil `else if` koşul denetlenir. Bu koşul karşılanıyorsa deyimlerinde `else if` bloğu yürütülür. Hiçbir koşul karşılanıyorsa deyimlerinde `else` bloğu yürütülür. Eğer başka herhangi bir sayıda ekleyebilirsiniz engeller ve ile kapatın bir `else` olarak engelleme &quot;diğer her şey&quot; koşul.

Çok sayıda koşulları test etmek için bir `switch` engelle:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Test etmek için değeri parantez içinde olduğu (örnekte `weekday` değişkeni). Her bir testi kullanan bir `case` iki nokta (:) ile biten deyimi. Varsa değerini bir `case` ifadesi ile eşleşen test değeri, bu büyük/küçük harf bloğundaki kod yürütülür. Her case deyimiyle kapatmak bir `break` deyimi. (Kesme her içerecek şekilde unutursanız `case` sonraki kod bloğu `case` deyimi de çalışır.) A `switch` blok genellikle sahip bir `default` deyimi için son durum olarak bir &quot;diğer her şey&quot; diğer durumlarda hiçbiri doğruysa çalıştırılan seçeneği.

Bir tarayıcıda görüntülenen son iki koşullu blokları sonucu:

![Razor Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Döngü kod

Genellikle aynı deyimleri tekrar tekrar çalıştırmak gerekir. Bunun için döngü. Örneğin, genellikle her öğe için aynı deyimleri verilerinin bir koleksiyonunu çalıştırın. Tam döngü istediğiniz kaç kez biliyorsanız, kullanabileceğiniz bir `for` döngü. Bu tür bir döngü sayımı veya sayım için kullanışlıdır:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Döngü ile başlayan `for` anahtar sözcüğü, parantez içindeki üç deyimi sonrasında, her sonlandırıldı noktalı virgül ile.

- İlk deyim parantez içine (`var i=10;`) sayaç oluşturur ve 10'a başlatır. Sayaç adı gerekmez `i` &#8212; herhangi bir değişken kullanabilirsiniz. Zaman `for` döngüsü çalıştırıldığında, sayaç otomatik olarak artırılır.
- İkinci deyim (`i < 21;`) ne kadar saymak istediğiniz bir koşul ayarlar. Bu durumda, en fazla 20 gitmek istediğiniz (sayaç 21'den az olsa da diğer bir deyişle, devam edin).
- Üçüncü deyim (`i++` ) yalnızca sayaç 1 döngü her çalıştığında olarak eklenmiş olması gerektiğini belirtir. bir artış işleci kullanır.

Ayraçlar içinde döngü her yinelemede çalıştıracak kodudur. Yeni bir paragraf biçimlendirme oluşturur (`<p>` öğesi) her zaman ve çıktıyı görüntüleme değeri, bir satır ekler `i` (sayacı). Bu sayfa çalıştırdığınızda, örnek çıktı, her satırda öğe sayısını gösteren bir metin ile görüntüleme 11 satırları oluşturur.

![Razor Img11](introducing-razor-syntax-c/_static/image11.jpg)

Bir koleksiyon veya dizi ile çalışıyorsanız, sık kullandığınız bir `foreach` döngü. Bir koleksiyon benzer nesnelerin grubudur ve `foreach` döngü, gerçekleştirdiğiniz koleksiyondaki her öğe üzerinde bir görev sağlar. Bu tür bir döngü koleksiyonlar için uygundur çünkü aksine bir `for` döngü yazmanız gerekmez sayaç artırın veya bir sınır belirleyin. Bunun yerine, `foreach` döngü kodunun işlemi tamamlanana kadar toplulukta yalnızca geçer.

Örneğin, aşağıdaki kod öğeleri döndürür `Request.ServerVariables` web sunucunuz hakkında bilgi içeren bir nesne koleksiyonu. Bunu kullanan bir `foreac` yeni bir oluşturarak her öğenin adını görüntülemek için h döngü `<li>` HTML madde işaretli liste öğesi.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

`foreach` Anahtar sözcüğü, koleksiyondaki tek bir öğeyi temsil eden bir değişkeni bildirmek burada parantezle izlendiği (örnekte `var item`) ve ardından `in` döngü koleksiyonu ardından anahtar sözcüğü. Gövdesinde `foreach` döngü, daha önce bildirilen değişken kullanarak geçerli öğeye erişebilir.

![Razor Img12](introducing-razor-syntax-c/_static/image12.jpg)

Daha genel amaçlı bir döngü oluşturmak için kullanın `while` deyimi:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A `while` döngü ile başlayan `while` parantez ile ne kadar süreyle döngünün devam belirttiğiniz anahtar sözcüğü (burada için sürece `countNum` 50'den az), ardından yinelemek için blok. Genellikle döngü Artır (Ekle) veya azaltma (Çıkart) bir değişken veya sayım için kullanılan nesne. Örnekte, `+=` işleci ekler 1 `countNum` döngünün her çalıştığında. (Geriye doğru sayan bir döngü içinde bir değişken azaltma için azaltma işleci kullanırsınız `-=`).

## <a name="objects-and-collections"></a>Nesneler ve Koleksiyonlar

ASP.NET Web sitesi içinde hemen her şeyi web sayfası dahil olmak üzere, bir nesne var. Bu bölümde, ile sık kodunuzda çalışabilecek önemli bazı nesneler açıklanmaktadır.

### <a name="page-objects"></a>Sayfa nesneleri

En temel ASP.NET sayfası nesnedir. Herhangi bir hak kazanan nesnesi olmadan doğrudan sayfası nesnenin özelliklerine erişebilirsiniz. Aşağıdaki kod sayfanın dosya yolunu alır kullanarak `Request` sayfasının nesne:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Bunu yapmak için özellikleri ve yöntemleri geçerli sayfa nesnesine başvuran temizleyin, isteğe bağlı olarak anahtar sözcüğünü kullanabilirsiniz `this` kodunuzda sayfa nesnesi temsil etmek için. İle önceki kod örneğinde, işte `this` sayfayı temsil etmek için eklenir:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Özelliklerini kullanabilirsiniz `Page` nesne gibi çok bilgi almak için:

- `Request`. Bu önceden gördüğünüz gibi ne tür bir tarayıcı yapılan istek URL'sini sayfa, kullanıcı kimliği, vb. dahil olmak üzere, geçerli istek hakkındaki bilgiler koleksiyonudur.
- `Response`. Sunucu kodu çalıştırma bittiğinde, tarayıcıya gönderilen yanıt (sayfa) hakkında bilgi koleksiyonudur. Örneğin, yanıtınıza yazmak için bu özelliği kullanabilirsiniz. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Koleksiyon nesnelerini (diziler ve sözlükleri)

A *koleksiyon* koleksiyonu gibi aynı türden nesneleri grubudur `Customer` veritabanı nesneleri. ASP.NET içeren pek çok yerleşik koleksiyonlar gibi `Request.Files` koleksiyonu.

Genellikle, koleksiyonları verilerle çalışırsınız. İki ortak koleksiyon türü *dizi* ve *sözlük*. Bir dizi benzer öğeleri koleksiyonu depolamak istediğiniz ancak her bir öğe tutmak için ayrı bir değişken oluşturmak istemeyen yararlı olur:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Dizilerle, bir özel veri türü gibi bildirdiğiniz `string`, `int`, veya `DateTime`. Bir dizi değişkeni içerebilir, köşeli ayraçlar için bildirimi ekleme belirtmek için (gibi `string[]` veya `int[]`). Öğeleri konumlarına (dizin) kullanarak bir diziye veya kullanarak erişebileceğiniz `foreach` deyimi. Dizi dizinleri sıfır tabanlı &#8212; diğer bir deyişle, birinci öğenin konumundaki konumlandırın 0, ikinci öğesi konum 1 ve benzeri.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Bir dizideki öğe sayısını alarak belirleyebilirsiniz, `Length` özelliği. (Diziyi aramak için) dizisinde belirli bir öğenin konumunu almak için kullanın `Array.IndexOf` yöntemi. Bir dizinin içeriğini de ters gibi şeyler yapabilirsiniz ( `Array.Reverse` yöntemi) veya içeriği sıralama ( `Array.Sort` yöntemi).

Bir tarayıcıda görüntülenen dize dizisi kodun çıktısı:

![Razor Img13](introducing-razor-syntax-c/_static/image13.jpg)

Bir anahtar/değer çifti koleksiyonunu ayarlayın veya karşılık gelen değeri almak için anahtar (veya ad) sağlarsınız sözlüktür:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Bir sözlüğü oluşturmak için kullandığınız `new` yeni bir sözlük nesnesi oluşturduğunuzu göstermek için anahtar sözcüğü. Bir değişken kullanarak bir sözlük atayabilirsiniz `var` anahtar sözcüğü. Açılı ayraçlar kullanarak sözlükteki öğe veri türlerini belirtin ( `< >` ). Bildirimin sonunda, bu gerçekte yeni bir sözlük oluşturur bir yöntem olduğundan, parantez çifti eklemeniz gerekir.

Öğeleri sözlüğüne eklenecek çağırabilirsiniz `Add` sözlük değişkeniyle yöntemi (`myScores` bu durumda) ve ardından bir anahtar ve değer belirtin. Alternatif olarak, köşeli ayraç anahtarı belirtmek ve aşağıdaki örnekte olduğu gibi basit bir atama yapmak için kullanabilirsiniz:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Sözlükten bir değer almak için köşeli ayraç içinde anahtarı belirtin:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Parametrelere sahip yöntemleri çağırma

Bu makalede daha önce belirtildiği gibi yöntemler ile program nesneler olabilir. Örneğin, bir `Database` nesne içerebilir bir `Database.Connect` yöntemi. Bir veya daha fazla parametre birçok yöntem de var. A *parametresi* bir yöntem için geçirdiğiniz değerdir, görevini tamamlamak üzere yöntemi etkinleştirmek için. Örneğin, bakmak için bir bildirim `Request.MapPath` üç parametre almayan yöntemi:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(Satır daha okunabilir yapmak için sarmalanmış. Satır sonları iç dizeleri dışında neredeyse tüm yerleştirin koyabilirsiniz unutmayın tırnak işareti içine alınmış.)

Bu yöntem, belirtilen sanal yolu sunucuda karşılık gelen fiziksel yolu döndürür. Yönteminin üç parametreler `virtualPath`, `baseVirtualDir`, ve `allowCrossAppMapping`. (Bildiriminde kabul verilerin veri türleriyle parametreleri listelendiğine dikkat edin.) Bu yöntemi çağırdığınızda, tüm üç parametrelerin değerlerini belirtmeniz gerekir.

Razor sözdizimi, bir yönteme parametreleri geçirmek için iki seçenek sunar: *konumsal parametreler* ve *adlandırılmış parametreleri*. Konumsal parametreler kullanan bir yöntem çağırmak için yöntem bildiriminde belirtilen katı bir sırayla parametreleri geçirin. (Genellikle bu sırada yöntemi için belgeleri okuyarak bilmeniz.) Sırayı izlemelidir ve herhangi bir parametre atlayamazsınız &#8212; gerekiyorsa, boş bir dize geçirirseniz (`""`) veya `null` için bir değer yoksa, konumsal bir parametre için.

Aşağıdaki örnekte adlı bir klasör olduğunu varsayar *betikleri* kullanarak Web sitenizde. Kod çağrıları `Request.MapPath` doğru sırada üç parametre değerlerini yöntemi ve geçirir. Ardından, elde edilen eşlenen yolun görüntüler.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Bir yöntem fazla parametre varsa, kodunuzu daha okunabilir adlandırılmış parametreleri kullanarak tutabilirsiniz. Adlandırılmış parametreler kullanılarak bir yöntem çağırmak için ardından iki nokta üst üste (:), ardından değeri tarafından parametre adı belirtin. Adlandırılmış parametreler avantajı, bunları istediğiniz herhangi bir sırada geçirebilirsiniz ' dir. (Bir dezavantajı yöntem çağrısında kompakt olmasıdır.)

Aşağıdaki örnek, yukarıdakilerle aynı yöntemini çağırır, ancak adlı değerlerini sağlamak için parametreleri kullanır:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Gördüğünüz gibi farklı parametreler geçirilir. Ancak, önceki örnekte ve bu örneği çalıştırırsanız, aynı değeri döneceksiniz.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Hataları işleme

### <a name="try-catch-statements"></a>Try-Catch deyimleri

Denetiminiz dışındaki bir nedenle başarısız olabilir, kodunuzda genellikle deyimleri sahip olacaksınız. Örneğin:

- Kodunuzu oluşturmak veya bir dosyaya erişmek çalışırsa, her türlü hata oluşabilir. İstediğiniz dosyayı var olmayabilir, kilitli olabilir, kod değil izinlere sahip ve benzeri.
- Benzer şekilde, veritabanındaki kayıtları güncelleştirmek kodunuzu çalışırsa, izin sorunları olabilir, veritabanı bağlantısını bırakılabilir, kaydetmek için verileri geçersiz vb. olabilir.

Programlama dilinde, bu gibi durumlarda çağrılır *özel durumları*. Kodunuz bir özel durumla karşılaşırsa (oluşturur) oluşturur. hata iletisi o, kullanıcılara en iyi sinir bozucu:

![Razor Img14](introducing-razor-syntax-c/_static/image14.jpg)

Durumlarda burada kodunuzu çalıştırdığınızca özel durumlar ve hata iletileri bu tür önlemek için kullanabileceğiniz `try/catch` deyimleri. İçinde `try` deyimi, denetimi kodu çalıştırın. Bir veya daha fazla `catch` deyimleri için özel konum (belirli tür özel durumlar) hatalar oluşmuş olabilir. Kadar içerebilir `catch` ifadeleri yazarken öngörerek hatalara bakmak gerekir.

> [!NOTE]
> Kullanmaktan kaçının öneririz `Response.Redirect` yönteminde `try/catch` deyimleri, bir özel durum sayfanızın neden olabileceği için.


Aşağıdaki örnek ilk isteğe bir metin dosyası oluşturur ve ardından kullanıcının dosyayı açma sağlayan bir düğme görüntüleyen bir sayfa görüntülenir. Örnek kasıtlı olarak hatalı dosya adı kullanır, böylece bir özel durum neden olur. Kodu içerir `catch` deyimleri için iki olası özel durumları: `FileNotFoundException`, dosya adı hatalı olması durumunda gerçekleşir ve `DirectoryNotFoundException`, ASP.NET bile klasörü bulamıyorsanız gerçekleşir. (, Örnek bir deyimde her şeyin düzgün çalıştığından, nasıl çalıştığını görmek için açıklama durumundan çıkarabilirsiniz.)

Kodunuzu özel durumu işlemek istemediğiniz ederseniz, önceki ekran görüntüsündeki gibi bir hata sayfası görürsünüz. Ancak, `try/catch` bölüm, kullanıcı bu tür hataları görmemesi yardımcı olur.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

**Visual Basic ile programlama**


[Ek: Visual Basic Dil ve sözdizimi](https://go.microsoft.com/fwlink/?LinkId=202908)


**Başvuru belgeleri**


[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[C# dili](https://msdn.microsoft.com/library/kx37x362.aspx)
