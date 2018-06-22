---
title: ASP.NET Core MVC’ye Genel Bakış
author: ardalis
description: ASP.NET Core MVC web uygulamaları oluşturmak için zengin bir çerçeve nasıl olduğunu öğrenin ve Model-View-Controller kullanarak API'leri düzeni tasarlayın.
ms.author: riande
ms.date: 01/08/2018
uid: mvc/overview
ms.openlocfilehash: aca34f91e8c7efaa34263ddf830b1662a2518969
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272598"
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="260cf-103">ASP.NET Core MVC’ye Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="260cf-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="260cf-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="260cf-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="260cf-105">ASP.NET Core MVC web uygulamaları oluşturmak için zengin bir çerçeve olan ve Model-View-Controller kullanarak API'leri düzeni tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="260cf-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="260cf-106">MVC modeli nedir?</span><span class="sxs-lookup"><span data-stu-id="260cf-106">What is the MVC pattern?</span></span>

<span data-ttu-id="260cf-107">Model-View-Controller (MVC) tasarım örüntüsü bir uygulama bileşenlerinin üç ana gruplara ayırır: modeller, görünümler ve denetleyiciler.</span><span class="sxs-lookup"><span data-stu-id="260cf-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="260cf-108">Elde etmek için bu deseni yardımcı [sorunları ayrılması](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="260cf-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="260cf-109">Bu deseni kullanarak, kullanıcı isteklerini kullanıcı eylemleri gerçekleştirmek ve/veya sorgu sonuçları almak için modeli ile çalışmak için sorumlu denetleyicisi yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="260cf-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="260cf-110">Denetleyici kullanıcıya gösterilmesi için görüntüyü seçer ve gerektirdiği herhangi bir Model veri ile sağlar.</span><span class="sxs-lookup"><span data-stu-id="260cf-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="260cf-111">Aşağıdaki diyagramda üç ana bileşeni gösterir ve hangilerinin diğer başvuru:</span><span class="sxs-lookup"><span data-stu-id="260cf-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![MVC örüntüsü](overview/_static/mvc.png)

<span data-ttu-id="260cf-113">Bu kabul edilebilir açıklıkta sorumlulukları karmaşıklık açısından uygulama kodu, hata ayıklama ve bir şey (model, görünüm veya denetleyicisi) tek bir işin test etmeyi daha kolay olduğundan ölçek yardımcı olur (ve izleyen [tek sorumluluk İlkesi ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="260cf-113">This delineation of responsibilities helps you scale the application in terms of complexity because it's easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="260cf-114">Bu güncelleştirme, test ve iki veya daha fazla üç bu alanlar arasında yayılan bağımlılıkları olan hata ayıklama kodu daha zordur.</span><span class="sxs-lookup"><span data-stu-id="260cf-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="260cf-115">Örneğin, kullanıcı arabirimi mantığı, iş mantığı daha sık değiştirmek için eğilimi gösterir.</span><span class="sxs-lookup"><span data-stu-id="260cf-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="260cf-116">Sunu kodu ve iş mantığı tek bir nesnede birleştirildiğinde, kullanıcı arabirimi her değiştiğinde iş mantığı içeren bir nesne değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="260cf-116">If presentation code and business logic are combined in a single object, an object containing business logic must be modified every time the user interface is changed.</span></span> <span data-ttu-id="260cf-117">Bu genellikle hataları tanıtır ve iş mantığını her en az kullanıcı arabirimi değişiklikten sonra retesting gerektirir.</span><span class="sxs-lookup"><span data-stu-id="260cf-117">This often introduces errors and requires the retesting of business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="260cf-118">Görünüm ve denetleyici modeline bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="260cf-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="260cf-119">Ancak, model, görünüm ne denetleyicisi bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="260cf-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="260cf-120">Bu, avantajları ayırma biridir.</span><span class="sxs-lookup"><span data-stu-id="260cf-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="260cf-121">Bu ayrım oluşturulur ve test modeli görsel sunumu bağımsız olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="260cf-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="260cf-122">Model sorumlulukları</span><span class="sxs-lookup"><span data-stu-id="260cf-122">Model Responsibilities</span></span>

<span data-ttu-id="260cf-123">Modelin MVC uygulamasındaki uygulama ve iş mantığı veya tarafından gerçekleştirilmesi gereken işlemleri durumunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="260cf-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="260cf-124">İş mantığı modelde uygulama durumunu sürdürmek için tüm uygulama mantığı ile birlikte kapsüllenmiş.</span><span class="sxs-lookup"><span data-stu-id="260cf-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="260cf-125">Kesin türü belirtilmiş görünümleri, verileri içerecek şekilde tasarlanmış ViewModel türleri genellikle bu görünümde göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="260cf-125">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="260cf-126">Denetleyici oluşturur ve bu ViewModel örnekler modelden doldurur.</span><span class="sxs-lookup"><span data-stu-id="260cf-126">The controller creates and populates these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="260cf-127">MVC tasarım örüntüsü kullanan bir uygulama modelinde düzenlemek için birçok yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="260cf-127">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="260cf-128">Bazı hakkında daha fazla bilgi [model türlerinin farklı türde](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="260cf-128">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="260cf-129">Görünüm sorumlulukları</span><span class="sxs-lookup"><span data-stu-id="260cf-129">View Responsibilities</span></span>

<span data-ttu-id="260cf-130">Görünümler, kullanıcı arabirimi aracılığıyla içerik sunmak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="260cf-130">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="260cf-131">Kullandıkları [Razor görüntüleme altyapısı](#razor-view-engine) .NET kodu HTML biçimlendirmesini katıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="260cf-131">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="260cf-132">Görünümler içinde en az mantığının olmalıdır ve bunlara herhangi bir mantık içerik sunmak için ilişkili olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="260cf-132">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="260cf-133">Büyük bir bölümünü mantığı görünümde gerçekleştirmek için gereken dosyaları bir karmaşık modeli verileri görüntülemek için bulursanız, kullanmayı bir [görünümü bileşen](views/view-components.md), ViewModel, ya da görünümü basitleştirmek için şablonu görüntüle.</span><span class="sxs-lookup"><span data-stu-id="260cf-133">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="260cf-134">Denetleyici sorumlulukları</span><span class="sxs-lookup"><span data-stu-id="260cf-134">Controller Responsibilities</span></span>

<span data-ttu-id="260cf-135">Denetleyicileri kullanıcı etkileşimini işleyen, modelle çalışan ve sonuç olarak işlemek için bir görünüm seçin bileşenleridir.</span><span class="sxs-lookup"><span data-stu-id="260cf-135">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="260cf-136">Bir MVC uygulamasında görünüm yalnızca bilgileri görüntüler; Denetleyici işler ve kullanıcı girişini ve etkileşimini yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="260cf-136">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="260cf-137">MVC örüntüsü denetleyicisi ilk giriş noktası ve hangi modelle çalışmak için türleri ve işlemek için hangi görünüm seçmek için sorumludur (Bu nedenle, ad - denetimleri uygulama için belirtilen bir isteğin nasıl yanıt vereceğini).</span><span class="sxs-lookup"><span data-stu-id="260cf-137">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="260cf-138">Tarafından çok sayıda sorumlulukları denetleyicileri aşırı karmaşık döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="260cf-138">Controllers shouldn't be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="260cf-139">Denetleyici mantığında fazla karmaşık hale gelmesini tutmak için kullanın [tek sorumluluk ilkesine](http://deviq.com/single-responsibility-principle/) İtme iş mantığı denetleyicisi dışında ve etki alanı modeline.</span><span class="sxs-lookup"><span data-stu-id="260cf-139">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="260cf-140">Denetleyici eylemleri sık aynı tür Eylemler gerçekleştirir bulursanız, takip edebilirsiniz [kendiniz ilkesine yineleme](http://deviq.com/don-t-repeat-yourself/) bu ortak eylemlere taşıyarak [filtreleri](#filters).</span><span class="sxs-lookup"><span data-stu-id="260cf-140">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="260cf-141">ASP.NET Core MVC nedir</span><span class="sxs-lookup"><span data-stu-id="260cf-141">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="260cf-142">ASP.NET Core MVC çerçevesi bir hafif, açık kaynaklı, ASP.NET Core ile kullanmak için en iyi duruma getirilmiş yüksek düzeyde sınanabilir bir sunu çerçevesidir ' dir.</span><span class="sxs-lookup"><span data-stu-id="260cf-142">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="260cf-143">ASP.NET Core MVC sorunları temiz ayrılması sağlayan dinamik Web siteleri oluşturmak için desenleri dayanan bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="260cf-143">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="260cf-144">Biçimlendirme üzerinde tam denetim verir, TDD kolay geliştirme destekler ve en son web standartlarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="260cf-144">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="260cf-145">Özellikler</span><span class="sxs-lookup"><span data-stu-id="260cf-145">Features</span></span>

<span data-ttu-id="260cf-146">ASP.NET Core MVC aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="260cf-146">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="260cf-147">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="260cf-147">Routing</span></span>](#routing)
* [<span data-ttu-id="260cf-148">Model bağlama</span><span class="sxs-lookup"><span data-stu-id="260cf-148">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="260cf-149">Model doğrulaması</span><span class="sxs-lookup"><span data-stu-id="260cf-149">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="260cf-150">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="260cf-150">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="260cf-151">Filtreler</span><span class="sxs-lookup"><span data-stu-id="260cf-151">Filters</span></span>](#filters)
* [<span data-ttu-id="260cf-152">Alanlar</span><span class="sxs-lookup"><span data-stu-id="260cf-152">Areas</span></span>](#areas)
* [<span data-ttu-id="260cf-153">Web API'leri</span><span class="sxs-lookup"><span data-stu-id="260cf-153">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="260cf-154">Test Edilebilirlik</span><span class="sxs-lookup"><span data-stu-id="260cf-154">Testability</span></span>](#testability)
* [<span data-ttu-id="260cf-155">Razor görüntüleme altyapısı</span><span class="sxs-lookup"><span data-stu-id="260cf-155">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="260cf-156">Kesin türü belirtilmiş görünümleri</span><span class="sxs-lookup"><span data-stu-id="260cf-156">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="260cf-157">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="260cf-157">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="260cf-158">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="260cf-158">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="260cf-159">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="260cf-159">Routing</span></span>

<span data-ttu-id="260cf-160">ASP.NET Core MVC üstünde oluşturulan [ASP.NET Core'nın Yönlendirme](../fundamentals/routing.md), olanak sağlayan güçlü bir URL eşlemesi bileşeni anlaşılabilir ve aranabilir URL'lere sahip uygulamalar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="260cf-160">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="260cf-161">Bu, web sunucusundaki dosyaları nasıl düzenlendiği için bakmadan bağlantı oluşturma ve arama motoru iyileştirme (SEO) için iyi iş uygulamanızın URL adlandırma modelleri tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="260cf-161">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="260cf-162">Rota değeri kısıtlamaları, Varsayılanları ve isteğe bağlı değerler destekler uygun rota şablonu sözdizimini kullanarak yollarınızı tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="260cf-162">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="260cf-163">*Kurala dayalı yönlendirme* URL genel tanımlamanızı biçimleri etkinleştirir, uygulamanızın kabul eder ve nasıl görüntülerin her birini biçimleri bir özel eylem yöntemine üzerinde denetleyicisi eşler.</span><span class="sxs-lookup"><span data-stu-id="260cf-163">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="260cf-164">Gelen bir istek alındığında yönlendirme altyapısını URL ayrıştırır ve tanımlanan URL biçimlerinden birini için eşleşen ve ilişkili denetleyicinin eylem yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="260cf-164">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="260cf-165">*Öznitelik yönlendirme* denetleyicileri ve eylemleri uygulamanızın yolları tanımlayın özniteliklere sahip tasarlayarak yönlendirme bilgilerini belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="260cf-165">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="260cf-166">Başka bir deyişle, denetleyici ve eylem ile ilişkili oldukları yanındaki route tanımlarını yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="260cf-166">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

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

### <a name="model-binding"></a><span data-ttu-id="260cf-167">Model bağlama</span><span class="sxs-lookup"><span data-stu-id="260cf-167">Model binding</span></span>

<span data-ttu-id="260cf-168">ASP.NET Core MVC [model bağlama](models/model-binding.md) denetleyicisi işleyebileceği nesnelerini istemci isteği verilerini (form değerleri, rota verileri, sorgu dizesi parametreleri, HTTP üstbilgileri) dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="260cf-168">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="260cf-169">Sonuç olarak, denetleyici mantığınızı gelen istek verileri getirme işini yapmak zorunda değildir; yalnızca kendi eylem yöntemleri için parametre olarak veri yok.</span><span class="sxs-lookup"><span data-stu-id="260cf-169">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="260cf-170">Model doğrulama</span><span class="sxs-lookup"><span data-stu-id="260cf-170">Model validation</span></span>

<span data-ttu-id="260cf-171">ASP.NET Core MVC destekleyen [doğrulama](models/validation.md) , model nesnesi veri ek açıklaması doğrulama öznitelikleri ile dekorasyon tarafından.</span><span class="sxs-lookup"><span data-stu-id="260cf-171">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="260cf-172">Doğrulama özniteliklerinin değerleri sunucuya gönderilen önce istemci tarafında denetlenir yanı denetleyici eylemini önce sunucuda olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="260cf-172">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

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

<span data-ttu-id="260cf-173">Bir denetleyici eylemi:</span><span class="sxs-lookup"><span data-stu-id="260cf-173">A controller action:</span></span>

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

<span data-ttu-id="260cf-174">Framework doğrulama isteği verileri istemci ve sunucu işler.</span><span class="sxs-lookup"><span data-stu-id="260cf-174">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="260cf-175">Model türlerinde belirtilen doğrulama mantığını oluşturulmuş görünümler örtük ek açıklamaları olarak eklenir ve tarayıcı ile zorlanır [jQuery doğrulama](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="260cf-175">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="260cf-176">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="260cf-176">Dependency injection</span></span>

<span data-ttu-id="260cf-177">ASP.NET Core sahip için yerleşik destek [bağımlılık ekleme (dı)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="260cf-177">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="260cf-178">ASP.NET Core mvc'de [denetleyicileri](controllers/dependency-injection.md) istek gerekli hizmetleri izlemek vermeden kendi oluşturucular aracılığıyla [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="260cf-178">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="260cf-179">Uygulamanızı de kullanabilirsiniz [bağımlılık ekleme görünümünde dosyaları](views/dependency-injection.md)kullanarak `@inject` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="260cf-179">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

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

### <a name="filters"></a><span data-ttu-id="260cf-180">FilTReleri</span><span class="sxs-lookup"><span data-stu-id="260cf-180">Filters</span></span>

<span data-ttu-id="260cf-181">[Filtreler](controllers/filters.md) geliştiricilerin, özel durum işleme veya yetkilendirme gibi çapraz kesme sorunlarını yalıtma.</span><span class="sxs-lookup"><span data-stu-id="260cf-181">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="260cf-182">Filtreleri eylem yöntemleri için çalışmakta olan özel öncesi ve sonrası işleme mantığı etkinleştirin ve belirli bir istek için yürütme ardışık düzeni içinde belirli zamanlarda çalışacak şekilde yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="260cf-182">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="260cf-183">Filtreler denetleyicileri veya öznitelik olarak Eylemler uygulanabilir (veya genel olarak çalıştırılabilir).</span><span class="sxs-lookup"><span data-stu-id="260cf-183">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="260cf-184">Birkaç filtreleri (gibi `Authorize`) framework dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="260cf-184">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="260cf-185">Alanları</span><span class="sxs-lookup"><span data-stu-id="260cf-185">Areas</span></span>

<span data-ttu-id="260cf-186">[Alanları](controllers/areas.md) daha küçük işlevsel gruplamalarda büyük bir ASP.NET Core MVC Web uygulaması bölüm için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="260cf-186">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="260cf-187">Bir uygulama içinde MVC yapısının bir alandır.</span><span class="sxs-lookup"><span data-stu-id="260cf-187">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="260cf-188">MVC projesinde, Model, denetleyici ve görünüm gibi mantıksal bileşenlerin farklı klasörlerde tutulur ve MVC bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır.</span><span class="sxs-lookup"><span data-stu-id="260cf-188">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="260cf-189">Büyük bir uygulama için uygulama ayrı yüksek düzey alanlarına işlevlerin bölümlemek için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="260cf-189">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="260cf-190">Örneğin, bir e-ticaret uygulamayla checkout, faturalama ve arama vb. gibi birden çok iş birimleri. Her bu birimleri kendi mantıksal bileşen görünümleri, denetleyicileri ve modeli vardır.</span><span class="sxs-lookup"><span data-stu-id="260cf-190">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="260cf-191">Web API'leri</span><span class="sxs-lookup"><span data-stu-id="260cf-191">Web APIs</span></span>

<span data-ttu-id="260cf-192">Web siteleri oluşturmak için harika bir platform olmaya ek olarak, ASP.NET Core MVC Web API'ları oluşturmak için harika desteğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="260cf-192">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="260cf-193">Tarayıcılar ve mobil cihazlar dahil olmak üzere istemcileri geniş bir yelpazedeki ulaşmak hizmetler oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="260cf-193">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="260cf-194">HTTP İçerik anlaşma desteği için yerleşik destekle framework içeren [biçimlendirmek veri](xref:web-api/advanced/formatting) JSON veya XML olarak.</span><span class="sxs-lookup"><span data-stu-id="260cf-194">The framework includes support for HTTP content-negotiation with built-in support to [format data](xref:web-api/advanced/formatting) as JSON or XML.</span></span> <span data-ttu-id="260cf-195">Yazma [özel biçimlendiricileri](xref:web-api/advanced/custom-formatters) kendi biçimleri için destek eklemek için.</span><span class="sxs-lookup"><span data-stu-id="260cf-195">Write [custom formatters](xref:web-api/advanced/custom-formatters) to add support for your own formats.</span></span>

<span data-ttu-id="260cf-196">Bağlantı oluşturma iletilir desteğini etkinleştirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="260cf-196">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="260cf-197">Kolayca desteğini etkinleştirme [çıkış noktaları arası kaynak paylaşımı (CORS)](http://www.w3.org/TR/cors/) böylece Web API'leri birden çok Web uygulamaları arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="260cf-197">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="260cf-198">Test Edilebilirlik</span><span class="sxs-lookup"><span data-stu-id="260cf-198">Testability</span></span>

<span data-ttu-id="260cf-199">Framework'ün kullanımını arabirimleri ve bağımlılık ekleme kılmaktadır birim testi ve framework (gibi bir TestHost ve Inmemory sağlayıcısı için Entity Framework) olun özellikler içerir [tümleştirme testleri](xref:test/integration-tests) hızlı ve kolay de.</span><span class="sxs-lookup"><span data-stu-id="260cf-199">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration tests](xref:test/integration-tests) quick and easy as well.</span></span> <span data-ttu-id="260cf-200">Daha fazla bilgi edinmek [denetleyici mantığında test etme](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="260cf-200">Learn more about [how to test controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="260cf-201">Razor görüntüleme altyapısı</span><span class="sxs-lookup"><span data-stu-id="260cf-201">Razor view engine</span></span>

<span data-ttu-id="260cf-202">[ASP.NET Core MVC görünümleri](views/overview.md) kullanmak [Razor görüntüleme altyapısı](views/razor.md) görünümlerini işlemek için.</span><span class="sxs-lookup"><span data-stu-id="260cf-202">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="260cf-203">Razor, katıştırılmış C# kod kullanarak görünümleri tanımlamak için bir compact, açıklayıcı ve sıvı şablon biçimlendirme dilidir.</span><span class="sxs-lookup"><span data-stu-id="260cf-203">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="260cf-204">Razor web içeriği sunucusundaki dinamik olarak oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="260cf-204">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="260cf-205">Sunucu kodu düzgün bir şekilde, istemci tarafı içerik ve kod da karıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="260cf-205">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="260cf-206">Tanımlayabilirsiniz Razor görünüm altyapısını kullanarak [düzenleri](views/layout.md), [kısmi görünümler](views/partial.md) ve değiştirebilen bölümler.</span><span class="sxs-lookup"><span data-stu-id="260cf-206">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="260cf-207">Kesin türü belirtilmiş görünümleri</span><span class="sxs-lookup"><span data-stu-id="260cf-207">Strongly typed views</span></span>

<span data-ttu-id="260cf-208">MVC Razor görünümleri kesinlikle modelinizi göre yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="260cf-208">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="260cf-209">Denetleyicileri kesin türü belirtilmiş bir model türü denetleme ve IntelliSense desteği sağlamak kendi görünümlerinizi etkinleştirme görünümlerine geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="260cf-209">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="260cf-210">Örneğin, bir model türü aşağıdaki görünümü işleyen `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="260cf-210">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="260cf-211">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="260cf-211">Tag Helpers</span></span>

<span data-ttu-id="260cf-212">[Etiket Yardımcıları](views/tag-helpers/intro.md) sunucu tarafı kodu oluşturma ve Razor dosyalarında HTML öğelerin işlenmesi katılmayı etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="260cf-212">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="260cf-213">Özel etiketler tanımlamak için etiket Yardımcıları kullanabilirsiniz (örneğin, `<environment>`) veya varolan etiketleri davranışını değiştirmek için (örneğin, `<label>`).</span><span class="sxs-lookup"><span data-stu-id="260cf-213">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="260cf-214">Öğe adı ve özniteliklerini temel alınarak belirli öğelere etiket Yardımcıları bağlayın.</span><span class="sxs-lookup"><span data-stu-id="260cf-214">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="260cf-215">Düzenleme deneyimi HTML hala korurken sunucu tarafı işleme yararları sağlarlar.</span><span class="sxs-lookup"><span data-stu-id="260cf-215">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="260cf-216">Formları, bağlantılar, yükleme varlıklar ve ortak GitHub depoları ve NuGet olarak daha kullanılabilir ve daha fazla - paketleri oluşturma gibi genel görevleri - birçok yerleşik etiket Yardımcılarında vardır.</span><span class="sxs-lookup"><span data-stu-id="260cf-216">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="260cf-217">Etiket Yardımcıları C# dilinde yazılan ve öğe adı, öznitelik adı veya üst etiket göre HTML öğeleri hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="260cf-217">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="260cf-218">Örneğin, LinkTagHelper, bir bağlantı oluşturmak için kullanılabilir yerleşik `Login` eylemi `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="260cf-218">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="260cf-219">`EnvironmentTagHelper` Farklı betikleri geliştirme, hazırlama ve üretim gibi çalışma zamanı ortamı göre kendi görünümlerinizi (örneğin, ham veya küçültülmüş) dahil etmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="260cf-219">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

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

<span data-ttu-id="260cf-220">Etiket Yardımcıları bir HTML kolay geliştirme deneyimi ve HTML ve Razor biçimlendirmesi oluşturmak için zengin bir IntelliSense ortam sağlar.</span><span class="sxs-lookup"><span data-stu-id="260cf-220">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="260cf-221">Yerleşik etiket Yardımcıları çoğu olan HTML öğeleri hedef ve öğe için sunucu tarafı öznitelikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="260cf-221">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="260cf-222">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="260cf-222">View Components</span></span>

<span data-ttu-id="260cf-223">[Görüntüleme bileşenleri](views/view-components.md) işleme mantığı paketini ve uygulama genelinde yeniden olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="260cf-223">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="260cf-224">Benzer şekilde [kısmi görünümler](views/partial.md), ancak ilişkili mantığı ile.</span><span class="sxs-lookup"><span data-stu-id="260cf-224">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
