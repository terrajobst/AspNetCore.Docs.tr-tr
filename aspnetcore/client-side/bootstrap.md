---
title: "Önyükleme güzel, yanıt veren siteleriyle oluşturma"
author: ardalis
description: 
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bd27832c-2877-4b7b-9337-e009361d845f
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bootstrap
ms.openlocfilehash: f89ad584600c3f12a936599de27f931aff0cd4b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="building-beautiful-responsive-sites-with-bootstrap"></a>Önyükleme güzel, yanıt veren siteleriyle oluşturma

<a name="bootstrap-index"></a>

Tarafından [Steve Smith](https://ardalis.com/)

Önyükleme şu anda yanıt veren web uygulamaları geliştirmek için en popüler web çerçevesidir. Yeni ön uç tasarım ve geliştirme ya da bir uzman olsanız birçok özellik ve web sitenizi kullanıcılarınızın deneyiminizi iyileştirebilir avantajları sunar. Önyükleme CSS ve JavaScript dosyaları kümesi olarak dağıtılır ve Web sitesinin veya uygulamanın ölçek verimli bir şekilde phone'lardan masaüstlerine tabletler yardımcı olmak için tasarlanmıştır.

## <a name="getting-started"></a>Başlarken

Önyükleme ile çalışmaya başlamak için birkaç yolu vardır. Visual Studio'da yeni bir web uygulaması başlatıyorsanız, servis talebi önyükleme önceden yüklenmiş gelecektir ASP.NET Core için varsayılan başlangıç şablonu seçebilirsiniz:

![Başlangıç şablonu çözüm görünümünde bootstrap](bootstrap/_static/bootstrap-in-starter-template.png)

Bir ASP.NET Core önyükleme ekleme proje için ekleme sorunudur *bower.json* bağımlılık olarak:

[!code-json[Main](../common/samples/WebApplication1/bower.json?highlight=5)]

Bu, bir ASP.NET Core projeye önyükleme eklemek için önerilen yoludur.

Önyükleme Bower, npm veya NuGet gibi birkaç paket yöneticileri birini kullanarak da yükleyebilirsiniz. Her durumda, işlem temelde aynıdır:

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
> ASP.NET Core önyükleme Bower gibi istemci tarafı bağımlılıklarını yüklemek için önerilen yol (kullanarak *bower.json*, yukarıda gösterildiği gibi). Npm/NuGet kullanımını önyükleme ASP.NET önceki sürümleri dahil olmak üzere web uygulamalarının diğer türleri için nasıl kolayca eklenebilen göstermek için gösterilir.

Önyükleme kendi yerel sürümlerini başvuran kullanacağı herhangi sayfalarında başvuru gerekir. Üretimde CDN kullanarak önyükleme başvuruda bulunmalıdır. Varsayılan ASP.NET sitesi şablonunda *_Layout.cshtml* dosya böylece şöyle:

[!code-html[Main](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Önyükleme'nın jQuery eklentileri birini kullanarak kullanacaksanız, jQuery başvuru gerekir.

## <a name="basic-templates-and-features"></a>Temel şablonları ve özellikleri

En temel önyükleme şablonu çok benzer görünür *_Layout.cshtml* gösterilen dosya yukarıdaki ve yalnızca gezinti ve sayfa kalan işlemek için bir yer için temel bir menü içerir.

### <a name="basic-navigation"></a>Temel gezinme

Bir dizi varsayılan şablonu kullanan `<div>` üst gezinti çubuğu ve ana sayfa gövdesini işlemek için öğeleri. HTML5 kullanıyorsanız, ilk değiştirebilirsiniz `<div>` ile etiketi bir `<nav>` aynı etkiye almak için etiket ancak daha kesin semantiği ile.  Bu ilk içinde `<div>` birkaç başka vardır görebilirsiniz. İlk olarak, bir `<div>` "kapsayıcı" ve ardından, iki fazla içinde sınıfı ile `<div>` öğeleri: "navbar-üstbilgisi" ve "navbar-Daralt".  Gezinti çubuğu üstbilgi div ekran 3 yatay çizgi gösteren bir belirli en küçük genişliği, olduğunda görünecek bir düğme içerir (bir sözde "hamburger simgesi"). Simge saf HTML ve CSS kullanılarak işlenir; hiçbir görüntü gereklidir. Bu, her biriyle simgesi görüntüler koddur <span> beyaz çubukların birini işleme etiketler:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Ayrıca, sol üst görünen uygulama adını içerir.  Ana Gezinti menüsünde tarafından işlenen `<ul>` ikinci div öğesinde ve hakkında ev ve kişi için bağlantılar içerir. Kayıt ve oturum açma için ek bağlantılar, 29 satırındaki _LoginPartial satırına eklenir. Gezinti her sayfasının ana gövdesi içinde başka bir işlenmeden `<div>`, "kapsayıcı" ve "İçerik gövdesi" sınıflarıyla işaretli. Burada gösterilen basit varsayılan _Layout dosyasında sayfasının içeriği sayfa ve basit bir sonra ilişkili belirli görünüm tarafından işlenen `<footer>` sonuna eklenen `<div>` öğesi.  Sayfa hakkında yerleşik kullanarak nasıl göründüğünü görebilirsiniz bu şablonu:

![Sayfa hakkında](bootstrap/_static/about-page-wide.png)

Pencerenin belirli genişliği düştüğünde sağ, üst "hamburger" düğmesiyle daraltılmış gezinti çubuğu görüntülenir:

![hamburger menüsüyle sayfası hakkında](bootstrap/_static/about-page-hamburger.png)

Simgeye tıklayarak sayfa üstten aşağı slayt dikey bir bölümü menü öğelerinde ortaya çıkarır:

![Genişletilmiş hamburger menüsüyle sayfası hakkında](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Tipografi ve bağlantıları

Önyükleme sitenin temel tipografi, renklerini ve biçimlendirmeyi kendi CSS dosyasında bağlantı kurar. Varsayılan stiller tablolar, düğmeleri, form öğeleri, görüntüler ve daha fazla bilgi için bu CSS dosyası içerir ([daha fazla bilgi edinin](http://getbootstrap.com/css/)). Özellikle yararlı bir özellik sonraki kapsamdaki Kılavuz düzeni sistemidir.

### <a name="grids"></a>Kılavuzları

En popüler özelliklerinden önyükleme biri kendi Kılavuz düzeni sistemidir. Modern web uygulamaları kaçının kullanarak `<table>` bunun yerine bu öğe kullanımını gerçek tablo verilere erişimi kısıtlayarak düzeni için etiketi. Bunun yerine, satırları ve sütunları bir dizi kullanarak düzenlendiği `<div>` öğeleri ve uygun CSS sınıfları. Bu yaklaşımın dikey dar ekranlarda gibi telefonlarda görüntülemek için kılavuzları düzenleme yeteneğini de dahil olmak üzere, çeşitli avantajları vardır.

[Önyükleme'nın Kılavuz düzeni sistem](http://getbootstrap.com/css/#grid) on iki sütuna bağlıdır. Bu sayı 1, 2, 3 veya 4 sütun eşit ayrılabilir ve sütun genişliklerini gösterebilir için 1 içinde seçildi, çünkü/dikey ekran genişliği 12. Kılavuz düzeni sistem kullanmaya başlamak için bir kapsayıcı ile başlaması gereken `<div>` ve Satır Ekle `<div>`, aşağıda gösterildiği gibi:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Sonra ek ekleyin `<div>` her sütun için öğeleri ve sütun sayısını belirtin, `<div>` (12 dışında) "sütun - md-" ile başlayan bir CSS sınıfı bir parçası olarak bulunması gereken. Örneğin, yalnızca iki sütun eşit boyutta olmasını istiyorsanız, her biri için "sütun-md-6" sınıfının kullanın. Bu durumda "md" kısaltması "Orta"'dır ve masaüstü bilgisayar standart boyutlu görüntü boyutları başvurur. Aralarından seçim yapabileceğiniz dört farklı seçenekler vardır ve her için daha yüksek genişlikleri (ekran genişliği bağımsız olarak düzeltilmesi düzeni istiyorsanız, yalnızca xs sınıfları belirtebilmeniz) geçersiz kılınmadığı sürece kullanılır.

CSS sınıfı öneki | Cihaz katmanı | Genişlik
:---: | :---: | :---:
Sütun-xs - | Telefonları | < 768px
Sütun-sm - | Tabletler | > = 768px
Sütun-md - | Masaüstü bilgisayarlar | > = 992px
Sütun-lg - | Daha büyük Masaüstü görüntüler | > = 1200 piksel

İki sütun hem "sütun-md-6" elde edilen düzeni Masaüstü çözünürlüklerde iki sütun olacaktır, ancak bu iki sütun küçük aygıtlar (veya bir masaüstü üzerinde daha dar bir tarayıcı penceresi), kullanıcıların kolayca görüntülemesine izin vererek işlendiğinde dikey yığın belirtirken Yatay kaydırma gerek olmadan içeriği.

Yalnızca birden fazla sütun istediğinizde sütunları belirlemek gereken şekilde önyükleme her zaman bir tek sütun düzeni için varsayılan olarak ayarlanır. Açıkça belirtmek için isteyeceğiniz yalnızca bir kez bir `<div>` tüm 12 sütunları Al daha büyük bir aygıt katmanı davranışını geçersiz kılmak için olacaktır. Birden çok aygıt katmanı sınıf belirtirken, bazı noktalarda sütun işleme sıfırlamanız gerekebilir. Yalnızca bir belirli görünüm penceresinin içinde görülebilir clearfix div ekleme, aşağıda gösterildiği gibi elde edebilirsiniz:

![dar ve geniş görünüm penceresinin kılavuz](bootstrap/_static/narrow-and-wide-viewport-grid.png)

İkinci ve üçüncü bir satır "xs" düzeninde paylaşmak sırada yukarıdaki örnekte, birinci ve ikinci "md" düzeninde bir satır paylaşır. Clearfix olmadan `<div>`, ikinci ve üçüncü gösterilmiyor doğru "xs" view (tek, dört ve beş gösterilen unutmayın):

![clearfix kullanmadan kılavuz](bootstrap/_static/grid-without-clearfix.png)

Bu örnekte, yalnızca tek bir satır `<div>` kullanılan ve önyükleme hala çoğunlukla düzenini ve sütunlarını yığınlama açısından doğru şeyi vermedi. Genellikle, bir satır belirtmelisiniz `<div>` yatay her satır için düzeninizi gerektirir ve bir diğer içinde önyükleme kılavuzları Elbette yerleştirebilirsiniz. Bunu yaptığınızda, her iç içe geçmiş kılavuz sonra sütun sınıflarını kullanarak alt bölümlere ayrılabilir, yerleştirildiği, öğenin genişliğine % 100'ünü kaplar.

### <a name="jumbotron"></a>Jumbotron

Visual Studio 2012 ya da 2013 varsayılan ASP.NET MVC şablonları kullandıysanız, büyük olasılıkla eylem Jumbotron gördünüz. Büyük arka plan görüntüsü, eylem, bir değiştirici veya benzer öğeleri için bir çağrı görüntülemek için kullanılan bir sayfanın büyük bir tam genişlik bölümünü ifade eder. Bir sayfaya bir jumbotron eklemek için yalnızca ekleyin bir `<div>` ve "jumbotron" sınıfının verin, sonra bir kapsayıcı yerleştirin `<div>` içinde ve içeriğinizi ekleyin.  Biz, standart bir jumbotron görüntülediği ana başlıkları için kullanmak üzere sayfa hakkında kolayca ayarlayabilirsiniz:

![jumbotron örneği](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Düğmeleri

Varsayılan düğme sınıflar ve onların renkleri aşağıdaki çizimde gösterilmektedir.

![konulu düğmeleri](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Rozetleri

Gezinti öğesinin yanındaki küçük, genellikle sayısal çağrılar rozetleri bakın. Bunlar, iletileri veya bekleyen bildirimleri sayısı veya güncelleştirmeleri varlığını gösterebilir. Bu tür rozetleri belirtme ekleme olarak basit bir <span> "rozet" sınıfının ile metni içeren:

![konulu rozetleri](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Uyarılar

Bazı tür bildirimi, uyarı veya hata iletisi, uygulamanızın kullanıcılara gösterilecek gerekebilir. Standart uyarı sınıfları yararlı olduğu olmasıdır.  İlişkili renk düzenleri ile dört farklı önem düzeyleri şunlardır:

![konulu uyarıları](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Gezinti çubukları ve menüleri

Bizim düzeni zaten standart navbar içerir, ancak önyükleme temasının ek stil seçeneklerini destekler. Biz de kolayca yerine, tercih edilen değilse, açılan menüler yatay olarak eklemeyi de alt gezinti öğelerini navbar dikey olarak görüntülenecek tercih edebilirsiniz. Sekme şeritler gibi basit Gezinti menüleri üstünde oluşturulur <ul> öğeleri. Bu, çok yalnızca yalnızca bunları CSS sınıfları "nav" ve "sekmeleri nav" sağlayarak oluşturulabilir:

![konulu tabstrips](bootstrap/_static/theme-tabstrips.png)

Gezinti çubukları benzer şekilde oluşturulur, ancak biraz daha karmaşıktır.  İle başlayan bir `<nav>` veya `<div>` "navbar", bir kapsayıcı div içinde öğeleri geri kalanı tutan sınıfı ile. Üstbilgisinde bir gezinti çubuğu sayfamızı zaten içerir – aşağıda gösterilene yalnızca bu bilgisayarda bir açılır menü için destek ekleyen genişletir:

![konulu gezinti çubukları](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Ek öğeler

Varsayılan tema, HTML tabloları şeritli görünümleri için destek dahil olmak üzere, iyi biçimlendirilmiş bir stil sunmak için de kullanılabilir. Bu düğmelerin benzer stilleri etiketlerle vardır. Destek standart HTML ötesinde ek stile seçenekleri özel aşağı açılan menüler oluşturabilirsiniz `<select>` gezinti çubukları gibi varsayılan başlangıç sitemizi zaten kullanmakta birlikte öğesi. Bir ilerleme çubuğu gerekiyorsa, grupları listelemesine ve başlık ve içerik dahil panelleri yanı sıra aralarından seçim yapabileceğiniz çeşitli stilleri vardır.  Ek seçenekler burada standart önyükleme tema içinde keşfedin:

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Daha fazla temaları

Standart önyükleme Tema renkleri ve kendi uygulamanızın gereksinimlerine uyacak şekilde stilleri ayarlama bazılarını veya tümünü kendi CSS geçersiz kılarak genişletebilirsiniz. Hazır bir temayı başlatmak istiyorsanız, çevrimiçi, (ticari Temalar çeşitli olan) WrapBootstrap.com ve (ücretsiz Temalar sunan) Bootswatch.com gibi önyükleme Temalar specialize kullanılabilir birkaç tema galerileri vardır.  Kullanılabilir Ücretli şablonları bazıları işlevselliği yönetim menüleri ve panolar için zengin destek gibi temel önyükleme temasının üzerinde büyük bir bölümünü zengin tablolar ve göstergeleri sağlar. Popüler Ücretli şablon örneği Inspinia, satış $ AngularJS ve statik HTML sürümleri ek olarak ASP.NET MVC5 şablonu içeren 18 için şu anda içindir. Bir örnek ekran görüntüsü aşağıda gösterilmiştir.

![Örnek tema inspinia](bootstrap/_static/theme-inspinia.png)

Önyükleme temasının değiştirmek istiyorsanız, put *bootstrap.css* istediğiniz tema dosya **wwwroot/css** klasörü ve içindeki başvuruların değiştirmek *_Layout.cshtml* Bu işaret edecek şekilde.  Tüm ortamlar için bağlantıları değiştirin:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Kendi Pano oluşturmak istiyorsanız, kullanılabilir boş örnekten burada başlatabilirsiniz: [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Bileşenler

Zaten ele alınan bu öğeleri ek olarak, önyükleme çeşitli destekler [yerleşik UI bileşenlerini](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Önyükleme Glyphicons simgesi kümesinden içerir ([http://glyphicons.com](http://glyphicons.com)), 200'den simgelerle önyükleme etkin web uygulamanızı içinde kullanmak için ücretsiz olarak kullanılabilir. Aşağıda, yalnızca küçük bir örnek verilmiştir:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Giriş grupları

Ek metin ya da bir input öğesi düğmeleriyle paketleme giriş grupları kullanıcının daha sezgisel bir deneyim sağlayan izin ver:

![Giriş grupları](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>İçerik haritası

İçerik bir kullanıcı kendi en son geçmişini ya da bir sitenin gezinti hiyerarşi içinde derinliği göstermek için kullanılan ortak bir UI bileşeni var. Bunları "içerik haritası" sınıfı için uygulama tarafından kolayca eklemenize `<ol>` liste öğesi. "Sayfalandırma" sınıfı kullanarak sayfa numaralandırma için yerleşik destek içerir bir `<ul>` öğesi içinde bir `<nav>`. Esnek Katıştırılmış slayt gösterilerini ve video kullanarak Ekle `<iframe>`, `<embed>`, `<video>`, veya `<object>` önyükleme otomatik olarak stil öğeleri. Belirli bir en boy oranı "katıştırmak-esnek-16by9" gibi belirli sınıfları kullanarak belirtin.

## <a name="javascript-support"></a>JavaScript desteği

Önyükleme'nın JavaScript kitaplığı davranışlarını uygulamanız içinde program aracılığıyla denetlemenize izin vererek dahil bileşenleri için API desteği içerir. Ayrıca, *bootstrap.js* üzerinde bir düzine içerir geçişleri, kalıcı iletişim kutuları gibi ek özellikler sağlayan özel jQuery eklentileri kaydırma algılama, (burada kullanıcı belgede kaydırılan üzerinde göre stil güncelleştirme) ekranı kaydırarak olmayan şekilde davranışı, karusel'leri ve affixing menüleri penceresine daraltın. Lütfen daha fazla bilgi edinmek için önyükleme – yerleşik JavaScript eklentileri tümünün karşılamak için yeterli alan yok [http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Özet

Önyükleme hızlı ve verimli bir şekilde düzenlemeniz ve çok çeşitli Web sitelerinin ve uygulamaların stilini belirlemek için kullanılan bir web çerçevesidir sağlar. Temel tipografi ve stilleri bir eğlenceli, elle hazırlanmış veya ticari olarak satın alınan özel tema destek kolayca yönetilebilir görünüm sağlar. Bir konak geçmişte pahalı üçüncü taraf denetimleri gerçekleştirmek, açık ve modern web standartları desteklerken için şunlar gerekir web bileşenleri destekler.
