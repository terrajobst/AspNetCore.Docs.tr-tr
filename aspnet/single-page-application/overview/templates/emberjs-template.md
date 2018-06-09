---
uid: single-page-application/overview/templates/emberjs-template
title: EmberJS şablon | Microsoft Docs
author: xqiu
description: EmberJS şablonu
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566346"
---
<a name="emberjs-template"></a><span data-ttu-id="d5335-103">EmberJS şablonu</span><span class="sxs-lookup"><span data-stu-id="d5335-103">EmberJS template</span></span>
====================
<span data-ttu-id="d5335-104">tarafından [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="d5335-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="d5335-105">EmberJS MVC şablonu Nathan Totten, Thiago Santos ve Xinyang Qiu tarafından yazılır.</span><span class="sxs-lookup"><span data-stu-id="d5335-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="d5335-106">EmberJS MVC şablonu indirme</span><span class="sxs-lookup"><span data-stu-id="d5335-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="d5335-107">EmberJS SPA şablon hızla EmberJS kullanarak etkileşimli istemci tarafı web uygulamaları oluşturmaya başlamanıza yardımcı olmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d5335-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="d5335-108">"Tek sayfalı uygulama" (SPA) olan tek bir HTML sayfası yükler ve ardından sayfanın dinamik olarak yeni sayfa yüklenirken yerine güncelleştiren bir web uygulaması için genel bir terim.</span><span class="sxs-lookup"><span data-stu-id="d5335-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="d5335-109">İlk sayfa yükleme, AJAX istekleri aracılığıyla sunucusuyla SPA alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d5335-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="d5335-110">AJAX yeni bir şey değildir, ancak bugün oluşturmasına ve büyük ve karmaşık bir SPA uygulama korumasına kolaylaştırmak JavaScript çerçeveleri vardır.</span><span class="sxs-lookup"><span data-stu-id="d5335-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="d5335-111">Ayrıca, HTML 5 ve CSS3 zengin Uı'lar oluşturmak kolaylaştırarak.</span><span class="sxs-lookup"><span data-stu-id="d5335-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="d5335-112">EmberJS SPA şablonu kullanan [Ember](http://emberjs.com/) Sayfa güncelleştirmelerini AJAX istekleri işlemek için JavaScript kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="d5335-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="d5335-113">Ember.js veri bağlama sayfanın en son veriler ile eşitlenmesi için kullanır.</span><span class="sxs-lookup"><span data-stu-id="d5335-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="d5335-114">Böylece, herhangi bir JSON verilerine anlatılmaktadır ve DOM güncelleştiren kodu yazmak zorunda değilsiniz</span><span class="sxs-lookup"><span data-stu-id="d5335-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="d5335-115">Bunun yerine, verileri sunmak nasıl Ember.js anlatın HTML'de bildirim temelli öznitelikleri yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="d5335-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="d5335-116">Sunucu tarafında EmberJS şablonu neredeyse aynıdır [Çakıştırmaları SPA şablon](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="d5335-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="d5335-117">ASP.NET MVC HTML belgeleri ve istemci AJAX isteği işlemek için ASP.NET Web API sunmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="d5335-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="d5335-118">Şablon bu yönleri hakkında daha fazla bilgi için başvurmak [Çakıştırmaları şablonu](../introduction/knockoutjs-template.md) belgeleri.</span><span class="sxs-lookup"><span data-stu-id="d5335-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="d5335-119">Bu konuda Boşaltılan şablonu ve EmberJS şablonu arasındaki farklar odaklanır.</span><span class="sxs-lookup"><span data-stu-id="d5335-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="d5335-120">Bir EmberJS SPA şablonu projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="d5335-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="d5335-121">İndirin ve şablonu yukarıdaki karşıdan yükleme düğmesini tıklatarak yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d5335-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="d5335-122">Visual Studio yeniden başlatmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="d5335-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="d5335-123">İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="d5335-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="d5335-124">Altında **Visual C#** seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="d5335-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="d5335-125">Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="d5335-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="d5335-126">Proje adı ve'ı tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="d5335-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="d5335-127">İçinde **yeni proje** seçin **Ember.js SPA proje**.</span><span class="sxs-lookup"><span data-stu-id="d5335-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="d5335-128">EmberJS SPA şablon genel bakış</span><span class="sxs-lookup"><span data-stu-id="d5335-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="d5335-129">EmberJS şablon jQuery, Ember.js, kesintisiz, etkileşimli kullanıcı Arabirimi oluşturmak için Handlebars.js bir bileşimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="d5335-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="d5335-130">Ember.js bir istemci-tarafı MVC desen kullanan bir JavaScript kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="d5335-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="d5335-131">A *şablonu*Handlebars şablon dilinde yazılmış uygulama kullanıcı arabirimi açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d5335-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="d5335-132">Yayın modunda [Handlebars derleyici](https://github.com/Myslik/csharp-ember-handlebars) paketini ve handlebars şablonu derlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d5335-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="d5335-133">A *modeli* (Yapılacaklar listesi ve yapılacaklar öğelerini) sunucudan alır uygulama verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="d5335-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="d5335-134">A *denetleyicisi* uygulama durumu depolar.</span><span class="sxs-lookup"><span data-stu-id="d5335-134">A *controller* stores application state.</span></span> <span data-ttu-id="d5335-135">Denetleyicileri genellikle model veri sunmak için karşılık gelen şablonları.</span><span class="sxs-lookup"><span data-stu-id="d5335-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="d5335-136">A *Görünüm* uygulamadaki ilkel olayları dönüşür ve bunlar denetleyicisine geçirir.</span><span class="sxs-lookup"><span data-stu-id="d5335-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="d5335-137">A *yönlendirici* URL'leri ve şablonları eşitlenmiş şekilde kalmasının uygulama durumu yönetir.</span><span class="sxs-lookup"><span data-stu-id="d5335-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="d5335-138">Ayrıca, Ember veri kitaplığı JSON nesnelerinin (RESTful API'si aracılığıyla sunucusundan alınan) ve istemci modelleri eşitlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5335-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="d5335-139">EmberJS SPA şablon sekiz katmanlara betiklerini düzenler:</span><span class="sxs-lookup"><span data-stu-id="d5335-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="d5335-140">webapı\_adapter.js, webapı\_serializer.js: ASP.NET Web API ile çalışmak için Ember veri kitaplığı genişletir.</span><span class="sxs-lookup"><span data-stu-id="d5335-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="d5335-141">Scripts/helpers.js: yeni Ember Handlebars Yardımcıları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5335-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="d5335-142">Scripts/App.js: uygulamasını oluşturur ve seri hale getirici bağdaştırıcısı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d5335-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="d5335-143">Uygulama/betikleri/modelleri/\*.js: modelleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5335-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="d5335-144">Uygulama/betikleri/görünümler/\*.js: görünümleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5335-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="d5335-145">Uygulama/betikleri/denetleyicileri/\*.js: denetleyicileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5335-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="d5335-146">Uygulama/komut dosyaları/yolları, Scripts/app/router.js: yolları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5335-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="d5335-147">Şablonlar /\*.hbs: handlebars şablonları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5335-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="d5335-148">Bu komut dosyalarını daha ayrıntılı bazıları bakalım.</span><span class="sxs-lookup"><span data-stu-id="d5335-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="d5335-149">Modeller</span><span class="sxs-lookup"><span data-stu-id="d5335-149">Models</span></span>

<span data-ttu-id="d5335-150">Modeller uygulama/betikleri/modelleri klasöründe tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="d5335-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="d5335-151">İki model dosya vardır: todoItem.js ve todoList.js.</span><span class="sxs-lookup"><span data-stu-id="d5335-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="d5335-152">**TODO.model.js** Yapılacaklar listelerinde için istemci tarafı (tarayıcı) modelleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5335-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="d5335-153">İki model sınıfları vardır: Todoıtem ve todoList.</span><span class="sxs-lookup"><span data-stu-id="d5335-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="d5335-154">Ember içinde alt DS modelleri sınıflarıdır. Model.</span><span class="sxs-lookup"><span data-stu-id="d5335-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="d5335-155">Bir model öznitelikleri olan özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="d5335-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="d5335-156">Modelleri diğer modellerle ilişkiler tanımlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d5335-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="d5335-157">Modelleri diğer özelliklere bağlama özellikleri hesaplanan:</span><span class="sxs-lookup"><span data-stu-id="d5335-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="d5335-158">Modelleri gözlemlenen özelliği değiştiğinde çağrılan gözlemci işlevlerini sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="d5335-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="d5335-159">Görünümler</span><span class="sxs-lookup"><span data-stu-id="d5335-159">Views</span></span>

<span data-ttu-id="d5335-160">Görünümler uygulama/betikleri/görünümleri klasöründe tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="d5335-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="d5335-161">Bir görünüm uygulama kullanıcı Arabirimi olaylarından çevirir.</span><span class="sxs-lookup"><span data-stu-id="d5335-161">A view translates events from the application UI.</span></span> <span data-ttu-id="d5335-162">Olay işleyici Denetleyicisi işlevleri için geri arama veya yalnızca veri bağlamı doğrudan çağırın.</span><span class="sxs-lookup"><span data-stu-id="d5335-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="d5335-163">Örneğin, aşağıdaki kodu views/TodoItemEditView.js olur.</span><span class="sxs-lookup"><span data-stu-id="d5335-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="d5335-164">Olay işleme için bir giriş metin alanı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5335-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="d5335-165">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="d5335-165">Controller</span></span>

<span data-ttu-id="d5335-166">Denetleyicileri uygulama/betikleri/denetleyicileri klasöründe tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="d5335-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="d5335-167">Tek bir modeli temsil etmek için genişletme `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="d5335-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="d5335-168">Bir denetleyici de modelleri koleksiyonu genişleterek gösterebilir `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="d5335-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="d5335-169">Örneğin, bir dizi TodoListController temsil eden `todoList` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="d5335-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="d5335-170">Azalan düzende todoList Kimliğine göre denetleyicisi sıralar:</span><span class="sxs-lookup"><span data-stu-id="d5335-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="d5335-171">Adlı bir işlev denetleyicisi tanımlar `addTodoList`, yeni bir Yapılacaklar listesi oluşturur ve diziye ekler.</span><span class="sxs-lookup"><span data-stu-id="d5335-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="d5335-172">Bu işlev nasıl çağrılır görmek için şablonlar klasöründe todoListTemplate.html adlı şablon dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="d5335-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="d5335-173">Aşağıdaki şablonu kodu bir düğmeye bağlar `addTodoList` işlevi:</span><span class="sxs-lookup"><span data-stu-id="d5335-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="d5335-174">Denetleyici de içeren bir `error` bir hata iletisi tutan özelliği.</span><span class="sxs-lookup"><span data-stu-id="d5335-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="d5335-175">Hata iletisinde (aynı zamanda todoListTemplate.html) görüntülemek için şablon kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="d5335-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="d5335-176">Yollar</span><span class="sxs-lookup"><span data-stu-id="d5335-176">Routes</span></span>

<span data-ttu-id="d5335-177">Router.js yollar ve uygulama durumunu ayarlar görüntülenecek varsayılan şablonu tanımlar ve yolları URL'lerle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="d5335-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="d5335-178">TodoListRoute.js setupController işlevi geçersiz kılarak verileri için TodoListRoute yükler:</span><span class="sxs-lookup"><span data-stu-id="d5335-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="d5335-179">Ember URL'leri, rota adları, denetleyicileri ve şablonları eşleştirmek için adlandırma kuralları kullanır.</span><span class="sxs-lookup"><span data-stu-id="d5335-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="d5335-180">Daha fazla bilgi için bkz: [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS belgelerine.</span><span class="sxs-lookup"><span data-stu-id="d5335-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="d5335-181">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="d5335-181">Templates</span></span>

<span data-ttu-id="d5335-182">Şablonlar klasörüne dört şablonlarını içerir:</span><span class="sxs-lookup"><span data-stu-id="d5335-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="d5335-183">Application.hbs: uygulama başlatıldıktan sonra işlenen varsayılan şablonu.</span><span class="sxs-lookup"><span data-stu-id="d5335-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="d5335-184">About.hbs: "/ hakkında" rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="d5335-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="d5335-185">index.hbs: kök şablonu "/" rota.</span><span class="sxs-lookup"><span data-stu-id="d5335-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="d5335-186">todoList.hbs: şablon için "/ todo" rota.</span><span class="sxs-lookup"><span data-stu-id="d5335-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="d5335-187">\_navbar.hbs: Gezinti Menüsü şablonu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5335-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="d5335-188">Uygulama şablonu, bir ana sayfa gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="d5335-188">The application template acts like a master page.</span></span> <span data-ttu-id="d5335-189">Üstbilgi ve altbilgi "{{rota bağlı olarak diğer şablonlar eklemek için prizine}}" içerir.</span><span class="sxs-lookup"><span data-stu-id="d5335-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="d5335-190">Ember uygulama şablonları hakkında daha fazla bilgi için bkz: [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="d5335-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="d5335-191">"/ TodoList" şablonu iki döngü ifadeleri içeriyor.</span><span class="sxs-lookup"><span data-stu-id="d5335-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="d5335-192">Dış döngü `{{#each controller}}`ve iç döngü `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="d5335-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="d5335-193">Aşağıdaki kod bir yerleşik gösterir `Ember.Checkbox` görüntülemek, özelleştirilmiş `App.TodoItemEditView`, bir bağlantı ile bir `deleteTodo` eylem.</span><span class="sxs-lookup"><span data-stu-id="d5335-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="d5335-194">`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs tanımlanmış sınıf tanımlayan bir yardımcı dosyaları önbelleğe alınması ve şablonu eklemek için işlevi **hata ayıklama** ayarlanır **true** Web.config dosyasında.</span><span class="sxs-lookup"><span data-stu-id="d5335-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="d5335-195">Bu işlev, Views/Home/App.cshtml içinde tanımlanan ASP.NET MVC görünümü dosyasından çağrılır:</span><span class="sxs-lookup"><span data-stu-id="d5335-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="d5335-196">Bağımsız değişkenler olmadan adlı, işlev tüm şablonları klasöründeki şablon dosyaları işler.</span><span class="sxs-lookup"><span data-stu-id="d5335-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="d5335-197">Ayrıca, bir alt klasör veya bir özel şablon dosyası belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d5335-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="d5335-198">Zaman **hata ayıklama** olan **false** Web.config dosyasında uygulama paket öğesi "~/bundles/templates" içerir.</span><span class="sxs-lookup"><span data-stu-id="d5335-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="d5335-199">Bu paket öğesi Handlebars derleyici kitaplığını kullanarak BundleConfig.cs eklenir:</span><span class="sxs-lookup"><span data-stu-id="d5335-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
