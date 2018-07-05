---
uid: single-page-application/overview/templates/emberjs-template
title: EmberJS şablonu | Microsoft Docs
author: xqiu
description: EmberJS şablonu
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1f5b005180fed15c51b36417cdcd6acc98123c9a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374172"
---
<a name="emberjs-template"></a><span data-ttu-id="31de9-103">EmberJS şablonu</span><span class="sxs-lookup"><span data-stu-id="31de9-103">EmberJS template</span></span>
====================
<span data-ttu-id="31de9-104">tarafından [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="31de9-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="31de9-105">EmberJS MVC şablonu Nathan Totten, Thiago Santos ve Xinyang Qiu yazılır.</span><span class="sxs-lookup"><span data-stu-id="31de9-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="31de9-106">EmberJS MVC şablonu indirin</span><span class="sxs-lookup"><span data-stu-id="31de9-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="31de9-107">SPA EmberJS şablonu EmberJS kullanarak etkileşimli istemci tarafı web uygulamalarını hızlı bir şekilde oluşturmaya başlamanıza yardımcı olmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="31de9-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="31de9-108">"Tek sayfalı uygulama" (SPA) olduğundan tek bir HTML sayfası yükler ve ardından sayfanın dinamik olarak yeni sayfa yükleniyor yerine güncelleştiren bir web uygulaması için genel bir terimdir.</span><span class="sxs-lookup"><span data-stu-id="31de9-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="31de9-109">Başlangıç sayfası yükleme sonrası AJAX istekleri aracılığıyla sunucusuyla SPA anlatıyor.</span><span class="sxs-lookup"><span data-stu-id="31de9-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="31de9-110">AJAX yeni bir şey değildir, ancak bugün oluşturun ve büyük ve karmaşık bir SPA uygulama sürdürmek kolaylaştıran yeni JavaScript çerçevesi vardır.</span><span class="sxs-lookup"><span data-stu-id="31de9-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="31de9-111">Ayrıca, HTML 5 ve CSS3 zengin kullanıcı arabirimleri oluşturmak kolaylaşır.</span><span class="sxs-lookup"><span data-stu-id="31de9-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="31de9-112">EmberJS SPA şablonu kullanan [Ember](http://emberjs.com/) Sayfa güncelleştirmelerini AJAX isteklerini işlemek için JavaScript kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="31de9-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="31de9-113">Sayfanın en son verileri eşitlemek için veri bağlama Ember.js kullanır.</span><span class="sxs-lookup"><span data-stu-id="31de9-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="31de9-114">Böylece, herhangi bir JSON verilerini aracılığıyla size yol gösterir ve DOM güncelleştiren kod yazmanız gerekmez</span><span class="sxs-lookup"><span data-stu-id="31de9-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="31de9-115">Bunun yerine, Ember.js veri sunma hakkında bilgi HTML dosyasındaki bildirim temelli öznitelikleri yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="31de9-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="31de9-116">Sunucu tarafında EmberJS şablonu neredeyse aynıdır [SPA KnockoutJS şablonu](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="31de9-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="31de9-117">ASP.NET MVC, HTML belgeleri ve istemciden AJAX isteklerini işlemek için ASP.NET Web API sunmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="31de9-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="31de9-118">Şablonu bu yönleri hakkında daha fazla bilgi için başvurmak [KnockoutJS şablonu](../introduction/knockoutjs-template.md) belgeleri.</span><span class="sxs-lookup"><span data-stu-id="31de9-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="31de9-119">Bu konuda Knockout şablonu EmberJS şablonu arasındaki farkları ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="31de9-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="31de9-120">Bir EmberJS SPA şablonu projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="31de9-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="31de9-121">İndirin ve yukarıdaki indir düğmesine tıklayarak şablonu yükleyin.</span><span class="sxs-lookup"><span data-stu-id="31de9-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="31de9-122">Visual Studio'yu yeniden başlatmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="31de9-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="31de9-123">İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="31de9-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="31de9-124">Altında **Visual C#** seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="31de9-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="31de9-125">Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="31de9-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="31de9-126">Projeyi adlandırın ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="31de9-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="31de9-127">İçinde **yeni proje** seçin **Ember.js SPA proje**.</span><span class="sxs-lookup"><span data-stu-id="31de9-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="31de9-128">EmberJS SPA şablonuna genel bakış</span><span class="sxs-lookup"><span data-stu-id="31de9-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="31de9-129">EmberJS şablonu, jQuery, Ember.js, kesintisiz, etkileşimli bir kullanıcı Arabirimi oluşturmak için Handlebars.js bir bileşimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="31de9-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="31de9-130">Ember.js bir istemci-tarafı MVC desen kullanan bir JavaScript kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="31de9-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="31de9-131">A *şablon*Gidon şablon dilinde yazılı uygulama kullanıcı arabirimini açıklar.</span><span class="sxs-lookup"><span data-stu-id="31de9-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="31de9-132">Yayın modunda [Gidon derleyici](https://github.com/Myslik/csharp-ember-handlebars) gruplandırma ve gidon şablon derlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="31de9-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="31de9-133">A *modeli* sunucudan (ToDo listeler ve ToDo öğeleri) alır uygulama verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="31de9-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="31de9-134">A *denetleyicisi* uygulama durumunu depolar.</span><span class="sxs-lookup"><span data-stu-id="31de9-134">A *controller* stores application state.</span></span> <span data-ttu-id="31de9-135">Denetleyicileri, genellikle model verilerine karşılık gelen şablonları sunar.</span><span class="sxs-lookup"><span data-stu-id="31de9-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="31de9-136">A *görünümü* uygulamadaki temel olayları çevirir ve bunlar denetleyicisine geçirir.</span><span class="sxs-lookup"><span data-stu-id="31de9-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="31de9-137">A *yönlendirici* URL'leri ve şablonları eşitlenmiş tutmak, uygulama durumunu yönetir.</span><span class="sxs-lookup"><span data-stu-id="31de9-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="31de9-138">Ayrıca, Ember veri kitaplığı JSON nesneleri (sunucunun bir RESTful API'si üzerinden alınan) ve istemci modelleri eşitlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="31de9-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="31de9-139">SPA EmberJS şablonu sekiz katmanlara betiklerini düzenler:</span><span class="sxs-lookup"><span data-stu-id="31de9-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="31de9-140">webapı\_adapter.js, webapı\_serializer.js: ASP.NET Web API'si ile çalışmaya Ember veri kitaplığı genişletir.</span><span class="sxs-lookup"><span data-stu-id="31de9-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="31de9-141">Scripts/helpers.js: yeni Ember Gidon Yardımcıları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="31de9-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="31de9-142">Scripts/App.js: uygulaması oluşturur ve bu bağdaştırıcının ve seri hale getirici yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="31de9-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="31de9-143">Betikleri/uygulama/modelleri/\*.js: modelleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="31de9-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="31de9-144">Betikleri/uygulama/görünümler/\*.js: görünümleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="31de9-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="31de9-145">Betikleri/uygulama/denetleyicileri/\*.js: denetleyicileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="31de9-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="31de9-146">Uygulama/komut dosyaları/yolları, Scripts/app/router.js: yollarını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="31de9-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="31de9-147">Şablonlar /\*.hbs: gidon şablonları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="31de9-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="31de9-148">Daha ayrıntılı bir şekilde bu betiklerden bazıları en göz atalım.</span><span class="sxs-lookup"><span data-stu-id="31de9-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="31de9-149">Modeller</span><span class="sxs-lookup"><span data-stu-id="31de9-149">Models</span></span>

<span data-ttu-id="31de9-150">Modelleri betikleri/uygulama/modelleri klasöründe tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="31de9-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="31de9-151">İki model dosyası vardır: todoItem.js ve todoList.js.</span><span class="sxs-lookup"><span data-stu-id="31de9-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="31de9-152">**TODO.model.js** yapılacak işler listelerinin için istemci tarafı (tarayıcı) modelleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="31de9-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="31de9-153">İki model sınıfı vardır: Todoıtem ve yapılacaklar listesi.</span><span class="sxs-lookup"><span data-stu-id="31de9-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="31de9-154">Ember alt sınıfların DS modelleridir. Model.</span><span class="sxs-lookup"><span data-stu-id="31de9-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="31de9-155">Bir model öznitelikleri olan özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="31de9-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="31de9-156">Modelleri, diğer modellerle ilişkiler tanımlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="31de9-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="31de9-157">Modelleri hesaplanan diğer özelliklere bağlama özellikler:</span><span class="sxs-lookup"><span data-stu-id="31de9-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="31de9-158">Modelleri, gözlemlenen bir özellik değiştiğinde çağrılan gözlemci işlevleri sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="31de9-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="31de9-159">Görünümler</span><span class="sxs-lookup"><span data-stu-id="31de9-159">Views</span></span>

<span data-ttu-id="31de9-160">Görünümler betikleri/uygulama/görünümler klasöründe tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="31de9-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="31de9-161">Bir görünümü uygulama UI olaylardan çevirir.</span><span class="sxs-lookup"><span data-stu-id="31de9-161">A view translates events from the application UI.</span></span> <span data-ttu-id="31de9-162">Bir olay işleyicisi Denetleyicisi işlevleri için geri arama veya yalnızca veri bağlamı doğrudan çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="31de9-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="31de9-163">Örneğin, aşağıdaki kod views/TodoItemEditView.js olur.</span><span class="sxs-lookup"><span data-stu-id="31de9-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="31de9-164">Olay işleme için bir giriş metin alanı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="31de9-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="31de9-165">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="31de9-165">Controller</span></span>

<span data-ttu-id="31de9-166">Denetleyicileri betikleri/uygulama/denetleyicileri klasöründe tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="31de9-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="31de9-167">Tek bir modeli temsil etmek için genişletme `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="31de9-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="31de9-168">Bir denetleyici de modellerin bir koleksiyonu genişleterek gösterebilir `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="31de9-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="31de9-169">Örneğin, bir dizi TodoListController temsil `todoList` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="31de9-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="31de9-170">Denetleyiciye azalan düzende todoList Kimliğe göre sıralar:</span><span class="sxs-lookup"><span data-stu-id="31de9-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="31de9-171">Denetleyici adlı bir işlev tanımlar `addTodoList`, yeni bir Yapılacaklar listesi oluşturur ve diziye ekler.</span><span class="sxs-lookup"><span data-stu-id="31de9-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="31de9-172">Nasıl bu işlev çağrıldığında görmek için şablonlar klasöründeki todoListTemplate.html adlı şablon dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="31de9-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="31de9-173">Bir düğme için aşağıdaki şablon kodunu bağlar `addTodoList` işlevi:</span><span class="sxs-lookup"><span data-stu-id="31de9-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="31de9-174">Denetleyici de içeren bir `error` özelliği bir hata iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="31de9-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="31de9-175">Hata iletisinde (Ayrıca todoListTemplate.html) görüntülemek için şablon kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="31de9-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="31de9-176">Yollar</span><span class="sxs-lookup"><span data-stu-id="31de9-176">Routes</span></span>

<span data-ttu-id="31de9-177">Router.js yolları ve uygulama durumu ayarlar görüntülenecek varsayılan şablonu tanımlar ve yolları URL'lerle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="31de9-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="31de9-178">TodoListRoute.js setupController işlevi geçersiz kılarak, TodoListRoute için verileri yükler:</span><span class="sxs-lookup"><span data-stu-id="31de9-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="31de9-179">Ember URL'leri, rota adları, denetleyicileri ve şablonları eşleştirmek için adlandırma kuralları kullanır.</span><span class="sxs-lookup"><span data-stu-id="31de9-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="31de9-180">Daha fazla bilgi için [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS belgelerine.</span><span class="sxs-lookup"><span data-stu-id="31de9-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="31de9-181">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="31de9-181">Templates</span></span>

<span data-ttu-id="31de9-182">Şablonlar, dört şablonlarını içerir:</span><span class="sxs-lookup"><span data-stu-id="31de9-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="31de9-183">Application.hbs: uygulama başlatıldığında oluşturulduğunda varsayılan şablonu.</span><span class="sxs-lookup"><span data-stu-id="31de9-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="31de9-184">About.hbs: "/ hakkında" rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="31de9-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="31de9-185">index.hbs: kök şablonu "/" rota.</span><span class="sxs-lookup"><span data-stu-id="31de9-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="31de9-186">todoList.hbs: şablon için "/ todo" rota.</span><span class="sxs-lookup"><span data-stu-id="31de9-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="31de9-187">\_navbar.hbs: Gezinti Menüsü şablon tanımlar.</span><span class="sxs-lookup"><span data-stu-id="31de9-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="31de9-188">Uygulama şablonu, bir ana sayfa gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="31de9-188">The application template acts like a master page.</span></span> <span data-ttu-id="31de9-189">Bu, bir üstbilgi, altbilgi ve "{{rota bağlı olarak diğer şablonlar eklemek için bir çıkış}}" içerir.</span><span class="sxs-lookup"><span data-stu-id="31de9-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="31de9-190">Ember uygulama şablonları hakkında daha fazla bilgi için bkz. [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="31de9-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="31de9-191">"/ Yapılacaklar listesi" şablon iki döngü ifadeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="31de9-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="31de9-192">Dış döngü `{{#each controller}}`ve iç döngü `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="31de9-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="31de9-193">Aşağıdaki kod, bir yerleşik göstermektedir `Ember.Checkbox` görüntülemek, özelleştirilmiş `App.TodoItemEditView`ve bir bağlantıya sahip bir `deleteTodo` eylem.</span><span class="sxs-lookup"><span data-stu-id="31de9-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="31de9-194">`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs içinde tanımlanmış sınıf, bir yardımcı tanımlar dosyaları önbelleğe alınması ve şablonu eklemek için işlevi **hata ayıklama** ayarlanır **true** Web.config dosyasında.</span><span class="sxs-lookup"><span data-stu-id="31de9-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="31de9-195">Bu işlev, ASP.NET MVC görünüm dosyasından Views/Home/App.cshtml içinde tanımlanan çağrılır:</span><span class="sxs-lookup"><span data-stu-id="31de9-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="31de9-196">İşlev bağımsız değişken olmadan çağrıldığında tüm şablonlar klasöründe şablon dosyaları işler.</span><span class="sxs-lookup"><span data-stu-id="31de9-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="31de9-197">Ayrıca, bir alt klasör veya bir özel şablon dosyası da belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="31de9-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="31de9-198">Zaman **hata ayıklama** olduğu **false** Web.config dosyasında uygulama "~/bundles/templates" Paket öğesi içerir.</span><span class="sxs-lookup"><span data-stu-id="31de9-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="31de9-199">Bu paket öğesi Gidon derleyici kitaplığını kullanarak BundleConfig.cs eklenir:</span><span class="sxs-lookup"><span data-stu-id="31de9-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
