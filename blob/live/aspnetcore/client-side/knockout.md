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
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a><span data-ttu-id="e57db-103">ASP.NET Core Knockout.js MVVM Framework</span><span class="sxs-lookup"><span data-stu-id="e57db-103">Knockout.js MVVM Framework in ASP.NET Core</span></span>

<span data-ttu-id="e57db-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e57db-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e57db-105">Boşaltılan karmaşık veri tabanlı kullanıcı arabirimi oluşturulmasını basitleştirir popüler bir JavaScript Kitaplığı ' dir.</span><span class="sxs-lookup"><span data-stu-id="e57db-105">Knockout is a popular JavaScript library that simplifies the creation of complex data-based user interfaces.</span></span> <span data-ttu-id="e57db-106">Tek başına veya jQuery gibi diğer kitaplıkları ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e57db-106">It can be used alone or with other libraries, such as jQuery.</span></span> <span data-ttu-id="e57db-107">UI için yapılan değişiklikler, model onayladığında sağlayacak şekilde, bir JavaScript nesnesi olarak tanımlanan bir veri modeli kullanıcı Arabirimi öğeleri bağlamak için birincil amacı olan ve tersi.</span><span class="sxs-lookup"><span data-stu-id="e57db-107">Its primary purpose is to bind UI elements to an underlying data model defined as a JavaScript object, such that when changes are made to the UI, the model is updated, and vice versa.</span></span> <span data-ttu-id="e57db-108">Boşaltılan bir desen kullanılması, Model-View-ViewModel (MVVM) bir web uygulamasının istemci-tarafı davranış kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="e57db-108">Knockout facilitates the use of a Model-View-ViewModel (MVVM) pattern in a web application's client-side behavior.</span></span> <span data-ttu-id="e57db-109">Bir Boşaltılan'ın MVVM uygulama ile çalışırken, bilgi gerekir iki ana kavramlar Gözlemlenenler ve bağlamaları ' dir.</span><span class="sxs-lookup"><span data-stu-id="e57db-109">The two main concepts one must learn when working with Knockout's MVVM implementation are Observables and Bindings.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e57db-110">Başlarken</span><span class="sxs-lookup"><span data-stu-id="e57db-110">Getting started</span></span>

<span data-ttu-id="e57db-111">Boşaltılan bu nedenle yükleme bir tek JavaScript dosyası olarak dağıtılır ve kullanmaya olan çok basit kullanarak [bower](bower.md).</span><span class="sxs-lookup"><span data-stu-id="e57db-111">Knockout is deployed as a single JavaScript file, so installing and using it is very straightforward using [bower](bower.md).</span></span> <span data-ttu-id="e57db-112">Zaten sahip olduğu varsayılarak [bower](bower.md) ve [gulp](using-gulp.md) yapılandırılmış, açık *bower.json* , ASP.NET Core proje ve çakıştırma bağımlılık aşağıda gösterildiği gibi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e57db-112">Assuming you already have [bower](bower.md) and [gulp](using-gulp.md) configured, open *bower.json* in your ASP.NET Core project and add the knockout dependency as shown here:</span></span>

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

<span data-ttu-id="e57db-113">El ile görev Çalıştırıcı Gezgini (altında görünüm ‣ diğer pencereler ‣ görev Çalıştırıcı Gezgini) açarak bower çalıştırın ve ardından Görevler altında üzerinde bower sağ tıklayın ve Çalıştır'ı seçin sonra bu yerinde kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e57db-113">With this in place, you can then manually run bower by opening the Task Runner Explorer (under View ‣ Other Windows ‣ Task Runner Explorer) and then under Tasks, right-click on bower and select Run.</span></span> <span data-ttu-id="e57db-114">Sonuç şuna benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="e57db-114">The result should appear similar to this:</span></span>

![Görev Çalıştırıcı Gezgini içinde çalışan Boşaltılan bower](knockout/_static/bower-knockout.png)

<span data-ttu-id="e57db-116">Projenizin içinde bakarsanız şimdi `wwwroot` klasörünü lib klasörünün altındaki klasör yüklü Boşaltılan görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e57db-116">Now if you look in your project's `wwwroot` folder, you should see knockout installed under the lib folder.</span></span>

![LIB klasöründe yüklü çakıştırma](knockout/_static/wwwroot-knockout.png)

<span data-ttu-id="e57db-118">Bu, kullanıcılarınızın dosyasının önbelleğe alınmış bir kopyası zaten sahip olur ve bu nedenle hiç indirmek ihtiyacınız olmadığından olasılığını arttıkça, üretim ortamınızda, bir içerik teslim ağı veya CDN, yoluyla Boşaltılan başvuru önerilir.</span><span class="sxs-lookup"><span data-stu-id="e57db-118">It's recommended that in your production environment you reference knockout via a Content Delivery Network, or CDN, as this increases the likelihood that your users will already have a cached copy of the file and thus will not need to download it at all.</span></span> <span data-ttu-id="e57db-119">Microsoft Ajax CDN, burada da dahil olmak üzere birkaç CDN'ler üzerinde Boşaltılan kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="e57db-119">Knockout is available on several CDNs, including the Microsoft Ajax CDN, here:</span></span>

[<span data-ttu-id="e57db-120">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="e57db-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

<span data-ttu-id="e57db-121">Boşaltılan kullanacağı bir sayfada dahil etmek için yalnızca ekleyin bir `<script>` yerlerde, onu (uygulamanız ile veya bir CDN aracılığıyla) barındıracak dosyasına başvurma öğe:</span><span class="sxs-lookup"><span data-stu-id="e57db-121">To include Knockout on a page that will use it, simply add a `<script>` element referencing the file from wherever you will be hosting it (with your application, or via a CDN):</span></span>

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a><span data-ttu-id="e57db-122">Gözlemlenenler, ViewModels ve basit bağlama</span><span class="sxs-lookup"><span data-stu-id="e57db-122">Observables, ViewModels, and simple binding</span></span>

<span data-ttu-id="e57db-123">Zaten bir web sayfasında DOM doğrudan erişim aracılığıyla ya da öğeleri işlemek için JavaScript kullanarak veya jQuery gibi bir kitaplık kullanılarak alışık olabilir.</span><span class="sxs-lookup"><span data-stu-id="e57db-123">You may already be familiar with using JavaScript to manipulate elements on a web page, either via direct access to the DOM or using a library like jQuery.</span></span> <span data-ttu-id="e57db-124">Genellikle bu tür bir davranış doğrudan belirli kullanıcı eylemlerine yanıt öğesi değerlerini ayarlamak için kod yazma tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="e57db-124">Typically this kind of behavior is achieved by writing code to directly set element values in response to certain user actions.</span></span> <span data-ttu-id="e57db-125">Boşaltılan ile bildirim temelli bir yaklaşım üzerinden öğeleri sayfada bir nesne üzerinde özelliklerine bağlı bunun yerine, alınır.</span><span class="sxs-lookup"><span data-stu-id="e57db-125">With Knockout, a declarative approach is taken instead, through which elements on the page are bound to properties on an object.</span></span> <span data-ttu-id="e57db-126">DOM öğeleri işlemek için kod yazma, yerine kullanıcı eylemlerini yalnızca ViewModel nesnesi ile etkileşim ve çakıştırma mvc'deki sayfası öğeleri eşitlenmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="e57db-126">Instead of writing code to manipulate DOM elements, user actions simply interact with the ViewModel object, and Knockout takes care of ensuring the page elements are synchronized.</span></span>

<span data-ttu-id="e57db-127">Basit bir örnek olarak aşağıdaki sayfa listesi göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="e57db-127">As a simple example, consider the page list below.</span></span> <span data-ttu-id="e57db-128">İçerdiği bir `<span>` öğesi ile bir `data-bind` metin içeriği için authorName bağlı olduğunu belirten özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e57db-128">It includes a `<span>` element with a `data-bind` attribute indicating that the text content should be bound to authorName.</span></span> <span data-ttu-id="e57db-129">Bir değişken viewModel tek bir özellikte ile tanımlanan bir JavaScript bloğu içinde sonraki `authorName`, bazı değerine ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="e57db-129">Next, in a JavaScript block a variable viewModel is defined with a single property, `authorName`, set to some value.</span></span> <span data-ttu-id="e57db-130">Son olarak, bir çağrı `ko.applyBindings` , bu viewModel değişkeninde geçirme yapılır.</span><span class="sxs-lookup"><span data-stu-id="e57db-130">Finally, a call to `ko.applyBindings` is made, passing in this viewModel variable.</span></span>

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

<span data-ttu-id="e57db-131">Tarayıcı, içeriği görüntülendiğinde <span> öğesi viewModel değişkeninde değeriyle değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="e57db-131">When viewed in the browser, the content of the <span> element is replaced with the value in the viewModel variable:</span></span>

![Boşaltılan basit bağlama](knockout/_static/simple-binding-screenshot.png)

<span data-ttu-id="e57db-133">Artık basit tek yönlü bağlama çalışma sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="e57db-133">We now have simple one-way binding working.</span></span> <span data-ttu-id="e57db-134">Hiçbir yerde kodda biz aralık 's içeriği için bir değer atamak için JavaScript yazdım olduğunu dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e57db-134">Notice that nowhere in the code did we write JavaScript to assign a value to the span's contents.</span></span> <span data-ttu-id="e57db-135">Biz ViewModel değiştirmek istiyorsanız, biz bu başka bir adım yapmanıza ve bir HTML giriş metin kutusu ekleyin ve değeri için olduğu gibi bağlamak için:</span><span class="sxs-lookup"><span data-stu-id="e57db-135">If we want to manipulate the ViewModel, we can take this a step further and add an HTML input textbox, and bind to its value, like so:</span></span>

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

<span data-ttu-id="e57db-136">Sayfa yeniden yükleniyor, biz bu değer, giriş kutusu gerçekten bağlı olduğu bakın:</span><span class="sxs-lookup"><span data-stu-id="e57db-136">Reloading the page, we see that this value is indeed bound to the input box:</span></span>

![Boşaltılan Giriş bağlama](knockout/_static/input-binding-screenshot.png)

<span data-ttu-id="e57db-138">Ancak, biz textbox değeri değiştirirseniz, karşılık gelen değeri `<span>` öğesi değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="e57db-138">However, if we change the value in the textbox, the corresponding value in the `<span>` element doesn't change.</span></span> <span data-ttu-id="e57db-139">Neden?</span><span class="sxs-lookup"><span data-stu-id="e57db-139">Why not?</span></span>

<span data-ttu-id="e57db-140">Hiçbir şey bildirim sorundur `<span>` güncelleştirilmesi gereken.</span><span class="sxs-lookup"><span data-stu-id="e57db-140">The issue is that nothing notified the `<span>` that it needed to be updated.</span></span> <span data-ttu-id="e57db-141">ViewModel'in özellikleri özel bir türünde sarılır sürece yalnızca ViewModel güncelleştirme tek başına yeterli değildir.</span><span class="sxs-lookup"><span data-stu-id="e57db-141">Simply updating the ViewModel isn't by itself sufficient, unless the ViewModel's properties are wrapped in a special type.</span></span> <span data-ttu-id="e57db-142">Kullanmak ihtiyacımız **gözlemlenenler** ViewModel bunlar ortaya çıktığında otomatik olarak güncelleştirilen değişiklikler yapmanız herhangi bir özellik için de.</span><span class="sxs-lookup"><span data-stu-id="e57db-142">We need to use **observables** in the ViewModel for any properties that need to have changes automatically updated as they occur.</span></span> <span data-ttu-id="e57db-143">Kullanılacak ViewModel değiştirerek `ko.observable("value")` yalnızca "value" yerine, bir değişiklik yapıldığında, değerine bağlı HTML öğelerinin ViewModel güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="e57db-143">By changing the ViewModel to use `ko.observable("value")` instead of just "value", the ViewModel will update any HTML elements that are bound to its value whenever a change occurs.</span></span> <span data-ttu-id="e57db-144">Siz yazarken öğeleri bağlı değişiklikleri görmezsiniz bunlar odağı kaybedersiniz kadar giriş kutularının değerlerine güncelleştirmemeniz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e57db-144">Note that input boxes don't update their value until they lose focus, so you won't see changes to bound elements as you type.</span></span>

> [!NOTE]
> <span data-ttu-id="e57db-145">Her keypress ekleme sorunudur sonra canlı güncelleştirme desteği ekleme `valueUpdate: "afterkeydown"` için `data-bind` özniteliğin içeriği.</span><span class="sxs-lookup"><span data-stu-id="e57db-145">Adding support for live updating after each keypress is simply a matter of adding `valueUpdate: "afterkeydown"` to the `data-bind` attribute's contents.</span></span> <span data-ttu-id="e57db-146">Bu davranış kullanarak da elde edebilirsiniz `data-bind="textInput: authorName"` değerlerin anlık güncelleştirmeleri almak için.</span><span class="sxs-lookup"><span data-stu-id="e57db-146">You can also get this behavior by using `data-bind="textInput: authorName"` to get instant updates of values.</span></span> 

<span data-ttu-id="e57db-147">Ko.observable kullanacak şekilde güncelleştirdikten sonra bizim viewModel:</span><span class="sxs-lookup"><span data-stu-id="e57db-147">Our viewModel, after updating it to use ko.observable:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="e57db-148">Boşaltılan bağlamaları farklı türde destekler.</span><span class="sxs-lookup"><span data-stu-id="e57db-148">Knockout supports a number of different kinds of bindings.</span></span> <span data-ttu-id="e57db-149">Şu ana kadar nasıl bağlanacağını gördük `text` ve `value`.</span><span class="sxs-lookup"><span data-stu-id="e57db-149">So far we've seen how to bind to `text` and to `value`.</span></span> <span data-ttu-id="e57db-150">Ayrıca, herhangi bir belirtilen öznitelik bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e57db-150">You can also bind to any given attribute.</span></span> <span data-ttu-id="e57db-151">Örneğin, bir yer işareti etiketi ile bir köprü oluşturmak için `src` özniteliği viewModel bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="e57db-151">For instance, to create a hyperlink with an anchor tag, the `src` attribute can be bound to the viewModel.</span></span> <span data-ttu-id="e57db-152">Boşaltılan Ayrıca işlevleri için bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="e57db-152">Knockout also supports binding to functions.</span></span> <span data-ttu-id="e57db-153">Bunu göstermek için şimdi yazarın twitter tanıtıcısı dahil etmek için viewModel güncelleştirin ve twitter tanıtıcı yazarın twitter sayfasına bir bağlantı olarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e57db-153">To demonstrate this, let's update the viewModel to include the author's twitter handle, and display the twitter handle as a link to the author's twitter page.</span></span> <span data-ttu-id="e57db-154">Biz bu üç aşamada gerçekleştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e57db-154">We'll do this in three stages.</span></span>

<span data-ttu-id="e57db-155">İlk olarak, yazarın adından sonra parantez içinde göstereceğiz köprü görüntülenecek HTML ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e57db-155">First, add the HTML to display the hyperlink, which we'll show in parentheses after the author's name:</span></span>

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

<span data-ttu-id="e57db-156">Ardından, viewModel twitterUrl ve twitterAlias özellikleri içerecek şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="e57db-156">Next, update the viewModel to include the twitterUrl and twitterAlias properties:</span></span>

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

<span data-ttu-id="e57db-157">Bu noktada Biz bu twitter diğer adı için doğru URL'yi gitmek için twitterUrl güncelleştirilmemiş henüz – twitter.com yalnızca işaret eden dikkat edin. Ayrıca yeni Boşaltılan işlevi kullanıyorsanız fark `computed`, twitterUrl için.</span><span class="sxs-lookup"><span data-stu-id="e57db-157">Notice that at this point we haven't yet updated the twitterUrl to go to the correct URL for this twitter alias – it's just pointing at twitter.com. Also notice that we're using a new Knockout function, `computed`, for twitterUrl.</span></span> <span data-ttu-id="e57db-158">Bu değiştirirse, tüm kullanıcı Arabirimi öğeleri bildirir gözlemlenebilir bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="e57db-158">This is an observable function that will notify any UI elements if it changes.</span></span> <span data-ttu-id="e57db-159">Ancak, bu diğer özellikleri viewModel erişiminiz biz böylece her bir özellik kendi deyimi viewModel nasıl oluşturuyoruz değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e57db-159">However, for it to have access to other properties in the viewModel, we need to change how we are creating the viewModel, so that each property is its own statement.</span></span>

<span data-ttu-id="e57db-160">Düzenlenen viewModel bildirimi aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e57db-160">The revised viewModel declaration is shown below.</span></span> <span data-ttu-id="e57db-161">Bunu artık bir işlevi bildirildi.</span><span class="sxs-lookup"><span data-stu-id="e57db-161">It is now declared as a function.</span></span> <span data-ttu-id="e57db-162">Her bir özellik noktalı virgül ile biten kendi deyimi şimdi olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e57db-162">Notice that each property is its own statement now, ending with a semicolon.</span></span> <span data-ttu-id="e57db-163">Ayrıca twitterAlias özellik değeri erişmek için kendi başvuru içerecek şekilde (), yürütmek ihtiyacımız olmadığını dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e57db-163">Also notice that to access the twitterAlias property value, we need to execute it, so its reference includes ().</span></span>

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

<span data-ttu-id="e57db-164">Sonucu tarayıcıda beklendiği gibi çalışır:</span><span class="sxs-lookup"><span data-stu-id="e57db-164">The result works as expected in the browser:</span></span>

![Boşaltılan köprü](knockout/_static/hyperlink-screenshot.png)

<span data-ttu-id="e57db-166">Boşaltılan ayrıca click olayını gibi belirli kullanıcı Arabirimi öğesi olaylar için bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="e57db-166">Knockout also supports binding to certain UI element events, such as the click event.</span></span> <span data-ttu-id="e57db-167">Bu, kolay ve bildirimli olarak kullanıcı Arabirimi öğeleri uygulamanın viewModel işlevlerinde bağlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e57db-167">This allows you to easily and declaratively bind UI elements to functions within the application's viewModel.</span></span> <span data-ttu-id="e57db-168">Bir düğme ekleyebiliriz basit bir örnek olarak, tıklatıldığında, yazarın twitterAlias tümü büyük harf olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="e57db-168">As a simple example, we can add a button that, when clicked, modifies the author's twitterAlias to be all caps.</span></span>

<span data-ttu-id="e57db-169">İlk olarak, biz Ekle düğmesi, olay ve viewModel eklemek için yapacağız işlevi adına başvuran düğmenin bağlama tıklatın:</span><span class="sxs-lookup"><span data-stu-id="e57db-169">First, we add the button, binding to the button's click event, and referencing the function name we're going to add to the viewModel:</span></span>

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

<span data-ttu-id="e57db-170">Ardından, viewModel işlevi ekleyin ve viewModel'ın durumunu değiştirmek için yukarı wire.</span><span class="sxs-lookup"><span data-stu-id="e57db-170">Then, add the function to the viewModel, and wire it up to modify the viewModel's state.</span></span> <span data-ttu-id="e57db-171">TwitterAlias özelliği için yeni bir değer ayarlamak için size bir yöntem olarak çağırın ve yeni değer geçirmek dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e57db-171">Notice that to set a new value to the twitterAlias property, we call it as a method and pass in the new value.</span></span>

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

<span data-ttu-id="e57db-172">Kod çalıştırmasını ve düğmesini tıklatarak görüntülenen bağlantıyı beklendiği gibi değiştirir:</span><span class="sxs-lookup"><span data-stu-id="e57db-172">Running the code and clicking the button modifies the displayed link as expected:</span></span>

![Köprü büyük harfe çevirme](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a><span data-ttu-id="e57db-174">Denetim akışı</span><span class="sxs-lookup"><span data-stu-id="e57db-174">Control flow</span></span>

<span data-ttu-id="e57db-175">Boşaltılan koşullu ve döngü işlemler gerçekleştirebilir bağlamaları içerir.</span><span class="sxs-lookup"><span data-stu-id="e57db-175">Knockout includes bindings that can perform conditional and looping operations.</span></span> <span data-ttu-id="e57db-176">Döngü işlemleri kullanıcı Arabirimi listeler, menüleri ve kılavuzları veya tablolar için veri listeleri bağlama için özellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="e57db-176">Looping operations are especially useful for binding lists of data to UI lists, menus, and grids or tables.</span></span> <span data-ttu-id="e57db-177">Foreach bağlama dizi yineleme.</span><span class="sxs-lookup"><span data-stu-id="e57db-177">The foreach binding will iterate over an array.</span></span> <span data-ttu-id="e57db-178">Öğeleri eklendiğinde veya kullanıcı Arabirimi ağacında her öğenin tekrar oluşturmak zorunda kalmadan diziden kaldırıldığında observable dizisi ile kullanıldığında, kullanıcı Arabirimi öğeleri otomatik olarak güncelleştirilecektir.</span><span class="sxs-lookup"><span data-stu-id="e57db-178">When used with an observable array, it will automatically update the UI elements when items are added or removed from the array, without re-creating every element in the UI tree.</span></span> <span data-ttu-id="e57db-179">Aşağıdaki örnek bir oyun sonuçları observable dizisi içeren yeni bir viewModel kullanır.</span><span class="sxs-lookup"><span data-stu-id="e57db-179">The following example uses a new viewModel which includes an observable array of game results.</span></span> <span data-ttu-id="e57db-180">İki sütun kullanarak basit bir tabloya bağlı olduğu bir `foreach` bağlama `<tbody>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="e57db-180">It is bound to a simple table with two columns using a `foreach` binding on the `<tbody>` element.</span></span> <span data-ttu-id="e57db-181">Her `<tr>` öğesi içinde `<tbody>` gameResults koleksiyonun bir öğesi için bağlı.</span><span class="sxs-lookup"><span data-stu-id="e57db-181">Each `<tr>` element within `<tbody>` will be bound to an element of the gameResults collection.</span></span>

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

<span data-ttu-id="e57db-182">"Yeni" (applyBindings çağrısında) kullanarak oluşturmak bekliyoruz çünkü bu saat biz ViewModel büyük "V" ile kullandığınızdan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e57db-182">Notice that this time we're using ViewModel with a capital “V" because we expect to construct it using “new" (in the applyBindings call).</span></span> <span data-ttu-id="e57db-183">Çalıştırıldığında, sayfa şunlara sebep olur:</span><span class="sxs-lookup"><span data-stu-id="e57db-183">When executed, the page results in the following output:</span></span>

![Boşaltılan kayıt görünüm modeli](knockout/_static/record-screenshot.png)

<span data-ttu-id="e57db-185">Observable koleksiyonu çalıştığını göstermek için biraz daha fazla işlevsellik ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="e57db-185">To demonstrate that the observable collection is working, let's add a bit more functionality.</span></span> <span data-ttu-id="e57db-186">Biz ViewModel başka bir oyuna sonuçlarını kaydetmek yeteneğini de içerir ve ardından bir düğmeyi ve bu yeni işlev ile çalışmak için bazı kullanıcı Arabirimi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e57db-186">We can include the ability to record the results of another game to the ViewModel, and then add a button and some UI to work with this new function.</span></span>  <span data-ttu-id="e57db-187">İlk olarak, addResult yöntem oluşturalım:</span><span class="sxs-lookup"><span data-stu-id="e57db-187">First, let's create the addResult method:</span></span>

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

<span data-ttu-id="e57db-188">Bu yöntemi kullanarak bir düğme bağlamak `click` bağlama:</span><span class="sxs-lookup"><span data-stu-id="e57db-188">Bind this method to a button using the `click` binding:</span></span>

```html
<button data-bind="click: addResult">Add New Result</button>
```

<span data-ttu-id="e57db-189">Tarayıcıda sayfasını açın ve birkaç kez düğmesini yeni bir tablo satırının her tıklatma ile sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="e57db-189">Open the page in the browser and click the button a couple of times, resulting in a new table row with each click:</span></span>

![Sonuçlar ekleyin](knockout/_static/record-addresult-screenshot.png)

<span data-ttu-id="e57db-191">UI, genellikle ya da satır içi veya ayrı bir form yeni kayıtlar ekleme desteklemek için birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="e57db-191">There are a few ways to support adding new records in the UI, typically either inline or in a separate form.</span></span> <span data-ttu-id="e57db-192">Biz şeyi düzenlenebilir nitelikte. böylece metin kutuları ve dropdownlists kullanılacak tabloyu kolaylıkla değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e57db-192">We can easily modify the table to use textboxes and dropdownlists so that the whole thing is editable.</span></span> <span data-ttu-id="e57db-193">Yalnızca değiştirme `<tr>` gösterildiği gibi öğe:</span><span class="sxs-lookup"><span data-stu-id="e57db-193">Just change the `<tr>` element as shown:</span></span>

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

<span data-ttu-id="e57db-194">Unutmayın `$root` kök ViewModel, olası seçenekler Burada sunulan olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e57db-194">Note that `$root` refers to the root ViewModel, which is where the possible choices are exposed.</span></span> <span data-ttu-id="e57db-195">`$data`-her biri basit bir dizedir resultChoices dizinin tek tek bir öğeye başvuruyor bu durumda, belirli bir bağlam içinde ne olursa olsun geçerli modelin olduğu için ifade eder.</span><span class="sxs-lookup"><span data-stu-id="e57db-195">`$data` refers to whatever the current model is within a given context - in this case it refers to an individual element of the resultChoices array, each of which is a simple string.</span></span>

<span data-ttu-id="e57db-196">Bu değişiklikle, tüm kılavuz düzenlenebilir olur:</span><span class="sxs-lookup"><span data-stu-id="e57db-196">With this change, the entire grid becomes editable:</span></span>

![Düzenlenebilir kılavuz](knockout/_static/editable-grid-screenshot.png)

<span data-ttu-id="e57db-198">Boşaltılan kullanarak doğru Biz bu tümünün jQuery kullanılarak elde, ancak büyük olasılıkla bunu istemezsiniz neredeyse kadar etkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="e57db-198">If we weren't using Knockout, we could achieve all of this using jQuery, but most likely it would not be nearly as efficient.</span></span> <span data-ttu-id="e57db-199">Boşaltılan hangi kullanıcı Arabirimi öğeleri ViewModel öğeleri karşılık ilişkili veri izler ve yalnızca eklenemez, kaldırılamaz veya güncelleştirilmiş gerekiyorsa bu öğeleri güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="e57db-199">Knockout tracks which bound data items in the ViewModel correspond to which UI elements, and only updates those elements that need to be added, removed, or updated.</span></span> <span data-ttu-id="e57db-200">Bu kendisini jQuery veya doğrudan DOM işleme kullanarak elde etmek için önemli çaba götürecek ve biz sonra tablonun verilerine dayalı toplama sonuçları (örneğin, kazanç-kayıp kaydını) görüntülemek istiyorsanız, daha sonra bir kez daha üzerinden döngü ve ayrıştırmak ihtiyacımız HTML öğeleri.</span><span class="sxs-lookup"><span data-stu-id="e57db-200">It would take significant effort to achieve this ourselves using jQuery or direct DOM manipulation, and even then if we then wanted to display aggregate results (such as a win-loss record) based on the table's data, we would need to once more loop through it and parse the HTML elements.</span></span>  <span data-ttu-id="e57db-201">Boşaltılan ile kazanç-kayıp kayıt görüntüleme kısmı oldukça kolaydır.</span><span class="sxs-lookup"><span data-stu-id="e57db-201">With Knockout, displaying the win-loss record is trivial.</span></span> <span data-ttu-id="e57db-202">Biz ViewModel içinde hesaplamalar ve basit bir metin bağlama ile görüntülemek ve bir `<span>`.</span><span class="sxs-lookup"><span data-stu-id="e57db-202">We can perform the calculations within the ViewModel itself, and then display it with a simple text binding and a `<span>`.</span></span>

<span data-ttu-id="e57db-203">Kazanç-Kayıp kayıt dizesi oluşturmak için hesaplanan observable kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e57db-203">To build the win-loss record string, we can use a computed observable.</span></span> <span data-ttu-id="e57db-204">Observable özellikleri ViewModel içinde başvurular Not işlev çağrılarını olmalıdır, aksi halde bunlar observable değerini alacak değil (yani `gameResults()` değil `gameResults` gösterilen kodda):</span><span class="sxs-lookup"><span data-stu-id="e57db-204">Note that references to observable properties within the ViewModel must be function calls, otherwise they will not retrieve the value of the observable (i.e. `gameResults()` not `gameResults` in the code shown):</span></span>

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

<span data-ttu-id="e57db-205">Bu işlev bir aralık içinde bağlamak `<h1>` sayfanın üst kısmındaki öğe:</span><span class="sxs-lookup"><span data-stu-id="e57db-205">Bind this function to a span within the `<h1>` element at the top of the page:</span></span>

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

<span data-ttu-id="e57db-206">Sonuç:</span><span class="sxs-lookup"><span data-stu-id="e57db-206">The result:</span></span>

![Win kaybı](knockout/_static/record-winloss-screenshot.png)

<span data-ttu-id="e57db-208">Satırlar ekleme veya herhangi bir satırın sonuç sütunu seçili öğe değiştirme pencerenin üst kısmında gösterilen kaydı güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="e57db-208">Adding rows or modifying the selected element in any row's Result column will update the record shown at the top of the window.</span></span>

<span data-ttu-id="e57db-209">Değerleri bağlama yanı sıra, neredeyse her yasal JavaScript ifadesi bir bağlama içinde de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e57db-209">In addition to binding to values, you can also use almost any legal JavaScript expression within a binding.</span></span> <span data-ttu-id="e57db-210">Bir kullanıcı Arabirimi öğesi yalnızca bir değer belirli bir eşiği aştığında gibi belirli koşullar altında görünmelidir Örneğin, bu mantıksal olarak bağlama deyimi içinde belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e57db-210">For example, if a UI element should only appear under certain conditions, such as when a value exceeds a certain threshold, you can specify this logically within the binding expression:</span></span>

```html
<div data-bind="visible: customerValue > 100"></div>
```

<span data-ttu-id="e57db-211">Bu `<div>` customerValue 100'den olduğunda yalnızca görünür olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e57db-211">This `<div>` will only be visible when the customerValue is over 100.</span></span>

## <a name="templates"></a><span data-ttu-id="e57db-212">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="e57db-212">Templates</span></span>

<span data-ttu-id="e57db-213">Böylece kolayca UI davranışı içinden ayırmak veya artımlı olarak isteğe bağlı büyük bir uygulamaya kullanıcı Arabirimi öğeleri yük Boşaltılan şablonlar için destek vardır.</span><span class="sxs-lookup"><span data-stu-id="e57db-213">Knockout has support for templates, so that you can easily separate your UI from your behavior, or incrementally load UI elements into a large application on demand.</span></span> <span data-ttu-id="e57db-214">Her satır, yalnızca HTML out bir şablona çekme ve üzerinde veri bağlama çağrısında adıyla şablonu belirterek kendi şablonu yapmak için önceki örneğimizde güncelleştiriyoruz `<tbody>`.</span><span class="sxs-lookup"><span data-stu-id="e57db-214">We can update our previous example to make each row its own template by simply pulling the HTML out into a template and specifying the template by name in the data-bind call on `<tbody>`.</span></span>

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

<span data-ttu-id="e57db-215">Boşaltılan jQuery.tmpl kitaplığı gibi diğer şablon altyapılarını ve Underscore.js'ın şablon altyapısı da destekler.</span><span class="sxs-lookup"><span data-stu-id="e57db-215">Knockout also supports other templating engines, such as the jQuery.tmpl library and Underscore.js's templating engine.</span></span>

## <a name="components"></a><span data-ttu-id="e57db-216">Bileşenler</span><span class="sxs-lookup"><span data-stu-id="e57db-216">Components</span></span>

<span data-ttu-id="e57db-217">Bileşenleri, düzenlemenize ve genellikle UI kodu bağımlı olduğu ViewModel verilerin UI kodunu yeniden olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e57db-217">Components allow you to organize and reuse UI code, usually along with the ViewModel data on which the UI code depends.</span></span> <span data-ttu-id="e57db-218">Bir bileşen oluşturmak için kendi şablon ve onun viewModel belirtin ve bir ad vermek yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="e57db-218">To create a component, you simply need to specify its template and its viewModel, and give it a name.</span></span> <span data-ttu-id="e57db-219">Bu çağırarak yapılır `ko.components.register()`.</span><span class="sxs-lookup"><span data-stu-id="e57db-219">This is done by calling `ko.components.register()`.</span></span> <span data-ttu-id="e57db-220">Şablonlar ve viewmodel satır içi tanımlamanın yanı sıra, bunlar gibi bir kitaplık kullanılarak dış dosyalarından yüklenebilir *require.js*, sonuçta elde edilen çok temiz ve verimli kod.</span><span class="sxs-lookup"><span data-stu-id="e57db-220">In addition to defining the templates and viewmodel inline, they can be loaded from external files using a library like *require.js*, resulting in very clean and efficient code.</span></span>

## <a name="communicating-with-apis"></a><span data-ttu-id="e57db-221">API'leri ile iletişim</span><span class="sxs-lookup"><span data-stu-id="e57db-221">Communicating with APIs</span></span>

<span data-ttu-id="e57db-222">Boşaltılan JSON biçiminde herhangi bir veri ile çalışabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e57db-222">Knockout can work with any data in JSON format.</span></span> <span data-ttu-id="e57db-223">Almak ve çakıştırma kullanarak verileri kaydetmek için genel bir destekleyen jQuery ile yoludur `$.getJSON()` veri almak için işlevi ve `$.post()` tarayıcıdan bir API uç noktasına veri göndermesini yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e57db-223">A common way to retrieve and save data using Knockout is with jQuery, which supports the `$.getJSON()` function to retrieve data, and the `$.post()` method to send data from the browser to an API endpoint.</span></span> <span data-ttu-id="e57db-224">JSON veri göndermek ve almak için farklı bir şekilde tercih ederseniz, doğal olarak, çakıştırma ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="e57db-224">Of course, if you prefer a different way to send and receive JSON data, Knockout will work with it as well.</span></span>

## <a name="summary"></a><span data-ttu-id="e57db-225">Özet</span><span class="sxs-lookup"><span data-stu-id="e57db-225">Summary</span></span>

<span data-ttu-id="e57db-226">Boşaltılan ViewModel içinde tanımlanan istemci uygulamasının geçerli durumu için UI öğeleri bağlamak için basit ve zarif bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="e57db-226">Knockout provides a simple, elegant way to bind UI elements to the current state of the client application, defined in a ViewModel.</span></span> <span data-ttu-id="e57db-227">İşlenecek HTML öğelerine uygulanan data-bind özniteliği Boşaltılan'ın bağlama sözdizimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="e57db-227">Knockout's binding syntax uses the data-bind attribute, applied to HTML elements that are to be processed.</span></span> <span data-ttu-id="e57db-228">Boşaltılan verimli bir şekilde işlemek ve kullanıcı Arabirimi öğeleri izleyerek büyük veri kümelerini güncelleştirme ve etkilenen öğeleri yalnızca değişiklikleri işleme.</span><span class="sxs-lookup"><span data-stu-id="e57db-228">Knockout is able to efficiently render and update large data sets by tracking UI elements and only processing changes to affected elements.</span></span> <span data-ttu-id="e57db-229">Büyük uygulamalar şablonları ve isteğe bağlı harici dosyaları'ndan yüklenebilir bileşenleri kullanan kullanıcı Arabirimi mantığı bölmeniz.</span><span class="sxs-lookup"><span data-stu-id="e57db-229">Large applications can break up UI logic using templates and components, which can be loaded on demand from external files.</span></span> <span data-ttu-id="e57db-230">Şu anda 3, çakıştırma zengin istemci etkileşim gerektiren web uygulamalar geliştirebilir kararlı bir JavaScript kitaplığı sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="e57db-230">Currently version 3, Knockout is a stable JavaScript library that can improve web applications that require rich client interactivity.</span></span>
