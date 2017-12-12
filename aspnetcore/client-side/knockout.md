---
title: ASP.NET Core Knockout.js MVVM Framework
author: ardalis
description: 
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a>ASP.NET Core Knockout.js MVVM Framework

Tarafından [Steve Smith](https://ardalis.com/)

Boşaltılan karmaşık veri tabanlı kullanıcı arabirimi oluşturulmasını basitleştirir popüler bir JavaScript Kitaplığı ' dir. Tek başına veya jQuery gibi diğer kitaplıkları ile kullanılabilir. UI için yapılan değişiklikler, model onayladığında sağlayacak şekilde, bir JavaScript nesnesi olarak tanımlanan bir veri modeli kullanıcı Arabirimi öğeleri bağlamak için birincil amacı olan ve tersi. Boşaltılan bir desen kullanılması, Model-View-ViewModel (MVVM) bir web uygulamasının istemci-tarafı davranış kolaylaştırır. Bir Boşaltılan'ın MVVM uygulama ile çalışırken, bilgi gerekir iki ana kavramlar Gözlemlenenler ve bağlamaları ' dir.

## <a name="getting-started"></a>Başlarken

Boşaltılan bu nedenle yükleme bir tek JavaScript dosyası olarak dağıtılır ve kullanmaya olan çok basit kullanarak [bower](bower.md). Zaten sahip olduğu varsayılarak [bower](bower.md) ve [gulp](using-gulp.md) yapılandırılmış, açık *bower.json* , ASP.NET Core proje ve çakıştırma bağımlılık aşağıda gösterildiği gibi ekleyin:

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

El ile görev Çalıştırıcı Gezgini (altında görünüm ‣ diğer pencereler ‣ görev Çalıştırıcı Gezgini) açarak bower çalıştırın ve ardından Görevler altında üzerinde bower sağ tıklayın ve Çalıştır'ı seçin sonra bu yerinde kullanabilirsiniz. Sonuç şuna benzer görünmelidir:

![Görev Çalıştırıcı Gezgini içinde çalışan Boşaltılan bower](knockout/_static/bower-knockout.png)

Projenizin içinde bakarsanız şimdi `wwwroot` klasörünü lib klasörünün altındaki klasör yüklü Boşaltılan görmeniz gerekir.

![LIB klasöründe yüklü çakıştırma](knockout/_static/wwwroot-knockout.png)

Bu, kullanıcılarınızın dosyasının önbelleğe alınmış bir kopyası zaten sahip olur ve bu nedenle hiç indirmek ihtiyacınız olmadığından olasılığını arttıkça, üretim ortamınızda, bir içerik teslim ağı veya CDN, yoluyla Boşaltılan başvuru önerilir. Microsoft Ajax CDN, burada da dahil olmak üzere birkaç CDN'ler üzerinde Boşaltılan kullanılabilir:

[http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

Boşaltılan kullanacağı bir sayfada dahil etmek için yalnızca ekleyin bir `<script>` yerlerde, onu (uygulamanız ile veya bir CDN aracılığıyla) barındıracak dosyasına başvurma öğe:

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a>Gözlemlenenler, ViewModels ve basit bağlama

Zaten bir web sayfasında DOM doğrudan erişim aracılığıyla ya da öğeleri işlemek için JavaScript kullanarak veya jQuery gibi bir kitaplık kullanılarak alışık olabilir. Genellikle bu tür bir davranış doğrudan belirli kullanıcı eylemlerine yanıt öğesi değerlerini ayarlamak için kod yazma tarafından sağlanır. Boşaltılan ile bildirim temelli bir yaklaşım üzerinden öğeleri sayfada bir nesne üzerinde özelliklerine bağlı bunun yerine, alınır. DOM öğeleri işlemek için kod yazma, yerine kullanıcı eylemlerini yalnızca ViewModel nesnesi ile etkileşim ve çakıştırma mvc'deki sayfası öğeleri eşitlenmesini sağlar.

Basit bir örnek olarak aşağıdaki sayfa listesi göz önünde bulundurun. İçerdiği bir `<span>` öğesi ile bir `data-bind` metin içeriği için authorName bağlı olduğunu belirten özniteliği. Bir değişken viewModel tek bir özellikte ile tanımlanan bir JavaScript bloğu içinde sonraki `authorName`, bazı değerine ayarlayın. Son olarak, bir çağrı `ko.applyBindings` , bu viewModel değişkeninde geçirme yapılır.

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

Tarayıcı, içeriği görüntülendiğinde <span> öğesi viewModel değişkeninde değeriyle değiştirilir:

![Boşaltılan basit bağlama](knockout/_static/simple-binding-screenshot.png)

Artık basit tek yönlü bağlama çalışma sunuyoruz. Hiçbir yerde kodda biz aralık 's içeriği için bir değer atamak için JavaScript yazdım olduğunu dikkat edin. Biz ViewModel değiştirmek istiyorsanız, biz bu başka bir adım yapmanıza ve bir HTML giriş metin kutusu ekleyin ve değeri için olduğu gibi bağlamak için:

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

Sayfa yeniden yükleniyor, biz bu değer, giriş kutusu gerçekten bağlı olduğu bakın:

![Boşaltılan Giriş bağlama](knockout/_static/input-binding-screenshot.png)

Ancak, biz textbox değeri değiştirirseniz, karşılık gelen değeri `<span>` öğesi değiştirmez. Neden?

Hiçbir şey bildirim sorundur `<span>` güncelleştirilmesi gereken. ViewModel'in özellikleri özel bir türünde sarılır sürece yalnızca ViewModel güncelleştirme tek başına yeterli değildir. Kullanmak ihtiyacımız **gözlemlenenler** ViewModel bunlar ortaya çıktığında otomatik olarak güncelleştirilen değişiklikler yapmanız herhangi bir özellik için de. Kullanılacak ViewModel değiştirerek `ko.observable("value")` yalnızca "value" yerine, bir değişiklik yapıldığında, değerine bağlı HTML öğelerinin ViewModel güncelleştirir. Siz yazarken öğeleri bağlı değişiklikleri görmezsiniz bunlar odağı kaybedersiniz kadar giriş kutularının değerlerine güncelleştirmemeniz unutmayın.

> [!NOTE]
> Her keypress ekleme sorunudur sonra canlı güncelleştirme desteği ekleme `valueUpdate: "afterkeydown"` için `data-bind` özniteliğin içeriği. Bu davranış kullanarak da elde edebilirsiniz `data-bind="textInput: authorName"` değerlerin anlık güncelleştirmeleri almak için. 

Ko.observable kullanacak şekilde güncelleştirdikten sonra bizim viewModel:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

Boşaltılan bağlamaları farklı türde destekler. Şu ana kadar nasıl bağlanacağını gördük `text` ve `value`. Ayrıca, herhangi bir belirtilen öznitelik bağlayabilirsiniz. Örneğin, bir yer işareti etiketi ile bir köprü oluşturmak için `src` özniteliği viewModel bağlanabilir. Boşaltılan Ayrıca işlevleri için bağlamayı destekler. Bunu göstermek için şimdi yazarın twitter tanıtıcısı dahil etmek için viewModel güncelleştirin ve twitter tanıtıcı yazarın twitter sayfasına bir bağlantı olarak görüntülenir. Biz bu üç aşamada gerçekleştirirsiniz.

İlk olarak, yazarın adından sonra parantez içinde göstereceğiz köprü görüntülenecek HTML ekleyin:

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

Ardından, viewModel twitterUrl ve twitterAlias özellikleri içerecek şekilde güncelleştirin:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

Bu noktada Biz bu twitter diğer adı için doğru URL'yi gitmek için twitterUrl güncelleştirilmemiş henüz – twitter.com yalnızca işaret eden dikkat edin. Ayrıca yeni Boşaltılan işlevi kullanıyorsanız fark `computed`, twitterUrl için. Bu değiştirirse, tüm kullanıcı Arabirimi öğeleri bildirir gözlemlenebilir bir işlevdir. Ancak, bu diğer özellikleri viewModel erişiminiz biz böylece her bir özellik kendi deyimi viewModel nasıl oluşturuyoruz değiştirmeniz gerekir.

Düzenlenen viewModel bildirimi aşağıda gösterilmiştir. Bunu artık bir işlevi bildirildi. Her bir özellik noktalı virgül ile biten kendi deyimi şimdi olduğuna dikkat edin. Ayrıca twitterAlias özellik değeri erişmek için kendi başvuru içerecek şekilde (), yürütmek ihtiyacımız olmadığını dikkat edin.

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

Sonucu tarayıcıda beklendiği gibi çalışır:

![Boşaltılan köprü](knockout/_static/hyperlink-screenshot.png)

Boşaltılan ayrıca click olayını gibi belirli kullanıcı Arabirimi öğesi olaylar için bağlamayı destekler. Bu, kolay ve bildirimli olarak kullanıcı Arabirimi öğeleri uygulamanın viewModel işlevlerinde bağlamanıza olanak sağlar. Bir düğme ekleyebiliriz basit bir örnek olarak, tıklatıldığında, yazarın twitterAlias tümü büyük harf olarak değiştirir.

İlk olarak, biz Ekle düğmesi, olay ve viewModel eklemek için yapacağız işlevi adına başvuran düğmenin bağlama tıklatın:

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

Ardından, viewModel işlevi ekleyin ve viewModel'ın durumunu değiştirmek için yukarı wire. TwitterAlias özelliği için yeni bir değer ayarlamak için size bir yöntem olarak çağırın ve yeni değer geçirmek dikkat edin.

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

Kod çalıştırmasını ve düğmesini tıklatarak görüntülenen bağlantıyı beklendiği gibi değiştirir:

![Köprü büyük harfe çevirme](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a>Denetim akışı

Boşaltılan koşullu ve döngü işlemler gerçekleştirebilir bağlamaları içerir. Döngü işlemleri kullanıcı Arabirimi listeler, menüleri ve kılavuzları veya tablolar için veri listeleri bağlama için özellikle yararlıdır. Foreach bağlama dizi yineleme. Öğeleri eklendiğinde veya kullanıcı Arabirimi ağacında her öğenin tekrar oluşturmak zorunda kalmadan diziden kaldırıldığında observable dizisi ile kullanıldığında, kullanıcı Arabirimi öğeleri otomatik olarak güncelleştirilecektir. Aşağıdaki örnek bir oyun sonuçları observable dizisi içeren yeni bir viewModel kullanır. İki sütun kullanarak basit bir tabloya bağlı olduğu bir `foreach` bağlama `<tbody>` öğesi. Her `<tr>` öğesi içinde `<tbody>` gameResults koleksiyonun bir öğesi için bağlı.

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

"Yeni" (applyBindings çağrısında) kullanarak oluşturmak bekliyoruz çünkü bu saat biz ViewModel büyük "V" ile kullandığınızdan dikkat edin. Çalıştırıldığında, sayfa şunlara sebep olur:

![Boşaltılan kayıt görünüm modeli](knockout/_static/record-screenshot.png)

Observable koleksiyonu çalıştığını göstermek için biraz daha fazla işlevsellik ekleyelim. Biz ViewModel başka bir oyuna sonuçlarını kaydetmek yeteneğini de içerir ve ardından bir düğmeyi ve bu yeni işlev ile çalışmak için bazı kullanıcı Arabirimi ekleyin.  İlk olarak, addResult yöntem oluşturalım:

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

Bu yöntemi kullanarak bir düğme bağlamak `click` bağlama:

```html
<button data-bind="click: addResult">Add New Result</button>
```

Tarayıcıda sayfasını açın ve birkaç kez düğmesini yeni bir tablo satırının her tıklatma ile sonuçlanır:

![Sonuçlar ekleyin](knockout/_static/record-addresult-screenshot.png)

UI, genellikle ya da satır içi veya ayrı bir form yeni kayıtlar ekleme desteklemek için birkaç yolu vardır. Biz şeyi düzenlenebilir nitelikte. böylece metin kutuları ve dropdownlists kullanılacak tabloyu kolaylıkla değiştirebilirsiniz. Yalnızca değiştirme `<tr>` gösterildiği gibi öğe:

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

Unutmayın `$root` kök ViewModel, olası seçenekler Burada sunulan olduğu anlamına gelir. `$data`-her biri basit bir dizedir resultChoices dizinin tek tek bir öğeye başvuruyor bu durumda, belirli bir bağlam içinde ne olursa olsun geçerli modelin olduğu için ifade eder.

Bu değişiklikle, tüm kılavuz düzenlenebilir olur:

![Düzenlenebilir kılavuz](knockout/_static/editable-grid-screenshot.png)

Boşaltılan kullanarak doğru Biz bu tümünün jQuery kullanılarak elde, ancak büyük olasılıkla bunu istemezsiniz neredeyse kadar etkili olabilir. Boşaltılan hangi kullanıcı Arabirimi öğeleri ViewModel öğeleri karşılık ilişkili veri izler ve yalnızca eklenemez, kaldırılamaz veya güncelleştirilmiş gerekiyorsa bu öğeleri güncelleştirir. Bu kendisini jQuery veya doğrudan DOM işleme kullanarak elde etmek için önemli çaba götürecek ve biz sonra tablonun verilerine dayalı toplama sonuçları (örneğin, kazanç-kayıp kaydını) görüntülemek istiyorsanız, daha sonra bir kez daha üzerinden döngü ve ayrıştırmak ihtiyacımız HTML öğeleri.  Boşaltılan ile kazanç-kayıp kayıt görüntüleme kısmı oldukça kolaydır. Biz ViewModel içinde hesaplamalar ve basit bir metin bağlama ile görüntülemek ve bir `<span>`.

Kazanç-Kayıp kayıt dizesi oluşturmak için hesaplanan observable kullanabilirsiniz. Observable özellikleri ViewModel içinde başvurular Not işlev çağrılarını olmalıdır, aksi halde bunlar observable değerini alacak değil (yani `gameResults()` değil `gameResults` gösterilen kodda):

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

Bu işlev bir aralık içinde bağlamak `<h1>` sayfanın üst kısmındaki öğe:

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

Sonuç:

![Win kaybı](knockout/_static/record-winloss-screenshot.png)

Satırlar ekleme veya herhangi bir satırın sonuç sütunu seçili öğe değiştirme pencerenin üst kısmında gösterilen kaydı güncelleştirir.

Değerleri bağlama yanı sıra, neredeyse her yasal JavaScript ifadesi bir bağlama içinde de kullanabilirsiniz. Bir kullanıcı Arabirimi öğesi yalnızca bir değer belirli bir eşiği aştığında gibi belirli koşullar altında görünmelidir Örneğin, bu mantıksal olarak bağlama deyimi içinde belirtebilirsiniz:

```html
<div data-bind="visible: customerValue > 100"></div>
```

Bu `<div>` customerValue 100'den olduğunda yalnızca görünür olacaktır.

## <a name="templates"></a>Şablonlar

Böylece kolayca UI davranışı içinden ayırmak veya artımlı olarak isteğe bağlı büyük bir uygulamaya kullanıcı Arabirimi öğeleri yük Boşaltılan şablonlar için destek vardır. Her satır, yalnızca HTML out bir şablona çekme ve üzerinde veri bağlama çağrısında adıyla şablonu belirterek kendi şablonu yapmak için önceki örneğimizde güncelleştiriyoruz `<tbody>`.

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

Boşaltılan jQuery.tmpl kitaplığı gibi diğer şablon altyapılarını ve Underscore.js'ın şablon altyapısı da destekler.

## <a name="components"></a>Bileşenler

Bileşenleri, düzenlemenize ve genellikle UI kodu bağımlı olduğu ViewModel verilerin UI kodunu yeniden olanak sağlar. Bir bileşen oluşturmak için kendi şablon ve onun viewModel belirtin ve bir ad vermek yeterlidir. Bu çağırarak yapılır `ko.components.register()`. Şablonlar ve viewmodel satır içi tanımlamanın yanı sıra, bunlar gibi bir kitaplık kullanılarak dış dosyalarından yüklenebilir *require.js*, sonuçta elde edilen çok temiz ve verimli kod.

## <a name="communicating-with-apis"></a>API'leri ile iletişim

Boşaltılan JSON biçiminde herhangi bir veri ile çalışabilirsiniz. Almak ve çakıştırma kullanarak verileri kaydetmek için genel bir destekleyen jQuery ile yoludur `$.getJSON()` veri almak için işlevi ve `$.post()` tarayıcıdan bir API uç noktasına veri göndermesini yöntemi. JSON veri göndermek ve almak için farklı bir şekilde tercih ederseniz, doğal olarak, çakıştırma ile de çalışır.

## <a name="summary"></a>Özet

Boşaltılan ViewModel içinde tanımlanan istemci uygulamasının geçerli durumu için UI öğeleri bağlamak için basit ve zarif bir yol sağlar. İşlenecek HTML öğelerine uygulanan data-bind özniteliği Boşaltılan'ın bağlama sözdizimini kullanır. Boşaltılan verimli bir şekilde işlemek ve kullanıcı Arabirimi öğeleri izleyerek büyük veri kümelerini güncelleştirme ve etkilenen öğeleri yalnızca değişiklikleri işleme. Büyük uygulamalar şablonları ve isteğe bağlı harici dosyaları'ndan yüklenebilir bileşenleri kullanan kullanıcı Arabirimi mantığı bölmeniz. Şu anda 3, çakıştırma zengin istemci etkileşim gerektiren web uygulamalar geliştirebilir kararlı bir JavaScript kitaplığı sürümüdür.
