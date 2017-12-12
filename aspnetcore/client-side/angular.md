---
title: "AngularJS tek sayfa uygulamaları (SPAs) kullanma"
author: rick-anderson
description: "AngularJS kullanarak bir SPA stili ASP.NET uygulaması oluşturmayı öğrenin"
keywords: ASP.NET Core, AngularJS, SPA
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ccdf1625cdaf2400780500ac5ab86f41537964a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a>AngularJS ASP.NET Core ile tek sayfa uygulamaları (SPAs) için kullanma


Tarafından [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) ve [Scott Addie](https://scottaddie.com)

Bu makalede, AngularJS kullanarak SPA stili ASP.NET uygulamasının nasıl oluşturulacağını öğreneceksiniz.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-angularjs"></a>AngularJS nedir?

[AngularJS](https://angularjs.org/) tek sayfa uygulamaları (SPAs) ile çalışmak için yaygın olarak kullanılan Google gelen modern bir JavaScript çerçevedir. AngularJS açık MIT lisansı altında kaynaklıdır ve AngularJS geliştirme sürecini izlenebilir [GitHub deposunu](https://github.com/angular/angular.js). HTML şeklinde Açısal köşeli kullandığından kitaplığı Angular adı verilir.

AngularJS bir DOM düzenleme kitaplığı jQuery gibi değil, ancak bir alt kümesini jQLite adlı jQuery kullanır. AngularJS öncelikle, HTML etiket ekleyebilirsiniz bildirim temelli HTML özniteliklerini temel alır. AngularJS kullanarak tarayıcı deneyebilirsiniz [kod Okul Web sitesi](https://www.codeschool.com/courses/shaping-up-with-angularjs) veya [W3Schools Web sitesi](https://www.w3schools.com/angular/).

Bu makalede, burada Angular başlık çubuğunda bazı notlar ile AngularJS üzerinde odaklanır.

## <a name="getting-started"></a>Başlarken

ASP.NET uygulamanızı AngularJS kullanmaya başlamak için projenizin bir parçası olarak yüklemek, veya bir içerik teslim ağı (CDN) başvuru.

### <a name="installation"></a>Yükleme

AngularJS uygulamanıza eklemek için çeşitli yollar vardır. Visual Studio'da yeni bir ASP.NET Core web uygulaması başlatıyorsanız, yerleşik kullanarak AngularJS ekleyebilirsiniz [Bower](bower.md) destekler. Açık *bower.json*ve bir girdiyi `dependencies` özelliği:

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

Kaydetme sırasında *bower.json* dosyası, Angular projenizin içinde yüklenecek *wwwroot/lib* klasör. Ayrıca, içinde listelenecektir `Dependencies/Bower` klasör. Aşağıdaki ekran görüntüsüne bakın.

![AngularJS proje ile Çözüm Gezgini](angular/_static/angular-solution-explorer.png)

Ardından, eklemek bir `<script>` altına başvuru `<body>` , HTML sayfasının bölümünde veya *_Layout.cshtml* aşağıda gösterildiği gibi dosya:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

Üretim uygulamaları AngularJS gibi ortak kitaplıkları CDN'ler kullanan önerilir. Bunun gibi birkaç CDN'ler birinden AngularJS başvurabilir:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

Bir başvuru olduktan sonra *angular.js* komut dosyası, web sayfalarınıza AngularJS kullanmaya başlamak hazır.

## <a name="key-components"></a>Anahtar bileşenleri

AngularJS içeren bir dizi ana bileşen gibi *yönergeleri*, *şablonları*, *yineleyiciler*, *modülleri*,  *denetleyicileri*, *bileşenleri*, *bileşen yönlendirici* ve daha fazlası. Bu bileşenlerin birlikte nasıl davranış web sayfalarınıza eklemek için çalışır inceleyelim.

### <a name="directives"></a>Yönergeler

AngularJS kullanan [yönergeleri](https://docs.angularjs.org/guide/directive) HTML özel öznitelikler ve öğeler ile genişletmek için. AngularJS yönergesi aracılığıyla tanımlanır `data-ng-*` veya `ng-*` önekleri (`ng` kısaltması olan Açısal). AngularJS yönergesi iki tür vardır:

   1. **İlkel yönergeleri**: Bu Açısal ekibi tarafından önceden tanımlanmış ve AngularJS framework'ün parçasıdır.

   2. **Özel yönergeleri**: tanımlayabileceğiniz özel yönergeleri bunlar.

Tüm AngularJS uygulamalarında kullanılan ilkel yönergeleri biri `ng-app` AngularJS uygulama bootstraps yönergesi. Bu yönerge uygulanabilir `<body>` etiketi veya bir alt öğesi gövde. Uygulamada bir örnek görelim. ASP.NET projesinde olduğunuz varsayılarak, bir HTML dosyasına ya da ekleyebilirsiniz `wwwroot` klasörü, yeni bir denetleyici eylemi ve ilişkili bir görünüm ekleyin. Bu durumda, yeni bir ekledim `Directives` eylem yöntemine `HomeController.cs`. İlişkili görünüm burada gösterilir:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

Bu örnekler birbirlerinden bağımsız tutmak için paylaşılan düzeni dosyasını kullanıyorum değil. Biz gövde etiketi ile donatılmış görebilirsiniz `ng-app` bu sayfayı göstermek için yönergesi olup bir AngularJS uygulama. `{{2+2}}` Daha birazdan öğreneceksiniz bir Açısal veri bağlama ifadesidir. Bu uygulama çalıştırırsanız, sonuç şöyledir:

![Basit Açısal yönergesi](angular/_static/simple-directive.png)

AngularJS başka ilkel yönergeleri içerir:

`ng-controller`Hangi JavaScript denetleyicisi hangi görünüme bağlı belirler.

`ng-model`Bir HTML öğenin özelliklerinin değerlerini bağlı olan model belirler.

`ng-init`Geçerli kapsam için bir ifade biçiminde uygulama verilerini başlatmak için kullanılır.

`ng-if`Kaldırır veya sağlanan ifade truthiness üzerinde temel DOM verilen HTML öğesini yeniden oluşturur.

`ng-repeat`Belirtilen bir HTML bloğunu bir veri kümesi üzerinde tekrarlar.

`ng-show`Gösterir veya gizler sağlanan ifadesi temelinde verilen HTML öğesi.

AngularJS desteklenen tüm ilkel yönergeleri tam bir listesi için lütfen bakın [AngularJS belgeleri Web sitesinde yönerge bir belge bölümü](https://docs.angularjs.org/api/ng/directive).

### <a name="data-binding"></a>Veri bağlama

AngularJS sağlar [veri bağlama](https://docs.angularjs.org/guide/databinding) out-of--kullanarak box Destek `ng-bind` yönergesi veya bir veri ifade sözdizimini gibi bağlama `{{expression}}`. AngularJS bir görünüm şablonu ile eşitleme sırasında her zaman bir modelden veri nerede tutulur iki yönlü veri bağlamayı destekler. Görünüm değişiklikleri otomatik olarak modeldeki yansıtılır. Benzer şekilde, modeldeki değişiklikleri görünümünde yansıtılır.

Bir HTML dosyası veya bir denetleyici eylemi adlı bir eşlik eden görünümüyle oluşturma `Databinding`. Görünümde şunlardır:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

Bildirim yönergeleri veya veri bağlamayı kullanarak modeli değerlerini görüntüleyebilirsiniz (`ng-bind`). Sonuçta elde edilen sayfa, aşağıdaki gibi görünmelidir:

![Basit veri bağlama](angular/_static/simple-databinding.png)

### <a name="templates"></a>Şablonlar

[Şablonları](https://docs.angularjs.org/guide/templates) AngularJS içinde yalnızca düz HTML sayfaları AngularJS yönergeleri ve yapıları ile donatılmış şunlardır. AngularJS şablonunda yönergeleri, ifadeler, filtreleri ve form görünümü HTML ile birleştirmenin denetimleri bileşimi ' dir.

Şablonları göstermek için başka bir görünüm ekleyin ve aşağıdaki şekilde ekleyin:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

AngularJS yönergesi gibi şablonu sahip `ng-app`, `ng-init`, `ng-model` ve bağlamak için veri bağlama ifadesi sözdizimi `personName` özelliği. Tarayıcıda çalışan, görünüm aşağıdaki ekran görüntüsüne görünür:

![Basit şablonları örnek 1](angular/_static/simple-templates-1.png)

Giriş alanında yazarak adını değiştirirseniz, metnin yanında giriş alanını dinamik olarak görürsünüz eylemde Açısal iki yönlü veri bağlamayı gösteren güncelleştirme.

![Basit şablonları örnek 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a>İfadeler

[İfadeleri](https://docs.angularjs.org/guide/expression) içinde yazılmış JavaScript benzeri kod parçacıkları AngularJS içinde olan `{{ expression }}` sözdizimi. Bu deyimler verilerden HTML aynı şekilde bağlanır `ng-bind` yönergeleri. AngularJS ifadeleri ve normal JavaScript ifadeler arasındaki temel fark ifadeleri karşı değerlendirilir bu AngularJS olan `$scope` AngularJS nesne.

Bağ Aşağıda örnek AngularJS ifadelerinde `personName` ve basit bir JavaScript ifadesi hesaplanır:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

Tarayıcı görüntüler çalışan örnek `personName` veri ve hesaplama sonuçları:

![Basit ifadeler](angular/_static/simple-expressions.png)

### <a name="repeaters"></a>Yineleyiciler

AngularJS içinde yinelenen adlı bir ilkel yönergesi yapılır `ng-repeat`. `ng-repeat` Yönergesi üzerinde yinelenen bir veri dizisi uzunluğu belirtilen bir HTML öğesi bir görünümde tekrarlar. AngularJS yineleyiciler dizeleri veya nesneler dizisi yineleyebilirsiniz. Dize dizisi üzerinde yinelenen bir örnek kullanımı şöyledir:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

[Repeat yönergesi](https://docs.angularjs.org/api/ng/directive/ngRepeat) bu ekran görüntüsünde gösterilen Geliştirici Araçları'nda gördüğünüz gibi bir dizi sırasız bir listesini, liste öğelerini çıkarır:

![Yineleyici örneği](angular/_static/repeater.png)

Burada, nesnelerinin bir dizisi üzerinde yinelenen bir örnek verilmiştir. `ng-init` Yönergesi kurar bir `names` burada her öğe, ilk içeren bir nesne ve son adları dizisi. `ng-repeat` Atama, `name in names`, her dizi öğesi için bir liste öğesi çıkarır.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

Çıktı, bu durumda önceki örnektekiyle aynı şeklindedir.

Açısal döngü, yürütmeyi olduğu dayalı davranışı sağlamak bazı ek yönergeleri sağlar.

`$index`

Kullanım `$index` içinde `ng-repeat` hangi dizin döngü şu anda konumunu belirlemek için döngü açıktır.

`$even`ve`$odd`

Kullanım `$even` içinde `ng-repeat` döngü geçerli dizinde bile dizinlenmiş bir satır olup olmadığını belirlemek için döngü. Benzer şekilde, kullanın `$odd` geçerli dizini tek bir dizini oluşturulmuş satır olup olmadığını belirlemek için.

`$first`ve`$last`

Kullanım `$first` içinde `ng-repeat` geçerli dizin, Döngüdeki ilk satırın olup olmadığını belirlemek için döngü. Benzer şekilde, kullanın `$last` geçerli dizini son satırını olup olmadığını belirlemek için.

Aşağıda gösteren bir örnektir `$index`, `$even`, `$odd`, `$first`, ve `$last` eylem:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

Sonuçta çıktı şöyledir:

![Yineleyici örnek 2](angular/_static/repeaters2.png)

### <a name="scope"></a>$scope

`$scope`Görünüm (şablonu) ve (aşağıda açıklanmıştır) denetleyicisi arasındaki Birleştirici görevi gören bir JavaScript nesnesidir. AngularJS görünüm şablonunda bağlı değerleri yalnızca bildiği `$scope` denetleyicisi nesnesinde.

> [!NOTE]
> MVVM dünyada `$scope` AngularJS nesnesinde genellikle ViewModel tanımlanır. AngularJS takım başvurduğu `$scope` nesnesi olarak veri modeli. [AngularJS kapsamlarda hakkında daha fazla bilgi](https://docs.angularjs.org/guide/scope).

Özelliklerini ayarlamak nasıl gösteren basit bir örnek aşağıdadır `$scope` ayrı bir JavaScript dosyası içinde *scope.js*:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

Gözlemlemek `$scope` parametresi satır 2 denetleyicisine geçirildi. Bu nesne, görünüm bildiği ' dir. 3. satırda biz "Mary Jane" için "name" adlı bir özellik ayarlıyorsunuz.

Görünüm tarafından belirli bir özellik bulunamadı ne olur? Aşağıda tanımlanan görünüm "name" ve "Yaş" özellikleri için geçerlidir:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

Biz ifade sözdizimini kullanarak "name" özelliği görüntülemek için Angular soran 9 satırda dikkat edin. Satır 10 sonra "geçerlilik süresi", var olmayan bir özelliğe başvuruyor. Çalışan bir örnek "Mary Jane" ve hiçbir şey yaşı için ayarlanan adı gösterir. Eksik özelliklerini göz ardı edilir.

![Kapsam örneği](angular/_static/scope.png)

### <a name="modules"></a>Modüller

A [Modülü](https://docs.angularjs.org/guide/module) içinde AngularJS denetleyicisi, hizmetler, yönergeleri, vb. bir koleksiyonudur. `angular.module()` İşlev çağrısı oluşturmak, kaydetme ve AngularJS modüllerde almak için kullanılır. AngularJS takım ve üçüncü taraf kitaplıkların tarafından sevk dahil olmak üzere tüm modülleri kullanarak kaydedilmelidir `angular.module()` işlevi.

Bir AngularJS içinde yeni bir modül oluşturmayı gösteren kod parçacığını aşağıdadır. İlk parametre modülü adıdır. İkinci parametre bağımlılıkları diğer modülleri tanımlar. Bu makalenin sonraki bölümlerinde biz Bu bağımlılıklar geçirmek nasıl gösterilmesi bir `angular.module()` yöntem çağrısı.

```javascript
var personApp = angular.module('personApp', []);
```

Kullanım `ng-app` yönergesi sayfasında AngularJS modülü temsil eder. Bir modülü kullanmak, modül adı atamak için `personApp` Bu örnekte, için `ng-app` bizim şablonundaki yönerge.

```html
<body ng-app="personApp">
```

### <a name="controllers"></a>Denetleyiciler

[Denetleyicileri](https://docs.angularjs.org/guide/controller) içinde AngularJS kodunuz için giriş ilk noktasıdır. `<module name>.controller()` İşlev çağrısı oluşturmak ve denetleyicileri içinde AngularJS kaydetmek için kullanılır. `ng-controller` Yönergesi HTML sayfasında bir AngularJS denetleyicisi göstermek için kullanılır. Angular denetleyicisi rolünü durumunu ve veri modelinin davranışını ayarlamaktır (`$scope`). Denetleyicileri DOM doğrudan işlemek için kullanılmamalıdır.

Yeni bir denetleyicisi kaydeder kod parçacığını aşağıdadır. `personApp` Parçacığında bulunan değişkenine başvuruyor 2 satırında tanımlanan bir Açısal modülü.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

Görünümü kullanarak `ng-controller` yönergesi Denetleyici adı atar:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

"Mary" ve "karşılık Jane" sayfası gösterir `firstName` ve `lastName` ekli özellikler `$scope` nesnesi:

![Denetleyici örneği](angular/_static/controllers.png)

### <a name="components"></a>Bileşenler

[Bileşenleri](https://docs.angularjs.org/guide/component) içinde Angular 1.5.x izin kapsülleme ve tek tek HTML öğeleri oluşturma yeteneği. İçinde Açısal 1.4.x .directive() yöntemini kullanarak aynı özellik elde.

.Component() yöntemi kullanarak, geliştirme yönergesi ve denetleyici işlevselliğini sağlamasını basitleştirilmiştir. Diğer avantajlar şunlardır; Kapsam yalıtımı, en iyi yöntemler devralınmış ve geçiş Açısal 2 daha kolay bir görev haline gelir. `<module name>.component()` İşlev çağrısı oluşturmak ve bileşenleri AngularJS kaydetmek için kullanılır.

Yeni bir bileşen kaydeder kod parçacığını aşağıdadır. `personApp` Parçacığında bulunan değişkenine başvuruyor 2 satırında tanımlanan bir Açısal modülü.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

Burada size özel HTML öğesi görüntülemekte olduğunuz görüntüleyin.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

Bileşeni tarafından kullanılan ilişkili şablonu:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

"Aftab" ve "karşılık Ansari" sayfası gösterir `firstName` ve `lastName` ekli özellikler `vm` nesnesi:

![Bileşenleri örnek](angular/_static/components.png)

### <a name="services"></a>Hizmetler

[Hizmetleri](https://docs.angularjs.org/guide/services) içinde AngularJS Açısal uygulama ömrü kullanılabilecek bir dosyaya koyma soyutlanır paylaşılan kod için yaygın olarak kullanılır. Hizmetleri gevşek örneği olmayacaktır, bir hizmet örneği hizmete bağlı bir bileşen kullanılmadıkça anlamına gelir. Oluşturucuları AngularJS uygulamalarda kullanılan bir hizmet bir örnektir. Oluşturucuları kullanarak oluşturulur `myApp.factory()` işlev çağrısı, burada `myApp` modülüdür.

AngularJS factory'leri kullanmayı gösteren bir örnek aşağıdadır:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

Bu fabrikada denetleyicisinden çağırmak için geçirmek `personFactory` bir parametre olarak `controller` işlevi:

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a>Bir REST uç noktası için iletişim kurabilecek şekilde Hizmetleri kullanma

Aşağıda bir ASP.NET çekirdek Web API uç noktası ile etkileşim kurmak için uçtan uca örnek Hizmetleri içinde AngularJS kullanıyor. Örnek Web API öğesinden verileri alır ve bir görünüm şablonunda verileri görüntüler. İlk görünümüyle başlayalım:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

Bu görünümde sahibiz adlı bir Açısal Modülü `PersonsApp` bir denetleyici adı verilen ve `personController`. Kullanıyoruz `ng-repeat` kişilerin listesi yineleme. Özel JavaScript dosyaları üç 17-19 satırlarındaki başvuruyor.

*PersonApp.js* dosyasını kaydetmek için kullanılan `PersonsApp` modülü; ve sözdizimi önceki örneklere benzer. Kullanıyoruz `angular.module` biz ile çalışacaksınız modülü yeni bir örneğini oluşturmak için işlevi.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

Bir göz atalım *personFactory.js*, aşağıdaki. Modülün aramakta olduğunuz `factory` yöntemi bir fabrikası oluşturun. Satır 12 gösterir yerleşik Angular `$http` hizmeti bir web hizmetinden kişi bilgileri alınıyor.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

İçinde *personController.js*, modülün aramakta olduğunuz `controller` denetleyicisi oluşturmak için yöntem. `$scope` Nesnenin `people` özelliği personFactory (satır 13) döndürülen verileri atanır.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

Web API ve bunun arkasındaki modeli hızlı bir göz atalım. `Person` Modeldir POCO (düz eski CLR nesnesi) ile `Id`, `FirstName`, ve `LastName` özellikleri:

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

`Person` Denetleyicisi JSON biçimli bir listesini döndürür `Person` nesneler:

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

Uygulamayı eylem görelim:

![Denetleyici görüntüleme REST sonuçları](angular/_static/rest-bound.png)

Yapabilecekleriniz [Github'da uygulamanın yapısını görünümünde](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

> [!NOTE]
> AngularJS uygulamaları yapılandırılması hakkında daha fazla bilgi için bkz: [John Papa'nın Açısal Stil Kılavuzu](https://github.com/johnpapa/angular-styleguide)

&nbsp;

> [!NOTE]
> AngularJS modülü, denetleyici, Fabrika oluşturmak için yönergesi ve görünüm dosyaları kolayca Sayed Hashimi 's denetlediğinizden emin [SideWaffle şablon paketi Visual Studio için](http://sidewaffle.com/). Microsoft Visual Studio Web ekibi üzerinde üst düzey Program Yöneticisi sayed Hashimi olduğu ve SideWaffle şablonları altın standart olarak kabul edilir. Bu yazma zaman SideWaffle Visual Studio 2012 için 2013 ve 2015 kullanılabilir.

### <a name="routing-and-multiple-views"></a>Yönlendirme ve birden çok görünüm

AngularJS SPA (tek sayfa uygulaması) tabanlı gezinti işlemek için bir yerleşik rota sağlayıcısı sahiptir. AngularJS içinde yönlendirme ile çalışmak için eklemelisiniz `angular-route` Bower kullanarak kitaplığı. Görebilirsiniz [bower.json](#angular-bower-json) biz zaten bu bizim projesinde başvurduğunuzdan bu makalenin başlangıcında başvurulan dosya.

Paketi yüklendikten sonra komut dosyası başvurusunu ekleyin (*route.js Açısal*) görünümünüz için.

Şimdi kişi oluşturma ve gezinti ekleyin uygulama atalım. İlk olarak, uygulama bir kopyasını yeni bir oluşturarak yapacağız `PeopleController` çağrılan eylem `Spa` ve karşılık gelen `Spa.cshtml` Index.cshtml görünümünde kopyalayarak Görünüm `People` klasör. Komut dosyası için bir başvuru ekleyin `angular-route` (satır 11 bakın). Ayrıca bir `div` ile işaretlenen `ng-view` yönergesi (bkz. satır 6) görünümlerde yerleştirmek için yer tutucu olarak. Birçok ek kullanılmasını kalacaklarını *.js* 13-16 satırlarında başvuran dosyaları.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

Bir göz atalım *personModule.js* nasıl biz yönlendirme modülü başlatmasını görmek için dosya. Geçiriyoruz `ngRoute` modüle kitaplık olarak. Bu modül, uygulamamız içinde yönlendirme işler.

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

*PersonRoutes.js* dosya, aşağıda, rota sağlayıcısını temel yolları tanımlar. Satırlar 4-7 etkili bir şekilde, bir URL ile zaman söyleyerek Gezinti tanımlayın `/persons` olduğundan istenen adlı bir şablon kullanmak `partials/personlist` aracılığıyla çalışarak `personListController`. Satırları 8-11 belirtmek ayrıntı sayfası bir rota parametresi ile `personId`. URL bir desen eşleşmiyorsa, Angular varsayılan olarak `/persons` görünümü.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

`personlist.html` Kısmi görünüm yalnızca kişi listesini görüntülemek için gereken HTML içeren bir dosyadır.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

Denetleyici modülün kullanılarak tanımlanan `controller` işlevi *personListController.js*.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

Bu uygulamayı çalıştırmak ve gidin `people/spa#/persons` URL, biz görürsünüz:

![Kişiler Liste Görünümü](angular/_static/spa-persons.png)

Biz ayrıntı sayfaya, örneğin gidin, `people/spa#/persons/2`, biz ayrıntı kısmi görünümü görürsünüz:

![Kişinin ayrıntılı Görünüm](angular/_static/spa-persons-2.png)

Tam kaynak ve bu makalede gösterilen olmayan herhangi bir dosya görüntüleyebilirsiniz [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

### <a name="event-handlers"></a>Olay İşleyicileri

Vardır, HTML DOM giriş öğeleri olay işleme özellikleri eklemeyi AngularJS yönergesi AngularJS yerleşik olaylarının bir listesi aşağıdadır.

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> Kullanarak kendi olay işleyicileri ekleme [özel yönergeleri özellik içinde AngularJS](https://docs.angularjs.org/guide/directive).

Ne bakalım `ng-click` olay kablolu ayarlama. Adlı yeni bir JavaScript dosyası oluşturun *eventHandlerController.js*ve aşağıdakileri ekleyin:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

Yeni fark `sayName` işlevi `eventHandlerController` üzerinde yukarıdaki 5 satır. Tüm yöntemi yaptığını Hoş Geldiniz iletisi kullanıcı JavaScript uyarı artık gösteren için.

Aşağıdaki görünüm denetleyicisini işlevi bir AngularJS olay bağlar. Satır 9 sahip bir düğme üzerinde `ng-click` Açısal yönergesi uygulandı. Çağırır bizim `sayName` bağlı işlevi `$scope` nesne bu görünüme geçirildi.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

Çalışan bir örnek gösterilmektedir denetleyicinin `sayName` işlevi düğme tıklatıldığında otomatik olarak çağrılır.

![Tıklama olayı](angular/_static/events.png)

AngularJS yerleşik olay işleyicisi yönergeleri hakkında daha fazla ayrıntı için head için mutlaka [belgeleri Web sitesi](https://docs.angularjs.org/api/ng/directive/ngClick) AngularJS.

## <a name="additional-resources"></a>Ek kaynaklar

* [Açısal belgeleri](https://docs.angularjs.org)

* [Açısal 2 bilgisi](https://angular.io/)
