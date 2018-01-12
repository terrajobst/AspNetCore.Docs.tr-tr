---
title: "ASP.NET Core MVC’ye Genel Bakış"
author: ardalis
description: "ASP.NET Core MVC web uygulamaları oluşturmak için zengin bir çerçeve nasıl olduğunu öğrenin ve Model-View-Controller kullanarak API'leri düzeni tasarlayın."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 01/08/2018
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 33c293e15c0a7f18bbace9dc564fe11d93a7d509
ms.sourcegitcommit: df2157ae9aeea0075772719c29784425c783e82a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="07869-104">ASP.NET Core MVC’ye Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="07869-104">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="07869-105">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="07869-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="07869-106">ASP.NET Core MVC web uygulamaları oluşturmak için zengin bir çerçeve olan ve Model-View-Controller kullanarak API'leri düzeni tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="07869-106">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="07869-107">MVC modeli nedir?</span><span class="sxs-lookup"><span data-stu-id="07869-107">What is the MVC pattern?</span></span>

<span data-ttu-id="07869-108">Model-View-Controller (MVC) tasarım örüntüsü bir uygulama bileşenlerinin üç ana gruplara ayırır: modeller, görünümler ve denetleyiciler.</span><span class="sxs-lookup"><span data-stu-id="07869-108">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="07869-109">Elde etmek için bu deseni yardımcı [sorunları ayrılması](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="07869-109">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="07869-110">Bu deseni kullanarak, kullanıcı isteklerini kullanıcı eylemleri gerçekleştirmek ve/veya sorgu sonuçları almak için modeli ile çalışmak için sorumlu denetleyicisi yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="07869-110">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="07869-111">Denetleyici kullanıcıya gösterilmesi için görüntüyü seçer ve gerektirdiği herhangi bir Model veri ile sağlar.</span><span class="sxs-lookup"><span data-stu-id="07869-111">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="07869-112">Aşağıdaki diyagramda üç ana bileşeni gösterir ve hangilerinin diğer başvuru:</span><span class="sxs-lookup"><span data-stu-id="07869-112">The following diagram shows the three main components and which ones reference the others:</span></span>

![MVC örüntüsü](overview/_static/mvc.png)

<span data-ttu-id="07869-114">Bu kabul edilebilir açıklıkta sorumlulukları karmaşıklık açısından uygulama kodu, hata ayıklama ve bir şey (model, görünüm veya denetleyicisi) tek bir işin test etmeyi daha kolay olduğundan ölçek yardımcı olur (ve izleyen [tek sorumluluk İlkesi ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="07869-114">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="07869-115">Bu güncelleştirme, test ve iki veya daha fazla üç bu alanlar arasında yayılan bağımlılıkları olan hata ayıklama kodu daha zordur.</span><span class="sxs-lookup"><span data-stu-id="07869-115">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="07869-116">Örneğin, kullanıcı arabirimi mantığı, iş mantığı daha sık değiştirmek için eğilimi gösterir.</span><span class="sxs-lookup"><span data-stu-id="07869-116">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="07869-117">Sunu kodu ve iş mantığı tek bir nesnede birleştirildiğinde, kullanıcı arabirimi her değiştirdiğinizde iş mantığı içeren bir nesne değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="07869-117">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="07869-118">Bu, hatalara ve her en az kullanıcı arabirimi değiştirdikten sonra tüm iş mantığını retesting gerektiren olasılığı yüksektir.</span><span class="sxs-lookup"><span data-stu-id="07869-118">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="07869-119">Görünüm ve denetleyici modeline bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="07869-119">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="07869-120">Ancak, model, görünüm ne denetleyicisi bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="07869-120">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="07869-121">Bu, avantajları ayırma biridir.</span><span class="sxs-lookup"><span data-stu-id="07869-121">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="07869-122">Bu ayrım oluşturulur ve test modeli görsel sunumu bağımsız olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="07869-122">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="07869-123">Model sorumlulukları</span><span class="sxs-lookup"><span data-stu-id="07869-123">Model Responsibilities</span></span>

<span data-ttu-id="07869-124">Modelin MVC uygulamasındaki uygulama ve iş mantığı veya tarafından gerçekleştirilmesi gereken işlemleri durumunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="07869-124">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="07869-125">İş mantığı modelde uygulama durumunu sürdürmek için tüm uygulama mantığı ile birlikte kapsüllenmiş.</span><span class="sxs-lookup"><span data-stu-id="07869-125">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="07869-126">Kesin türü belirtilmiş görünümleri, verileri içerecek şekilde tasarlanmış ViewModel türleri genellikle bu görünümde göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="07869-126">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="07869-127">Denetleyici oluşturur ve bu ViewModel örnekler modelden doldurur.</span><span class="sxs-lookup"><span data-stu-id="07869-127">The controller creates and populates these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="07869-128">MVC tasarım örüntüsü kullanan bir uygulama modelinde düzenlemek için birçok yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="07869-128">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="07869-129">Bazı hakkında daha fazla bilgi [model türlerinin farklı türde](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="07869-129">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="07869-130">Görünüm sorumlulukları</span><span class="sxs-lookup"><span data-stu-id="07869-130">View Responsibilities</span></span>

<span data-ttu-id="07869-131">Görünümler, kullanıcı arabirimi aracılığıyla içerik sunmak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="07869-131">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="07869-132">Kullandıkları [Razor görüntüleme altyapısı](#razor-view-engine) .NET kodu HTML biçimlendirmesini katıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="07869-132">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="07869-133">Görünümler içinde en az mantığının olmalıdır ve bunlara herhangi bir mantık içerik sunmak için ilişkili olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="07869-133">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="07869-134">Büyük bir bölümünü mantığı görünümde gerçekleştirmek için gereken dosyaları bir karmaşık modeli verileri görüntülemek için bulursanız, kullanmayı bir [görünümü bileşen](views/view-components.md), ViewModel, ya da görünümü basitleştirmek için şablonu görüntüle.</span><span class="sxs-lookup"><span data-stu-id="07869-134">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="07869-135">Denetleyici sorumlulukları</span><span class="sxs-lookup"><span data-stu-id="07869-135">Controller Responsibilities</span></span>

<span data-ttu-id="07869-136">Denetleyicileri kullanıcı etkileşimini işleyen, modelle çalışan ve sonuç olarak işlemek için bir görünüm seçin bileşenleridir.</span><span class="sxs-lookup"><span data-stu-id="07869-136">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="07869-137">Bir MVC uygulamasında görünüm yalnızca bilgileri görüntüler; Denetleyici işler ve kullanıcı girişini ve etkileşimini yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="07869-137">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="07869-138">MVC örüntüsü denetleyicisi ilk giriş noktası ve hangi modelle çalışmak için türleri ve işlemek için hangi görünüm seçmek için sorumludur (Bu nedenle, ad - denetimleri uygulama için belirtilen bir isteğin nasıl yanıt vereceğini).</span><span class="sxs-lookup"><span data-stu-id="07869-138">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="07869-139">Denetleyicileri aşırı tarafından çok sayıda sorumlulukları karmaşık değil.</span><span class="sxs-lookup"><span data-stu-id="07869-139">Controllers should not be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="07869-140">Denetleyici mantığında fazla karmaşık hale gelmesini tutmak için kullanın [tek sorumluluk ilkesine](http://deviq.com/single-responsibility-principle/) İtme iş mantığı denetleyicisi dışında ve etki alanı modeline.</span><span class="sxs-lookup"><span data-stu-id="07869-140">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="07869-141">Denetleyici eylemleri sık aynı tür Eylemler gerçekleştirir bulursanız, takip edebilirsiniz [kendiniz ilkesine yineleme](http://deviq.com/don-t-repeat-yourself/) bu ortak eylemlere taşıyarak [filtreleri](#filters).</span><span class="sxs-lookup"><span data-stu-id="07869-141">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="07869-142">ASP.NET Core MVC nedir</span><span class="sxs-lookup"><span data-stu-id="07869-142">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="07869-143">ASP.NET Core MVC çerçevesi bir hafif, açık kaynaklı, ASP.NET Core ile kullanmak için en iyi duruma getirilmiş yüksek düzeyde sınanabilir bir sunu çerçevesidir ' dir.</span><span class="sxs-lookup"><span data-stu-id="07869-143">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="07869-144">ASP.NET Core MVC sorunları temiz ayrılması sağlayan dinamik Web siteleri oluşturmak için desenleri dayanan bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="07869-144">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="07869-145">Biçimlendirme üzerinde tam denetim verir, TDD kolay geliştirme destekler ve en son web standartlarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="07869-145">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="07869-146">Özellikler</span><span class="sxs-lookup"><span data-stu-id="07869-146">Features</span></span>

<span data-ttu-id="07869-147">ASP.NET Core MVC aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="07869-147">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="07869-148">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="07869-148">Routing</span></span>](#routing)
* [<span data-ttu-id="07869-149">Model bağlama</span><span class="sxs-lookup"><span data-stu-id="07869-149">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="07869-150">Model doğrulaması</span><span class="sxs-lookup"><span data-stu-id="07869-150">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="07869-151">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="07869-151">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="07869-152">Filtreler</span><span class="sxs-lookup"><span data-stu-id="07869-152">Filters</span></span>](#filters)
* [<span data-ttu-id="07869-153">Alanlar</span><span class="sxs-lookup"><span data-stu-id="07869-153">Areas</span></span>](#areas)
* [<span data-ttu-id="07869-154">Web API'leri</span><span class="sxs-lookup"><span data-stu-id="07869-154">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="07869-155">Test Edilebilirlik</span><span class="sxs-lookup"><span data-stu-id="07869-155">Testability</span></span>](#testability)
* [<span data-ttu-id="07869-156">Razor görüntüleme altyapısı</span><span class="sxs-lookup"><span data-stu-id="07869-156">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="07869-157">Kesin türü belirtilmiş görünümleri</span><span class="sxs-lookup"><span data-stu-id="07869-157">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="07869-158">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="07869-158">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="07869-159">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="07869-159">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="07869-160">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="07869-160">Routing</span></span>

<span data-ttu-id="07869-161">ASP.NET Core MVC üstünde oluşturulan [ASP.NET Core'nın Yönlendirme](../fundamentals/routing.md), olanak sağlayan güçlü bir URL eşlemesi bileşeni anlaşılabilir ve aranabilir URL'lere sahip uygulamalar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="07869-161">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="07869-162">Bu, web sunucusundaki dosyaları nasıl düzenlendiği için bakmadan bağlantı oluşturma ve arama motoru iyileştirme (SEO) için iyi iş uygulamanızın URL adlandırma modelleri tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="07869-162">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="07869-163">Rota değeri kısıtlamaları, Varsayılanları ve isteğe bağlı değerler destekler uygun rota şablonu sözdizimini kullanarak yollarınızı tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="07869-163">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="07869-164">*Kurala dayalı yönlendirme* URL genel tanımlamanızı biçimleri etkinleştirir, uygulamanızın kabul eder ve nasıl görüntülerin her birini biçimleri bir özel eylem yöntemine üzerinde denetleyicisi eşler.</span><span class="sxs-lookup"><span data-stu-id="07869-164">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="07869-165">Gelen bir istek alındığında yönlendirme altyapısını URL ayrıştırır ve tanımlanan URL biçimlerinden birini için eşleşen ve ilişkili denetleyicinin eylem yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="07869-165">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="07869-166">*Öznitelik yönlendirme* denetleyicileri ve eylemleri uygulamanızın yolları tanımlayın özniteliklere sahip tasarlayarak yönlendirme bilgilerini belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="07869-166">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="07869-167">Başka bir deyişle, denetleyici ve eylem ile ilişkili oldukları yanındaki route tanımlarını yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="07869-167">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a><span data-ttu-id="07869-168">Model bağlama</span><span class="sxs-lookup"><span data-stu-id="07869-168">Model binding</span></span>

<span data-ttu-id="07869-169">ASP.NET Core MVC [model bağlama](models/model-binding.md) denetleyicisi işleyebileceği nesnelerini istemci isteği verilerini (form değerleri, rota verileri, sorgu dizesi parametreleri, HTTP üstbilgileri) dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="07869-169">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="07869-170">Sonuç olarak, denetleyici mantığınızı gelen istek verileri getirme işini yapmak zorunda değildir; yalnızca kendi eylem yöntemleri için parametre olarak veri yok.</span><span class="sxs-lookup"><span data-stu-id="07869-170">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="07869-171">Model doğrulama</span><span class="sxs-lookup"><span data-stu-id="07869-171">Model validation</span></span>

<span data-ttu-id="07869-172">ASP.NET Core MVC destekleyen [doğrulama](models/validation.md) , model nesnesi veri ek açıklaması doğrulama öznitelikleri ile dekorasyon tarafından.</span><span class="sxs-lookup"><span data-stu-id="07869-172">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="07869-173">Doğrulama özniteliklerinin değerleri sunucuya gönderilen önce istemci tarafında denetlenir yanı denetleyici eylemini önce sunucuda olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="07869-173">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

<span data-ttu-id="07869-174">Bir denetleyici eylemi:</span><span class="sxs-lookup"><span data-stu-id="07869-174">A controller action:</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="07869-175">Framework doğrulama isteği verileri istemci ve sunucu işler.</span><span class="sxs-lookup"><span data-stu-id="07869-175">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="07869-176">Model türlerinde belirtilen doğrulama mantığını oluşturulmuş görünümler örtük ek açıklamaları olarak eklenir ve tarayıcı ile zorlanır [jQuery doğrulama](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="07869-176">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="07869-177">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="07869-177">Dependency injection</span></span>

<span data-ttu-id="07869-178">ASP.NET Core sahip için yerleşik destek [bağımlılık ekleme (dı)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="07869-178">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="07869-179">ASP.NET Core mvc'de [denetleyicileri](controllers/dependency-injection.md) istek gerekli hizmetleri izlemek vermeden kendi oluşturucular aracılığıyla [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="07869-179">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="07869-180">Uygulamanızı de kullanabilirsiniz [bağımlılık ekleme görünümünde dosyaları](views/dependency-injection.md)kullanarak `@inject` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="07869-180">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="07869-181">FilTReleri</span><span class="sxs-lookup"><span data-stu-id="07869-181">Filters</span></span>

<span data-ttu-id="07869-182">[Filtreler](controllers/filters.md) geliştiricilerin, özel durum işleme veya yetkilendirme gibi çapraz kesme sorunlarını yalıtma.</span><span class="sxs-lookup"><span data-stu-id="07869-182">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="07869-183">Filtreleri eylem yöntemleri için çalışmakta olan özel öncesi ve sonrası işleme mantığı etkinleştirin ve belirli bir istek için yürütme ardışık düzeni içinde belirli zamanlarda çalışacak şekilde yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="07869-183">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="07869-184">Filtreler denetleyicileri veya öznitelik olarak Eylemler uygulanabilir (veya genel olarak çalıştırılabilir).</span><span class="sxs-lookup"><span data-stu-id="07869-184">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="07869-185">Birkaç filtreleri (gibi `Authorize`) framework dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="07869-185">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="07869-186">Alanları</span><span class="sxs-lookup"><span data-stu-id="07869-186">Areas</span></span>

<span data-ttu-id="07869-187">[Alanları](controllers/areas.md) daha küçük işlevsel gruplamalarda büyük bir ASP.NET Core MVC Web uygulaması bölüm için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="07869-187">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="07869-188">Bir uygulama içinde MVC yapısının bir alandır.</span><span class="sxs-lookup"><span data-stu-id="07869-188">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="07869-189">MVC projesinde, Model, denetleyici ve görünüm gibi mantıksal bileşenlerin farklı klasörlerde tutulur ve MVC bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır.</span><span class="sxs-lookup"><span data-stu-id="07869-189">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="07869-190">Büyük bir uygulama için uygulama ayrı yüksek düzey alanlarına işlevlerin bölümlemek için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="07869-190">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="07869-191">Örneğin, bir e-ticaret uygulamayla checkout, faturalama ve arama vb. gibi birden çok iş birimleri. Her bu birimleri kendi mantıksal bileşen görünümleri, denetleyicileri ve modeli vardır.</span><span class="sxs-lookup"><span data-stu-id="07869-191">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="07869-192">Web API'leri</span><span class="sxs-lookup"><span data-stu-id="07869-192">Web APIs</span></span>

<span data-ttu-id="07869-193">Web siteleri oluşturmak için harika bir platform olmaya ek olarak, ASP.NET Core MVC Web API'ları oluşturmak için harika desteğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="07869-193">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="07869-194">Tarayıcılar ve mobil cihazlar dahil olmak üzere istemcileri geniş bir yelpazedeki ulaşmak hizmetler oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="07869-194">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="07869-195">HTTP İçerik anlaşma desteği için yerleşik destek ile framework içeren [veri biçimlendirme](models/formatting.md) JSON veya XML olarak.</span><span class="sxs-lookup"><span data-stu-id="07869-195">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="07869-196">Yazma [özel biçimlendiricileri](advanced/custom-formatters.md) kendi biçimleri için destek eklemek için.</span><span class="sxs-lookup"><span data-stu-id="07869-196">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="07869-197">Bağlantı oluşturma iletilir desteğini etkinleştirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="07869-197">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="07869-198">Kolayca desteğini etkinleştirme [çıkış noktaları arası kaynak paylaşımı (CORS)](http://www.w3.org/TR/cors/) böylece Web API'leri birden çok Web uygulamaları arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="07869-198">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="07869-199">Test Edilebilirlik</span><span class="sxs-lookup"><span data-stu-id="07869-199">Testability</span></span>

<span data-ttu-id="07869-200">Framework'ün kullanımını arabirimleri ve bağımlılık ekleme kılmaktadır birim testi ve framework (gibi bir TestHost ve Inmemory sağlayıcısı için Entity Framework) olun özellikler içerir [tümleştirme sınaması](../testing/integration-testing.md) hızlı ve kolay de.</span><span class="sxs-lookup"><span data-stu-id="07869-200">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="07869-201">Daha fazla bilgi edinmek [denetleyicisi mantığının test edilmesi](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="07869-201">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="07869-202">Razor görüntüleme altyapısı</span><span class="sxs-lookup"><span data-stu-id="07869-202">Razor view engine</span></span>

<span data-ttu-id="07869-203">[ASP.NET Core MVC görünümleri](views/overview.md) kullanmak [Razor görüntüleme altyapısı](views/razor.md) görünümlerini işlemek için.</span><span class="sxs-lookup"><span data-stu-id="07869-203">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="07869-204">Razor, katıştırılmış C# kod kullanarak görünümleri tanımlamak için bir compact, açıklayıcı ve sıvı şablon biçimlendirme dilidir.</span><span class="sxs-lookup"><span data-stu-id="07869-204">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="07869-205">Razor web içeriği sunucusundaki dinamik olarak oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="07869-205">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="07869-206">Sunucu kodu düzgün bir şekilde, istemci tarafı içerik ve kod da karıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="07869-206">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="07869-207">Tanımlayabilirsiniz Razor görünüm altyapısını kullanarak [düzenleri](views/layout.md), [kısmi görünümler](views/partial.md) ve değiştirebilen bölümler.</span><span class="sxs-lookup"><span data-stu-id="07869-207">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="07869-208">Kesin türü belirtilmiş görünümleri</span><span class="sxs-lookup"><span data-stu-id="07869-208">Strongly typed views</span></span>

<span data-ttu-id="07869-209">MVC Razor görünümleri kesinlikle modelinizi göre yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="07869-209">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="07869-210">Denetleyicileri kesin türü belirtilmiş bir model türü denetleme ve IntelliSense desteği sağlamak kendi görünümlerinizi etkinleştirme görünümlerine geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="07869-210">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="07869-211">Örneğin, bir model türü aşağıdaki görünümü işleyen `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="07869-211">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="07869-212">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="07869-212">Tag Helpers</span></span>

<span data-ttu-id="07869-213">[Etiket Yardımcıları](views/tag-helpers/intro.md) sunucu tarafı kodu oluşturma ve Razor dosyalarında HTML öğelerin işlenmesi katılmayı etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="07869-213">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="07869-214">Özel etiketler tanımlamak için etiket Yardımcıları kullanabilirsiniz (örneğin, `<environment>`) veya varolan etiketleri davranışını değiştirmek için (örneğin, `<label>`).</span><span class="sxs-lookup"><span data-stu-id="07869-214">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="07869-215">Öğe adı ve özniteliklerini temel alınarak belirli öğelere etiket Yardımcıları bağlayın.</span><span class="sxs-lookup"><span data-stu-id="07869-215">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="07869-216">Düzenleme deneyimi HTML hala korurken sunucu tarafı işleme yararları sağlarlar.</span><span class="sxs-lookup"><span data-stu-id="07869-216">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="07869-217">Formları, bağlantılar, yükleme varlıklar ve ortak GitHub depoları ve NuGet olarak daha kullanılabilir ve daha fazla - paketleri oluşturma gibi genel görevleri - birçok yerleşik etiket Yardımcılarında vardır.</span><span class="sxs-lookup"><span data-stu-id="07869-217">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="07869-218">Etiket Yardımcıları C# dilinde yazılan ve öğe adı, öznitelik adı veya üst etiket göre HTML öğeleri hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="07869-218">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="07869-219">Örneğin, LinkTagHelper, bir bağlantı oluşturmak için kullanılabilir yerleşik `Login` eylemi `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="07869-219">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="07869-220">`EnvironmentTagHelper` Farklı betikleri geliştirme, hazırlama ve üretim gibi çalışma zamanı ortamı göre kendi görünümlerinizi (örneğin, ham veya küçültülmüş) dahil etmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="07869-220">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

<span data-ttu-id="07869-221">Etiket Yardımcıları bir HTML kolay geliştirme deneyimi ve HTML ve Razor biçimlendirmesi oluşturmak için zengin bir IntelliSense ortam sağlar.</span><span class="sxs-lookup"><span data-stu-id="07869-221">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="07869-222">Yerleşik etiket Yardımcıları çoğu olan HTML öğeleri hedef ve öğe için sunucu tarafı öznitelikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="07869-222">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="07869-223">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="07869-223">View Components</span></span>

<span data-ttu-id="07869-224">[Görüntüleme bileşenleri](views/view-components.md) işleme mantığı paketini ve uygulama genelinde yeniden olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="07869-224">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="07869-225">Benzer şekilde [kısmi görünümler](views/partial.md), ancak ilişkili mantığı ile.</span><span class="sxs-lookup"><span data-stu-id="07869-225">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
