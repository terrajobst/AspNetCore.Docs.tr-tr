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
# <a name="custom-model-binding-in-aspnet-core"></a>ASP.NET core'da özel Model bağlama

Tarafından [Steve Smith](https://ardalis.com/)

Model bağlama (yöntemi bağımsız değişken olarak geçirilen), doğrudan model türleri yerine HTTP isteklerini daha çalışmak denetleyici eylemleri sağlar. Gelen istek veri ve uygulama modelleri arasında eşleme model bağlayıcılar tarafından işlenir. Geliştiriciler, özel model bağlayıcıları (genellikle, kendi sağlayıcınızı yazmanız gerekmez ancak) uygulayarak yerleşik model bağlama işlevselliğini genişletebilirsiniz.

[Github'dan örnek görüntüleme veya indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Varsayılan model Bağlayıcısı sınırlamaları

Varsayılan model bağlayıcıları, genel .NET Core veri türlerinin çoğu destekler ve çoğu Geliştirici gereksinimlerini karşılamalıdır. Giriş metin tabanlı, doğrudan model türleri istekten bağlamak beklerler. Bu bağlama önce giriş dönüştürme gerekebilir. Örneğin, model verileri aramak için kullanılan bir anahtara sahip olduğunda. Anahtara göre verileri getirmek için özel bir model bağlayıcısını kullanabilirsiniz.

## <a name="model-binding-review"></a>Model bağlama gözden geçirme

Model bağlama, üzerinde çalıştığı türleri için tanımları kullanır. A *basit tür* girişte tek bir dize dönüştürülür. A *karmaşık tür* birden çok giriş değerlerini dönüştürülür. Varlığını temel farkı framework belirleyen bir `TypeConverter`. Oluşturduğunuz bir tür dönüştürücüsü basit varsa önerilen `string`  ->  `SomeType` dış kaynaklara gerektirmeyen eşleme.

Kendi özel bir model bağlayıcı oluşturmadan önce bu gözden geçirme nasıl var olan model bağlayıcıları uygulanan olur. Göz önünde bulundurun [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) bayt dizileri base64 ile kodlanmış dizeleri dönüştürmek için kullanılabilir. Bayt dizileri genellikle dosyaları ya da veritabanı BLOB alanları depolanır.

### <a name="working-with-the-bytearraymodelbinder"></a>ByteArrayModelBinder ile çalışma

Dizeleri Base64 ile kodlanmış ikili verileri temsil etmek için kullanılabilir. Örneğin, aşağıdaki görüntüde bir dize olarak kodlanmış.

![DotNet bot](custom-model-binding/images/bot.png "dotnet Robotu")

Kodlanmış dizeyi küçük bir kısmı, aşağıdaki görüntüde gösterilmiştir:

![dotnet bot kodlanmış](custom-model-binding/images/encoded-bot.png "kodlanmış dotnet Robotu")

Bölümündeki yönergeleri [örnek'ın Benioku](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) base64 ile kodlanmış dize bir dosyasına dönüştürmek için.

ASP.NET Core MVC base64 ile kodlanmış bir dize alabilir ve kullanmak bir `ByteArrayModelBinder` Bayt dizisine dönüştürmek için. [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) uygulayan [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) eşler `byte[]` bağımsız değişkenleri `ByteArrayModelBinder`:

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

Kendi özel bir model bağlayıcı oluşturulurken, kendi uygulayabileceğiniz `IModelBinderProvider` yazın veya [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).

Aşağıdaki örnek nasıl kullanılacağını gösterir `ByteArrayModelBinder` base64 ile kodlanmış dizeye dönüştürmek için bir `byte[]` ve sonucu bir dosyaya kaydedin:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Base64 ile kodlanmış bir dize gibi bir araç kullanarak bu API yöntemine GÖNDEREBİLİR [Postman](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

Model bağlama, uygun şekilde adlandırılmış özellikleri ya da bağımsız değişkenler için bağlayıcı istek verileri bağlayabilirsiniz sürece başarılı olur. Aşağıdaki örnek nasıl kullanılacağını gösterir `ByteArrayModelBinder` görünüm modeli:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Özel model bağlayıcı örneği

Bu bölümde biz özel bir model bağlayıcısını uygulayacaksınız:

- Gelen istek verileri kesin türü belirtilmiş anahtar bağımsız değişkenleriyle dönüştürür.
- İlişkili varlık getirmek için Entity Framework Core kullanır.
- İlişkili varlık eylem yöntemi için bağımsız değişken olarak geçirir.

Aşağıdaki örnek kullanımları `ModelBinder` özniteliği `Author` modeli:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

Önceki kodda, `ModelBinder` öznitelik türünü belirten `IModelBinder` bağlamak için kullanılmalıdır `Author` eylem parametreleri.

Aşağıdaki `AuthorEntityBinder` sınıfı bağlamalar bir `Author` Entity Framework Core kullanan bir veri kaynağından varlık getirilirken tarafından parametre ve bir `authorId`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> Önceki `AuthorEntityBinder` sınıfı özel bir model bağlayıcısını göstermek için tasarlanmıştır. Sınıfı, arama senaryo için en iyi yöntemleri göstermek için tasarlanmamıştır. Arama için bağlama `authorId` ve bir eylem yöntemi veritabanında sorgu. Bu yaklaşım, model bağlama hatalardan ayıran `NotFound` durumları.

Aşağıdaki kod nasıl kullanılacağını gösterir `AuthorEntityBinder` bir eylem yöntemindeki:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

`ModelBinder` Özniteliği uygulamak için kullanılabilir `AuthorEntityBinder` varsayılan kuralları kullanmayan parametreleri için:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

Bu örnekte, varsayılan bağımsız değişken adını değil bu yana `authorId`, parametresini kullanarak belirtilen `ModelBinder` özniteliği. Denetleyici ve eylem yöntemi Basitleştirilmiş, eylem yönteminin varlığı bakmak için karşılaştırıldığında unutmayın. Entity Framework Core kullanan Yazar getirilecek mantıksal model bağlayıcıya taşınır. Bağlama çeşitli yöntemler varsa, bu önemli ölçüde basitleştirme olabilir `Author` model ve izlemek için yardımcı olabileceğini [KURU ilkesine](http://deviq.com/don-t-repeat-yourself/).

Uygulayabileceğiniz `ModelBinder` özniteliği için ayrı modeli özellikleri (gibi bir viewmodel üzerinde) veya olarak eylem metodu parametreleriyle belirli bir model bağlayıcı veya yalnızca bu tür veya eylem için model adını belirtmek için.

### <a name="implementing-a-modelbinderprovider"></a>Bir ModelBinderProvider uygulama

Uygulayabileceğiniz bir öznitelik uygulamak yerine `IModelBinderProvider`. Yerleşik bir çerçeve bağlayıcıları nasıl uygulandığını budur. Türü belirttiğinizde, bağlayıcı üzerinde çalışır, bu üretir, bağımsız değişken türünü belirtin **değil** , bağlayıcı giriş kabul eder. Aşağıdaki bağlayıcıyı çalışır `AuthorEntityBinder`. MVC'nin sağlayıcıları koleksiyonuna eklendiğinde kullanmanız gerekmez `ModelBinder` özniteliği `Author` veya `Author`-yazılan parametreleri.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Not: Yukarıdaki kod döndürür bir `BinderTypeModelBinder`. `BinderTypeModelBinder` model bağlayıcıları için bir üreteci davranır ve bağımlılık ekleme (dı) sağlar. `AuthorEntityBinder` EF Core erişmeye DI gerektirir. Kullanım `BinderTypeModelBinder` , model bağlayıcı DI hizmetlerinden gerekiyorsa.

Özel model bağlayıcı sağlayıcısı kullanmak için ekleyebilirsiniz `ConfigureServices`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Model bağlayıcıları değerlendirirken sağlayıcıları koleksiyonu sırayla incelenir. Bir bağlayıcı döndüren ilk sağlayıcısı kullanılır.

Aşağıdaki görüntüde varsayılan hata ayıklayıcı'dan model bağlayıcıları gösterir.

![Varsayılan model bağlayıcıları](custom-model-binding/images/default-model-binders.png "varsayılan model bağlayıcıları")

Sağlayıcınız koleksiyonun sonuna ekleme, özel bağlayıcı şansı olmadan önce çağrılan yerleşik bir model bağlayıcı neden olabilir. Bu örnekte, özel bir sağlayıcı için kullanıldığı emin olmak için koleksiyon başına eklenir `Author` eylem bağımsız değişkenleri.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Öneriler ve en iyi uygulamalar

Özel model bağlayıcıları:

- Durum kodları ayarlamak veya sonuçları döndürmek çalışmayın (örneğin, 404 bulunamadı). Model bağlama başarısız olursa bir [eylem filtresi](xref:mvc/controllers/filters) veya eylem yöntemi bir mantık hatası işleme.
- Yinelenen kod ve geniş kapsamlı kritik konular eylem yöntemlerinden ortadan kaldırmak için ekseriyetle faydalıdır.
- Genellikle bir dize özel bir türe dönüştürmek için kullanılmaması bir [ `TypeConverter` ](/dotnet/api/system.componentmodel.typeconverter) genellikle daha iyi bir seçenektir.
