---
title: ASP.NET Core özel model bağlama
author: ardalis
description: Model bağlamanın, denetleyici eylemlerinin ASP.NET Core doğrudan model türleriyle nasıl çalışmasına izin verdiğini öğrenin.
ms.author: riande
ms.date: 01/06/2020
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 92e7abbb9d9b4c29af429557a31e3ef403211976
ms.sourcegitcommit: 79850db9e79b1705b89f466c6f2c961ff15485de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75693953"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="78165-103">ASP.NET Core özel model bağlama</span><span class="sxs-lookup"><span data-stu-id="78165-103">Custom Model Binding in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="78165-104">[Steve Smith](https://ardalis.com/) ve [Kirk larkaya](https://twitter.com/serpent5) tarafından</span><span class="sxs-lookup"><span data-stu-id="78165-104">By [Steve Smith](https://ardalis.com/) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

<span data-ttu-id="78165-105">Model bağlama, denetleyici eylemlerinin HTTP istekleri yerine model türleriyle doğrudan (Yöntem bağımsız değişkenleri olarak geçirilir) çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="78165-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="78165-106">Gelen istek verileri ve uygulama modelleri arasındaki eşleme, model ciltleri tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="78165-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="78165-107">Geliştiriciler, özel model ciltlerinizi uygulayarak yerleşik model bağlama işlevini genişletebilir (genellikle kendi sağlayıcınızı yazmanız gerekmez).</span><span class="sxs-lookup"><span data-stu-id="78165-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

<span data-ttu-id="78165-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="78165-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="default-model-binder-limitations"></a><span data-ttu-id="78165-109">Varsayılan model Ciltçi sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="78165-109">Default model binder limitations</span></span>

<span data-ttu-id="78165-110">Varsayılan model ciltleri ortak .NET Core veri türlerinin çoğunu destekler ve çoğu geliştiricilerin ihtiyaçlarını karşılamaları gerekir.</span><span class="sxs-lookup"><span data-stu-id="78165-110">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="78165-111">İstek içinden doğrudan model türlerine metin tabanlı giriş bağlamayı bekler.</span><span class="sxs-lookup"><span data-stu-id="78165-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="78165-112">Bağlamayı bağlamadan önce girişi dönüştürmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="78165-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="78165-113">Örneğin, model verilerini aramak için kullanılabilecek bir anahtarınız olduğunda.</span><span class="sxs-lookup"><span data-stu-id="78165-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="78165-114">Anahtar temelinde veri getirmek için özel bir model Bağlayıcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78165-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="78165-115">Model bağlama incelemesi</span><span class="sxs-lookup"><span data-stu-id="78165-115">Model binding review</span></span>

<span data-ttu-id="78165-116">Model bağlama, üzerinde çalıştığı türler için belirli tanımları kullanır.</span><span class="sxs-lookup"><span data-stu-id="78165-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="78165-117">*Basit bir tür* , girişte tek bir dizeden dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="78165-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="78165-118">*Karmaşık bir tür* birden çok giriş değerinden dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="78165-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="78165-119">Framework, bir `TypeConverter`varlığına göre farkı belirler.</span><span class="sxs-lookup"><span data-stu-id="78165-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="78165-120">Dış kaynak gerektirmeyen basit bir `string` -> `SomeType` eşlemeye sahipseniz bir tür dönüştürücüsü oluşturmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="78165-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="78165-121">Kendi özel model cilinkini oluşturmadan önce, mevcut model ciltçileri 'nin nasıl uygulandığını gözden geçirdik.</span><span class="sxs-lookup"><span data-stu-id="78165-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="78165-122">Base64 ile kodlanmış dizeleri bayt dizilerine dönüştürmek için kullanılabilecek <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinder> göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="78165-122">Consider the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinder> which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="78165-123">Bayt dizileri genellikle dosya veya veritabanı blobu alanları olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="78165-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="78165-124">Bytearraymodelciltçi ile çalışma</span><span class="sxs-lookup"><span data-stu-id="78165-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="78165-125">İkili verileri temsil etmek için Base64 kodlamalı dizeler kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="78165-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="78165-126">Örneğin, bir görüntü dize olarak kodlanır.</span><span class="sxs-lookup"><span data-stu-id="78165-126">For example, an image can be encoded as a string.</span></span> <span data-ttu-id="78165-127">Örnek, [Base64String. txt](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/samples/3.x/CustomModelBindingSample/Base64String.txt)dosyasında Base64 kodlamalı dize olarak bir görüntü içerir.</span><span class="sxs-lookup"><span data-stu-id="78165-127">The sample includes an image as a base64-encoded string in [Base64String.txt](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/samples/3.x/CustomModelBindingSample/Base64String.txt).</span></span>

<span data-ttu-id="78165-128">ASP.NET Core MVC, Base64 kodlamalı bir dize alabilir ve bir bayt dizisine dönüştürmek için bir `ByteArrayModelBinder` kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="78165-128">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="78165-129"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinderProvider> `byte[]` bağımsız değişkenlerini `ByteArrayModelBinder`eşlenir:</span><span class="sxs-lookup"><span data-stu-id="78165-129">The <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinderProvider> maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        var loggerFactory = context.Services.GetRequiredService<ILoggerFactory>();
        return new ByteArrayModelBinder(loggerFactory);
    }

    return null;
}
```

<span data-ttu-id="78165-130">Kendi özel model cilinkini oluştururken kendi `IModelBinderProvider` türünü uygulayabilir veya <xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute>kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78165-130">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the <xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute>.</span></span>

<span data-ttu-id="78165-131">Aşağıdaki örnek, Base64 kodlamalı bir dizeyi bir `byte[]` dönüştürmek ve sonucu bir dosyaya kaydetmek için `ByteArrayModelBinder` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="78165-131">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/ImageController.cs?name=snippet_Post)]

<span data-ttu-id="78165-132">[Postman](https://www.getpostman.com/)gibi bir araç kullanarak bu API yöntemine Base64 kodlamalı bir dize gönderebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="78165-132">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="78165-133">![Postman](custom-model-binding/images/postman.png "Postman")</span><span class="sxs-lookup"><span data-stu-id="78165-133">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="78165-134">Bağlayıcı, istek verilerini uygun şekilde adlandırılmış özelliklere veya bağımsız değişkenlere bağlayabildiğinden, model bağlama başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="78165-134">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="78165-135">Aşağıdaki örnek, `ByteArrayModelBinder` bir görünüm modeliyle nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="78165-135">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/ImageController.cs?name=snippet_SaveProfile&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="78165-136">Özel model Ciltçi örneği</span><span class="sxs-lookup"><span data-stu-id="78165-136">Custom model binder sample</span></span>

<span data-ttu-id="78165-137">Bu bölümde, özel bir model cildi uygulayacağız:</span><span class="sxs-lookup"><span data-stu-id="78165-137">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="78165-138">Gelen istek verilerini kesin belirlenmiş anahtar bağımsız değişkenlerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="78165-138">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="78165-139">İlişkili varlığı getirmek için Entity Framework Core kullanır.</span><span class="sxs-lookup"><span data-stu-id="78165-139">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="78165-140">İlişkili varlığı eylem yöntemine bir bağımsız değişken olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="78165-140">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="78165-141">Aşağıdaki örnek, `Author` modelinde `ModelBinder` özniteliğini kullanır:</span><span class="sxs-lookup"><span data-stu-id="78165-141">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Data/Author.cs?highlight=6)]

<span data-ttu-id="78165-142">Yukarıdaki kodda `ModelBinder` özniteliği, `Author` eylem parametrelerini bağlamak için kullanılması gereken `IModelBinder` türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="78165-142">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span>

<span data-ttu-id="78165-143">Aşağıdaki `AuthorEntityBinder` sınıfı, Entity Framework Core ve `authorId`kullanarak bir veri kaynağından varlığı getirerek bir `Author` parametresini bağlar:</span><span class="sxs-lookup"><span data-stu-id="78165-143">The following `AuthorEntityBinder` class binds an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=snippet_Class)]

> [!NOTE]
> <span data-ttu-id="78165-144">Önceki `AuthorEntityBinder` sınıfı özel bir model cildi göstermek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="78165-144">The preceding `AuthorEntityBinder` class is intended to illustrate a custom model binder.</span></span> <span data-ttu-id="78165-145">Sınıfı, bir arama senaryosu için en iyi yöntemleri göstermeye yönelik değildir.</span><span class="sxs-lookup"><span data-stu-id="78165-145">The class isn't intended to illustrate best practices for a lookup scenario.</span></span> <span data-ttu-id="78165-146">Arama için `authorId` bağlayın ve veritabanını bir eylem yöntemine sorgulayın.</span><span class="sxs-lookup"><span data-stu-id="78165-146">For lookup, bind the `authorId` and query the database in an action method.</span></span> <span data-ttu-id="78165-147">Bu yaklaşım, model bağlama başarısızlıklarını `NotFound` durumlardan ayırır.</span><span class="sxs-lookup"><span data-stu-id="78165-147">This approach separates model binding failures from `NotFound` cases.</span></span>

<span data-ttu-id="78165-148">Aşağıdaki kod, `AuthorEntityBinder` bir eylem yönteminde nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="78165-148">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=snippet_Get&highlight=2)]

<span data-ttu-id="78165-149">`ModelBinder` özniteliği, varsayılan kuralları kullanmayan parametrelere `AuthorEntityBinder` uygulamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="78165-149">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=snippet_GetById&highlight=2)]

<span data-ttu-id="78165-150">Bu örnekte, bağımsız değişkenin adı varsayılan `authorId`olmadığından, parametresinde `ModelBinder` özniteliği kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="78165-150">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="78165-151">Hem denetleyici hem de eylem yöntemi, eylem yöntemindeki varlığı aramaya kıyasla basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="78165-151">Both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="78165-152">Entity Framework Core kullanarak yazarı getirme mantığı model cilde taşınır.</span><span class="sxs-lookup"><span data-stu-id="78165-152">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="78165-153">Bu, `Author` modeline bağlanan çeşitli yöntemlere sahip olduğunuzda önemli bir basitleştirme olabilir.</span><span class="sxs-lookup"><span data-stu-id="78165-153">This can be a considerable simplification when you have several methods that bind to the `Author` model.</span></span>

<span data-ttu-id="78165-154">Yalnızca bu tür veya eylem için belirli bir model Bağlayıcısı veya model adı belirtmek için, `ModelBinder` özniteliğini tek tek model özelliklerine (ViewModel üzerinde gibi) veya eylem yöntemi parametrelerine uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78165-154">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="78165-155">ModelBinderProvider uygulama</span><span class="sxs-lookup"><span data-stu-id="78165-155">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="78165-156">Bir özniteliği uygulamak yerine, `IModelBinderProvider`uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78165-156">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="78165-157">Yerleşik çerçeve ciltçileri uygulanır.</span><span class="sxs-lookup"><span data-stu-id="78165-157">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="78165-158">Cildin üzerinde çalıştığı türü belirttiğinizde, cildin kabul ettiği girişi **değil** , oluşturduğu bağımsız değişkenin türünü belirtirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78165-158">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="78165-159">Aşağıdaki Ciltçi sağlayıcısı `AuthorEntityBinder`ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="78165-159">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="78165-160">MVC 'nin sağlayıcılar koleksiyonuna eklendiğinde, `Author` veya `Author`türü belirlenmiş parametrelerde `ModelBinder` özniteliğini kullanmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="78165-160">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author`-typed parameters.</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="78165-161">Note: Yukarıdaki kod bir `BinderTypeModelBinder`döndürür.</span><span class="sxs-lookup"><span data-stu-id="78165-161">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="78165-162">`BinderTypeModelBinder`, model ciltleri için bir fabrika görevi görür ve bağımlılık ekleme (DI) sağlar.</span><span class="sxs-lookup"><span data-stu-id="78165-162">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="78165-163">`AuthorEntityBinder`, EF Core erişim için dı gerektiriyor.</span><span class="sxs-lookup"><span data-stu-id="78165-163">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="78165-164">Model Ciltçi 'nin dı 'den hizmet gerektirmesi durumunda `BinderTypeModelBinder` kullanın.</span><span class="sxs-lookup"><span data-stu-id="78165-164">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="78165-165">Özel model Ciltçi sağlayıcısını kullanmak için, `ConfigureServices`içine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="78165-165">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

<span data-ttu-id="78165-166">Model ciltleri değerlendirilirken, sağlayıcı koleksiyonu sırayla incelenir.</span><span class="sxs-lookup"><span data-stu-id="78165-166">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="78165-167">Bir cildi döndüren ilk sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="78165-167">The first provider that returns a binder is used.</span></span> <span data-ttu-id="78165-168">Sağlayıcınızı koleksiyonun sonuna eklemek, özel ciltçinin bir şansı olmadan önce yerleşik bir model cilde yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="78165-168">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="78165-169">Bu örnekte, özel sağlayıcı, `Author` eylem bağımsız değişkenleri için kullanıldığından emin olmak için koleksiyonun başlangıcına eklenir.</span><span class="sxs-lookup"><span data-stu-id="78165-169">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

### <a name="polymorphic-model-binding"></a><span data-ttu-id="78165-170">Polimorfik model bağlama</span><span class="sxs-lookup"><span data-stu-id="78165-170">Polymorphic model binding</span></span>

<span data-ttu-id="78165-171">Türetilmiş türlerin farklı modellerine bağlama polimorfik model bağlama olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="78165-171">Binding to different models of derived types is known as polymorphic model binding.</span></span> <span data-ttu-id="78165-172">İstek değeri, belirli türetilmiş model türüne bağlanması gerektiğinde polimorfik özel model bağlama gerekir.</span><span class="sxs-lookup"><span data-stu-id="78165-172">Polymorphic custom model binding is required when the request value must be bound to the specific derived model type.</span></span> <span data-ttu-id="78165-173">Polimorfik model bağlama:</span><span class="sxs-lookup"><span data-stu-id="78165-173">Polymorphic model binding:</span></span>

* <span data-ttu-id="78165-174">Tüm dillerle birlikte çalışmak üzere tasarlanan bir REST API için tipik değildir.</span><span class="sxs-lookup"><span data-stu-id="78165-174">Isn't typical for a REST API that's designed to interoperate with all languages.</span></span>
* <span data-ttu-id="78165-175">, Bağlantılı modeller hakkında neden olmasını zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="78165-175">Makes it difficult to reason about the bound models.</span></span>

<span data-ttu-id="78165-176">Ancak, bir uygulama çok biçimli model bağlama gerektiriyorsa, bir uygulama aşağıdaki koda benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="78165-176">However, if an app requires polymorphic model binding, an implementation might look like the following code:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/PolymorphicModelBindingSample/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="78165-177">Öneriler ve en iyi uygulamalar</span><span class="sxs-lookup"><span data-stu-id="78165-177">Recommendations and best practices</span></span>

<span data-ttu-id="78165-178">Özel model ciltleri:</span><span class="sxs-lookup"><span data-stu-id="78165-178">Custom model binders:</span></span>

- <span data-ttu-id="78165-179">Durum kodları veya dönüş sonuçları ayarlanmamalıdır (örneğin, 404 bulunamadı).</span><span class="sxs-lookup"><span data-stu-id="78165-179">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="78165-180">Model bağlama başarısız olursa, eylem yönteminin kendisi içindeki bir [eylem filtresinin](xref:mvc/controllers/filters) veya mantığının hata işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="78165-180">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="78165-181">, Yinelenen kodu ve eylem yöntemlerinden çapraz kesme sorunlarını ortadan kaldırmak için en yararlı seçenektir.</span><span class="sxs-lookup"><span data-stu-id="78165-181">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="78165-182">Genellikle bir dizeyi özel bir türe dönüştürmek için kullanılmamalıdır, <xref:System.ComponentModel.TypeConverter> genellikle daha iyi bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="78165-182">Typically shouldn't be used to convert a string into a custom type, a <xref:System.ComponentModel.TypeConverter> is usually a better option.</span></span>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="78165-183">[Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="78165-183">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="78165-184">Model bağlama, denetleyici eylemlerinin HTTP istekleri yerine model türleriyle doğrudan (Yöntem bağımsız değişkenleri olarak geçirilir) çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="78165-184">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="78165-185">Gelen istek verileri ve uygulama modelleri arasındaki eşleme, model ciltleri tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="78165-185">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="78165-186">Geliştiriciler, özel model ciltlerinizi uygulayarak yerleşik model bağlama işlevini genişletebilir (genellikle kendi sağlayıcınızı yazmanız gerekmez).</span><span class="sxs-lookup"><span data-stu-id="78165-186">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

<span data-ttu-id="78165-187">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="78165-187">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="default-model-binder-limitations"></a><span data-ttu-id="78165-188">Varsayılan model Ciltçi sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="78165-188">Default model binder limitations</span></span>

<span data-ttu-id="78165-189">Varsayılan model ciltleri ortak .NET Core veri türlerinin çoğunu destekler ve çoğu geliştiricilerin ihtiyaçlarını karşılamaları gerekir.</span><span class="sxs-lookup"><span data-stu-id="78165-189">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="78165-190">İstek içinden doğrudan model türlerine metin tabanlı giriş bağlamayı bekler.</span><span class="sxs-lookup"><span data-stu-id="78165-190">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="78165-191">Bağlamayı bağlamadan önce girişi dönüştürmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="78165-191">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="78165-192">Örneğin, model verilerini aramak için kullanılabilecek bir anahtarınız olduğunda.</span><span class="sxs-lookup"><span data-stu-id="78165-192">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="78165-193">Anahtar temelinde veri getirmek için özel bir model Bağlayıcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78165-193">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="78165-194">Model bağlama incelemesi</span><span class="sxs-lookup"><span data-stu-id="78165-194">Model binding review</span></span>

<span data-ttu-id="78165-195">Model bağlama, üzerinde çalıştığı türler için belirli tanımları kullanır.</span><span class="sxs-lookup"><span data-stu-id="78165-195">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="78165-196">*Basit bir tür* , girişte tek bir dizeden dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="78165-196">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="78165-197">*Karmaşık bir tür* birden çok giriş değerinden dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="78165-197">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="78165-198">Framework, bir `TypeConverter`varlığına göre farkı belirler.</span><span class="sxs-lookup"><span data-stu-id="78165-198">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="78165-199">Dış kaynak gerektirmeyen basit bir `string` -> `SomeType` eşlemeye sahipseniz bir tür dönüştürücüsü oluşturmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="78165-199">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="78165-200">Kendi özel model cilinkini oluşturmadan önce, mevcut model ciltçileri 'nin nasıl uygulandığını gözden geçirdik.</span><span class="sxs-lookup"><span data-stu-id="78165-200">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="78165-201">Base64 ile kodlanmış dizeleri bayt dizilerine dönüştürmek için kullanılabilecek <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinder> göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="78165-201">Consider the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinder> which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="78165-202">Bayt dizileri genellikle dosya veya veritabanı blobu alanları olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="78165-202">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="78165-203">Bytearraymodelciltçi ile çalışma</span><span class="sxs-lookup"><span data-stu-id="78165-203">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="78165-204">İkili verileri temsil etmek için Base64 kodlamalı dizeler kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="78165-204">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="78165-205">Örneğin, bir görüntü dize olarak kodlanır.</span><span class="sxs-lookup"><span data-stu-id="78165-205">For example, an image can be encoded as a string.</span></span> <span data-ttu-id="78165-206">Örnek, [Base64String. txt](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/samples/2.x/CustomModelBindingSample/Base64String.txt)dosyasında Base64 kodlamalı dize olarak bir görüntü içerir.</span><span class="sxs-lookup"><span data-stu-id="78165-206">The sample includes an image as a base64-encoded string in [Base64String.txt](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/samples/2.x/CustomModelBindingSample/Base64String.txt).</span></span>

<span data-ttu-id="78165-207">ASP.NET Core MVC, Base64 kodlamalı bir dize alabilir ve bir bayt dizisine dönüştürmek için bir `ByteArrayModelBinder` kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="78165-207">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="78165-208"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinderProvider> `byte[]` bağımsız değişkenlerini `ByteArrayModelBinder`eşlenir:</span><span class="sxs-lookup"><span data-stu-id="78165-208">The <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinderProvider> maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

<span data-ttu-id="78165-209">Kendi özel model cilinkini oluştururken kendi `IModelBinderProvider` türünü uygulayabilir veya <xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute>kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78165-209">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the <xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute>.</span></span>

<span data-ttu-id="78165-210">Aşağıdaki örnek, Base64 kodlamalı bir dizeyi bir `byte[]` dönüştürmek ve sonucu bir dosyaya kaydetmek için `ByteArrayModelBinder` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="78165-210">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/ImageController.cs?name=post1)]

<span data-ttu-id="78165-211">[Postman](https://www.getpostman.com/)gibi bir araç kullanarak bu API yöntemine Base64 kodlamalı bir dize gönderebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="78165-211">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="78165-212">![Postman](custom-model-binding/images/postman.png "Postman")</span><span class="sxs-lookup"><span data-stu-id="78165-212">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="78165-213">Bağlayıcı, istek verilerini uygun şekilde adlandırılmış özelliklere veya bağımsız değişkenlere bağlayabildiğinden, model bağlama başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="78165-213">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="78165-214">Aşağıdaki örnek, `ByteArrayModelBinder` bir görünüm modeliyle nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="78165-214">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="78165-215">Özel model Ciltçi örneği</span><span class="sxs-lookup"><span data-stu-id="78165-215">Custom model binder sample</span></span>

<span data-ttu-id="78165-216">Bu bölümde, özel bir model cildi uygulayacağız:</span><span class="sxs-lookup"><span data-stu-id="78165-216">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="78165-217">Gelen istek verilerini kesin belirlenmiş anahtar bağımsız değişkenlerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="78165-217">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="78165-218">İlişkili varlığı getirmek için Entity Framework Core kullanır.</span><span class="sxs-lookup"><span data-stu-id="78165-218">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="78165-219">İlişkili varlığı eylem yöntemine bir bağımsız değişken olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="78165-219">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="78165-220">Aşağıdaki örnek, `Author` modelinde `ModelBinder` özniteliğini kullanır:</span><span class="sxs-lookup"><span data-stu-id="78165-220">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Data/Author.cs?highlight=6)]

<span data-ttu-id="78165-221">Yukarıdaki kodda `ModelBinder` özniteliği, `Author` eylem parametrelerini bağlamak için kullanılması gereken `IModelBinder` türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="78165-221">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span>

<span data-ttu-id="78165-222">Aşağıdaki `AuthorEntityBinder` sınıfı, Entity Framework Core ve `authorId`kullanarak bir veri kaynağından varlığı getirerek bir `Author` parametresini bağlar:</span><span class="sxs-lookup"><span data-stu-id="78165-222">The following `AuthorEntityBinder` class binds an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> <span data-ttu-id="78165-223">Önceki `AuthorEntityBinder` sınıfı özel bir model cildi göstermek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="78165-223">The preceding `AuthorEntityBinder` class is intended to illustrate a custom model binder.</span></span> <span data-ttu-id="78165-224">Sınıfı, bir arama senaryosu için en iyi yöntemleri göstermeye yönelik değildir.</span><span class="sxs-lookup"><span data-stu-id="78165-224">The class isn't intended to illustrate best practices for a lookup scenario.</span></span> <span data-ttu-id="78165-225">Arama için `authorId` bağlayın ve veritabanını bir eylem yöntemine sorgulayın.</span><span class="sxs-lookup"><span data-stu-id="78165-225">For lookup, bind the `authorId` and query the database in an action method.</span></span> <span data-ttu-id="78165-226">Bu yaklaşım, model bağlama başarısızlıklarını `NotFound` durumlardan ayırır.</span><span class="sxs-lookup"><span data-stu-id="78165-226">This approach separates model binding failures from `NotFound` cases.</span></span>

<span data-ttu-id="78165-227">Aşağıdaki kod, `AuthorEntityBinder` bir eylem yönteminde nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="78165-227">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="78165-228">`ModelBinder` özniteliği, varsayılan kuralları kullanmayan parametrelere `AuthorEntityBinder` uygulamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="78165-228">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="78165-229">Bu örnekte, bağımsız değişkenin adı varsayılan `authorId`olmadığından, parametresinde `ModelBinder` özniteliği kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="78165-229">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="78165-230">Hem denetleyici hem de eylem yöntemi, eylem yöntemindeki varlığı aramaya kıyasla basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="78165-230">Both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="78165-231">Entity Framework Core kullanarak yazarı getirme mantığı model cilde taşınır.</span><span class="sxs-lookup"><span data-stu-id="78165-231">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="78165-232">Bu, `Author` modeline bağlanan çeşitli yöntemlere sahip olduğunuzda önemli bir basitleştirme olabilir.</span><span class="sxs-lookup"><span data-stu-id="78165-232">This can be a considerable simplification when you have several methods that bind to the `Author` model.</span></span>

<span data-ttu-id="78165-233">Yalnızca bu tür veya eylem için belirli bir model Bağlayıcısı veya model adı belirtmek için, `ModelBinder` özniteliğini tek tek model özelliklerine (ViewModel üzerinde gibi) veya eylem yöntemi parametrelerine uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78165-233">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="78165-234">ModelBinderProvider uygulama</span><span class="sxs-lookup"><span data-stu-id="78165-234">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="78165-235">Bir özniteliği uygulamak yerine, `IModelBinderProvider`uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78165-235">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="78165-236">Yerleşik çerçeve ciltçileri uygulanır.</span><span class="sxs-lookup"><span data-stu-id="78165-236">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="78165-237">Cildin üzerinde çalıştığı türü belirttiğinizde, cildin kabul ettiği girişi **değil** , oluşturduğu bağımsız değişkenin türünü belirtirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78165-237">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="78165-238">Aşağıdaki Ciltçi sağlayıcısı `AuthorEntityBinder`ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="78165-238">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="78165-239">MVC 'nin sağlayıcılar koleksiyonuna eklendiğinde, `Author` veya `Author`türü belirlenmiş parametrelerde `ModelBinder` özniteliğini kullanmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="78165-239">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author`-typed parameters.</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="78165-240">Note: Yukarıdaki kod bir `BinderTypeModelBinder`döndürür.</span><span class="sxs-lookup"><span data-stu-id="78165-240">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="78165-241">`BinderTypeModelBinder`, model ciltleri için bir fabrika görevi görür ve bağımlılık ekleme (DI) sağlar.</span><span class="sxs-lookup"><span data-stu-id="78165-241">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="78165-242">`AuthorEntityBinder`, EF Core erişim için dı gerektiriyor.</span><span class="sxs-lookup"><span data-stu-id="78165-242">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="78165-243">Model Ciltçi 'nin dı 'den hizmet gerektirmesi durumunda `BinderTypeModelBinder` kullanın.</span><span class="sxs-lookup"><span data-stu-id="78165-243">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="78165-244">Özel model Ciltçi sağlayıcısını kullanmak için, `ConfigureServices`içine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="78165-244">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-10)]

<span data-ttu-id="78165-245">Model ciltleri değerlendirilirken, sağlayıcı koleksiyonu sırayla incelenir.</span><span class="sxs-lookup"><span data-stu-id="78165-245">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="78165-246">Bir cildi döndüren ilk sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="78165-246">The first provider that returns a binder is used.</span></span> <span data-ttu-id="78165-247">Sağlayıcınızı koleksiyonun sonuna eklemek, özel ciltçinin bir şansı olmadan önce yerleşik bir model cilde yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="78165-247">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="78165-248">Bu örnekte, özel sağlayıcı, `Author` eylem bağımsız değişkenleri için kullanıldığından emin olmak için koleksiyonun başlangıcına eklenir.</span><span class="sxs-lookup"><span data-stu-id="78165-248">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

### <a name="polymorphic-model-binding"></a><span data-ttu-id="78165-249">Polimorfik model bağlama</span><span class="sxs-lookup"><span data-stu-id="78165-249">Polymorphic model binding</span></span>

<span data-ttu-id="78165-250">Türetilmiş türlerin farklı modellerine bağlama polimorfik model bağlama olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="78165-250">Binding to different models of derived types is known as polymorphic model binding.</span></span> <span data-ttu-id="78165-251">İstek değeri, belirli türetilmiş model türüne bağlanması gerektiğinde polimorfik özel model bağlama gerekir.</span><span class="sxs-lookup"><span data-stu-id="78165-251">Polymorphic custom model binding is required when the request value must be bound to the specific derived model type.</span></span> <span data-ttu-id="78165-252">Polimorfik model bağlama:</span><span class="sxs-lookup"><span data-stu-id="78165-252">Polymorphic model binding:</span></span>

* <span data-ttu-id="78165-253">Tüm dillerle birlikte çalışmak üzere tasarlanan bir REST API için tipik değildir.</span><span class="sxs-lookup"><span data-stu-id="78165-253">Isn't typical for a REST API that's designed to interoperate with all languages.</span></span>
* <span data-ttu-id="78165-254">, Bağlantılı modeller hakkında neden olmasını zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="78165-254">Makes it difficult to reason about the bound models.</span></span>

<span data-ttu-id="78165-255">Ancak, bir uygulama çok biçimli model bağlama gerektiriyorsa, bir uygulama aşağıdaki koda benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="78165-255">However, if an app requires polymorphic model binding, an implementation might look like the following code:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/PolymorphicModelBindingSample/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="78165-256">Öneriler ve en iyi uygulamalar</span><span class="sxs-lookup"><span data-stu-id="78165-256">Recommendations and best practices</span></span>

<span data-ttu-id="78165-257">Özel model ciltleri:</span><span class="sxs-lookup"><span data-stu-id="78165-257">Custom model binders:</span></span>

- <span data-ttu-id="78165-258">Durum kodları veya dönüş sonuçları ayarlanmamalıdır (örneğin, 404 bulunamadı).</span><span class="sxs-lookup"><span data-stu-id="78165-258">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="78165-259">Model bağlama başarısız olursa, eylem yönteminin kendisi içindeki bir [eylem filtresinin](xref:mvc/controllers/filters) veya mantığının hata işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="78165-259">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="78165-260">, Yinelenen kodu ve eylem yöntemlerinden çapraz kesme sorunlarını ortadan kaldırmak için en yararlı seçenektir.</span><span class="sxs-lookup"><span data-stu-id="78165-260">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="78165-261">Genellikle bir dizeyi özel bir türe dönüştürmek için kullanılmamalıdır, <xref:System.ComponentModel.TypeConverter> genellikle daha iyi bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="78165-261">Typically shouldn't be used to convert a string into a custom type, a <xref:System.ComponentModel.TypeConverter> is usually a better option.</span></span>

::: moniker-end
