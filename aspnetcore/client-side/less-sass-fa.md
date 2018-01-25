---
title: "Daha az Sass ve ASP.NET Core harika yazı tipi"
author: ardalis
description: "Daha az Sass ve yazı tipi harika ASP.NET Core uygulamaları kullanmayı öğrenin."
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/less-sass-fa
ms.openlocfilehash: 764b11bbd301c0116488265d32f7d46dfc5bce27
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a>Daha az stil uygulamalarla, Sass ve yazı tipi harika ASP.NET Core içinde giriş

Tarafından [Steve Smith](https://ardalis.com/)

Stil ve genel denemek için kullanıcıların web uygulamalarının giderek yüksek beklentilerini vardır. Modern web uygulamaları sık zengin araçları ve tanımlama ve kendi görünüm tutarlı bir şekilde yönetmek için çerçeveleri yararlanın. Çerçeveler ister [önyükleme](http://getbootstrap.com/) stilleri ve düzen seçeneklerini web siteleri için ortak bir dizi tanımlama doğru uzun bir yol gidebilirsiniz. Bununla birlikte, çoğu Önemsiz olmayan siteler ayrıca sitenin arabirimi daha sezgisel hale gelmesine yardımcı görüntü olmayan simgeler kolay erişmesini yanı sıra etkili bir şekilde tanımlamak ve stilleri ve geçişli stil sayfası (CSS) dosyaları korumak için yararlı. Where olan dilleri ve Destek Araçları [daha az](http://lesscss.org/) ve [Sass](http://sass-lang.com/), kitaplıklar gibi ve [yazı tipi harika](http://fontawesome.io/), geldikçe.

## <a name="css-preprocessor-languages"></a>CSS önişlemci dilleri

Diğer dillere temel dili ile çalışmaya deneyimini geliştirmek için derlenmiş dillerde önişlemcilerini adlandırılır. CSS için iki popüler önişlemcilerini vardır: küçük ve Sass.  Bu önişlemcilerini değişkenleri ve büyük ve karmaşık stil sayfaları devamlılığını iyileştirmek iç içe geçmiş kuralları için destek gibi CSS için özellikler ekleyin. Değişkenleri olarak basit bir şey için bile desteği olmayan bir dil olarak CSS çok basit ve bu CSS dosyaları yinelenen bloated yapıp eğilimindedir. Önişlemcilerini aracılığıyla gerçek programlama dili özellik ekleme çoğaltma azaltmak ve Stil kurallarının daha iyi düzenleme sağlamaya yardımcı olabilir. Visual Studio hem daha az için yerleşik destek ve Sass yanı sıra, bu dillerde çalışırken geliştirme deneyimi daha fazla artırabilir uzantıları sağlar.

Bu CSS önişlemcilerini okunabilirlik ve stil bilgilerini Bakımı nasıl artırabilir hızlı örnek olarak göz önünde bulundurun:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

Daha az kullanarak, bu çoğaltma tümünün ortadan kaldırmak için yazılabilir kullanarak bir *mixin* ("karıştırmak" olanak tanıdığından ve bu nedenle adlı bir sınıf veya içine başka bir kural kümesi özelliklerinden):

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>daha az

Daha az CSS önişlemci Node.js kullanarak çalışır. Daha az yüklemek için bir komut isteminden düğüm paketi Yöneticisi (npm) kullanın (-g anlamına gelir "Genel"):

```console
npm install -g less
```

Visual Studio kullanıyorsanız, küçük projeniz için bir veya daha çok daha az dosyaları ekleyerek ve derleme zamanında işlemeye Gulp (veya Grunt) yapılandırma ile başlayabilirsiniz. Ekleme bir *stilleri* , projenizin klasörüne ve ardından yeni bir dosya adlı daha az ekleyin *main.less* bu klasöre.

![Daha az dosyası ekleme](less-sass-fa/_static/add-less-file.png)

Eklendikten sonra klasör yapısı aşağıdakine benzer görünmelidir:

![Klasör yapısı](less-sass-fa/_static/folder-structure.png)

Artık, CSS ile derlenen ve wwwroot klasörüne Gulp tarafından dağıtılan olan dosyasına bazı temel stil ekleyebilirsiniz.

Değiştirme *main.less* tek bir temel renkten bir basit renk paleti oluşturur aşağıdaki içerik dahil etmek.

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base`ve diğer @-prefixed değişkenleri öğeleridir. Bunların her biri, bir renk temsil eder. Dışında `@base`, renk işlevler kullanılarak hazırsınız: aydınlatmak, koyu ve döndür. Aydınlatmak ve koyu oldukça çok neler bekleyebileceğiniz; yapın döndürme bir renginin (etrafında renk Tekerlek) derece sayısına göre ayarlar. Daha az işlemci bu değişkenleri nasıl çalıştığını göstermek için herhangi bir yerde kullanmak üzere ihtiyacımız için kullanılmaz, değişkenleri yoksaymayı akıllıca olur. Sınıfları `.baseColor`, vb. her üretilen CSS dosyasında değişkenlerinin hesaplanan değerler göstermek.

### <a name="getting-started"></a>Başlarken

Oluşturma bir **npm yapılandırma dosyası** (*package.json*) proje klasörünüzdeki ve başvurmak için düzenlemek `gulp` ve `gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

Proje klasörünüzdeki ya da Visual Studio ya da bir komut isteminde bağımlılıkları yükler **Çözüm Gezgini** (**bağımlılıkları > npm > paketler geri**).

```console
npm install
```

![VS paketler geri yükleme](less-sass-fa/_static/restore-packages.png)

Proje klasöründe oluşturma bir **Gulp yapılandırma dosyası** (*gulpfile.js*) otomatik işlemi tanımlamak için.  Bir değişken daha az temsil etmek için dosya ve daha az çalışacak bir görev üstüne ekleyin:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Açık **görev Çalıştırıcı Gezgini** (**Görünüm > Diğer Pencereler > Görev Çalıştırıcı Gezgini**). Görevler arasında adlı yeni bir görev görmelisiniz `less`. Pencereyi yenilemek gerekebilir.

Çalıştırma `less` görev ve ne burada gösterilen için benzer bir çıktı bakın:

![daha az görev Çalıştırıcı](less-sass-fa/_static/less-task-runner.png)

*Wwwroot/css* klasörü artık yeni bir dosya içerir *main.css*:

![oluşturulan ana css](less-sass-fa/_static/main-css-created.png)

Açık *main.css* ve aşağıdaki gibi görebilirsiniz:

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

Basit bir HTML sayfasına ekleyin *wwwroot* klasörü ve başvuru *main.css* eylem renk paletindeki görmek için.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

Gördüğünüz üzerinde 180 derece döndür `@base` üretmek için kullanılan `@background` rengini karşıt renk tekerleği içinde sonuçlandı `@base`:

![daha az test örneği](less-sass-fa/_static/less-test-screenshot.png)

Daha az Ayrıca iç içe geçmiş kuralları, yanı sıra iç içe geçmiş medya sorguları destekler. Örneğin, menüler ayrıntılı CSS kuralları sonuçlanabilir gibi tanımlama iç içe geçmiş hiyerarşileri bu ister:

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

İdeal olarak tüm ilgili stil kurallarını birlikte CSS dosyası içinde yer alır, ancak uygulamada bir şey yok kuralı ve belki de bloğu açıklamaları dışında bu kural zorlama.

Daha az kullanarak kurallar tanımlama şöyle görünür:

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

Bu durumda, tüm alt öğelerinin unutmayın `nav` kapsamında yer alır. Artık tüm üst öğeler yinelenmesinin değil (`nav`, `li`, `a`), ve toplam satır sayısı da bıraktı (bazıları olsa olan ikinci örnekte aynı satırlarındaki değerleri koyma sonucunu). Bu kuruluş, tüm kurallar açıkça sınırlanmış bir kapsam içinde belirli bir kullanıcı Arabirimi öğesi için bu durumda görmek için dosyayı geri kalanından küme ayraçları ayarlayın çok yararlı olabilir.

`&` Sözdizimi daha az Seçicisi, bir özellik olan & Geçerli Seçici üst temsil eden. Bu nedenle, içinde {...} blok, `&` temsil eden bir `a` etiketi ve bu nedenle `&:link` eşdeğerdir `a:link`.

Medya sorguları, esnek tasarımlar oluşturmak için son derece yararlıdır da yoğun yineleme ve CSS karmaşıklığı katkıda bulunabilirsiniz. Böylece tüm sınıf tanımı içinde farklı yinelenmesi gerekmez daha az medya sorguları sınıfları içinde iç içe en üst düzey verir `@media` öğeleri. Örneğin, bir yanıt menüsü CSS şöyledir:

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

Bu daha iyi daha az tanımlanabilir:

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

Başka bir özellik zaten anlatıldığı daha az önceden tanımlanmış değişkenler arasından oluşturulması stil öznitelikleri izin vererek, matematiksel işlemler için desteğidir. Bu temel değişkeni değiştirilebilir ve tüm bağımlı değerleri otomatik olarak değiştir ilgili stilleri çok daha kolay güncelleştirme hale getirir.

Özellikle büyük siteler için CSS dosyaları (ve bir özellikle medya sorguları kullanılıyorsa), bunlarla çalışmaya yönetilmeleri yapmadan zamanla oldukça büyük alma eğilimindedir. Less dosyaları birlikte kullanarak çekilen ayrı olarak tanımlanabilir `@import` yönergeleri. Daha az da bireysel CSS dosyaları de içe aktarmak için isterseniz kullanılabilir.

*Mixins* parametreleri kabul edebilir ve ne zaman belirli mixins etkili tanımlamak için bildirim temelli bir yolunu sağlar mixin koruyucuları formunda daha az destekler koşullu mantık. Mixin koruyucuları için yaygın bir kullanımdır nasıl ışık göre renkleri ayarlamak için veya koyu kaynak rengi. Renk için bir parametre kabul eden bir mixin göz önüne alındığında, bir mixin koruma o rengi temel mixin değiştirmek için kullanılabilir:

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

Bizim geçerli verilen `@base` değerini `#663333`, bu daha az komut dosyasını aşağıdaki CSS üretir:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Daha az birkaç ek özellikler sağlar, ancak bu, bazı bu gücünü dil ön işleme fikir.

## <a name="sass"></a>Sass

Sass birçok aynı özellikleri, ancak biraz farklı bir sözdizimi için destek sağlayan küçük, benzer. JavaScript yerine Ruby kullanılarak oluşturulmuştur ve böylece farklı kurulum gereksinimleri vardır. Özgün Sass dil süslü ayraçlar veya noktalı virgül kullanmadı, ancak bunun yerine boşluk ve girinti kullanarak kapsamını tanımlı. Sürümünde Sass 3, yeni bir sözdizimi sunulmuştur, **SCSS** ("Sassy CSS"). Girinti düzeyleri ve boşluk yoksayar ve bunun yerine noktalı ve süslü ayraçlar kullanır SCSS, CSS'ye benzer.

Sass yüklemek için genellikle, (Mac üzerinde önceden yüklenmiş) Ruby yüklemeniz ve ardından çalıştırın:

```console
gem install sass
```

Visual Studio çalıştırıyorsanız, daha az olduğu gibi ancak, Sass ile çok aynı şekilde kullanmaya başlayabilir. Açık *package.json* ve "gulp-sass" paketi eklemek `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

Ardından, değişiklik *gulpfile.js* sass değişkeni ve Sass dosyalarınızı derlemek ve sonuçları wwwroot klasöre yerleştirmek için bir görev eklemek için:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Sass dosya ekleyebilirsiniz artık *main2.scss* için *stilleri* projenin kök klasöründe:

![scss dosyası ekleme](less-sass-fa/_static/add-scss-file.png)

Açık *main2.scss* ve aşağıdakileri ekleyin:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Tüm dosyalarını kaydedin. Yenilediğinizde şimdi **görev Çalıştırıcı Gezgini**, gördüğünüz bir `sass` görev. Çalıştırın ve konum */wwwroot/css* klasör. Var olan şimdi bir *main2.css* dosyasıyla bu içeriği:

```css
body {
    background-color: #CC0000;
}
```

Sass çok daha az, benzer yararlarını sunarak yaptığı aynı olan iç içe geçme destekler. Dosyaları işlevi tarafından bölünebilir ve kullanma dahil `@import` yönergesi:

```sass
@import 'anotherfile';
```

Sass kullanarak mixins de destekler `@mixin` bunları tanımlamak için anahtar sözcüğü ve `@include` Bu örnekte olduğu gibi dahil etmek için [sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Mixins yanı sıra Sass ayrıca devralma, kavramı başka genişletmek bir sınıf destekler. Kavramsal olarak bir mixin, ancak daha az CSS kod sonuçlarında benzer. Kullanılarak gerçekleştirilir `@extend` anahtar sözcüğü. Out mixins denemek için aşağıdakileri ekleyin, *main2.scss* dosyası:

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Çıktıyı inceleyin *main2.css* çalıştırdıktan sonra `sass` içinde görev **görev Çalıştırıcı Gezgini**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Tüm uyarı mixin ortak özelliklerinin her sınıfında yinelenip dikkat edin. Mixin çoğaltma geliştirme zaman ortadan kaldırmanıza yardımcı olmak için iyi bir iş vermedi, ancak bunu hala CSS gerekli CSS dosyaları - olası performans sorunu büyük sonuçta çoğaltma da, birçok oluşturuyor.

Uyarı mixin ile şimdi değiştirmek bir `.alert` sınıfı ve değiştirme `@include` için `@extend` (genişletmek hatırlamak `.alert`değil `alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Sass kez daha çalıştırın ve sonuçta elde edilen CSS inceleyin:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Yalnızca sayıda gerektiği gibi özellikleri artık tanımlanır ve daha iyi CSS oluşturulur.

Sass işlevleri ve koşullu mantık işlemleri, daha az benzer de içerir. Aslında, iki dil özellikleri çok benzer.

## <a name="less-or-sass"></a>Daha az veya Sass?

Hala daha az kullanın veya Sass genellikle daha iyi olup konusunda hiçbir anlaşma yok (veya hatta verilip özgün Sass ya da daha yeni SCSS söz dizimi Sass içinde tercih). Büyük olasılıkla en önemli için bir karardır **Bu araçlardan birini kullanın**, yalnızca elle CSS dosyalarınızı kodlama için aygıtlardır. Yaptıktan sonra karar, her iki Less ve Sass iyi seçenek olan.

## <a name="font-awesome"></a>Harika yazı tipi

CSS önişlemcilerini yanı sıra, başka bir harika stil modern web uygulamaları için yazı tipi harika bir kaynaktır. Yazı tipi Başar web uygulamalarınızda serbestçe kullanılabilir 500'den fazla ölçeklenebilir vektör simgeleri sağlayan bir araç takımıdır. İlk önyükleme ile çalışmak için tasarlanmıştır ancak bu framework veya herhangi bir JavaScript kitaplıklarını üzerinde hiçbir bağımlılık içeriyor.

Yazı tipi harika ile çalışmaya başlamak için en kolay yolu, ortak içerik teslim ağı (CDN) konumuna kullanarak başvuru eklemektir:

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

Ayrıca, Visual Studio projenizi "bağımlılıkları" ekleyerek içinde ekleyebileceğiniz *bower.json*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

Başvuru yazı tipi harika bir sayfada oluşturduktan sonra simgeler uygulamanıza genellikle önekine sahip "fa-" satır içi HTML öğeleriniz için yazı tipi harika sınıfları uygulayarak ekleyebilirsiniz (gibi `<span>` veya `<i>`).  Örneğin, basit listeleri ve menüleri aşağıdakine benzer bir kod kullanarak simgeler ekleyebilirsiniz:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

Bu aşağıdaki tarayıcıda oluşturur - her öğenin yanındaki simge dikkat edin:

![listesi simgeleri](less-sass-fa/_static/list-icons-screenshot.png)

Burada kullanılabilen simgeleri tam bir listesi görüntüleyebilirsiniz:

http://fontawesome.io/icons/

## <a name="summary"></a>Özet

Modern web uygulamaları, temiz, sezgisel ve kullanımı kolay aygıtları çeşitli esnek, akıcı tasarımları giderek talep. Bu hedeflere ulaşmak için gerekli CSS stil sayfaları karmaşıklığını yönetme önişlemci LIKE daha az kullanarak veya Sass en iyi şekilde yapılır. Ayrıca, araç takımları yazı tipi harika gibi hızlı bir şekilde metinsel Gezinti menüleri iyi bilinen simgeleri sağlayın ve uygulamanızın veya düğmeleri, genel kullanıcı geliştirme deneyimi.
