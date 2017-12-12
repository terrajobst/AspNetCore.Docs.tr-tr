---
title: "Daha az Sass ve ASP.NET Core harika yazı tipi"
author: ardalis
description: "Daha az Sass ve yazı tipi harika ASP.NET Core uygulamaları kullanmayı öğrenin."
keywords: "Daha az, Sass, yazı tipi harika, önişlemcilerini ASP.NET Çekirdeği"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 94c988f9-95fd-425d-b37e-7f846598c6d4
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/less-sass-fa
ms.openlocfilehash: 159377300d33e98393fd6705d0fec578f8f6b735
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="5be7a-104">Daha az stil uygulamalarla, Sass ve yazı tipi harika ASP.NET Core içinde giriş</span><span class="sxs-lookup"><span data-stu-id="5be7a-104">Introduction to styling applications with Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="5be7a-105">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5be7a-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5be7a-106">Stil ve genel denemek için kullanıcıların web uygulamalarının giderek yüksek beklentilerini vardır.</span><span class="sxs-lookup"><span data-stu-id="5be7a-106">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="5be7a-107">Modern web uygulamaları sık zengin araçları ve tanımlama ve kendi görünüm tutarlı bir şekilde yönetmek için çerçeveleri yararlanın.</span><span class="sxs-lookup"><span data-stu-id="5be7a-107">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="5be7a-108">Çerçeveler ister [önyükleme](http://getbootstrap.com/) stilleri ve düzen seçeneklerini web siteleri için ortak bir dizi tanımlama doğru uzun bir yol gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5be7a-108">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="5be7a-109">Bununla birlikte, çoğu Önemsiz olmayan siteler ayrıca sitenin arabirimi daha sezgisel hale gelmesine yardımcı görüntü olmayan simgeler kolay erişmesini yanı sıra etkili bir şekilde tanımlamak ve stilleri ve geçişli stil sayfası (CSS) dosyaları korumak için yararlı.</span><span class="sxs-lookup"><span data-stu-id="5be7a-109">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="5be7a-110">Where olan dilleri ve Destek Araçları [daha az](http://lesscss.org/) ve [Sass](http://sass-lang.com/), kitaplıklar gibi ve [yazı tipi harika](http://fontawesome.io/), geldikçe.</span><span class="sxs-lookup"><span data-stu-id="5be7a-110">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="5be7a-111">CSS önişlemci dilleri</span><span class="sxs-lookup"><span data-stu-id="5be7a-111">CSS preprocessor languages</span></span>

<span data-ttu-id="5be7a-112">Diğer dillere temel dili ile çalışmaya deneyimini geliştirmek için derlenmiş dillerde önişlemcilerini adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5be7a-112">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="5be7a-113">CSS için iki popüler önişlemcilerini vardır: küçük ve Sass.</span><span class="sxs-lookup"><span data-stu-id="5be7a-113">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="5be7a-114">Bu önişlemcilerini değişkenleri ve büyük ve karmaşık stil sayfaları devamlılığını iyileştirmek iç içe geçmiş kuralları için destek gibi CSS için özellikler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5be7a-114">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="5be7a-115">Değişkenleri olarak basit bir şey için bile desteği olmayan bir dil olarak CSS çok basit ve bu CSS dosyaları yinelenen bloated yapıp eğilimindedir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-115">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="5be7a-116">Önişlemcilerini aracılığıyla gerçek programlama dili özellik ekleme çoğaltma azaltmak ve Stil kurallarının daha iyi düzenleme sağlamaya yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-116">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="5be7a-117">Visual Studio hem daha az için yerleşik destek ve Sass yanı sıra, bu dillerde çalışırken geliştirme deneyimi daha fazla artırabilir uzantıları sağlar.</span><span class="sxs-lookup"><span data-stu-id="5be7a-117">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="5be7a-118">Bu CSS önişlemcilerini okunabilirlik ve stil bilgilerini Bakımı nasıl artırabilir hızlı örnek olarak göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5be7a-118">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

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

<span data-ttu-id="5be7a-119">Daha az kullanarak, bu çoğaltma tümünün ortadan kaldırmak için yazılabilir kullanarak bir *mixin* ("karıştırmak" olanak tanıdığından ve bu nedenle adlı bir sınıf veya içine başka bir kural kümesi özelliklerinden):</span><span class="sxs-lookup"><span data-stu-id="5be7a-119">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

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

## <a name="less"></a><span data-ttu-id="5be7a-120">daha az</span><span class="sxs-lookup"><span data-stu-id="5be7a-120">Less</span></span>

<span data-ttu-id="5be7a-121">Daha az CSS önişlemci Node.js kullanarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="5be7a-121">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="5be7a-122">Daha az yüklemek için bir komut isteminden düğüm paketi Yöneticisi (npm) kullanın (-g anlamına gelir "Genel"):</span><span class="sxs-lookup"><span data-stu-id="5be7a-122">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="5be7a-123">Visual Studio kullanıyorsanız, küçük projeniz için bir veya daha çok daha az dosyaları ekleyerek ve derleme zamanında işlemeye Gulp (veya Grunt) yapılandırma ile başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5be7a-123">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="5be7a-124">Ekleme bir *stilleri* , projenizin klasörüne ve ardından yeni bir dosya adlı daha az ekleyin *main.less* bu klasöre.</span><span class="sxs-lookup"><span data-stu-id="5be7a-124">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Daha az dosyası ekleme](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="5be7a-126">Eklendikten sonra klasör yapısı aşağıdakine benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="5be7a-126">Once added, your folder structure should look something like this:</span></span>

![Klasör yapısı](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="5be7a-128">Artık, CSS ile derlenen ve wwwroot klasörüne Gulp tarafından dağıtılan olan dosyasına bazı temel stil ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5be7a-128">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="5be7a-129">Değiştirme *main.less* tek bir temel renkten bir basit renk paleti oluşturur aşağıdaki içerik dahil etmek.</span><span class="sxs-lookup"><span data-stu-id="5be7a-129">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

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

<span data-ttu-id="5be7a-130">`@base`ve diğer @-prefixed değişkenleri öğeleridir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-130">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="5be7a-131">Bunların her biri, bir renk temsil eder.</span><span class="sxs-lookup"><span data-stu-id="5be7a-131">Each of them represents a color.</span></span> <span data-ttu-id="5be7a-132">Dışında `@base`, renk işlevler kullanılarak ayarlanır: aydınlatmak, koyu ve döndür.</span><span class="sxs-lookup"><span data-stu-id="5be7a-132">Except for `@base`, they are set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="5be7a-133">Aydınlatmak ve koyu oldukça çok neler bekleyebileceğiniz; yapın döndürme bir renginin (etrafında renk Tekerlek) derece sayısına göre ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5be7a-133">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="5be7a-134">Daha az işlemci bu değişkenleri nasıl çalıştığını göstermek için herhangi bir yerde kullanmak üzere ihtiyacımız için kullanılmaz, değişkenleri yoksaymayı akıllıca olur.</span><span class="sxs-lookup"><span data-stu-id="5be7a-134">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="5be7a-135">Sınıfları `.baseColor`, vb. her üretilen CSS dosyasında değişkenlerinin hesaplanan değerler göstermek.</span><span class="sxs-lookup"><span data-stu-id="5be7a-135">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that is produced.</span></span>

### <a name="getting-started"></a><span data-ttu-id="5be7a-136">Başlarken</span><span class="sxs-lookup"><span data-stu-id="5be7a-136">Getting started</span></span>

<span data-ttu-id="5be7a-137">Oluşturma bir **npm yapılandırma dosyası** (*package.json*) proje klasörünüzdeki ve başvurmak için düzenlemek `gulp` ve `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="5be7a-137">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

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

<span data-ttu-id="5be7a-138">Proje klasörünüzdeki ya da Visual Studio ya da bir komut isteminde bağımlılıkları yükler **Çözüm Gezgini** (**bağımlılıkları > npm > paketler geri**).</span><span class="sxs-lookup"><span data-stu-id="5be7a-138">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![VS paketler geri yükleme](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="5be7a-140">Proje klasöründe oluşturma bir **Gulp yapılandırma dosyası** (*gulpfile.js*) otomatik işlemi tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="5be7a-140">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="5be7a-141">Bir değişken daha az temsil etmek için dosya ve daha az çalışacak bir görev üstüne ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5be7a-141">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

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

<span data-ttu-id="5be7a-142">Açık **görev Çalıştırıcı Gezgini** (**Görünüm > Diğer Pencereler > Görev Çalıştırıcı Gezgini**).</span><span class="sxs-lookup"><span data-stu-id="5be7a-142">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="5be7a-143">Görevler arasında adlı yeni bir görev görmelisiniz `less`.</span><span class="sxs-lookup"><span data-stu-id="5be7a-143">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="5be7a-144">Pencereyi yenilemek gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-144">You might have to refresh the window.</span></span>

<span data-ttu-id="5be7a-145">Çalıştırma `less` görev ve ne burada gösterilen için benzer bir çıktı bakın:</span><span class="sxs-lookup"><span data-stu-id="5be7a-145">Run the `less` task, and you see output similar to what is shown here:</span></span>

![daha az görev Çalıştırıcı](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="5be7a-147">*Wwwroot/css* klasörü artık yeni bir dosya içerir *main.css*:</span><span class="sxs-lookup"><span data-stu-id="5be7a-147">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![oluşturulan ana css](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="5be7a-149">Açık *main.css* ve aşağıdaki gibi görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5be7a-149">Open *main.css* and you see something like the following:</span></span>

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

<span data-ttu-id="5be7a-150">Basit bir HTML sayfasına ekleyin *wwwroot* klasörü ve başvuru *main.css* eylem renk paletindeki görmek için.</span><span class="sxs-lookup"><span data-stu-id="5be7a-150">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

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

<span data-ttu-id="5be7a-151">Gördüğünüz üzerinde 180 derece döndür `@base` üretmek için kullanılan `@background` rengini karşıt renk tekerleği içinde sonuçlandı `@base`:</span><span class="sxs-lookup"><span data-stu-id="5be7a-151">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![daha az test örneği](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="5be7a-153">Daha az Ayrıca iç içe geçmiş kuralları, yanı sıra iç içe geçmiş medya sorguları destekler.</span><span class="sxs-lookup"><span data-stu-id="5be7a-153">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="5be7a-154">Örneğin, menüler ayrıntılı CSS kuralları sonuçlanabilir gibi tanımlama iç içe geçmiş hiyerarşileri bu ister:</span><span class="sxs-lookup"><span data-stu-id="5be7a-154">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

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

<span data-ttu-id="5be7a-155">İdeal olarak tüm ilgili stil kurallarını birlikte CSS dosyası içinde yer alır, ancak uygulamada bir şey yok kuralı ve belki de bloğu açıklamaları dışında bu kural zorlama.</span><span class="sxs-lookup"><span data-stu-id="5be7a-155">Ideally all of the related style rules will be placed together within the CSS file, but in practice there is nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="5be7a-156">Daha az kullanarak kurallar tanımlama şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="5be7a-156">Defining these same rules using Less looks like this:</span></span>

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

<span data-ttu-id="5be7a-157">Bu durumda, tüm alt öğelerinin unutmayın `nav` kapsamında yer alır.</span><span class="sxs-lookup"><span data-stu-id="5be7a-157">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="5be7a-158">Artık tüm üst öğeler yinelenmesinin değil (`nav`, `li`, `a`), ve (bazı, olmasına rağmen bir sonuç ikinci örnekte aynı satırlarındaki değerleri yerleştirme) toplam satır sayısı da bıraktı.</span><span class="sxs-lookup"><span data-stu-id="5be7a-158">There is no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that is a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="5be7a-159">Bu kuruluş, tüm kurallar açıkça sınırlanmış bir kapsam içinde belirli bir kullanıcı Arabirimi öğesi için bu durumda görmek için dosyayı geri kalanından küme ayraçları ayarlayın çok yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-159">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="5be7a-160">`&` Sözdizimi daha az Seçicisi, bir özellik olan & Geçerli Seçici üst temsil eden.</span><span class="sxs-lookup"><span data-stu-id="5be7a-160">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="5be7a-161">Bu nedenle, içinde {...}</span><span class="sxs-lookup"><span data-stu-id="5be7a-161">So, within the a {...}</span></span> <span data-ttu-id="5be7a-162">blok, `&` temsil eden bir `a` etiketi ve bu nedenle `&:link` eşdeğerdir `a:link`.</span><span class="sxs-lookup"><span data-stu-id="5be7a-162">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="5be7a-163">Medya sorguları, esnek tasarımlar oluşturmak için son derece yararlıdır da yoğun yineleme ve CSS karmaşıklığı katkıda bulunabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5be7a-163">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="5be7a-164">Böylece tüm sınıf tanımı içinde farklı yinelenmesi gerekmez daha az medya sorguları sınıfları içinde iç içe en üst düzey verir `@media` öğeleri.</span><span class="sxs-lookup"><span data-stu-id="5be7a-164">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="5be7a-165">Örneğin, bir yanıt menüsü CSS şöyledir:</span><span class="sxs-lookup"><span data-stu-id="5be7a-165">For example, here is CSS for a responsive menu:</span></span>

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

<span data-ttu-id="5be7a-166">Bu daha iyi daha az tanımlanabilir:</span><span class="sxs-lookup"><span data-stu-id="5be7a-166">This can be better defined in Less as:</span></span>

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

<span data-ttu-id="5be7a-167">Başka bir özellik zaten anlatıldığı daha az önceden tanımlanmış değişkenler arasından oluşturulması stil öznitelikleri izin vererek, matematiksel işlemler için desteğidir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-167">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="5be7a-168">Bu temel değişkeni değiştirilebilir ve tüm bağımlı değerleri otomatik olarak değiştir ilgili stilleri çok daha kolay güncelleştirme hale getirir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-168">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="5be7a-169">Özellikle büyük siteler için CSS dosyaları (ve bir özellikle medya sorguları kullanılıyorsa), bunlarla çalışmaya yönetilmeleri yapmadan zamanla oldukça büyük alma eğilimindedir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-169">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="5be7a-170">Less dosyaları birlikte kullanarak çekilen ayrı olarak tanımlanabilir `@import` yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="5be7a-170">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="5be7a-171">Daha az da bireysel CSS dosyaları de içe aktarmak için isterseniz kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-171">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="5be7a-172">*Mixins* parametreleri kabul edebilir ve ne zaman belirli mixins etkili tanımlamak için bildirim temelli bir yolunu sağlar mixin koruyucuları formunda daha az destekler koşullu mantık.</span><span class="sxs-lookup"><span data-stu-id="5be7a-172">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="5be7a-173">Mixin koruyucuları için yaygın bir kullanımdır nasıl ışık göre renkleri ayarlamak için veya koyu kaynak rengi.</span><span class="sxs-lookup"><span data-stu-id="5be7a-173">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="5be7a-174">Renk için bir parametre kabul eden bir mixin göz önüne alındığında, bir mixin koruma o rengi temel mixin değiştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5be7a-174">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

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

<span data-ttu-id="5be7a-175">Bizim geçerli verilen `@base` değerini `#663333`, bu daha az komut dosyasını aşağıdaki CSS üretir:</span><span class="sxs-lookup"><span data-stu-id="5be7a-175">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="5be7a-176">Daha az birkaç ek özellikler sağlar, ancak bu, bazı bu gücünü dil ön işleme fikir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-176">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="5be7a-177">Sass</span><span class="sxs-lookup"><span data-stu-id="5be7a-177">Sass</span></span>

<span data-ttu-id="5be7a-178">Sass birçok aynı özellikleri, ancak biraz farklı bir sözdizimi için destek sağlayan küçük, benzer.</span><span class="sxs-lookup"><span data-stu-id="5be7a-178">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="5be7a-179">JavaScript yerine Ruby kullanılarak oluşturulmuştur ve böylece farklı kurulum gereksinimleri vardır.</span><span class="sxs-lookup"><span data-stu-id="5be7a-179">It is built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="5be7a-180">Özgün Sass dil süslü ayraçlar veya noktalı virgül kullanmayan ancak bunun yerine boşluk ve girinti kullanarak kapsamı tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="5be7a-180">The original Sass language did not use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="5be7a-181">Sürümünde Sass 3, yeni bir sözdizimi sunulmuştur, **SCSS** ("Sassy CSS").</span><span class="sxs-lookup"><span data-stu-id="5be7a-181">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="5be7a-182">Girinti düzeyleri ve boşluk yoksayar ve bunun yerine noktalı ve süslü ayraçlar kullanır SCSS, CSS'ye benzer.</span><span class="sxs-lookup"><span data-stu-id="5be7a-182">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="5be7a-183">Sass yüklemek için genellikle, (Mac üzerinde önceden yüklenmiş) Ruby yüklemeniz ve ardından çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="5be7a-183">To install Sass, typically you would first install Ruby (pre-installed on Mac), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="5be7a-184">Visual Studio çalıştırıyorsanız, daha az olduğu gibi ancak, Sass ile çok aynı şekilde kullanmaya başlayabilir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-184">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="5be7a-185">Açık *package.json* ve "gulp-sass" paketi eklemek `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="5be7a-185">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="5be7a-186">Ardından, değişiklik *gulpfile.js* sass değişkeni ve Sass dosyalarınızı derlemek ve sonuçları wwwroot klasöre yerleştirmek için bir görev eklemek için:</span><span class="sxs-lookup"><span data-stu-id="5be7a-186">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

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

<span data-ttu-id="5be7a-187">Sass dosya ekleyebilirsiniz artık *main2.scss* için *stilleri* projenin kök klasöründe:</span><span class="sxs-lookup"><span data-stu-id="5be7a-187">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![scss dosyası ekleme](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="5be7a-189">Açık *main2.scss* ve aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5be7a-189">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="5be7a-190">Tüm dosyalarını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="5be7a-190">Save all of your files.</span></span> <span data-ttu-id="5be7a-191">Yenilediğinizde şimdi **görev Çalıştırıcı Gezgini**, gördüğünüz bir `sass` görev.</span><span class="sxs-lookup"><span data-stu-id="5be7a-191">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="5be7a-192">Çalıştırın ve konum */wwwroot/css* klasör.</span><span class="sxs-lookup"><span data-stu-id="5be7a-192">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="5be7a-193">Var olan şimdi bir *main2.css* dosyasıyla bu içeriği:</span><span class="sxs-lookup"><span data-stu-id="5be7a-193">There is now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="5be7a-194">Sass çok daha az, benzer yararlarını sunarak yaptığı aynı olan iç içe geçme destekler.</span><span class="sxs-lookup"><span data-stu-id="5be7a-194">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="5be7a-195">Dosyaları işlevi tarafından bölünebilir ve kullanma dahil `@import` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="5be7a-195">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="5be7a-196">Sass kullanarak mixins de destekler `@mixin` bunları tanımlamak için anahtar sözcüğü ve `@include` Bu örnekte olduğu gibi dahil etmek için [sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="5be7a-196">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="5be7a-197">Mixins yanı sıra Sass ayrıca devralma, kavramı başka genişletmek bir sınıf destekler.</span><span class="sxs-lookup"><span data-stu-id="5be7a-197">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="5be7a-198">Kavramsal olarak bir mixin, ancak daha az CSS kod sonuçlarında benzer.</span><span class="sxs-lookup"><span data-stu-id="5be7a-198">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="5be7a-199">Kullanılarak gerçekleştirilir `@extend` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="5be7a-199">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="5be7a-200">Out mixins denemek için aşağıdakileri ekleyin, *main2.scss* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5be7a-200">To try out mixins, add the following to your *main2.scss* file:</span></span>

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

<span data-ttu-id="5be7a-201">Çıktıyı inceleyin *main2.css* çalıştırdıktan sonra `sass` içinde görev **görev Çalıştırıcı Gezgini**:</span><span class="sxs-lookup"><span data-stu-id="5be7a-201">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

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

<span data-ttu-id="5be7a-202">Tüm uyarı mixin ortak özelliklerinin her sınıfında yinelenip dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="5be7a-202">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="5be7a-203">Mixin çoğaltma geliştirme zaman ortadan kaldırmanıza yardımcı olmak için iyi bir iş vermedi, ancak bunu hala CSS gerekli CSS dosyaları - olası performans sorunu büyük sonuçta çoğaltma da, birçok oluşturuyor.</span><span class="sxs-lookup"><span data-stu-id="5be7a-203">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="5be7a-204">Uyarı mixin ile şimdi değiştirmek bir `.alert` sınıfı ve değiştirme `@include` için `@extend` (genişletmek hatırlamak `.alert`değil `alert`):</span><span class="sxs-lookup"><span data-stu-id="5be7a-204">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

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

<span data-ttu-id="5be7a-205">Sass kez daha çalıştırın ve sonuçta elde edilen CSS inceleyin:</span><span class="sxs-lookup"><span data-stu-id="5be7a-205">Run Sass once more, and examine the resulting CSS:</span></span>

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

<span data-ttu-id="5be7a-206">Yalnızca sayıda gerektiği gibi özellikleri artık tanımlanır ve daha iyi CSS oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5be7a-206">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="5be7a-207">Sass işlevleri ve koşullu mantık işlemleri, daha az benzer de içerir.</span><span class="sxs-lookup"><span data-stu-id="5be7a-207">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="5be7a-208">Aslında, iki dil özellikleri çok benzer.</span><span class="sxs-lookup"><span data-stu-id="5be7a-208">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="5be7a-209">Daha az veya Sass?</span><span class="sxs-lookup"><span data-stu-id="5be7a-209">Less or Sass?</span></span>

<span data-ttu-id="5be7a-210">Hala daha az kullanın veya Sass genellikle daha iyi olup konusunda hiçbir anlaşma yok (veya hatta verilip özgün Sass ya da daha yeni SCSS söz dizimi Sass içinde tercih).</span><span class="sxs-lookup"><span data-stu-id="5be7a-210">There is still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="5be7a-211">Büyük olasılıkla en önemli için bir karardır **Bu araçlardan birini kullanın**, yalnızca elle CSS dosyalarınızı kodlama için aygıtlardır.</span><span class="sxs-lookup"><span data-stu-id="5be7a-211">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="5be7a-212">Yaptıktan sonra karar, her iki Less ve Sass iyi seçenek olan.</span><span class="sxs-lookup"><span data-stu-id="5be7a-212">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="5be7a-213">Harika yazı tipi</span><span class="sxs-lookup"><span data-stu-id="5be7a-213">Font Awesome</span></span>

<span data-ttu-id="5be7a-214">CSS önişlemcilerini yanı sıra, başka bir harika stil modern web uygulamaları için yazı tipi harika bir kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="5be7a-214">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="5be7a-215">Yazı tipi Başar web uygulamalarınızda serbestçe kullanılabilir 500'den fazla ölçeklenebilir vektör simgeleri sağlayan bir araç takımıdır.</span><span class="sxs-lookup"><span data-stu-id="5be7a-215">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="5be7a-216">İlk önyükleme ile çalışmak için tasarlanmıştır ancak bu framework veya herhangi bir JavaScript kitaplıklarını üzerinde hiçbir bağımlılık içeriyor.</span><span class="sxs-lookup"><span data-stu-id="5be7a-216">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="5be7a-217">Yazı tipi harika ile çalışmaya başlamak için en kolay yolu, ortak içerik teslim ağı (CDN) konumuna kullanarak başvuru eklemektir:</span><span class="sxs-lookup"><span data-stu-id="5be7a-217">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="5be7a-218">Ayrıca, Visual Studio projenizi "bağımlılıkları" ekleyerek içinde ekleyebileceğiniz *bower.json*:</span><span class="sxs-lookup"><span data-stu-id="5be7a-218">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

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

<span data-ttu-id="5be7a-219">Başvuru yazı tipi harika bir sayfada oluşturduktan sonra simgeler uygulamanıza genellikle önekine sahip "fa-" satır içi HTML öğeleriniz için yazı tipi harika sınıfları uygulayarak ekleyebilirsiniz (gibi `<span>` veya `<i>`).</span><span class="sxs-lookup"><span data-stu-id="5be7a-219">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="5be7a-220">Örneğin, basit listeleri ve menüleri aşağıdakine benzer bir kod kullanarak simgeler ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5be7a-220">For example, you can add icons to simple lists and menus using code like this:</span></span>

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

<span data-ttu-id="5be7a-221">Bu aşağıdaki tarayıcıda oluşturur - her öğenin yanındaki simge dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="5be7a-221">This produces the following in the browser - note the icon beside each item:</span></span>

![listesi simgeleri](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="5be7a-223">Burada kullanılabilen simgeleri tam bir listesi görüntüleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5be7a-223">You can view a complete list of the available icons here:</span></span>

<span data-ttu-id="5be7a-224">http://fontawesome.io/icons/</span><span class="sxs-lookup"><span data-stu-id="5be7a-224">http://fontawesome.io/icons/</span></span>

## <a name="summary"></a><span data-ttu-id="5be7a-225">Özet</span><span class="sxs-lookup"><span data-stu-id="5be7a-225">Summary</span></span>

<span data-ttu-id="5be7a-226">Modern web uygulamaları, temiz, sezgisel ve kullanımı kolay aygıtları çeşitli esnek, akıcı tasarımları giderek talep.</span><span class="sxs-lookup"><span data-stu-id="5be7a-226">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="5be7a-227">Bu hedeflere ulaşmak için gerekli CSS stil sayfaları karmaşıklığını yönetme önişlemci LIKE daha az kullanarak veya Sass en iyi şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="5be7a-227">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="5be7a-228">Ayrıca, araç takımları yazı tipi harika gibi hızlı bir şekilde metinsel Gezinti menüleri iyi bilinen simgeleri sağlayın ve uygulamanızın veya düğmeleri, genel kullanıcı geliştirme deneyimi.</span><span class="sxs-lookup"><span data-stu-id="5be7a-228">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
