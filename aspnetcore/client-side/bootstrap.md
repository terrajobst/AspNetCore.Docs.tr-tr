---
title: Önyükleme ve ASP.NET Core ile güzel, yanıt veren siteler oluşturma
author: ardalis
description: ASP.NET Core ile hızlı yanıt veren web uygulamaları geliştirmek için önyükleme kullanmayı öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: client-side/bootstrap
ms.openlocfilehash: 1ccf33b299739f5aa963a53feb70b44b290443ca
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753878"
---
# <a name="build-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>Önyükleme ve ASP.NET Core ile güzel, yanıt veren siteler oluşturma

<a name="bootstrap-index"></a>

Tarafından [Steve Smith](https://ardalis.com/)

Önyükleme şu anda hızlı yanıt veren web uygulamaları geliştirmek için en popüler web çerçevesidir. Yeni ön uç tasarım ve geliştirme ya da bir uzman olmanızdan bir dizi özellik ve kullanıcılarınızın deneyimini web sitenizle avantajları sunar. Önyükleme CSS ve JavaScript dosyalarına bir dizi dağıtılır ve Web sitenize veya uygulamanıza ölçek verimli bir şekilde phone'lardan masaüstlerine tabletler yardımcı olmak için tasarlanmıştır.

## <a name="get-started"></a>Kullanmaya başlayın

Bootstrap ile kullanmaya başlamanın birkaç yolu vardır. Visual Studio'da yeni bir web uygulaması başlatılıyor, büyük/küçük harf önyükleme önceden yüklenmiş gelir ASP.NET Core için varsayılan başlangıç şablonu seçebilirsiniz:

![Başlangıç şablonu Çözüm Görünümü'nde önyükleme](bootstrap/_static/bootstrap-in-starter-template.png)

Önyükleme eklemek için bir ASP.NET Core projesi için ekleme sorunudur *bower.json* bağımlılık olarak:

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

Önyükleme için bir ASP.NET Core projesi eklemek için önerilen yöntem budur.

Önyükleme birkaç paket yöneticileri, Bower, npm ve NuGet gibi birini kullanarak da yükleyebilirsiniz. Her durumda, işlem temelde aynıdır:

### <a name="bower"></a>Bower

```console
bower install bootstrap
```

### <a name="npm"></a>npm

```console
npm install bootstrap
```

### <a name="nuget"></a>NuGet

```console
Install-Package bootstrap
```

> [!NOTE]
> ASP.NET core'da önyükleme Bower gibi istemci tarafı bağımlılıkları yüklemeniz için önerilen yöntem (kullanarak *bower.json*, yukarıda gösterildiği gibi). Npm/NuGet kullanımı, Bootstrap, ASP.NET'in önceki sürümlerinde de dahil olmak üzere web uygulamalarının diğer türleri için nasıl bir kolayca eklenebilir göstermek için gösterilir.

Kendi yerel önyükleme sürümlerini başvuran kullanacak olan tüm sayfaları başvurmak gerekir. Üretim ortamında bir CDN kullanarak önyükleme başvurmalıdır. Varsayılan ASP.NET Core sitesi şablonunda *_Layout.cshtml* dosya şekilde şöyle:

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Herhangi bir önyükleme'nın jQuery eklentilerini kullanarak kullanacaksanız, jQuery başvuru gerekir.

## <a name="basic-templates-and-features"></a>Temel şablonları ve özellikleri

En temel önyükleme şablonu çok benzer görünür *_Layout.cshtml* gösterilen dosya yukarıdaki ve yalnızca gezinti ve sayfanın geri kalanını işlemek için bir yer için temel bir menü içerir.

### <a name="basic-navigation"></a>Temel gezinme

Bir dizi varsayılan şablon kullanan `<div>` öğeleri bir üst gezinti çubuğunda ve ana sayfa gövdesi, oluşturulacak. HTML5'i kullanıyorsanız, ilk değiştirebilirsiniz `<div>` etiketi ile bir `<nav>` aynı etkiyi görmek için etiket ancak daha kesin semantiğe sahip. Bu ilk içinde `<div>` birkaç başka vardır görebilirsiniz. İlk olarak, bir `<div>` "container" ve ardından, iki daha fazla içinde bir sınıf ile `<div>` öğeleri: "navbar-header" ve "gezinti çubuğunu Daralt". Üst gezinti çubuğunda div 3 yatay çizgi gösteren belirli en az genişliği, ekrandaki olduğunda görünecek bir düğme içerir (sözde bir "hamburger simgesini"). Simgesi, saf HTML ve CSS kullanarak işlenir; hiçbir görüntü gereklidir. Bu, her biri ile bir simge görüntüler kodudur <span> beyaz çubuklardan birine işleme etiketler:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Ayrıca, sol üst köşedeki görünen uygulama adını içerir. Ana Gezinti menüsünü tarafından işlenen `<ul>` ikinci div öğesinde ve yaklaşık, ev ve ilgili kişi yönelik bağlantılar içerir. Gezinti başka ana gövdesi her sayfanın işlenen `<div>`"container" ve "İçerik gövdesi" sınıfları ile işaretli. Basit varsayılan \_sayfasının içeriğini sayfası, ardından basit ile ilişkili olan görünüme göre işlenir, burada gösterilen Düzen dosyası `<footer>` sonuna eklenir `<div>` öğesi. Yerleşik sayfasını kullanarak nasıl göründüğünü görebilirsiniz. Bu şablonu:

![Sayfa hakkında](bootstrap/_static/about-page-wide.png)

Pencerenin belirli bir genişlik düştüğünde daraltılmış gezinti, sağ, üst "hamburger" düğmesi görünür:

![hamburger menü hakkında sayfası](bootstrap/_static/about-page-hamburger.png)

Simgesine tıklayarak dikey çekmecede, sayfanın üst kısmından aşağı slayt menü öğeleri gösterir:

![hamburger menü sayfasıyla ilgili genişletilmiş](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Tipografi ve bağlantılar

Önyükleme sitenin temel tipografi, renk ve biçimlendirme, CSS dosyası içinde bağlantı kurar. Bu CSS dosyası varsayılan stillerini tablolar, düğmeler, form öğeleri, görüntüleri ve daha fazlasını içerir ([daha fazla bilgi edinin](http://getbootstrap.com/css/)). Bir özellikle yararlı sonraki kapsamdaki kılavuz düzen sistemi özelliğidir.

### <a name="grids"></a>Kılavuzları

En yaygın özellikleri, Bootstrap, kılavuz düzen sistemi biridir. Modern web uygulamaları kullanarak kaçınmak `<table>` düzeni, bunun yerine bu öğenin kullanılması gerçek tablosal verilere erişimi kısıtlayarak için etiket. Bunun yerine, sütunları ve satırları bir dizi kullanarak düzenlendiği `<div>` öğeleri ve uygun CSS sınıfları. Bu yaklaşımın dikey dar ekranlarda gibi telefonlarda görüntülenecek Kılavuzlar düzenini ayarlama olanağı dahil olmak üzere, çeşitli avantajları vardır.

[Önyükleme'nın kılavuz düzen sistemi](http://getbootstrap.com/css/#grid) on iki sütuna göre. Bu sayı 1, 2, 3 veya 4 sütunlar eşit bir şekilde ayrılabilir ve sütun genişliklerini değişebilir için 1 içinde seçilen/12 ekranın dikey genişliği. Kılavuz düzen sistemi kullanmaya başlamak için bir kapsayıcı ile başlaması gereken `<div>` ve sonra bir satır ekleyin `<div>`, burada gösterildiği gibi:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Ardından, ek ekleyin `<div>` her sütun için öğeleri ve sütun sayısını belirtin, `<div>` (12 dışında), "- md - sütun" ile başlayan bir CSS sınıfı bir parçası olarak kaplayacağı. Örneğin, eşit boyutta iki sütun olmasını istiyorsanız, her biri için "sütun-md-6" sınıfının kullanın. Bu durumda "md" kısaltması "Orta"'dır ve masaüstü bilgisayar standart boyutlu görüntü boyutları için ifade eder. Seçebileceğiniz dört farklı seçenekler vardır ve her daha yüksek genişliği (ekran genişliği bağımsız olarak düzeltilmesi düzenini istiyorsanız, yalnızca xs sınıfları belirtebilmeniz) geçersiz kılınmadığı sürece kullanılır.

CSS sınıfı ön eki | Cihaz katmanı | Genişlik
:---: | :---: | :---:
Sütun-xs - | Telefonlar | < 768px
Sütun-sm - | Tabletler | > 768px =
Sütun-md - | Masaüstü bilgisayarlar | > 992px =
Sütun-lg - | Daha büyük Masaüstü görüntüler | > = 1200 piksel

İki sütun hem "sütun-md-6" elde edilen düzeni ile masaüstü çözünürlükte, iki sütun olacaktır, ancak bu iki sütun daha küçük (veya bir masaüstü üzerinde daha dar bir tarayıcı penceresi), kullanıcıların kolayca görüntülemek işlendiğinde dikey yığın belirtirken Yatay kaydırma gerek olmadan içeriği.

Böylece, yalnızca birden fazla sütun istediğinizde sütunları belirlemek önyükleme her zaman tek sütunlu düzeni için varsayılan olarak kullanılır. Açıkça belirtmek için isteyeceğiniz yalnızca bir kez bir `<div>` daha büyük bir cihaz katmanı davranışını geçersiz kılmak için tüm 12 sütuna kaplamaya olacaktır. Birden çok cihaz katmanı sınıf belirtirken, bazı noktalarda sütun işleme sıfırlama gerekebilir. Yalnızca bir belirli görünüm penceresinin içinde görünür olan bir clearfix div ekleme, burada gösterildiği gibi bunu gerçekleştirebilirsiniz:

![dar ve geniş bir Görünüm penceresi kılavuz](bootstrap/_static/narrow-and-wide-viewport-grid.png)

İki ve üç satır "xs" düzenine paylaşmak sırada yukarıdaki örnekte, birinci ve ikinci bir satırda "md" Düzen paylaşın. Clearfix olmadan `<div>`, iki ve üç gösterilmiyor doğru "xs" Görünümü'nde (yalnızca biri, dört ve beş gösterilir unutmayın):

![clearfix kullanmadan kılavuz](bootstrap/_static/grid-without-clearfix.png)

Bu örnekte, yalnızca tek bir satır `<div>` kullanılan ve önyükleme hala çoğunlukla düzenini ve sütunlarını yığınlama doğru şeyi vermedi. Genellikle, bir satırı belirtmeniz gerekir `<div>` yatay her satır için düzeninizi gerektirir ve Elbette, önyükleme Kılavuzlar içinde başka bir iç içe yerleştirebilirsiniz. Bunu yaptığınızda her iç içe geçmiş kılavuz %100 ardından sütun sınıflarını kullanarak alt bölümlere ayrılabilir içine yerleştirilir, öğenin genişliğinin dolduracaktır.

### <a name="jumbotron"></a>Jumbotron

Visual Studio 2012 veya 2013'de varsayılan ASP.NET Core MVC şablonlarında kullandıysanız, büyük olasılıkla uygulamada Jumbotron gördünüz. Büyük arka plan görüntüsü, eylem, bir değiştirici veya benzer öğeleri görüntülemek için kullanılan bir sayfanın büyük bir tam genişlikli bölümüne başvurur. Bir sayfaya bir jumbotron eklemek için basitçe ekleyin bir `<div>` ve "jumbotron" sınıfının bekledikten sonra bir kapsayıcıya Yerleştir `<div>` içinde ve içeriğinizi ekleyin. Standart bir jumbotron görüntüler ana başlıkları için kullanılacak bir sayfa hakkında size bir kolayca yapabilirsiniz:

![jumbotron örneği](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Düğmeleri

Varsayılan düğme sınıfları ve öğe renklerinin aşağıdaki çizimde gösterilmektedir.

![temalı düğmeleri](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Rozetleri

Bir gezinti öğesinin yanındaki küçük, genellikle sayısal çağrılar rozetleri bakın. Bunlar, iletileri veya bekleyen bildirimleri bir dizi veya güncelleştirmelerin varlığını belirtebilir. Böyle rozetleri belirtme ekleme olarak basit bir `<span>` "rozet" sınıfı ile metni içeren:

![temalı rozetleri](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Uyarılar

Bir tür bildirimi, uyarı veya hata iletisi, uygulamanızın kullanıcılara görüntülenecek gerekebilir. Standart uyarı sınıflarını yararlı olduğu olmasıdır. İle ilişkili renk düzenleri dört farklı önem düzeyleri şunlardır:

![temalı uyarıları](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Gezinti çubukları ve menüler

Bizim Düzen standart bir gezinti zaten içerir ancak önyükleme temasının ek stil seçeneklerini destekler. Biz de bir kolayca yerine tercih edilen, açılır menüleri yatay olarak eklemeyi de alt gezinti öğeleri gezinti dikey olarak görüntülenecek tercih edebilirsiniz. Üst kısmındaki sekme şeritler gibi basit Gezinti menüleri yerleşik `<ul>` öğeleri. Bu, çok basit yalnızca bunları CSS sınıfları "nav" ve "sekmeleri nav" sağlayarak oluşturulabilir:

![temalı tabstrips](bootstrap/_static/theme-tabstrips.png)

Gezinti çubukları benzer şekilde oluşturulur, ancak biraz daha karmaşıktır. Bunlar ile başlayan bir `<nav>` veya `<div>` "navbar", bir kapsayıcı div içinde kalan öğelerin tutan bir sınıfı ile. Kendi üst bilgisinde bir gezinti sayfamızı zaten içerir – aşağıda gösterilene yalnızca bu, bir açılan menü için destek ekleyerek genişletir:

![temalı gezinti çubukları](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Ek öğeler

Varsayılan tema HTML tabloları şeritli görünümleri için destek dahil olmak üzere, düzgün şekilde biçimlendirilmiş bir stil sunmak için de kullanılabilir. Bu düğmelerin benzer stiller etiket vardır. Standart HTML ötesinde ek stil seçeneklerini destekleyen özel açılan menüler oluşturabileceğiniz `<select>` öğesi, gezinti çubukları gibi varsayılan başlangıç sitemizi zaten kullanmakta yanı sıra. Bir ilerleme çubuğu gerekiyorsa, grupları ve bir başlık ve içerik dahil panelleri yanı sıra, aralarından seçim yapabileceğiniz çeşitli stili vardır. Standart önyükleme temasının burada ek seçenekleri keşfedin:

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Daha fazla tema

Standart önyükleme temasının kendi uygulama gereksinimlerinize uyacak şekilde stili ve renklerini ayarlama bazılarını veya tümünü kendi CSS geçersiz kılarak genişletebilirsiniz. Kullanıma hazır bir temayı başlatmak istiyorsanız, birden çok tema galeriler çevrimiçi, önyükleme temalar, (ticari Temalar çeşitli olan) WrapBootstrap.com ve (ücretsiz Temalar sunan) Bootswatch.com gibi specialize kullanılabilir vardır. Zengin grafikler ve göstergeler ile bazı Ücretli şablanların işlevleri temel önyükleme temasının yönetim menüleri ve panolar için zengin destek gibi temel alarak büyük ölçüde sağlar. Aşağıdaki ekran görüntüsünde gösterilen Inspinia popüler Ücretli şablon örneğidir:

![Örnek tema inspinia](bootstrap/_static/theme-inspinia.png)

Önyükleme temanızı değiştirmek istiyorsanız, yerleştirme *bootstrap.css* istediğiniz tema dosyası **wwwroot/css** klasörü içindeki başvurular değiştirip *_Layout.cshtml* Bu işaret edecek şekilde. Tüm ortamlar için bağlantıları değiştirin:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Kendi Pano oluşturmak istiyorsanız, buradan kullanılabilir ücretsiz örnekten başlayın: [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Bileşenler

Zaten ele alınan bu öğelere ek olarak, önyükleme çeşitli için destek içerir. [yerleşik UI bileşenlerini](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Önyükleme Glyphicons simgesi kümelerini içerir ([http://glyphicons.com](http://glyphicons.com)), 200'den fazla simgelerle önyükleme etkin web uygulamanızın içinde kullanmak için ücretsiz olarak kullanılabilir. Yalnızca küçük bir örnek aşağıda verilmiştir:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Giriş grupları

Ek metin ya da bir input öğesi düğmeleriyle paketleme giriş grupları daha sezgisel bir deneyim ile kullanıcı sağlama izin ver:

![Giriş grupları](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>İçerik haritası

İçerik Haritaları, en son geçmişini ya da bir sitenin Gezinti hiyerarşisi içinde derinliği bir kullanıcıya göstermek için kullanılan ortak bir kullanıcı Arabirimi bileşenidir. Herhangi bir "içerik haritası" sınıfı uygulayarak bunları kolayca ekleyin `<ol>` liste öğesi. "Sayfalandırma" sınıfı kullanarak sayfalandırma için yerleşik destek içerir. bir `<ul>` öğesi içinde bir `<nav>`. Kullanarak, hızlı yanıt veren Katıştırılmış slayt gösterileri ve video eklemek `<iframe>`, `<embed>`, `<video>`, veya `<object>` önyükleme otomatik olarak stil öğeleri. "Ekleme-hızlı yanıt veren-16by9" gibi belirli sınıfları kullanarak belirli bir en boy oranını belirtin.

## <a name="javascript-support"></a>JavaScript desteği

Önyükleme'nın JavaScript kitaplığı, davranışları uygulama içinde program aracılığıyla denetlemenize olanak sağlayan eklenen bileşenler için API desteği içerir. Ayrıca, *bootstrap.js* üzerinde bir düzine içeren özel jQuery eklentileri, geçişleri kalıcı iletişim kutuları gibi ek özellikler sağlayan kaydırma algılama, (burada kullanıcının belgeye kaydırılan üzerinde temel stilleri güncelleştirme) ekranın kaydırılması olmayan şekilde davranışı ve görüntülendiği döngüler yer affixing menüleri penceresine daraltın. Lütfen daha fazla bilgi edinmek için önyükleme – yerleşik JavaScript eklentilerin tümü için karşılamak için yeterli alan yok [ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Özet

Önyükleme hızla ve verimli bir şekilde düzenlemeniz ve çok çeşitli Web sitelerinin ve uygulamaların stilini belirlemek için kullanılan bir web çerçevesi sağlar. Stiller ve temel tipografi bir eğlenceli, elle oluşturulmuş veya ticari olarak satın alınan özel bir tema destek kolayca işlenebilir görünüm sağlar. Bu, çok sayıda, daha önce açık ve modern web standartları desteklerken gerçekleştirmek için pahalı üçüncü taraf denetimleri zorunlu web bileşenleri destekler.
