---
title: ASP.NET Core özel Model bağlamanın
author: ardalis
description: Model bağlama ASP.NET Core modeli türleri ile doğrudan çalışmak denetleyici eylemleri nasıl olanak tanıdığını öğrenin.
manager: wpickett
ms.author: riande
ms.date: 04/10/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: a687753083d3b11898e9ff35828780a5ad240854
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="4f6e4-103">ASP.NET Core özel Model bağlamanın</span><span class="sxs-lookup"><span data-stu-id="4f6e4-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="4f6e4-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4f6e4-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4f6e4-105">Model bağlama türleriyle (yöntemi bağımsız değişken olarak geçirilen), doğrudan modeli yerine HTTP isteklerini daha çalışmak denetleyici eylemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="4f6e4-106">Gelen istek veri ve uygulama modelleri arasında eşleme model bağlayıcılar tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="4f6e4-107">Geliştiriciler, özel model bağlayıcıları (genellikle, kendi sağlayıcınızı yazmanız gerekmez ancak) uygulayarak yerleşik model bağlama işlevini genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="4f6e4-108">Görüntülemek veya örnek Github'dan indirin</span><span class="sxs-lookup"><span data-stu-id="4f6e4-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="4f6e4-109">Varsayılan model Bağlayıcısı sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="4f6e4-109">Default model binder limitations</span></span>

<span data-ttu-id="4f6e4-110">Varsayılan model bağlayıcıları ortak .NET Core veri türleri çoğunu destekler ve çoğu Geliştirici gereksinimlerini karşılamalıdır.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="4f6e4-111">Metin tabanlı giriş istekten doğrudan model türleri bağlamak bekler.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="4f6e4-112">Bu bağlama önce giriş dönüştürme gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="4f6e4-113">Örneğin, model verileri aramak için kullanılan bir anahtara sahip olduğunda.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="4f6e4-114">Anahtara göre verileri getirmek için özel bir model bağlayıcısını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="4f6e4-115">Model bağlama gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="4f6e4-115">Model binding review</span></span>

<span data-ttu-id="4f6e4-116">Model bağlama, üzerinde çalıştığı türleri için belirli tanımları kullanır.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="4f6e4-117">A *basit tür* girişte tek bir dizeden dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="4f6e4-118">A *karmaşık tür* birden çok giriş değerleri dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="4f6e4-119">Framework varlığını temel fark belirler bir `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="4f6e4-120">Oluşturduğunuz tür dönüştürücüsünü basit bir varsa önerilen `string`  ->  `SomeType` dış kaynaklar gerektirmeyen eşleme.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="4f6e4-121">Kendi özel model bağlayıcısını oluşturmadan önce bu gözden geçirme nasıl varolan model bağlayıcıları uygulanan olur.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="4f6e4-122">Göz önünde bulundurun [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) base64 ile kodlanmış dize bayt diziye dönüştürmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="4f6e4-123">Bayt dizileri genellikle dosyaları ya da veritabanı BLOB alanları depolanır.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="4f6e4-124">ByteArrayModelBinder ile çalışma</span><span class="sxs-lookup"><span data-stu-id="4f6e4-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="4f6e4-125">Base64 kodlu dize ikili verileri göstermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="4f6e4-126">Örneğin, aşağıdaki resimde bir dize olarak kodlanmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="4f6e4-127">![DotNet bot](custom-model-binding/images/bot.png "dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="4f6e4-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="4f6e4-128">Aşağıdaki görüntüde kodlu bir dize küçük bir bölümü gösterilir:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="4f6e4-129">![Kodlanmış dotnet bot](custom-model-binding/images/encoded-bot.png "kodlanmış dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="4f6e4-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="4f6e4-130">' Ndaki yönergeleri izleyin [örnek'ın Benioku dosyası](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) base64 ile kodlanmış dize bir dosyasına dönüştürmek için.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="4f6e4-131">ASP.NET Core MVC base64 ile kodlanmış bir dize alabilir ve kullanmak bir `ByteArrayModelBinder` bir bayt dizisine dönüştürme.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="4f6e4-132">[ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) hangi uygulayan [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) eşlemeleri `byte[]` bağımsız değişkenleri `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="4f6e4-133">Kendi özel model bağlayıcı oluşturulurken, kendi uygulayabileceğiniz `IModelBinderProvider` yazın veya [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="4f6e4-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="4f6e4-134">Aşağıdaki örnekte nasıl kullanılacağını gösterir `ByteArrayModelBinder` base64 ile kodlanmış bir dizeye dönüştürmek için bir `byte[]` ve sonucu bir dosyaya kaydedin:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="4f6e4-135">Base64 ile kodlanmış bir dize gibi bir araç kullanarak bu API yöntemine NAKLEDEBİLİRSİNİZ [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="4f6e4-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="4f6e4-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="4f6e4-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="4f6e4-137">Bağlayıcı istek verileri uygun adlandırılmış özellikleri veya bağımsız değişkenler bağlayabilir sürece, model bağlama işlemi başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="4f6e4-138">Aşağıdaki örnekte nasıl kullanılacağını gösterir `ByteArrayModelBinder` bir görünüm model ile:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="4f6e4-139">Özel model bağlayıcı örneği</span><span class="sxs-lookup"><span data-stu-id="4f6e4-139">Custom model binder sample</span></span>

<span data-ttu-id="4f6e4-140">Bu bölümde biz özel bir model bağlayıcısını uygulamak:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="4f6e4-141">Gelen istek verileri kesin türü belirtilmiş anahtar bağımsız değişkenleriyle dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="4f6e4-142">İlişkili varlık getirmek Entity Framework Çekirdek kullanır.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="4f6e4-143">İlişkili varlık eylem yöntemi için bağımsız değişken olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="4f6e4-144">Aşağıdaki örnek kullanır `ModelBinder` özniteliği `Author` modeli:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="4f6e4-145">Önceki kod `ModelBinder` öznitelik türünü belirten `IModelBinder` bağlamak için kullanılmalıdır `Author` eylem parametrelerini.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="4f6e4-146">`AuthorEntityBinder` Bağlamak için kullanılan bir `Author` varlık Entity Framework Çekirdek kullanarak bir veri kaynağından getirme tarafından parametre ve bir `authorId`:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="4f6e4-147">Aşağıdaki kodu nasıl kullanılacağını gösterir `AuthorEntityBinder` bir eylem yöntemindeki:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="4f6e4-148">`ModelBinder` Özniteliği uygulamak için kullanılabilir `AuthorEntityBinder` varsayılan kuralları kullanmayan parametreleri için:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="4f6e4-149">Bu örnekte, varsayılan bağımsız değişken adını değil bu yana `authorId`, parametresi kullanılarak belirtilen `ModelBinder` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="4f6e4-150">Denetleyici ve eylem yöntemi Basitleştirilmiş, eylem yöntemi varlıkta bakmak için karşılaştırıldığında unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="4f6e4-151">Entity Framework Çekirdek kullanan Yazar getirmek için mantığı için model bağlayıcı taşınır.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="4f6e4-152">Bağlama Yazar modeline ve izlemek için yardımcı olabilir çeşitli yöntemler varsa, bu önemli ölçüde kolaylaştırma olabilir [KURU ilkesine](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="4f6e4-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="4f6e4-153">Uygulayabileceğiniz `ModelBinder` özniteliği tek tek model özelliklerine (gibi bir viewmodel üzerinde) veya belirli bir model bağlayıcı veya model adı. yalnızca bu tür veya eylemi belirlemek için eylem yöntemi parametrelerine.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="4f6e4-154">Bir ModelBinderProvider uygulama</span><span class="sxs-lookup"><span data-stu-id="4f6e4-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="4f6e4-155">Bir öznitelik uygulamak yerine, uygulayabileceğiniz `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="4f6e4-156">Yerleşik bir çerçeve bağlayıcıları nasıl uygulandığını budur.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="4f6e4-157">Türü belirttiğinizde, bağlayıcı üzerinde çalışır, bu üretir, bağımsız değişken türü belirttiğiniz **değil** giriş, bağlayıcı kabul eder.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="4f6e4-158">Aşağıdaki bağlayıcı sağlayıcısı çalışır `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="4f6e4-159">MVC'ın sağlayıcıları koleksiyonuna eklendiğinde kullanmanız gerekmez `ModelBinder` özniteliği `Author` veya `Author` parametreleri belirtilmiş.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="4f6e4-160">Not: Yukarıdaki kod döndüren bir `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="4f6e4-161">`BinderTypeModelBinder` model bağlayıcıları için bir üreteci olarak davranır ve bağımlılık ekleme (dı) sağlar.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="4f6e4-162">`AuthorEntityBinder` EF çekirdek erişmek dı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="4f6e4-163">Kullanım `BinderTypeModelBinder` , model bağlayıcı dı Hizmetleri'nden gerektiriyorsa.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="4f6e4-164">Bir özel model bağlayıcı sağlayıcısı kullanmak için bunu ekleyin `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="4f6e4-165">Model bağlayıcıları değerlendirirken, sağlayıcıları koleksiyonu sırayla incelenir.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="4f6e4-166">Bir bağlayıcı döndürür ilk sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="4f6e4-167">Aşağıdaki resimde varsayılan model bağlayıcıları hata ayıklayıcı gelen gösterir.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="4f6e4-168">![Varsayılan model bağlayıcıları](custom-model-binding/images/default-model-binders.png "varsayılan model bağlayıcıları")</span><span class="sxs-lookup"><span data-stu-id="4f6e4-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="4f6e4-169">Topluluğun sonuna sağlayıcınız ekleme özel bağlayıcı sıkıştırılabilmesi önce çağrılan yerleşik model bağlayıcı içinde neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="4f6e4-170">Bu örnekte, özel sağlayıcı için kullanıldığından emin olmak için koleksiyonunun başına eklenir `Author` eylem bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="4f6e4-171">Öneriler ve en iyi yöntemler</span><span class="sxs-lookup"><span data-stu-id="4f6e4-171">Recommendations and best practices</span></span>

<span data-ttu-id="4f6e4-172">Özel model bağlayıcıları:</span><span class="sxs-lookup"><span data-stu-id="4f6e4-172">Custom model binders:</span></span>
- <span data-ttu-id="4f6e4-173">Durum kodları ayarlamak veya sonuçları döndürmek çalışmayın (örneğin, 404 bulunamadı).</span><span class="sxs-lookup"><span data-stu-id="4f6e4-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="4f6e4-174">Model bağlama başarısız olursa, bir [eylem filtresi](xref:mvc/controllers/filters) veya eylem yöntemi mantık hata işleme.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="4f6e4-175">Yinelenen kodu ve eylem yöntemleri arası kesme kaygılarını ortadan kaldırmak için çok kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="4f6e4-176">Genellikle bir özel tür bir dize dönüştürmek için kullanılmaması bir [ `TypeConverter` ](/dotnet/api/system.componentmodel.typeconverter) genellikle daha iyi bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="4f6e4-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
