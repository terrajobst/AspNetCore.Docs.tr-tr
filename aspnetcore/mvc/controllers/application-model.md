---
title: ASP.NET Core uygulama modelinde ile çalışma
author: ardalis
description: Okuma ve ASP.NET Core MVC öğeleri nasıl davranacağını değiştirmek için uygulama modelle öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/application-model
ms.openlocfilehash: f61d04f6cf0aa054566d9f48a030cf268f2ba72a
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="work-with-the-application-model-in-aspnet-core"></a><span data-ttu-id="faaa4-103">ASP.NET Core uygulama modelinde ile çalışma</span><span class="sxs-lookup"><span data-stu-id="faaa4-103">Work with the application model in ASP.NET Core</span></span>

<span data-ttu-id="faaa4-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="faaa4-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="faaa4-105">ASP.NET Core MVC tanımlayan bir *uygulama modeli* bir MVC uygulamanın bileşenleri temsil eden.</span><span class="sxs-lookup"><span data-stu-id="faaa4-105">ASP.NET Core MVC defines an *application model* representing the components of an MVC app.</span></span> <span data-ttu-id="faaa4-106">Okuma ve MVC öğeleri nasıl davranacağını değiştirmek için Bu modelle.</span><span class="sxs-lookup"><span data-stu-id="faaa4-106">You can read and manipulate this model to modify how MVC elements behave.</span></span> <span data-ttu-id="faaa4-107">Varsayılan olarak, MVC parametreleri ve yönlendirme nasıl davranacağını hangi sınıfların denetleyicileri olduğu kabul edilir ve bu sınıfların hangi yöntemlere eylemlerdir belirlemek için belirli kuralları izler.</span><span class="sxs-lookup"><span data-stu-id="faaa4-107">By default, MVC follows certain conventions to determine which classes are considered to be controllers, which methods on those classes are actions, and how parameters and routing behave.</span></span> <span data-ttu-id="faaa4-108">Bu davranış, kendi kuralları oluşturarak ve bunları genel olarak veya öznitelik olarak uygulayarak, uygulamanızın gereksinimlerine uyacak şekilde özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="faaa4-108">You can customize this behavior to suit your app's needs by creating your own conventions and applying them globally or as attributes.</span></span>

## <a name="models-and-providers"></a><span data-ttu-id="faaa4-109">Modelleri ve sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="faaa4-109">Models and Providers</span></span>

<span data-ttu-id="faaa4-110">ASP.NET Core MVC uygulama modeli soyut arabirimleri ve bir MVC uygulaması açıklamak somut uygulama sınıfları içerir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-110">The ASP.NET Core MVC application model include both abstract interfaces and concrete implementation classes that describe an MVC application.</span></span> <span data-ttu-id="faaa4-111">Bu model uygulamanın denetleyicileri, Eylemler, eylem parametrelerini, yolları ve varsayılan kuralları göre filtreler keşfetme MVC sonucudur.</span><span class="sxs-lookup"><span data-stu-id="faaa4-111">This model is the result of MVC discovering the app's controllers, actions, action parameters, routes, and filters according to default conventions.</span></span> <span data-ttu-id="faaa4-112">Uygulama modeli çalışarak, uygulamanızın farklı kuralarına varsayılan MVC davranışı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="faaa4-112">By working with the application model, you can modify your app to follow different conventions from the default MVC behavior.</span></span> <span data-ttu-id="faaa4-113">Parametreler, adları, yollar ve filtreleri tüm eylemlerin ve denetleyicilerin için yapılandırma verileri olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-113">The parameters, names, routes, and filters are all used as configuration data for actions and controllers.</span></span>

<span data-ttu-id="faaa4-114">ASP.NET Core MVC uygulama modeli aşağıdaki yapıya sahiptir:</span><span class="sxs-lookup"><span data-stu-id="faaa4-114">The ASP.NET Core MVC Application Model has the following structure:</span></span>

* <span data-ttu-id="faaa4-115">ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="faaa4-115">ApplicationModel</span></span>
    * <span data-ttu-id="faaa4-116">Denetleyicileri (ControllerModel)</span><span class="sxs-lookup"><span data-stu-id="faaa4-116">Controllers (ControllerModel)</span></span>
        * <span data-ttu-id="faaa4-117">Eylemler (ActionModel)</span><span class="sxs-lookup"><span data-stu-id="faaa4-117">Actions (ActionModel)</span></span>
            * <span data-ttu-id="faaa4-118">Parametreler (ParameterModel)</span><span class="sxs-lookup"><span data-stu-id="faaa4-118">Parameters (ParameterModel)</span></span>

<span data-ttu-id="faaa4-119">Ortak bir modelin her düzeyi erişimi `Properties` koleksiyonu ve alt düzey erişmek ve hiyerarşideki üst düzeyler tarafından ayarlanan özellik değerlerini üzerine yazın.</span><span class="sxs-lookup"><span data-stu-id="faaa4-119">Each level of the model has access to a common `Properties` collection, and lower levels can access and overwrite property values set by higher levels in the hierarchy.</span></span> <span data-ttu-id="faaa4-120">Özellikler için kalıcı `ActionDescriptor.Properties` Eylemler ne zaman oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="faaa4-120">The properties are persisted to the `ActionDescriptor.Properties` when the actions are created.</span></span> <span data-ttu-id="faaa4-121">Bir istek işlenirken, ardından herhangi bir özellik eklenemez veya değiştirilemez bir kuralı aracılığıyla erişilebilir `ActionContext.ActionDescriptor.Properties`.</span><span class="sxs-lookup"><span data-stu-id="faaa4-121">Then when a request is being handled, any properties a convention added or modified can be accessed through `ActionContext.ActionDescriptor.Properties`.</span></span> <span data-ttu-id="faaa4-122">Özellikleri kullanarak, filtreleri, model bağlayıcıları, vb. bir eylem başına temelinde yapılandırmak için harika bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="faaa4-122">Using properties is a great way to configure your filters, model binders, etc. on a per-action basis.</span></span>

> [!NOTE]
> <span data-ttu-id="faaa4-123">`ActionDescriptor.Properties` Toplama, uygulama başlatma tamamlandıktan sonra iş parçacığı (yazar için) güvenli değil.</span><span class="sxs-lookup"><span data-stu-id="faaa4-123">The `ActionDescriptor.Properties` collection isn't thread safe (for writes) once app startup has finished.</span></span> <span data-ttu-id="faaa4-124">Kuralları, güvenli bir şekilde veri bu koleksiyona eklemek için en iyi yoludur.</span><span class="sxs-lookup"><span data-stu-id="faaa4-124">Conventions are the best way to safely add data to this collection.</span></span>

### <a name="iapplicationmodelprovider"></a><span data-ttu-id="faaa4-125">IApplicationModelProvider</span><span class="sxs-lookup"><span data-stu-id="faaa4-125">IApplicationModelProvider</span></span>

<span data-ttu-id="faaa4-126">ASP.NET Core MVC tarafından tanımlanan bir sağlayıcı desenini kullanarak uygulama modelini yükler [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="faaa4-126">ASP.NET Core MVC loads the application model using a provider pattern, defined by the [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface.</span></span> <span data-ttu-id="faaa4-127">Bu bölümde bazı nasıl iç uygulama ayrıntılarını kapsayan bu sağlayıcı işlevleri.</span><span class="sxs-lookup"><span data-stu-id="faaa4-127">This section covers some of the internal implementation details of how this provider functions.</span></span> <span data-ttu-id="faaa4-128">Bu ileri düzeyde bir konudur - uygulama modeli yararlanan çoğu uygulamaları kuralları ile çalışarak bunu yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-128">This is an advanced topic - most apps that leverage the application model should do so by working with conventions.</span></span>

<span data-ttu-id="faaa4-129">Uygulamaları `IApplicationModelProvider` arabirimi "kaydırma" birbirine, her uygulama arama ile `OnProvidersExecuting` göre artan kendi `Order` özelliği.</span><span class="sxs-lookup"><span data-stu-id="faaa4-129">Implementations of the `IApplicationModelProvider` interface "wrap" one another, with each implementation calling `OnProvidersExecuting` in ascending order based on its `Order` property.</span></span> <span data-ttu-id="faaa4-130">`OnProvidersExecuted` Yöntemi sonra çağrılır ters sırada.</span><span class="sxs-lookup"><span data-stu-id="faaa4-130">The `OnProvidersExecuted` method is then called in reverse order.</span></span> <span data-ttu-id="faaa4-131">Framework birkaç sağlayıcı tanımlar:</span><span class="sxs-lookup"><span data-stu-id="faaa4-131">The framework defines several providers:</span></span>

<span data-ttu-id="faaa4-132">İlk (`Order=-1000`):</span><span class="sxs-lookup"><span data-stu-id="faaa4-132">First (`Order=-1000`):</span></span>

* [`DefaultApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

<span data-ttu-id="faaa4-133">Sonra (`Order=-990`):</span><span class="sxs-lookup"><span data-stu-id="faaa4-133">Then (`Order=-990`):</span></span>

* [`AuthorizationApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> <span data-ttu-id="faaa4-134">Hangi iki sağlayıcıları için aynı değeri ile sırayla `Order` denir tanımlanmamış ve sonra bu nedenle dayanıyordu gerekir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-134">The order in which two providers with the same value for `Order` are called is undefined, and therefore shouldn't be relied upon.</span></span>

> [!NOTE]
> <span data-ttu-id="faaa4-135">`IApplicationModelProvider` framework yazarların genişletmek için Gelişmiş bir kavramdır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-135">`IApplicationModelProvider` is an advanced concept for framework authors to extend.</span></span> <span data-ttu-id="faaa4-136">Genel olarak, uygulama kuralları kullanmanız gerekir ve çerçeveleri sağlayıcıları kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-136">In general, apps should use conventions and frameworks should use providers.</span></span> <span data-ttu-id="faaa4-137">Anahtar fark sağlayıcıları her zaman önce kuralları çalıştırılan.</span><span class="sxs-lookup"><span data-stu-id="faaa4-137">The key distinction is that providers always run before conventions.</span></span>

<span data-ttu-id="faaa4-138">`DefaultApplicationModelProvider` ASP.NET Core MVC tarafından kullanılan varsayılan davranışlar çoğunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="faaa4-138">The `DefaultApplicationModelProvider` establishes many of the default behaviors used by ASP.NET Core MVC.</span></span> <span data-ttu-id="faaa4-139">Sorumlulukları içerir:</span><span class="sxs-lookup"><span data-stu-id="faaa4-139">Its responsibilities include:</span></span>

* <span data-ttu-id="faaa4-140">Genel filtre bağlamına ekleme</span><span class="sxs-lookup"><span data-stu-id="faaa4-140">Adding global filters to the context</span></span>
* <span data-ttu-id="faaa4-141">Bağlamına denetleyicileri ekleme</span><span class="sxs-lookup"><span data-stu-id="faaa4-141">Adding controllers to the context</span></span>
* <span data-ttu-id="faaa4-142">Eylem olarak ortak denetleyicisi yöntemler ekleme</span><span class="sxs-lookup"><span data-stu-id="faaa4-142">Adding public controller methods as actions</span></span>
* <span data-ttu-id="faaa4-143">Eylem yöntemi parametrelerine bağlamına ekleme</span><span class="sxs-lookup"><span data-stu-id="faaa4-143">Adding action method parameters to the context</span></span>
* <span data-ttu-id="faaa4-144">Yol ve diğer öznitelikleri uygulama</span><span class="sxs-lookup"><span data-stu-id="faaa4-144">Applying route and other attributes</span></span>

<span data-ttu-id="faaa4-145">Bazı yerleşik davranışları tarafından uygulanan `DefaultApplicationModelProvider`.</span><span class="sxs-lookup"><span data-stu-id="faaa4-145">Some built-in behaviors are implemented by the `DefaultApplicationModelProvider`.</span></span> <span data-ttu-id="faaa4-146">Bu sağlayıcı oluşturmaktan sorumlu [ `ControllerModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), sırayla başvuran [ `ActionModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), ve [ `ParameterModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) örnekleri.</span><span class="sxs-lookup"><span data-stu-id="faaa4-146">This provider is responsible for constructing the [`ControllerModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), which in turn references [`ActionModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [`PropertyModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), and [`ParameterModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instances.</span></span> <span data-ttu-id="faaa4-147">`DefaultApplicationModelProvider` Ve gelecekte değiştirecek bir iç framework uygulaması ayrıntı bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-147">The `DefaultApplicationModelProvider` class is an internal framework implementation detail that can and will change in the future.</span></span> 

<span data-ttu-id="faaa4-148">`AuthorizationApplicationModelProvider` İlişkili davranışı uygulamak için sorumlu `AuthorizeFilter` ve `AllowAnonymousFilter` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="faaa4-148">The `AuthorizationApplicationModelProvider` is responsible for applying the behavior associated with the `AuthorizeFilter` and `AllowAnonymousFilter` attributes.</span></span> <span data-ttu-id="faaa4-149">[Bu öznitelikler hakkında daha fazla bilgi](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="faaa4-149">[Learn more about these attributes](xref:security/authorization/simple).</span></span>

<span data-ttu-id="faaa4-150">`CorsApplicationModelProvider` İlişkili davranışı uygulayan `IEnableCorsAttribute` ve `IDisableCorsAttribute`ve `DisableCorsAuthorizationFilter`.</span><span class="sxs-lookup"><span data-stu-id="faaa4-150">The `CorsApplicationModelProvider` implements behavior associated with the `IEnableCorsAttribute` and `IDisableCorsAttribute`, and the `DisableCorsAuthorizationFilter`.</span></span> <span data-ttu-id="faaa4-151">[CORS hakkında daha fazla bilgi](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="faaa4-151">[Learn more about CORS](xref:security/cors).</span></span>

## <a name="conventions"></a><span data-ttu-id="faaa4-152">Kurallar</span><span class="sxs-lookup"><span data-stu-id="faaa4-152">Conventions</span></span>

<span data-ttu-id="faaa4-153">Uygulama modeli tüm model veya sağlayıcısı geçersiz kılma daha modelleri davranışını özelleştirmek için basit bir yol sağlamak kuralı soyutlamalar tanımlar.</span><span class="sxs-lookup"><span data-stu-id="faaa4-153">The application model defines convention abstractions that provide a simpler way to customize the behavior of the models than overriding the entire model or provider.</span></span> <span data-ttu-id="faaa4-154">Bu soyutlama, uygulamanızın davranışını değiştirmek için önerilen yoldur.</span><span class="sxs-lookup"><span data-stu-id="faaa4-154">These abstractions are the recommended way to modify your app's behavior.</span></span> <span data-ttu-id="faaa4-155">Kuralları özelleştirmeleri dinamik olarak uygulanacak kod yazmak bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="faaa4-155">Conventions provide a way for you to write code that will dynamically apply customizations.</span></span> <span data-ttu-id="faaa4-156">Sırada [filtreleri](xref:mvc/controllers/filters) framework'ün davranışını değiştirme sağladıkları, özelleştirmeleri nasıl tüm uygulama birlikte kablolu denetlemenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-156">While [filters](xref:mvc/controllers/filters) provide a means of modifying the framework's behavior, customizations let you control how the whole app is wired together.</span></span>

<span data-ttu-id="faaa4-157">Aşağıdaki kuralları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="faaa4-157">The following conventions are available:</span></span>

* [`IApplicationModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

<span data-ttu-id="faaa4-158">MVC seçenekler ekleyerek veya uygulama kuralları uygulanır `Attribute`s ve bunları denetleyicileri, Eylemler veya eylem parametrelerini uygulayarak (benzer şekilde [ `Filters` ](xref:mvc/controllers/filters)).</span><span class="sxs-lookup"><span data-stu-id="faaa4-158">Conventions are applied by adding them to MVC options or by implementing `Attribute`s and applying them to controllers, actions, or action parameters (similar to [`Filters`](xref:mvc/controllers/filters)).</span></span> <span data-ttu-id="faaa4-159">Uygulama başlıyor, her isteğin bir parçası değil, filtreleri farklı olarak, yalnızca kuralları çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-159">Unlike filters, conventions are only executed when the app is starting, not as part of each request.</span></span>

### <a name="sample-modifying-the-applicationmodel"></a><span data-ttu-id="faaa4-160">Örnek: ApplicationModel değiştirme</span><span class="sxs-lookup"><span data-stu-id="faaa4-160">Sample: Modifying the ApplicationModel</span></span>

<span data-ttu-id="faaa4-161">Aşağıdaki kural, bir özellik için uygulama modeli eklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-161">The following convention is used to add a property to the application model.</span></span> 

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

<span data-ttu-id="faaa4-162">Uygulama modeli kuralları, seçeneği olarak uygulanır, MVC eklendiğinde `ConfigureServices` içinde `Startup`.</span><span class="sxs-lookup"><span data-stu-id="faaa4-162">Application model conventions are applied as options when MVC is added in `ConfigureServices` in `Startup`.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

<span data-ttu-id="faaa4-163">Özellikler erişilebilir `ActionDescriptor` özellikler koleksiyonu içinde denetleyici eylemleri:</span><span class="sxs-lookup"><span data-stu-id="faaa4-163">Properties are accessible from the `ActionDescriptor` properties collection within controller actions:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a><span data-ttu-id="faaa4-164">Örnek: ControllerModel açıklama değiştirme</span><span class="sxs-lookup"><span data-stu-id="faaa4-164">Sample: Modifying the ControllerModel Description</span></span>

<span data-ttu-id="faaa4-165">Önceki örnekte olduğu gibi denetleyici modeline ayrıca özel özellikleri eklemek için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-165">As in the previous example, the controller model can also be modified to include custom properties.</span></span> <span data-ttu-id="faaa4-166">Bunlar mevcut özelliklerini uygulama modelde belirtilen aynı adda geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="faaa4-166">These will override existing properties with the same name specified in the application model.</span></span> <span data-ttu-id="faaa4-167">Aşağıdaki kural öznitelik denetleyici düzeyinde bir açıklama ekler:</span><span class="sxs-lookup"><span data-stu-id="faaa4-167">The following convention attribute adds a description at the controller level:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

<span data-ttu-id="faaa4-168">Bu kural, bir denetleyici özniteliği olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-168">This convention is applied as an attribute on a controller.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

<span data-ttu-id="faaa4-169">Önceki örneklerde olduğu gibi aynı şekilde "Açıklama" özelliği erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-169">The "description" property is accessed in the same manner as in previous examples.</span></span>

### <a name="sample-modifying-the-actionmodel-description"></a><span data-ttu-id="faaa4-170">Örnek: ActionModel açıklama değiştirme</span><span class="sxs-lookup"><span data-stu-id="faaa4-170">Sample: Modifying the ActionModel Description</span></span>

<span data-ttu-id="faaa4-171">Ayrı özniteliği kuralı ayrı Eylemler, uygulama veya denetleyici düzeyinde uygulanmış davranışı geçersiz kılma uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-171">A separate attribute convention can be applied to individual actions, overriding behavior already applied at the application or controller level.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

<span data-ttu-id="faaa4-172">Bu eylem önceki örnekteki denetleyicisi içinde uygulanan nasıl denetleyici düzeyinde kuralı geçersiz kılmaları gösterir:</span><span class="sxs-lookup"><span data-stu-id="faaa4-172">Applying this to an action within the previous example's controller demonstrates how it overrides the controller-level convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a><span data-ttu-id="faaa4-173">Örnek: ParameterModel değiştirme</span><span class="sxs-lookup"><span data-stu-id="faaa4-173">Sample: Modifying the ParameterModel</span></span>

<span data-ttu-id="faaa4-174">Aşağıdaki kural değiştirmek için eylem parametrelerini uygulanabilir kendi `BindingInfo`.</span><span class="sxs-lookup"><span data-stu-id="faaa4-174">The following convention can be applied to action parameters to modify their `BindingInfo`.</span></span> <span data-ttu-id="faaa4-175">Aşağıdaki kural parametresi bir rota parametresini olmalıdır; diğer olası bağlama kaynakları (örneğin, sorgu dizesi değerlerini) göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-175">The following convention requires that the parameter be a route parameter; other potential binding sources (such as query string values) are ignored.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

<span data-ttu-id="faaa4-176">Öznitelik için herhangi bir eylem parametre uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="faaa4-176">The attribute may be applied to any action parameter:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a><span data-ttu-id="faaa4-177">Örnek: ActionModel adını değiştirme</span><span class="sxs-lookup"><span data-stu-id="faaa4-177">Sample: Modifying the ActionModel Name</span></span>

<span data-ttu-id="faaa4-178">Aşağıdaki kural değiştirir `ActionModel` güncelleştirmek için *adı* onu Uygulanacağı eylem.</span><span class="sxs-lookup"><span data-stu-id="faaa4-178">The following convention modifies the `ActionModel` to update the *name* of the action to which it's applied.</span></span> <span data-ttu-id="faaa4-179">Yeni bir ad özniteliği için bir parametre olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-179">The new name is provided as a parameter to the attribute.</span></span> <span data-ttu-id="faaa4-180">Bu eylem yöntemine erişmek için kullanılan rota etkileyecek şekilde bu yeni ad yönlendirme tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-180">This new name is used by routing, so it will affect the route used to reach this action method.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

<span data-ttu-id="faaa4-181">Bu öznitelik, bir eylem yöntemine uygulanan `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="faaa4-181">This attribute is applied to an action method in the `HomeController`:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

<span data-ttu-id="faaa4-182">Yöntem adı olsa bile `SomeName`, özniteliğin yöntem adını kullanarak MVC kuralı geçersiz kılar ve eylem adı ile değiştirir `MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="faaa4-182">Even though the method name is `SomeName`, the attribute overrides the MVC convention of using the method name and replaces the action name with `MyCoolAction`.</span></span> <span data-ttu-id="faaa4-183">Bu nedenle, bu eylem erişmek için kullanılan rota olduğu `/Home/MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="faaa4-183">Thus, the route used to reach this action is `/Home/MyCoolAction`.</span></span>

> [!NOTE]
> <span data-ttu-id="faaa4-184">Bu örnekte temelde aynı yerleşik kullanmakla, [EylemAdı](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="faaa4-184">This example is essentially the same as using the built-in [ActionName](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) attribute.</span></span>

### <a name="sample-custom-routing-convention"></a><span data-ttu-id="faaa4-185">Örnek: Özel yönlendirme kuralı</span><span class="sxs-lookup"><span data-stu-id="faaa4-185">Sample: Custom Routing Convention</span></span>

<span data-ttu-id="faaa4-186">Kullanabileceğiniz bir `IApplicationModelConvention` yönlendirmenin nasıl çalıştığını özelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="faaa4-186">You can use an `IApplicationModelConvention` to customize how routing works.</span></span> <span data-ttu-id="faaa4-187">Örneğin, aşağıdaki kural denetleyicileri ad alanları değiştirerek kendi yollar dahil `.` ile ad alanında `/` rotadaki:</span><span class="sxs-lookup"><span data-stu-id="faaa4-187">For example, the following convention will incorporate Controllers' namespaces into their routes, replacing `.` in the namespace with `/` in the route:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

<span data-ttu-id="faaa4-188">Kuralı, bir başlatma seçeneği olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-188">The convention is added as an option in Startup.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> <span data-ttu-id="faaa4-189">Kurallarına ekleyebilirsiniz, [ara yazılımı](xref:fundamentals/middleware/index) erişerek `MvcOptions` kullanma `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span><span class="sxs-lookup"><span data-stu-id="faaa4-189">You can add conventions to your [middleware](xref:fundamentals/middleware/index) by accessing `MvcOptions` using `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span></span>

<span data-ttu-id="faaa4-190">Bu örnek denetleyicisi "Namespace" adına sahip olduğu özniteliği yönlendirme kullanmıyorsanız yolları Bu kuralı uygular.</span><span class="sxs-lookup"><span data-stu-id="faaa4-190">This sample applies this convention to routes that are not using attribute routing where the controller has  "Namespace" in its name.</span></span> <span data-ttu-id="faaa4-191">Bu kural aşağıdaki denetleyicisiyle gösterir:</span><span class="sxs-lookup"><span data-stu-id="faaa4-191">The following controller demonstrates this convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a><span data-ttu-id="faaa4-192">Uygulama modeli WebApiCompatShim kullanımda</span><span class="sxs-lookup"><span data-stu-id="faaa4-192">Application Model Usage in WebApiCompatShim</span></span>

<span data-ttu-id="faaa4-193">ASP.NET Core MVC farklı kümesinden kuralları ASP.NET Web API 2 kullanır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-193">ASP.NET Core MVC uses a different set of conventions from ASP.NET Web API 2.</span></span> <span data-ttu-id="faaa4-194">Özel kuralları kullanarak, bir Web API uygulaması ile tutarlı olacak şekilde bir ASP.NET Core MVC uygulamanın davranışı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="faaa4-194">Using custom conventions, you can modify an ASP.NET Core MVC app's behavior to be consistent with that of a Web API app.</span></span> <span data-ttu-id="faaa4-195">Microsoft gelir [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) özellikle bu amaç için.</span><span class="sxs-lookup"><span data-stu-id="faaa4-195">Microsoft ships the [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specifically for this purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="faaa4-196">Daha fazla bilgi edinmek [ASP.NET Web API geçirmek](xref:migration/webapi).</span><span class="sxs-lookup"><span data-stu-id="faaa4-196">Learn more about [migrate from ASP.NET Web API](xref:migration/webapi).</span></span>

<span data-ttu-id="faaa4-197">Web API uyumluluk dolgusu kullanmak için paket projenize ekleyin ve ardından kuralları için MVC çağırarak eklemeniz gerekir `AddWebApiConventions` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="faaa4-197">To use the Web API Compatibility Shim, you need to add the package to your project and then add the conventions to MVC by calling `AddWebApiConventions` in `Startup`:</span></span>

```c#
services.AddMvc().AddWebApiConventions();
```

<span data-ttu-id="faaa4-198">Dolgu tarafından sağlanan kuralları yalnızca uygulanmış belirli öznitelikler beklendiğinden uygulama bölümlerini uygulanır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-198">The conventions provided by the shim are only applied to parts of the app that have had certain attributes applied to them.</span></span> <span data-ttu-id="faaa4-199">Aşağıdaki dört öznitelikler denetleyicileri dolgu 's kuralları tarafından değiştirilmiş kendi kurallarına sahip denetlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="faaa4-199">The following four attributes are used to control which controllers should have their conventions modified by the shim's conventions:</span></span>

* [<span data-ttu-id="faaa4-200">UseWebApiActionConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="faaa4-200">UseWebApiActionConventionsAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [<span data-ttu-id="faaa4-201">UseWebApiOverloadingAttribute</span><span class="sxs-lookup"><span data-stu-id="faaa4-201">UseWebApiOverloadingAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [<span data-ttu-id="faaa4-202">UseWebApiParameterConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="faaa4-202">UseWebApiParameterConventionsAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [<span data-ttu-id="faaa4-203">UseWebApiRoutesAttribute</span><span class="sxs-lookup"><span data-stu-id="faaa4-203">UseWebApiRoutesAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a><span data-ttu-id="faaa4-204">Eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="faaa4-204">Action Conventions</span></span>

<span data-ttu-id="faaa4-205">`UseWebApiActionConventionsAttribute` Kendi adına göre eylemler için HTTP yöntemini eşlemek için kullanılır (örneğin, `Get` için eşleyen `HttpGet`).</span><span class="sxs-lookup"><span data-stu-id="faaa4-205">The `UseWebApiActionConventionsAttribute` is used to map the HTTP method to actions based on their name (for instance, `Get` would map to `HttpGet`).</span></span> <span data-ttu-id="faaa4-206">Yalnızca öznitelik yönlendirmeyi kullanmayan eylemleri için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-206">It only applies to actions that don't use attribute routing.</span></span>

### <a name="overloading"></a><span data-ttu-id="faaa4-207">Aşırı Yükleme</span><span class="sxs-lookup"><span data-stu-id="faaa4-207">Overloading</span></span>

<span data-ttu-id="faaa4-208">`UseWebApiOverloadingAttribute` Uygulamak için kullanılan `WebApiOverloadingApplicationModelConvention` kuralı.</span><span class="sxs-lookup"><span data-stu-id="faaa4-208">The `UseWebApiOverloadingAttribute` is used to apply the `WebApiOverloadingApplicationModelConvention` convention.</span></span> <span data-ttu-id="faaa4-209">Bu kuralı ekler bir `OverloadActionConstraint` eylem seçimi işlemi için hangi sınırlar bu isteği karşılayan tüm isteğe bağlı olmayan parametreler için aday eylemler.</span><span class="sxs-lookup"><span data-stu-id="faaa4-209">This convention adds an `OverloadActionConstraint` to the action selection process, which limits candidate actions to those for which the request satisfies all non-optional parameters.</span></span>

### <a name="parameter-conventions"></a><span data-ttu-id="faaa4-210">Parametre kuralları</span><span class="sxs-lookup"><span data-stu-id="faaa4-210">Parameter Conventions</span></span>

<span data-ttu-id="faaa4-211">`UseWebApiParameterConventionsAttribute` Uygulamak için kullanılan `WebApiParameterConventionsApplicationModelConvention` Eylem kuralı.</span><span class="sxs-lookup"><span data-stu-id="faaa4-211">The `UseWebApiParameterConventionsAttribute` is used to apply the `WebApiParameterConventionsApplicationModelConvention` action convention.</span></span> <span data-ttu-id="faaa4-212">Bu kural eylemi parametre olarak kullanılan basit türler karmaşık türler isteği gövdesinden bağlı sırasında varsayılan olarak, URI'den ilişkili belirtir.</span><span class="sxs-lookup"><span data-stu-id="faaa4-212">This convention specifies that simple types used as action parameters are bound from the URI by default, while complex types are bound from the request body.</span></span>

### <a name="routes"></a><span data-ttu-id="faaa4-213">Yollar</span><span class="sxs-lookup"><span data-stu-id="faaa4-213">Routes</span></span>

<span data-ttu-id="faaa4-214">`UseWebApiRoutesAttribute` Denetimleri olup olmadığını `WebApiApplicationModelConvention` denetleyicisi kuralı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="faaa4-214">The `UseWebApiRoutesAttribute` controls whether the `WebApiApplicationModelConvention` controller convention is applied.</span></span> <span data-ttu-id="faaa4-215">Etkinleştirildiğinde, bu kural için destek eklemek için kullanılan [alanları](xref:mvc/controllers/areas) rota.</span><span class="sxs-lookup"><span data-stu-id="faaa4-215">When enabled, this convention is used to add support for [areas](xref:mvc/controllers/areas) to the route.</span></span>

<span data-ttu-id="faaa4-216">Uyumluluk Paketi kuralları kümesi yanı sıra içerir bir `System.Web.Http.ApiController` temel Web API'si tarafından sağlanan yerini sınıfı.</span><span class="sxs-lookup"><span data-stu-id="faaa4-216">In addition to a set of conventions, the compatibility package includes a `System.Web.Http.ApiController` base class that replaces the one provided by Web API.</span></span> <span data-ttu-id="faaa4-217">Web API ve devralma için yazılmış denetleyicilerinizi böylece gelen kendi `ApiController` bunlar, ASP.NET Core MVC çalıştırılırken tasarlanan şekilde çalışması için.</span><span class="sxs-lookup"><span data-stu-id="faaa4-217">This allows your controllers written for Web API and inheriting from its `ApiController` to work as they were designed, while running on ASP.NET Core MVC.</span></span> <span data-ttu-id="faaa4-218">Bu temel denetleyici sınıfı tüm ile donatılmış `UseWebApi*` yukarıda listelenen öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="faaa4-218">This base controller class is decorated with all of the `UseWebApi*` attributes listed above.</span></span> <span data-ttu-id="faaa4-219">`ApiController` Özellikleri, yöntemleri ve Web API'si bulunanlar uyumlu sonuç türleri kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="faaa4-219">The `ApiController` exposes properties, methods, and result types that are compatible with those found in Web API.</span></span>

## <a name="using-apiexplorer-to-document-your-app"></a><span data-ttu-id="faaa4-220">Uygulamanızı belge için ApiExplorer kullanma</span><span class="sxs-lookup"><span data-stu-id="faaa4-220">Using ApiExplorer to Document Your App</span></span>

<span data-ttu-id="faaa4-221">Uygulama modeli kullanıma sunan bir [ `ApiExplorer` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) her düzeyde uygulamanın yapısı geçiş yapmak için kullanılan özellik.</span><span class="sxs-lookup"><span data-stu-id="faaa4-221">The application model exposes an [`ApiExplorer`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) property at each level that can be used to traverse the app's structure.</span></span> <span data-ttu-id="faaa4-222">Bunun için kullanılabilir [Swagger gibi araçları kullanarak, Web API'leri için Yardım sayfalarına üret](xref:tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="faaa4-222">This can be used to [generate help pages for your Web APIs using tools like Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="faaa4-223">`ApiExplorer` Özelliği düzenlemenizi sağlayan bir `IsVisible` uygulamanızın modeli hangi kısımlarının açılmamalıdır belirtmek için ayarlanabilir özelliği.</span><span class="sxs-lookup"><span data-stu-id="faaa4-223">The `ApiExplorer` property exposes an `IsVisible` property that can be set to specify which parts of your app's model should be exposed.</span></span> <span data-ttu-id="faaa4-224">Bir kural kullanarak bu ayarı yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="faaa4-224">You can configure this setting using a convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

<span data-ttu-id="faaa4-225">Bu yaklaşım (ve gerekirse ek kuralları) kullanarak etkinleştirin veya uygulamanızda herhangi bir düzeyde API görünürlük devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="faaa4-225">Using this approach (and additional conventions if required), you can enable or disable API visibility at any level within your app.</span></span> 
