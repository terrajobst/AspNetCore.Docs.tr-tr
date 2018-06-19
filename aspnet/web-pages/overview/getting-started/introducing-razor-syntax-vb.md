---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Razor sözdizimini (Visual Basic) kullanarak ASP.NET Web programlamaya giriş | Microsoft Docs
author: tfitzmac
description: Bu ekte Razor sözdizimini kullanarak Visual Basic'te, ASP.NET Web sayfaları ile programlama genel bir bakış sağlar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: aad951a0e4344dbaafbdcc3b3980307a26fa75fc
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483490"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Razor sözdizimini (Visual Basic) kullanarak ASP.NET Web programlamaya giriş
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede Visual Basic ve Razor sözdizimini kullanan ASP.NET Web sayfaları ile programlama genel bir bakış sağlar. ASP.NET, dinamik web sayfaları web sunucuları üzerinde çalışan Microsoft teknolojisidir.
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


Razor sözdizimi ile ASP.NET Web sayfalarını kullanarak çoğu örnekler C# kullanın. Ancak, Razor sözdizimini Visual Basic de destekler. Bir web sayfası ile oluşturduğunuz program Visual Basic'te bir ASP.NET web sayfası için bir *.vbhtml* dosya adı uzantısı ve Visual Basic kodu ekleyin. Bu makalede Visual Basic dili ve ASP.NET Web sayfaları oluşturmak için sözdizimi ile çalışmaya genel bir bakış sağlar.

> [!NOTE]
> Microsoft WebMatrix için varsayılan Web sitesi şablonları (**Firincilik**, **Fotoğraf Galerisi**, ve **başlangıç sitesi**, vs.) C# ve Visual Basic sürümlerinde kullanılabilir. Visual Basic şablon NuGet paketlerini yükleyebilirsiniz. Web sitesi şablonları adlı bir klasörde, sitenizin kök klasörüne yüklenir *Microsoft Templates*.


## <a name="the-top-8-programming-tips"></a>8 programlama ipuçları

Bu bölümde kesinlikle Razor sözdizimini kullanan ASP.NET sunucusu kod yazmaya başladığınızda bilmeniz gereken birkaç ipucu listelenir.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Kod kullanarak bir sayfa ekleyin @ karakteri

`@` Karakter satır içi ifadeler, tek deyimli blokları ve birden fazla deyim blokları başlatır:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML kodlaması**
> 
> Kullanarak bir sayfa içeriği görüntülediğinizde `@` karakter, önceki örneklerde olduğu gibi çıktı ASP.NET HTML olarak kodlar. Bu ayrılmış HTML karakterlerini değiştirir (gibi `<` ve `>` ve `&`) HTML etiketleri veya varlıklar olarak yorumlanmak yerine bir web sayfasında karakter olarak görüntülenecek karakterleri etkinleştirmek kodlarıyla. HTML kodlaması olmadan sunucu kodunuzu çıktısını düzgün görüntülenmeyebilir ve sayfa güvenlik risklerine maruz bırakabileceğinden.
> 
> Amacınız biçimlendirme olarak etiketlerini işler HTML biçimlendirmesi çıkış olup olmadığını (örneğin `<p></p>` paragrafı için veya `<em></em>` metin vurgulamak için), bölümüne bakın [birleştirme metin, biçimlendirme ve kod blokları kodda](#BM_CombiningTextMarkupAndCode) bu makalenin ilerisinde yer.
> 
> Daha fazla bilgiyi, HTML kodlaması hakkında [ASP.NET Web Pages sitelerinde HTML formları ile çalışma](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Kod blokları koduyla içine... Bitiş kodu

Kod bloğu bir veya daha fazla kod deyimleri içerir ve anahtar sözcükleriyle içine `Code` ve `End Code`. Açılış yerleştirin `Code` anahtar sözcüğü hemen sonra `@` karakter &#8212; olamaz boşluk aralarında.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. Bir blok içinde satır sonu her kod açıklaması bitiş

Visual Basic kod bloğunda her deyimi bir satır sonu ile sona erer. (Makalenin sonraki bölümlerinde uzun kod açıklaması birden çok satıra gerekirse sarmalamak için bir yol görürsünüz.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Değerlerini depolamak için değişkenleri kullanma

Değerleri depolayabilir bir *değişkeni*dizeler, sayılar ve tarihleri, vb. dahil olmak üzere. Kullanarak yeni bir değişken oluşturmak `Dim` anahtar sözcüğü. Doğrudan sayfasını kullanarak bir değişken değerleri ekleyebilirsiniz `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Değişmez değer dize değerleri çift tırnak işaretleri içine alın

A *dize* metin olarak kabul edilir karakter sırasıdır. Bir dize belirtmek için çift tırnak işaretleri içine alın:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Bir dize değeri çift tırnak işaretleri eklemek için iki çift tırnak işareti karakteri ekleyin. Sayfa çıktısının bir kez görünmesi çift tırnak karakteri istiyorsanız olarak girin `""` tırnak işaretli içinde dize ve iki kez görünmesini istiyorsanız olarak girin `""""` tırnak işaretli dizesi içinde.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Visual Basic kodu büyük küçük harfe duyarlı değil

Visual Basic Dil büyük küçük harfe duyarlı değil. Programlama anahtar sözcükler (gibi `Dim`, `If`, ve `True`) ve değişken adları (gibi `myString`, veya `subTotal`) herhangi bir durumda yazılabilir.

Aşağıdaki kod satırlarını değişkene bir değer atamak `lastname` küçük harf kullanarak ad ve bir büyük harf adını kullanarak sayfasına değişken değeri çıktı.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![vb sözdizimi 5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Kodlama çoğunu nesnelerle çalışmayı içerir

Bir nesne ile program bir şeyi temsil eden &#8212; bir sayfa, bir metin kutusu, bir dosya, görüntü, bir web isteği, bir e-posta iletisi, bir müşteri kaydı (veritabanı satır) vb. Nesnelerin özelliklerini açıklayan özellikleri vardır &#8212; bir metin kutusu nesnesi bir `Text` istek nesnesi özelliğine sahip bir `Url` bir e-posta iletisi özelliğine sahip bir `From` özelliğine ve bir müşteri nesnesi sahiptir bir `FirstName` özellik. Nesneleri olan yöntemlerini de &quot;fiiller&quot; yapabilirler. Örnekler bir dosya nesnesinin `Save` yöntemi, bir görüntü nesnenin `Rotate` yöntemi ve e-posta nesnenin `Send` yöntemi.

Genellikle ile karşılaşmayacağınızı `Request` form değerleri gibi bilgileri verir nesne alanları sayfasında (metin kutuları, vb.), tarayıcının ne tür, sayfa, kullanıcı kimliği, vb. URL'sini istekte. Bu örnek özelliklerine erişmek nasıl gösterir `Request` nesne ve nasıl çağrılacağını `MapPath` yöntemi `Request` sayfasının mutlak yolu sunucuda verir nesnesi:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Kararları kod yazma

Bir anahtar dinamik web sayfaları koşullara dayanarak ne yapacağınıza belirlemek özelliğidir. Bunu yapmak için en yaygın yolu `If` deyimi (ve isteğe bağlı `Else` deyimi).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

Deyim `If IsPost` kestirme yol yazma, `If IsPost = True`. İle birlikte `If` deyimleri, çeşitli yollarla koşullarda, kod, yineleme bloklarını test etmek için vardır ve vb., bu makalenin sonraki bölümlerinde açıklanmıştır.

Bir tarayıcıda görüntülenen sonuç (tıkladıktan sonra **gönderme**):

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET ve POST yöntemleri ve IsPost özelliği**
> 
> Web sayfaları (HTTP) için kullanılan protokol yöntemleri çok sınırlı sayıda destekler (&quot;fiiller&quot;) sunucuya isteği yapmak için kullanılır. İki en yaygın bir sayfa okumak için kullanılan GET ve sayfa göndermek için kullanılan POST olanlardır. Genel olarak, bir kullanıcı bir sayfa istekleri ilk kez sayfa GET kullanarak istendi. Kullanıcı bir formda doldurur ve ardından tıklattığında **gönderme**, tarayıcı sunucuya bir POST isteği yapar.
> 
> Programlama web sayfasını işlemek nasıl bilmesi bir sayfa bir GET veya POST olarak istenen olup olmadığını öğrenmek yararlıdır. ASP.NET Web Pages'da kullandığınız `IsPost` bir isteğin bir GET veya POST olup olmadığını görmek için özellik. Bir POST isteğiyse `IsPost` özelliği true döndürür ve metin kutuları bir form üzerinde değerlerini okuma gibi şeyler yapabilirsiniz. Birçok örnek göreceksiniz sayfanın farklı değerine bağlı olarak işlemek nasıl Göster `IsPost`.


## <a name="a-simple-code-example"></a>Basit bir kod örneğidir

Bu yordam temel programlama tekniklerinin gösteren bir sayfa oluşturulacağını gösterir. Örnekte, kullanıcıların bunları ekler ve sonucu görüntüler sonra iki sayıyı girin olanak sağlayan bir sayfa oluşturun.

1. Düzenleyicinizde, yeni bir dosya oluşturun ve adlandırın *AddNumbers.vbhtml*.
2. Aşağıdaki kod ve biçimlendirme zaten sayfasındaki herhangi bir şey değiştirme sayfanın kopyalayın.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Dikkat edilecek bazı öğeleri şunlardır:

    - `@` Karakter sayfasında ilk kod bloğunu başlar ve kendisinden `totalMessage` değişkeni katıştırılmış altına.
    - Sayfanın üstündeki blok sınırlanan `Code...End Code`.
    - Değişkenleri `total`, `num1`, `num2`, ve `totalMessage` birkaç numarası ve bir dize depolar.
    - Atanan değişmez dize değeri `totalMessage` çift tırnak işaretleri değişkenidir.
    - Visual Basic kodu zaman büyük/küçük harfe duyarlı olmadığı için `totalMessage` değişkeni sayfanın altı kullanıldığında, adı yalnızca sayfanın üst kısmındaki değişken bildirimi yazımını ile eşleşmesi gerekir. Büyük küçük harf önemli değildir.
    - İfade `num1.AsInt()`  +  `num2.AsInt()` nesneleri ve yöntemleri ile nasıl çalışılacağı gösterir. `AsInt` Yöntemi her değişken bir eklenebilecek tamsayıya (tamsayı) bir kullanıcı tarafından girilen dize dönüştürür.
    - `<form>` Etiketi de içeren bir `method="post"` özniteliği. Bu belirtir kullanıcı tıklattığında **Ekle**, sayfa HTTP POST yöntemini kullanarak sunucuya gönderilir. Ne zaman sayfa gönderildiğinde, kodu `If IsPost` true ve koşullu değerlendirir kod sayıları ekleme sonucu görüntülenirken çalıştırır.
3. Sayfayı kaydedin ve tarayıcıda çalıştırın. (Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.) İki tam sayılar girin ve ardından **Ekle** düğmesi.

    ![Razor Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic dili ve sözdizimi

Daha önce bir ASP.NET web sayfası oluşturma ve sunucu kodunu HTML biçimlendirmesi nasıl ekleyebileceğiniz temel bir örneği gördünüz. Burada Razor sözdizimini kullanarak ASP.NET sunucusu kod yazmak için Visual Basic kullanılarak temellerini öğreneceksiniz &#8212; diğer bir deyişle, programlama dili kuralları.

(Özellikle, C, C++, C#, Visual Basic veya JavaScript kullandıysanız) programlama ile deneyimli değilseniz, ne burada okuma çoğunu tanıdık gelecektir. Büyük olasılıkla yalnızca nasıl WebMatrix kod biçimlendirmede eklenen ile öğrenmeniz gerekir *.vbhtml* dosyaları.

### <a id="BM_CombiningTextMarkupAndCode"></a>  Metin, biçimlendirme ve kod blokları içinde kodu birleştirme

Sunucu kod bloğu, genellikle metin ve biçimlendirme sayfasına çıkış istersiniz. Sunucu kod bloğu kodu değil ve, bunun yerine olarak işleneceğini metin içeriyorsa, ASP.NET metnin kodunu ayırt etmek gerekir. Bunu yapmanın birkaç yolu vardır.

- Gibi bir HTML blok öğesindeki metni içine `<p></p>` veya `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    HTML öğesi, metin, ek HTML öğeleri ve sunucu kodu ifadeleri içerebilir. ASP.NET, HTML etiketi açılış gördüğünde (örneğin, `<p>`), her şeyi öğesi oluşturur ve bunun içeriği olarak tarayıcı (ve çözümler için sunucu kodu ifadeleri).

- Kullanım `@:` işleci veya `<text>` öğesi. `@:` Tek satırlık bir düz metin veya eşleşmeyen HTML etiketleri; içeren içerik çıkarır `<text>` öğesi çıktısını almak için birden fazla satır alır. Bu seçenekler, çıkışı bir parçası olarak bir HTML öğesini işlemek istemediğiniz zaman yararlıdır.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    Aşağıdaki örnek, önceki örnekte yineler ancak tek bir çift kullanan `<text>` etiketleri oluşturmak için metni alın.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    Aşağıdaki örnekte, `<text>` ve `</text>` etiketlerini içine üç satır, bunların tümü sahip bazı uncontained metin ve eşleşmeyen HTML etiketleri (`<br />`), sunucu kodu ve eşleşen HTML etiketleri yanı sıra. Yine de her satırın tek tek koyun `@:` işleci; her iki şekilde çalışır.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Olduğunda, çıktı metin bu bölümde gösterilen &#8212; bir HTML öğesi kullanarak `@:` işleci veya `<text>` öğesi &#8212; ASP.NET olmayan HTML olarak kodlanacak çıktı. (Daha önce belirtildiği gibi ASP.NET sunucu kodu ifadeleri ve tarafından öncesinde sunucu kod blokları çıktısını kodlamak `@`, bu bölümde belirtildiği özel durumlar hariç.)

### <a name="whitespace"></a>Boşluk

Ek boşluk deyimi içinde (ve bir değişmez dize değeri dışında) deyim etkilemez:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Birden çok satırlara uzun deyimleri ayırma

Uzun kod açıklaması alt çizgi karakterini kullanarak birden çok çizgiye bölün `_` (Visual Basic'te adlandırılan *devamlılığı karakteri*) her kod satırının sonra. Break deyimi sonraki satıra üzerine satırın sonuna bir boşluk ve devamlılığı karakteri ekleyin. Deyim bir sonraki satırında devam edin. Deyimleri okunabilirliğini artırmak gerektiği kadar satır üzerine kayabilir. Aşağıdaki deyimleri aynıdır:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Ancak, bir değişmez dize değeri ortasında bir satır kaydırılamıyor. Aşağıdaki örnek çalışmıyor:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Yukarıdaki kod gibi birden çok satır sarmalar uzun bir dize birleştirmek için size kullanmanız gerekir *birleştirme işleci* (`&`), hangi bu makalenin sonraki bölümlerinde görürsünüz.

### <a name="code-comments"></a>Kod açıklamaları

Açıklamalar, kendiniz veya başkaları için Notlar bırakın olanak tanır. Razor sözdizimi açıklamaları öneki ile `@*` ve sonunda `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Kod blokları içinde Razor sözdizimi açıklamaları kullanabilirsiniz ya da tek tırnak işareti olan sıradan Visual Basic açıklama karakterini kullanabilirsiniz (`'`) her satır için önek.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Değişkenler

Bir değişken verilerini depolamak için kullandığınız adlandırılmış bir nesnedir. Herhangi bir şey değişkenleri adı verebilirsiniz ancak adı alfabetik bir karakter ile başlamalı ve boşluk ya da ayrılmış karakterler içeremez. Visual Basic'te, daha önce gördüğünüz gibi büyük/küçük harf bir değişken adı önemli değildir.

### <a name="variables-and-data-types"></a>Değişkenleri ve veri türleri

Bir değişken ne tür veriler değişkende depolanır gösteren bir özel veri türü olabilir. Dize değerlerini depolayan dize değişkenleri olabilir (gibi &quot;Merhaba Dünya&quot;), tam sayı değerleri (örneğin, 3 veya 79) depolamak tamsayı değişkenleri ve biçimleri (4/12/2012 veya Mart 2009 gibi çeşitli tarih değerlerini depolama tarih değişkenleri ). Ve kullanabileceğiniz birçok diğer veri türleri vardır.

Ancak, bir değişken için bir tür belirtmek zorunda değilsiniz. Çoğu durumda ASP.NET değişkeni verileri nasıl kullanıldığını temel türü tahmin. (Bazen bir türü belirtmeniz gerekir; bu doğru olduğu örnekler görürsünüz.)

Bir değişken türü belirtmeden bildirmek için kullanmak `Dim` değişken adı artı (örneğin, `Dim myVar`). Bir değişken türü ile bildirmek için kullanmak `Dim` ve ardından değişkeni artı `As` ve ardından tür adını (örneği için `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

Aşağıdaki örnek, bir web sayfasında değişkenleri kullanan bazı satır içi ifadeler gösterir.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Dönüştürme ve veri türleri test etme

Bazen ASP.NET genellikle bir veri türü otomatik olarak belirlemek de, işlem gerçekleştirilemiyor. Bu nedenle, açık bir dönüştürme gerçekleştirerek ASP.NET yardımcı olması gerekebilir. Bazen türleri dönüştürme gerekmez olsa bile, veri türünü, birlikte çalışabilir görmek için test etmek yardımcı olur.

En yaygın olarak bir tam sayı veya tarih gibi bir dizeyi başka bir türüne dönüştürmek sahip olur. Aşağıdaki örnek, burada, bir dizeyi sayıya dönüştürme gerekir tipik bir durumu gösterir.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Bir kural olarak, kullanıcı girişi için dize olarak gelir. Bir sayı girin kullanıcıya sorulur olsa bile ve bir rakam girmiş olduğunuz da kullanıcı girişi gönderildiğinde ve kodda okuma olsa bile, veri dize biçiminde sağlanır. Bu nedenle, dize bir sayıya dönüştürmeniz gerekir. Bunları, dönüştürmeden değerlerine aritmetik gerçekleştirmeye çalışırsanız ASP.NET iki dizeyi eklediğinden örnekte, aşağıdaki hata, sonuçları:

`Cannot implicitly convert type 'string' to 'int'.`

Tamsayılara değerleri dönüştürmek için arama `AsInt` yöntemi. Dönüştürme başarılı olursa, sayıları daha sonra ekleyebilirsiniz.

Aşağıdaki tabloda bazı yaygın dönüştürme ve test yöntemleri değişkenleri listeler.


: satır:: sütun: <strong>yöntemi</strong> : sütun uç:: sütun: <strong>açıklama</strong> : sütun uç:: sütun: <strong>örnek</strong> : sütun uç:: satır sonu:
* * *
: satır:: sütun: `AsInt(), IsInt()` : sütun uç:: sütun: bir tam sayı temsil eden bir dize dönüştürür (gibi &quot;593&quot;) bir tamsayı.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `AsBool(), IsBool()` : sütun uç:: sütun: bir dizeyi gibi dönüştürür &quot;true&quot; veya &quot;false&quot; bir Boolean türüne.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `AsFloat(), IsFloat()` : sütun uç:: sütun: gibi ondalık bir değeri olan bir dize dönüştürür &quot;1.3&quot; veya &quot;7.439&quot; bir kayan noktalı sayı.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `AsDecimal(), IsDecimal()` : sütun uç:: sütun: gibi ondalık bir değeri olan bir dize dönüştürür &quot;1.3&quot; veya &quot;7.439&quot; ondalık sayıya. (ASP.NET, ondalık sayı bir kayan nokta numarasından daha kesin.) : sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `AsDateTime(), IsDateTime()` : sütun uç:: sütun: ASP.NET için bir tarih ve saat değerini temsil eden bir dize dönüştürür `DateTime` türü.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `ToString()` : sütun uç:: sütun: başka bir veri türü bir dizeye dönüştürür.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    : sütun uç:: satır sonu:


## <a name="operators"></a>İşleçler

Bir işleç bir anahtar sözcük veya ASP ne tür bir ifadede gerçekleştirmek için komutu bir karakter değil. Visual Basic birçok işleçleri destekler, ancak yalnızca ASP.NET web sayfaları geliştirmeye başlamak için birkaç tanıması gerekir. Aşağıdaki tabloda, en yaygın işleçleri özetler.


: satır:: sütun: <strong>işleci</strong> : sütun uç:: sütun: <strong>açıklama</strong> : sütun uç:: sütun: <strong>örnekleri</strong> : sütun uç:: satır sonu:
* * *
: satır:: sütun: `+ - * /` : sütun uç:: sütun: matematik işleçleri sayısal ifadelerde kullanılır.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `=` : sütun uç:: sütun: atama ve eşitlik. Bağlam, bağlı olarak ya da sol tarafında nesnede deyiminin sağ taraftaki değer atayan veya eşitlik için değerleri denetler.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `<>` : sütun uç:: sütun: eşitsizlik. Döndürür `True` değerler eşit değilse.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `< > <= >=` : sütun uç:: sütun:'den, büyük, küçük veya eşit ve büyüktür veya eşittir.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `&` : sütun uç:: sütun: dizeleri katılmak için kullanılan birleştirme.
: sütun uç:: sütun: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `+= -=` : sütun uç:: sütun: ekleme ve 1 (sırasıyla) bir değişkeninden artırma ve azaltma işleçleri.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `.` : sütun uç:: sütun: nokta. Nesneleri ve özellikleri ve yöntemleri ayırt etmek için kullanılır.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `()` : sütun uç:: sütun: parantez. Parametreleri yöntemleri ve erişim üyeleri dizileri ve koleksiyonları geçirmek için Grup ifadeleri için kullanılır.
: sütun uç:: sütun: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `Not` : sütun uç:: sütun: değil. True değeri false ve tersi yönde tersine çevirir. Sınamak için bir toplu şekilde genelde kullanılan `False` (diğer bir deyişle, için değil `True`).
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    : sütun uç:: satır sonu:
* * *
: satır:: sütun: `AndAlso OrElse` : sütun uç:: sütun: mantıksal ve ve veya birlikte koşulları hangi bağlamak için kullanılır.
: sütun uç:: sütun: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    : sütun uç:: satır sonu:

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Sanal kök başvuran: ~ işleci ve Href yöntemi

İçinde bir *.cshtml* veya *.vbhtml* dosyası, sanal kök yolu kullanarak başvuru `~` işleci. Bu bir sitede sayfaları taşıyabilirsiniz ve diğer sayfalara içerdikleri tüm bağlantılar bozuk olmayacaktır çünkü çok kullanışlıdır. Ayrıca, her zamankinden Web sitenizi farklı bir konuma taşımanız durumunda kullanışlıdır. Bazı örnekler şunlardır:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Web sitesi ise `http://myserver/myapp`, işte sayfa çalıştığında ASP.NET bu yollar nasıl kabul eder:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Aslında bu yollar değişkeni değerleri olarak görmez, ancak, ne oldukları ise ASP.NET yollar kabul eder.)

Kullanabileceğiniz `~` işleci (yukarıdaki gibi) sunucu kodu hem de bu gibi biçimlendirme:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Biçimlendirme içinde kullandığınız `~` görüntü dosyaları, diğer web sayfalarını ve CSS dosyaları gibi kaynaklarına yollar oluşturmak için işleç. Sayfa çalıştığında, ASP.NET (kod ve biçimlendirme) sayfadan arar ve tüm çözümler `~` uygun yolu başvuruları.

## <a name="conditional-logic-and-loops"></a>Koşullu mantık ve döngüler

ASP.NET sunucusu kod koşullar ve bir döngü çalışan başka bir deyişle, belirli sayıda kod deyimleri yineler yazma kod temelinde görevleri gerçekleştirmenize olanak sağlar).

### <a name="testing-conditions"></a>Koşullar test etme

Kullandığınız basit bir koşulu test etmek için `If...Then` döndürür deyimi `True` veya `False` belirttiğiniz bir testi temel alan:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

`If` Anahtar sözcüğü bir blok başlatır. Gerçek test (koşul) izleyen `If` anahtar sözcüğü ve true veya false değerini döndürür. `If` Deyimi sona erdirir ile `Then`. Test true ise, çalıştırılacak deyimleri kapsadığı `If` ve `End If`. Bir `If` deyimi dahil edebileceğiniz bir `Else` koşul yanlış ise, çalıştırılacak deyimleri belirtir engelle:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Varsa bir `If` deyimi kod bloğu başlatır ve normal kullanmak zorunda değilsiniz `Code...End Code` blokları içerecek şekilde deyimleri. Eklemeniz yeterlidir `@` bloğuna ve çalışır. Bu yaklaşım çalışır `If` diğer Visual Basic dahil olmak üzere kod blokları tarafından izlenen anahtar sözcükleri programlama yanı sıra `For`, `For Each`, `Do While`vb.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Bir veya daha fazla kullanarak birden çok koşul ekleyebilirsiniz `ElseIf` engeller:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

Bu örnekte, içinde ilk koşul, `If` blok true değil `ElseIf` koşul denetlenir. Bu koşul karşılandığında deyimlerinde `ElseIf` blok çalıştırılır. Hiç bir koşul karşılanıyorsa deyimlerinde `Else` blok çalıştırılır. Herhangi bir sayıda ekleyebilirsiniz `ElseIf` engeller ve ile kapatın bir `Else` olarak engellemek &quot;şey&quot; koşulu.

Çok sayıda koşullar sınamak için kullanın bir `Select Case` engelle:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Test etmek için parantez içinde (örnekte haftanın günü değişkeni) değerdir. Tek tek her test kullanan bir `Case` bir değer listeler deyimi. Varsa değerini bir `Case` deyimi, kodda test değeri ile eşleşen `Case` blok gerçekleştirilir.

Bir tarayıcıda görüntülenen son iki koşullu blokları sonucu:

![Razor Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Döngü kodu

Genellikle aynı deyimleri tekrar tekrar çalıştırmanız gerekir. Bunun için döngüyle. Örneğin, genellikle aynı deyimlerini her öğe için bir veri koleksiyonunda çalıştırın. Tam döngü istediğiniz kaç kez biliyorsanız, kullanabileceğiniz bir `For` döngü. Bu tür bir döngü sayım veya sayım için özellikle yararlı olur:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Döngü ile başlayan `For` anahtar sözcüğü, üç öğeleri tarafından takip:

- Hemen sonra `For` deyimi, bir sayaç değişken bildirme (kullanmak zorunda değilsiniz `Dim`) ve ardından olarak aralık belirtmek `i = 10 to 20`. Bu değişkeni anlamına gelir `i` 10 sayım başlatmak ve 20 (dahil) ulaşana kadar devam edin.
- Arasında `For` ve `Next` deyimleri olduğu blok içeriği. Bu, çalıştırılacak bir veya daha fazla kod deyimleri her döngü ile içerebilir.
- `Next i` Deyimi döngü sona erdirir. Bu sayaç artırılır ve sonraki döngü başlatır.

Kod arasında satır `For` ve `Next` satırlarını her döngü için çalışan bir kod içerir. Yeni bir paragraf biçimlendirme oluşturur (`<p>` öğesi) her zaman ve değerini görüntüleme çıktısı için bir satır ekler i (sayaç). Bu sayfayı çalıştırdığınızda, örnek öğe sayısını gösteren her satırın metinle çıkış görüntüleme 11 satırları oluşturur.

![Razor Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Bir koleksiyonu veya dizisi ile çalışıyorsanız, sık kullandığınız bir `For Each` döngü. Bir koleksiyon benzer nesneleri oluşan bir gruptur ve `For Each` döngü koleksiyondaki her öğe bir görevi yerine getirmek sağlar. Bu tür bir döngü koleksiyonlar için uygun olan çünkü aksine bir `For` döngüsü, sınırı sayaç artırın veya zorunda değilsiniz. Bunun yerine, `For Each` döngü kodunu işlemi tamamlanana kadar toplulukta yalnızca geçer.

Bu örnek öğeleri döndürür `Request.ServerVariables` (web sunucunuz hakkında bilgi içeren) koleksiyon. Kullandığı bir `For Each` yeni oluşturarak her öğenin adını görüntülemek için döngü `<li>` öğesi bir HTML madde işaretli listede.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

`For Each` Anahtar sözcüğü koleksiyonunda tek bir öğeyi temsil eden bir değişken arkasından (örnekte `myItem`), ardından `In` döngü istediğiniz koleksiyonu ve ardından bir anahtar sözcük,. Gövdesinde `For Each` döngü, daha önce bildirilen değişkeni kullanarak geçerli öğesi erişebilir.

![Razor Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Daha fazla genel amaçlı bir döngü oluşturmak üzere kullanmanız `Do While` deyimi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Bu döngü ile başlayan `Do While` anahtar sözcüğünü bir koşul tarafından ardından yinelenecek bloğu tarafından. Döngüler genellikle Artır (Ekle) veya azaltma (Çıkart) bir değişken veya sayım için kullanılan nesne. Örnekte, `+=` işleci döngünün her çalıştığında bir değişken değerine 1 ekler. (Bir değişken sayıları aşağı Döngüdeki düşürmek için azaltma işleci kullanırsınız `-=`.)

## <a name="objects-and-collections"></a>Nesneleri ve koleksiyonları

ASP.NET Web sitesi, neredeyse her şeyi web sayfası dahil olmak üzere bir nesnesidir. Bu bölümde, ile sık kodunuzda karşılaşmayacağınızı bazı önemli nesneleri anlatılmaktadır.

### <a name="page-objects"></a>Sayfa nesneleri

En temel ASP.NET sayfası nesnedir. Hiçbir belirleme nesnesi olmadan doğrudan sayfa nesnesinin özellikleri erişebilir. Aşağıdaki kod sayfanın dosya yolunu alır kullanarak `Request` sayfasının nesnesi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Özelliklerini kullanabilirsiniz `Page` nesne gibi bir çok sayıda bilgi, almak için:

- `Request`. Bu önceden gördüğünüz gibi tarayıcının ne tür, sayfa, kullanıcı kimliği, vb. URL'sini istekte dahil olmak üzere geçerli istek hakkındaki bilgiler koleksiyonudur.
- `Response`. Bu, sunucu kodu çalışmasını bitirdikten sonra tarayıcıya gönderilen yanıt (sayfa) hakkında bilgi koleksiyonudur. Örneğin, bilgi yanıtına yazmak için bu özelliği kullanabilirsiniz.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Koleksiyon nesnesi (diziler ve sözlükler)

Aynı türde bir koleksiyonu gibi bir grup koleksiyonudur `Customer` nesnelerini bir veritabanından. ASP.NET içeren birçok yerleşik koleksiyonlar gibi `Request.Files` koleksiyonu.

Koleksiyonlarda verileri genellikle çalışmak. İki ortak koleksiyon türü *dizi* ve *sözlük*. Bir dizi benzer öğeleri koleksiyonu depolamak istiyorsanız, ancak her bir öğeyi tutmak için ayrı bir değişken oluşturmak istemediğiniz durumlarda yararlıdır:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Dizilerle, bir özel veri türü gibi bildirdiğiniz `String`, `Integer`, veya `DateTime`. Bir dizi değişkeni içerebilir, değişken adı bildiriminde parantez eklemek belirtmek için (gibi `Dim myVar() As String`). Öğeleri konumlarını (dizin) kullanarak bir dizi veya kullanarak erişebilirsiniz `For Each` deyimi. Dizi dizinleri sıfır tabanlı &#8212; diğer bir deyişle, ilk öğe adresindeki konumlandırın 0, ikinci öğesi konum 1 ve benzeri.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Alarak dizideki öğelerin sayısını belirleyebilirsiniz kendi `Length` özelliği. Belirli bir öğenin dizideki konumunu almak için (diğer bir deyişle, dizi aramak için), kullanın `Array.IndexOf` yöntemi. Bir dizinin içeriğini da ters gibi şeyler yapabilirsiniz ( `Array.Reverse` yöntemi) veya içeriği sıralama ( `Array.Sort` yöntemi).

Bir tarayıcıda görüntülenen dize dizisi kodu çıktı:

![Razor Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Bir anahtar (veya ad) karşılık gelen değeri ayarlayamaz veya Burada sağladığınız anahtar/değer çiftleri koleksiyonu dictionary'si:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Bir sözlük oluşturmak için kullandığınız `New` yeni oluşturuyorsanız göstermek için anahtar sözcüğü `Dictionary` nesnesi. Bir değişken kullanarak bir sözlük atayabilirsiniz `Dim` anahtar sözcüğü. Parantez kullanılarak sözlükteki öğe veri türlerini gösterir ( `( )` ). Bildirim sonunda, bu yeni bir sözlük oluşturur yöntemi gerçekte olduğundan başka bir çift ayraç, eklemeniz gerekir.

Sözlüğe öğeler eklemek için çağırabilirsiniz `Add` sözlük değişkenin yöntemi (`myScores` bu durumda) ve ardından bir anahtar ve bir değer belirtin. Alternatif olarak, anahtarı belirtmek ve aşağıdaki örnekteki gibi basit bir atama yapmak için parantezleri kullanabilirsiniz:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Sözlükten bir değer almak için parantez içinde anahtarı belirtin:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Parametrelerle yöntemleri çağırma

Bu makalede anlatıldığı gibi ile program nesnelerin yöntemleri vardır. Örneğin, bir `Database` nesnesi olabilir bir `Database.Connect` yöntemi. Birçok yöntem de bir veya daha fazla parametre vardır. A *parametresi* bir yönteme geçirin bir değer, görevini tamamlamak üzere yöntemini etkinleştirmek için. Örneğin, bir bildirimi bakın `Request.MapPath` üç parametre alır yöntemi:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Bu yöntem, belirtilen sanal yolu sunucuda karşılık gelen fiziksel yolu döndürür. Yöntem için üç parametreleri `virtualPath`, `baseVirtualDir`, ve `allowCrossAppMapping`. (Kabul verileri veri türleriyle parametreleri bildiriminde listelenen dikkat edin.) Bu yöntemi çağırdığınızda, tüm üç parametreleri için değerler girmeniz gerekir.

Visual Basic Razor sözdizimi ile kullanırken, bir yöntem parametreleri geçirme için iki seçeneğiniz vardır: *konumsal parametreler* veya *adlandırılmış parametreleri*. Konumsal parametreler kullanarak bir yöntemi çağırmak için yöntem bildiriminde belirtilen katı bir sırada parametreleri geçirin. (Genellikle bu sırada yöntemi belgelerine okuyarak bilmeniz.) Sırayı izler ve parametrelerinden herhangi birini atlayamazsınız &#8212; gerekirse, boş bir dize geçirdiğiniz (`""`) için bir değer yoksa konumsal bir parametre için ya da null.

Aşağıdaki örnek adlı bir klasör olduğunu varsayar *betikleri* Web sitenizde. Kod çağrıları `Request.MapPath` üç parametre doğru sırada yöntemi ve geçişleri değerlerini. Ardından, elde edilen eşlenen yolun görüntüler.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Bir yöntem için birçok parametrelerini olduğunda adlandırılmış parametreler kullanılarak temizleyici ve daha okunabilir kodunuzu tutabilirsiniz. Adlandırılmış parametreler kullanılarak bir yöntemi çağırmak için belirtin ve ardından parametre adı `:=` değeri girin. Adlandırılmış parametreler avantajı, bunları istediğiniz herhangi bir sırada ekleyebilirsiniz ' dir. (Bir dezavantajı yöntem çağrısının olarak compact olmamasıdır.)

Aşağıdaki örnek yukarıdaki gibi aynı yöntemini çağırır, ancak adlı parametre değerlerini sağlamak için kullanır:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Gördüğünüz gibi parametreleri farklı bir sırayla geçirilir. Ancak, önceki örnekte ve bu örnek çalıştırırsanız, aynı değere döneceksiniz.

## <a name="handling-errors"></a>Hataları işleme

### <a name="try-catch-statements"></a>Try-Catch deyimleri

Genellikle, denetimi dışında kalan nedeniyle başarısız olabilir, kodunuzda deyimleri sahip olacaksınız. Örneğin:

- Kodunuzu açın, oluşturmak, okumak veya bir dosya yazmak çalışırsa, her türlü hataları oluşabilir. İstediğiniz dosya var olmayabilir, kilitli olabilir, kodu değil izinlere sahip ve benzeri.
- Benzer şekilde, kodunuzu veritabanındaki kayıtları güncelleştirme çalışırsa, izin sorunları olabilir, veritabanı bağlantısı bırakılabilir, kaydetmek için verileri geçersiz vb. olabilir.

Bu durumlarda programlama dilinde denir *özel durumları*. Kodunuzu bir özel durum karşılaşırsa (atar) oluşturan bir hata iletisi diğer bir deyişle, en iyi kullanıcılara sinir bozucu.

![Razor Img14](introducing-razor-syntax-vb/_static/image14.jpg)

Burada kodunuzu karşılaşabileceğiniz özel durumlar durumlarda ve hata iletileri bu tür önlemek için kullanabileceğiniz `Try/Catch` deyimleri. İçinde `Try` deyimi, denetimi kod çalıştırma. Bir veya daha `Catch` deyimleri için özel konum (belirli tür özel durumlar) hatalar oluşmuş olabilir. Kadar içerebilir `Catch` deyimleri yazarken gereken bekleme hataları aramak.

> [!NOTE]
> Kullanmaktan kaçının öneririz `Response.Redirect` yönteminde `Try/Catch` deyimleri, çünkü bu bir özel durum sayfanızda neden olabilir.


Aşağıdaki örnek ilk istek üzerinde bir metin dosyası oluşturur ve kullanıcının dosyayı açma olanak sağlayan bir düğme görüntüleyen bir sayfada görüntülenir. Örnek kasıtlı olarak hatalı dosya adı kullanır, böylece bir özel durumuna neden olur. Kod içeren `Catch` deyimleri iki olası özel durumlar için: `FileNotFoundException`, dosya adı hatalı olması durumunda gerçekleşir ve `DirectoryNotFoundException`, ASP.NET bile klasörünü bulamazsanız oluşur. (, Örnek bir deyimde her şeyin doğru şekilde çalışırken, nasıl çalıştığı görmek için açıklamadan çıkarın.)

Kodunuzu özel durum işleme alamadık, önceki ekran görüntüsü gibi hata sayfasını görürsünüz. Ancak, `Try/Catch` bölüm, kullanıcı bu tür hataların görmemesi yardımcı olur.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

### <a name="reference-documentation"></a>Başvuru belgeleri

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic dili](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
