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
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a><span data-ttu-id="2b7f8-104">AngularJS ASP.NET Core ile tek sayfa uygulamaları (SPAs) için kullanma</span><span class="sxs-lookup"><span data-stu-id="2b7f8-104">Using AngularJS for Single Page Applications (SPAs) with ASP.NET Core</span></span>


<span data-ttu-id="2b7f8-105">Tarafından [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="2b7f8-105">By [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="2b7f8-106">Bu makalede, AngularJS kullanarak SPA stili ASP.NET uygulamasının nasıl oluşturulacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-106">In this article, you will learn how to build a SPA-style ASP.NET application using AngularJS.</span></span>

<span data-ttu-id="2b7f8-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2b7f8-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-angularjs"></a><span data-ttu-id="2b7f8-108">AngularJS nedir?</span><span class="sxs-lookup"><span data-stu-id="2b7f8-108">What is AngularJS?</span></span>

<span data-ttu-id="2b7f8-109">[AngularJS](https://angularjs.org/) tek sayfa uygulamaları (SPAs) ile çalışmak için yaygın olarak kullanılan Google gelen modern bir JavaScript çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-109">[AngularJS](https://angularjs.org/) is a modern JavaScript framework from Google commonly used to work with Single Page Applications (SPAs).</span></span> <span data-ttu-id="2b7f8-110">AngularJS açık MIT lisansı altında kaynaklıdır ve AngularJS geliştirme sürecini izlenebilir [GitHub deposunu](https://github.com/angular/angular.js).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-110">AngularJS is open sourced under MIT license, and the development progress of AngularJS can be followed on [its GitHub repository](https://github.com/angular/angular.js).</span></span> <span data-ttu-id="2b7f8-111">HTML şeklinde Açısal köşeli kullandığından kitaplığı Angular adı verilir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-111">The library is called Angular because HTML uses angular-shaped brackets.</span></span>

<span data-ttu-id="2b7f8-112">AngularJS bir DOM düzenleme kitaplığı jQuery gibi değil, ancak bir alt kümesini jQLite adlı jQuery kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-112">AngularJS is not a DOM manipulation library like jQuery, but it uses a subset of jQuery called jQLite.</span></span> <span data-ttu-id="2b7f8-113">AngularJS öncelikle, HTML etiket ekleyebilirsiniz bildirim temelli HTML özniteliklerini temel alır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-113">AngularJS is primarily based on declarative HTML attributes that you can add to your HTML tags.</span></span> <span data-ttu-id="2b7f8-114">AngularJS kullanarak tarayıcı deneyebilirsiniz [kod Okul Web sitesi](https://www.codeschool.com/courses/shaping-up-with-angularjs) veya [W3Schools Web sitesi](https://www.w3schools.com/angular/).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-114">You can try AngularJS in your browser using the [Code School website](https://www.codeschool.com/courses/shaping-up-with-angularjs) or  [W3Schools website](https://www.w3schools.com/angular/).</span></span>

<span data-ttu-id="2b7f8-115">Bu makalede, burada Angular başlık çubuğunda bazı notlar ile AngularJS üzerinde odaklanır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-115">This article focuses on AngularJS with some notes on where Angular is heading.</span></span>

## <a name="getting-started"></a><span data-ttu-id="2b7f8-116">Başlarken</span><span class="sxs-lookup"><span data-stu-id="2b7f8-116">Getting started</span></span>

<span data-ttu-id="2b7f8-117">ASP.NET uygulamanızı AngularJS kullanmaya başlamak için projenizin bir parçası olarak yüklemek, veya bir içerik teslim ağı (CDN) başvuru.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-117">To start using AngularJS in your ASP.NET application, you must either install it as part of your project, or reference it from a content delivery network (CDN).</span></span>

### <a name="installation"></a><span data-ttu-id="2b7f8-118">Yükleme</span><span class="sxs-lookup"><span data-stu-id="2b7f8-118">Installation</span></span>

<span data-ttu-id="2b7f8-119">AngularJS uygulamanıza eklemek için çeşitli yollar vardır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-119">There are several ways to add AngularJS to your application.</span></span> <span data-ttu-id="2b7f8-120">Visual Studio'da yeni bir ASP.NET Core web uygulaması başlatıyorsanız, yerleşik kullanarak AngularJS ekleyebilirsiniz [Bower](bower.md) destekler.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-120">If you’re starting a new ASP.NET Core web application in Visual Studio, you can add AngularJS using the built-in [Bower](bower.md) support.</span></span> <span data-ttu-id="2b7f8-121">Açık *bower.json*ve bir girdiyi `dependencies` özelliği:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-121">Open *bower.json*, and add an entry to the `dependencies` property:</span></span>

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

<span data-ttu-id="2b7f8-122">Kaydetme sırasında *bower.json* dosyası, Angular projenizin içinde yüklenecek *wwwroot/lib* klasör.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-122">Upon saving the *bower.json* file, Angular will be installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="2b7f8-123">Ayrıca, içinde listelenecektir `Dependencies/Bower` klasör.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-123">Additionally, it will be listed within the `Dependencies/Bower` folder.</span></span> <span data-ttu-id="2b7f8-124">Aşağıdaki ekran görüntüsüne bakın.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-124">See the screenshot below.</span></span>

![AngularJS proje ile Çözüm Gezgini](angular/_static/angular-solution-explorer.png)

<span data-ttu-id="2b7f8-126">Ardından, eklemek bir `<script>` altına başvuru `<body>` , HTML sayfasının bölümünde veya *_Layout.cshtml* aşağıda gösterildiği gibi dosya:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-126">Next, add a `<script>` reference to the bottom of the `<body>` section of your HTML page or *_Layout.cshtml* file, as shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

<span data-ttu-id="2b7f8-127">Üretim uygulamaları AngularJS gibi ortak kitaplıkları CDN'ler kullanan önerilir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-127">It's recommended that production applications utilize CDNs for common libraries like AngularJS.</span></span> <span data-ttu-id="2b7f8-128">Bunun gibi birkaç CDN'ler birinden AngularJS başvurabilir:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-128">You can reference AngularJS from one of several CDNs, such as this one:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

<span data-ttu-id="2b7f8-129">Bir başvuru olduktan sonra *angular.js* komut dosyası, web sayfalarınıza AngularJS kullanmaya başlamak hazır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-129">Once you have a reference to the *angular.js* script file, you're ready to begin using AngularJS in your web pages.</span></span>

## <a name="key-components"></a><span data-ttu-id="2b7f8-130">Anahtar bileşenleri</span><span class="sxs-lookup"><span data-stu-id="2b7f8-130">Key components</span></span>

<span data-ttu-id="2b7f8-131">AngularJS içeren bir dizi ana bileşen gibi *yönergeleri*, *şablonları*, *yineleyiciler*, *modülleri*,  *denetleyicileri*, *bileşenleri*, *bileşen yönlendirici* ve daha fazlası.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-131">AngularJS includes a number of major components, such as *directives*, *templates*, *repeaters*, *modules*, *controllers*, *components*, *component router* and more.</span></span> <span data-ttu-id="2b7f8-132">Bu bileşenlerin birlikte nasıl davranış web sayfalarınıza eklemek için çalışır inceleyelim.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-132">Let's examine how these components work together to add behavior to your web pages.</span></span>

### <a name="directives"></a><span data-ttu-id="2b7f8-133">Yönergeler</span><span class="sxs-lookup"><span data-stu-id="2b7f8-133">Directives</span></span>

<span data-ttu-id="2b7f8-134">AngularJS kullanan [yönergeleri](https://docs.angularjs.org/guide/directive) HTML özel öznitelikler ve öğeler ile genişletmek için.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-134">AngularJS uses [directives](https://docs.angularjs.org/guide/directive) to extend HTML with custom attributes and elements.</span></span> <span data-ttu-id="2b7f8-135">AngularJS yönergesi aracılığıyla tanımlanır `data-ng-*` veya `ng-*` önekleri (`ng` kısaltması olan Açısal).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-135">AngularJS directives are defined via `data-ng-*` or `ng-*` prefixes (`ng` is short for angular).</span></span> <span data-ttu-id="2b7f8-136">AngularJS yönergesi iki tür vardır:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-136">There are two types of AngularJS directives:</span></span>

   1. <span data-ttu-id="2b7f8-137">**İlkel yönergeleri**: Bu Açısal ekibi tarafından önceden tanımlanmış ve AngularJS framework'ün parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-137">**Primitive Directives**: These are predefined by the Angular team and are part of the AngularJS framework.</span></span>

   2. <span data-ttu-id="2b7f8-138">**Özel yönergeleri**: tanımlayabileceğiniz özel yönergeleri bunlar.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-138">**Custom Directives**: These are custom directives that you can define.</span></span>

<span data-ttu-id="2b7f8-139">Tüm AngularJS uygulamalarında kullanılan ilkel yönergeleri biri `ng-app` AngularJS uygulama bootstraps yönergesi.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-139">One of the primitive directives used in all AngularJS applications is the `ng-app` directive, which bootstraps the AngularJS application.</span></span> <span data-ttu-id="2b7f8-140">Bu yönerge uygulanabilir `<body>` etiketi veya bir alt öğesi gövde.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-140">This directive can be applied to the `<body>` tag or to a child element of the body.</span></span> <span data-ttu-id="2b7f8-141">Uygulamada bir örnek görelim.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-141">Let's see an example in action.</span></span> <span data-ttu-id="2b7f8-142">ASP.NET projesinde olduğunuz varsayılarak, bir HTML dosyasına ya da ekleyebilirsiniz `wwwroot` klasörü, yeni bir denetleyici eylemi ve ilişkili bir görünüm ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-142">Assuming you're in an ASP.NET project, you can either add an HTML file to the `wwwroot` folder, or add a new controller action and an associated view.</span></span> <span data-ttu-id="2b7f8-143">Bu durumda, yeni bir ekledim `Directives` eylem yöntemine `HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-143">In this case, I've added a new `Directives` action method to `HomeController.cs`.</span></span> <span data-ttu-id="2b7f8-144">İlişkili görünüm burada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-144">The associated view is shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

<span data-ttu-id="2b7f8-145">Bu örnekler birbirlerinden bağımsız tutmak için paylaşılan düzeni dosyasını kullanıyorum değil.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-145">To keep these samples independent of one another, I'm not using the shared layout file.</span></span> <span data-ttu-id="2b7f8-146">Biz gövde etiketi ile donatılmış görebilirsiniz `ng-app` bu sayfayı göstermek için yönergesi olup bir AngularJS uygulama.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-146">You can see that we decorated the body tag with the `ng-app` directive to indicate this page is an AngularJS application.</span></span> <span data-ttu-id="2b7f8-147">`{{2+2}}` Daha birazdan öğreneceksiniz bir Açısal veri bağlama ifadesidir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-147">The `{{2+2}}` is an Angular data binding expression that you will learn more about in a moment.</span></span> <span data-ttu-id="2b7f8-148">Bu uygulama çalıştırırsanız, sonuç şöyledir:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-148">Here is the result if you run this application:</span></span>

![Basit Açısal yönergesi](angular/_static/simple-directive.png)

<span data-ttu-id="2b7f8-150">AngularJS başka ilkel yönergeleri içerir:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-150">Other primitive directives in AngularJS include:</span></span>

<span data-ttu-id="2b7f8-151">`ng-controller`Hangi JavaScript denetleyicisi hangi görünüme bağlı belirler.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-151">`ng-controller` Determines which JavaScript controller is bound to which view.</span></span>

<span data-ttu-id="2b7f8-152">`ng-model`Bir HTML öğenin özelliklerinin değerlerini bağlı olan model belirler.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-152">`ng-model` Determines the model to which the values of an HTML element's properties are bound.</span></span>

<span data-ttu-id="2b7f8-153">`ng-init`Geçerli kapsam için bir ifade biçiminde uygulama verilerini başlatmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-153">`ng-init` Used to initialize the application data in the form of an expression for the current scope.</span></span>

<span data-ttu-id="2b7f8-154">`ng-if`Kaldırır veya sağlanan ifade truthiness üzerinde temel DOM verilen HTML öğesini yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-154">`ng-if` Removes or recreates the given HTML element in the DOM based on the truthiness of the expression provided.</span></span>

<span data-ttu-id="2b7f8-155">`ng-repeat`Belirtilen bir HTML bloğunu bir veri kümesi üzerinde tekrarlar.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-155">`ng-repeat` Repeats a given block of HTML over a set of data.</span></span>

<span data-ttu-id="2b7f8-156">`ng-show`Gösterir veya gizler sağlanan ifadesi temelinde verilen HTML öğesi.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-156">`ng-show` Shows or hides the given HTML element based on the expression provided.</span></span>

<span data-ttu-id="2b7f8-157">AngularJS desteklenen tüm ilkel yönergeleri tam bir listesi için lütfen bakın [AngularJS belgeleri Web sitesinde yönerge bir belge bölümü](https://docs.angularjs.org/api/ng/directive).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-157">For a full list of all primitive directives supported in AngularJS, please refer to the [directive documentation section on the AngularJS documentation website](https://docs.angularjs.org/api/ng/directive).</span></span>

### <a name="data-binding"></a><span data-ttu-id="2b7f8-158">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="2b7f8-158">Data binding</span></span>

<span data-ttu-id="2b7f8-159">AngularJS sağlar [veri bağlama](https://docs.angularjs.org/guide/databinding) out-of--kullanarak box Destek `ng-bind` yönergesi veya bir veri ifade sözdizimini gibi bağlama `{{expression}}`.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-159">AngularJS provides [data binding](https://docs.angularjs.org/guide/databinding) support out-of-the-box using either the `ng-bind` directive or a data binding expression syntax such as `{{expression}}`.</span></span> <span data-ttu-id="2b7f8-160">AngularJS bir görünüm şablonu ile eşitleme sırasında her zaman bir modelden veri nerede tutulur iki yönlü veri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-160">AngularJS supports two-way data binding where data from a model is kept in synchronization with a view template at all times.</span></span> <span data-ttu-id="2b7f8-161">Görünüm değişiklikleri otomatik olarak modeldeki yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-161">Any changes to the view are automatically reflected in the model.</span></span> <span data-ttu-id="2b7f8-162">Benzer şekilde, modeldeki değişiklikleri görünümünde yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-162">Likewise, any changes in the model are reflected in the view.</span></span>

<span data-ttu-id="2b7f8-163">Bir HTML dosyası veya bir denetleyici eylemi adlı bir eşlik eden görünümüyle oluşturma `Databinding`.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-163">Create either an HTML file or a controller action with an accompanying view named `Databinding`.</span></span> <span data-ttu-id="2b7f8-164">Görünümde şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-164">Include the following in the view:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

<span data-ttu-id="2b7f8-165">Bildirim yönergeleri veya veri bağlamayı kullanarak modeli değerlerini görüntüleyebilirsiniz (`ng-bind`).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-165">Notice that you can display model values using either directives or data binding (`ng-bind`).</span></span> <span data-ttu-id="2b7f8-166">Sonuçta elde edilen sayfa, aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-166">The resulting page should look like this:</span></span>

![Basit veri bağlama](angular/_static/simple-databinding.png)

### <a name="templates"></a><span data-ttu-id="2b7f8-168">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="2b7f8-168">Templates</span></span>

<span data-ttu-id="2b7f8-169">[Şablonları](https://docs.angularjs.org/guide/templates) AngularJS içinde yalnızca düz HTML sayfaları AngularJS yönergeleri ve yapıları ile donatılmış şunlardır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-169">[Templates](https://docs.angularjs.org/guide/templates) in AngularJS are just plain HTML pages decorated with AngularJS directives and artifacts.</span></span> <span data-ttu-id="2b7f8-170">AngularJS şablonunda yönergeleri, ifadeler, filtreleri ve form görünümü HTML ile birleştirmenin denetimleri bileşimi ' dir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-170">A template in AngularJS is a mixture of directives, expressions, filters, and controls that combine with HTML to form the view.</span></span>

<span data-ttu-id="2b7f8-171">Şablonları göstermek için başka bir görünüm ekleyin ve aşağıdaki şekilde ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-171">Add another view to demonstrate templates, and add the following to it:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

<span data-ttu-id="2b7f8-172">AngularJS yönergesi gibi şablonu sahip `ng-app`, `ng-init`, `ng-model` ve bağlamak için veri bağlama ifadesi sözdizimi `personName` özelliği.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-172">The template has AngularJS directives like `ng-app`, `ng-init`, `ng-model` and data binding expression syntax to bind the `personName` property.</span></span> <span data-ttu-id="2b7f8-173">Tarayıcıda çalışan, görünüm aşağıdaki ekran görüntüsüne görünür:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-173">Running in the browser, the view looks like the screenshot below:</span></span>

![Basit şablonları örnek 1](angular/_static/simple-templates-1.png)

<span data-ttu-id="2b7f8-175">Giriş alanında yazarak adını değiştirirseniz, metnin yanında giriş alanını dinamik olarak görürsünüz eylemde Açısal iki yönlü veri bağlamayı gösteren güncelleştirme.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-175">If you change the name by typing in the input field, you will see the text next to the input field dynamically update, showing Angular two-way data binding in action.</span></span>

![Basit şablonları örnek 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a><span data-ttu-id="2b7f8-177">İfadeler</span><span class="sxs-lookup"><span data-stu-id="2b7f8-177">Expressions</span></span>

<span data-ttu-id="2b7f8-178">[İfadeleri](https://docs.angularjs.org/guide/expression) içinde yazılmış JavaScript benzeri kod parçacıkları AngularJS içinde olan `{{ expression }}` sözdizimi.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-178">[Expressions](https://docs.angularjs.org/guide/expression) in AngularJS are JavaScript-like code snippets that are written inside the `{{ expression }}` syntax.</span></span> <span data-ttu-id="2b7f8-179">Bu deyimler verilerden HTML aynı şekilde bağlanır `ng-bind` yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-179">The data from these expressions is bound to HTML the same way as `ng-bind` directives.</span></span> <span data-ttu-id="2b7f8-180">AngularJS ifadeleri ve normal JavaScript ifadeler arasındaki temel fark ifadeleri karşı değerlendirilir bu AngularJS olan `$scope` AngularJS nesne.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-180">The main difference between AngularJS expressions and regular JavaScript expressions is that AngularJS expressions are evaluated against the `$scope` object in AngularJS.</span></span>

<span data-ttu-id="2b7f8-181">Bağ Aşağıda örnek AngularJS ifadelerinde `personName` ve basit bir JavaScript ifadesi hesaplanır:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-181">The AngularJS expressions in the sample below bind `personName` and a simple JavaScript calculated expression:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

<span data-ttu-id="2b7f8-182">Tarayıcı görüntüler çalışan örnek `personName` veri ve hesaplama sonuçları:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-182">The example running in the browser displays the `personName` data and the results of the calculation:</span></span>

![Basit ifadeler](angular/_static/simple-expressions.png)

### <a name="repeaters"></a><span data-ttu-id="2b7f8-184">Yineleyiciler</span><span class="sxs-lookup"><span data-stu-id="2b7f8-184">Repeaters</span></span>

<span data-ttu-id="2b7f8-185">AngularJS içinde yinelenen adlı bir ilkel yönergesi yapılır `ng-repeat`.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-185">Repeating in AngularJS is done via a primitive directive called `ng-repeat`.</span></span> <span data-ttu-id="2b7f8-186">`ng-repeat` Yönergesi üzerinde yinelenen bir veri dizisi uzunluğu belirtilen bir HTML öğesi bir görünümde tekrarlar.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-186">The `ng-repeat` directive repeats a given HTML element in a view over the length of a repeating data array.</span></span> <span data-ttu-id="2b7f8-187">AngularJS yineleyiciler dizeleri veya nesneler dizisi yineleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-187">Repeaters in AngularJS can repeat over an array of strings or objects.</span></span> <span data-ttu-id="2b7f8-188">Dize dizisi üzerinde yinelenen bir örnek kullanımı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-188">Here is a sample usage of repeating over an array of strings:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

<span data-ttu-id="2b7f8-189">[Repeat yönergesi](https://docs.angularjs.org/api/ng/directive/ngRepeat) bu ekran görüntüsünde gösterilen Geliştirici Araçları'nda gördüğünüz gibi bir dizi sırasız bir listesini, liste öğelerini çıkarır:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-189">The [repeat directive](https://docs.angularjs.org/api/ng/directive/ngRepeat) outputs a series of list items in an unordered list, as you can see in the developer tools shown in this screenshot:</span></span>

![Yineleyici örneği](angular/_static/repeater.png)

<span data-ttu-id="2b7f8-191">Burada, nesnelerinin bir dizisi üzerinde yinelenen bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-191">Here is an example that repeats over an array of objects.</span></span> <span data-ttu-id="2b7f8-192">`ng-init` Yönergesi kurar bir `names` burada her öğe, ilk içeren bir nesne ve son adları dizisi.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-192">The `ng-init` directive establishes a `names` array, where each element is an object containing first and last names.</span></span> <span data-ttu-id="2b7f8-193">`ng-repeat` Atama, `name in names`, her dizi öğesi için bir liste öğesi çıkarır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-193">The `ng-repeat` assignment, `name in names`, outputs a list item for every array element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

<span data-ttu-id="2b7f8-194">Çıktı, bu durumda önceki örnektekiyle aynı şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-194">The output in this case is the same as in the previous example.</span></span>

<span data-ttu-id="2b7f8-195">Açısal döngü, yürütmeyi olduğu dayalı davranışı sağlamak bazı ek yönergeleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-195">Angular provides some additional directives that can help provide behavior based on where the loop is in its execution.</span></span>

`$index`

<span data-ttu-id="2b7f8-196">Kullanım `$index` içinde `ng-repeat` hangi dizin döngü şu anda konumunu belirlemek için döngü açıktır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-196">Use `$index` in the `ng-repeat` loop to determine which index position your loop currently is on.</span></span>

<span data-ttu-id="2b7f8-197">`$even`ve`$odd`</span><span class="sxs-lookup"><span data-stu-id="2b7f8-197">`$even` and `$odd`</span></span>

<span data-ttu-id="2b7f8-198">Kullanım `$even` içinde `ng-repeat` döngü geçerli dizinde bile dizinlenmiş bir satır olup olmadığını belirlemek için döngü.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-198">Use `$even` in the `ng-repeat` loop to determine whether the current index in your loop is an even indexed row.</span></span> <span data-ttu-id="2b7f8-199">Benzer şekilde, kullanın `$odd` geçerli dizini tek bir dizini oluşturulmuş satır olup olmadığını belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-199">Similarly, use `$odd` to determine if the current index is an odd indexed row.</span></span>

<span data-ttu-id="2b7f8-200">`$first`ve`$last`</span><span class="sxs-lookup"><span data-stu-id="2b7f8-200">`$first` and `$last`</span></span>

<span data-ttu-id="2b7f8-201">Kullanım `$first` içinde `ng-repeat` geçerli dizin, Döngüdeki ilk satırın olup olmadığını belirlemek için döngü.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-201">Use `$first` in the `ng-repeat` loop to determine whether the current index in your loop is the first row.</span></span> <span data-ttu-id="2b7f8-202">Benzer şekilde, kullanın `$last` geçerli dizini son satırını olup olmadığını belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-202">Similarly, use `$last` to determine if the current index is the last row.</span></span>

<span data-ttu-id="2b7f8-203">Aşağıda gösteren bir örnektir `$index`, `$even`, `$odd`, `$first`, ve `$last` eylem:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-203">Below is a sample that shows `$index`, `$even`, `$odd`, `$first`, and `$last` in action:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

<span data-ttu-id="2b7f8-204">Sonuçta çıktı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-204">Here is the resulting output:</span></span>

![Yineleyici örnek 2](angular/_static/repeaters2.png)

### <a name="scope"></a><span data-ttu-id="2b7f8-206">$scope</span><span class="sxs-lookup"><span data-stu-id="2b7f8-206">$scope</span></span>

<span data-ttu-id="2b7f8-207">`$scope`Görünüm (şablonu) ve (aşağıda açıklanmıştır) denetleyicisi arasındaki Birleştirici görevi gören bir JavaScript nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-207">`$scope` is a JavaScript object that acts as glue between the view (template) and the controller (explained below).</span></span> <span data-ttu-id="2b7f8-208">AngularJS görünüm şablonunda bağlı değerleri yalnızca bildiği `$scope` denetleyicisi nesnesinde.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-208">A view template in AngularJS only knows about the values attached to the `$scope` object in the controller.</span></span>

> [!NOTE]
> <span data-ttu-id="2b7f8-209">MVVM dünyada `$scope` AngularJS nesnesinde genellikle ViewModel tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-209">In the MVVM world, the `$scope` object in AngularJS is often defined as the ViewModel.</span></span> <span data-ttu-id="2b7f8-210">AngularJS takım başvurduğu `$scope` nesnesi olarak veri modeli.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-210">The AngularJS team refers to the `$scope` object as the Data-Model.</span></span> <span data-ttu-id="2b7f8-211">[AngularJS kapsamlarda hakkında daha fazla bilgi](https://docs.angularjs.org/guide/scope).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-211">[Learn more about Scopes in AngularJS](https://docs.angularjs.org/guide/scope).</span></span>

<span data-ttu-id="2b7f8-212">Özelliklerini ayarlamak nasıl gösteren basit bir örnek aşağıdadır `$scope` ayrı bir JavaScript dosyası içinde *scope.js*:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-212">Below is a simple example showing how to set properties on `$scope` within a separate JavaScript file, *scope.js*:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

<span data-ttu-id="2b7f8-213">Gözlemlemek `$scope` parametresi satır 2 denetleyicisine geçirildi.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-213">Observe the `$scope` parameter passed to the controller on line 2.</span></span> <span data-ttu-id="2b7f8-214">Bu nesne, görünüm bildiği ' dir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-214">This object is what the view knows about.</span></span> <span data-ttu-id="2b7f8-215">3. satırda biz "Mary Jane" için "name" adlı bir özellik ayarlıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-215">On line 3, we are setting a property called "name" to "Mary Jane".</span></span>

<span data-ttu-id="2b7f8-216">Görünüm tarafından belirli bir özellik bulunamadı ne olur?</span><span class="sxs-lookup"><span data-stu-id="2b7f8-216">What happens when a particular property is not found by the view?</span></span> <span data-ttu-id="2b7f8-217">Aşağıda tanımlanan görünüm "name" ve "Yaş" özellikleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-217">The view defined below refers to "name" and "age" properties:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

<span data-ttu-id="2b7f8-218">Biz ifade sözdizimini kullanarak "name" özelliği görüntülemek için Angular soran 9 satırda dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-218">Notice on line 9 that we are asking Angular to show the "name" property using expression syntax.</span></span> <span data-ttu-id="2b7f8-219">Satır 10 sonra "geçerlilik süresi", var olmayan bir özelliğe başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-219">Line 10 then refers to "age", a property that does not exist.</span></span> <span data-ttu-id="2b7f8-220">Çalışan bir örnek "Mary Jane" ve hiçbir şey yaşı için ayarlanan adı gösterir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-220">The running example shows the name set to "Mary Jane" and nothing for age.</span></span> <span data-ttu-id="2b7f8-221">Eksik özelliklerini göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-221">Missing properties are ignored.</span></span>

![Kapsam örneği](angular/_static/scope.png)

### <a name="modules"></a><span data-ttu-id="2b7f8-223">Modüller</span><span class="sxs-lookup"><span data-stu-id="2b7f8-223">Modules</span></span>

<span data-ttu-id="2b7f8-224">A [Modülü](https://docs.angularjs.org/guide/module) içinde AngularJS denetleyicisi, hizmetler, yönergeleri, vb. bir koleksiyonudur. `angular.module()` İşlev çağrısı oluşturmak, kaydetme ve AngularJS modüllerde almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-224">A [module](https://docs.angularjs.org/guide/module) in AngularJS is a collection of controllers, services, directives, etc. The `angular.module()` function call is used to create, register, and retrieve modules in AngularJS.</span></span> <span data-ttu-id="2b7f8-225">AngularJS takım ve üçüncü taraf kitaplıkların tarafından sevk dahil olmak üzere tüm modülleri kullanarak kaydedilmelidir `angular.module()` işlevi.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-225">All modules, including those shipped by the AngularJS team and third party libraries, should be registered using the `angular.module()` function.</span></span>

<span data-ttu-id="2b7f8-226">Bir AngularJS içinde yeni bir modül oluşturmayı gösteren kod parçacığını aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-226">Below is a snippet of code that shows how to create a new module in AngularJS.</span></span> <span data-ttu-id="2b7f8-227">İlk parametre modülü adıdır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-227">The first parameter is the name of the module.</span></span> <span data-ttu-id="2b7f8-228">İkinci parametre bağımlılıkları diğer modülleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-228">The second parameter defines dependencies on other modules.</span></span> <span data-ttu-id="2b7f8-229">Bu makalenin sonraki bölümlerinde biz Bu bağımlılıklar geçirmek nasıl gösterilmesi bir `angular.module()` yöntem çağrısı.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-229">Later in this article, we will be showing how to pass these dependencies to an `angular.module()` method call.</span></span>

```javascript
var personApp = angular.module('personApp', []);
```

<span data-ttu-id="2b7f8-230">Kullanım `ng-app` yönergesi sayfasında AngularJS modülü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-230">Use the `ng-app` directive to represent an AngularJS module on the page.</span></span> <span data-ttu-id="2b7f8-231">Bir modülü kullanmak, modül adı atamak için `personApp` Bu örnekte, için `ng-app` bizim şablonundaki yönerge.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-231">To use a module, assign the name of the module, `personApp` in this example, to the `ng-app` directive in our template.</span></span>

```html
<body ng-app="personApp">
```

### <a name="controllers"></a><span data-ttu-id="2b7f8-232">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="2b7f8-232">Controllers</span></span>

<span data-ttu-id="2b7f8-233">[Denetleyicileri](https://docs.angularjs.org/guide/controller) içinde AngularJS kodunuz için giriş ilk noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-233">[Controllers](https://docs.angularjs.org/guide/controller) in AngularJS are the first point of entry for your code.</span></span> <span data-ttu-id="2b7f8-234">`<module name>.controller()` İşlev çağrısı oluşturmak ve denetleyicileri içinde AngularJS kaydetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-234">The `<module name>.controller()` function call is used to create and register controllers in AngularJS.</span></span> <span data-ttu-id="2b7f8-235">`ng-controller` Yönergesi HTML sayfasında bir AngularJS denetleyicisi göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-235">The `ng-controller` directive is used to represent an AngularJS controller on the HTML page.</span></span> <span data-ttu-id="2b7f8-236">Angular denetleyicisi rolünü durumunu ve veri modelinin davranışını ayarlamaktır (`$scope`).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-236">The role of the controller in Angular is to set state and behavior of the data model (`$scope`).</span></span> <span data-ttu-id="2b7f8-237">Denetleyicileri DOM doğrudan işlemek için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-237">Controllers should not be used to manipulate the DOM directly.</span></span>

<span data-ttu-id="2b7f8-238">Yeni bir denetleyicisi kaydeder kod parçacığını aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-238">Below is a snippet of code that registers a new controller.</span></span> <span data-ttu-id="2b7f8-239">`personApp` Parçacığında bulunan değişkenine başvuruyor 2 satırında tanımlanan bir Açısal modülü.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-239">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

<span data-ttu-id="2b7f8-240">Görünümü kullanarak `ng-controller` yönergesi Denetleyici adı atar:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-240">The view using the `ng-controller` directive assigns the controller name:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

<span data-ttu-id="2b7f8-241">"Mary" ve "karşılık Jane" sayfası gösterir `firstName` ve `lastName` ekli özellikler `$scope` nesnesi:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-241">The page shows "Mary" and "Jane" that correspond to the `firstName` and `lastName` properties attached to the `$scope` object:</span></span>

![Denetleyici örneği](angular/_static/controllers.png)

### <a name="components"></a><span data-ttu-id="2b7f8-243">Bileşenler</span><span class="sxs-lookup"><span data-stu-id="2b7f8-243">Components</span></span>

<span data-ttu-id="2b7f8-244">[Bileşenleri](https://docs.angularjs.org/guide/component) içinde Angular 1.5.x izin kapsülleme ve tek tek HTML öğeleri oluşturma yeteneği.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-244">[Components](https://docs.angularjs.org/guide/component) in Angular 1.5.x allow for the encapsulation and capability of creating individual HTML elements.</span></span> <span data-ttu-id="2b7f8-245">İçinde Açısal 1.4.x .directive() yöntemini kullanarak aynı özellik elde.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-245">In Angular 1.4.x you could achieve the same feature using the .directive() method.</span></span>

<span data-ttu-id="2b7f8-246">.Component() yöntemi kullanarak, geliştirme yönergesi ve denetleyici işlevselliğini sağlamasını basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-246">By using the .component() method, development is simplified gaining the functionality of the directive and the controller.</span></span> <span data-ttu-id="2b7f8-247">Diğer avantajlar şunlardır; Kapsam yalıtımı, en iyi yöntemler devralınmış ve geçiş Açısal 2 daha kolay bir görev haline gelir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-247">Other benefits include; scope isolation, best practices are inherent, and migration to Angular 2 becomes an easier task.</span></span> <span data-ttu-id="2b7f8-248">`<module name>.component()` İşlev çağrısı oluşturmak ve bileşenleri AngularJS kaydetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-248">The `<module name>.component()` function call is used to create and register components in AngularJS.</span></span>

<span data-ttu-id="2b7f8-249">Yeni bir bileşen kaydeder kod parçacığını aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-249">Below is a snippet of code that registers a new component.</span></span> <span data-ttu-id="2b7f8-250">`personApp` Parçacığında bulunan değişkenine başvuruyor 2 satırında tanımlanan bir Açısal modülü.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-250">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

<span data-ttu-id="2b7f8-251">Burada size özel HTML öğesi görüntülemekte olduğunuz görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-251">The view where we are displaying the custom HTML element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

<span data-ttu-id="2b7f8-252">Bileşeni tarafından kullanılan ilişkili şablonu:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-252">The associated template used by component:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

<span data-ttu-id="2b7f8-253">"Aftab" ve "karşılık Ansari" sayfası gösterir `firstName` ve `lastName` ekli özellikler `vm` nesnesi:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-253">The page shows "Aftab" and "Ansari" that correspond to the `firstName` and `lastName` properties attached to the `vm` object:</span></span>

![Bileşenleri örnek](angular/_static/components.png)

### <a name="services"></a><span data-ttu-id="2b7f8-255">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="2b7f8-255">Services</span></span>

<span data-ttu-id="2b7f8-256">[Hizmetleri](https://docs.angularjs.org/guide/services) içinde AngularJS Açısal uygulama ömrü kullanılabilecek bir dosyaya koyma soyutlanır paylaşılan kod için yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-256">[Services](https://docs.angularjs.org/guide/services) in AngularJS are commonly used for shared code that is abstracted away into a file which can be used throughout the lifetime of an Angular application.</span></span> <span data-ttu-id="2b7f8-257">Hizmetleri gevşek örneği olmayacaktır, bir hizmet örneği hizmete bağlı bir bileşen kullanılmadıkça anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-257">Services are lazily instantiated, meaning that there will not be an instance of a service unless a component that depends on the service gets used.</span></span> <span data-ttu-id="2b7f8-258">Oluşturucuları AngularJS uygulamalarda kullanılan bir hizmet bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-258">Factories are an example of a service used in AngularJS applications.</span></span> <span data-ttu-id="2b7f8-259">Oluşturucuları kullanarak oluşturulur `myApp.factory()` işlev çağrısı, burada `myApp` modülüdür.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-259">Factories are created using the `myApp.factory()` function call, where `myApp` is the module.</span></span>

<span data-ttu-id="2b7f8-260">AngularJS factory'leri kullanmayı gösteren bir örnek aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-260">Below is an example that shows how to use factories in AngularJS:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

<span data-ttu-id="2b7f8-261">Bu fabrikada denetleyicisinden çağırmak için geçirmek `personFactory` bir parametre olarak `controller` işlevi:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-261">To call this factory from the controller, pass `personFactory` as a parameter to the `controller` function:</span></span>

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a><span data-ttu-id="2b7f8-262">Bir REST uç noktası için iletişim kurabilecek şekilde Hizmetleri kullanma</span><span class="sxs-lookup"><span data-stu-id="2b7f8-262">Using services to talk to a REST endpoint</span></span>

<span data-ttu-id="2b7f8-263">Aşağıda bir ASP.NET çekirdek Web API uç noktası ile etkileşim kurmak için uçtan uca örnek Hizmetleri içinde AngularJS kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-263">Below is an end-to-end example using services in AngularJS to interact with an ASP.NET Core Web API endpoint.</span></span> <span data-ttu-id="2b7f8-264">Örnek Web API öğesinden verileri alır ve bir görünüm şablonunda verileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-264">The example gets data from the Web API and displays the data in a view template.</span></span> <span data-ttu-id="2b7f8-265">İlk görünümüyle başlayalım:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-265">Let's start with the view first:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

<span data-ttu-id="2b7f8-266">Bu görünümde sahibiz adlı bir Açısal Modülü `PersonsApp` bir denetleyici adı verilen ve `personController`.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-266">In this view, we have an Angular module called `PersonsApp` and a controller called `personController`.</span></span> <span data-ttu-id="2b7f8-267">Kullanıyoruz `ng-repeat` kişilerin listesi yineleme.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-267">We are using `ng-repeat` to iterate over the list of persons.</span></span> <span data-ttu-id="2b7f8-268">Özel JavaScript dosyaları üç 17-19 satırlarındaki başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-268">We are referencing three custom JavaScript files on lines 17-19.</span></span>

<span data-ttu-id="2b7f8-269">*PersonApp.js* dosyasını kaydetmek için kullanılan `PersonsApp` modülü; ve sözdizimi önceki örneklere benzer.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-269">The *personApp.js* file is used to register the `PersonsApp` module; and, the syntax is similar to previous examples.</span></span> <span data-ttu-id="2b7f8-270">Kullanıyoruz `angular.module` biz ile çalışacaksınız modülü yeni bir örneğini oluşturmak için işlevi.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-270">We are using the `angular.module` function to create a new instance of the module that we will be working with.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

<span data-ttu-id="2b7f8-271">Bir göz atalım *personFactory.js*, aşağıdaki.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-271">Let's take a look at *personFactory.js*, below.</span></span> <span data-ttu-id="2b7f8-272">Modülün aramakta olduğunuz `factory` yöntemi bir fabrikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-272">We are calling the module’s `factory` method to create a factory.</span></span> <span data-ttu-id="2b7f8-273">Satır 12 gösterir yerleşik Angular `$http` hizmeti bir web hizmetinden kişi bilgileri alınıyor.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-273">Line 12 shows the built-in Angular `$http` service retrieving people information from a web service.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

<span data-ttu-id="2b7f8-274">İçinde *personController.js*, modülün aramakta olduğunuz `controller` denetleyicisi oluşturmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-274">In *personController.js*, we are calling the module’s `controller` method to create the controller.</span></span> <span data-ttu-id="2b7f8-275">`$scope` Nesnenin `people` özelliği personFactory (satır 13) döndürülen verileri atanır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-275">The `$scope` object's `people` property is assigned the data returned from the personFactory (line 13).</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

<span data-ttu-id="2b7f8-276">Web API ve bunun arkasındaki modeli hızlı bir göz atalım.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-276">Let's take a quick look at the Web API and the model behind it.</span></span> <span data-ttu-id="2b7f8-277">`Person` Modeldir POCO (düz eski CLR nesnesi) ile `Id`, `FirstName`, ve `LastName` özellikleri:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-277">The `Person` model is a POCO (Plain Old CLR Object) with `Id`, `FirstName`, and `LastName` properties:</span></span>

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

<span data-ttu-id="2b7f8-278">`Person` Denetleyicisi JSON biçimli bir listesini döndürür `Person` nesneler:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-278">The `Person` controller returns a JSON-formatted list of `Person` objects:</span></span>

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

<span data-ttu-id="2b7f8-279">Uygulamayı eylem görelim:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-279">Let's see the application in action:</span></span>

![Denetleyici görüntüleme REST sonuçları](angular/_static/rest-bound.png)

<span data-ttu-id="2b7f8-281">Yapabilecekleriniz [Github'da uygulamanın yapısını görünümünde](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-281">You can [view the application's structure on GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="2b7f8-282">AngularJS uygulamaları yapılandırılması hakkında daha fazla bilgi için bkz: [John Papa'nın Açısal Stil Kılavuzu](https://github.com/johnpapa/angular-styleguide)</span><span class="sxs-lookup"><span data-stu-id="2b7f8-282">For more on structuring AngularJS applications, see [John Papa's Angular Style Guide](https://github.com/johnpapa/angular-styleguide)</span></span>

&nbsp;

> [!NOTE]
> <span data-ttu-id="2b7f8-283">AngularJS modülü, denetleyici, Fabrika oluşturmak için yönergesi ve görünüm dosyaları kolayca Sayed Hashimi 's denetlediğinizden emin [SideWaffle şablon paketi Visual Studio için](http://sidewaffle.com/).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-283">To create AngularJS module, controller, factory, directive and view files easily, be sure to check out Sayed Hashimi's [SideWaffle template pack for Visual Studio](http://sidewaffle.com/).</span></span> <span data-ttu-id="2b7f8-284">Microsoft Visual Studio Web ekibi üzerinde üst düzey Program Yöneticisi sayed Hashimi olduğu ve SideWaffle şablonları altın standart olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-284">Sayed Hashimi is a Senior Program Manager on the Visual Studio Web Team at Microsoft and SideWaffle templates are considered the gold standard.</span></span> <span data-ttu-id="2b7f8-285">Bu yazma zaman SideWaffle Visual Studio 2012 için 2013 ve 2015 kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-285">At the time of this writing, SideWaffle is available for Visual Studio 2012, 2013, and 2015.</span></span>

### <a name="routing-and-multiple-views"></a><span data-ttu-id="2b7f8-286">Yönlendirme ve birden çok görünüm</span><span class="sxs-lookup"><span data-stu-id="2b7f8-286">Routing and multiple views</span></span>

<span data-ttu-id="2b7f8-287">AngularJS SPA (tek sayfa uygulaması) tabanlı gezinti işlemek için bir yerleşik rota sağlayıcısı sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-287">AngularJS has a built-in route provider to handle SPA (Single Page Application) based navigation.</span></span> <span data-ttu-id="2b7f8-288">AngularJS içinde yönlendirme ile çalışmak için eklemelisiniz `angular-route` Bower kullanarak kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-288">To work with routing in AngularJS, you must add the `angular-route` library using Bower.</span></span> <span data-ttu-id="2b7f8-289">Görebilirsiniz [bower.json](#angular-bower-json) biz zaten bu bizim projesinde başvurduğunuzdan bu makalenin başlangıcında başvurulan dosya.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-289">You can see in the [bower.json](#angular-bower-json) file referenced at the start of this article that we are already referencing it in our project.</span></span>

<span data-ttu-id="2b7f8-290">Paketi yüklendikten sonra komut dosyası başvurusunu ekleyin (*route.js Açısal*) görünümünüz için.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-290">After you install the package, add the script reference (*angular-route.js*) to your view.</span></span>

<span data-ttu-id="2b7f8-291">Şimdi kişi oluşturma ve gezinti ekleyin uygulama atalım.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-291">Now let's take the Person App we have been building and add navigation to it.</span></span> <span data-ttu-id="2b7f8-292">İlk olarak, uygulama bir kopyasını yeni bir oluşturarak yapacağız `PeopleController` çağrılan eylem `Spa` ve karşılık gelen `Spa.cshtml` Index.cshtml görünümünde kopyalayarak Görünüm `People` klasör.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-292">First, we will make a copy of the app by creating a new `PeopleController` action called `Spa` and a corresponding `Spa.cshtml` view by copying the Index.cshtml view in the `People` folder.</span></span> <span data-ttu-id="2b7f8-293">Komut dosyası için bir başvuru ekleyin `angular-route` (satır 11 bakın).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-293">Add a script reference to `angular-route` (see line 11).</span></span> <span data-ttu-id="2b7f8-294">Ayrıca bir `div` ile işaretlenen `ng-view` yönergesi (bkz. satır 6) görünümlerde yerleştirmek için yer tutucu olarak.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-294">Also add a `div` marked with the `ng-view` directive (see line 6) as a placeholder to place views in.</span></span> <span data-ttu-id="2b7f8-295">Birçok ek kullanılmasını kalacaklarını *.js* 13-16 satırlarında başvuran dosyaları.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-295">We are going to be using several additional *.js* files which are referenced on lines 13-16.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

<span data-ttu-id="2b7f8-296">Bir göz atalım *personModule.js* nasıl biz yönlendirme modülü başlatmasını görmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-296">Let's take a look at *personModule.js* file to see how we are instantiating the module with routing.</span></span> <span data-ttu-id="2b7f8-297">Geçiriyoruz `ngRoute` modüle kitaplık olarak.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-297">We are passing `ngRoute` as a library into the module.</span></span> <span data-ttu-id="2b7f8-298">Bu modül, uygulamamız içinde yönlendirme işler.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-298">This module handles routing in our application.</span></span>

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

<span data-ttu-id="2b7f8-299">*PersonRoutes.js* dosya, aşağıda, rota sağlayıcısını temel yolları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-299">The *personRoutes.js* file, below, defines routes based on the route provider.</span></span> <span data-ttu-id="2b7f8-300">Satırlar 4-7 etkili bir şekilde, bir URL ile zaman söyleyerek Gezinti tanımlayın `/persons` olduğundan istenen adlı bir şablon kullanmak `partials/personlist` aracılığıyla çalışarak `personListController`.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-300">Lines 4-7 define navigation by effectively saying, when a URL with `/persons` is requested, use a template called `partials/personlist` by working through `personListController`.</span></span> <span data-ttu-id="2b7f8-301">Satırları 8-11 belirtmek ayrıntı sayfası bir rota parametresi ile `personId`.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-301">Lines 8-11 indicate a detail page with a route parameter of `personId`.</span></span> <span data-ttu-id="2b7f8-302">URL bir desen eşleşmiyorsa, Angular varsayılan olarak `/persons` görünümü.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-302">If the URL doesn't match one of the patterns, Angular defaults to the `/persons` view.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

<span data-ttu-id="2b7f8-303">`personlist.html` Kısmi görünüm yalnızca kişi listesini görüntülemek için gereken HTML içeren bir dosyadır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-303">The `personlist.html` file is a partial view containing only the HTML needed to display person list.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

<span data-ttu-id="2b7f8-304">Denetleyici modülün kullanılarak tanımlanan `controller` işlevi *personListController.js*.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-304">The controller is defined by using the module's `controller` function in *personListController.js*.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

<span data-ttu-id="2b7f8-305">Bu uygulamayı çalıştırmak ve gidin `people/spa#/persons` URL, biz görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-305">If we run this application and navigate to the `people/spa#/persons` URL, we will see:</span></span>

![Kişiler Liste Görünümü](angular/_static/spa-persons.png)

<span data-ttu-id="2b7f8-307">Biz ayrıntı sayfaya, örneğin gidin, `people/spa#/persons/2`, biz ayrıntı kısmi görünümü görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-307">If we navigate to a detail page, for example `people/spa#/persons/2`, we will see the detail partial view:</span></span>

![Kişinin ayrıntılı Görünüm](angular/_static/spa-persons-2.png)

<span data-ttu-id="2b7f8-309">Tam kaynak ve bu makalede gösterilen olmayan herhangi bir dosya görüntüleyebilirsiniz [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-309">You can view the full source and any files not shown in this article on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

### <a name="event-handlers"></a><span data-ttu-id="2b7f8-310">Olay İşleyicileri</span><span class="sxs-lookup"><span data-stu-id="2b7f8-310">Event Handlers</span></span>

<span data-ttu-id="2b7f8-311">Vardır, HTML DOM giriş öğeleri olay işleme özellikleri eklemeyi AngularJS yönergesi</span><span class="sxs-lookup"><span data-stu-id="2b7f8-311">There are a number of directives in AngularJS that add event-handling capabilities to the input elements in your HTML DOM.</span></span> <span data-ttu-id="2b7f8-312">AngularJS yerleşik olaylarının bir listesi aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-312">Below is a list of the events that are built into AngularJS.</span></span>

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
> <span data-ttu-id="2b7f8-313">Kullanarak kendi olay işleyicileri ekleme [özel yönergeleri özellik içinde AngularJS](https://docs.angularjs.org/guide/directive).</span><span class="sxs-lookup"><span data-stu-id="2b7f8-313">You can add your own event handlers using the [custom directives feature in AngularJS](https://docs.angularjs.org/guide/directive).</span></span>

<span data-ttu-id="2b7f8-314">Ne bakalım `ng-click` olay kablolu ayarlama.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-314">Let's look at how the `ng-click` event is wired up.</span></span> <span data-ttu-id="2b7f8-315">Adlı yeni bir JavaScript dosyası oluşturun *eventHandlerController.js*ve aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2b7f8-315">Create a new JavaScript file named *eventHandlerController.js*, and add the following to it:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

<span data-ttu-id="2b7f8-316">Yeni fark `sayName` işlevi `eventHandlerController` üzerinde yukarıdaki 5 satır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-316">Notice the new `sayName` function in `eventHandlerController` on line 5 above.</span></span> <span data-ttu-id="2b7f8-317">Tüm yöntemi yaptığını Hoş Geldiniz iletisi kullanıcı JavaScript uyarı artık gösteren için.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-317">All the method is doing for now is showing a JavaScript alert to the user with a welcome message.</span></span>

<span data-ttu-id="2b7f8-318">Aşağıdaki görünüm denetleyicisini işlevi bir AngularJS olay bağlar.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-318">The view below binds a controller function to an AngularJS event.</span></span> <span data-ttu-id="2b7f8-319">Satır 9 sahip bir düğme üzerinde `ng-click` Açısal yönergesi uygulandı.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-319">Line 9 has a button on which the `ng-click` Angular directive has been applied.</span></span> <span data-ttu-id="2b7f8-320">Çağırır bizim `sayName` bağlı işlevi `$scope` nesne bu görünüme geçirildi.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-320">It calls our `sayName` function, which is attached to the `$scope` object passed to this view.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

<span data-ttu-id="2b7f8-321">Çalışan bir örnek gösterilmektedir denetleyicinin `sayName` işlevi düğme tıklatıldığında otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-321">The running example demonstrates that the controller's `sayName` function is called automatically when the button is clicked.</span></span>

![Tıklama olayı](angular/_static/events.png)

<span data-ttu-id="2b7f8-323">AngularJS yerleşik olay işleyicisi yönergeleri hakkında daha fazla ayrıntı için head için mutlaka [belgeleri Web sitesi](https://docs.angularjs.org/api/ng/directive/ngClick) AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2b7f8-323">For more detail on AngularJS built-in event handler directives, be sure to head to the [documentation website](https://docs.angularjs.org/api/ng/directive/ngClick) of AngularJS.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b7f8-324">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2b7f8-324">Additional resources</span></span>

* [<span data-ttu-id="2b7f8-325">Açısal belgeleri</span><span class="sxs-lookup"><span data-stu-id="2b7f8-325">Angular Docs</span></span>](https://docs.angularjs.org)

* [<span data-ttu-id="2b7f8-326">Açısal 2 bilgisi</span><span class="sxs-lookup"><span data-stu-id="2b7f8-326">Angular 2 Info</span></span>](https://angular.io/)
