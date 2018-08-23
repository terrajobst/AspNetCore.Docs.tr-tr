---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
title: Ana sayfalar (C#) iç içe geçmiş | Microsoft Docs
author: rick-anderson
description: İçindeki başka bir ana sayfa iç içe işlemi gösterilmektedir.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 32b7fb6e-d74b-4048-91f8-70631b2523ee
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1b60b0b7ce4be66bc24ccbc1d25ce4dc56766815
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754926"
---
<a name="nested-master-pages-c"></a>İç içe geçmiş ana sayfalar (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.pdf)

> İçindeki başka bir ana sayfa iç içe işlemi gösterilmektedir.


## <a name="introduction"></a>Giriş

Ana sayfalar ile site geneli bir düzen uygulama son dokuz eğitim kursunda gördük. İçinde koysalar ana sayfalar, ana sayfa içerik sayfası içerik sayfası temelinde özelleştirilebilir özel bölgeler ile birlikte ortak biçimlendirme tanımlamak için sayfa Geliştirici olanak tanır. Ana sayfa ContentPlaceHolder denetimlerinde özelleştirilebilir bölgeleri gösterir; ContentPlaceHolder denetimleri için özelleştirilmiş biçimlendirme, içerik sayfası içerik denetimleri aracılığıyla tanımlanır.

Tüm site kullanılan tek bir düzen varsa, size bugüne kadarki incelediniz sayfayı teknikleri mükemmeldir. Ancak birçok büyük Web siteleri, çeşitli bölümlerde özelleştirilmiş bir site düzeni vardır. Örneğin, hasta bilgilerine, etkinlikler ve faturalandırma yönetmek için hastane personeli tarafından kullanılan bir sağlık hizmetleri uygulamasını göz önünde bulundurun. Bu uygulamadaki web sayfaları üç türde olabilir:

- Personel kullanılabilirlik, burada güncelleştirebilirsiniz ekibi üyesi özgü sayfa zamanlamaları görüntüleyemezsiniz veya istek tatil süresi.
- Hasta özgü sayfa personel burada görüntülemek veya belirli bir Hasta için bilgileri düzenleyin.
- Faturalandırma özgü sayfa müşavirler burada geçerli gözden geçirme talep durumları ve finansal raporlar.

Her sayfanın üst ve alt kısmında bulunan sık kullanılan bağlantılar bir dizi menü gibi yaygın bir düzen paylaşabilir. Ancak, personel, hasta- ve faturalandırma özgü sayfa bu genel bir düzeni özelleştirme yapmanız gerekebilir. Örneğin, belki de tüm personel özgü sayfa şu anda oturum açmış kullanıcının kullanılabilirlik ve günlük zamanlama gösteren bir takvim ve görev listesini içermelidir. Ad, adres ve sigorta, bilgileri düzenleniyor hasta bilgilerini göstermek, belki de tüm hasta özgü sayfa gerekir.

Kullanarak bu tür özel düzenler oluşturmak mümkündür *içe geçmiş ana sayfalar*. Yukarıdaki senaryoyu uygulamak için biz site genelinde düzen, menüsünü ve alt bilgi içeriği özelleştirilebilir bölgeleri tanımlama ContentPlaceHolder ile tanımlanan bir ana sayfa oluşturarak başlarsınız. Biz, sonra üç iç içe geçmiş ana sayfalar, bir web sayfasının her türü için oluşturursunuz. Her iç içe geçmiş ana sayfa içerik ana sayfa kullanan içerik sayfalarını türünde arasında tanımlarsınız. Diğer bir deyişle, hasta özgü içerik sayfaları için iç içe geçmiş ana sayfa biçimlendirmesi ve düzenlenmekte olan hasta hakkında bilgi görüntülemek için programlama mantığını içerir. Yeni bir Hasta özgü sayfa oluştururken size, bu iç içe geçmiş ana sayfa için bağlayın.

Bu öğreticide, iç içe geçmiş ana sayfalar avantajlarını vurgulayarak başlatır. Ardından, oluşturma ve iç içe geçmiş ana sayfalar kullanma işlemini göstermektedir.

> [!NOTE]
> İç içe geçmiş ana sayfalar, .NET Framework'ün 2.0 sürümünden başlayarak mümkün olmuştur. Ancak, Visual Studio 2005 iç içe geçmiş ana sayfalar için tasarım zamanı desteğini içermiyordu. Güzel bir haberimiz var Visual Studio 2008 iç içe geçmiş ana sayfalar için zengin bir tasarım zamanı deneyimi sahip olmasıdır. İç içe geçmiş ana sayfalar kullanmak istiyorsanız, ancak yine de Visual Studio 2005 kullanıyorsanız, kullanıma [Scott Guthrie](https://weblogs.asp.net/scottgu/)ın blog girişine [VS 2005 tasarım zamanı iç içe geçmiş ana sayfalar için ipuçları](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>İç içe geçmiş ana sayfalar avantajları

Birçok Web sitesi, ıpam'da bir site tasarımı ve bunun yanı sıra daha özelleştirilmiş tasarımları sayfaların belirli türler için belirli sahiptir. Örneğin, demo web uygulamamıza ilkel bir yönetim bölümündeki oluşturduk (sayfalarında `~/Admin` klasörü). Şu anda web sayfaları'nda `~/Admin` klasörü yönetim bölümündeki bu sayfaları olarak aynı ana sayfaya kullanın (yani, `Site.master` veya `Alternate.master`kullanıcının seçimine bağlı olarak).

> [!NOTE]
> Şimdilik sitemizi yalnızca bir ana sayfa olduğunu anlatabilirsiniz `Site.master`. Bu öğreticinin ilerleyen bölümlerinde "Kullanarak bir iç içe geçmiş ana sayfa için yönetim bölümündeki ile" başlayarak iki (veya daha fazla) ana sayfalar ile iç içe geçmiş ana sayfalar kullanarak adresi.


Biz ek bilgiler veya aksi halde sitedeki diğer sayfalarında olmaz bağlantılar eklemek için yönetim sayfaların düzenini özelleştirmek için sorulan olduğunu hayal edin. Bu gereksinimin uygulanması için dört teknik vardır:

1. El ile içerik her sayfa için yönetim özgü bilgileri ve bağlantılar ekleme `~/Admin` klasör.
2. Güncelleştirme `Site.master` Yönetim bölümünde özgü bilgileri ve bağlantılar içerir ve kod göster veya gizle bu bölümleri için ana sayfasına eklemek için ana sayfa tabanlı olup yönetim sayfalardan birine ziyaret edilen üzerinde.
3. Yeni bir ana sayfa oluşturma yönetim bölümündeki özellikle için biçimlendirmeden kopyalayabilirsiniz `Site.master`Yönetim bölümünde özgü bilgileri ve bağlantılar ekleme ve ardından içerik sayfalarında güncelleştirme `~/Admin` bu yeni bir şablonu kullanmak için klasör Sayfa.
4. Bağlamalarını kaydetmek için bir iç içe geçmiş ana sayfa oluşturma `Site.master` ve içerik sayfalarını olmayan `~/Admin` klasör kullanımı bu yeni iç içe geçmiş ana sayfa. Bu iç içe geçmiş ana sayfa yalnızca ek bilgi ve bağlantılar yönetim sayfalarına belirli içerir ve önceden tanımlanmış biçimlendirme yinelemek zorunda kalmaz `Site.master`.

İlk seçenek az palatable olacaktır. Ana sayfaları kullanarak tüm genel biçimlendirme yeni ASP.NET sayfaları için el ile kopyalayıp gerek kalmadan uzağa taşımak noktasıdır. İkinci seçenek kabul edilebilir olmakla birlikte, uygulamayı, nadiren görüntülenir ve ana sayfayı düzenleme geliştiriciler bu biçimlendirme geçici olarak çalışır ve ne zaman hatırlamak zorunda gerektiren biçimlendirme Ana sayfalarla'kurmak toplu siparişler gibi daha sürdürülebilir hale getirir, gizli olduğunda bazı biçimlendirme ve tam olarak görüntülenir. Bu yaklaşım, bu tek bir ana sayfa tarafından barındırılabilmesi için gereken web sayfalarına giderek daha fazla tür özelleştirmeleri daha az tenable olacaktır.

Üçüncü seçenek dağınıklığı ortadan kaldırır ve karmaşıklığı ile ikinci seçenek surfaced verir. Bununla birlikte, üç kişinin seçeneği asıl sakıncası bize ortak düzenden kopyalayıp gerektirir `Site.master` yeni Yönetim bölümünde özgü ana sayfaya. Biz daha sonra site geneli bir düzen değiştirmeye karar verirseniz iki yerde değiştirmeyi unutmayın gerekir.

Dördüncü seçeneği, iç içe geçmiş ana sayfalar ve ikinci ve üçüncü seçenek en iyi iletin. Farklı dosyalarına ayrılmış belirli bölgelere özel içeriği sırada bir dosyada - üst düzey ana sayfası - site geneli bir düzen bilgileri korunur.

Bu öğreticide bir görünüm oluşturma ve basit bir iç içe geçmiş ana sayfa kullanarak başlar. Yeni bir üst düzey ana sayfa, iki iç içe geçmiş ana sayfalar ve iki içerik sayfalarını oluştururuz. "Kullanarak bir iç içe geçmiş ana sayfa için yönetim bölümündeki ile" başlayarak, iç içe geçmiş ana sayfalar kullanımı dahil etmek için var olan ana sayfa mimarimiz güncelleştirme sırasında bakacağız. Özellikle, biz bir iç içe geçmiş ana sayfa oluşturma ve içerik sayfalarındaki için ek özel içerik dahil etmek kullanma `~/Admin` klasör.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>1. adım: Basit ve en üst düzey bir ana sayfa oluşturma

İç içe geçmiş ana oluşturma var olan bir ana birine sayfaları dayalı ve ardından bu yeni iç içe geçmiş ana sayfası yerine en üst düzey ana sayfa kullanmak için mevcut bir içerik sayfasını güncelleştirme karmaşıklık mevcut içerik sayfalarıyla zaten belirli beklediğiniz çünkü kapsar Üst düzey ana sayfasında tanımlanan ContentPlaceHolder denetler. Bu nedenle, iç içe geçmiş ana sayfa aynı ada sahip aynı ContentPlaceHolder denetimleri de dahil etmelisiniz. Ayrıca, belirli bir tanıtım uygulamamız iki ana sayfa vardır (`Site.master` ve `Alternate.master`) dinamik olarak atanmış Bu karmaşıklığı için daha fazla ekleyen, kullanıcı tercihleri temelinde bir içerik sayfası. Biz iç içe geçmiş ana sayfalar, bu öğreticinin ilerleyen bölümlerinde kullanmak için var olan uygulamayı güncelleştirme sırasında görünür ancak şimdi ana sayfalar örnek ilk odaklanmak basit bir iç içe geçmiş.

Adlı yeni bir klasör oluşturun `NestedMasterPages` ve ardından yeni bir ana sayfa dosyası adlı klasöre ekleyin `Simple.master`. (Şekil 1 Bu klasör ve dosya eklendikten sonra çözüm Gezgini'nin ekran görüntüsü için bkz.) Sürükleme `AlternateStyles.css` tasarımcıya Çözüm Gezgini'nden stil sayfası dosyası. Bu ekler bir `<link>` stil sayfası dosyasındaki öğesine `<head>` öğesi, hangi sonra ana sayfanın `<head>` öğenin biçimlendirme gibi görünmelidir:


[!code-aspx[Main](nested-master-pages-cs/samples/sample1.aspx)]

Ardından Web formu içinde aşağıdaki işaretlemeyi ekleyin `Simple.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample2.aspx)]

Bu işaretleme, Lacivert arka plan üzerinde beyaz büyük yazı sayfanın üst kısmındaki "İç içe geçmiş ana sayfalar (Basit)" başlıklı bir bağlantı gösterilir. Altında olan `MainContent` ContentPlaceHolder. Şekil 1 gösterir `Simple.master` Visual Studio Tasarımcısı'nda yüklendiğinde ana sayfa.


[![İç içe geçmiş ana sayfa içerik özel yönetim bölümündeki sayfaları tanımlar.](nested-master-pages-cs/_static/image2.png)](nested-master-pages-cs/_static/image1.png)

**Şekil 01**: iç içe geçmiş ana sayfa tanımlar içerik belirli yönetim bölümündeki sayfaları ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>2. adım: basit bir iç içe geçmiş ana sayfa oluşturma

`Simple.master` iki ContentPlaceHolder denetimler de içerir: `MainContent` ekledik ile birlikte Web formu içinde ContentPlaceHolder `head` ContentPlaceHolder ' `<head>` öğesi. İçerik sayfası oluşturur ve öğeyi olsaydık `Simple.master` içerik sayfasını iki ContentPlaceHolder başvuran iki içerik denetimleri gerekir. Benzer şekilde, biz bir iç içe geçmiş ana sayfa oluşturma ve öğeyi `Simple.master` iç içe geçmiş ana sayfa iki içerik denetimi olacaktır.

Yeni bir iç içe geçmiş ana sayfa için ekleyelim `NestedMasterPages` adlı klasöre `SimpleNested.master`. Sağ `NestedMasterPages` klasörü ve Yeni Öğe Ekle öğesini seçin. Bu Şekil 2'deki yeni öğe Ekle iletişim kutusu getirir. Yeni ana sayfa adını yazın ve ana sayfa şablon türünü seçin. Ana sayfada bir iç içe geçmiş ana sayfa olması gerektiğini belirtmek için "Ana sayfa seçin" onay kutusunu kontrol edin.

Ardından, Ekle düğmesine tıklayın. Bu aynı seçin bir içerik sayfasının bir ana sayfasına bağlanırken gördüğünüz bir ana sayfa iletişim kutusu görüntülenir (bkz: Şekil 3). Seçin `Simple.master` ana sayfasında `NestedMasterPages` klasörü ve Tamam'a tıklayın.

> [!NOTE]
> ASP.NET Web sitesi yerine Web sitesi proje modeli Web uygulaması projesi modelini kullanarak oluşturduysanız, Şekil 2'deki yeni öğe Ekle iletişim kutusunda "ana sayfa seçin" onay kutusunu görmezsiniz. Web uygulaması projesi modeli kullanılırken bir iç içe geçmiş ana sayfa oluşturma (ana sayfa şablonu) yerine iç içe geçmiş ana sayfa şablon seçmeniz gerekir. İç içe geçmiş ana sayfa şablonunu seçme ve Ekle seçeneğine tıkladıktan sonra aynı Şekil 3'te gösterilen iletişim kutusu görünür ana sayfa seçin.


[![Denetleme &quot;Select ana sayfa&quot; iç içe geçmiş ana sayfa eklemek için onay kutusu](nested-master-pages-cs/_static/image5.png)](nested-master-pages-cs/_static/image4.png)

**Şekil 02**: iç içe geçmiş ana sayfa eklemek için "ana sayfa seçin" onay kutusunu işaretleyin ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image6.png))


[![İç içe geçmiş ana sayfa Simple.master ana sayfasına bağlama](nested-master-pages-cs/_static/image8.png)](nested-master-pages-cs/_static/image7.png)

**Şekil 03**: iç içe geçmiş ana sayfa için bağlama `Simple.master` ana sayfa ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image9.png))


İç içe geçmiş ana sayfa bildirim temelli biçimlendirme, aşağıda gösterilen üst düzey ana sayfanın iki ContentPlaceHolder denetimlere başvurma iki içerik denetimlerini içerir.


[!code-aspx[Main](nested-master-pages-cs/samples/sample3.aspx)]

Dışında `<%@ Master %>` yönergesi, iç içe geçmiş ana sayfa ilk bildirim temelli biçimlendirme başlangıçta aynı üst düzey ana sayfa için içerik sayfası bağlama sırasında oluşturulan biçimlendirmeyi aynıdır. Gibi bir içerik sayfasının `<%@ Page %>` yönergesi `<%@ Master %>` burada yönergesini içeren bir `MasterPageFile` iç içe geçmiş ana sayfa üst ana sayfa belirten özniteliği. İç içe geçmiş ana sayfa ve aynı üst düzey ana sayfaya bağlı bir içerik sayfasının arasındaki temel fark, iç içe geçmiş ana sayfa ContentPlaceHolder denetimleri içerebilir ' dir. İçerik sayfaları biçimlendirme nerede özelleştirebilirsiniz bölgeleri iç içe geçmiş ana sayfa ContentPlaceHolder denetimleri tanımlar.

Bu iç içe geçmiş ana sayfa "SimpleNested gelen, Hello!" metni görüntüler için güncelleştirin karşılık gelen içerik denetiminde `MainContent` ContentPlaceHolder denetimi.


[!code-aspx[Main](nested-master-pages-cs/samples/sample4.aspx)]

Bu ayrıca yaptıktan sonra iç içe geçmiş ana sayfa kaydedin ve ardından yeni bir içerik sayfasına ekleme `NestedMasterPages` adlı klasöre `Default.aspx`ve öğeyi `SimpleNested.master` ana sayfa. Bu sayfa ekleme sırasında hiçbir içerik denetimlerini (bkz: Şekil 4) içerip içermediğini Şaşırmış olabilir! İçerik sayfası yalnızca erişip kendi *üst* sayfanın ContentPlaceHolder Yöneticisi. `SimpleNested.master` herhangi bir ContentPlaceHolder denetim içermez. Bu nedenle, bu ana sayfaya bağlı herhangi bir içerik sayfasında herhangi bir içerik denetimleri içeremez.


[![Yeni içerik sayfası hiçbir içerik denetimlerini içerir.](nested-master-pages-cs/_static/image11.png)](nested-master-pages-cs/_static/image10.png)

**Şekil 04**: yeni içerik sayfası içeren Hayır içerik denetimleri ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image12.png))


İç içe geçmiş ana sayfa güncelleştirme yapmak ihtiyacımız olan (`SimpleNested.master`) ContentPlaceHolder denetimler eklemek için. Genellikle bir ContentPlaceHolder böylece onun alt ana sayfa veya herhangi bir üst düzey ana sayfanın ContentPlaceHolder ile çalışmak için içerik sayfası izin vererek onun üst ana sayfası tarafından tanımlanan her ContentPlaceHolder eklemek için iç içe geçmiş ana sayfalar isteyeceksiniz. denetimler.

Güncelleştirme `SimpleNested.master` iki içerik denetimlerini bir ContentPlaceHolder eklemek için ana sayfa. ContentPlaceHolder denetimleri kendi içerik denetimi başvurduğu ContentPlaceHolder denetimi aynı adı verin. Diğer bir deyişle, adlı ContentPlaceHolder denetim ekleme `MainContent` içeriği denetlemek de `SimpleNested.master` başvuran `MainContent` ContentPlaceHolder ' `Simple.master`. Başvuran içerik denetimi kullanarak aynı şeyi yapmak `head` ContentPlaceHolder.

> [!NOTE]
> Bu adlandırma Simetri ı iç içe geçmiş ana sayfa ContentPlaceHolder denetimlerinde ContentPlaceHolder en üst düzey ana sayfasında aynı adlandırma önerilir, ancak gerekli değildir. İç içe geçmiş ana sayfanızda ContentPlaceHolder denetimleri istediğiniz adı verebilirsiniz. Ancak, ı ContentPlaceHolder ile karşılık unutmayın kolay my üst düzey ana sayfa ve iç içe geçmiş ana sayfalar aynı adları kullanıyorsanız sayfanın hangi bölgelerde.


Bu eklemeler yaptıktan sonra `SimpleNested.master` bildirim temelli biçimlendirme ana sayfanın aşağıdakine benzer görünmelidir:


[!code-aspx[Main](nested-master-pages-cs/samples/sample5.aspx)]

Silme `Default.aspx` içerik sayfası oluşturduğumuz ve sonra ona bağlama yeniden eklemek `SimpleNested.master` ana sayfa. Bu kez Visual Studio iki ekler içerik denetimleri `Default.aspx`, ContentPlaceHolder başvuran artık tanımlanan `SimpleNested.master` (bkz. Şekil 6). "Hello, Default.aspx gelen!" metni Ekle Başvurulan içeriği denetleyen `MainContent`.

Şekil 5, burada - söz konusu üç varlıkları gösterir `Simple.master`, `SimpleNested.master`, ve `Default.aspx` - ve birbirleriyle nasıl ilişki kuracağını. Diyagramda gösterildiği gibi iç içe geçmiş ana sayfa içerik denetimleri için üst öğesinin ContentPlaceHolder uygular. Bu bölgeler için içerik sayfası erişilebilir olması gerekiyorsa, iç içe geçmiş ana sayfa İçerik denetimlerine kendi ContentPlaceHolder eklemeniz gerekir.


[![İçerik sayfasının düzenini en üst düzey ve iç içe geçmiş ana sayfalar dikte](nested-master-pages-cs/_static/image14.png)](nested-master-pages-cs/_static/image13.png)

**Şekil 05**: içerik sayfasının düzenini en üst düzey ve iç içe geçmiş ana sayfalar dikte ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image15.png))


Bu davranışı nasıl içerik sayfası veya ana sayfa yalnızca kendi üst ana sayfasını cognizant olduğunu gösterir. Bu davranış, ayrıca Visual Studio tasarımcısı tarafından belirtilir. Şekil 6 için tasarımcı gösterir `Default.aspx`. İçerik sayfasından hangi bölgelerde düzenlenebilir ve hangi kısımları olmayan Tasarımcı açıkça gösterilmektedir, ancak iç içe geçmiş ana sayfa düzenlenemez bölgeleri nelerdir ve üst düzey ana sayfadan bölgeleri nelerdir belirsizliğinin değil.


[![İçerik sayfası şimdi içerik denetimleri için iç içe geçmiş ana sayfa ContentPlaceHolder içerir.](nested-master-pages-cs/_static/image17.png)](nested-master-pages-cs/_static/image16.png)

**Şekil 06**: içerik sayfası artık içeren içerik denetimleri için iç içe geçmiş ana sayfa ContentPlaceHolder ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>3. adım: ikinci basit iç içe geçmiş ana sayfa ekleme

İç içe geçmiş ana sayfalar avantajı, birden çok iç içe geçmiş ana sayfalar olduğunda daha dikkati çekiyor. Bu Avantajdan göstermek için başka bir iç içe geçmiş ana sayfa oluşturma `NestedMasterPages` klasörü; adı bu yeni iç içe geçmiş ana sayfa `SimpleNestedAlternate.master` ve öğeyi `Simple.master` ana sayfa. Adım 2'de yaptığımız gibi iç içe geçmiş ana sayfa iki içerik denetimlerini ContentPlaceHolder denetimleri ekleyin. Ayrıca "Hello, SimpleNestedAlternate gelen!" metni Ekle üst düzey ana sayfa için karşılık gelen içerik denetiminde `MainContent` ContentPlaceHolder. Bu değişiklikleri yaptıktan sonra bildirim temelli biçimlendirme, yeni iç içe geçmiş ana sayfanın aşağıdakine benzer görünmelidir:


[!code-aspx[Main](nested-master-pages-cs/samples/sample6.aspx)]

Bir içerik sayfasının adlandırılmış oluşturma `Alternate.aspx` içinde `NestedMasterPages` klasörü ve öğeyi `SimpleNestedAlternate.master` iç içe geçmiş ana sayfa. "Hello, diğer gelen!" metni Ekle karşılık gelen içerik denetiminde `MainContent`. Şekil 7 gösterir `Alternate.aspx` Visual Studio tasarımcısı görüntülendiğinde.


[![Alternate.aspx SimpleNestedAlternate.master ana sayfaya bağlanır](nested-master-pages-cs/_static/image20.png)](nested-master-pages-cs/_static/image19.png)

**Şekil 07**: `Alternate.aspx` bağlı `SimpleNestedAlternate.master` ana sayfa ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image21.png))


Şekil 7'ye tasarımcıda bir Şekil 6 tasarımcıda karşılaştırın. Üst düzey ana sayfasında tanımlanan aynı düzeni hem içerik sayfalarını paylaşma (`Simple.master`), yani "İç içe geçmiş ana sayfalar öğretici (Basit)" başlığı. Her ikisi de kendi üst ana sayfaları - tanımlanan farklı içerik "SimpleNested gelen, Hello!" metni henüz Şekil 6 ve "Den SimpleNestedAlternate, Hello!" Şekil 7'de. Verildiyse bu farklar burada basit, ancak bu örnek daha anlamlı farklar içerecek şekilde genişletebilirsiniz. Örneğin, `SimpleNested.master` sayfası, içerik sayfalarını belirli seçenekleri içeren bir menü içerebilir, ancak `SimpleNestedAlternate.master` bağlamak için içerik sayfalarına testlerinizle ilgili olabilecek bilgilere sahip olabilir.

Şimdi, ıpam'da site düzenine değişiklik yapmak ihtiyacımız düşünün. Örneğin, tüm içerik sayfalarına ortak bağlantıların listesini eklemek istedik düşünün. En üst düzey ana sayfa güncelleştiriyoruz bunu sağlamak için `Simple.master`. Burada tüm değişiklikler hemen kendi iç içe geçmiş ana sayfalar ve uzantısı, bunların içerik sayfalarını yansıtılır.

Hangi biz değiştirebilirsiniz ıpam'da site düzenini bir kolayca göstermek için açık `Simple.master` arasında aşağıdaki işaretlemeyi ekleyin ve ana sayfa `topContent` ve `mainContent` `<div>` öğeleri:


[!code-aspx[Main](nested-master-pages-cs/samples/sample7.aspx)]

Bu iki bağlantı bağlar her sayfanın üst kısmında ekler `Simple.master`, `SimpleNested.master`, veya `SimpleNestedAlternate.master`; tüm iç içe geçmiş ana sayfalar ve bunların içerik sayfalarını bu değişiklikler hemen uygulanır. Şekil 8 gösterir `Alternate.aspx` bir tarayıcıdan görüntülendiğinde. (Şekil 7'ye kıyasla) sayfanın üst kısmındaki bağlantıların eklenmesi unutmayın.


[![Hemen yansıtılmasını iç içe geçmiş ana sayfalar ve Their içerik sayfalarını olan üst düzey ana sayfaya değiştirildi](nested-master-pages-cs/_static/image23.png)](nested-master-pages-cs/_static/image22.png)

**Şekil 08**: hemen yansıtılmasını iç içe geçmiş ana sayfalar ve Their içerik sayfalarını olan üst düzey ana sayfaya değiştirildi ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>İç içe geçmiş ana sayfa için yönetim bölümündeki kullanma

Bu noktada sunuyoruz iç içe geçmiş ana avantajları sayfaları attıktan ve oluşturmak ve bunları bir ASP.NET uygulamasında kullanmak nasıl gördünüz. Adım 1, 2 ve 3, örnekler, ancak yeni bir üst düzey ana sayfası, yeni iç içe geçmiş ana sayfalar ve yeni içerik sayfaları oluşturma dahil. Yeni bir iç içe geçmiş ana sayfa için bir Web sitesi ile var olan bir üst düzey ana sayfa ve içerik sayfalarındaki ekleme nedir?

Mevcut bir Web sitesini bir iç içe geçmiş ana sayfa tümleştirme ve var olan içerik sayfalarıyla ilişkilendirme sıfırdan başlıyor değerinden biraz daha fazla çaba gerektirir. Adım 4, 5, 6 ve 7 biz tanıtım uygulamamız adlı yeni bir iç içe geçmiş ana sayfa içerecek şekilde genişletmek gibi bu zorluklar keşfedin `AdminNested.master` yönetici için yönergeler içerir ve ASP.NET sayfaları tarafından kullanılan `~/Admin` klasör.

Bir iç içe geçmiş ana sayfa tanıtım uygulamamıza tümleştirme aşağıdaki kısıtlamalar getirebilir sunar:

- Var olan içeriğin sayfaları içinde `~/Admin` klasörünüz kendi ana sayfasından belirli beklentileri. Yeni başlayanlar için mevcut olması için belirli ContentPlaceHolder denetimleri beklerler. Ayrıca, `~/Admin/AddProduct.aspx` ve `~/Admin/Products.aspx` sayfaları ana sayfanın genel arama `RefreshRecentProductsGrid` yöntemine kendi `GridMessageText` özelliği veya bir olay işleyicisi için kendi `PricesDoubled` olay. Sonuç olarak, iç içe geçmiş ana sayfamızı aynı ContentPlaceHolder ve Genel üyeler sağlamanız gerekir.
- Önceki öğreticide geliştirdik `BasePage` dinamik olarak ayarlamak için sınıf `Page` nesnenin `MasterPageFile` özelliği temel bir oturum değişkeni üzerinde. Nasıl için dinamik ana sayfalar iç içe geçmiş ana sayfalar kullanırken destekliyoruz?

Bu iki aşama, iç içe geçmiş ana sayfa oluşturma ve mevcut bizim içerik sayfalarından kullanılmakta belirir. Biz araştırmak ve bu sorunları ortaya çıktıkları anda surmount.

## <a name="step-4-creating-the-nested-master-page"></a>4. adım: iç içe geçmiş ana sayfa oluşturma

Bizim ilk görev yönetim bölümündeki sayfaları tarafından kullanılmak üzere iç içe geçmiş ana sayfa oluşturmaktır. Adım 2'de yeni bir ekleme ana sayfa iç içe olduğunda gördüğümüz gibi iç içe geçmiş ana sayfa üst ana sayfasını belirtmek ihtiyacımız var. Ancak sahip olduğumuz en üst düzey iki ana sayfa: `Site.master` ve `Alternate.master`. Oluşturduğumuz geri çağırma `Alternate.master` önceki öğreticide ve belirleyen kod yazdınız `BasePage` ayarlama sayfası nesnenin sınıf `MasterPageFile` ya da çalışma zamanında özellik `Site.master` veya `Alternate.master` değerine göre `MyMasterPage` Oturum değişkeni.

Uygun üst düzey ana sayfa kullanmasını sağlayacak şekilde iç içe geçmiş ana sayfamızı nasıl yapılandırıyoruz? İki seçenek sunuyoruz:

- İki iç içe geçmiş ana sayfalar oluşturma `AdminNestedSite.master` ve `AdminNestedAlternate.master`ve bunları üst düzey ana sayfalar için bağlama `Site.master` ve `Alternate.master`sırasıyla. İçinde `BasePage`, ardından ayarladığımız `Page` nesnenin `MasterPageFile` uygun iç içe geçmiş ana sayfa için.
- Tek bir iç içe geçmiş ana sayfa oluşturursanız ve içerik sayfaları söz konusu ana sayfa kullanın. Ardından, çalışma zamanında, iç içe geçmiş ana sayfa ayarlamak ihtiyacımız `MasterPageFile` zamanında uygun üst düzey ana sayfa özelliği. (, Artık anladığınızda, ana sayfalar de sahip bir `MasterPageFile` özellik.)

İkinci seçenek kullanalım. Tek bir iç içe geçmiş ana sayfa dosyası içinde oluşturma `~/Admin` adlı klasöre `AdminNested.master`. Çünkü her ikisi de `Site.master` ve `Alternate.master` aynı ContentPlaceHolder denetimler kümesini varsa, size bağlamak için önerilse de hangi ana sayfayı, kendisine bağladığınız farketmez `Site.master` tutarlılık'ın çok için.


[![İç içe geçmiş ana sayfa ~/Admin klasöre ekleyin.](nested-master-pages-cs/_static/image26.png)](nested-master-pages-cs/_static/image25.png)

**Şekil 09**: iç içe geçmiş ana sayfa için ekleme `~/Admin` klasör. ([Tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image27.png))


İç içe geçmiş ana sayfa dört ContentPlaceHolder denetimi ile ana sayfa bağlı olduğundan, Visual Studio dört ekler yeni iç içe geçmiş ana sayfa dosyanın ilk biçimlendirme denetimlerini içerik. 2 ve 3 ContentPlaceHolder denetiminin her içerik denetimi ekleyin, üst düzey ana sayfanın ContentPlaceHolder denetimi aynı adı vererek adımlarda yaptığımız gibi. Ayrıca karşılık gelen içerik denetimi aşağıdaki işaretlemeyi ekleyin `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-cs/samples/sample8.html)]

Ardından, tanımlama `instructions` CSS sınıfı içinde `Styles.css` ve `AlternateStyles.css` CSS dosyaları. Aşağıdaki CSS kurallarını ile biçimlendirilmiş HTML öğeleri neden `instructions` açık sarı arka plan rengi siyah, düz bir kenarlık ile görüntülenecek sınıf:


[!code-css[Main](nested-master-pages-cs/samples/sample9.css)]

Bu işaretleme, iç içe geçmiş ana sayfaya eklendiğinden, yalnızca bu iç içe geçmiş ana sayfa (yani, yönetim bölümündeki sayfaları) kullanan bu sayfalarda görünür.

İç içe geçmiş ana sayfanıza bu eklemeler yaptıktan sonra bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](nested-master-pages-cs/samples/sample10.aspx)]

Her bir içerik denetimi ContentPlaceHolder denetimi olduğuna dikkat edin ContentPlaceHolder denetimleri `ID` özellikleri değerlerine karşılık gelen en üst düzey bir ana sayfa ContentPlaceHolder denetimlerinde olarak atanır. Yönetim bölümünde özel biçimlendirme Ayrıca, görünür `MainContent` ContentPlaceHolder.

Şekil 10 gösteren `AdminNested.master` Visual Studio'nun Tasarımcı görüntülendiğinde iç içe geçmiş ana sayfa. Üst kısmında sarı kutusundaki yönergeleri gördüğünüz `MainContent` içerik denetimi.


[![İç içe geçmiş ana sayfa yönergeler için yönetici eklemek için üst düzey ana sayfa genişletir.](nested-master-pages-cs/_static/image29.png)](nested-master-pages-cs/_static/image28.png)

**Şekil 10**: iç içe geçmiş ana sayfa yönergeler için yönetici eklemek için üst düzey ana sayfa genişletir. ([Tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>5. adım: yeni bir iç içe geçmiş ana sayfa kullanmak için mevcut içerik sayfaları güncelleştirme

İhtiyacımız bağlamak için yönetim bölümüne yeni bir içerik sayfası ekliyoruz herhangi bir zamanda `AdminNested.master` oluşturduğumuz ana sayfa. Ancak sayfaları içerik mevcut hakkında neler? Sitedeki tüm içerik sayfalarının şu anda, türetilen `BasePage` programlı olarak çalışma zamanında ana sayfa içerik sayfasının ayarlar sınıfı. Bu yönetim bölümündeki içerik sayfaları için istediğimiz davranış değildir. Bunun yerine, her zaman kullanmak için bu içerik sayfaları istiyoruz `AdminNested.master` sayfası. Bu, çalışma zamanında sağ üst düzey içerik sayfası seçmek için iç içe geçmiş ana sayfa sorumluluğu olacaktır.

Elde etmek için en iyi şekilde bu davranışı adlı yeni bir özel taban sayfası sınıfı oluşturmak için istenen `AdminBasePage` genişleten `BasePage` sınıfı. `AdminBasePage` ardından kılabilirsiniz `SetMasterPageFile` ayarlayıp `Page` nesnenin `MasterPageFile` sabit kodlu değer "~ / Admin/AdminNested.master". Bu şekilde, herhangi bir sayfa, türetilen `AdminBasePage` kullanacağı `AdminNested.master`öğesinden türetilen herhangi bir sayfa bilgileriyse `BasePage` olacaktır, `MasterPageFile` özelliğini ayarlamak için dinamik olarak ya da "~ / Site.master" veya "~ / Alternate.master" değerine göre `MyMasterPage` Oturum değişkeni.

Başlamak için yeni bir sınıf dosyası ekleyerek `App_Code` adlı klasöre `AdminBasePage.cs`. Sahip `AdminBasePage` genişletmek `BasePage` ve daha sonra geçersiz `SetMasterPageFile` yöntemi. Bu yönteme atama `MasterPageFile` değeri "~ / Admin/AdminNested.master". Sınıfınıza bu değişiklikleri yaptıktan sonra dosya aşağıdakine benzer görünmelidir:


[!code-csharp[Main](nested-master-pages-cs/samples/sample11.cs)]

Artık mevcut içerik sayfaları bölümüne türetilen yönetim sağlamak ihtiyacımız `AdminBasePage` yerine `BasePage`. Her içerik sayfası için arka plan kod sınıfı dosyaya gidin `~/Admin` klasör bu değişiklik yapın. Örneğin, `~/Admin/Default.aspx` arka plan kod sınıfı bildirimden değiştirirsiniz:


[!code-csharp[Main](nested-master-pages-cs/samples/sample12.cs)]

Hedef:


[!code-csharp[Main](nested-master-pages-cs/samples/sample13.cs)]

Şekil 11 gösterilmektedir nasıl en üst düzey bir ana sayfa (`Site.master` veya `Alternate.master`), iç içe geçmiş ana sayfa (`AdminNested.master`), ve yönetim bölümüne içerik sayfalarını birbirleriyle.


[![İç içe geçmiş ana sayfa içerik özel yönetim bölümündeki sayfaları tanımlar.](nested-master-pages-cs/_static/image32.png)](nested-master-pages-cs/_static/image31.png)

**Şekil 11**: iç içe geçmiş ana sayfa tanımlar içerik belirli yönetim bölümündeki sayfaları ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>6. adım: yansıtma ana sayfanın genel yöntemleri ve özellikleri

Sözcüğünün `~/Admin/AddProduct.aspx` ve `~/Admin/Products.aspx` sayfaları ana sayfayla program aracılığıyla etkileşim: `~/Admin/AddProduct.aspx` çağrıları ana sayfaya genel `RefreshRecentProductsGrid` yöntemi ve kümeleri kendi `GridMessageText` özelliği; `~/Admin/Products.aspx` için bir olay işleyicisi sahip `PricesDoubled` olay. Önceki öğreticide oluşturduğumuz bir soyut `BaseMasterPage` bu genel üyeleri tanımlanmış sınıf.

`~/Admin/AddProduct.aspx` Ve `~/Admin/Products.aspx` sayfaları varsayılır, ana sayfa öğesinden türetilen `BaseMasterPage` sınıfı. `AdminNested.master` Sayfasında, ancak şu anda genişletir `System.Web.UI.MasterPage` sınıfı. Ziyaret edildiğinde bir sonucu olarak `~/Admin/Products.aspx` bir `InvalidCastException` iletiyle oluşturuldu: "nesne türünün yayımlanamıyor ' ASP.admin\_adminnested\_ana ' 'BaseMasterPage' türüne."

Bunu sağlamak için ihtiyacımız düzeltmek için `AdminNested.master` arka plan kod sınıfı genişletmeniz `BaseMasterPage`. İç içe geçmiş ana sayfa arka plan kod sınıfı bildirimden güncelleştirin:


[!code-csharp[Main](nested-master-pages-cs/samples/sample14.cs)]

Hedef:


[!code-csharp[Main](nested-master-pages-cs/samples/sample15.cs)]

Biz henüz tamamlanmadı. Çünkü `BaseMasterPage` sınıfı soyut, geçersiz kılmak ihtiyacımız `abstract` üyeleri `RefreshRecentProductsGrid` ve `GridMessageText`. Bu üyeleri tarafından en üst düzey ana sayfalar, kullanıcı arabirimlerini güncelleştirmek için kullanılır. (Aslında, yalnızca `Site.master` ana sayfayı hem genişletme olduğundan bu yöntemleri en üst düzey her iki ana sayfalar uygulamak rağmen bu yöntemleri kullanan `BaseMasterPage`.)

Bu üyeleri uygulamak gerekirken `AdminNested.master`, tüm bu uygulamalar gereken yapmak için yalnızca aynı üye iç içe geçmiş ana sayfa tarafından kullanılan en üst düzey ana sayfa çağırın. Örneğin, bir içerik sayfasının yönetim bölümündeki çağırdığında iç içe geçmiş ana sayfa `RefreshRecentProductsGrid` yöntemi, tüm iç içe geçmiş ana sayfa ihtiyaçlarınızın yapmak için buna karşılık, çağrı `Site.master` veya `Alternate.master`'s `RefreshRecentProductsGrid` yöntemi.

Bunu başarmak için aşağıdakileri ekleyerek başlangıç `@MasterType` üstüne yönerge `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample16.aspx)]

Bu geri çağırma `@MasterType` yönergesi adlı arka plan kod sınıfı için kesin türü belirtilmiş bir özellik ekler `Master`. Daha sonra geçersiz `RefreshRecentProductsGrid` ve `GridMessageText` üyeleri ve çağrı yalnızca temsilci `Master`yöntemi ilgili:


[!code-csharp[Main](nested-master-pages-cs/samples/sample17.cs)]

Bu kod bir yerde Yönetim bölümüne içerik sayfalarını ziyaret edin ve olmalıdır. Şekil 12 gösterir `~/Admin/Products.aspx` sayfasında bir tarayıcıdan görüntülendiğinde. Gördüğünüz gibi iç içe geçmiş ana sayfasında tanımlanan yönetim yönergeleri kutusu, sayfa içerir.


[![Yönetim bölümündeki içerik sayfalarında her sayfanın üst kısmındaki yönergeleri içerir.](nested-master-pages-cs/_static/image35.png)](nested-master-pages-cs/_static/image34.png)

**Şekil 12**: İçerik sayfalarında üst her sayfasının Yönetim bölümüne dahil yönergeleri ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>7. adım: çalışma zamanında uygun üst düzey ana sayfa kullanma

Tüm içerik yönetim bölümündeki sayfalarında tam işlevsel olsa da, bunların tümü aynı üst düzey ana sayfayı kullanın ve kullanıcı tarafından seçilen ana sayfasındaki Yoksay `ChooseMasterPage.aspx`. İç içe geçmiş ana sayfa vardır. Bunun nedeni, bu, davranıştır kendi `MasterPageFile` statik özellik kümesine `Site.master` içinde kendi `<%@ Master %>` yönergesi.

İhtiyacımız ayarlamak için son kullanıcı tarafından seçilen en üst düzey ana sayfa kullanılacak `AdminNested.master`'s `MasterPageFile` özellik değeri `MyMasterPage` oturum değişkeni. İçerik sayfaları ayarladığımız çünkü `MasterPageFile` özelliklerinde `BasePage`, iç içe geçmiş ana sayfa ayarlarsınız düşünebilirsiniz `MasterPageFile` özelliğinde `BaseMasterPage` veya `AdminNested.master`ait arka plan kod sınıfı. Ayarladığınız gerektiği için bu, ancak çalışmaz `MasterPageFile` sonuna kadar PreInit aşama özelliği. Biz program aracılığıyla bir ana sayfa sayfa yaşam döngüsüne dokunabilir en erken zamanı (PreInit aşama sonra oluşan) Init aşamadır.

Bu nedenle, iç içe geçmiş ana sayfa ayarlamak ihtiyacımız `MasterPageFile` içerik sayfalarından özelliği. Yalnızca içerik kullanan sayfaları `AdminNested.master` ana sayfa türetilen `AdminBasePage`. Bu nedenle, bu mantığı buraya koyabilirsiniz. Adım 5'te biz geçersiz kılınmış `SetMasterPageFile` ayarlama yöntemi `Page` nesnenin `MasterPageFile` özelliğini "~ / Admin/AdminNested.master". Güncelleştirme `SetMasterPageFile` ana sayfa da ayarlanacak `MasterPageFile` özelliğini oturumunda depolanmış sonucu:


[!code-csharp[Main](nested-master-pages-cs/samples/sample18.cs)]

`GetMasterPageFileFromSession` Ekledik yöntemi `BasePage` sınıfı önceki bir öğreticide, uygun ana sayfa dosya yolu tabanlı oturum değişken değerini döndürür.

Yerinde bu değişiklik, kullanıcının ana sayfa seçimini Yönetim bölümüne taşır. Şekil 13 gösteren Şekil 12 olarak, ancak kullanıcının ana sayfa seçime değiştirildikten sonra aynı sayfada `Alternate.master`.


[![İç içe geçmiş Yönetim sayfası, kullanıcı tarafından seçilen en üst düzey ana sayfa kullanır.](nested-master-pages-cs/_static/image38.png)](nested-master-pages-cs/_static/image37.png)

**Şekil 13**: üst düzey ana sayfası seçilen kullanıcı tarafından iç içe Yönetim sayfasını kullanır ([tam boyutlu görüntüyü görmek için tıklatın](nested-master-pages-cs/_static/image39.png))


## <a name="summary"></a>Özet

LIKE kadar nasıl içerik sayfalarını bir ana sayfaya bağlayabilir, bu iç içe geçmiş oluşturmak mümkündür alt ana sayfa sahip ana sayfa üst ana sayfaya bağlayın. Alt ana sayfa içerik denetimleri her üst öğesinin ContentPlaceHolder tanımlayabilirsiniz; kendi ContentPlaceHolder denetimler (diğer biçimlendirme için olduğu gibi)'nı, ardından bu içerik denetimlerine ekleyebilirsiniz. İç içe geçmiş ana sayfalar, burada tüm sayfaları ıpam'da bir görünüm paylaşma henüz site belirli bölümlerini benzersiz özelleştirmelere ihtiyaç büyük web uygulamalarında oldukça kullanışlıdır.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İç içe geçmiş ASP.NET ana sayfaları](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [İç içe geçmiş ana sayfalar ve VS 2005 tasarım zamanı için ipuçları](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Ana sayfa desteği VS 2008 iç içe geçmiş](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar 1998'de bu yana birden çok ASP/ASP.NET books ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileri ile çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott, konumunda ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](specifying-the-master-page-programmatically-cs.md)
> [İleri](creating-a-site-wide-layout-using-master-pages-vb.md)
