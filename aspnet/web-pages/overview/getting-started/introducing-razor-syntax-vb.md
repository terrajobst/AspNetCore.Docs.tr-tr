---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: ASP.NET Web programlama Razor söz dizimini (Visual Basic) kullanarak giriş | Microsoft Docs
author: tfitzmac
description: Bu ekte Razor sözdizimini kullanarak Visual Basic'te, ASP.NET Web sayfaları ile programlamaya genel bir bakış sağlar.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: cbec035533c37723afcd5bf4aa0c6e1c83dbae23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755334"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>ASP.NET Web programlama Razor söz dizimini (Visual Basic) kullanarak giriş
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede Razor sözdizimi ve Visual Basic kullanarak ASP.NET Web sayfaları ile programlamaya genel bir bakış sağlar. ASP.NET dinamik web sayfaları web sunucuları üzerinde çalışan için Microsoft'un teknolojisidir.
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


Razor sözdizimi olan ASP.NET Web Pages kullanılarak çoğu örnekler C# kullanın. Ancak, Razor sözdizimi Visual Basic de destekler. Program Visual Basic'te bir ASP.NET web sayfası için bir web sayfası oluşturun. bir *.vbhtml* dosya adı uzantısı ve Visual Basic kodunu ekleyin. Bu makalede, ASP.NET Web sayfaları oluşturmak için sözdizimi ve Visual Basic dili ile çalışmaya genel bir bakış sağlar.

> [!NOTE]
> Microsoft WebMatrix için varsayılan Web sitesi şablonları (**Pastane**, **Fotoğraf Galerisi**, ve **başlangıç sitesi**, vb.) C# ve Visual Basic sürümlerinde kullanılabilir. Visual Basic şablonları tarafından NuGet paketleri olarak yükleyebilirsiniz. Adlı bir klasörde, sitenizin kök klasöründe yüklü Web sitesi şablonları *Microsoft Templates*.


## <a name="the-top-8-programming-tips"></a>Üst 8 programlama ipuçları

Bu bölümde kesinlikle Razor sözdizimini kullanan ASP.NET sunucusu kod yazmaya başladığınızda bilmeniz gereken birkaç ipucu listelenir.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Kod kullanarak bir sayfa ekleyin @ karakteri

`@` Karakter satır içi ifadeler, tek deyim blokları ve birden fazla deyim blokları başlatır:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML kodlama**
> 
> Kullanarak bir sayfanın içeriğini görüntülediğinizde `@` karakter, önceki örneklerde olduğu gibi çıkış ASP.NET HTML olarak kodlar. Bu ayrılmış HTML karakterlerini değiştirir (gibi `<` ve `>` ve `&`) karakterlerini HTML etiketleri veya varlıklar yorumlanmasını yerine bir web sayfasında karakter olarak görüntülenecek etkinleştirme kodları ile. Sunucu kodunuz çıktısını HTML kodlaması olmadan düzgün görüntülenmeyebilir ve bir sayfa güvenlik risklerine maruz.
> 
> Amacınız etiketlerini işler biçimlendirmesi olarak HTML biçimlendirmesi çıkış olup olmadığını (örneğin `<p></p>` paragrafı veya `<em></em>` metni vurgulamak için), bölümüne bakın [metin birleştirme, işaretleme ve kod blokları içinde kod](#BM_CombiningTextMarkupAndCode) bu makalenin ilerleyen bölümlerinde.
> 
> Daha fazla bilgi edinebilirsiniz, HTML kodlaması hakkında [ASP.NET Web sayfaları sitelerinde HTML formlarla çalışma](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Kod ile kod blokları içine aldığınız... Bitiş kodu

Bir kod bloğu bir veya daha fazla kod deyimlerini içerir ve anahtar sözcüklerle içine `Code` ve `End Code`. Açılış yerleştirin `Code` anahtar sözcüğü hemen sonra `@` karakter &#8212; olamaz boşluk bunlar arasında.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. Bir blok içinde her kod açıklaması satır sonu ile bitmelidir

Bir Visual Basic kod bloğu içinde her deyim bir satır sonu ile sona erer. (Makalenin sonraki bölümlerinde gerekirse uzun kod deyimi birden fazla satır içine sarmak için bir yol görürsünüz.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Değerleri depolamak için değişkenleri kullanma

Değerleri depolayabilir bir *değişkeni*dizeler, sayılar ve tarihler, vb. dahil olmak üzere. Bir değişken kullanarak yeni oluşturduğunuz `Dim` anahtar sözcüğü. Değişken değerleri doğrudan bir sayfasını kullanarak içinde ekleyebilirsiniz `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Değişmez değer dize değerleri çift tırnak içine alın

A *dize* metin olarak kabul edilir karakterden oluşan bir dizidir. Bir dizeyi belirtmek için çift tırnak içine alın:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Bir dize değeri çift tırnak işaretleri eklemek için iki çift tırnak işareti karakteri Ekle. Çift tırnak karakteri sayfa çıktısında görünmesi istiyorsanız olarak girin `""` tırnak içindeki dize ve iki kez görünmesini istiyorsanız olarak girin `""""` alıntılanmış dize içinde.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Visual Basic kodunu büyük küçük harfe duyarlı değil

Visual Basic dili büyük/küçük harfe duyarlı değildir. Programlama anahtar sözcükleri (gibi `Dim`, `If`, ve `True`) ve değişken adları (gibi `myString`, veya `subTotal`) her durumda yazılabilir.

Aşağıdaki kod satırlarını değişkene bir değer atamak `lastname` bir küçük harf kullanarak adlandırın ve ardından değişken değeri için büyük bir adı kullanarak sayfayı çıktı.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![vb söz dizimi 5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Nesneler ile çalışma kodlamanızı çoğunu içerir

Bir nesne ile programlayabileceğiniz bir şeyi temsil eder &#8212; bir sayfa, bir metin kutusu, bir dosya, görüntü, bir web isteği, bir e-posta iletisi, bir müşteri kaydı (veritabanı satır) vb. Nesnelerin özelliklerini tanımlayan özellikleri vardır &#8212; bir metin kutusu nesnesine sahip bir `Text` bir istek nesnesi özelliğine sahip bir `Url` özelliğine sahip bir e-posta iletisi bir `From` özelliği ve müşteri nesnesi olan bir `FirstName` özellik. Nesneleri yöntemlerle de &quot;fiilleri&quot; yerine getirebilirsiniz. Örnekler, bir dosya nesnesinin `Save` yöntemi, bir görüntü nesnenin `Rotate` yöntemi ve bir e-posta nesnenin `Send` yöntemi.

Genellikle ile çalışacaksınız `Request` ne tür bir tarayıcı, sayfa, kullanıcı kimliği, vb. URL'sini istekte (metin kutuları, vb.) sayfasında alanları form değerleri gibi bilgileri sağlayan nesne. Bu örnek özelliklerine erişmek nasıl gösterir `Request` nesne ve nasıl çağrılacağını `MapPath` yöntemi `Request` sayfasının mutlak yolu sunucu üzerinde size nesnesi:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Kararları kod yazabilirsiniz.

Bir anahtar dinamik web sayfaları ne koşullara göre yapılacağını belirlemek özelliğidir. Bunu yapmak için en yaygın yolu `If` deyimi (ve isteğe bağlı `Else` deyimi).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

Deyim `If IsPost` yazma toplu yoludur `If IsPost = True`. İle birlikte `If` deyimleri, bir yineleme kod bloklarını koşulları test için çeşitli yollar vardır ve bu şekilde, bu makalenin sonraki bölümlerinde açıklanmıştır.

Bir tarayıcıda görüntülenen sonuç (tıkladıktan sonra **Gönder**):

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET ve POST yöntemleri ve IsPost özelliği**
> 
> Çok sınırlı bir dizi yöntem (HTTP) Web sayfaları için kullanılan protokol destekler (&quot;fiilleri&quot;) sunucuya isteklerinde bulunmak için kullanılır. İki en yaygın bir okumak için kullanılan GET ve POST, bir sayfa göndermek için kullanılan olanlardır. Genel olarak, ilk kez bir kullanıcı bir sayfa istediğinde, GET kullanarak sayfa istenmektedir. Kullanıcının bir formda doldurur daha sonra tıklatırsa **Gönder**, tarayıcı, sunucuya bir POST isteği yapar.
> 
> Web programlama, böylece sayfa işleme bildiğiniz bir sayfaya bir GET veya POST olarak istenen olup olmadığını bilmek yararlıdır. ASP.NET Web sayfaları'nda kullanabileceğiniz `IsPost` bir GET veya POST isteği olup olmadığını görmek için özellik. Bir POST isteğiyse `IsPost` özelliği true döndürür ve bir form üzerinde metin kutuları değerlerini okuma gibi şeyler yapabilirsiniz. Birçok örnekler göreceksiniz sayfanın değerine bağlı olarak farklı şekilde işlemek nasıl Göster `IsPost`.


## <a name="a-simple-code-example"></a>Basit bir kod örneğidir

Bu yordam, temel programlama tekniklerini gösteren bir sayfa oluşturma işlemini gösterir. Örnekte, kullanıcılar bunları ekler ve sonucu görüntüler iki sayıyı girin sağlayan bir sayfa oluşturun.

1. Düzenleyicinizde, yeni bir dosya oluşturun ve adlandırın *AddNumbers.vbhtml*.
2. Aşağıdaki kodu ve biçimlendirmeyi sayfasındaki herhangi bir şey değiştirerek sayfasına kopyalayın.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Not yapabileceğiniz bazı işlemler aşağıda verilmiştir:

    - `@` Karakter sayfasında ilk kod bloğunu başlar ve kendisinden `totalMessage` değişkeni altına katıştırılmış.
    - Sayfanın üst kısmındaki blok içine `Code...End Code`.
    - Değişkenleri `total`, `num1`, `num2`, ve `totalMessage` birkaç sayı ve bir dize depolar.
    - Atanan değişmez dize değeri `totalMessage` çift tırnak işareti bulunan bir değişkendir.
    - Visual Basic kod zaman büyük/küçük harfe duyarlı olmadığı için `totalMessage` değişkeni sayfanın altına kullanılıyor, adı yalnızca sayfanın üstündeki değişken bildirimi yazımını ile eşleşmesi gerekiyor. Büyük/küçük harf önemi yoktur.
    - İfade `num1.AsInt()`  +  `num2.AsInt()` nesneleri ve yöntemleri ile çalışma hakkında bilgi verilmektedir. `AsInt` Yöntemi her bir değişken üzerinde eklenebilecek tamsayı (tamsayı) bir kullanıcı tarafından girilen bir dize dönüştürür.
    - `<form>` Etiket içeren bir `method="post"` özniteliği. Kullanıcı tıkladığında bu belirten **Ekle**, sayfanın HTTP POST yöntemini kullanarak sunucuya gönderilir. Ne zaman sayfa gönderildiğinde, kodu `If IsPost` true ve koşullu değerlendirir kod çalıştırır, sayıları eklemenin sonucunu görüntüleme.
3. Sayfayı kaydedin ve bir tarayıcıda çalıştırın. (Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.) İki tam sayılar girin ve ardından **Ekle** düğmesi.

    ![Razor Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic Dil ve sözdizimi

Daha önce bir temel örnek, bir ASP.NET web sayfası oluşturmak nasıl ve sunucu kodunu HTML biçimlendirmesi nasıl ekleyebileceğinizi gördünüz. Burada, Razor sözdizimini kullanan ASP.NET sunucusu kod yazmak için Visual Basic kullanmanın temellerini öğreneceksiniz &#8212; diğer bir deyişle, programlama dili kuralları.

(Özellikle, C, C++, C#, Visual Basic veya JavaScript kullandıysanız) programlama deneyimli değilseniz ne burada okuma çoğunu tanıdık gelecektir. Yalnızca nasıl WebMatrix kod biçimlendirme içinde eklenen ile kendinizi alıştırın gerekecektir *.vbhtml* dosyaları.

### <a id="BM_CombiningTextMarkupAndCode"></a>  Metni İşaretleme ve kod blokları içinde kod birleştirme

Sunucu kod bloklarında genellikle metin ve biçimlendirme sayfasına çıkış isteyebilirsiniz. Sunucusu kod bloğu kod değil ve, bunun yerine olarak işleneceğini metin içeriyorsa, ASP.NET koddan metni ayırt etmek mümkün olması gerekir. Bunu yapmanın birkaç yolu vardır.

- Bir HTML blok öğede gibi metin içine `<p></p>` veya `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    HTML öğesi, metin ve ek HTML öğelerinin sunucu kodu ifadeleri içerebilir. ASP.NET, HTML etiketi açılışı gördüğünde (örneğin, `<p>`), her şeyi öğesi işler ve içeriği olarak olan tarayıcı (ve çözümler sunucu kodu ifadeler).

- Kullanım `@:` işleci veya `<text>` öğesi. `@:` Tek satırlık bir düz metin veya HTML etiketleri eşleşmeyen; içeren içerik çıkarır `<text>` öğesi çıktısını almak için birden fazla satır alır. Bu seçenekler, çıkış bir parçası olarak bir HTML öğesini işlemek istemediğiniz zaman yararlıdır.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    Aşağıdaki örnek, önceki örnekte yineler ancak tek bir çift kullanan `<text>` metin işlemek için etiketler.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    Aşağıdaki örnekte, `<text>` ve `</text>` etiketlerini bazı uncontained metin ve eşleşmeyen HTML etiketleri olması her biri üç satır içine (`<br />`), yanı sıra sunucu kodu ve eşleşen HTML etiketleri. Yine de her satırın tek tek önünde `@:` işleci; her iki şekilde çalışır.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Ne zaman, çıktı metin bu bölümde gösterildiği &#8212; bir HTML öğesini kullanarak `@:` işleci veya `<text>` öğesi &#8212; ASP.NET olmayan HTML olarak kodlanacak çıktı. (Daha önce belirtildiği gibi ASP.NET sunucusu kod ifadeleri tarafından öncelenen sunucusu kod bloğu ve çıkış kodlama `@`, bu bölümde belirtilen özel durumlar hariç.)

### <a name="whitespace"></a>Boşluk

Fazladan boşlukları deyimindeki (ve bir dize sabit değeri dışındaki) deyim etkilemez:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Uzun deyimlerin birkaç satıra yeni

Alt çizgi karakterini kullanarak uzun kod açıklaması birden fazla satıra bölebilir `_` (Visual Basic'te adlandırılan *devamı karakteri*) her kod satırının sonra. Break sonraki satırdaki bir deyimi satırın sonunda boşluk ve devamı karakteri ekleyin. İfade sonraki satırda devam edin. Deyimleri okunabilirliği artırmak gerektiği kadar satırlarına sarabilirsiniz. Aşağıdaki deyimler aynıdır:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Ancak, bir dize sabit değeri ortasında bir satır kaydırma olamaz. Aşağıdaki örnek çalışmaz:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Yukarıdaki kodu gibi çoklu satırlara sarar uzun bir dize birleştirmek için kullanmanız gerekecektir *birleştirme işlecini* (`&`), bu makalenin sonraki bölümlerinde görürsünüz.

### <a name="code-comments"></a>Kod açıklamaları

Açıklama notları kendiniz veya başkaları için terk etmenize imkan. Razor söz dizimi açıklamaları önekiyle `@*` ve ile sona erdi `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Kod blokları içinde Razor söz dizimi açıklamaları kullanabilirsiniz ya da tek bir teklif olan normal Visual Basic açıklama karakter kullanabilirsiniz (`'`) her satır için önek.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Değişkenler

Bir değişken verilerini depolamak için kullandığınız adlandırılmış bir nesnedir. Herhangi bir şey değişkenleri ad verebilirsiniz ancak adı, alfabetik bir karakter ile başlamalı ve boşluk ya da ayrılmış karakterler içeremez. Visual Basic'te, daha önce gördüğünüz gibi büyük/küçük harf bir değişken adı önemli değildir.

### <a name="variables-and-data-types"></a>Değişkenleri ve veri türleri

Ne tür veriler bir değişkende depolandığını belirtir bir özel veri türü bir değişken olabilir. Dize değerleri saklayan string değişkeni olabilir (gibi &quot;Merhaba Dünya&quot;), tam sayı değerleri (örneğin, 3 veya 79) depolayan tamsayı değişkenleri ve çeşitli biçimlerde (4/12/2012 veya 2009 yılı Mart gibi tarih değerlerini depolar tarih değişkenleri ). Ve kullanabileceğiniz diğer birçok veri türleri vardır.

Ancak, bir değişken için bir tür belirtmeniz gerekmez. Çoğu durumda, ASP.NET, out değişkeni içinde verilerin nasıl kullanıldığı temel tür anlayabilir. (Bazen bir türünü belirtmeniz gerekir; bunun true olduğu örnekler göreceksiniz.)

Bir tür belirtmeden bir değişken bildirmek için kullanın `Dim` ayrıca değişken adı (örneğin, `Dim myVar`). Bir türle bir değişken bildirmek için kullanın `Dim` değişken adından, artı `As` ve sonra tür adı (örneğin, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

Aşağıdaki örnek, bir web sayfasında değişkenleri kullanan bazı satır içi ifadeler gösterir.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Bir tarayıcıda görüntülenen sonuç:

![Razor Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Dönüştürme ve veri türleri test etme

ASP.NET genellikle bir veri türü otomatik olarak belirleyebilir, ancak bazen bunu dönüştürülemez. Bu nedenle, ASP.NET açık bir dönüştürme gerçekleştirerek yardımcı olacak birçok gerekebilir. Bazen türleri dönüştürmek yoksa bile, ne tür verilere, ile çalışabilir engellemediğini test etmek için yardımcı olur.

En yaygın bir tamsayı veya tarih gibi bir dizeyi başka bir türe dönüştürmek sahip olduğu. Aşağıdaki örnek, bir sayıyı bir dize burada dönüştürmeniz gerekir tipik bir servis talebi gösterir.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Bir kural olarak, kullanıcı girişini dize olarak size gelir. Bir sayı girin kullanıcıdan ve kullanıcı girişi gönderilir ve kodda okuma zaman bir basamak girdikten bile verileri dize biçiminde bile. Bu nedenle, dizeyi sayıya dönüştürme gerekir. Dönüştürmeden değerleri üzerinde aritmetik işlemleri denerseniz, iki dizeyi ASP.NET eklenemiyor çünkü örnekte, şu hata, sonuçları:

`Cannot implicitly convert type 'string' to 'int'.`

Tam sayılar değerlerini dönüştürmek için çağrı `AsInt` yöntemi. Dönüştürme başarılı olursa, sayıları ekleyebilirsiniz.

Aşağıdaki tablo bazı yaygın dönüştürme ve test yöntemleri değişkenleri listeler.


:::row:::
    :::column:::
        <strong>Yöntemi</strong>
    :::column-end:::
    :::column:::
        <strong>Açıklama</strong>
    :::column-end:::
    :::column:::
        <strong>Örnek</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Bir tamsayı temsil eden bir dize dönüştürür (gibi &quot;593&quot;) bir tamsayı.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Gibi bir dize dönüştürür &quot;true&quot; veya &quot;false&quot; Boole türü.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Gibi ondalık bir değeri içeren bir dize dönüştürür &quot;1.3&quot; veya &quot;7.439&quot; bir kayan noktalı sayı.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Gibi ondalık bir değeri içeren bir dize dönüştürür &quot;1.3&quot; veya &quot;7.439&quot; ondalık bir sayı. (ASP.NET, bir ondalık kayan noktalı sayıdan daha kesin sayıdır.) :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        ASP.NET için bir tarih ve saat değerini temsil eden bir dize dönüştürür `DateTime` türü.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Herhangi bir veri türü, bir dizeye dönüştürür.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a>İşleçler

Bir anahtar sözcük veya ne tür bir ifadede gerçekleştirilecek komut ASP karakter işlecidir. Visual Basic birçok işleçleri destekler, ancak yalnızca ASP.NET web sayfaları geliştirmeye başlamak için birkaç tanıması gerekir. En yaygın işleçleri aşağıdaki tabloda özetlenmiştir.


:::row:::
    :::column:::
        <strong>İşleci</strong>
    :::column-end:::
    :::column:::
        <strong>Açıklama</strong>
    :::column-end:::
    :::column:::
        <strong>Örnekler</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        Sayısal ifadeler kullanılan matematik işleçleri.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Atama ve eşitlik. Bağlama, bağlı olarak ya da sol taraftaki nesneye sağ tarafında bir ifade değeri atar veya eşitlik değerlerini denetler.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Eşitsizlik. Döndürür `True` değerler eşit değilse.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        İkinciden küçük, büyük, küçük veya eşit ve büyüktür veya eşittir.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Birleştirme dizeleri birleştirmek için kullanılır.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        Ekleme ve 1 (sırasıyla) bir değişkenden gelen çıkarmayı artırma ve azaltma işleçleri.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Nokta. Nesneleri ve özellikleri ve yöntemleri ayırt etmek için kullanılır.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parantezler. Grup ifadeleri için yöntemlere ve üyelerine erişim diziler ve Koleksiyonlar parametreleri geçirmek için kullanılır.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Değil. True değeri FALSE ve tersine çevirir. Test etmek için bir toplu şekilde genellikle kullanılan `False` (diğer bir deyişle, için değil `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        Mantıksal AND ve OR koşulları birlikte hangi bağlamak için kullanılır.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Sanal kök başvuran: ~ işleci ve Href yöntemi

İçinde bir *.cshtml* veya *.vbhtml* dosyası, kök sanal yolu kullanarak başvurabilirsiniz `~` işleci. Bu sayfaları bir siteye taşıyabilirsiniz ve içerdikleri diğer sayfalara giden bağlantıları kopmuş olmayacak çünkü çok kullanışlıdır. Ayrıca, hiç olmadığı kadar Web sitenizi farklı bir konuma taşımanız durumunda kullanışlıdır. Bazı örnekler şunlardır:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Web sitesi ise `http://myserver/myapp`, işte sayfa çalıştığında ASP.NET bu yolların nasıl değerlendirir:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Bu yolları aslında değişken değerleri olarak göremezsiniz ancak alacağı ne oldukları olan ASP.NET yolları değerlendirir.)

Kullanabileceğiniz `~` işleci hem de sunucu kodu (yukarıdaki gibi) bu gibi biçimlendirme içinde:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Biçimlendirme içinde kullandığınız `~` yollara görüntü dosyaları, diğer web sayfaları ve CSS dosyaları gibi kaynakları oluşturmak için işleç. Sayfa çalıştığında, ASP.NET (kod ve biçimlendirme) sayfası görünür ve tüm çözümler `~` başvurularını uygun yolu.

## <a name="conditional-logic-and-loops"></a>Koşullu mantık ve döngüler

ASP.NET sunucusu kod koşullar ve bir döngünün çalışan diğer bir deyişle, belirli bir sayıda kod deyimlerini yineler yazma kod göre görevleri gerçekleştirmenize olanak tanıyan).

### <a name="testing-conditions"></a>Test koşulları

Kullandığınız basit bir koşulunu test etmek için `If...Then` döndüren deyimi `True` veya `False` belirttiğiniz bir testi temel alan:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

`If` Anahtar sözcüğü, bir blok başlatır. Gerçek test (durum) izleyen `If` anahtar sözcüğü ve true veya false değerini döndürür. `If` Deyimi ile sona erer `Then`. Sınama true olduğunda çalıştırılacak deyimleri kapsadığı `If` ve `End If`. Bir `If` deyimi içerebilir bir `Else` koşul false ise çalıştırılacak deyimleri belirtir engelle:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Varsa bir `If` deyimi başlatan bir kod bloğu, normal kullanmak zorunda değilsiniz `Code...End Code` blokları içerecek şekilde deyimleri. Yalnızca ekleyebilirsiniz `@` bloğuna ve çalışır. Bu yaklaşım çalışır `If` diğer Visual Basic dahil olmak üzere kod blokları tarafından izlenen anahtar sözcükleri programlama yanı sıra `For`, `For Each`, `Do While`vb.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Bir veya daha fazla kullanarak birden çok koşul ekleyebilirsiniz `ElseIf` blokları:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

Bu örnekte, ilk olarak koşulu, `If` blok true değil `ElseIf` koşul denetlenir. Bu koşul karşılanıyorsa deyimlerinde `ElseIf` bloğu yürütülür. Hiçbir koşul karşılanıyorsa deyimlerinde `Else` bloğu yürütülür. Herhangi bir sayıda ekleyebilirsiniz `ElseIf` engeller ve ile kapatın bir `Else` olarak block &quot;diğer her şey&quot; koşul.

Çok sayıda koşulları test etmek için bir `Select Case` engelle:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Test etmek için parantez (örneğin, hafta içi değişkeni içinde) değerdir. Her bir testi kullanan bir `Case` listeleyen bir değer ifadesi. Varsa değerini bir `Case` deyimi, kodu test değeri ile eşleşen `Case` bloğu yürütülür.

Bir tarayıcıda görüntülenen son iki koşullu blokları sonucu:

![Razor Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Döngü kod

Genellikle aynı deyimleri tekrar tekrar çalıştırmak gerekir. Bunun için döngü. Örneğin, genellikle her öğe için aynı deyimleri verilerinin bir koleksiyonunu çalıştırın. Tam döngü istediğiniz kaç kez biliyorsanız, kullanabileceğiniz bir `For` döngü. Bu tür bir döngü sayımı veya sayım için kullanışlıdır:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Döngü ile başlayan `For` anahtar sözcüğü, üç öğeleri tarafından takip:

- Hemen sonra `For` deyimi bir sayaç değişkeni bildirme (kullanmak zorunda değilsiniz `Dim`) ve ardından de aralık belirtmek `i = 10 to 20`. Değişkeni başka bir deyişle `i` 10'da sayımı Başlat ve 20 (sınırlar dahil) ulaşana kadar devam.
- Arasında `For` ve `Next` deyimleri blok içeriği olan. Bu, yürütülen bir veya daha fazla kod deyimlerini her döngü ile içerebilir.
- `Next i` Deyimi, döngü sona erdirir. Bu sayaç artırılır ve döngünün sonraki yinelemesine başlatır.

Arasında kod satırının `For` ve `Next` satırlarını döngünün her yinelemesinden için çalışan kodu içerir. Yeni bir paragraf biçimlendirme oluşturur (`<p>` öğesi) her zaman ve çıktıyı görüntüleme değeri, bir satır ekler i (sayaç). Bu sayfa çalıştırdığınızda, örnek çıktı, her satırda öğe sayısını gösteren bir metin ile görüntüleme 11 satırları oluşturur.

![Razor Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Bir koleksiyon veya dizi ile çalışıyorsanız, sık kullandığınız bir `For Each` döngü. Bir koleksiyon benzer nesnelerin grubudur ve `For Each` döngü, gerçekleştirdiğiniz koleksiyondaki her öğe üzerinde bir görev sağlar. Bu tür bir döngü koleksiyonlar için uygundur çünkü aksine bir `For` döngü yazmanız gerekmez sayaç artırın veya bir sınır belirleyin. Bunun yerine, `For Each` döngü kodunun işlemi tamamlanana kadar toplulukta yalnızca geçer.

Bu örnekte öğeleri döndürür `Request.ServerVariables` (web sunucunuza hakkında bilgi içeren) koleksiyonu. Bunu kullanan bir `For Each` yeni bir oluşturarak her öğenin adını görüntülemek için döngü `<li>` HTML madde işaretli liste öğesinde.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

`For Each` Anahtar sözcüğü koleksiyondaki tek bir öğeyi temsil eden bir değişken arkasından (örnekte `myItem`) ve ardından `In` döngü koleksiyonu ardından anahtar sözcüğü. Gövdesinde `For Each` döngü, daha önce bildirilen değişken kullanarak geçerli öğeye erişebilir.

![Razor Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Daha genel amaçlı bir döngü oluşturmak için kullanın `Do While` deyimi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Bu döngü ile başlayan `Do While` anahtar sözcüğü, bir koşul tarafından izlenen yinelemek için blok tarafından izlenen. Genellikle döngü Artır (Ekle) veya azaltma (Çıkart) bir değişken veya sayım için kullanılan nesne. Örnekte, `+=` işleci döngü her çalıştığında bir değişken değerine 1 ekler. (Geriye doğru sayan bir döngü içinde bir değişken azaltma için azaltma işleci kullanırsınız `-=`.)

## <a name="objects-and-collections"></a>Nesneler ve Koleksiyonlar

ASP.NET Web sitesi içinde hemen her şeyi web sayfası dahil olmak üzere, bir nesne var. Bu bölümde, ile sık kodunuzda çalışabilecek önemli bazı nesneler açıklanmaktadır.

### <a name="page-objects"></a>Sayfa nesneleri

En temel ASP.NET sayfası nesnedir. Herhangi bir hak kazanan nesnesi olmadan doğrudan sayfası nesnenin özelliklerine erişebilirsiniz. Aşağıdaki kod sayfanın dosya yolunu alır kullanarak `Request` sayfasının nesne:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Özelliklerini kullanabilirsiniz `Page` nesne gibi çok bilgi almak için:

- `Request`. Bu önceden gördüğünüz gibi ne tür bir tarayıcı yapılan istek URL'sini sayfa, kullanıcı kimliği, vb. dahil olmak üzere, geçerli istek hakkındaki bilgiler koleksiyonudur.
- `Response`. Sunucu kodu çalıştırma bittiğinde, tarayıcıya gönderilen yanıt (sayfa) hakkında bilgi koleksiyonudur. Örneğin, yanıtınıza yazmak için bu özelliği kullanabilirsiniz.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Koleksiyon nesnelerini (diziler ve sözlükleri)

Bir koleksiyonu, koleksiyonu gibi aynı türden nesneleri grubudur `Customer` veritabanı nesneleri. ASP.NET içeren pek çok yerleşik koleksiyonlar gibi `Request.Files` koleksiyonu.

Genellikle, koleksiyonları verilerle çalışırsınız. İki ortak koleksiyon türü *dizi* ve *sözlük*. Bir dizi benzer öğeleri koleksiyonu depolamak istediğiniz ancak her bir öğe tutmak için ayrı bir değişken oluşturmak istemeyen yararlı olur:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Dizilerle, bir özel veri türü gibi bildirdiğiniz `String`, `Integer`, veya `DateTime`. Bir dizi değişkeni içerebilir, Değişken bildiriminde parantez ekleyin belirtmek için (gibi `Dim myVar() As String`). Öğeleri konumlarına (dizin) kullanarak bir diziye veya kullanarak erişebileceğiniz `For Each` deyimi. Dizi dizinleri sıfır tabanlı &#8212; diğer bir deyişle, birinci öğenin konumundaki konumlandırın 0, ikinci öğesi konum 1 ve benzeri.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Bir dizideki öğe sayısını alarak belirleyebilirsiniz, `Length` özelliği. Dizideki belirli bir öğenin konumunu almak için (diğer bir deyişle, diziyi aramak için), kullanın `Array.IndexOf` yöntemi. Bir dizinin içeriğini de ters gibi şeyler yapabilirsiniz ( `Array.Reverse` yöntemi) veya içeriği sıralama ( `Array.Sort` yöntemi).

Bir tarayıcıda görüntülenen dize dizisi kodun çıktısı:

![Razor Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Bir anahtar/değer çifti koleksiyonunu ayarlayın veya karşılık gelen değeri almak için anahtar (veya ad) sağlarsınız sözlüktür:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Bir sözlüğü oluşturmak için kullandığınız `New` yeni oluşturuyorsanız göstermek için anahtar sözcüğü `Dictionary` nesne. Bir değişken kullanarak bir sözlük atayabilirsiniz `Dim` anahtar sözcüğü. Veri türleri parantez kullanılarak Sözlüğü'ndeki öğelerin kullanımını gösterir ( `( )` ). Bildirimin sonunda, bu gerçekte yeni bir sözlük oluşturur bir yöntem olduğundan başka bir parantez çifti eklemeniz gerekir.

Öğeleri sözlüğüne eklenecek çağırabilirsiniz `Add` sözlük değişkeniyle yöntemi (`myScores` bu durumda) ve ardından bir anahtar ve değer belirtin. Alternatif olarak, anahtarı belirtmek ve aşağıdaki örnekte olduğu gibi basit bir atama yapmak için parantez kullanın:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Sözlükten bir değer almak için parantez içinde anahtarı belirtin:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Parametrelere sahip yöntemleri çağırma

Bu makalenin önceki bölümlerinde anlatıldığı gibi ile program nesneleri yöntemi vardır. Örneğin, bir `Database` nesne içerebilir bir `Database.Connect` yöntemi. Bir veya daha fazla parametre birçok yöntem de var. A *parametresi* bir yöntem için geçirdiğiniz değerdir, görevini tamamlamak üzere yöntemi etkinleştirmek için. Örneğin, bakmak için bir bildirim `Request.MapPath` üç parametre almayan yöntemi:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Bu yöntem, belirtilen sanal yolu sunucuda karşılık gelen fiziksel yolu döndürür. Yönteminin üç parametreler `virtualPath`, `baseVirtualDir`, ve `allowCrossAppMapping`. (Bildiriminde kabul verilerin veri türleriyle parametreleri listelendiğine dikkat edin.) Bu yöntemi çağırdığınızda, tüm üç parametrelerin değerlerini belirtmeniz gerekir.

Razor sözdizimi olan Visual Basic kullanıyorsanız, bir yönteme parametreleri geçirmek için iki seçeneğiniz vardır: *konumsal parametreler* veya *adlandırılmış parametreleri*. Konumsal parametreler kullanan bir yöntem çağırmak için yöntem bildiriminde belirtilen katı bir sırayla parametreleri geçirin. (Genellikle bu sırada yöntemi için belgeleri okuyarak bilmeniz.) Sırayı izlemelidir ve herhangi bir parametre atlayamazsınız &#8212; gerekiyorsa, boş bir dize geçirirseniz (`""`) veya için bir değer yoksa, konumsal bir parametre için null.

Aşağıdaki örnekte adlı bir klasör olduğunu varsayar *betikleri* kullanarak Web sitenizde. Kod çağrıları `Request.MapPath` doğru sırada üç parametre değerlerini yöntemi ve geçirir. Ardından, elde edilen eşlenen yolun görüntüler.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Bir yöntem parametre fazla olduğunda, adlandırılmış parametreleri kullanarak daha temiz ve daha okunabilir kodunuzu tutabilirsiniz. Adlandırılmış parametreler kullanılarak bir yöntemi çağırmak için parametre adından belirtin `:=` ve sonra değerini sağlayın. Adlandırılmış parametreler bir avantajı, bunları istediğiniz herhangi bir sırada ekleyebilirsiniz sağlamasıdır. (Bir dezavantajı yöntem çağrısında kompakt olmasıdır.)

Aşağıdaki örnek, yukarıdakilerle aynı yöntemini çağırır, ancak adlı değerlerini sağlamak için parametreleri kullanır:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Gördüğünüz gibi farklı parametreler geçirilir. Ancak, önceki örnekte ve bu örneği çalıştırırsanız, aynı değeri döneceksiniz.

## <a name="handling-errors"></a>Hataları işleme

### <a name="try-catch-statements"></a>Try-Catch deyimleri

Denetiminiz dışındaki bir nedenle başarısız olabilir, kodunuzda genellikle deyimleri sahip olacaksınız. Örneğin:

- Kodunuzu açmak, oluşturmak, okumak veya bir dosyaya yazmak çalışırsa, her türlü hata oluşabilir. İstediğiniz dosyayı var olmayabilir, kilitli olabilir, kod değil izinlere sahip ve benzeri.
- Benzer şekilde, veritabanındaki kayıtları güncelleştirmek kodunuzu çalışırsa, izin sorunları olabilir, veritabanı bağlantısını bırakılabilir, kaydetmek için verileri geçersiz vb. olabilir.

Programlama dilinde, bu gibi durumlarda çağrılır *özel durumları*. Kodunuz bir özel durumla karşılaşırsa (oluşturur) oluşturur. bir hata iletisi diğer bir deyişle, en iyi kullanıcılara sinir bozucu.

![Razor Img14](introducing-razor-syntax-vb/_static/image14.jpg)

Durumlarda burada kodunuzu çalıştırdığınızca özel durumlar ve hata iletileri bu tür önlemek için kullanabileceğiniz `Try/Catch` deyimleri. İçinde `Try` deyimi, denetimi kodu çalıştırın. Bir veya daha fazla `Catch` deyimleri için özel konum (belirli tür özel durumlar) hatalar oluşmuş olabilir. Kadar içerebilir `Catch` kapasitesinden hataları aramak ifadeleri yazarken gerekir.

> [!NOTE]
> Kullanmaktan kaçının öneririz `Response.Redirect` yönteminde `Try/Catch` deyimleri, bir özel durum sayfanızın neden olabileceği için.


Aşağıdaki örnek ilk isteğe bir metin dosyası oluşturur ve ardından kullanıcının dosyayı açma sağlayan bir düğme görüntüleyen bir sayfa görüntülenir. Örnek kasıtlı olarak hatalı dosya adı kullanır, böylece bir özel durum neden olur. Kodu içerir `Catch` deyimleri için iki olası özel durumları: `FileNotFoundException`, dosya adı hatalı olması durumunda gerçekleşir ve `DirectoryNotFoundException`, ASP.NET bile klasörü bulamıyorsanız gerçekleşir. (, Örnek bir deyimde her şeyin düzgün çalıştığından, nasıl çalıştığını görmek için açıklama durumundan çıkarabilirsiniz.)

Kodunuzu özel durumu işlemek istemediğiniz ederseniz, önceki ekran görüntüsündeki gibi bir hata sayfası görürsünüz. Ancak, `Try/Catch` bölüm, kullanıcı bu tür hataları görmemesi yardımcı olur.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

### <a name="reference-documentation"></a>Başvuru belgeleri

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic dili](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
