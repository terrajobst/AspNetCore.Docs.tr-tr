---
title: ASP.NET core'da model bağlama
author: tdykstra
description: ASP.NET Core model bağlamanın nasıl çalıştığı ve davranışını özelleştirmek nasıl öğrenin.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 05/31/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 7d62ccecdacbd34a38a1fd8c58979a9b09cf86e8
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750207"
---
# <a name="model-binding-in-aspnet-core"></a>ASP.NET core'da model bağlama

Bu makalede, hangi model bağlama, nasıl çalıştığını ve davranışını özelleştirmek nasıl açıklanmaktadır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).

## <a name="what-is-model-binding"></a>Model bağlama nedir

Denetleyicileri ve Razor sayfaları HTTP isteklerinden gelen verilerle çalışır. Örneğin, bir kayıt anahtarı rota verilerini sağlayabilir ve gönderilen form alanlarını, modelin özellikleri için değer sağlayabilir. Bu değerlerin her birini alıp bunları .NET türleri için dizeleri dönüştürmek için kod yazma, sıkıcı ve hataya açık olacaktır. Model bağlama bu işlemini otomatikleştirir. Model bağlama sistemi:

* Rota verileri, form alanlarını ve sorgu dizeleri gibi çeşitli kaynaklardan gelen verileri alır.
* Veri denetleyicilerine ve Razor sayfalarında yöntem parametreleri ve ortak özellikleri sağlar.
* .NET türlerine verileri dönüştürür dize.
* Karmaşık türler özelliklerini güncelleştirir.

## <a name="example"></a>Örnek

Aşağıdaki bir eylem yönteminin olduğunu varsayalım:

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

Ve uygulama bu URL'yi bir istekle alır:

```
http://contoso.com/api/pets/2?DogsOnly=true
```

Eylem yönteminin yönlendirme sistem seçtikten sonra model bağlama aşağıdaki adımları yine de geçer:

* İlk parametresi bulur `GetByID`, adlandırılmış tamsayı `id`.
* HTTP isteği kullanılabilir kaynakları arar ve bulduğu `id` "2" rota verilerindeki =.
* "2" dizesi 2 değeri tamsayıya dönüştürür.
* Sonraki parametresi bulur `GetByID`, adlı bir Boole değeri `dogsOnly`.
* Kaynakları aracılığıyla arar ve bulduğu "DogsOnly = true" sorgu dizesi içinde. Ad eşleştirme büyük küçük harfe duyarlı değildir.
* "True" dizesi, Boole olarak dönüştürür `true`.

Framework sonra çağıran `GetById` yöntemi için 2'deki geçirme `id` parametresi ve `true` için `dogsOnly` parametresi.

Önceki örnekte, model bağlama hedefleri basit türler yöntem parametreleri ' dir. Hedefleri, bir karmaşık tür özelliklerini de olabilir. Her bir özellik başarıyla bağlandıktan sonra [model doğrulama](xref:mvc/models/validation) bu özellik için gerçekleşir. Hangi veri modeli ve bağlama veya doğrulama hataları, bağımlı kaydı depolanan [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) veya [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState). Bu işlem başarılı olduysa, uygulamanın denetler öğrenmek [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) bayrağı.

## <a name="targets"></a>Hedefler

Model bağlama hedefleri aşağıdaki tür için değerleri bulmak çalışır:

* Bir isteği yönlendirilir denetleyici eylem yönteminin parametreleri.
* Bir isteği yönlendirilir Razor sayfaları işleyici yönteminin parametreleri. 
* Etki alanı denetleyicisinin genel özellikleri veya `PageModel` öznitelikleri tarafından belirtilen sınıf.

### <a name="bindproperty-attribute"></a>[BindProperty] özniteliği

Bir denetleyici için bir ortak özellik uygulanabilir veya `PageModel` neden bu özellik hedeflemek, model bağlama sınıfı:

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a>[BindProperties] özniteliği

ASP.NET Core 2.1 ve sonraki sürümlerinde kullanılabilir.  Bir denetleyiciye uygulanabilir veya `PageModel` sınıfın tüm ortak özellikleri hedeflemek için model bağlama bildirmek için sınıfı:

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a>Model bağlama HTTP GET istekleri için

Varsayılan olarak, özellikler için HTTP GET isteklerini bağlı değil. Genellikle, bir GET isteği için ihtiyacınız olan bir kayıt kimliği parametresi. Kayıt kimliği, veritabanında öğeyi aramak için kullanılır. Bu nedenle, modelin bir örneğini tutan bir özelliği bağlamak için gerek yoktur. İstediğiniz özellikler GET isteklerinden verilere bağlı senaryolarda ayarlamak `SupportsGet` özelliğini `true`:

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a>Kaynakları

Varsayılan olarak, model bağlama bir HTTP isteğindeki aşağıdaki kaynaklardan veri anahtar-değer çiftleri biçiminde alır:

1. Form alanlarını 
1. İstek gövdesi (için [[ApiController] özniteliği olan denetleyicileri](xref:web-api/index#binding-source-parameter-inference).)
1. Rota verileri
1. Sorgu dizesi parametreleri
1. Karşıya yüklenen dosyalar 

Her hedef parametresi veya özelliği için kaynakları bu listede gösterilen sırayla taranır. Birkaç özel durum vardır:

* Veri ve sorgu dize değerleri yalnızca basit türleri için kullanılan yol.
* Karşıya yüklenen dosyalar uygulayan hedef türlerine bağlı `IFormFile` veya `IEnumerable<IFormFile>`.

Varsayılan davranışı doğru sonuçları alamazsanız, herhangi belirli bir hedef için kullanılacak kaynağı belirlemek için aşağıdaki özniteliklerden birini kullanabilirsiniz. 

* [[FromQuery] ](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -Sorgu dizesi değerleri alır. 
* [[FromRoute] ](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -Değerler rota verilerini alır.
* [[FromForm] ](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -Gönderilen form alanının değerleri alır.
* [[FromBody] ](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -Gövdeden değerleri alır.
* [[FromHeader] ](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -Değerleri HTTP üst bilgileri alır.

Bu öznitelikler:

* Model özellikleri ayrı ayrı eklenir (değil için model sınıfı) aşağıdaki örnekte olduğu gibi:

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* İsteğe bağlı olarak bir oluşturucuda model adı değeri kabul edin. Özellik adı, istekteki değerle eşleşmeyen durumunda bu seçeneği sağlanır. Örneğin, istek değeri adında, aşağıdaki örnekte olduğu gibi bir tire sahip bir üstbilgi olabilir:

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a>[FromBody] özniteliği

İstek gövdesi veri giriş biçimlendiricileri isteğin içerik türüyle belirli kullanılarak ayrıştırılır. Giriş biçimlendiricileri açıklanmıştır [bu makalenin ilerleyen bölümlerinde](#input-formatters).

Uygulama `[FromBody]` eylem yönteminin başına birden fazla parametre. ASP.NET Core çalışma zamanı giriş biçimlendirici için istek akışı okuma sorumluluğunu atar. İstek akışı okunduktan sonra artık diğer bağlama için yeniden okumak için hazır değil `[FromBody]` parametreleri.

### <a name="additional-sources"></a>Ek kaynaklar

Kaynak veri modeline bağlama sistem tarafından sağlanan *değer sağlayıcıları*. Yazma ve diğer kaynaklardan model bağlama için veri alma özel değer sağlayıcılarını kaydedin. Örneğin, tanımlama bilgisi veya oturum durumu verileri isteyebilirsiniz. Yeni bir kaynaktan veri almak için:

* Uygulayan bir sınıf oluşturma `IValueProvider`.
* Uygulayan bir sınıf oluşturma `IValueProviderFactory`.
* Fabrika sınıfında kaydetme `Startup.ConfigureServices`.

Örnek uygulamayı içeren bir [değer sağlayıcısındaki](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) ve [Fabrika](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) değerlerini tanımlama bilgilerini alır. örnek. Kayıt kodu işte `Startup.ConfigureServices`:

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

Özel değer sağlayıcı, gösterilen kod yerleşik değer sağlayıcıları tüm koyar.  Bunu yapmak için ilk çağrı listesinde `Insert(0, new CookieValueProviderFactory())` yerine `Add`.

## <a name="no-source-for-a-model-property"></a>Model bir özellik için bir kaynağı yok

Hiçbir değer için bir model özelliğine bulunması durumunda varsayılan olarak, model durumu hatası oluşturulmadı. Özelliği null veya varsayılan değere ayarlanır:

* Boş değer atanabilir basit türler ayarlandığında `null`.
* Atanamayan değer türleri ayarlanmış `default(T)`. Örneğin, bir parametre `int id` 0 olarak ayarlayın.
* Karmaşık türler için model bağlama özellikleri ayarlamadan varsayılan Oluşturucusu kullanarak örneği oluşturur.
* Diziler ayarlanmış `Array.Empty<T>()`dışında `byte[]` diziler ayarlandığında `null`.

Hiçbir şey için bir model özelliğine form alanlarını bulunduğunda, model durumu geçersiz, kullanın [[BindRequired] özniteliği](#bindrequired-attribute).

Not Bu `[BindRequired]` gönderilen formu verilerden model bağlama için değil, bir istek gövdesi JSON veya XML verilerinde davranış uygulanır. İstek gövdesi verileri tarafından işlenir [giriş biçimlendiricileri](#input-formatters).

## <a name="type-conversion-errors"></a>Tür dönüştürme hataları

Bir kaynak bulunamadı, ancak hedef türüne dönüştürülemiyor, model durumu geçersiz olarak işaretlenmiş. Hedef parametre ya da özelliği null veya varsayılan değer, önceki bölümde belirtildiği gibi ayarlanır.

Sahip bir API denetleyicisi, `[ApiController]` özniteliği, otomatik bir HTTP 400 yanıt geçersiz model durumu sonuçları.

Bir Razor sayfası bir hata iletisiyle sayfasını yeniden görüntüleyin:

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

İstemci tarafı doğrulama Razor sayfaları forma gönderilmesi çoğu bozuk veri yakalar. Bu doğrulama önceki vurgulanmış kodu tetiklemek zorlaştırır. Örnek uygulamayı içeren bir **geçersiz tarih ile Gönder** bozuk veri koyar, bir düğme **işe giriş tarihi** alan ve formu gönderir. Bu düğme, veri dönüştürme hataları meydana geldiğinde sayfayı yeniden görüntülemek için kodun nasıl çalıştığını gösterir.

Önceki kodun sayfası görüntülendiğinde, form alanı geçersiz giriş gösterilmez. Model özelliği null veya varsayılan değere ayarlanmış olmasıdır. Geçersiz giriş bir hata iletisi görüntülenir. Ancak, form alanı bozuk verileri yeniden görüntülemek istiyorsanız, model özelliğine bir dize yapma ve veri dönüştürme el ile yapılması göz önünde bulundurun.

Tür dönüştürme hatalarının model durumuna hatalara neden istemiyorsanız aynı stratejisi önerilir. Bu durumda, model özelliğine bir dize olun.

## <a name="simple-types"></a>Basit türler

Model bağlayıcı, kaynak dizeleri dönüştürebilir basit türleri şunlardır:

* [Boole değeri](xref:System.ComponentModel.BooleanConverter)
* [Bayt](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)
* [Char](xref:System.ComponentModel.CharConverter)
* [Tarih/saat](xref:System.ComponentModel.DateTimeConverter)
* [DateTimeOffset](xref:System.ComponentModel.DateTimeOffsetConverter)
* [Ondalık](xref:System.ComponentModel.DecimalConverter)
* [çift](xref:System.ComponentModel.DoubleConverter)
* [Sabit listesi](xref:System.ComponentModel.EnumConverter)
* [GUID](xref:System.ComponentModel.GuidConverter)
* [Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)
* [Tek](xref:System.ComponentModel.SingleConverter)
* [TimeSpan](xref:System.ComponentModel.TimeSpanConverter)
* [UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)
* [URI](xref:System.UriTypeConverter)
* [Sürüm](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a>Karmaşık türler

Karmaşık bir tür, genel bir varsayılan oluşturucu ve bağlamak için genel yazılabilir özellikleri olmalıdır. Model bağlama oluştuğunda genel varsayılan oluşturucu kullanılarak sınıfının örneği. 

Model bağlama için her bir özellik karmaşık türün adı deseni için kaynakları arar *prefix.property_name*. Hiçbir şey bulunursa, yalnızca görünüyor *property_name* öneki olmadan.

Bağlama için bir parametre, parametre adı önekidir. Bağlama için bir `PageModel` ortak özelliği, ön eki olan genel özellik adı. Bazı özniteliklere sahip bir `Prefix` olanak tanıyan özellik parametresi veya özellik adı varsayılan kullanımını geçersiz kılar.

Örneğin, karmaşık tür aşağıdaki olduğunu varsayın `Instructor` sınıfı:

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a>Ön ek parametre adı =

Adlı bir parametre modelin bağlı olup olmadığını `instructorToUpdate`:

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

Model bağlama başlatır kaynakları anahtarı için bakarak `instructorToUpdate.ID`. Bulunamazsa, arar `ID` öneki olmadan.

### <a name="prefix--property-name"></a>Ön ek özellik adı =

Bağlanılacak modeli adlı bir özellik olup olmadığını `Instructor` denetleyicisinin veya `PageModel` sınıfı:

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

Model bağlama başlatır kaynakları anahtarı için bakarak `Instructor.ID`. Bulunamazsa, arar `ID` öneki olmadan.

### <a name="custom-prefix"></a>Özel ön eki

Adlı bir parametre modelin bağlı olup olmadığını `instructorToUpdate` ve `Bind` özniteliği belirtir `Instructor` ön ek olarak:

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

Model bağlama başlatır kaynakları anahtarı için bakarak `Instructor.ID`. Bulunamazsa, arar `ID` öneki olmadan.

### <a name="attributes-for-complex-type-targets"></a>Karmaşık tür hedefleri için öznitelikler

Birkaç yerleşik öznitelikler, karmaşık türler, model bağlama denetlemek için kullanılabilir:

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> Bu öznitelikler, model etkileyen değerleri kaynağına gönderilen veri formu bağlamayı. İstek gövdesi JSON ve XML işlemi gönderilen giriş biçimlendiricileri etkilemez. Giriş biçimlendiricileri açıklanmıştır [bu makalenin ilerleyen bölümlerinde](#input-formatters).
>
> Ayrıca bkz `[Required]` özniteliğini [Model doğrulama](xref:mvc/models/validation#required-attribute).

### <a name="bindrequired-attribute"></a>[BindRequired] özniteliği

Yalnızca model özellikleri, yöntem parametreleri için uygulanabilir. Model durumu hatası bağlanıyorsa bu değeri eklemek için neden model bağlama için bir modelin özelliği oluşamaz. Örnek buradadır:

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a>[BindNever] özniteliği

Yalnızca model özellikleri, yöntem parametreleri için uygulanabilir. Model bağlama bir modelin özelliği ayarı öğesinden engeller. Örnek buradadır:

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a>[Bağlama] özniteliği

Bir sınıf ya da bir yöntem parametresi için uygulanabilir. Model bağlama bir modelin hangi özelliklerin eklenmesi gereken belirtir.

Aşağıdaki örnekte, yalnızca belirtilen özelliklerini `Instructor` modeli, herhangi bir işleyici veya eylem yöntemi çağrıldığında bağlıdır:

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

Aşağıdaki örnekte, yalnızca belirtilen özelliklerini `Instructor` model zaman bağlı `OnPost` yöntemi çağrılır:

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

`[Bind]` Özniteliği içinde overposting karşı korumak için kullanılabilir *oluşturma* senaryoları. Dışlanan özellikler için null veya varsayılan değeri olan yerine değiştirilmedi ayarlandığından iyi düzenleme senaryolarda işe yaramaz. Görünüm modelleri overposting karşı savunma için önerilen yerine `[Bind]` özniteliği. Daha fazla bilgi için [overposting hakkında güvenlik notu](xref:data/ef-mvc/crud#security-note-about-overposting).

## <a name="collections"></a>Koleksiyonlar

Basit türler hedefler için model bağlama için eşleşme arar *parameter_name* veya *property_name*. Eşleşme bulunursa, desteklenen biçimlerden öneki olmadan arar. Örneğin:

* Bağlanacak parametre adlı bir dizi olduğunu varsayın `selectedCourses`:

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* Form veya sorgu dizesi verileri aşağıdaki biçimlerden birinde olabilir:
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* Aşağıdaki biçimi yalnızca form verilerini desteklenir:

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* Tüm önceki örnek biçimler için model bağlama için iki öğelerin bir dizisi geçirir `selectedCourses` parametresi:

  * [0] selectedCourses 1050 =
  * [1] selectedCourses 2000 =

  Bu alt simge numaraları kullan (...) veri biçimleri [0]... [1]...) Bunlar sırayla sıfırdan başlayarak numaralandırılır emin olmanız gerekir. Alt simge numaralandırmayı herhangi bir boşluk varsa, boşluğun sonra tüm öğeleri göz ardı edilir. Örneğin, alt simgeler 0 ve 0 ile 1 yerine 2 ise, ikinci öğesi göz ardı edilir.

## <a name="dictionaries"></a>sözlükler

İçin `Dictionary` hedefler, model bağlama için bir eşleşme arar *parameter_name* veya *property_name*. Eşleşme bulunursa, desteklenen biçimlerden öneki olmadan arar. Örneğin:

* Hedef parametre olduğunu varsayın bir `Dictionary<string, string>` adlı `selectedCourses`:

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* Gönderilen formundaki veya sorgu dizesi verileri aşağıdaki örneklerden birini gibi görünebilir:

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* Tüm önceki örnek biçimler için model bağlama için iki öğeyi sözlüğü geçirir `selectedCourses` parametresi:

  * selectedCourses["1050"]="Chemistry"
  * selectedCourses ["2000"] "Ekonomisinden" =

## <a name="special-data-types"></a>Özel veri türleri

Model bağlama işleyebilir bazı özel veri türleri vardır.

### <a name="iformfile-and-iformfilecollection"></a>IFormFile ve IFormFileCollection

HTTP isteğinde karşıya yüklenen dosya.  Ayrıca desteklenen olan `IEnumerable<IFormFile>` birden çok dosya için.

### <a name="cancellationtoken"></a>cancellationToken

Zaman uyumsuz denetleyicileri etkinliğinde iptal etmek için kullanılır.

### <a name="formcollection"></a>FormCollection

Tüm değerleri gönderilen form verileri almak için kullanılır.

## <a name="input-formatters"></a>Giriş biçimlendiricileri

İstek gövdesinde veri, JSON, XML veya başka bir biçime olabilir. Bu verileri ayrıştırmak için model bağlama kullanan bir *giriş biçimlendirici* belirli bir içerik türünü işleyecek şekilde yapılandırılır. Varsayılan olarak, ASP.NET Core, JSON verilerini işlemek için giriş JSON tabanlı biçimlendiricileri içerir. Diğer içerik türleri için diğer biçimlendiricileri ekleyebilirsiniz.

ASP.NET Core giriş biçimlendiricileri göre seçer [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) özniteliği. Hiçbir öznitelik varsa, bunu kullanan [Content-Type üstbilgisi](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).

Yerleşik XML giriş biçimlendiricileri kullanmak için:

* Yükleme `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet paketi.

* İçinde `Startup.ConfigureServices`, çağrı <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> veya <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* Uygulama `Consumes` özniteliği denetleyici sınıflarına veya istek gövdesinde XML beklemelisiniz eylem yöntemleri.

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  Daha fazla bilgi için [giriş XML serileştirme](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).

## <a name="exclude-specified-types-from-model-binding"></a>Model bağlamanın dışında belirtilen türlerini dışla

Model bağlama ve doğrulama systems davranış tarafından yönlendirilen [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata). Özelleştirebileceğiniz `ModelMetadata` ayrıntıları sağlayıcıya ekleyerek [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders). Yerleşik ayrıntıları sağlayıcıları, model bağlama veya belirtilen tür için doğrulama devre dışı bırakmak için kullanılabilir.

Belirtilen bir türün tüm modeller üzerinde model bağlama devre dışı bırakmak için ekleme bir <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> içinde `Startup.ConfigureServices`. Örneğin, devre dışı bırakma türü tüm modeller üzerinde model bağlama için `System.Version`:

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

Belirtilen türün özelliklerini doğrulamasını devre dışı bırakmak için ekleme bir <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> içinde `Startup.ConfigureServices`. Örneğin, türün özelliklerini doğrulama devre dışı bırakmak için `System.Guid`:

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a>Özel model bağlayıcıları

Model bağlama özel yazarak genişletebilir model bağlayıcısını ve kullanarak `[ModelBinder]` özniteliği için belirtilen bir hedef seçin. Daha fazla bilgi edinin [özel model bağlama](xref:mvc/advanced/custom-model-binding).

## <a name="manual-model-binding"></a>El ile model bağlama

Model bağlama çağrılacak el ile kullanarak <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> yöntemi. Yöntemi, hem de tanımlanır `ControllerBase` ve `PageModel` sınıfları. Yöntem aşırı yüklemeleri kullanılacak öneki ve değer sağlayıcısı belirtmenizi sağlar. Yöntem döndürür `false` model bağlama başarısız olursa. Örnek buradadır:

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a>[FromServices] özniteliği

Bu özniteliğin adı, bir veri kaynağını belirtin, model bağlama özniteliklerini desenini izler. Ancak bir değer sağlayıcısı veri bağlama hakkında değil. Bir türün bir örneği alır [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. Amacı, bir hizmet, yalnızca belirli bir yöntemi çağrılırsa ihtiyacınız olduğunda Oluşturucu ekleme için alternatif sağlamaktır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
