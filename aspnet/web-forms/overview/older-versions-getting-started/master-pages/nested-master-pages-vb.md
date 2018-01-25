---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: "İç içe geçmiş ana sayfalar (VB) | Microsoft Docs"
author: rick-anderson
description: "İçinde başka bir ana sayfa iç içe gösterilmektedir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9059e358311cc80b6a64aa3ee1168f4ffcd4e94c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="nested-master-pages-vb"></a>İç içe geçmiş ana sayfalar (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> İçinde başka bir ana sayfa iç içe gösterilmektedir.


## <a name="introduction"></a>Giriş

Son dokuz öğreticileri süresince biz ana sayfalar site genelinde düzeniyle uygulamak öğrendiniz. Buna koysalar ana sayfalar bize, ana sayfa temelinde içerik sayfası içeriği sayfa özelleştirilebilir belirli bölgeler birlikte ortak biçimlendirme tanımlamak için sayfa Geliştirici izin verin. Bir ana sayfa ContentPlaceHolder denetimlerinde özelleştirilebilir bölgeleri gösterir; ContentPlaceHolder denetimleri için özelleştirilmiş biçimlendirme, içerik denetimleri aracılığıyla içerik sayfasını tanımlanır.

Biz bugüne kadarki incelediniz ana sayfa teknikleri tüm site genelinde kullanılan tek bir düzen varsa mükemmeldir. Bununla birlikte, birçok büyük Web siteleri özelleştirilmiş bir site düzenini çeşitli bölümlerde vardır. Örneğin, hastaneler personeli tarafından hasta bilgileri, etkinlikler ve faturalama yönetmek için kullanılan bir sağlık uygulama göz önünde bulundurun. Bu uygulama web sayfalarında üç türde olabilir:

- Personel kullanılabilirlik, burada güncelleştirebilirsiniz personel üye özgü sayfaları zamanlamaları görüntüleyin veya istek tatil süresi.
- Burada personel bilgilerini görüntüleme veya belirli bir süre bekleyin için düzenleme hasta özgü sayfaları.
- Talep durumları ve finansal raporlar faturalama özgü sayfaları muhasebe burada geçerli gözden geçirin.

Her sayfanın üst ve alt boyunca sık kullanılan bağlantılar bir dizi menü gibi ortak bir düzeni paylaşır. Ancak bu genel düzeni özelleştirmek personel - hasta- ve faturalama özgü sayfaları gerekebilir. Örneğin, belki de tüm personel özgü sayfaları şu anda oturum açmış kullanıcının kullanılabilirlik ve günlük zamanlama gösteren bir takvim ve görev listesi içermelidir. Belki de adını, adresini ve bilgilerini düzenleniyor hasta sigorta bilgilerini göstermek için tüm hasta özgü sayfaları gerekir.

Kullanarak bu tür özel düzenler oluşturmak mümkündür *ana sayfalar iç içe geçmiş*. Yukarıdaki senaryoyu uygulamak için biz site genelinde düzeni, menü ve altbilgi içerik ContentPlaceHolders için özelleştirilebilir bölgeleri tanımlama ile tanımlanan bir ana sayfa oluşturarak başlarsınız. Biz, ardından üç iç içe geçmiş ana sayfalar, bir web sayfası her tür oluşturursunuz. Her iç içe geçmiş ana sayfa ana sayfa kullanan içerik sayfalarını türü arasında içerik tanımlarsınız. İç içe geçmiş ana sayfa hasta özgü içerik sayfaları için diğer bir deyişle, biçimlendirme ve düzenlenen hasta hakkında bilgi görüntülemek için programlı mantığını içerir. Yeni bir Hasta özgü sayfa oluştururken, biz, bu bir iç içe geçmiş ana sayfa bağlayın.

Bu öğretici, iç içe geçmiş ana sayfalar yararları vurgulayarak başlatır. Ardından, oluşturma ve iç içe geçmiş ana sayfalar kullanmanız gösterir.

> [!NOTE]
> İç içe geçmiş ana sayfalar, .NET Framework 2.0 sürümünde bu yana mümkün olmuştur. Ancak, Visual Studio 2005 iç içe geçmiş ana sayfalar için tasarım zamanı desteği içermiyordu. İyi haber, Visual Studio 2008 iç içe geçmiş ana sayfalar için zengin bir tasarım zamanı deneyimi sunar ' dir. İç içe geçmiş ana sayfalar kullanmak, ancak Visual Studio 2005 kullanmaya devam kullanıma [Scott Guthrie](https://weblogs.asp.net/scottgu/)ın blog girişi, [ipuçları VS 2005 tasarım zamanı iç içe geçmiş ana sayfalar için](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>İç içe geçmiş ana sayfalar yararları

Birçok Web sitesi bir kapsayıcı sitesi tasarımı ve bunun yanı sıra daha özelleştirilmiş tasarımları belirli sayfaları türleri için özel sahiptir. Örneğin, tanıtım web uygulamamız ilkel Yönetim bölümünde oluşturduk (sayfalarında `~/Admin` klasörü). Şu anda web sayfalarında `~/Admin` klasörü yönetim bölümündeki bu sayfaları olarak aynı ana sayfa kullanın (yani, `Site.master` veya `Alternate.master`kullanıcının seçime bağlı olarak).

> [!NOTE]
> Şu an için sitemizi yalnızca bir ana sayfa olduğunu içeriğini `Site.master`. Bu öğreticinin ilerleyen bölümlerinde "Kullanarak bir iç içe geçmiş ana sayfa için yönetim bölümle" Başlangıç iki (veya daha fazla) ana sayfalar ile iç içe geçmiş ana sayfalar kullanarak adresi.


Biz ek bilgiler veya aksi halde sitedeki diğer sayfalarında olmaz bağlantılar dahil etmek için yönetim sayfaların düzenini özelleştirmek için sorulan olduğunu düşünün. Bu gereksinim uygulamak için dört teknikler şunlardır:

1. İçerik her sayfasına yönetim özgü bilgileri ve bağlantıları el ile Ekle `~/Admin` klasör.
2. Güncelleştirme `Site.master` yönetim bölüm özgü bilgileri ve bağlantıları içerir ve kod göstermek veya gizlemek bu bölümler için ana sayfa eklemek için ana sayfa bağlı olup olmadığını Yönetim sayfalarını birini ziyaret edilen üzerinde.
3. Yeni bir ana sayfa oluşturma biçimlendirmeden üzerinden Yönetim bölümünde özellikle için kopyalama `Site.master`, Yönetim bölümünde özgü bilgileri ve bağlantıları ekleyin ve içerik sayfalarına güncelleştirme `~/Admin` bu yeni bir şablonu kullanmak için klasör Sayfa.
4. Bağlar iç içe geçmiş bir ana sayfa oluşturma `Site.master` ve içerik sayfalarına sahip `~/Admin` bu yeni klasör kullanımı, ana sayfa iç içe geçmiş. Bu iç içe geçmiş ana sayfa yalnızca ek bilgi ve bağlantılar yönetim sayfalarına belirli içerir ve zaten tanımlanmış biçimlendirme yinelenecek ihtiyaç duymaz `Site.master`.

İlk seçenek az palatable olacaktır. Ana sayfalar kullanmanın tüm el ile kopyalayın ve yeni ASP.NET sayfaları için ortak biçimlendirme yapıştırmak zorunda çıktığınızda taşımak için noktasıdır. İkinci seçenek kabul edilebilir olmakla birlikte, uygulama, ana sayfalar, nadiren görüntülenir ve ana sayfa düzenleme geliştiriciler bu biçimlendirme olarak çözmek için ve ne zaman hatırlamak zorunda gerektiren biçimlendirme ile yukarı toplu siparişler gibi daha az korunabilir hale getirir, tam olarak gizli olduğu durumlarda belirli biçimlendirme karşı görüntülenir. Bu yaklaşım daha da fazla tür web sayfaları bu tek bir ana sayfa tarafından barındırılabilmesi için gereken özelleştirmeleri daha az tenable olacaktır.

Üçüncü seçenek gösterip kaldırır ve karmaşıklık ikinci seçeneği ile surfaced verir. Ancak, ortak düzeninden kopyalayıp kurmamızı gerektiren seçeneği üç bilgisayarın asıl sakıncası olan `Site.master` yeni yönetim bölüm özgü ana sayfaya. Site genelinde düzenini değiştirmek sizi daha sonra karar verirseniz iki yerde değiştirmeyi unutmayın sahip.

Dördüncü seçeneği iç içe geçmiş ana sayfalar, bize en iyisi ikinci ve üçüncü seçenekleri verin. Farklı dosyalarına ayrılmış belirli bölgelerine özel içeriği üzerindeyken site genelinde düzen bilgilerini bir dosyada - en üst düzey ana sayfa - korunur.

Bu öğretici bir görünüm oluşturma ve basit bir iç içe geçmiş ana sayfa kullanarak başlar. Yeni üst düzey ana sayfa, iki iç içe geçmiş ana sayfalar ve iki içerik sayfaları oluşturuyoruz. "Kullanarak bir iç içe geçmiş ana sayfa için yönetim bölümle" başlayarak, biz iç içe geçmiş ana sayfalar kullanımını dahil etmek için var olan ana sayfa mimarimizin güncelleştirme sırasında arayın. Özellikle, biz iç içe geçmiş bir ana sayfa oluşturun ve içerik sayfalar için ek özel içerik dahil etmek için kullanın `~/Admin` klasör.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>1. adım: basit bir üst düzey ana sayfa oluşturma

İç içe geçmiş ana oluşturma var olan bir ana birine sayfaları dayalı ve var olan içerik sayfaları zaten belirli beklediğiniz olduğundan en üst düzey ana sayfası yerine bu yeni iç içe geçmiş ana sayfa kullanmak için varolan bir içerik sayfasını güncelleştirme bazı karmaşıklık kapsar Üst düzey ana sayfada tanımlı ContentPlaceHolder denetler. Bu nedenle, iç içe geçmiş ana sayfa aynı adlarıyla aynı ContentPlaceHolder denetimler de dahil etmelisiniz. Ayrıca, iki ana sayfa belirli demo uygulamamız sahiptir (`Site.master` ve `Alternate.master`), dinamik olarak atanır daha fazla bu karmaşıklık ekleyen bir kullanıcının tercihlerinize göre alarak içerik sayfası. Biz iç içe geçmiş ana sayfalar daha sonra Bu öğreticide kullanmak üzere var olan uygulamayı güncelleştirme sırasında görünür, ancak şimdi ana sayfalar örnek ilk odaklanmak basit bir iç içe geçmiş.

Adlı yeni bir klasör oluşturun `NestedMasterPages` ve ardından yeni bir ana sayfa dosyası adlı klasöre ekleyin `Simple.master`. (Bu klasör ve dosya eklendikten sonra Çözüm Gezgini ekran görüntüsü için bkz: Şekil 1.) Sürükleme `AlternateStyles.css` tasarımcıya Çözüm Gezgini'nden stil sayfası dosyası. Bu ekler bir `<link>` stil sayfası dosyasındaki öğesine `<head>` olacağı öğesi ana sayfanın `<head>` öğenin biçimlendirmesi gibi görünmelidir:


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Ardından, Web formunu içinde aşağıdaki biçimlendirmeleri eklemek `Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Bu biçimlendirme, sayfanın üst kısmındaki Lacivert arka plan üzerinde büyük bir beyaz yazı "İç içe ana sayfalar (Basit)" başlıklı bir bağlantı görüntüler. Altında olan `MainContent` ContentPlaceHolder. Şekil 1 gösterir `Simple.master` Visual Studio Tasarımcısı'nda yüklendiğinde ana sayfa.


[![İç içe geçmiş ana sayfa içerik Özel Yönetim bölümünde sayfalara tanımlar.](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Şekil 01**: iç içe geçmiş ana sayfa tanımlar içerik belirli sayfalara Yönetim bölümünde ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>2. adım: basit bir iç içe geçmiş ana sayfa oluşturma

`Simple.master`iki ContentPlaceHolder denetimleri içerir: `MainContent` eklediğimiz boyunca ile Web Form içinde ContentPlaceHolder `head` ContentPlaceHolder ' `<head>` öğesi. Bir içerik sayfası oluşturun ve ona bağlamak için olsaydı `Simple.master` içerik sayfasını iki ContentPlaceHolders için başvuran iki içerik denetimlerine sahip. Benzer şekilde, biz iç içe geçmiş bir ana sayfa oluşturun ve onu bağladıktan `Simple.master` iç içe geçmiş ana sayfa iki içerik denetimlerine sahip olacaktır.

Yeni bir iç içe geçmiş ana sayfasına ekleyelim `NestedMasterPages` adlı klasörü `SimpleNested.master`. Sağ `NestedMasterPages` klasörü ve Yeni Öğe Ekle'i seçin. Bu Şekil 2'deki yeni öğe Ekle iletişim kutusunu açar. Yeni ana sayfa adını yazın ve ana sayfa şablon türünü seçin. Yeni ana sayfa iç içe geçmiş ana sayfa olması gerektiğini belirtmek için "Select ana sayfa" onay kutusunu işaretleyin.

Ardından, Ekle düğmesini tıklatın. Bu aynı seçin bir içerik sayfasının bir ana sayfaya bağlanırken gördüğünüz bir ana sayfa iletişim kutusu görüntülenir (bkz: Şekil 3). Seçin `Simple.master` ana sayfasında `NestedMasterPages` klasörü ve Tamam'ı tıklatın.

> [!NOTE]
> Web sitesi projesini modeli yerine Web uygulama projesi modelini kullanarak ASP.NET Web sitenizi oluşturduysanız, Şekil 2'deki yeni öğe Ekle iletişim kutusunda "ana sayfa seç" onay kutusunu görmez. Web uygulama projesi modeli kullanılırken bir iç içe geçmiş ana sayfa oluşturmak için (yerine ana sayfa şablonu) iç içe geçmiş ana sayfa şablonu seçmelisiniz. İç içe geçmiş ana sayfa şablonu seçilmesi ve Ekle düğmesine sonra aynı Şekil 3'te gösterilen iletişim kutusu görünür bir ana sayfa seçin.


[![Denetleme &quot;Select ana sayfa&quot; bir iç içe geçmiş ana sayfa eklemek için onay kutusu](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Şekil 02**: iç içe geçmiş ana sayfa eklemek için "ana sayfa seç" onay kutusunu işaretleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image6.png))


[![İç içe geçmiş ana sayfa Simple.master ana sayfasına bağlama](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Şekil 03**: iç içe geçmiş ana sayfayı bağlamak `Simple.master` ana sayfa ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image9.png))


İç içe geçmiş ana sayfa bildirim temelli biçimlendirme, aşağıda gösterilen üst düzey ana sayfa iki ContentPlaceHolder denetimi başvuran iki içerik denetimleri içerir.


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

Dışında `<%@ Master %>` yönerge, iç içe geçmiş ana sayfa ilk bildirim temelli biçimlendirme başlangıçta aynı üst düzey ana sayfaya bir içerik sayfasını bağlama sırasında oluşturulan biçimlendirme aynıdır. Gibi bir içerik sayfasının `<%@ Page %>` yönergesi, `<%@ Master %>` burada yönergesi içeren bir `MasterPageFile` iç içe geçmiş ana sayfasının üst ana sayfasında belirten özniteliği. İç içe geçmiş ana sayfa ve aynı üst düzey ana sayfaya bağlı bir içerik sayfasını arasındaki temel fark, iç içe geçmiş ana sayfa ContentPlaceHolder denetimlerini içerebilir ' dir. İç içe geçmiş ana sayfa ContentPlaceHolder denetimleri içerik sayfalarını biçimlendirme burada özelleştirebilirsiniz bölgeleri tanımlayın.

Bu iç içe geçmiş ana sayfa "Hello, SimpleNested gelen!" metnini görüntüler şekilde güncelleştirin karşılık gelen içerik denetimindeki `MainContent` ContentPlaceHolder denetimi.


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Bu ek yaptıktan sonra iç içe geçmiş ana sayfa kaydedin ve ardından yeni bir içerik sayfasına ekleyin `NestedMasterPages` adlı klasörü `Default.aspx`ve onu bağladıktan `SimpleNested.master` ana sayfa. Bu sayfa ekleme bağlı, (bkz: Şekil 4) hiçbir içerik denetimleri içerip içermediğini şaşırabilirsiniz! Bir içerik sayfasını yalnızca erişebilir, *üst* sayfanın ContentPlaceHolders için ana. `SimpleNested.master`Tüm ContentPlaceHolder denetimlerinin içermez. Bu nedenle, bu ana sayfaya bağlı herhangi bir içerik sayfasında tüm içerik denetimlerinin içeremez.


[![Yeni içerik sayfası hiçbir içerik denetimleri içerir](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Şekil 04**: yeni içerik sayfasını içeren Hayır içerik denetimleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image12.png))


İç içe geçmiş ana sayfa güncelleştirme yapmak ihtiyacımız olan (`SimpleNested.master`) ContentPlaceHolder denetimleri eklenecek. Genellikle bir ContentPlaceHolder böylelikle kendi alt ana sayfa veya herhangi bir üst düzey ana sayfa ContentPlaceHolder ile çalışmak için içerik sayfasını izin vererek, üst ana sayfası tarafından tanımlanan her ContentPlaceHolder eklemek için iç içe geçmiş ana sayfalar isteyeceksiniz. denetler.

Güncelleştirme `SimpleNested.master` bir ContentPlaceHolder iki kendi içerik denetimlerini dahil etmek için ana sayfa. ContentPlaceHolder denetimleri, içerik denetimi başvurduğu ContentPlaceHolder denetimi aynı adı verin. Diğer bir deyişle, adlı ContentPlaceHolder denetim ekleme `MainContent` içeriği kontrol `SimpleNested.master` başvuran `MainContent` ContentPlaceHolder ' `Simple.master`. Aynı şeyi başvuran içerik denetimi yapmak `head` ContentPlaceHolder.

> [!NOTE]
> Bu adlandırma simetrisi ı iç içe geçmiş ana sayfa ContentPlaceHolder denetimlerinde ContentPlaceHolders için üst düzey ana sayfasında aynı adlandırma öneririz, ancak gerekli değildir. İç içe geçmiş ana sayfanızda ContentPlaceHolder denetimleri istediğiniz adı verebilirsiniz. Ancak, t ContentPlaceHolders için ile karşılık anımsaması kolay my üst düzey ana sayfa ve iç içe geçmiş ana sayfalar aynı adı kullanırsanız, sayfanın hangi bölgeleri.


Bu eklemeleri yaptıktan sonra `SimpleNested.master` ana sayfanın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Silme `Default.aspx` içerik oluşturduğumuz sayfa ve ardından, kendisine bağlama yeniden ekleyin `SimpleNested.master` ana sayfa. Bu süre Visual Studio ekler iki içerik denetimleri için `Default.aspx`, ContentPlaceHolders için başvuran şimdi tanımlanan `SimpleNested.master` (bkz. Şekil 6). "Hello, Default.aspx gelen!" metin ekleme Başvurulan içeriği kontrol `MainContent`.

Şekil 5 gösterir burada - söz konusu üç varlık `Simple.master`, `SimpleNested.master`, ve `Default.aspx` - ve birbirleriyle nasıl ilişkili olduğunu. Aşağıdaki diyagramda gösterildiği gibi iç içe geçmiş ana sayfa içerik denetimleri için ana öğenin ContentPlaceHolder uygular. Bu bölgeler için içerik sayfasını erişilebilir olması gerekiyorsa, iç içe geçmiş ana sayfa İçerik denetimlerine kendi ContentPlaceHolders için eklemeniz gerekir.


[![En üst düzey ve iç içe geçmiş ana sayfalar içerik sayfasının düzeni dikte](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Şekil 05**: içerik sayfasının düzeni en üst düzey ve iç içe geçmiş ana sayfalar dikte ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image15.png))


Bu davranışı nasıl bir içerik sayfasının veya ana sayfa yalnızca kendi üst ana sayfası emin gösterir. Bu davranış da Visual Studio tasarımcı tarafından belirtilir. Şekil 6 gösterir Tasarımcı `Default.aspx`. Tasarımcı açıkça hangi bölgeleri içerik sayfasından düzenlenebilir ve ne bölümleri olmayan gösterirken, iç içe geçmiş ana sayfa düzenlenemeyen bölgeleri nelerdir ve en üst düzey ana sayfa bölge nelerdir ayırt etmek değil.


[![İçerik sayfası şimdi içerik denetimleri için iç içe geçmiş ana sayfa ContentPlaceHolders için içerir.](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Şekil 06**: içerik sayfası artık içeren içerik denetimleri için iç içe geçmiş ana sayfa ContentPlaceHolders için ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>3. adım: ikinci basit bir iç içe geçmiş ana sayfası ekleme

Birden çok iç içe geçmiş ana sayfalar olduğunda, iç içe geçmiş ana sayfalar yararı daha açıktır. Bu avantajı anlamak için başka bir iç içe geçmiş ana sayfa oluşturma `NestedMasterPages` ; klasör adı bu yeni iç içe geçmiş ana sayfa `SimpleNestedAlternate.master` ve onu bağladıktan `Simple.master` ana sayfa. 2. adımda yaptığımız gibi iç içe geçmiş ana sayfa iki içerik denetimlerini ContentPlaceHolder denetimleri ekleyin. Ayrıca, "Hello, SimpleNestedAlternate gelen!" metin eklemek üst düzey ana sayfaya ait karşılık gelen içerik denetimindeki `MainContent` ContentPlaceHolder. Bu değişiklikleri yaptıktan sonra yeni iç içe geçmiş ana sayfanızın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Adlı bir içerik sayfasını oluşturmak `Alternate.aspx` içinde `NestedMasterPages` klasörü ve onu bağladıktan `SimpleNestedAlternate.master` iç içe geçmiş ana sayfa. "Hello, diğer gelen!" metin ekleme karşılık gelen içerik denetimindeki `MainContent`. Şekil 7 gösterir `Alternate.aspx` Visual Studio tasarımcısı görüntülendiğinde.


[![Alternate.aspx SimpleNestedAlternate.master ana sayfaya bağlı](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Şekil 07**: `Alternate.aspx` bağlı `SimpleNestedAlternate.master` ana sayfa ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image21.png))


Şekil 7'ye Şekil 6 tasarımcıda tasarımcıda karşılaştırın. Her iki içerik sayfa üst düzey ana sayfada tanımlı aynı düzeni paylaşan (`Simple.master`), yani "İç içe ana sayfalar öğretici (Basit)" başlığı. Her ikisi de kendi üst ana sayfalarında - tanımlanmış ayrı içeriği "Hello, SimpleNested gelen!" metin henüz Şekil 6 ve "Hello, SimpleNestedAlternate gelen!" Şekil 7'de. Verilen, bu farklar burada Önemsiz, ancak bu örnek daha anlamlı farklar içerecek şekilde genişletebilirsiniz. Örneğin, `SimpleNested.master` sayfası, içerik sayfalarına özel seçenekleri menüsüyle içerebilir, ancak `SimpleNestedAlternate.master` bağlamak için içerik sayfalarına ilgili bilgi olabilir.

Şimdi, biz kapsayıcı site düzenine bir değişiklik yapmak için gereken düşünün. Örneğin, biz ortak bağlantıların listesini tüm içerik sayfalarına eklemek istediğinizi düşünelim. Bu en üst düzey ana sayfa güncelleştiriyoruz gerçekleştirmek için `Simple.master`. Herhangi bir değişiklik vardır, kendi iç içe geçmiş ana sayfalar ve uzantılarının, kendi içerik sayfalarının hemen yansıtılır.

İle biz değiştirebilirsiniz kapsayıcı site düzenini kolaylığı göstermek için açık `Simple.master` ana sayfa ve arasında aşağıdaki biçimlendirmeleri eklemek `topContent` ve `mainContent` `<div>` öğeleri:


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Bu iki bağlantı bağlar her sayfanın üstünde ekler `Simple.master`, `SimpleNested.master`, veya `SimpleNestedAlternate.master`; bu değişiklikleri tüm iç içe geçmiş ana sayfalar ve kendi içerik sayfalarının hemen uygulanır. Şekil 8 gösterir `Alternate.aspx` bir tarayıcıdan görüntülendiğinde. Sayfanın üst kısmındaki (Şekil 7'ye kıyasla) bağlantıların eklenmesi unutmayın.


[![Hemen yansıtılır Their içerik sayfaları ve iç içe geçmiş ana sayfalar olan üst düzey ana sayfaya değiştirildi](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Şekil 08**: en üst düzey ana sayfaya değiştirilen hemen yansıtılır Their içerik sayfaları ve iç içe geçmiş ana sayfalar olan ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Yönetim bölümünde için bir iç içe geçmiş ana sayfa kullanma

Bu noktada biz iç içe geçmiş ana avantajları sayfaları attıktan ve oluşturmak ve bunları bir ASP.NET uygulamasındaki öğrendiniz. Adım 1, 2 ve 3, örneklerde, Bununla birlikte, yeni bir üst düzey ana sayfa, yeni iç içe geçmiş ana sayfalar ve yeni içerik sayfaları oluşturma dahil. Ne hakkında yeni bir iç içe geçmiş ana sayfa bir Web sitesi için var olan bir üst düzey ana sayfa ile ve içerik sayfaları ekleme?

Mevcut bir Web bir iç içe geçmiş ana sayfa tümleştirme ve var olan içerik sayfalarıyla ilişkilendirme sıfırdan değerinden biraz daha fazla çaba gerektirir. Adım 4, 5, 6 ve 7 biz adlı yeni bir iç içe geçmiş ana sayfa içerecek şekilde demo uygulamamız büyütmek gibi bu zorluklar keşfedin `AdminNested.master` yönetici için yönergeler içerir ve ASP.NET sayfaları tarafından kullanılan `~/Admin` klasör.

İç içe geçmiş ana sayfa demo uygulamamıza tümleştirme aşağıdaki zorluklardan sunar:

- Var olan içerik sayfaları `~/Admin` klasörünüz belirli beklentilerini kendi ana sayfası. Yeni başlayanlar bulunması için belirli ContentPlaceHolder denetimleri bekler. Ayrıca, `~/Admin/AddProduct.aspx` ve `~/Admin/Products.aspx` sayfaları çağrı ana sayfanın ortak `RefreshRecentProductsGrid` set yöntemi, kendi `GridMessageText` özelliği, veya bir olay işleyicisi için kendi `PricesDoubled` olay. Sonuç olarak, iç içe geçmiş ana sayfamızı aynı ContentPlaceHolders için ve Genel üyeler sağlamanız gerekir.
- Önceki öğreticide biz Gelişmiş `BasePage` dinamik olarak belirlemek için sınıf `Page` nesnesinin `MasterPageFile` özelliğinin temel bir oturum değişkeni. Nasıl için dinamik ana sayfalar iç içe geçmiş ana sayfalar kullanırken destekliyoruz?

İç içe geçmiş ana sayfası oluşturmak ve varolan bizim içerik sayfalarından kullanın, bu iki zorluk belirir. Biz araştırın ve bu sorunları çıktıkları anda surmount.

## <a name="step-4-creating-the-nested-master-page"></a>4. adım: iç içe geçmiş ana sayfa oluşturma

Bizim ilk Yönetim bölümünde sayfalarında tarafından kullanılacak iç içe geçmiş ana sayfa oluşturmak için bir görevdir. Ana sayfa yeni bir ekleme iç içe geçmiş zaman 2. adımda gördüğümüz gibi iç içe geçmiş ana sayfasının üst ana sayfasında belirtmeniz gerekir. Ancak biz iki üst düzey ana sayfa: `Site.master` ve `Alternate.master`. Oluşturduğumuz geri çağırma `Alternate.master` önceki öğreticideki ve kodu yazdığınız `BasePage` sınıfı bu kümesi `Page` nesnenin `MasterPageFile` özelliği ya da çalışma zamanında `Site.master` veya `Alternate.master` değerinebağlıolarak`MyMasterPage` Oturum değişkeni.

Nasıl uygun en üst düzey ana sayfayı kullanan böylece biz iç içe geçmiş ana sayfamızı yapılandırırım? Biz, iki seçeneğiniz vardır:

- İki iç içe geçmiş ana sayfalar oluşturmanıza `AdminNestedSite.master` ve `AdminNestedAlternate.master`ve en üst düzey ana sayfalar bağlamak `Site.master` ve `Alternate.master`sırasıyla. İçinde `BasePage`, daha sonra ayarlarız `Page` nesnenin `MasterPageFile` uygun iç içe geçmiş ana sayfaya.
- Tek bir iç içe geçmiş ana sayfa oluşturun ve bu belirli ana sayfayı kullanan içerik sayfaları sahip. Ardından, çalışma zamanında biz iç içe geçmiş ana sayfa ayarlamanız gerekir `MasterPageFile` çalışma zamanında uygun en üst düzey ana sayfaya özelliği. (, Artık dahil edilir gibi ana sayfalar de bir `MasterPageFile` özelliğini.)

İkinci seçenek kullanalım. Tek bir iç içe geçmiş ana sayfa dosyasının oluşturma `~/Admin` adlı klasörü `AdminNested.master`. Çünkü her ikisi de `Site.master` ve `Alternate.master` , bağlamak için önerilir ancak hangi ana sayfa onu, bağladıktan önemli değildir, aynı ContentPlaceHolder denetimleri kümesini sahip `Site.master` tutarlılık'ın artırmak amacıyla için.


[![İç içe geçmiş ana sayfa ~/Admin klasörüne ekleyin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Şekil 09**: iç içe geçmiş ana sayfaya ekleme `~/Admin` klasör. ([Tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image27.png))


İç içe geçmiş ana sayfa dört ContentPlaceHolder denetimleri ile bir ana sayfa bağlı olduğundan, Visual Studio dört ekler içerik denetimlerine yeni iç içe geçmiş ana sayfa dosyasının ilk biçimlendirme. 2 ve 3 ContentPlaceHolder denetim her içerik denetiminde ekleme, üst düzey ana sayfa ContentPlaceHolder denetimi aynı adı verip adımlarda yaptığımız gibi. Karşılık gelen içerik denetimi de aşağıdaki biçimlendirme eklemek `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Ardından, tanımlamak `instructions` CSS sınıfı `Styles.css` ve `AlternateStyles.css` CSS dosyaları. Aşağıdaki CSS kuralları ile biçimlendirilmiş HTML öğeleri neden `instructions` açık sarı arka plan rengi siyah, düz bir sınır ile görüntülenecek sınıf:


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Bu biçimlendirme iç içe geçmiş ana sayfaya eklendiğinden yalnızca bu iç içe geçmiş ana sayfa (yani, Yönetim bölümünde sayfaları) kullanan bu sayfalarda görünür.

İç içe geçmiş ana sayfanıza Bu eklemeleri yaptıktan sonra bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Her içerik denetimi ContentPlaceHolder denetimi ve, sahip olduğuna dikkat edin ContentPlaceHolder denetimleri `ID` özellikleri en üst düzey ana sayfa karşılık gelen ContentPlaceHolder denetimlerinde aynı değerlere atanır. Yönetim bölümünde özgü biçimlendirme Üstelik görünür `MainContent` ContentPlaceHolder.

Şekil 10 gösteren `AdminNested.master` Visual Studio'nun Designer üzerinden görüntülendiğinde iç içe geçmiş ana sayfa. Sarı kutunun üst kısmındaki yönergeleri görebilirsiniz `MainContent` içerik denetimi.


[![İç içe geçmiş ana sayfa yönetici yönergeleri dahil etmek için üst düzey ana sayfa genişletir.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Şekil 10**: iç içe geçmiş ana sayfa yönetici yönergeleri dahil etmek için üst düzey ana sayfa genişletir. ([Tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>5. adım: yeni iç içe geçmiş ana sayfa kullanmak için var olan içerik sayfalarını güncelleştirme

Yeni bir içerik sayfasını ihtiyacımız bağlamak için yönetim bölümüne eklediğimiz zaman `AdminNested.master` yeni oluşturduğumuz ana sayfa. Ancak ne var olan sayfaları içerik? Sitedeki tüm içerik sayfalarının türetin şu anda `BasePage` program aracılığıyla çalışma zamanında içerik sayfasının ana sayfa ayarlar sınıfı. Bu Yönetim bölümünde içerik sayfaları için istiyoruz davranış değildir. Bunun yerine, her zaman kullanmak için bu içerik sayfaları istiyoruz `AdminNested.master` sayfası. Çalışma zamanında sağ üst düzey içerik sayfasını seçmek için iç içe geçmiş ana sayfa sorumluluğunda olacaktır.

Elde etmek için en iyi şekilde bu davranışı adlı yeni bir özel ana sayfa sınıfı oluşturmak için istenen `AdminBasePage` genişleten `BasePage` sınıfı. `AdminBasePage`ardından kılabilirsiniz `SetMasterPageFile` ve `Page` nesnenin `MasterPageFile` sabit kodlanmış değerine "~ / Admin/AdminNested.master". Bu şekilde, herhangi bir sayfayı, türer `AdminBasePage` kullanacağı `AdminNested.master`, herhangi bir sayfayı, türetilen ancak `BasePage` olacaktır kendi `MasterPageFile` özelliğini ayarlamak için dinamik olarak ya da "~ / Site.master" veya "~ / Alternate.master" değerine göre `MyMasterPage` Oturum değişkeni.

Yeni bir sınıf dosyasına eklemeye başlayın `App_Code` adlı klasörü `AdminBasePage.vb`. Sahip `AdminBasePage` genişletmek `BasePage` ve daha sonra geçersiz `SetMasterPageFile` yöntemi. Bu yönteme atamak `MasterPageFile` değer "~ / Admin/AdminNested.master". Sınıfınızda bu değişiklikleri yaptıktan sonra dosya aşağıdakine benzer görünmelidir:


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Şimdi varolan içerik sayfaları bölümüne türetilen yönetim sağlamak ihtiyacımız `AdminBasePage` yerine `BasePage`. Arka plandaki kod sınıfı dosyaya her içerik sayfasında gidin `~/Admin` klasörünü ve bu değişiklik yapın. Örneğin, `~/Admin/Default.aspx` arka plandaki kod sınıfı bildiriminden değişeceğinden:


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

Hedef:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

Şekil 11 gösterilmektedir nasıl en üst düzey ana sayfa (`Site.master` veya `Alternate.master`), iç içe geçmiş ana sayfa (`AdminNested.master`), ve yönetim bölüm içerik sayfaları birbirleriyle.


[![İç içe geçmiş ana sayfa içerik Özel Yönetim bölümünde sayfalara tanımlar.](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Şekil 11**: iç içe geçmiş ana sayfa tanımlar içerik belirli sayfalara Yönetim bölümünde ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>6. adım: yansıtma ana sayfasının genel yöntemler ve Özellikler

Sözcüğünün `~/Admin/AddProduct.aspx` ve `~/Admin/Products.aspx` sayfalarını program aracılığıyla etkileşim ana sayfa ile: `~/Admin/AddProduct.aspx` çağrıları ana sayfa ortak `RefreshRecentProductsGrid` yöntemi ve kümelerini kendi `GridMessageText` özelliği; `~/Admin/Products.aspx` için bir olay işleyicisi sahip `PricesDoubled` olay. Önceki öğreticide oluşturduğumuz bir `MustInherit` `BaseMasterPage` bu ortak üyeleri tanımlı sınıfı.

`~/Admin/AddProduct.aspx` Ve `~/Admin/Products.aspx` sayfaları varsayın kendi ana sayfa öğesinden türetilen `BaseMasterPage` sınıfı. `AdminNested.master` Sayfasında, ancak şu anda genişletir `System.Web.UI.MasterPage` sınıfı. Ziyaret eden sonuç olarak, `~/Admin/Products.aspx` bir `InvalidCastException` iletiyle oluşturulur: "nesne türünün ' ASP.admin\_adminnested\_ana ' 'BaseMasterPage' türüne."

Bunu sağlamak için ihtiyacımız düzeltmek için `AdminNested.master` arka plandaki kod sınıfı genişletmeniz `BaseMasterPage`. İç içe geçmiş ana sayfa arka plandaki kod sınıfı bildiriminden güncelleştirin:


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

Hedef:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Biz henüz tamamlanmadı. Olarak işaretlenmiş üyeleri geçersiz kılmak ihtiyacımız `MustOverride`, yani `RefreshRecentProductsGrid` ve `GridMessageText`. Bu üyeler, üst düzey ana sayfalar tarafından kullanıcı arabirimlerini güncelleştirmek için kullanılır. (Yalnızca gerçekten, `Site.master` ana sayfayı kullanan bu yöntemlerin her ikisi de genişletmek bu yana en üst düzey her iki ana sayfalar bu yöntemleri uygulamak rağmen `BaseMasterPage`.)

Bu üye uygulamak ihtiyacımız sırada `AdminNested.master`, bu uygulamaları yapmanız gereken tek şey yalnızca iç içe geçmiş ana sayfa tarafından kullanılan en üst düzey ana sayfa aynı üye çağırın. Örneğin, bir içerik sayfasını Yönetim bölümünde çağırdığında iç içe geçmiş ana sayfa `RefreshRecentProductsGrid` yöntemi, tüm iç içe geçmiş ana sayfa gereken yapmak için sırayla, çağrı `Site.master` veya `Alternate.master`'s `RefreshRecentProductsGrid` yöntemi.

Bunun için aşağıdakileri ekleyerek başlayın `@MasterType` üstüne yönerge `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Sözcüğünün `@MasterType` yönergesi adlı arka plandaki kod sınıfı kesin türü belirtilmiş bir özellik ekler `Master`. Daha sonra geçersiz `RefreshRecentProductsGrid` ve `GridMessageText` üyeleri ve yalnızca çağrısı temsilci `Master`yöntemi ilgili:


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Yerinde şu kodla ziyaret edip içerik sayfaları Yönetim bölümünde olmalıdır. Şekil 12 gösterir `~/Admin/Products.aspx` sayfasında bir tarayıcıdan görüntülendiğinde. Gördüğünüz gibi sayfa iç içe geçmiş ana sayfada tanımlı yönetim yönergeleri kutusunu içerir.


[![İçerik sayfaları Yönetim bölümünde her sayfanın üst kısmındaki yönergeleri içerir](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Şekil 12**: üst her sayfasının Yönetim bölümüne dahil yönergeleri içerik sayfalarında ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>7. adım: çalışma zamanında uygun en üst düzey ana sayfa kullanma

Yönetim bölümünde tüm içerik sayfalarına tam olarak işlevsel olsa da, bunlar tümü aynı üst düzey ana sayfa kullanan ve bir kullanıcı tarafından seçilen ana sayfayı yoksay `ChooseMasterPage.aspx`. İç içe geçmiş ana sayfa sahip due için bu davranış olabilir, `MasterPageFile` özelliği statik olarak ayarlanırsa `Site.master` içinde kendi `<%@ Master %>` yönergesi.

İhtiyacımız ayarlamak için son kullanıcı tarafından seçilen en üst düzey ana sayfa kullanmak için `AdminNested.master`'s `MasterPageFile` özellik değerine `MyMasterPage` oturum değişkeni. İçerik sayfaları ayarlarız çünkü `MasterPageFile` özelliklerinde `BasePage`, iç içe geçmiş ana sayfa ayarlarsınız düşünebilirsiniz `MasterPageFile` özelliğinde `BaseMasterPage` veya `AdminNested.master`ait arka plandaki kod sınıfı. Ayarlamış olmanız gerekir çünkü bu, ancak çalışmaz `MasterPageFile` PreInit aşama sonuna özelliği. Biz program aracılığıyla bir ana sayfa sayfa yaşam döngüsü içine dokunabilirsiniz en erken zaman (PreInit aşama sonra oluşan) Init aşamasıdır.

Bu nedenle, iç içe geçmiş ana sayfa ayarlamak ihtiyacımız `MasterPageFile` içerik sayfalarından özelliği. Yalnızca içerik kullanan sayfaları `AdminNested.master` ana sayfa türetilen `AdminBasePage`. Bu nedenle, bu mantığı buraya koyabilirsiniz. Adım 5'te biz geçersiz kılınmış `SetMasterPageFile` yöntemi, sayfa nesnenin ayarı `MasterPageFile` özelliğine "~ / Admin/AdminNested.master". Güncelleştirme `SetMasterPageFile` ana sayfanın da ayarlamaya `MasterPageFile` oturumunda depolanmış sonucu özelliğine:


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

`GetMasterPageFileFromSession` İçin eklediğimiz yöntemi `BasePage` sınıfı önceki öğreticide uygun ana sayfa dosya yoluna bağlı olarak oturum değişken değeri döndürür.

Bu değişiklikle yerinde kullanıcının ana sayfa seçimini Yönetim bölümüne taşır. Şekil 13 gösteren Şekil 12 olarak, ancak kullanıcı kendi ana sayfa seçimi değiştikten sonra aynı sayfa `Alternate.master`.


[![İç içe geçmiş yönetim sayfasının en üst düzey ana sayfa kullanıcı tarafından seçilen kullanır](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Şekil 13**: iç içe yönetim sayfasının en üst düzey ana sayfası seçilen kullanıcı tarafından kullanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](nested-master-pages-vb/_static/image39.png))


## <a name="summary"></a>Özet

LIKE kadar nasıl içerik sayfalarına ana sayfaya bağlayabilirsiniz, iç içe geçmiş oluşturmak mümkündür alt ana sayfa sahip ana sayfalar üst ana sayfaya bağlayın. Alt ana sayfa, her grubun üst ContentPlaceHolders için içerik denetimleri tanımlamak; kendi ContentPlaceHolder denetimleri (yanı sıra diğer biçimlendirme)'nı, ardından bu içerik denetimlerine ekleyebilirsiniz. İç içe geçmiş ana sayfalar burada tüm sayfaları kapsayıcı bir görünüm paylaşmak henüz site belirli bölümlerini benzersiz özelleştirmelere ihtiyaç büyük web uygulamalarında oldukça faydalıdır.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İç içe geçmiş ASP.NET ana sayfalar](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [İç içe geçmiş ana sayfalar ve VS 2005 tasarım zamanı için ipuçları](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Ana sayfa desteği VS 2008 iç içe geçmiş](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar birden çok ASP/ASP.NET books ve 4GuysFromRolla.com kurucusu, 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Tan adresindeki ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](specifying-the-master-page-programmatically-vb.md)
