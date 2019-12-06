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
# <a name="custom-model-binding-in-aspnet-core"></a>ASP.NET Core özel model bağlama

[Steve Smith](https://ardalis.com/) tarafından

Model bağlama, denetleyici eylemlerinin HTTP istekleri yerine model türleriyle doğrudan (Yöntem bağımsız değişkenleri olarak geçirilir) çalışmasına izin verir. Gelen istek verileri ve uygulama modelleri arasındaki eşleme, model ciltleri tarafından işlenir. Geliştiriciler, özel model ciltlerinizi uygulayarak yerleşik model bağlama işlevini genişletebilir (genellikle kendi sağlayıcınızı yazmanız gerekmez).

[GitHub 'dan örnek görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Varsayılan model Ciltçi sınırlamaları

Varsayılan model ciltleri ortak .NET Core veri türlerinin çoğunu destekler ve çoğu geliştiricilerin ihtiyaçlarını karşılamaları gerekir. İstek içinden doğrudan model türlerine metin tabanlı giriş bağlamayı bekler. Bağlamayı bağlamadan önce girişi dönüştürmeniz gerekebilir. Örneğin, model verilerini aramak için kullanılabilecek bir anahtarınız olduğunda. Anahtar temelinde veri getirmek için özel bir model Bağlayıcısı kullanabilirsiniz.

## <a name="model-binding-review"></a>Model bağlama incelemesi

Model bağlama, üzerinde çalıştığı türler için belirli tanımları kullanır. *Basit bir tür* , girişte tek bir dizeden dönüştürülür. *Karmaşık bir tür* birden çok giriş değerinden dönüştürülür. Framework, bir `TypeConverter`varlığına göre farkı belirler. Dış kaynak gerektirmeyen basit bir `string` -> `SomeType` eşlemeye sahipseniz bir tür dönüştürücüsü oluşturmanız önerilir.

Kendi özel model cilinkini oluşturmadan önce, mevcut model ciltçileri 'nin nasıl uygulandığını gözden geçirdik. Base64 ile kodlanmış dizeleri bayt dizilerine dönüştürmek için kullanılabilecek [Bytearraymodelciltçi](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) 'yi düşünün. Bayt dizileri genellikle dosya veya veritabanı blobu alanları olarak depolanır.

### <a name="working-with-the-bytearraymodelbinder"></a>Bytearraymodelciltçi ile çalışma

İkili verileri temsil etmek için Base64 kodlamalı dizeler kullanılabilir. Örneğin, aşağıdaki resim bir dize olarak kodlanabilir.

![DotNet bot](custom-model-binding/images/bot.png "DotNet bot")

Kodlanmış dizenin küçük bir kısmı aşağıdaki görüntüde gösterilmiştir:

![DotNet bot kodlamalı](custom-model-binding/images/encoded-bot.png "DotNet bot kodlamalı")

Base64 ile kodlanmış dizeyi bir dosyaya dönüştürmek için [ÖRNEĞIN Benioku](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) dosyasındaki yönergeleri izleyin.

ASP.NET Core MVC, Base64 kodlamalı bir dize alabilir ve bir bayt dizisine dönüştürmek için bir `ByteArrayModelBinder` kullanabilir. [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) Maps uygulayan [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) , `byte[]` bağımsız değişkenleri `ByteArrayModelBinder`:

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

Kendi özel model cilinkini oluştururken kendi `IModelBinderProvider` türünü uygulayabilir veya [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute)kullanabilirsiniz.

Aşağıdaki örnek, Base64 kodlamalı bir dizeyi bir `byte[]` dönüştürmek ve sonucu bir dosyaya kaydetmek için `ByteArrayModelBinder` nasıl kullanacağınızı gösterir:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

[Postman](https://www.getpostman.com/)gibi bir araç kullanarak bu API yöntemine Base64 kodlamalı bir dize gönderebilirsiniz:

![Postman](custom-model-binding/images/postman.png "Postman")

Bağlayıcı, istek verilerini uygun şekilde adlandırılmış özelliklere veya bağımsız değişkenlere bağlayabildiğinden, model bağlama başarılı olur. Aşağıdaki örnek, `ByteArrayModelBinder` bir görünüm modeliyle nasıl kullanacağınızı gösterir:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Özel model Ciltçi örneği

Bu bölümde, özel bir model cildi uygulayacağız:

- Gelen istek verilerini kesin belirlenmiş anahtar bağımsız değişkenlerine dönüştürür.
- İlişkili varlığı getirmek için Entity Framework Core kullanır.
- İlişkili varlığı eylem yöntemine bir bağımsız değişken olarak geçirir.

Aşağıdaki örnek, `Author` modelinde `ModelBinder` özniteliğini kullanır:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

Yukarıdaki kodda `ModelBinder` özniteliği, `Author` eylem parametrelerini bağlamak için kullanılması gereken `IModelBinder` türünü belirtir.

Aşağıdaki `AuthorEntityBinder` sınıfı, Entity Framework Core ve `authorId`kullanarak bir veri kaynağından varlığı getirerek bir `Author` parametresini bağlar:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> Önceki `AuthorEntityBinder` sınıfı özel bir model cildi göstermek için tasarlanmıştır. Sınıfı, bir arama senaryosu için en iyi yöntemleri göstermeye yönelik değildir. Arama için `authorId` bağlayın ve veritabanını bir eylem yöntemine sorgulayın. Bu yaklaşım, model bağlama başarısızlıklarını `NotFound` durumlardan ayırır.

Aşağıdaki kod, `AuthorEntityBinder` bir eylem yönteminde nasıl kullanılacağını gösterir:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

`ModelBinder` özniteliği, varsayılan kuralları kullanmayan parametrelere `AuthorEntityBinder` uygulamak için kullanılabilir:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

Bu örnekte, bağımsız değişkenin adı varsayılan `authorId`olmadığından, parametresinde `ModelBinder` özniteliği kullanılarak belirtilir. Hem denetleyici hem de eylem yöntemi, eylem yöntemindeki varlığı aramaya kıyasla basitleştirilmiştir. Entity Framework Core kullanarak yazarı getirme mantığı model cilde taşınır. Bu, `Author` modeline bağlanan çeşitli yöntemlere sahip olduğunuzda önemli bir basitleştirme olabilir.

Yalnızca bu tür veya eylem için belirli bir model Bağlayıcısı veya model adı belirtmek için, `ModelBinder` özniteliğini tek tek model özelliklerine (ViewModel üzerinde gibi) veya eylem yöntemi parametrelerine uygulayabilirsiniz.

### <a name="implementing-a-modelbinderprovider"></a>ModelBinderProvider uygulama

Bir özniteliği uygulamak yerine, `IModelBinderProvider`uygulayabilirsiniz. Yerleşik çerçeve ciltçileri uygulanır. Cildin üzerinde çalıştığı türü belirttiğinizde, cildin kabul ettiği girişi **değil** , oluşturduğu bağımsız değişkenin türünü belirtirsiniz. Aşağıdaki Ciltçi sağlayıcısı `AuthorEntityBinder`ile birlikte kullanılabilir. MVC 'nin sağlayıcılar koleksiyonuna eklendiğinde, `Author` veya `Author`türü belirlenmiş parametrelerde `ModelBinder` özniteliğini kullanmanız gerekmez.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Note: Yukarıdaki kod bir `BinderTypeModelBinder`döndürür. `BinderTypeModelBinder`, model ciltleri için bir fabrika görevi görür ve bağımlılık ekleme (DI) sağlar. `AuthorEntityBinder`, EF Core erişim için dı gerektiriyor. Model Ciltçi 'nin dı 'den hizmet gerektirmesi durumunda `BinderTypeModelBinder` kullanın.

Özel model Ciltçi sağlayıcısını kullanmak için, `ConfigureServices`içine ekleyin:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Model ciltleri değerlendirilirken, sağlayıcı koleksiyonu sırayla incelenir. Bir cildi döndüren ilk sağlayıcı kullanılır.

Aşağıdaki görüntüde, hata ayıklayıcıdan varsayılan model ciltçileri gösterilmektedir.

![Varsayılan model ciltleri](custom-model-binding/images/default-model-binders.png "Varsayılan model ciltleri")

Sağlayıcınızı koleksiyonun sonuna eklemek, özel ciltçinin bir şansı olmadan önce yerleşik bir model cilde yol açabilir. Bu örnekte, özel sağlayıcı, `Author` eylem bağımsız değişkenleri için kullanıldığından emin olmak için koleksiyonun başlangıcına eklenir.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

### <a name="polymorphic-model-binding"></a>Polimorfik model bağlama

Türetilmiş türlerin farklı modellerine bağlama polimorfik model bağlama olarak bilinir. İstek değeri, belirli türetilmiş model türüne bağlanması gerektiğinde polimorfik özel model bağlama gerekir. Polimorfik model bağlama:

* Tüm dillerle birlikte çalışmak üzere tasarlanan bir REST API için tipik değildir.
* , Bağlantılı modeller hakkında neden olmasını zorlaştırır.

Ancak, bir uygulama çok biçimli model bağlama gerektiriyorsa, bir uygulama aşağıdaki koda benzeyebilir:

[!code-csharp[](custom-model-binding/3.0sample/PolymorphicModelBinding/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a>Öneriler ve en iyi uygulamalar

Özel model ciltleri:

- Durum kodları veya dönüş sonuçları ayarlanmamalıdır (örneğin, 404 bulunamadı). Model bağlama başarısız olursa, eylem yönteminin kendisi içindeki bir [eylem filtresinin](xref:mvc/controllers/filters) veya mantığının hata işlemesi gerekir.
- , Yinelenen kodu ve eylem yöntemlerinden çapraz kesme sorunlarını ortadan kaldırmak için en yararlı seçenektir.
- Genellikle bir dizeyi özel bir türe dönüştürmek için kullanılmamalıdır, bir [TypeConverter](/dotnet/api/system.componentmodel.typeconverter) genellikle daha iyi bir seçenektir.
