---
title: ASP.NET Core özel model bağlama
author: ardalis
description: Model bağlamanın, denetleyici eylemlerinin ASP.NET Core doğrudan model türleriyle nasıl çalışmasına izin verdiğini öğrenin.
ms.author: riande
ms.date: 11/13/2018
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 91f42393ffee3249f9167e10eaea7b279a7cb70b
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878405"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="ae881-103">ASP.NET Core özel model bağlama</span><span class="sxs-lookup"><span data-stu-id="ae881-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="ae881-104">[Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="ae881-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ae881-105">Model bağlama, denetleyici eylemlerinin HTTP istekleri yerine model türleriyle doğrudan (Yöntem bağımsız değişkenleri olarak geçirilir) çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="ae881-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="ae881-106">Gelen istek verileri ve uygulama modelleri arasındaki eşleme, model ciltleri tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="ae881-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="ae881-107">Geliştiriciler, özel model ciltlerinizi uygulayarak yerleşik model bağlama işlevini genişletebilir (genellikle kendi sağlayıcınızı yazmanız gerekmez).</span><span class="sxs-lookup"><span data-stu-id="ae881-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="ae881-108">GitHub 'dan örnek görüntüleme veya indirme</span><span class="sxs-lookup"><span data-stu-id="ae881-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="ae881-109">Varsayılan model Ciltçi sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="ae881-109">Default model binder limitations</span></span>

<span data-ttu-id="ae881-110">Varsayılan model ciltleri ortak .NET Core veri türlerinin çoğunu destekler ve çoğu geliştiricilerin ihtiyaçlarını karşılamaları gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae881-110">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="ae881-111">İstek içinden doğrudan model türlerine metin tabanlı giriş bağlamayı bekler.</span><span class="sxs-lookup"><span data-stu-id="ae881-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="ae881-112">Bağlamayı bağlamadan önce girişi dönüştürmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ae881-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="ae881-113">Örneğin, model verilerini aramak için kullanılabilecek bir anahtarınız olduğunda.</span><span class="sxs-lookup"><span data-stu-id="ae881-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="ae881-114">Anahtar temelinde veri getirmek için özel bir model Bağlayıcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae881-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="ae881-115">Model bağlama incelemesi</span><span class="sxs-lookup"><span data-stu-id="ae881-115">Model binding review</span></span>

<span data-ttu-id="ae881-116">Model bağlama, üzerinde çalıştığı türler için belirli tanımları kullanır.</span><span class="sxs-lookup"><span data-stu-id="ae881-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="ae881-117">*Basit bir tür* , girişte tek bir dizeden dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="ae881-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="ae881-118">*Karmaşık bir tür* birden çok giriş değerinden dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="ae881-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="ae881-119">Framework, a `TypeConverter`'nın varlığına göre farkı belirler.</span><span class="sxs-lookup"><span data-stu-id="ae881-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="ae881-120">Dış kaynak gerektirmeyen basit `string`  ->  `SomeType` bir eşlemeye sahipseniz bir tür dönüştürücüsü oluşturmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="ae881-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="ae881-121">Kendi özel model cilinkini oluşturmadan önce, mevcut model ciltçileri 'nin nasıl uygulandığını gözden geçirdik.</span><span class="sxs-lookup"><span data-stu-id="ae881-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="ae881-122">Base64 ile kodlanmış dizeleri bayt dizilerine dönüştürmek için kullanılabilecek [Bytearraymodelciltçi](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) 'yi düşünün.</span><span class="sxs-lookup"><span data-stu-id="ae881-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="ae881-123">Bayt dizileri genellikle dosya veya veritabanı blobu alanları olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="ae881-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="ae881-124">Bytearraymodelciltçi ile çalışma</span><span class="sxs-lookup"><span data-stu-id="ae881-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="ae881-125">İkili verileri temsil etmek için Base64 kodlamalı dizeler kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae881-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="ae881-126">Örneğin, aşağıdaki resim bir dize olarak kodlanabilir.</span><span class="sxs-lookup"><span data-stu-id="ae881-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="ae881-127">![DotNet bot](custom-model-binding/images/bot.png "DotNet bot")</span><span class="sxs-lookup"><span data-stu-id="ae881-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="ae881-128">Kodlanmış dizenin küçük bir kısmı aşağıdaki görüntüde gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="ae881-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="ae881-129">![DotNet bot kodlamalı](custom-model-binding/images/encoded-bot.png "DotNet bot kodlamalı")</span><span class="sxs-lookup"><span data-stu-id="ae881-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="ae881-130">Base64 ile kodlanmış dizeyi bir dosyaya dönüştürmek için [ÖRNEĞIN Benioku](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) dosyasındaki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="ae881-130">Follow the instructions in the [sample's README](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="ae881-131">ASP.NET Core MVC, Base64 kodlamalı bir dize alabilir ve bunu bir `ByteArrayModelBinder` bayt dizisine dönüştürmek için kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="ae881-131">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="ae881-132">[IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) `byte[]` [eşleme bağımsız değişkenlerini uygulayan ByteArrayModelBinderProvider:](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) `ByteArrayModelBinder`</span><span class="sxs-lookup"><span data-stu-id="ae881-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="ae881-133">Kendi özel model cilinkini oluştururken kendi `IModelBinderProvider` türünü uygulayabilir veya [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute)kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae881-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="ae881-134">Aşağıdaki örnek, bir base64 kodlu dizeyi `ByteArrayModelBinder` `byte[]` öğesine dönüştürmek ve sonucu bir dosyaya kaydetmek için nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="ae881-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="ae881-135">[Postman](https://www.getpostman.com/)gibi bir araç kullanarak bu API yöntemine Base64 kodlamalı bir dize gönderebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ae881-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="ae881-136">![Postman](custom-model-binding/images/postman.png "Postman")</span><span class="sxs-lookup"><span data-stu-id="ae881-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="ae881-137">Bağlayıcı, istek verilerini uygun şekilde adlandırılmış özelliklere veya bağımsız değişkenlere bağlayabildiğinden, model bağlama başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="ae881-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="ae881-138">Aşağıdaki örnek, bir görünüm modeliyle nasıl `ByteArrayModelBinder` kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="ae881-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="ae881-139">Özel model Ciltçi örneği</span><span class="sxs-lookup"><span data-stu-id="ae881-139">Custom model binder sample</span></span>

<span data-ttu-id="ae881-140">Bu bölümde, özel bir model cildi uygulayacağız:</span><span class="sxs-lookup"><span data-stu-id="ae881-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="ae881-141">Gelen istek verilerini kesin belirlenmiş anahtar bağımsız değişkenlerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="ae881-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="ae881-142">İlişkili varlığı getirmek için Entity Framework Core kullanır.</span><span class="sxs-lookup"><span data-stu-id="ae881-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="ae881-143">İlişkili varlığı eylem yöntemine bir bağımsız değişken olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="ae881-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="ae881-144">Aşağıdaki örnek `ModelBinder` `Author` modeldeki özniteliğini kullanır:</span><span class="sxs-lookup"><span data-stu-id="ae881-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="ae881-145">Yukarıdaki kodda `ModelBinder` öznitelik, eylem parametrelerini bağlamak `Author` için kullanılması gereken `IModelBinder` türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="ae881-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span>

<span data-ttu-id="ae881-146">Aşağıdaki `AuthorEntityBinder` sınıf, Entity Framework Core ve `Author` ' i kullanarak bir veri `authorId`kaynağından varlığı getirerek bir parametreyi bağlar:</span><span class="sxs-lookup"><span data-stu-id="ae881-146">The following `AuthorEntityBinder` class binds an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> <span data-ttu-id="ae881-147">Önceki `AuthorEntityBinder` sınıf özel bir model cildi göstermeye yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="ae881-147">The preceding `AuthorEntityBinder` class is intended to illustrate a custom model binder.</span></span> <span data-ttu-id="ae881-148">Sınıfı, bir arama senaryosu için en iyi yöntemleri göstermeye yönelik değildir.</span><span class="sxs-lookup"><span data-stu-id="ae881-148">The class isn't intended to illustrate best practices for a lookup scenario.</span></span> <span data-ttu-id="ae881-149">Arama için, veritabanını bağlayın `authorId` ve bir eylem yöntemine sorgulayın.</span><span class="sxs-lookup"><span data-stu-id="ae881-149">For lookup, bind the `authorId` and query the database in an action method.</span></span> <span data-ttu-id="ae881-150">Bu yaklaşım, model bağlama başarısızlıklarını `NotFound` durumlardan ayırır.</span><span class="sxs-lookup"><span data-stu-id="ae881-150">This approach separates model binding failures from `NotFound` cases.</span></span>

<span data-ttu-id="ae881-151">Aşağıdaki kod, `AuthorEntityBinder` bir eylem yönteminin nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="ae881-151">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="ae881-152">Özniteliği, varsayılan kuralları kullanmayan parametreleri `AuthorEntityBinder` uygulamak için kullanılabilir: `ModelBinder`</span><span class="sxs-lookup"><span data-stu-id="ae881-152">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="ae881-153">Bu örnekte, bağımsız değişkenin adı varsayılan `authorId`olmadığından parametresi `ModelBinder` kullanılarak parametresi belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ae881-153">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="ae881-154">Hem denetleyici hem de eylem yöntemi, eylem yöntemindeki varlığı aramaya kıyasla basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ae881-154">Both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="ae881-155">Entity Framework Core kullanarak yazarı getirme mantığı model cilde taşınır.</span><span class="sxs-lookup"><span data-stu-id="ae881-155">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="ae881-156">Bu, `Author` modele bağlanan çeşitli yöntemlere sahip olduğunuzda büyük ölçüde basitleştirmesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="ae881-156">This can be a considerable simplification when you have several methods that bind to the `Author` model.</span></span>

<span data-ttu-id="ae881-157">Özniteliği, `ModelBinder` yalnızca bu tür veya eylem için belirli bir model cildi veya model adı belirtmek üzere tek tek model özelliklerine (ViewModel üzerinde gibi) veya eylem yöntemi parametrelerine uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae881-157">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="ae881-158">ModelBinderProvider uygulama</span><span class="sxs-lookup"><span data-stu-id="ae881-158">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="ae881-159">Bir özniteliği uygulamak yerine, öğesini uygulayabilirsiniz `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="ae881-159">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="ae881-160">Yerleşik çerçeve ciltçileri uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ae881-160">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="ae881-161">Cildin üzerinde çalıştığı türü belirttiğinizde, cildin kabul ettiği girişi **değil** , oluşturduğu bağımsız değişkenin türünü belirtirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae881-161">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="ae881-162">Aşağıdaki Ciltçi sağlayıcısı ile birlikte `AuthorEntityBinder`kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae881-162">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="ae881-163">MVC 'nin sağlayıcılar koleksiyonuna eklendiğinde, `ModelBinder` `Author` veya `Author`türü belirlenmiş parametrelerde özniteliğini kullanmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ae881-163">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author`-typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="ae881-164">Not: Yukarıdaki kod bir `BinderTypeModelBinder`döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae881-164">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="ae881-165">`BinderTypeModelBinder`Model ciltleri için bir fabrika işlevi görür ve bağımlılık ekleme (dı) sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae881-165">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="ae881-166">İçin dı 'nin EF Core erişimi gerekir.`AuthorEntityBinder`</span><span class="sxs-lookup"><span data-stu-id="ae881-166">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="ae881-167">Model `BinderTypeModelBinder` Ciltçi 'nin dı 'den hizmet gerektirmesi durumunda kullanın.</span><span class="sxs-lookup"><span data-stu-id="ae881-167">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="ae881-168">Özel model Ciltçi sağlayıcısını kullanmak için, içine `ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ae881-168">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="ae881-169">Model ciltleri değerlendirilirken, sağlayıcı koleksiyonu sırayla incelenir.</span><span class="sxs-lookup"><span data-stu-id="ae881-169">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="ae881-170">Bir cildi döndüren ilk sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ae881-170">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="ae881-171">Aşağıdaki görüntüde, hata ayıklayıcıdan varsayılan model ciltçileri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ae881-171">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="ae881-172">![varsayılan model ciltleri](custom-model-binding/images/default-model-binders.png "varsayılan model ciltleri")</span><span class="sxs-lookup"><span data-stu-id="ae881-172">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="ae881-173">Sağlayıcınızı koleksiyonun sonuna eklemek, özel ciltçinin bir şansı olmadan önce yerleşik bir model cilde yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="ae881-173">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="ae881-174">Bu örnekte, özel sağlayıcı, eylem bağımsız değişkenleri için `Author` kullanıldığından emin olmak için koleksiyonun başlangıcına eklenir.</span><span class="sxs-lookup"><span data-stu-id="ae881-174">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

### <a name="polymorphic-model-binding"></a><span data-ttu-id="ae881-175">Polimorfik model bağlama</span><span class="sxs-lookup"><span data-stu-id="ae881-175">Polymorphic model binding</span></span>

<span data-ttu-id="ae881-176">Türetilmiş türlerin farklı modellerine bağlama polimorfik model bağlama olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="ae881-176">Binding to different models of derived types is known as polymorphic model binding.</span></span> <span data-ttu-id="ae881-177">İstek değeri, belirli türetilmiş model türüne bağlanmalıdır, özel model bağlama gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae881-177">Custom model binding is required when the request value must be bound to the specific derived model type.</span></span> <span data-ttu-id="ae881-178">Bu yaklaşım gerekli olmadığı takdirde polimorfik model bağlamasını önlemeyi öneririz.</span><span class="sxs-lookup"><span data-stu-id="ae881-178">Unless this approach is required, we recommend avoiding polymorphic model binding.</span></span> <span data-ttu-id="ae881-179">Polimorfik model bağlama, ilişkili modeller hakkında neden olmasını zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="ae881-179">Polymorphic model binding makes it difficult to reason about the bound models.</span></span> <span data-ttu-id="ae881-180">Ancak, bir uygulama çok biçimli model bağlama gerektiriyorsa, bir uygulama aşağıdaki koda benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="ae881-180">However, if an app requires polymorphic model binding, an implementation might look like the following code:</span></span>

<span data-ttu-id="ae881-181">Türetilmiş türlerin farklı modellerine bağlama polimorfik model bağlama olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="ae881-181">Binding to different models of derived types is known as polymorphic model binding.</span></span> <span data-ttu-id="ae881-182">İstek değeri, belirli türetilmiş model türüne bağlanmalıdır, özel model bağlama gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae881-182">Custom model binding is required when the request value must be bound to the specific derived model type.</span></span> <span data-ttu-id="ae881-183">Polimorfik model bağlama:</span><span class="sxs-lookup"><span data-stu-id="ae881-183">Polymorphic model binding:</span></span>

* <span data-ttu-id="ae881-184">Tüm dillerle birlikte çalışmak üzere tasarlanan bir REST API için tipik değildir.</span><span class="sxs-lookup"><span data-stu-id="ae881-184">Isn't typical for a REST API that's designed to interoperate with all languages.</span></span>
* <span data-ttu-id="ae881-185">, Bağlantılı modeller hakkında neden olmasını zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="ae881-185">Makes it difficult to reason about the bound models.</span></span>

<span data-ttu-id="ae881-186">Ancak, bir uygulama çok biçimli model bağlama gerektiriyorsa, bir uygulama aşağıdaki koda benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="ae881-186">However, if an app requires polymorphic model binding, an implementation might look like the following code:</span></span>

[!code-csharp[](custom-model-binding/3.0sample/PolymorphicModelBinding/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="ae881-187">Öneriler ve en iyi uygulamalar</span><span class="sxs-lookup"><span data-stu-id="ae881-187">Recommendations and best practices</span></span>

<span data-ttu-id="ae881-188">Özel model ciltleri:</span><span class="sxs-lookup"><span data-stu-id="ae881-188">Custom model binders:</span></span>

- <span data-ttu-id="ae881-189">Durum kodları veya dönüş sonuçları ayarlanmamalıdır (örneğin, 404 bulunamadı).</span><span class="sxs-lookup"><span data-stu-id="ae881-189">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="ae881-190">Model bağlama başarısız olursa, eylem yönteminin kendisi içindeki bir [eylem filtresinin](xref:mvc/controllers/filters) veya mantığının hata işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae881-190">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="ae881-191">, Yinelenen kodu ve eylem yöntemlerinden çapraz kesme sorunlarını ortadan kaldırmak için en yararlı seçenektir.</span><span class="sxs-lookup"><span data-stu-id="ae881-191">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="ae881-192">Genellikle bir [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) dizeyi özel bir türe dönüştürmek için kullanılmamalıdır, genellikle daha iyi bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="ae881-192">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
