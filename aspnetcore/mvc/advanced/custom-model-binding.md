---
title: ASP.NET Core özel model bağlama
author: ardalis
description: Model bağlamanın, denetleyici eylemlerinin ASP.NET Core doğrudan model türleriyle nasıl çalışmasına izin verdiğini öğrenin.
ms.author: riande
ms.date: 12/05/2019
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 625cc6c9ca5a2c22d028ea25f8fc0d942b71f12d
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881124"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="5cf8a-103">ASP.NET Core özel model bağlama</span><span class="sxs-lookup"><span data-stu-id="5cf8a-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="5cf8a-104">[Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="5cf8a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5cf8a-105">Model bağlama, denetleyici eylemlerinin HTTP istekleri yerine model türleriyle doğrudan (Yöntem bağımsız değişkenleri olarak geçirilir) çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="5cf8a-106">Gelen istek verileri ve uygulama modelleri arasındaki eşleme, model ciltleri tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="5cf8a-107">Geliştiriciler, özel model ciltlerinizi uygulayarak yerleşik model bağlama işlevini genişletebilir (genellikle kendi sağlayıcınızı yazmanız gerekmez).</span><span class="sxs-lookup"><span data-stu-id="5cf8a-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="5cf8a-108">GitHub 'dan örnek görüntüleme veya indirme</span><span class="sxs-lookup"><span data-stu-id="5cf8a-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="5cf8a-109">Varsayılan model Ciltçi sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="5cf8a-109">Default model binder limitations</span></span>

<span data-ttu-id="5cf8a-110">Varsayılan model ciltleri ortak .NET Core veri türlerinin çoğunu destekler ve çoğu geliştiricilerin ihtiyaçlarını karşılamaları gerekir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-110">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="5cf8a-111">İstek içinden doğrudan model türlerine metin tabanlı giriş bağlamayı bekler.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="5cf8a-112">Bağlamayı bağlamadan önce girişi dönüştürmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="5cf8a-113">Örneğin, model verilerini aramak için kullanılabilecek bir anahtarınız olduğunda.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="5cf8a-114">Anahtar temelinde veri getirmek için özel bir model Bağlayıcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="5cf8a-115">Model bağlama incelemesi</span><span class="sxs-lookup"><span data-stu-id="5cf8a-115">Model binding review</span></span>

<span data-ttu-id="5cf8a-116">Model bağlama, üzerinde çalıştığı türler için belirli tanımları kullanır.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="5cf8a-117">*Basit bir tür* , girişte tek bir dizeden dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="5cf8a-118">*Karmaşık bir tür* birden çok giriş değerinden dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="5cf8a-119">Framework, bir `TypeConverter`varlığına göre farkı belirler.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="5cf8a-120">Dış kaynak gerektirmeyen basit bir `string` -> `SomeType` eşlemeye sahipseniz bir tür dönüştürücüsü oluşturmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="5cf8a-121">Kendi özel model cilinkini oluşturmadan önce, mevcut model ciltçileri 'nin nasıl uygulandığını gözden geçirdik.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="5cf8a-122">Base64 ile kodlanmış dizeleri bayt dizilerine dönüştürmek için kullanılabilecek [Bytearraymodelciltçi](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) 'yi düşünün.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="5cf8a-123">Bayt dizileri genellikle dosya veya veritabanı blobu alanları olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="5cf8a-124">Bytearraymodelciltçi ile çalışma</span><span class="sxs-lookup"><span data-stu-id="5cf8a-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="5cf8a-125">İkili verileri temsil etmek için Base64 kodlamalı dizeler kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="5cf8a-126">Örneğin, aşağıdaki resim bir dize olarak kodlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="5cf8a-127">![DotNet bot](custom-model-binding/images/bot.png "DotNet bot")</span><span class="sxs-lookup"><span data-stu-id="5cf8a-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="5cf8a-128">Kodlanmış dizenin küçük bir kısmı aşağıdaki görüntüde gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="5cf8a-129">![DotNet bot kodlamalı](custom-model-binding/images/encoded-bot.png "DotNet bot kodlamalı")</span><span class="sxs-lookup"><span data-stu-id="5cf8a-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="5cf8a-130">Base64 ile kodlanmış dizeyi bir dosyaya dönüştürmek için [ÖRNEĞIN Benioku](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) dosyasındaki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-130">Follow the instructions in the [sample's README](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="5cf8a-131">ASP.NET Core MVC, Base64 kodlamalı bir dize alabilir ve bir bayt dizisine dönüştürmek için bir `ByteArrayModelBinder` kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-131">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="5cf8a-132">[IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) Maps uygulayan [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) , `byte[]` bağımsız değişkenleri `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="5cf8a-133">Kendi özel model cilinkini oluştururken kendi `IModelBinderProvider` türünü uygulayabilir veya [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute)kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="5cf8a-134">Aşağıdaki örnek, Base64 kodlamalı bir dizeyi bir `byte[]` dönüştürmek ve sonucu bir dosyaya kaydetmek için `ByteArrayModelBinder` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="5cf8a-135">[Postman](https://www.getpostman.com/)gibi bir araç kullanarak bu API yöntemine Base64 kodlamalı bir dize gönderebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="5cf8a-136">![Postman](custom-model-binding/images/postman.png "Postman")</span><span class="sxs-lookup"><span data-stu-id="5cf8a-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="5cf8a-137">Bağlayıcı, istek verilerini uygun şekilde adlandırılmış özelliklere veya bağımsız değişkenlere bağlayabildiğinden, model bağlama başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="5cf8a-138">Aşağıdaki örnek, `ByteArrayModelBinder` bir görünüm modeliyle nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="5cf8a-139">Özel model Ciltçi örneği</span><span class="sxs-lookup"><span data-stu-id="5cf8a-139">Custom model binder sample</span></span>

<span data-ttu-id="5cf8a-140">Bu bölümde, özel bir model cildi uygulayacağız:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="5cf8a-141">Gelen istek verilerini kesin belirlenmiş anahtar bağımsız değişkenlerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="5cf8a-142">İlişkili varlığı getirmek için Entity Framework Core kullanır.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="5cf8a-143">İlişkili varlığı eylem yöntemine bir bağımsız değişken olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="5cf8a-144">Aşağıdaki örnek, `Author` modelinde `ModelBinder` özniteliğini kullanır:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="5cf8a-145">Yukarıdaki kodda `ModelBinder` özniteliği, `Author` eylem parametrelerini bağlamak için kullanılması gereken `IModelBinder` türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span>

<span data-ttu-id="5cf8a-146">Aşağıdaki `AuthorEntityBinder` sınıfı, Entity Framework Core ve `authorId`kullanarak bir veri kaynağından varlığı getirerek bir `Author` parametresini bağlar:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-146">The following `AuthorEntityBinder` class binds an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> <span data-ttu-id="5cf8a-147">Önceki `AuthorEntityBinder` sınıfı özel bir model cildi göstermek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-147">The preceding `AuthorEntityBinder` class is intended to illustrate a custom model binder.</span></span> <span data-ttu-id="5cf8a-148">Sınıfı, bir arama senaryosu için en iyi yöntemleri göstermeye yönelik değildir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-148">The class isn't intended to illustrate best practices for a lookup scenario.</span></span> <span data-ttu-id="5cf8a-149">Arama için `authorId` bağlayın ve veritabanını bir eylem yöntemine sorgulayın.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-149">For lookup, bind the `authorId` and query the database in an action method.</span></span> <span data-ttu-id="5cf8a-150">Bu yaklaşım, model bağlama başarısızlıklarını `NotFound` durumlardan ayırır.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-150">This approach separates model binding failures from `NotFound` cases.</span></span>

<span data-ttu-id="5cf8a-151">Aşağıdaki kod, `AuthorEntityBinder` bir eylem yönteminde nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-151">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="5cf8a-152">`ModelBinder` özniteliği, varsayılan kuralları kullanmayan parametrelere `AuthorEntityBinder` uygulamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-152">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="5cf8a-153">Bu örnekte, bağımsız değişkenin adı varsayılan `authorId`olmadığından, parametresinde `ModelBinder` özniteliği kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-153">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="5cf8a-154">Hem denetleyici hem de eylem yöntemi, eylem yöntemindeki varlığı aramaya kıyasla basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-154">Both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="5cf8a-155">Entity Framework Core kullanarak yazarı getirme mantığı model cilde taşınır.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-155">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="5cf8a-156">Bu, `Author` modeline bağlanan çeşitli yöntemlere sahip olduğunuzda önemli bir basitleştirme olabilir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-156">This can be a considerable simplification when you have several methods that bind to the `Author` model.</span></span>

<span data-ttu-id="5cf8a-157">Yalnızca bu tür veya eylem için belirli bir model Bağlayıcısı veya model adı belirtmek için, `ModelBinder` özniteliğini tek tek model özelliklerine (ViewModel üzerinde gibi) veya eylem yöntemi parametrelerine uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-157">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="5cf8a-158">ModelBinderProvider uygulama</span><span class="sxs-lookup"><span data-stu-id="5cf8a-158">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="5cf8a-159">Bir özniteliği uygulamak yerine, `IModelBinderProvider`uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-159">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="5cf8a-160">Yerleşik çerçeve ciltçileri uygulanır.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-160">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="5cf8a-161">Cildin üzerinde çalıştığı türü belirttiğinizde, cildin kabul ettiği girişi **değil** , oluşturduğu bağımsız değişkenin türünü belirtirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-161">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="5cf8a-162">Aşağıdaki Ciltçi sağlayıcısı `AuthorEntityBinder`ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-162">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="5cf8a-163">MVC 'nin sağlayıcılar koleksiyonuna eklendiğinde, `Author` veya `Author`türü belirlenmiş parametrelerde `ModelBinder` özniteliğini kullanmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-163">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author`-typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="5cf8a-164">Note: Yukarıdaki kod bir `BinderTypeModelBinder`döndürür.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-164">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="5cf8a-165">`BinderTypeModelBinder`, model ciltleri için bir fabrika görevi görür ve bağımlılık ekleme (DI) sağlar.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-165">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="5cf8a-166">`AuthorEntityBinder`, EF Core erişim için dı gerektiriyor.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-166">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="5cf8a-167">Model Ciltçi 'nin dı 'den hizmet gerektirmesi durumunda `BinderTypeModelBinder` kullanın.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-167">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="5cf8a-168">Özel model Ciltçi sağlayıcısını kullanmak için, `ConfigureServices`içine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-168">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="5cf8a-169">Model ciltleri değerlendirilirken, sağlayıcı koleksiyonu sırayla incelenir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-169">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="5cf8a-170">Bir cildi döndüren ilk sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-170">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="5cf8a-171">Aşağıdaki görüntüde, hata ayıklayıcıdan varsayılan model ciltçileri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-171">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="5cf8a-172">![Varsayılan model ciltleri](custom-model-binding/images/default-model-binders.png "Varsayılan model ciltleri")</span><span class="sxs-lookup"><span data-stu-id="5cf8a-172">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="5cf8a-173">Sağlayıcınızı koleksiyonun sonuna eklemek, özel ciltçinin bir şansı olmadan önce yerleşik bir model cilde yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-173">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="5cf8a-174">Bu örnekte, özel sağlayıcı, `Author` eylem bağımsız değişkenleri için kullanıldığından emin olmak için koleksiyonun başlangıcına eklenir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-174">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

### <a name="polymorphic-model-binding"></a><span data-ttu-id="5cf8a-175">Polimorfik model bağlama</span><span class="sxs-lookup"><span data-stu-id="5cf8a-175">Polymorphic model binding</span></span>

<span data-ttu-id="5cf8a-176">Türetilmiş türlerin farklı modellerine bağlama polimorfik model bağlama olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-176">Binding to different models of derived types is known as polymorphic model binding.</span></span> <span data-ttu-id="5cf8a-177">İstek değeri, belirli türetilmiş model türüne bağlanması gerektiğinde polimorfik özel model bağlama gerekir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-177">Polymorphic custom model binding is required when the request value must be bound to the specific derived model type.</span></span> <span data-ttu-id="5cf8a-178">Polimorfik model bağlama:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-178">Polymorphic model binding:</span></span>

* <span data-ttu-id="5cf8a-179">Tüm dillerle birlikte çalışmak üzere tasarlanan bir REST API için tipik değildir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-179">Isn't typical for a REST API that's designed to interoperate with all languages.</span></span>
* <span data-ttu-id="5cf8a-180">, Bağlantılı modeller hakkında neden olmasını zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-180">Makes it difficult to reason about the bound models.</span></span>

<span data-ttu-id="5cf8a-181">Ancak, bir uygulama çok biçimli model bağlama gerektiriyorsa, bir uygulama aşağıdaki koda benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-181">However, if an app requires polymorphic model binding, an implementation might look like the following code:</span></span>

[!code-csharp[](custom-model-binding/3.0sample/PolymorphicModelBinding/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="5cf8a-182">Öneriler ve en iyi uygulamalar</span><span class="sxs-lookup"><span data-stu-id="5cf8a-182">Recommendations and best practices</span></span>

<span data-ttu-id="5cf8a-183">Özel model ciltleri:</span><span class="sxs-lookup"><span data-stu-id="5cf8a-183">Custom model binders:</span></span>

- <span data-ttu-id="5cf8a-184">Durum kodları veya dönüş sonuçları ayarlanmamalıdır (örneğin, 404 bulunamadı).</span><span class="sxs-lookup"><span data-stu-id="5cf8a-184">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="5cf8a-185">Model bağlama başarısız olursa, eylem yönteminin kendisi içindeki bir [eylem filtresinin](xref:mvc/controllers/filters) veya mantığının hata işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-185">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="5cf8a-186">, Yinelenen kodu ve eylem yöntemlerinden çapraz kesme sorunlarını ortadan kaldırmak için en yararlı seçenektir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-186">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="5cf8a-187">Genellikle bir dizeyi özel bir türe dönüştürmek için kullanılmamalıdır, bir [TypeConverter](/dotnet/api/system.componentmodel.typeconverter) genellikle daha iyi bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="5cf8a-187">Typically shouldn't be used to convert a string into a custom type, a [TypeConverter](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
