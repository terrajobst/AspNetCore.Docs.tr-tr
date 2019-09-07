---
title: ASP.NET Core özel model bağlama
author: ardalis
description: Model bağlamanın, denetleyici eylemlerinin ASP.NET Core doğrudan model türleriyle nasıl çalışmasına izin verdiğini öğrenin.
ms.author: riande
ms.date: 11/13/2018
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 27e19012b6f878f5e3d08846593a7513bd584a4c
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773494"
---
# <a name="custom-model-binding-in-aspnet-core"></a>ASP.NET Core özel model bağlama

[Steve Smith](https://ardalis.com/) tarafından

Model bağlama, denetleyici eylemlerinin HTTP istekleri yerine model türleriyle doğrudan (Yöntem bağımsız değişkenleri olarak geçirilir) çalışmasına izin verir. Gelen istek verileri ve uygulama modelleri arasındaki eşleme, model ciltleri tarafından işlenir. Geliştiriciler, özel model ciltlerinizi uygulayarak yerleşik model bağlama işlevini genişletebilir (genellikle kendi sağlayıcınızı yazmanız gerekmez).

[GitHub 'dan örnek görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Varsayılan model Ciltçi sınırlamaları

Varsayılan model ciltleri ortak .NET Core veri türlerinin çoğunu destekler ve çoğu geliştiricilerin ihtiyaçlarını karşılamaları gerekir. İstek içinden doğrudan model türlerine metin tabanlı giriş bağlamayı bekler. Bağlamayı bağlamadan önce girişi dönüştürmeniz gerekebilir. Örneğin, model verilerini aramak için kullanılabilecek bir anahtarınız olduğunda. Anahtar temelinde veri getirmek için özel bir model Bağlayıcısı kullanabilirsiniz.

## <a name="model-binding-review"></a>Model bağlama incelemesi

Model bağlama, üzerinde çalıştığı türler için belirli tanımları kullanır. *Basit bir tür* , girişte tek bir dizeden dönüştürülür. *Karmaşık bir tür* birden çok giriş değerinden dönüştürülür. Framework, a `TypeConverter`'nın varlığına göre farkı belirler. Dış kaynak gerektirmeyen basit `string`  ->  `SomeType` bir eşlemeye sahipseniz bir tür dönüştürücüsü oluşturmanız önerilir.

Kendi özel model cilinkini oluşturmadan önce, mevcut model ciltçileri 'nin nasıl uygulandığını gözden geçirdik. Base64 ile kodlanmış dizeleri bayt dizilerine dönüştürmek için kullanılabilecek [Bytearraymodelciltçi](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) 'yi düşünün. Bayt dizileri genellikle dosya veya veritabanı blobu alanları olarak depolanır.

### <a name="working-with-the-bytearraymodelbinder"></a>Bytearraymodelciltçi ile çalışma

İkili verileri temsil etmek için Base64 kodlamalı dizeler kullanılabilir. Örneğin, aşağıdaki resim bir dize olarak kodlanabilir.

![DotNet bot](custom-model-binding/images/bot.png "DotNet bot")

Kodlanmış dizenin küçük bir kısmı aşağıdaki görüntüde gösterilmiştir:

![DotNet bot kodlamalı](custom-model-binding/images/encoded-bot.png "DotNet bot kodlamalı")

Base64 ile kodlanmış dizeyi bir dosyaya dönüştürmek için [ÖRNEĞIN Benioku](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) dosyasındaki yönergeleri izleyin.

ASP.NET Core MVC, Base64 kodlamalı bir dize alabilir ve bunu bir `ByteArrayModelBinder` bayt dizisine dönüştürmek için kullanabilir. [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) `byte[]` [eşleme bağımsız değişkenlerini uygulayan ByteArrayModelBinderProvider:](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) `ByteArrayModelBinder`

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

Aşağıdaki örnek, bir base64 kodlu dizeyi `ByteArrayModelBinder` `byte[]` öğesine dönüştürmek ve sonucu bir dosyaya kaydetmek için nasıl kullanılacağını gösterir:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

[Postman](https://www.getpostman.com/)gibi bir araç kullanarak bu API yöntemine Base64 kodlamalı bir dize gönderebilirsiniz:

![Postman](custom-model-binding/images/postman.png "Postman")

Bağlayıcı, istek verilerini uygun şekilde adlandırılmış özelliklere veya bağımsız değişkenlere bağlayabildiğinden, model bağlama başarılı olur. Aşağıdaki örnek, bir görünüm modeliyle nasıl `ByteArrayModelBinder` kullanılacağını gösterir:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Özel model Ciltçi örneği

Bu bölümde, özel bir model cildi uygulayacağız:

- Gelen istek verilerini kesin belirlenmiş anahtar bağımsız değişkenlerine dönüştürür.
- İlişkili varlığı getirmek için Entity Framework Core kullanır.
- İlişkili varlığı eylem yöntemine bir bağımsız değişken olarak geçirir.

Aşağıdaki örnek `ModelBinder` `Author` modeldeki özniteliğini kullanır:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

Yukarıdaki kodda `ModelBinder` öznitelik, eylem parametrelerini bağlamak `Author` için kullanılması gereken `IModelBinder` türünü belirtir.

Aşağıdaki `AuthorEntityBinder` sınıf, Entity Framework Core ve `Author` ' i kullanarak bir veri `authorId`kaynağından varlığı getirerek bir parametreyi bağlar:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> Önceki `AuthorEntityBinder` sınıf özel bir model cildi göstermeye yöneliktir. Sınıfı, bir arama senaryosu için en iyi yöntemleri göstermeye yönelik değildir. Arama için, veritabanını bağlayın `authorId` ve bir eylem yöntemine sorgulayın. Bu yaklaşım, model bağlama başarısızlıklarını `NotFound` durumlardan ayırır.

Aşağıdaki kod, `AuthorEntityBinder` bir eylem yönteminin nasıl kullanılacağını gösterir:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

Özniteliği, varsayılan kuralları kullanmayan parametreleri `AuthorEntityBinder` uygulamak için kullanılabilir: `ModelBinder`

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

Bu örnekte, bağımsız değişkenin adı varsayılan `authorId`olmadığından parametresi `ModelBinder` kullanılarak parametresi belirtilir. Hem denetleyici hem de eylem yöntemi, eylem yöntemindeki varlığı aramaya kıyasla basitleştirilmiştir. Entity Framework Core kullanarak yazarı getirme mantığı model cilde taşınır. Bu, `Author` modele bağlanan çeşitli yöntemlere sahip olduğunuzda büyük ölçüde basitleştirmesi olabilir.

Özniteliği, `ModelBinder` yalnızca bu tür veya eylem için belirli bir model cildi veya model adı belirtmek üzere tek tek model özelliklerine (ViewModel üzerinde gibi) veya eylem yöntemi parametrelerine uygulayabilirsiniz.

### <a name="implementing-a-modelbinderprovider"></a>ModelBinderProvider uygulama

Bir özniteliği uygulamak yerine, öğesini uygulayabilirsiniz `IModelBinderProvider`. Yerleşik çerçeve ciltçileri uygulanır. Cildin üzerinde çalıştığı türü belirttiğinizde, cildin kabul ettiği girişi **değil** , oluşturduğu bağımsız değişkenin türünü belirtirsiniz. Aşağıdaki Ciltçi sağlayıcısı ile birlikte `AuthorEntityBinder`kullanılabilir. MVC 'nin sağlayıcılar koleksiyonuna eklendiğinde, `ModelBinder` `Author` veya `Author`türü belirlenmiş parametrelerde özniteliğini kullanmanız gerekmez.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Not: Yukarıdaki kod bir `BinderTypeModelBinder`döndürür. `BinderTypeModelBinder`Model ciltleri için bir fabrika işlevi görür ve bağımlılık ekleme (dı) sağlar. İçin dı 'nin EF Core erişimi gerekir.`AuthorEntityBinder` Model `BinderTypeModelBinder` Ciltçi 'nin dı 'den hizmet gerektirmesi durumunda kullanın.

Özel model Ciltçi sağlayıcısını kullanmak için, içine `ConfigureServices`ekleyin:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Model ciltleri değerlendirilirken, sağlayıcı koleksiyonu sırayla incelenir. Bir cildi döndüren ilk sağlayıcı kullanılır.

Aşağıdaki görüntüde, hata ayıklayıcıdan varsayılan model ciltçileri gösterilmektedir.

![varsayılan model ciltleri](custom-model-binding/images/default-model-binders.png "varsayılan model ciltleri")

Sağlayıcınızı koleksiyonun sonuna eklemek, özel ciltçinin bir şansı olmadan önce yerleşik bir model cilde yol açabilir. Bu örnekte, özel sağlayıcı, eylem bağımsız değişkenleri için `Author` kullanıldığından emin olmak için koleksiyonun başlangıcına eklenir.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

### <a name="polymorphic-model-binding"></a>Polimorfik model bağlama

Türetilmiş türlerin farklı modellerine bağlama polimorfik model bağlama olarak bilinir. İstek değeri, belirli türetilmiş model türüne bağlanmalıdır, özel model bağlama gerekir. Bu yaklaşım gerekli olmadığı takdirde polimorfik model bağlamasını önlemeyi öneririz. Polimorfik model bağlama, ilişkili modeller hakkında neden olmasını zorlaştırır. Ancak, bir uygulama çok biçimli model bağlama gerektiriyorsa, bir uygulama aşağıdaki koda benzeyebilir:

[!code-csharp[](custom-model-binding/3.0sample/PolymorphicModelBinding/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a>Öneriler ve en iyi uygulamalar

Özel model ciltleri:

- Durum kodları veya dönüş sonuçları ayarlanmamalıdır (örneğin, 404 bulunamadı). Model bağlama başarısız olursa, eylem yönteminin kendisi içindeki bir [eylem filtresinin](xref:mvc/controllers/filters) veya mantığının hata işlemesi gerekir.
- , Yinelenen kodu ve eylem yöntemlerinden çapraz kesme sorunlarını ortadan kaldırmak için en yararlı seçenektir.
- Genellikle bir [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) dizeyi özel bir türe dönüştürmek için kullanılmamalıdır, genellikle daha iyi bir seçenektir.
