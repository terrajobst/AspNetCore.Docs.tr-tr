---
title: "Özel Model bağlama"
author: ardalis
description: "ASP.NET Core MVC model bağlamanın özelleştirme."
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: d8b94f53954c5ab63ccf3aab4eb7a7a7dbea487b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="custom-model-binding"></a>Özel Model bağlama

Tarafından [Steve Smith](https://ardalis.com/)

Model bağlama türleriyle (yöntemi bağımsız değişken olarak geçirilen), doğrudan modeli yerine HTTP isteklerini daha çalışmak denetleyici eylemleri sağlar. Gelen istek veri ve uygulama modelleri arasında eşleme model bağlayıcılar tarafından işlenir. Geliştiriciler, özel model bağlayıcıları (genellikle, kendi sağlayıcınızı yazmanız gerekmez ancak) uygulayarak yerleşik model bağlama işlevini genişletebilirsiniz.

[Görüntülemek veya örnek Github'dan indirin](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Varsayılan model Bağlayıcısı sınırlamaları

Varsayılan model bağlayıcıları ortak .NET Core veri türleri çoğunu destekler ve çoğu Geliştirici gereksinimlerini karşılamalıdır. Metin tabanlı giriş istekten doğrudan model türleri bağlamak bekler. Bu bağlama önce giriş dönüştürme gerekebilir. Örneğin, model verileri aramak için kullanılan bir anahtara sahip olduğunda. Anahtara göre verileri getirmek için özel bir model bağlayıcısını kullanabilirsiniz.

## <a name="model-binding-review"></a>Model bağlama gözden geçirme

Model bağlama, üzerinde çalıştığı türleri için belirli tanımları kullanır. A *basit tür* girişte tek bir dizeden dönüştürülür. A *karmaşık tür* birden çok giriş değerleri dönüştürülür. Framework varlığını temel fark belirler bir `TypeConverter`. Oluşturduğunuz tür dönüştürücüsünü basit bir varsa önerilen `string`  ->  `SomeType` dış kaynaklar gerektirmeyen eşleme.

Kendi özel model bağlayıcısını oluşturmadan önce bu gözden geçirme nasıl varolan model bağlayıcıları uygulanan olur. Göz önünde bulundurun [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) base64 ile kodlanmış dize bayt diziye dönüştürmek için kullanılabilir. Bayt dizileri genellikle dosyaları ya da veritabanı BLOB alanları depolanır.

### <a name="working-with-the-bytearraymodelbinder"></a>ByteArrayModelBinder ile çalışma

Base64 kodlu dize ikili verileri göstermek için kullanılabilir. Örneğin, aşağıdaki resimde bir dize olarak kodlanmış olmalıdır.

![DotNet bot](custom-model-binding/images/bot.png "dotnet bot")

Aşağıdaki görüntüde kodlu bir dize küçük bir bölümü gösterilir:

![Kodlanmış dotnet bot](custom-model-binding/images/encoded-bot.png "kodlanmış dotnet bot")

' Ndaki yönergeleri izleyin [örnek'ın Benioku dosyası](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) base64 ile kodlanmış dize bir dosyasına dönüştürmek için.

ASP.NET Core MVC base64 ile kodlanmış bir dize alabilir ve kullanmak bir `ByteArrayModelBinder` bir bayt dizisine dönüştürme. [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) hangi uygulayan [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) eşlemeleri `byte[]` bağımsız değişkenleri `ByteArrayModelBinder`:

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

Kendi özel model bağlayıcı oluşturulurken, kendi uygulayabileceğiniz `IModelBinderProvider` yazın veya [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).

Aşağıdaki örnekte nasıl kullanılacağını gösterir `ByteArrayModelBinder` base64 ile kodlanmış bir dizeye dönüştürmek için bir `byte[]` ve sonucu bir dosyaya kaydedin:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Base64 ile kodlanmış bir dize gibi bir araç kullanarak bu API yöntemine NAKLEDEBİLİRSİNİZ [Postman](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

Bağlayıcı istek verileri uygun adlandırılmış özellikleri veya bağımsız değişkenler bağlayabilir sürece, model bağlama işlemi başarılı olur. Aşağıdaki örnekte nasıl kullanılacağını gösterir `ByteArrayModelBinder` bir görünüm model ile:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Özel model bağlayıcı örneği

Bu bölümde biz özel bir model bağlayıcısını uygulamak:

- Gelen istek verileri kesin türü belirtilmiş anahtar bağımsız değişkenleriyle dönüştürür.
- İlişkili varlık getirmek Entity Framework Çekirdek kullanır.
- İlişkili varlık eylem yöntemi için bağımsız değişken olarak geçirir.

Aşağıdaki örnek kullanır `ModelBinder` özniteliği `Author` modeli:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

Önceki kod `ModelBinder` öznitelik türünü belirten `IModelBinder` bağlamak için kullanılmalıdır `Author` eylem parametrelerini. 

`AuthorEntityBinder` Bağlamak için kullanılan bir `Author` varlık Entity Framework Çekirdek kullanarak bir veri kaynağından getirme tarafından parametre ve bir `authorId`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

Aşağıdaki kodu nasıl kullanılacağını gösterir `AuthorEntityBinder` bir eylem yöntemindeki:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

`ModelBinder` Özniteliği uygulamak için kullanılabilir `AuthorEntityBinder` varsayılan kuralları kullanmayın parametreleri için:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

Bağımsız değişken adı varsayılan olmadığından bu örnekte, `authorId`, parametresi kullanılarak belirtilen `ModelBinder` özniteliği. Denetleyici ve eylem yöntemi Basitleştirilmiş, eylem yöntemi varlıkta bakmak için karşılaştırıldığında unutmayın. Entity Framework Çekirdek kullanan Yazar getirmek için mantığı için model bağlayıcı taşınır. Bağlama Yazar modeline ve izlemek için yardımcı olabilir çeşitli yöntemler varsa, bu önemli ölçüde kolaylaştırma olabilir [KURU ilkesine](http://deviq.com/don-t-repeat-yourself/).

Uygulayabileceğiniz `ModelBinder` özniteliği tek tek model özelliklerine (gibi bir viewmodel üzerinde) veya belirli bir model bağlayıcı veya model adı. yalnızca bu tür veya eylemi belirlemek için eylem yöntemi parametrelerine.

### <a name="implementing-a-modelbinderprovider"></a>Bir ModelBinderProvider uygulama

Bir öznitelik uygulamak yerine, uygulayabileceğiniz `IModelBinderProvider`. Yerleşik bir çerçeve bağlayıcıları nasıl uygulandığını budur. Türü belirttiğinizde, bağlayıcı üzerinde çalışır, bu üretir, bağımsız değişken türü belirttiğiniz **değil** giriş, bağlayıcı kabul eder. Aşağıdaki bağlayıcı sağlayıcısı çalışır `AuthorEntityBinder`. MVC'ın sağlayıcıları koleksiyonuna eklendiğinde kullanmanız gerekmez `ModelBinder` özniteliği `Author` veya `Author` parametreleri belirtilmiş.

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Not: Yukarıdaki kod döndüren bir `BinderTypeModelBinder`. `BinderTypeModelBinder`model bağlayıcıları için bir üreteci olarak davranır ve bağımlılık ekleme (dı) sağlar. `AuthorEntityBinder` EF çekirdek erişmek dı gerektirir. Kullanım `BinderTypeModelBinder` , model bağlayıcı dı Hizmetleri'nden gerektiriyorsa.

Bir özel model bağlayıcı sağlayıcısı kullanmak için bunu ekleyin `ConfigureServices`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Model bağlayıcıları değerlendirirken, sağlayıcıları koleksiyonu sırayla incelenir. Bir bağlayıcı döndürür ilk sağlayıcısı kullanılır.

Aşağıdaki resimde varsayılan model bağlayıcıları hata ayıklayıcı gelen gösterir.

![Varsayılan model bağlayıcıları](custom-model-binding/images/default-model-binders.png "varsayılan model bağlayıcıları")

Topluluğun sonuna sağlayıcınız ekleme özel bağlayıcı sıkıştırılabilmesi önce çağrılan yerleşik model bağlayıcı içinde neden olabilir. Bu örnekte, özel sağlayıcı için kullanıldığından emin olmak için koleksiyonunun başına eklenir `Author` eylem bağımsız değişkenleri.

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Öneriler ve en iyi yöntemler

Özel model bağlayıcıları:
- Durum kodları ayarlamak veya sonuçları döndürmek denememeniz gerekir (örneğin, 404 bulunamadı). Model bağlama başarısız olursa, bir [eylem filtresi](xref:mvc/controllers/filters) veya eylem yöntemi mantık hata işleme.
- Yinelenen kodu ve eylem yöntemleri arası kesme kaygılarını ortadan kaldırmak için çok kullanışlıdır.
- Genellikle bir özel tür bir dize dönüştürmek için kullanılmamalıdır bir [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) genellikle daha iyi bir seçenektir.
