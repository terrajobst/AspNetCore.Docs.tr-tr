---
title: ASP.NET core'da özel Model bağlama
author: ardalis
description: Model bağlama denetleyici eylemleri doğrudan model türleri içinde ASP.NET Core ile çalışmaya nasıl olanak tanıdığını öğrenin.
ms.author: riande
ms.date: 11/13/2018
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 1da42829270e8ff4a626a45aec4d4e825062bd4f
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635303"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="a954d-103">ASP.NET core'da özel Model bağlama</span><span class="sxs-lookup"><span data-stu-id="a954d-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="a954d-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a954d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a954d-105">Model bağlama (yöntemi bağımsız değişken olarak geçirilen), doğrudan model türleri yerine HTTP isteklerini daha çalışmak denetleyici eylemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="a954d-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="a954d-106">Gelen istek veri ve uygulama modelleri arasında eşleme model bağlayıcılar tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="a954d-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="a954d-107">Geliştiriciler, özel model bağlayıcıları (genellikle, kendi sağlayıcınızı yazmanız gerekmez ancak) uygulayarak yerleşik model bağlama işlevselliğini genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a954d-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="a954d-108">Github'dan örnek görüntüleme veya indirme</span><span class="sxs-lookup"><span data-stu-id="a954d-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="a954d-109">Varsayılan model Bağlayıcısı sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="a954d-109">Default model binder limitations</span></span>

<span data-ttu-id="a954d-110">Varsayılan model bağlayıcıları, genel .NET Core veri türlerinin çoğu destekler ve çoğu Geliştirici gereksinimlerini karşılamalıdır.</span><span class="sxs-lookup"><span data-stu-id="a954d-110">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="a954d-111">Giriş metin tabanlı, doğrudan model türleri istekten bağlamak beklerler.</span><span class="sxs-lookup"><span data-stu-id="a954d-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="a954d-112">Bu bağlama önce giriş dönüştürme gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="a954d-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="a954d-113">Örneğin, model verileri aramak için kullanılan bir anahtara sahip olduğunda.</span><span class="sxs-lookup"><span data-stu-id="a954d-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="a954d-114">Anahtara göre verileri getirmek için özel bir model bağlayıcısını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a954d-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="a954d-115">Model bağlama gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="a954d-115">Model binding review</span></span>

<span data-ttu-id="a954d-116">Model bağlama, üzerinde çalıştığı türleri için tanımları kullanır.</span><span class="sxs-lookup"><span data-stu-id="a954d-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="a954d-117">A *basit tür* girişte tek bir dize dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="a954d-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="a954d-118">A *karmaşık tür* birden çok giriş değerlerini dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="a954d-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="a954d-119">Varlığını temel farkı framework belirleyen bir `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="a954d-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="a954d-120">Oluşturduğunuz bir tür dönüştürücüsü basit varsa önerilen `string`  ->  `SomeType` dış kaynaklara gerektirmeyen eşleme.</span><span class="sxs-lookup"><span data-stu-id="a954d-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="a954d-121">Kendi özel bir model bağlayıcı oluşturmadan önce bu gözden geçirme nasıl var olan model bağlayıcıları uygulanan olur.</span><span class="sxs-lookup"><span data-stu-id="a954d-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="a954d-122">Göz önünde bulundurun [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) bayt dizileri base64 ile kodlanmış dizeleri dönüştürmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a954d-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="a954d-123">Bayt dizileri genellikle dosyaları ya da veritabanı BLOB alanları depolanır.</span><span class="sxs-lookup"><span data-stu-id="a954d-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="a954d-124">ByteArrayModelBinder ile çalışma</span><span class="sxs-lookup"><span data-stu-id="a954d-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="a954d-125">Dizeleri Base64 ile kodlanmış ikili verileri temsil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a954d-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="a954d-126">Örneğin, aşağıdaki görüntüde bir dize olarak kodlanmış.</span><span class="sxs-lookup"><span data-stu-id="a954d-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="a954d-127">![DotNet bot](custom-model-binding/images/bot.png "dotnet Robotu")</span><span class="sxs-lookup"><span data-stu-id="a954d-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="a954d-128">Kodlanmış dizeyi küçük bir kısmı, aşağıdaki görüntüde gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a954d-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="a954d-129">![dotnet bot kodlanmış](custom-model-binding/images/encoded-bot.png "kodlanmış dotnet Robotu")</span><span class="sxs-lookup"><span data-stu-id="a954d-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="a954d-130">Bölümündeki yönergeleri [örnek'ın Benioku](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) base64 ile kodlanmış dize bir dosyasına dönüştürmek için.</span><span class="sxs-lookup"><span data-stu-id="a954d-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="a954d-131">ASP.NET Core MVC base64 ile kodlanmış bir dize alabilir ve kullanmak bir `ByteArrayModelBinder` Bayt dizisine dönüştürmek için.</span><span class="sxs-lookup"><span data-stu-id="a954d-131">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="a954d-132">[ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) uygulayan [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) eşler `byte[]` bağımsız değişkenleri `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="a954d-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="a954d-133">Kendi özel bir model bağlayıcı oluşturulurken, kendi uygulayabileceğiniz `IModelBinderProvider` yazın veya [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="a954d-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="a954d-134">Aşağıdaki örnek nasıl kullanılacağını gösterir `ByteArrayModelBinder` base64 ile kodlanmış dizeye dönüştürmek için bir `byte[]` ve sonucu bir dosyaya kaydedin:</span><span class="sxs-lookup"><span data-stu-id="a954d-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="a954d-135">Base64 ile kodlanmış bir dize gibi bir araç kullanarak bu API yöntemine GÖNDEREBİLİR [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="a954d-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="a954d-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="a954d-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="a954d-137">Model bağlama, uygun şekilde adlandırılmış özellikleri ya da bağımsız değişkenler için bağlayıcı istek verileri bağlayabilirsiniz sürece başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="a954d-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="a954d-138">Aşağıdaki örnek nasıl kullanılacağını gösterir `ByteArrayModelBinder` görünüm modeli:</span><span class="sxs-lookup"><span data-stu-id="a954d-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="a954d-139">Özel model bağlayıcı örneği</span><span class="sxs-lookup"><span data-stu-id="a954d-139">Custom model binder sample</span></span>

<span data-ttu-id="a954d-140">Bu bölümde biz özel bir model bağlayıcısını uygulayacaksınız:</span><span class="sxs-lookup"><span data-stu-id="a954d-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="a954d-141">Gelen istek verileri kesin türü belirtilmiş anahtar bağımsız değişkenleriyle dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="a954d-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="a954d-142">İlişkili varlık getirmek için Entity Framework Core kullanır.</span><span class="sxs-lookup"><span data-stu-id="a954d-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="a954d-143">İlişkili varlık eylem yöntemi için bağımsız değişken olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="a954d-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="a954d-144">Aşağıdaki örnek kullanımları `ModelBinder` özniteliği `Author` modeli:</span><span class="sxs-lookup"><span data-stu-id="a954d-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="a954d-145">Önceki kodda, `ModelBinder` öznitelik türünü belirten `IModelBinder` bağlamak için kullanılmalıdır `Author` eylem parametreleri.</span><span class="sxs-lookup"><span data-stu-id="a954d-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span>

<span data-ttu-id="a954d-146">Aşağıdaki `AuthorEntityBinder` sınıfı bağlamalar bir `Author` Entity Framework Core kullanan bir veri kaynağından varlık getirilirken tarafından parametre ve bir `authorId`:</span><span class="sxs-lookup"><span data-stu-id="a954d-146">The following `AuthorEntityBinder` class binds an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> <span data-ttu-id="a954d-147">Önceki `AuthorEntityBinder` sınıfı özel bir model bağlayıcısını göstermek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a954d-147">The preceding `AuthorEntityBinder` class is intended to illustrate a custom model binder.</span></span> <span data-ttu-id="a954d-148">Sınıfı, arama senaryo için en iyi yöntemleri göstermek için tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="a954d-148">The class isn't intended to illustrate best practices for a lookup scenario.</span></span> <span data-ttu-id="a954d-149">Arama için bağlama `authorId` ve bir eylem yöntemi veritabanında sorgu.</span><span class="sxs-lookup"><span data-stu-id="a954d-149">For lookup, bind the `authorId` and query the database in an action method.</span></span> <span data-ttu-id="a954d-150">Bu yaklaşım, model bağlama hatalardan ayıran `NotFound` durumları.</span><span class="sxs-lookup"><span data-stu-id="a954d-150">This approach separates model binding failures from `NotFound` cases.</span></span>

<span data-ttu-id="a954d-151">Aşağıdaki kod nasıl kullanılacağını gösterir `AuthorEntityBinder` bir eylem yöntemindeki:</span><span class="sxs-lookup"><span data-stu-id="a954d-151">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="a954d-152">`ModelBinder` Özniteliği uygulamak için kullanılabilir `AuthorEntityBinder` varsayılan kuralları kullanmayan parametreleri için:</span><span class="sxs-lookup"><span data-stu-id="a954d-152">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="a954d-153">Bu örnekte, varsayılan bağımsız değişken adını değil bu yana `authorId`, parametresini kullanarak belirtilen `ModelBinder` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a954d-153">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="a954d-154">Denetleyici ve eylem yöntemi Basitleştirilmiş, eylem yönteminin varlığı bakmak için karşılaştırıldığında unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a954d-154">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="a954d-155">Entity Framework Core kullanan Yazar getirilecek mantıksal model bağlayıcıya taşınır.</span><span class="sxs-lookup"><span data-stu-id="a954d-155">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="a954d-156">Bağlama çeşitli yöntemler varsa, bu önemli ölçüde basitleştirme olabilir `Author` model ve izlemek için yardımcı olabileceğini [KURU ilkesine](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="a954d-156">This can be a considerable simplification when you have several methods that bind to the `Author` model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="a954d-157">Uygulayabileceğiniz `ModelBinder` özniteliği için ayrı modeli özellikleri (gibi bir viewmodel üzerinde) veya olarak eylem metodu parametreleriyle belirli bir model bağlayıcı veya yalnızca bu tür veya eylem için model adını belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="a954d-157">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="a954d-158">Bir ModelBinderProvider uygulama</span><span class="sxs-lookup"><span data-stu-id="a954d-158">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="a954d-159">Uygulayabileceğiniz bir öznitelik uygulamak yerine `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="a954d-159">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="a954d-160">Yerleşik bir çerçeve bağlayıcıları nasıl uygulandığını budur.</span><span class="sxs-lookup"><span data-stu-id="a954d-160">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="a954d-161">Türü belirttiğinizde, bağlayıcı üzerinde çalışır, bu üretir, bağımsız değişken türünü belirtin **değil** , bağlayıcı giriş kabul eder.</span><span class="sxs-lookup"><span data-stu-id="a954d-161">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="a954d-162">Aşağıdaki bağlayıcıyı çalışır `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="a954d-162">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="a954d-163">MVC'nin sağlayıcıları koleksiyonuna eklendiğinde kullanmanız gerekmez `ModelBinder` özniteliği `Author` veya `Author`-yazılan parametreleri.</span><span class="sxs-lookup"><span data-stu-id="a954d-163">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author`-typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="a954d-164">Not: Yukarıdaki kod döndürür bir `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="a954d-164">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="a954d-165">`BinderTypeModelBinder` model bağlayıcıları için bir üreteci davranır ve bağımlılık ekleme (dı) sağlar.</span><span class="sxs-lookup"><span data-stu-id="a954d-165">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="a954d-166">`AuthorEntityBinder` EF Core erişmeye DI gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a954d-166">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="a954d-167">Kullanım `BinderTypeModelBinder` , model bağlayıcı DI hizmetlerinden gerekiyorsa.</span><span class="sxs-lookup"><span data-stu-id="a954d-167">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="a954d-168">Özel model bağlayıcı sağlayıcısı kullanmak için ekleyebilirsiniz `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a954d-168">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="a954d-169">Model bağlayıcıları değerlendirirken sağlayıcıları koleksiyonu sırayla incelenir.</span><span class="sxs-lookup"><span data-stu-id="a954d-169">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="a954d-170">Bir bağlayıcı döndüren ilk sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a954d-170">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="a954d-171">Aşağıdaki görüntüde varsayılan hata ayıklayıcı'dan model bağlayıcıları gösterir.</span><span class="sxs-lookup"><span data-stu-id="a954d-171">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="a954d-172">![Varsayılan model bağlayıcıları](custom-model-binding/images/default-model-binders.png "varsayılan model bağlayıcıları")</span><span class="sxs-lookup"><span data-stu-id="a954d-172">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="a954d-173">Sağlayıcınız koleksiyonun sonuna ekleme, özel bağlayıcı şansı olmadan önce çağrılan yerleşik bir model bağlayıcı neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a954d-173">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="a954d-174">Bu örnekte, özel bir sağlayıcı için kullanıldığı emin olmak için koleksiyon başına eklenir `Author` eylem bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a954d-174">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="a954d-175">Öneriler ve en iyi uygulamalar</span><span class="sxs-lookup"><span data-stu-id="a954d-175">Recommendations and best practices</span></span>

<span data-ttu-id="a954d-176">Özel model bağlayıcıları:</span><span class="sxs-lookup"><span data-stu-id="a954d-176">Custom model binders:</span></span>

- <span data-ttu-id="a954d-177">Durum kodları ayarlamak veya sonuçları döndürmek çalışmayın (örneğin, 404 bulunamadı).</span><span class="sxs-lookup"><span data-stu-id="a954d-177">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="a954d-178">Model bağlama başarısız olursa bir [eylem filtresi](xref:mvc/controllers/filters) veya eylem yöntemi bir mantık hatası işleme.</span><span class="sxs-lookup"><span data-stu-id="a954d-178">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="a954d-179">Yinelenen kod ve geniş kapsamlı kritik konular eylem yöntemlerinden ortadan kaldırmak için ekseriyetle faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="a954d-179">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="a954d-180">Genellikle bir dize özel bir türe dönüştürmek için kullanılmaması bir [ `TypeConverter` ](/dotnet/api/system.componentmodel.typeconverter) genellikle daha iyi bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="a954d-180">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
